# Styling: Nativewind

Tailwind classes on React Native primitives: same elements, `className` in place of a style object, so the mockup's classes carry over almost verbatim. Reached from [React Native (Expo)](../implementing.md#react-native-expo) when `nativewind` is in `package.json`.

## Check the installed major first

Sleek's HTML is Tailwind v4 (the document loads `@tailwindcss/browser@4`), and the two Nativewind lines consume different Tailwind majors:

| Installed Nativewind | Tailwind syntax it expects | Sleek's classes |
| -------------------- | -------------------------- | --------------- |
| v4                   | Tailwind v3                | convert v4 → v3 |
| v5                   | Tailwind v4                | use as-is       |

Read the resolved version from `package.json` or the lockfile. On a fresh install, install unpinned and then check which major landed, since it decides everything below.

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

`@theme` / `@theme inline` is v4 syntax with no v3 equivalent: under Nativewind v4 the tokens go into `theme.extend` in `tailwind.config.js`, which is what makes `bg-primary` and `text-foreground` resolve at all. Under v5 the `@theme` block ports to the CSS entry as it stands. Either way, convert the `oklch(...)` values to hex on the way in — those values reach React Native's color parser.

## Configuration

Nativewind needs all four pieces: the babel preset, the metro wrapper, `tailwind.config.js`, and the CSS entry imported in the root layout. A missing metro or babel step presents as every class doing nothing at all, with no error — when the app renders unstyled, check these four before rewriting classes.
