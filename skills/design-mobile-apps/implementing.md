# Implementing Designs

When the user wants the designs built as code rather than previewed, **always fetch the component HTML code** and build from it; the screenshots confirm the result.

Use `GET /api/v1/projects/:id/components/:componentId` to fetch each screen's code. The `componentId` comes from the chat run's `result.operations`.

Component code can be large. Fetch it with a shell command that writes the response body straight to disk, so the code goes to a file without passing through your text output.

## Which version to use

Each component carries a `versions[]` array and an `activeVersion`, a **nullable** number. **By default, use the entry where `versions[i].version === activeVersion`**: that's the code currently shown in Sleek. When `activeVersion` is `null`, or when no entry matches it, the server resolves the entry with the **highest `version`** — resolve it the same way.

If the user's prompt pins specific versions, follow those instead (see [Pinned versions](#pinned-versions) below).

## Pinned versions

The user's prompt may include a pin block telling you to implement specific historical versions instead of the current ones, like this:

```
... at this exact state instead of the project's current version:
- component V1StGXR8Z5j: version qkP2mN7bXsT
- component 8rJvL4wYhKd: version zC5tRb9WqNm
- theme MfD3xQ6nJyU: version 7hSvA2kLpEr
```

When you see a pin block, implement those exact versions instead of `activeVersion`. Components not named in the pin block continue to use their active version. Theme ids surface only inside pin blocks; this skill exposes no separate endpoint to enumerate them.

**On a pinned checkpoint the HTML's theme may not be the pinned theme.** Sleek generates every version's HTML against the project's *currently active* theme, while the screenshot renderer honours `themeVersionOverrides`. So the two can disagree on a checkpoint, and the screenshot is the authority for color, radius and font.

### Fetching the right code

For each pinned component, find the entry in `versions[]` whose `versions[i].id` matches the given version id and use its `code`. Match on `id`, the string; `version` is the numeric index, and a pinned component takes its code from that matched entry alone.

### Screenshots of pinned versions

Pass `componentVersionOverrides` and `themeVersionOverrides` to `POST /api/v1/screenshots`:

```json
{
  "componentIds": ["V1StGXR8Z5j"],
  "projectId": "Nq4bZp8wLcT",
  "componentVersionOverrides": { "V1StGXR8Z5j": "qkP2mN7bXsT" },
  "themeVersionOverrides": { "MfD3xQ6nJyU": "7hSvA2kLpEr" }
}
```

Keys are component / theme public ids; values are the corresponding `versions[i].id`. Entities missing from a map fall back to their active version. Include the override maps whenever the prompt specified pinned versions.

## HTML prototypes

The component `code` is a complete HTML document. Save it directly to a `.html` file. No build step needed.

## Native frameworks

Each component document is a **mockup**: a picture of a working screen, drawn in HTML because HTML is what a design tool renders.

