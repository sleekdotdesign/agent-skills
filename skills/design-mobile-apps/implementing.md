# Implementing Designs

When the user wants to implement the designs in code (not just preview them), **always fetch the component HTML code** and build from it; the screenshots confirm the result.

Use `GET /api/v1/projects/:id/components/:componentId` to fetch each screen's code. The `componentId` comes from the chat run's `result.operations`.

Component code can be large. When saving it to files, avoid writing the content through your text output: it's slow and wastes tokens. Instead, use shell commands to fetch the API response and write it directly to disk (e.g., pipe the response body into a file).

## Which version to use

Each component carries a `versions[]` array and an `activeVersion: number`. **By default, use the entry where `versions[i].version === activeVersion`**: that's the code currently shown in Sleek.

If the user's prompt pins specific versions, follow those instead (see [Pinned versions](#pinned-versions) below).

## Pinned versions

The user's prompt may include a pin block telling you to implement specific historical versions instead of the current ones, like this:

```
... at this exact state instead of the project's current version:
- component cmp_abc: version ver_001
- component cmp_def: version ver_002
- theme thm_ghi: version ver_003
```

When you see a pin block, implement those exact versions instead of `activeVersion`. Components not named in the pin block continue to use their active version. Theme IDs surface only inside pin blocks; this skill exposes no separate endpoint to enumerate them.

### Fetching the right code

For each pinned component, find the entry in `versions[]` where `versions[i].id` matches the given version id (e.g. `ver_001`) and use its `code`. Do **not** fall back to `activeVersion` for pinned components.

### Screenshots of pinned versions

Pass `componentVersionOverrides` and `themeVersionOverrides` to `POST /api/v1/screenshots`:

```json
{
  "componentIds": ["cmp_abc"],
  "projectId": "proj_xyz",
  "componentVersionOverrides": { "cmp_abc": "ver_001" },
  "themeVersionOverrides": { "thm_ghi": "ver_003" }
}
```

Keys are component / theme public ids; values are the corresponding `versions[i].id`. Entities missing from a map fall back to their active version. Include the override maps whenever the prompt specified pinned versions.

## HTML prototypes

The component `code` is a complete HTML document. Save it directly to a `.html` file. No build step needed.

## Native frameworks (React Native, SwiftUI, etc.)

Each component document is a **mockup**: a picture of a working screen, drawn in HTML because HTML is what a design tool renders. Read it accordingly.

