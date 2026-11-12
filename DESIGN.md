---
version: "alpha"
name: "vue-pure-admin Design System"
description: "Visual design guidance for the Vue 3 + Vite + Element Plus admin system, based on the existing implementation."
owner: "pure-admin"
project: "vue-pure-admin"
repo: "https://github.com/pure-admin/vue-pure-admin"
status: "active"
language: "zh-CN"
---

## Overview

This document describes the visual design system of `pure-admin/vue-pure-admin` and provides implementation guidance for developers and AI coding assistants. It reflects the existing project styles; where it recommends consistency beyond the current implementation, that is explicitly marked as a guideline rather than an existing runtime feature.

The project is a Vue 3 admin system built with Element Plus and Tailwind CSS. Its design combines a configurable primary theme color, several menu palettes, light/dark modes, responsive layouts, and component-level styles.

The primary source files are `public/platform-config.json`, `src/style/index.scss`, `src/style/theme.scss`, `src/style/dark.scss`, `src/style/reset.scss`, `src/style/sidebar.scss`, `src/style/element-plus.scss`, `src/layout/hooks/useDataThemeChange.ts`, `src/store/modules/epTheme.ts`, and `src/layout/components/lay-setting/index.vue`.

## Colors

### Existing theme behavior

The application supports a light/dark appearance mode and several menu color themes. The settings panel allows users to change appearance mode, menu theme color, and layout. The selected settings are persisted in local storage. The primary Element Plus color is applied at runtime through `--el-color-primary` and related light/dark variants.

The existing menu palettes are defined by `html[data-theme="..."]` selectors in `src/style/theme.scss`:

| Theme key | Description | Palette accent |
| --- | --- | --- |
| `light` | Light menu | `#4091f7` |
| `default` | Dodger-blue / navy menu | `#4091f7` |
| `saucePurple` | Deep violet | `#693ac9` |
| `pink` | Deep pink | `#d84493` |
| `dusk` | Crimson | `#e13c39` |
| `volcano` | Orange-red | `#e85f33` |
| `mingQing` | Emerald / teal | `#59bfc1` |
| `auroraGreen` | Lime green | `#60ac80` |

The theme swatches and identifiers are listed in `src/layout/hooks/useDataThemeChange.ts`. Note that the menu theme palette and Element Plus primary color are related but distinct concerns: the runtime hook sets the primary color according to the selected theme and configuration.

### Semantic color tokens

```yaml
colors:
  primary: "#409EFF"
  menuAccent: "#4091f7"
  pageBackground: "#f0f2f5"
  surface: "#ffffff"
  textPrimary: "var(--el-text-color-primary)"
  textRegular: "var(--el-text-color-regular)"
  border: "var(--pure-border-color)"
  success: "var(--el-color-success)"
  warning: "var(--el-color-warning)"
  danger: "var(--el-color-danger)"
  info: "var(--el-color-info)"
```

These are semantic references for design and implementation. Some are Element Plus variables; the exact values may depend on the active theme and Element Plus configuration.

### Color usage rules

- Use `var(--el-color-primary)` for primary actions, links, focus/selection emphasis, active navigation indicators, progress indicators, and other theme-aware accents.
- Use Element Plus semantic variables such as `--el-color-success`, `--el-color-warning`, `--el-color-danger`, and `--el-color-info` for status meaning. Do not use the primary color to communicate error or success.
- Use `--el-text-color-primary` and `--el-text-color-regular` for text where the component supports theme-aware text colors.
- Use `--pure-theme-*` variables for menu-specific backgrounds, hover states, and selected text. Do not substitute menu colors for general page surfaces.
- Use `--pure-border-color` for reusable subtle borders where appropriate.
- Keep status color meaning consistent across badges, alerts, charts, and action feedback.
- Ensure text and controls remain legible in both light and dark modes.
- For a component that should react to theme changes, prefer semantic CSS variables over literal hex/RGB values.

### Dynamic and fixed colors

Dynamic colors include the primary Element Plus color and the menu theme variables. They respond to the selected theme when components consume the corresponding variables. Static literals remain fixed and may not adapt to theme changes. When adding a fixed color, document why it must remain fixed and verify it in light and dark modes.

## Typography

### Font family

The global font stack is defined in `src/style/reset.scss`:

```css
"Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB",
"Microsoft YaHei", "微软雅黑", Arial, sans-serif
```

Code and preformatted text use a monospace stack defined in the same file.

### Existing typography and recommended scale

