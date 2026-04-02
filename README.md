# Mellow SCSS

A SASS-based utility CSS framework that generates customizable utility classes from configuration maps. Define your design tokens (colors, spacing, breakpoints, typography) and get a complete set of utility classes tailored to your project.

## Installation

```bash
npm install mellow-scss
```

## Quick Start

### Use with defaults

Import the library and it generates utility classes with sensible defaults:

```scss
@use 'mellow-scss';
```

### Custom configuration

Override any defaults by calling the `main` mixin with your own configuration:

```scss
@use 'mellow-scss' as mellow;
@use 'mellow-scss/src/abstracts' as *;
@use 'mellow-scss/src/defaults' as *;
@use 'mellow-scss/src/base' as *;

@include mellow.main(
    $colors: (
        light: (
            'primary': (#1a1a2e, #16213e, #0f3460, #e94560, #f5f5f5),
            'secondary': (#2d3436, #636e72, #b2bec3, #dfe6e9, #f8f9fa),
        ),
        dark: (
            'primary': (#f5f5f5, #e94560, #0f3460, #16213e, #1a1a2e),
            'secondary': (#f8f9fa, #dfe6e9, #b2bec3, #636e72, #2d3436),
        ),
    ),
    $breakpoints: (
        'sm': '640px',
        'md': '768px',
        'lg': '1024px',
        'xl': '1280px',
    ),
    $grid-column-count: 16
);
```

### Override defaults via `@use ... with`

Key configuration variables support `!default` flags for direct overrides:

```scss
@use 'mellow-scss/src/defaults/breakpoints' with (
    $breakpoints: (
        'sm': '640px',
        'md': '768px',
        'lg': '1024px',
        'xl': '1280px',
        '2xl': '1536px',
    )
);
@use 'mellow-scss/src/defaults/spacing' with (
    $determinants: (
        start: 0.5,
        end: 8,
        scale: 0.5,
    )
);
@use 'mellow-scss';
```

## Available Utility Classes

### Colors

The color system uses a **setter/getter** pattern for flexible theming.

**Setters** assign color palettes to abstract variables:
```html
<div class="x-primary">  <!-- links --x-* vars to --primary-* -->
<div class="y-secondary"> <!-- links --y-* vars to --secondary-* -->
```

**Getters** apply those variables to CSS properties:
```html
<p class="c-x-500">Primary text (shade 500)</p>
<div class="b-y-300">Secondary background (shade 300)</div>
```

**Theming** via class-based switching:
```html
<body class="light"> <!-- or class="dark" -->
```

### Spacing

Generated from a configurable range (default: 0.25 to 10, step 0.25). Uses CSS custom properties for composition.

| Class | Property | Example |
|-------|----------|---------|
| `.p-{size}` | padding | `.p-1`, `.p-025` |
| `.pt-{size}` | padding-top | `.pt-2` |
| `.pb-{size}` | padding-bottom | `.pb-05` |
| `.pl-{size}` | padding-left | `.pl-4` |
| `.pr-{size}` | padding-right | `.pr-1` |
| `.pil-{size}` | padding-inline | `.pil-2` |
| `.pbl-{size}` | padding-block | `.pbl-1` |
| `.m-{size}` | margin | `.m-1`, `.m-4` |
| `.mt-{size}` | margin-top | `.mt-2` |
| `.mb-{size}` | margin-bottom | `.mb-05` |
| `.ml-{size}` | margin-left | `.ml-auto` |
| `.mr-{size}` | margin-right | `.mr-4` |
| `.mil-{size}` | margin-inline | `.mil-2` |
| `.mbl-{size}` | margin-block | `.mbl-1` |
| `.gp-{size}` | gap | `.gp-2` |
| `.rg-{size}` | row-gap | `.rg-1` |
| `.cg-{size}` | column-gap | `.cg-4` |

**Negative margins:** `.-m-1`, `.-m-025`, etc.

Size values: dots are removed from class names (e.g., `0.25` becomes `025`, `1.5` becomes `15`).

### Flexbox

```html
<div class="d-flex jc-center ai-center gp-2">
    <div class="as-stretch">Flex item</div>
</div>
```

| Prefix | Property | Values |
|--------|----------|--------|
| `.d-` | display | `flex`, `inline-flex` |
| `.sd-` | flex-direction | `row`, `column`, `row-reverse`, `column-reverse` |
| `.jc-` | justify-content | `flex-start`, `center`, `space-between`, etc. |
| `.ai-` | align-items | `stretch`, `center`, `flex-start`, `baseline`, etc. |
| `.ac-` | align-content | `flex-start`, `center`, `space-between`, etc. |
| `.as-` | align-self | `auto`, `center`, `stretch`, etc. |
| (none) | flex-wrap | `.wrap`, `.nowrap`, `.wrap-reverse` |

**Flex flow:** `.flex-flow-row-wrap`, `.flex-flow-column-nowrap`, etc.

