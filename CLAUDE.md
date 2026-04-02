# Mellow CSS - Development Guide

## Project Overview

Mellow CSS (`mellow-scss`) is a SASS-based utility CSS framework. Users configure color palettes, spacing scales, breakpoints, typography, grid, borders, effects, and more — the library generates utility classes from that configuration.

## Architecture

- **Entry point:** `src/main.scss` — defines `@mixin main()` which orchestrates all class generation
- **`src/abstracts/`** — Core mixins (`_mixins.scss`), functions (`_functions.scss`), and media query mixins (`_mediaqueries.scss`)
- **`src/defaults/`** — Default configuration values for each utility category (properties, classnames maps, value lists)
- **`test/`** — sass-true unit tests (Jest runner)

### Utility categories

| Category | Defaults file | Mixins |
|----------|--------------|--------|
| Colors | `_colors.scss` | `set-themed-colors`, `set-colors-with-respective-custom-property`, `get-color-for-each-colorname-nuance-getter` |
| Spacing | `_spacing.scss` | `generate-spacing-setter`, `generate-spacing-getter`, `generate-negative-spacing-setter` |
| Flexbox | `_flexbox.scss` | `generate-basic-flexbox-options`, `generate-responsive-flexbox-options`, `generate-flex-flow-options` |
| Grid | `_grid.scss` | `generate-static-grid-classes`, `generate-grid-template-classes` + responsive variants |
| Layout | `_layout.scss` | `generate-static-layout-classes` + responsive |
| Typography | `_typography.scss` | `generate-static-typography-classes`, `generate-typography-from-range-of-values` + responsive |
| Sizing | `_sizing.scss` | `generate-static-sizing-classes` + responsive |
| Borders | `_borders.scss` | `generate-static-border-classes`, `generate-border-width-classes`, `generate-border-radius-classes` |
| Effects | `_effects.scss` | `generate-opacity-classes`, `generate-box-shadow-classes` |
| Interactivity | `_interactivity.scss` | `generate-static-interactivity-classes` |
| Transitions | `_transitions.scss` | `generate-static-transition-classes`, `generate-transition-duration-classes`, `generate-transition-delay-classes` |

### Key shared infrastructure

- **`static-classes` mixin:** Shared generator for layout, typography, grid, sizing, borders, interactivity, transitions. Takes properties map + classnames map. Uses `sanitize-classname` to handle special chars.
- **`sanitize-classname` function:** Converts spaces→dashes, `%`→`pct`, removes dots. Used by `static-classes` for safe CSS class names.
- **Forwarding pattern:** `defaults/_index.scss` forwards with `default-*` prefix (e.g., `default-sizing-*`). All configuration flows directly from defaults into `main.scss`.

## Commands

```bash
npm test              # Full test suite: lint + unit tests
npm run test:lint     # Prettier check + stylelint
npm run test:sass     # sass-true unit tests (Jest)
npm run build         # Compile to dist/mellow.css
npm run build:watch   # Watch mode compilation
npm run lint:fix      # Auto-fix formatting and lint issues
```

## Testing

- **71 unit tests** across 11 test files using sass-true + Jest
- Tests cover: functions, all mixin categories, edge cases (special chars, responsive, negative values)
- After modifying any mixin or function, run `npm test` to verify
- After adding new utilities, verify build output: `npm run build && wc -l dist/mellow.css`

## Code Style

- SCSS with `@use`/`@forward` module system (no `@import`)
- Prettier: tab width 4, single quotes
- Stylelint: `stylelint-config-standard-scss`
- Conventional commits for semantic-release (`feat:`, `fix:`, `chore:`, etc.)

## Adding New Utility Categories

1. Create `src/defaults/_newcategory.scss` with `$properties`, `$classnames-map`, and any value lists
2. Update `src/defaults/_index.scss` (`@forward 'newcategory' as default-newcategory-*`)
3. Add generation mixins in `src/abstracts/_mixins.scss` (use `static-classes` for enum-style properties, import the defaults file directly)
4. Wire into `src/main.scss` (add parameters + `@include` calls)
5. Add tests in `test/_newcategory.test.scss` and include in `test/test.scss`
6. Run `npm test` and `npm run build`
