# Styling: StyleSheet

React Native's built-in styling. Reached from [React Native (Expo)](../SKILL.md#react-native-expo) when the repo has no styling library.

## Token module

Export the theme from one module — colors, radii, spacing steps, font families — and import it in every component, so a token change stays a one-file edit and no component carries a literal color. Convert the `oklch(...)` values from the document's `:root` block to hex or `rgb()` here, once.

## Units

React Native styles are unitless numbers in density-independent pixels, so Tailwind converts numerically rather than by feel. The scale is `0.25rem` per step:

| Class          | Style                             |
| -------------- | --------------------------------- |
| `p-4`          | `padding: 16`                     |
| `px-5 py-3`    | `paddingHorizontal: 20, paddingVertical: 12` |
| `gap-3`        | `gap: 12`                         |
| `text-sm`      | `fontSize: 14`                    |
| `rounded-xl`   | `borderRadius:` the resolved `--radius-xl` number |
| `w-1/2`        | `width: '50%'` (percentages are strings) |

`rem`, `em`, `vh` and `vw` have no equivalent — a size that depends on the screen reads from `useWindowDimensions()`.

## Composition

- One `StyleSheet.create({ … })` per component file, holding everything static.
- Conditional styling composes with arrays: `style={[styles.card, isActive && styles.cardActive]}`.
- Values that genuinely change per render — a measured width, an animated height — stay inline; everything else lives in the sheet.