**Responsive:** All flexbox classes have responsive variants (`.md-d-flex`, `.lg-jc-center`, etc.)

### CSS Grid

```html
<div class="d-grid gtc-3 gp-2">
    <div class="gc-span-2">Spans 2 columns</div>
    <div>Single column</div>
    <div class="gc-full">Full width row</div>
</div>
```

| Prefix | Property | Values |
|--------|----------|--------|
| `.d-` | display | `grid`, `inline-grid` |
| `.gtc-{n}` | grid-template-columns | `1` through `12` (repeat n, 1fr) |
| `.gtr-{n}` | grid-template-rows | `1` through `12` |
| `.gc-span-{n}` | grid-column span | `1` through `12` |
| `.gc-full` | grid-column: 1 / -1 | Full width |
| `.gc-start-{n}` | grid-column-start | `1` through `13` |
| `.gc-end-{n}` | grid-column-end | `1` through `13` |
| `.gr-span-{n}` | grid-row span | `1` through `12` |
| `.gr-full` | grid-row: 1 / -1 | Full height |
| `.gr-start-{n}` | grid-row-start | `1` through `13` |
| `.gr-end-{n}` | grid-row-end | `1` through `13` |
| `.gaf-` | grid-auto-flow | `row`, `column`, `dense`, `row-dense`, `column-dense` |
| `.ji-` | justify-items | `start`, `end`, `center`, `stretch` |
| `.gai-` | align-items (grid) | `start`, `end`, `center`, `stretch`, `baseline` |
| `.gjc-` | justify-content (grid) | `start`, `end`, `center`, `space-between`, etc. |
| `.gac-` | align-content (grid) | `start`, `end`, `center`, `space-between`, etc. |
| `.pi-` | place-items | `start`, `end`, `center`, `stretch` |
| `.js-` | justify-self | `auto`, `start`, `end`, `center`, `stretch` |
| `.gas-` | align-self (grid) | `auto`, `start`, `end`, `center`, `stretch`, `baseline` |

### Sizing (Width & Height)

```html
<div class="w-100pct h-100vh">Full viewport</div>
<img class="w-50pct h-auto" />
```

| Prefix | Property | Values |
|--------|----------|--------|
| `.w-` | width | `0`, `auto`, `25pct`, `50pct`, `75pct`, `100pct`, `100vw`, `fit-content`, `min-content`, `max-content` |
| `.min-w-` | min-width | `0`, `25pct`-`100pct`, `100vw`, `fit-content`, `min-content`, `max-content` |
| `.max-w-` | max-width | `none`, `25pct`-`100pct`, `100vw`, `fit-content`, `min-content`, `max-content` |
| `.h-` | height | `0`, `auto`, `25pct`-`100pct`, `100vh`, `100dvh`, `fit-content`, `min-content`, `max-content` |
| `.min-h-` | min-height | `0`, `25pct`-`100pct`, `100vh`, `100dvh`, `fit-content`, `min-content`, `max-content` |
| `.max-h-` | max-height | `none`, `25pct`-`100pct`, `100vh`, `100dvh`, `fit-content`, `min-content`, `max-content` |

### Layout

| Prefix | Property | Values |
|--------|----------|--------|
| `.d-` | display | `block`, `inline`, `inline-block` |
| `.pos-` | position | `absolute`, `relative`, `fixed`, `sticky`, `static` |
| `.ovrf-` | overflow | `scroll`, `visible`, `clip`, `hidden`, `auto` |
| `.ovrf-x-` | overflow-x | same |
| `.ovrf-y-` | overflow-y | same |
| `.z-index-` | z-index | `0` through `5` |
| `.vis-` | visibility | `visible`, `hidden`, `collapse` |
| `.obj-f-` | object-fit | `contain`, `cover`, `fill`, `none`, `scale-down` |
| `.iso-` | isolation | `auto`, `isolate` |

### Typography

**Static classes:**

| Prefix | Property |
|--------|----------|
| `.fw-` | font-weight (`normal`, `bold`, `lighter`, `bolder`) |
| `.fst-` | font-style (`normal`, `italic`, `oblique`) |
| `.ta-` | text-align (`start`, `end`, `center`, `left`, `right`, `justify`) |
| `.tt-` | text-transform (`capitalize`, `uppercase`, `lowercase`, `none`) |
| `.tdl-` | text-decoration-line (`none`, `underline`, `overline`, `line-through`) |
| `.wsp-` | white-space (`normal`, `nowrap`, `pre`, `pre-wrap`) |
| `.wd-` | word-break (`normal`, `break-all`, `keep-all`, `break-word`) |
| `.to-` | text-overflow (`clip`, `ellipsis`, `fade`) |

**Sized classes** (generated from the spacing range):