The codebase uses local font sizes rather than one centralized type scale. For example, menu labels commonly use `14px`, menu icons `18px`, and the tag bar `14px`. The following scale is a recommended consistency guide, not a claim that every size is currently globally tokenized:

```yaml
typography:
  fontFamily: '"Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", "微软雅黑", Arial, sans-serif'
  lineHeight: 1.5
  fontSize:
    caption: "12px"
    compact: "13px"
    body: "14px"
    bodyLarge: "16px"
    sectionTitle: "18px"
    pageTitle: "20px"
  fontWeight:
    regular: 400
    medium: 500
    semibold: 600
    bold: 700
```

### Typography usage rules

- Use `14px` for dense admin UI controls, menu items, and secondary interface text, consistent with existing patterns.
- Use `16px` for comfortable body copy and prominent form or panel content.
- Use `18px`–`20px` for section and page headings; establish hierarchy with weight and spacing rather than adding many arbitrary sizes.
- Use `12px`–`13px` only for compact metadata and helper text; ensure it remains readable.
- Use the global font stack unless a component has a documented reason to use another family.
- Avoid setting font sizes inline when a shared utility class or reusable style can express the same role.

## Layout and Spacing

### Existing layout measurements

The layout styles in `src/style/sidebar.scss` establish several fixed structural measurements:

| Element | Existing value |
| --- | ---: |
| Expanded vertical sidebar | `210px` |
| Collapsed sidebar | `54px` |
| Horizontal header | `48px` high |
| Menu item | `50px` high |
| Tag bar | `34px` high |
| Global line height | `1.5` |

These values are implementation details and should be changed only with responsive layout testing.

### Recommended spacing scale

```yaml
spacing:
  1: "4px"
  2: "8px"
  3: "12px"
  4: "16px"
  5: "20px"
  6: "24px"
  8: "32px"
```

Use the existing Tailwind utilities and project conventions where possible. The scale above is a recommendation for new or refactored UI; it is not currently a single enforced global spacing-token implementation.

### Layout rules

- Keep page gutters, card spacing, and form spacing consistent within a view.
- Align labels, values, controls, and table columns to a clear grid.
- Preserve the existing responsive behavior, including mobile navigation and the configurable page stretch option.
- Avoid unnecessary nesting of cards and panels; use hierarchy and spacing to separate content.
- Do not change sidebar/header dimensions in isolation; check vertical, horizontal, mixed, collapsed, and mobile layouts.

## Elevation & Depth

The project uses restrained shadows and borders, particularly for cards, dropdowns, and overlays. Prefer a subtle border or shadow rather than combining multiple strong effects.

```yaml
elevation:
  none: "none"
  subtle: "0 1px 2px rgb(0 0 0 / 8%)"
  popover: "0 2px 8px rgb(0 0 0 / 15%)"
```

Use these as guidance for new UI. Existing component-specific shadows may differ. In dark mode, verify that shadows and borders remain visible without creating bright halos.

## Shapes

The existing project uses modest corner rounding in menus, tags, popovers, and controls. Keep the overall shape language compact and functional.

```yaml
rounded:
  small: "3px"
  default: "4px"
  medium: "6px"
  pill: "9999px"
```

- Use small/default radii for compact controls, tags, and menu highlights.
- Use medium radii for cards or larger surfaces when consistent with the surrounding UI.
- Use pill rounding only for badges or intentionally pill-shaped controls.
- Avoid mixing sharp, highly rounded, and pill shapes arbitrarily in one interface.

## Components

### Navigation menu

Use `--pure-theme-menu-bg`, `--pure-theme-menu-text`, `--pure-theme-menu-hover`, `--pure-theme-sub-menu-bg`, `--pure-theme-sub-menu-active-text`, `--pure-theme-menu-title-hover`, and `--pure-theme-menu-active-before` for menu theme styling. Use `--el-color-primary` for active indicators that should follow the primary color. The theme definitions live in `src/style/theme.scss`; menu behavior is styled in `src/style/sidebar.scss`.

### Tabs and tags

The tag bar uses theme-aware text and primary-color active states. Prefer `var(--el-text-color-primary)` for normal text and `var(--el-color-primary)` for selected or emphasized states. Verify close-button hover and active-tag contrast in both themes. Existing styles are in `src/layout/components/lay-tag/index.scss`.

### Buttons

- Reserve primary styling for the main action in a local action group.
- Use neutral/default styling for secondary actions.
- Use danger styling for destructive actions and make destructive intent explicit in the label.
- Use Element Plus component props and semantic types before adding custom CSS.
- Avoid multiple competing primary buttons in the same small region.