- **Structure, layout and styling are the spec.** Element hierarchy, flex direction, spacing, sizing, colors, typography, radii, image URLs and icon names — reproduce these exactly.
- **Content is placeholder.** The names, numbers, copy, avatars, list items and "active" states are stand-ins that make the screen legible. Rebuild the container and feed it real state, props or API data — see [Build the feature, not the mockup](#build-the-feature-not-the-mockup).
- **The mockup draws the app's chrome** — tab bar, header, drawer — inside every screen that shows it, because each screen is a standalone document. Chrome belongs to the app's navigation, so build it there once and let each screen render its own content.
- **Screenshots are the visual target.** Check each built screen against them, shot with `fullHeight: true` — [Show the results](designing.md#3-show-the-results) covers that framing and why a viewport shot never proves something is missing.

The HTML tells you _how_ to build it; the screenshot tells you _what_ it should look like; the app supplies the data.

**Building with React Native / Expo?** [React Native (Expo)](#react-native-expo) carries the conversion — flex direction, `<Text>`, insets, the route tree, the styling approach. Read it before the first component.

### Icons

Sleek uses [Iconify](https://iconify.design) icons in the format `prefix:name` (e.g., `solar:heart-bold`, `material-symbols:search-rounded`, `lucide:settings`). The most common sets are **Solar**, **Hugeicons**, **Material Symbols** and **MDI**.

**Use the exact icons from the HTML code** — same set, same name. Matching icons is what carries design fidelity.

When implementing icons:

1. **Check if the project already has an icon system** that supports the same sets Sleek uses (Solar, Hugeicons, Material Symbols, MDI). If so, use it. Note: `@expo/vector-icons` carries different sets, so a project on `@expo/vector-icons` takes the Iconify route below.
2. **Otherwise, fetch the SVGs from the Iconify API and embed them in the code:**

   ```
   GET https://api.iconify.design/{prefix}/{name}.svg
   ```

   Example: `https://api.iconify.design/solar/heart-bold.svg`

   Collect all icon names from the HTML, fetch their SVGs, and save them as static assets or string constants in the codebase. For **React Native / Expo**, render them with `react-native-svg`'s `SvgXml` component, which works in Expo Go with no additional native dependencies.

### Fonts

The HTML includes Google Fonts via `<link>` tags in the `<head>`. Use the same fonts and weights when implementing in a native framework. Extract the font family names and weights from the `<link>` tags.

### Design tokens

Every component document inlines the project's theme in the `<style type="text/tailwindcss">` block in `<head>`: a `:root` rule holding the raw values (`--background`, `--foreground`, `--primary`, `--border`, `--radius`, `--font-*`, …) plus an `@theme inline` block naming them for Tailwind. The same theme is inlined into every screen of a project, so port it once into a single token module and have every component read from that module.

Three of those values need work on the way across:

- **Colors arrive as `oklch(...)`.** React Native's color parser reads hex, `rgb()` and named colors, so convert each token once, in the token module, and every consumer stays correct.
- **`--radius` is a base, not a value.** The document derives `--radius-xs … --radius-4xl` from it with `calc()`; resolve the arithmetic to numbers.
- **`--shape: squircle`**, when present, turns on CSS `corner-shape` and multiplies every radius by `--shape-multiplier` where the browser supports it. React Native has neither, so use plain `borderRadius` and match the rounding the screenshot shows.

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

`flex-col` is already the default and needs nothing; `flex-1` maps to `flex: 1`; `inline-flex` behaves as `flex`, since React Native has no inline layout. Sweep every `flex` in the source and give each one an explicit direction.

### Text

Every string renders inside `<Text>` — including strings hiding in a `&&` branch, a ternary or a stray `{' '}`. A bare string under a `<View>` is a hard runtime crash (`Text strings must be rendered within a <Text> component`). `<span>` and `<p>` both become `<Text>`, and spans nested inside a paragraph become nested `<Text>`. Text styling (`fontSize`, `color`, `fontWeight`, `lineHeight`) sits on the `<Text>` itself, since it does not cross a `View` boundary.

### Insets

The mockup has no notch, status bar or home indicator: it renders edge to edge inside a fixed frame. On a device, an unguarded screen draws its header under the status bar and its bottom bar under the home indicator. Inset with **`react-native-safe-area-context`** — React Native core exports a `SafeAreaView` of its own that only insets on iOS, so import from the package — and wrap the app once in its `SafeAreaProvider`.

Inset each screen exactly once:

- A screen rendered under a navigator header is already inset by the navigator; render it in a plain `View`.
- A screen with `headerShown: false`, and every modal screen, insets itself: `SafeAreaView` around the screen, or `useSafeAreaInsets()` when you need the numbers — a header you drew yourself, a floating or absolutely positioned bottom bar, a sticky CTA, or content that scrolls under the status bar while its padding respects it.
- `edges` narrows a `SafeAreaView` to the sides that still need guarding, which is how a screen that keeps the navigator header but draws its own bottom bar insets the bottom alone.

### Shadows

iOS reads `shadowColor`, `shadowOffset`, `shadowOpacity` and `shadowRadius`; Android reads `elevation` and derives its own blur and offset from it, so a design's exact shadow lands as an approximation there. Set both and accept the difference. On Android the shadowed view also needs a `backgroundColor` for `elevation` to draw at all.

### Gradients and hover

- **Gradients.** `bg-linear-*` and `linear-gradient(...)` have no style equivalent; render them with `expo-linear-gradient` as a component behind the content.
- **Hover.** `hover:` variants compile but never fire on a touch device. Carry the intent to the press state instead: `Pressable`'s `({ pressed })` style callback, or the `active:` variant.

### Infer the route tree first

Do this before implementing any screen. Each Sleek screen is a standalone document that draws the entire chrome, so the same tab bar is baked into every screen that shows one. Read the screen set as one app, derive the navigator from it, and let each screen render only its own content — the navigator owns the chrome and draws it once.

Use **expo-router**. Four signals carry the structure:

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

### Definition of done

Check each line against every screen in the project; the work is done when no screen leaves a box open.

- [ ] The screen has a route in the navigator, and its own file draws none of the chrome the navigator owns.
- [ ] Every `flex` in the source HTML resolved to an explicit direction, and the built screen's axis matches the screenshot.
- [ ] Every string renders inside `<Text>`; the app boots on iOS and Android with no `Text strings must be rendered` crash.
- [ ] Headerless and modal screens inset themselves, `SafeAreaProvider` wraps the root, and nothing is inset twice.
- [ ] Colors, radii and fonts come from the one token module — searching the screen files for `oklch(` or a raw hex returns nothing.
- [ ] Icons are the exact Iconify names from the HTML; fonts are the exact families and weights from the `<link>` tags.
- [ ] Every value that would change at runtime reads from state, props or the API.
- [ ] Shadows set `elevation` alongside the iOS `shadow*` props.
- [ ] The screen has been compared against its own `fullHeight: true` screenshot, not just the viewport shot.