| Prefix | Property | Unit |
|--------|----------|------|
| `.fs-{size}` | font-size | rem |
| `.ls-{size}` | letter-spacing | px |
| `.ws-{size}` | word-spacing | px |
| `.lh-{size}` | line-height | (unitless) |

### Borders

```html
<div class="bs-solid bw-2 rounded-lg">Bordered box</div>
```

| Class | Property |
|-------|----------|
| `.bs-{style}` | border-style (`none`, `solid`, `dashed`, `dotted`, `double`, etc.) |
| `.bw-{n}` | border-width (`0`, `1`, `2`, `4`, `8` in px) |
| `.btw-{n}` | border-top-width |
| `.brw-{n}` | border-right-width |
| `.bbw-{n}` | border-bottom-width |
| `.blw-{n}` | border-left-width |
| `.rounded-{size}` | border-radius (`none`, `sm`, (default), `md`, `lg`, `xl`, `full`) |
| `.rounded-tl-{size}` | border-top-left-radius |
| `.rounded-tr-{size}` | border-top-right-radius |
| `.rounded-br-{size}` | border-bottom-right-radius |
| `.rounded-bl-{size}` | border-bottom-left-radius |

### Effects

```html
<div class="opacity-75 shadow-lg">Semi-transparent with large shadow</div>
```

| Class | Property |
|-------|----------|
| `.opacity-{n}` | opacity (`0`, `5`, `10`, `20`, `25`, `30`, `40`, `50`, `60`, `70`, `75`, `80`, `90`, `95`, `100`) |
| `.shadow-{size}` | box-shadow (`none`, `sm`, (default), `md`, `lg`, `xl`, `inner`) |

### Interactivity

| Prefix | Property | Values |
|--------|----------|--------|
| `.cur-` | cursor | `auto`, `default`, `pointer`, `wait`, `text`, `move`, `help`, `not-allowed`, `none`, `grab`, `grabbing`, `zoom-in`, `zoom-out`, etc. |
| `.pe-` | pointer-events | `none`, `auto` |
| `.us-` | user-select | `none`, `text`, `all`, `auto` |
| `.resize-` | resize | `none`, `both`, `horizontal`, `vertical` |
| `.scroll-` | scroll-behavior | `auto`, `smooth` |
| `.touch-` | touch-action | `auto`, `none`, `pan-x`, `pan-y`, `manipulation`, `pinch-zoom` |

### Transitions

```html
<button class="tp-all duration-300 ease-ease-in-out">Animated button</button>
```

| Class | Property |
|-------|----------|
| `.tp-{property}` | transition-property (`none`, `all`, `color`, `background-color`, `border-color`, `opacity`, `box-shadow`, `transform`) |
| `.ease-{fn}` | transition-timing-function (`linear`, `ease`, `ease-in`, `ease-out`, `ease-in-out`) |
| `.duration-{ms}` | transition-duration (`0`, `75`, `100`, `150`, `200`, `300`, `500`, `700`, `1000` in ms) |
| `.delay-{ms}` | transition-delay (`0`, `75`, `100`, `150`, `200`, `300`, `500`, `700`, `1000` in ms) |

## Responsive Design

All layout, typography, flexbox, grid, and sizing classes support responsive variants using breakpoint prefixes:

| Breakpoint | Prefix | Min-width |
|------------|--------|-----------|
| Extra small | `xs-` | 0 |
| Small | `sm-` | 576px |
| Medium | `md-` | 768px |
| Large | `lg-` | 992px |
| Extra large | `xl-` | 1200px |

```html
<!-- Stack on mobile, 3 columns on medium, 4 on large -->
<div class="d-grid gtc-1 md-gtc-3 lg-gtc-4 gp-2">
    <div>Item</div>
</div>

<!-- Full width on mobile, half on medium -->
<div class="w-100pct md-w-50pct">Responsive width</div>
```

## Reducing Output Size

The full library generates ~17,800 lines of CSS. For production, use a CSS purge tool to remove unused classes:

**With PostCSS and PurgeCSS:**

```bash
npm install -D @fullhuman/postcss-purgecss postcss postcss-cli
```

```js
// postcss.config.js
module.exports = {
    plugins: [
        require('@fullhuman/postcss-purgecss')({
            content: ['./src/**/*.html', './src/**/*.jsx', './src/**/*.vue'],
            defaultExtractor: (content) => content.match(/[\w-/:%.]+(?<!:)/g) || [],
        }),
    ],
};
```

```bash
npx postcss dist/mellow.css -o dist/mellow.min.css
```

**With Tailwind CSS's standalone CLI** (just the purge part):

PurgeCSS typically reduces the output to only the classes actually used in your HTML, often cutting 90%+ of the file size.

## Development

```bash
npm install           # Install dependencies
npm test              # Run linting + unit tests
npm run build         # Compile to dist/mellow.css
npm run build:watch   # Watch mode
npm run lint:fix      # Auto-fix formatting
```

## License

ISC