### Forms

- Keep labels, fields, helper text, validation feedback, and required indicators visually distinct.
- Use primary color for focus and interaction emphasis, not as the default color of all form content.
- Use semantic status colors for validation outcomes.
- Preserve the global form label styling in `src/style/element-plus.scss` unless a specific component requires an accessible override.

### Cards, tables, and dialogs

- Use theme surfaces and Element Plus variables where available.
- Keep table density appropriate to the information volume; maintain clear row and column alignment.
- Keep dialogs focused on one task and use consistent header/footer spacing.
- Check dark-mode overrides in `src/style/dark.scss` for all customized surfaces.

## Motion

```yaml
motion:
  standardDuration: "0.3s"
  quickDuration: "0.12s"
  easing: "ease"
```

The global transition token `--pure-transition-duration` is currently `0.3s`. Prefer short, purposeful transitions for hover, selection, and layout changes. Respect reduced-motion preferences when introducing motion-intensive interactions.

## Theme Modes and Configuration

Default platform values are configured in `public/platform-config.json`, including `Theme`, `DarkMode`, `ThemeMode`, and `EpThemeColor`. Runtime theme behavior is implemented in `src/layout/hooks/useDataThemeChange.ts` and `src/store/modules/epTheme.ts`; the settings UI is in `src/layout/components/lay-setting/index.vue`.

When introducing or changing a theme:

1. Define its menu palette in `src/style/theme.scss` using the existing `data-theme` pattern.
2. Add or update the matching swatch and theme key in `useDataThemeChange.ts`.
3. Confirm the runtime primary color behavior and persistence.
4. Check light/dark mode, vertical/horizontal/mix layouts, collapsed navigation, and mobile presentation.
5. Check semantic status colors separately from the brand/primary color.
6. Do not promise runtime customization for values that are still hardcoded in component styles.

## Accessibility

- Maintain readable contrast for text, icons, controls, and selected states in both light and dark modes.
- Do not rely on color alone to convey success, warning, error, or selection; pair color with text, icons, or shape where appropriate.
- Preserve visible keyboard focus for interactive controls.
- Ensure hover styles do not remove or obscure text and icon contrast.
- Keep touch targets usable on mobile devices.
- Avoid unnecessary animation and support reduced-motion preferences for new animated experiences.

## Do's and Don'ts

### Do

- Use `var(--el-color-primary)` and Element Plus semantic color variables for theme-aware UI.
- Use `--pure-theme-*` variables for menu-specific color roles.
- Reuse the global font stack and existing Tailwind utilities.
- Test theme-aware components in both light and dark appearances.
- Keep layout and component dimensions consistent with existing patterns.
- Add a new token when a repeated visual role needs a shared value.

### Don't

- Do not hardcode a primary blue in components that should follow the selected theme.
- Do not use success, warning, or danger colors as decorative accents without semantic meaning.
- Do not assume every current style is dynamically themeable; component-level literals exist.
- Do not introduce a new spacing, radius, or typography value when an existing role is suitable.
- Do not make a theme change without checking contrast and responsive layouts.
- Do not treat this guide's recommended scales as already implemented global CSS variables.

## Implementation References

- `public/platform-config.json` — default platform/theme configuration
- `src/style/index.scss` — global CSS variables and common styles
- `src/style/theme.scss` — menu theme palettes
- `src/style/dark.scss` — dark-mode styling
- `src/style/reset.scss` — base typography and reset
- `src/style/sidebar.scss` — menu and layout styling
- `src/style/element-plus.scss` — Element Plus overrides
- `src/layout/hooks/useDataThemeChange.ts` — runtime theme switching
- `src/store/modules/epTheme.ts` — primary theme color state and persistence
- `src/layout/components/lay-setting/index.vue` — user-facing settings panel
- `src/layout/components/lay-tag/index.scss` — tag bar styling

## Change Checklist

Before merging a visual change, verify:

- [ ] The color has a semantic role and uses a token where appropriate.
- [ ] Text and controls remain legible in light and dark modes.
- [ ] Active, hover, focus, disabled, success, warning, and error states are distinguishable.
- [ ] Typography and spacing match the existing hierarchy.
- [ ] The change works in vertical, horizontal, and mixed layouts where relevant.
- [ ] Mobile and collapsed-sidebar layouts remain usable.
- [ ] Any new theme token or theme option is documented and wired into runtime behavior.
- [ ] The document is updated if the project’s actual design tokens or behavior changed.