- **Structure, layout and styling are the spec.** Element hierarchy, flex direction, spacing, sizing, colors, typography, radii, image URLs and icon names — reproduce these exactly.
- **Content is placeholder.** The names, numbers, copy, avatars, list items and "active" states are stand-ins that make the screen legible. Rebuild the container and feed it real state, props or API data — see [Build the feature, not the mockup](#build-the-feature-not-the-mockup).
- **The mockup draws the app's chrome** — tab bar, header, drawer — inside every screen that shows it, because each screen is a standalone document. Chrome belongs to the app's navigation, so build it there once and let each screen render its own content.
- **Screenshots are the visual target.** Check each built screen against its **review shot**; a user shot crops the page and never proves something is missing.

The HTML tells you _how_ to build it; the screenshot tells you _what_ it should look like; the app supplies the data.

### Icons

Sleek uses [Iconify](https://iconify.design) icons in the format `prefix:name` (e.g. `ph:heart-fill`, `solar:heart-bold`, `lucide:settings`). Sleek's design prompt steers toward four sets — **Phosphor** (`ph:`), **Solar** (`solar:`), **MingCute** (`mingcute:`) and **Hugeicons** (`hugeicons:`) — while allowing any Iconify set, so read the prefixes out of the HTML rather than assuming them.

**Use the exact icons from the HTML code** — same set, same name. Matching icons is what carries design fidelity.

When implementing icons:

1. **Check whether the project already has an icon system** that resolves the same names. On a repo that already has `@expo/vector-icons`, `mdi:*` is a free rename: Iconify's `mdi` and the package's `MaterialCommunityIcons` are the same upstream set with matching glyph names, so `mdi:account-check` is `<MaterialCommunityIcons name="account-check" />`. The rest take the Iconify route below — `solar:*` and `hugeicons:*` have no counterpart in the package at all, and `material-symbols:*` is a different Google generation from its `MaterialIcons`. For any other prefix, check the package's set list before assuming a match.
2. **Otherwise, fetch the SVGs from the Iconify API and embed them in the code:**

   ```
   GET https://api.iconify.design/{prefix}/{name}.svg
   ```

   Example: `https://api.iconify.design/solar/heart-bold.svg`

   Collect all icon names from the HTML, fetch their SVGs, and save them as static assets or string constants in the codebase. For **React Native / Expo**, render them with `react-native-svg`'s `SvgXml` component, which works in Expo Go with no additional native dependencies.

### Fonts

The `<head>` `<link>` tags give you the font **families**. They request each family's entire published weight range (or a 100–900 fallback for a family Sleek doesn't recognise), so the weights in those URLs describe what Google Fonts can serve, not what the design uses. The weights actually used are in the body's `font-*` Tailwind classes. Take the families from the `<link>` tags, read the weights off the classes, and bundle only those.

### Design tokens

Every component document inlines the project's theme in the `<style type="text/tailwindcss">` block in `<head>`: a `:root` rule holding the raw values (`--background`, `--foreground`, `--primary`, `--border`, `--radius`, `--font-*`, …) plus an `@theme inline` block naming them for Tailwind. The same theme is inlined into every screen of a project, so port it once into a single token module and have every component read from that module.

Two of those values need work on the way across:

- **`--radius` is a base, not a value.** The document derives `--radius-xs … --radius-4xl` from it with `calc()`; resolve the arithmetic to numbers.
- **`--shape` is always present**, valued `round` or `squircle` — test the value, not its presence. `squircle` turns on CSS `corner-shape` and multiplies every radius by `--shape-multiplier` where the browser supports it. React Native has neither, so use plain `borderRadius` and match the rounding the screenshot shows.

Colors arrive as `oklch(...)`. Whether they survive the crossing depends on the styling library — the file you read below says so for that branch.

---

## React Native (Expo)

Sleek renders web HTML with Tailwind v4 classes (the document loads `@tailwindcss/browser@4` — read the `<head>` to confirm). Several web defaults land differently in React Native, and the failures below are **silent**: the app compiles, runs, and is wrong. Work through them before the first component.

**Styling.** Match what the repo already does. Detect it, then read that file:

| Evidence                                      | Read                                           |
| --------------------------------------------- | ---------------------------------------------- |
| `nativewind` in `package.json`                | [styling/nativewind.md](styling/nativewind.md)  |
| `uniwind` in `package.json`                   | [styling/uniwind.md](styling/uniwind.md)        |
| neither, or existing `StyleSheet.create` files | [styling/stylesheet.md](styling/stylesheet.md) |

Each file carries its own token conversion, class syntax and configuration. For a repo with no styling library and a user with no preference, use StyleSheet: it needs no build configuration, so it cannot be misconfigured into silently doing nothing.

### Flex direction

The most common silent failure. On the web `display: flex` lays children out in a **row**; in React Native the default `flexDirection` is `'column'`. Sleek writes the web idiom, so `class="flex items-center gap-3"` is a horizontal row that renders as a vertical stack until you say `flexDirection: 'row'` (or `flex-row`).

`flex-col` is already the default and needs nothing; `flex-1` maps to `flex: 1`; `inline-flex` is dropped by the style engine with a value warning, which lands on the same layout as `flex` because React Native has no inline mode. Sweep every `flex` in the source and give each one an explicit direction.

### Text

Every string renders inside `<Text>` — including strings hiding in a `&&` branch, a ternary or a stray `{' '}`. A bare string under a `<View>` fails **silently**: React Native's New Architecture routes `Text strings must be rendered within a <Text> component.` through `console.error`, so it appears in a development build's console and nowhere else. A production build carries no such message — the string is simply missing from the screen, and nothing throws.

`<span>` and `<p>` both become `<Text>`, and spans nested inside a paragraph become nested `<Text>`. Text styling (`fontSize`, `color`, `fontWeight`, `lineHeight`) sits on the `<Text>` itself, since it does not cross a `View` boundary.

### Insets

The mockup has no notch, status bar or home indicator: it renders edge to edge inside a fixed frame. On a device, an unguarded screen draws its header under the status bar and its bottom bar under the home indicator or the Android navigation bar — Expo SDK 54 and later make Android edge-to-edge the default, so Android content draws under the system bars just as iOS content does. Inset with **`react-native-safe-area-context`** — React Native core exports a `SafeAreaView` of its own that only insets on iOS, so import from the package — and wrap the app once in its `SafeAreaProvider`.

Inset each edge of each screen exactly once:

- A native-stack navigator header insets the **top only**. A screen under one still owns its bottom edge, so unless a tab bar or other navigator chrome covers it, give the screen `edges={['bottom']}` or bottom padding from `useSafeAreaInsets()`.
- A screen with `headerShown: false`, and every modal screen, insets itself top and bottom: `SafeAreaView` around the screen, or `useSafeAreaInsets()` when you need the numbers — a header you drew yourself, a floating or absolutely positioned bottom bar, a sticky CTA, or content that scrolls under the status bar while its padding respects it.
- `edges` narrows a `SafeAreaView` to the sides that still need guarding, which is how a screen keeps the navigator header and insets the bottom alone.

### Shadows

`boxShadow` takes the mockup's CSS shadow across 1:1 on both platforms — `boxShadow: '0px 4px 12px rgba(0,0,0,0.15)'`, or the array-of-objects form. It is New Architecture only, which is the only architecture React Native ships from 0.82, so it is the default choice and the one that keeps Android matching the design.

Legacy fallback, for a project on an older React Native or still on the old architecture: iOS reads `shadowColor`, `shadowOffset`, `shadowOpacity` and `shadowRadius`; Android reads `elevation` and derives its own blur and offset from it, so the design's exact shadow lands as an approximation there. Set both, and give the shadowed view a `backgroundColor` so `elevation` draws at all.

### Gradients and hover

- **Gradients.** `bg-linear-*` and `linear-gradient(...)` have no style equivalent; render them with `expo-linear-gradient` as a component behind the content.
- **Hover.** `hover:` variants compile but never fire on a touch device. Carry the intent to the press state instead: `Pressable`'s `({ pressed })` style callback, or the `active:` variant.

### Infer the route tree first

Do this before implementing any screen. Each Sleek screen is a standalone document that draws the entire chrome, so the same tab bar is baked into every screen that shows one. Read the screen set as one app, derive the navigator from it, and let each screen render only its own content — the navigator owns the chrome and draws it once.

Use **expo-router**, importing each navigator from the path that actually exports it:

- `Stack` from `expo-router`.
- `Tabs` from `expo-router/js-tabs`. The root `Tabs` export is deprecated as of `expo-router@57`, and `expo-router/tabs` currently aliases `js-tabs` and appears reserved for native tabs, so name `js-tabs` explicitly.
- `Drawer` from `expo-router/drawer` — the root exports no `Drawer` at all. It also needs `react-native-reanimated`, `react-native-worklets` and `react-native-gesture-handler` (SDK 56+) installed alongside it.

Four signals carry the structure:

1. **The same bottom bar means Tab siblings** — those screens sit under one `_layout` sharing a single tab bar.
2. **A back arrow in the header means a Stack child** of wherever the screen is reached from.
3. **An overlay, sheet or dialog means a modal route** — `presentation: 'modal'`, or `@gorhom/bottom-sheet` for a draggable sheet.
4. **A hamburger menu means a Drawer**, with its destinations as Drawer screens.

Worked example: Home, Search and Profile all show the same bottom bar → tab siblings. ProductDetail shows a back arrow → Stack child inside the Home tab. Settings is reached from a menu → Stack or Drawer screen.

Two lookalikes are components rather than routes: horizontal tabs inside the content area (`react-native-tab-view`, or a plain segmented control), and a floating action button (an absolutely positioned `Pressable`).

Tab bar and header styling go through `screenOptions` for as long as the design stays inside what those options express; a custom shape, a floating or pill-shaped bar, a raised center button, a custom active indicator or a header carrying a logo and search field is cheaper as a component passed to the `tabBar` prop or `options.header`. Either way, match the icons, labels and active/inactive states from the design.

A mockup can't show a keyboard, so it never does: every screen with a `TextInput` still needs `KeyboardAvoidingView` or an equivalent.

### Build the feature, not the mockup

A mockup shipped verbatim is a convincing dead app. For each screen, name the feature the UI represents, then wire it: every value that would change at runtime reads from state, props, context or the API; buttons run actions; forms validate and submit; lists fetch; navigation carries params between screens. The hardcoded name, the "5", the highlighted tab and the pre-filled form are all the same job.

A mockup also shows one moment — full, happy, populated. Decide separately what each screen does with zero items, with one, while loading, and on error.

---

## Definition of done

For a native build. Every box below is a **silent** failure: the app compiles and runs with the box open. Check each line against every screen in the project; the work is done when no screen leaves a box open.

- [ ] The screen has a route in the navigator, and its own file draws none of the chrome the navigator owns.
- [ ] Every `flex` in the source HTML resolved to an explicit direction, and the built screen's axis matches its screenshot.
- [ ] Grepping the screen file finds no JSX text node or `{expression}` string whose nearest element is anything but `<Text>`, and a development build's console prints no `Text strings must be rendered within a <Text> component.`
- [ ] Headerless and modal screens inset themselves, every screen's bottom edge is accounted for on Android, `SafeAreaProvider` wraps the root, and nothing is inset twice.
- [ ] Colors, radii and fonts come from the one token module — grepping the screen files for a raw hex or `rgb(` returns nothing.
- [ ] Icons are the exact Iconify names from the HTML, set and name.
- [ ] Fonts are the exact families from the `<link>` tags, bundled at the weights the body's `font-*` classes use.
- [ ] Shadows render on Android as well as iOS: `boxShadow`, or `elevation` alongside the `shadow*` props on a legacy-architecture project.
- [ ] Gradients render through `expo-linear-gradient` — grepping the screens for `bg-linear`, `bg-gradient` and `linear-gradient(` returns nothing.
- [ ] Every `hover:` in the source HTML landed on a press state: `Pressable`'s `({ pressed })` callback or `active:`.
- [ ] Every screen holding a `TextInput` wraps it in `KeyboardAvoidingView` or an equivalent.
- [ ] Grep each screen for display literals — quoted strings and numbers that reach the user — and account for every hit: it reads from state, props, context or the API, or it is genuinely static copy.
- [ ] Each screen has a decided empty, single-item, loading and error state.
- [ ] The styling library is proven configured: one throwaway class renders visibly before the first screen is built.
- [ ] The screen has been compared against its own **review shot**, not the user shot.
