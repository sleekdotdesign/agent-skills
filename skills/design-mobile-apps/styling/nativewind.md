# Styling: Nativewind

Tailwind classes on React Native primitives: same elements, `className` in place of a style object, so the mockup's classes carry over almost verbatim. Reached from [React Native (Expo)](../implementing.md#react-native-expo) when `nativewind` is in `package.json`.

## Check the installed major first

The two Nativewind lines consume different Tailwind majors, and everything below branches on which one is installed:

| Installed Nativewind | Tailwind it expects | Sleek's classes | `oklch(...)` tokens                                            |
| -------------------- | ------------------- | --------------- | -------------------------------------------------------------- |
| v4                   | v3 (`tailwindcss: >3.3.0`) | convert v4 → v3 | convert to hex — `react-native-css-interop` rejects `oklch` |
| v5                   | v4 (`tailwindcss: >4.1.11`) | use as-is    | keep as `oklch` — `react-native-css` converts them at build time, so converting by hand loses precision for nothing |

Read the resolved version from `package.json` or the lockfile. An unpinned install lands **v4**: `latest` is 4.2.6 and v5 has sat on the `preview` tag for over a year, so a v5 project is a deliberate `nativewind@preview` choice.

## Tailwind v4 → v3, for the constructs Sleek emits

| Sleek (Tailwind v4)        | Nativewind v4 (Tailwind v3)                      |
| -------------------------- | ------------------------------------------------ |
| `shadow-xs`, `shadow-sm`   | `shadow-sm`, `shadow`                            |
| `rounded-xs`, `rounded-sm` | `rounded-sm`, `rounded`                          |
| `outline-hidden`           | `outline-none`                                   |
| `bg-linear-to-b`           | `bg-gradient-to-b` (then `expo-linear-gradient`) |
| `bg-(--token)`             | `bg-[var(--token)]`, or a config color           |

A class that resolves to nothing is usually one of these renames. Under v5 none of it applies.

## Tokens

`@theme` / `@theme inline` is v4 syntax with no v3 equivalent: under Nativewind v4 the tokens go into `theme.extend` in `tailwind.config.js`, which is what makes `bg-primary` and `text-foreground` resolve at all. Under v5 the `@theme` block ports to the CSS entry as it stands.

## Configuration

A missing build step presents as every class doing nothing at all, **silently**. When the app renders unstyled, check this list before rewriting classes — and check the list for the major you actually have, because they differ:

**v4** — the babel preset, the metro wrapper, `tailwind.config.js`, and the CSS entry imported in the root layout.

**v5** — the peer deps `tailwindcss`, `react-native-css`, `react-native-reanimated` and `react-native-safe-area-context`; a `postcss.config.mjs` loading `@tailwindcss/postcss`; the metro wrapper; the CSS entry imported in the root layout; and an `"overrides": { "lightningcss": "1.30.1" }` pin in `package.json`. v5 uses no babel preset and no `tailwind.config.js`, so a missing one of those is not the cause.
