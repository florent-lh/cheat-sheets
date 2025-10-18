# Tailwind CheatSheet

## Table des Matières

1. [Installation et Configuration](#installation-et-configuration)
2. [Layout](#layout)
3. [Flexbox](#flexbox)
4. [Grid](#grid)
5. [Spacing (Margin & Padding)](#spacing-margin--padding)
6. [Sizing (Width & Height)](#sizing-width--height)
7. [Typography](#typography)
8. [Backgrounds](#backgrounds)
9. [Borders](#borders)
10. [Effects](#effects)
11. [Filters](#filters)
12. [Tables](#tables)
13. [Transitions & Animation](#transitions--animation)
14. [Transforms](#transforms)
15. [Interactivity](#interactivity)
16. [SVG](#svg)
17. [Accessibility](#accessibility)
18. [Responsive Design](#responsive-design)
19. [Dark Mode](#dark-mode)
20. [Pseudo-classes](#pseudo-classes)
21. [Pseudo-elements](#pseudo-elements)
22. [Colors](#colors)
23. [Customization](#customization)
24. [Plugins](#plugins)
25. [Best Practices](#best-practices)
26. [Nouveautés Tailwind v3+](#nouveautés-tailwind-v3)
27. [Arbitrary Values](#arbitrary-values)
28. [Container Queries](#container-queries)
29. [Dynamic Classes](#dynamic-classes)

---

## Installation et Configuration

### Installation via npm
```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init
```

### Configuration CDN (développement uniquement)
```html
<script src="https://cdn.tailwindcss.com"></script>
```

### Configuration de base (tailwind.config.js)
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{html,js,jsx,ts,tsx}",
    "./pages/**/*.{html,js,jsx,ts,tsx}",
    "./components/**/*.{html,js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### CSS de base
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### Build
```bash
npx tailwindcss -i ./src/input.css -o ./dist/output.css --watch
```

---

## Layout

### Display
```html
<!-- Block & Inline -->
<div class="block">Block</div>
<div class="inline">Inline</div>
<div class="inline-block">Inline Block</div>

<!-- Flex & Grid -->
<div class="flex">Flex</div>
<div class="inline-flex">Inline Flex</div>
<div class="grid">Grid</div>
<div class="inline-grid">Inline Grid</div>

<!-- Autres -->
<div class="hidden">Hidden</div>
<div class="contents">Contents</div>
<div class="flow-root">Flow Root</div>
<div class="table">Table</div>
<div class="table-row">Table Row</div>
<div class="table-cell">Table Cell</div>
```

### Position
```html
<div class="static">Static</div>
<div class="fixed">Fixed</div>
<div class="absolute">Absolute</div>
<div class="relative">Relative</div>
<div class="sticky">Sticky</div>

<!-- Placement -->
<div class="top-0">Top 0</div>
<div class="right-0">Right 0</div>
<div class="bottom-0">Bottom 0</div>
<div class="left-0">Left 0</div>
<div class="inset-0">All sides 0</div>
<div class="inset-x-0">Horizontal 0</div>
<div class="inset-y-0">Vertical 0</div>

<!-- Valeurs -->
<div class="top-4">Top 1rem</div>
<div class="top-1/2">Top 50%</div>
<div class="top-full">Top 100%</div>
<div class="-top-4">Top -1rem</div>
```

### Z-Index
```html
<div class="z-0">z-index: 0</div>
<div class="z-10">z-index: 10</div>
<div class="z-20">z-index: 20</div>
<div class="z-30">z-index: 30</div>
<div class="z-40">z-index: 40</div>
<div class="z-50">z-index: 50</div>
<div class="z-auto">z-index: auto</div>
<div class="-z-10">z-index: -10</div>
```

### Overflow
```html
<div class="overflow-auto">Auto</div>
<div class="overflow-hidden">Hidden</div>
<div class="overflow-visible">Visible</div>
<div class="overflow-scroll">Scroll</div>
<div class="overflow-x-auto">X Auto</div>
<div class="overflow-y-auto">Y Auto</div>
<div class="overflow-x-hidden">X Hidden</div>
<div class="overflow-y-scroll">Y Scroll</div>
```

### Object Fit & Position
```html
<img class="object-contain" />
<img class="object-cover" />
<img class="object-fill" />
<img class="object-none" />
<img class="object-scale-down" />

<!-- Position -->
<img class="object-center" />
<img class="object-top" />
<img class="object-right" />
<img class="object-bottom" />
<img class="object-left" />
<img class="object-top-right" />
```

### Float & Clear
```html
<div class="float-left">Float Left</div>
<div class="float-right">Float Right</div>
<div class="float-none">Float None</div>

<div class="clear-left">Clear Left</div>
<div class="clear-right">Clear Right</div>
<div class="clear-both">Clear Both</div>
<div class="clear-none">Clear None</div>
```

### Visibility
```html
<div class="visible">Visible</div>
<div class="invisible">Invisible (prend de l'espace)</div>
<div class="collapse">Collapse</div>
```

---

## Flexbox

### Flex Direction
```html
<div class="flex flex-row">Row (défaut)</div>
<div class="flex flex-row-reverse">Row Reverse</div>
<div class="flex flex-col">Column</div>
<div class="flex flex-col-reverse">Column Reverse</div>
```

### Flex Wrap
```html
<div class="flex flex-wrap">Wrap</div>
<div class="flex flex-wrap-reverse">Wrap Reverse</div>
<div class="flex flex-nowrap">No Wrap</div>
```

### Justify Content
```html
<div class="flex justify-start">Start</div>
<div class="flex justify-end">End</div>
<div class="flex justify-center">Center</div>
<div class="flex justify-between">Space Between</div>
<div class="flex justify-around">Space Around</div>
<div class="flex justify-evenly">Space Evenly</div>
<div class="flex justify-stretch">Stretch</div>
```

### Align Items
```html
<div class="flex items-start">Start</div>
<div class="flex items-end">End</div>
<div class="flex items-center">Center</div>
<div class="flex items-baseline">Baseline</div>
<div class="flex items-stretch">Stretch</div>
```

### Align Content
```html
<div class="flex content-start">Start</div>
<div class="flex content-end">End</div>
<div class="flex content-center">Center</div>
<div class="flex content-between">Space Between</div>
<div class="flex content-around">Space Around</div>
<div class="flex content-evenly">Space Evenly</div>
<div class="flex content-stretch">Stretch</div>
```

### Align Self
```html
<div class="self-auto">Auto</div>
<div class="self-start">Start</div>
<div class="self-end">End</div>
<div class="self-center">Center</div>
<div class="self-stretch">Stretch</div>
<div class="self-baseline">Baseline</div>
```

### Flex Grow & Shrink
```html
<div class="flex-1">flex: 1 1 0%</div>
<div class="flex-auto">flex: 1 1 auto</div>
<div class="flex-initial">flex: 0 1 auto</div>
<div class="flex-none">flex: none</div>

<div class="grow">flex-grow: 1</div>
<div class="grow-0">flex-grow: 0</div>

<div class="shrink">flex-shrink: 1</div>
<div class="shrink-0">flex-shrink: 0</div>
```

### Flex Basis
```html
<div class="basis-0">0</div>
<div class="basis-1">0.25rem</div>
<div class="basis-auto">auto</div>
<div class="basis-full">100%</div>
<div class="basis-1/2">50%</div>
<div class="basis-1/3">33.333%</div>
```

### Gap
```html
<div class="flex gap-0">gap: 0</div>
<div class="flex gap-1">gap: 0.25rem</div>
<div class="flex gap-2">gap: 0.5rem</div>
<div class="flex gap-4">gap: 1rem</div>
<div class="flex gap-8">gap: 2rem</div>

<div class="flex gap-x-4">column-gap: 1rem</div>
<div class="flex gap-y-4">row-gap: 1rem</div>
```

### Order
```html
<div class="order-1">order: 1</div>
<div class="order-2">order: 2</div>
<div class="order-first">order: -9999</div>
<div class="order-last">order: 9999</div>
<div class="order-none">order: 0</div>
```

---

## Grid

### Grid Template Columns
```html
<div class="grid grid-cols-1">1 colonne</div>
<div class="grid grid-cols-2">2 colonnes</div>
<div class="grid grid-cols-3">3 colonnes</div>
<div class="grid grid-cols-4">4 colonnes</div>
<div class="grid grid-cols-5">5 colonnes</div>
<div class="grid grid-cols-6">6 colonnes</div>
<div class="grid grid-cols-12">12 colonnes</div>
<div class="grid grid-cols-none">none</div>
```

### Grid Template Rows
```html
<div class="grid grid-rows-1">1 ligne</div>
<div class="grid grid-rows-2">2 lignes</div>
<div class="grid grid-rows-3">3 lignes</div>
<div class="grid grid-rows-6">6 lignes</div>
<div class="grid grid-rows-none">none</div>
```

### Grid Column Span
```html
<div class="col-span-1">span 1</div>
<div class="col-span-2">span 2</div>
<div class="col-span-3">span 3</div>
<div class="col-span-full">span all</div>
<div class="col-auto">auto</div>

<!-- Start / End -->
<div class="col-start-1">start at 1</div>
<div class="col-start-2">start at 2</div>
<div class="col-end-3">end at 3</div>
<div class="col-end-auto">end auto</div>
```

### Grid Row Span
```html
<div class="row-span-1">span 1</div>
<div class="row-span-2">span 2</div>
<div class="row-span-3">span 3</div>
<div class="row-span-full">span all</div>
<div class="row-auto">auto</div>

<!-- Start / End -->
<div class="row-start-1">start at 1</div>
<div class="row-start-2">start at 2</div>
<div class="row-end-3">end at 3</div>
```

### Grid Auto Flow
```html
<div class="grid grid-flow-row">row</div>
<div class="grid grid-flow-col">col</div>
<div class="grid grid-flow-dense">dense</div>
<div class="grid grid-flow-row-dense">row dense</div>
<div class="grid grid-flow-col-dense">col dense</div>
```

### Grid Auto Columns & Rows
```html
<div class="auto-cols-auto">auto</div>
<div class="auto-cols-min">min-content</div>
<div class="auto-cols-max">max-content</div>
<div class="auto-cols-fr">minmax(0, 1fr)</div>

<div class="auto-rows-auto">auto</div>
<div class="auto-rows-min">min-content</div>
<div class="auto-rows-max">max-content</div>
<div class="auto-rows-fr">minmax(0, 1fr)</div>
```

### Place Items & Content
```html
<!-- Place Items (align + justify items) -->
<div class="grid place-items-start">start</div>
<div class="grid place-items-end">end</div>
<div class="grid place-items-center">center</div>
<div class="grid place-items-stretch">stretch</div>

<!-- Place Content (align + justify content) -->
<div class="grid place-content-center">center</div>
<div class="grid place-content-start">start</div>
<div class="grid place-content-end">end</div>
<div class="grid place-content-between">space-between</div>
<div class="grid place-content-around">space-around</div>
<div class="grid place-content-evenly">space-evenly</div>

<!-- Place Self -->
<div class="place-self-auto">auto</div>
<div class="place-self-start">start</div>
<div class="place-self-end">end</div>
<div class="place-self-center">center</div>
<div class="place-self-stretch">stretch</div>
```

---

## Spacing (Margin & Padding)

### Échelle de spacing
```
0    = 0
px   = 1px
0.5  = 0.125rem (2px)
1    = 0.25rem (4px)
1.5  = 0.375rem (6px)
2    = 0.5rem (8px)
2.5  = 0.625rem (10px)
3    = 0.75rem (12px)
3.5  = 0.875rem (14px)
4    = 1rem (16px)
5    = 1.25rem (20px)
6    = 1.5rem (24px)
7    = 1.75rem (28px)
8    = 2rem (32px)
9    = 2.25rem (36px)
10   = 2.5rem (40px)
11   = 2.75rem (44px)
12   = 3rem (48px)
14   = 3.5rem (56px)
16   = 4rem (64px)
20   = 5rem (80px)
24   = 6rem (96px)
28   = 7rem (112px)
32   = 8rem (128px)
36   = 9rem (144px)
40   = 10rem (160px)
44   = 11rem (176px)
48   = 12rem (192px)
52   = 13rem (208px)
56   = 14rem (224px)
60   = 15rem (240px)
64   = 16rem (256px)
72   = 18rem (288px)
80   = 20rem (320px)
96   = 24rem (384px)
```

### Padding
```html
<!-- Tous les côtés -->
<div class="p-0">padding: 0</div>
<div class="p-4">padding: 1rem</div>
<div class="p-8">padding: 2rem</div>

<!-- Horizontal / Vertical -->
<div class="px-4">padding-left & right: 1rem</div>
<div class="py-4">padding-top & bottom: 1rem</div>

<!-- Côtés individuels -->
<div class="pt-4">padding-top: 1rem</div>
<div class="pr-4">padding-right: 1rem</div>
<div class="pb-4">padding-bottom: 1rem</div>
<div class="pl-4">padding-left: 1rem</div>

<!-- Start / End (RTL-aware) -->
<div class="ps-4">padding-inline-start: 1rem</div>
<div class="pe-4">padding-inline-end: 1rem</div>
```

### Margin
```html
<!-- Tous les côtés -->
<div class="m-0">margin: 0</div>
<div class="m-4">margin: 1rem</div>
<div class="m-auto">margin: auto</div>

<!-- Horizontal / Vertical -->
<div class="mx-4">margin-left & right: 1rem</div>
<div class="my-4">margin-top & bottom: 1rem</div>
<div class="mx-auto">margin-left & right: auto (centrer)</div>

<!-- Côtés individuels -->
<div class="mt-4">margin-top: 1rem</div>
<div class="mr-4">margin-right: 1rem</div>
<div class="mb-4">margin-bottom: 1rem</div>
<div class="ml-4">margin-left: 1rem</div>

<!-- Margins négatives -->
<div class="-m-4">margin: -1rem</div>
<div class="-mt-4">margin-top: -1rem</div>
<div class="-mx-4">margin-left & right: -1rem</div>

<!-- Start / End -->
<div class="ms-4">margin-inline-start: 1rem</div>
<div class="me-4">margin-inline-end: 1rem</div>
```

### Space Between
```html
<div class="space-x-4">Espace horizontal entre enfants</div>
<div class="space-y-4">Espace vertical entre enfants</div>
<div class="-space-x-4">Espace négatif horizontal</div>
<div class="space-x-reverse">Reverse l'ordre</div>
```

---

## Sizing (Width & Height)

### Width
```html
<!-- Valeurs fixes -->
<div class="w-0">width: 0</div>
<div class="w-px">width: 1px</div>
<div class="w-4">width: 1rem</div>
<div class="w-64">width: 16rem</div>
<div class="w-96">width: 24rem</div>

<!-- Pourcentages -->
<div class="w-1/2">width: 50%</div>
<div class="w-1/3">width: 33.333%</div>
<div class="w-2/3">width: 66.666%</div>
<div class="w-1/4">width: 25%</div>
<div class="w-3/4">width: 75%</div>
<div class="w-1/5">width: 20%</div>
<div class="w-full">width: 100%</div>

<!-- Viewport -->
<div class="w-screen">width: 100vw</div>
<div class="w-svw">width: 100svw</div>
<div class="w-lvw">width: 100lvw</div>
<div class="w-dvw">width: 100dvw</div>

<!-- Auto & Special -->
<div class="w-auto">width: auto</div>
<div class="w-min">width: min-content</div>
<div class="w-max">width: max-content</div>
<div class="w-fit">width: fit-content</div>
```

### Min/Max Width
```html
<div class="min-w-0">min-width: 0</div>
<div class="min-w-full">min-width: 100%</div>
<div class="min-w-min">min-width: min-content</div>
<div class="min-w-max">min-width: max-content</div>
<div class="min-w-fit">min-width: fit-content</div>

<div class="max-w-xs">max-width: 20rem</div>
<div class="max-w-sm">max-width: 24rem</div>
<div class="max-w-md">max-width: 28rem</div>
<div class="max-w-lg">max-width: 32rem</div>
<div class="max-w-xl">max-width: 36rem</div>
<div class="max-w-2xl">max-width: 42rem</div>
<div class="max-w-3xl">max-width: 48rem</div>
<div class="max-w-4xl">max-width: 56rem</div>
<div class="max-w-5xl">max-width: 64rem</div>
<div class="max-w-6xl">max-width: 72rem</div>
<div class="max-w-7xl">max-width: 80rem</div>
<div class="max-w-full">max-width: 100%</div>
<div class="max-w-screen-sm">max-width: 640px</div>
<div class="max-w-screen-md">max-width: 768px</div>
<div class="max-w-screen-lg">max-width: 1024px</div>
<div class="max-w-screen-xl">max-width: 1280px</div>
<div class="max-w-screen-2xl">max-width: 1536px</div>
```

### Height
```html
<!-- Valeurs fixes -->
<div class="h-0">height: 0</div>
<div class="h-px">height: 1px</div>
<div class="h-4">height: 1rem</div>
<div class="h-64">height: 16rem</div>
<div class="h-96">height: 24rem</div>

<!-- Pourcentages -->
<div class="h-1/2">height: 50%</div>
<div class="h-1/3">height: 33.333%</div>
<div class="h-full">height: 100%</div>

<!-- Viewport -->
<div class="h-screen">height: 100vh</div>
<div class="h-svh">height: 100svh (small)</div>
<div class="h-lvh">height: 100lvh (large)</div>
<div class="h-dvh">height: 100dvh (dynamic)</div>

<!-- Auto & Special -->
<div class="h-auto">height: auto</div>
<div class="h-min">height: min-content</div>
<div class="h-max">height: max-content</div>
<div class="h-fit">height: fit-content</div>
```

### Min/Max Height
```html
<div class="min-h-0">min-height: 0</div>
<div class="min-h-full">min-height: 100%</div>
<div class="min-h-screen">min-height: 100vh</div>
<div class="min-h-svh">min-height: 100svh</div>
<div class="min-h-lvh">min-height: 100lvh</div>
<div class="min-h-dvh">min-height: 100dvh</div>

<div class="max-h-0">max-height: 0</div>
<div class="max-h-full">max-height: 100%</div>
<div class="max-h-screen">max-height: 100vh</div>
<div class="max-h-svh">max-height: 100svh</div>
<div class="max-h-lvh">max-height: 100lvh</div>
<div class="max-h-dvh">max-height: 100dvh</div>
```

### Size (Width & Height ensemble)
```html
<div class="size-0">width & height: 0</div>
<div class="size-4">width & height: 1rem</div>
<div class="size-full">width & height: 100%</div>
<div class="size-screen">width & height: 100vw/vh</div>
```

---

## Typography

### Font Family
```html
<p class="font-sans">Sans-serif</p>
<p class="font-serif">Serif</p>
<p class="font-mono">Monospace</p>
```

### Font Size
```html
<p class="text-xs">0.75rem (12px)</p>
<p class="text-sm">0.875rem (14px)</p>
<p class="text-base">1rem (16px)</p>
<p class="text-lg">1.125rem (18px)</p>
<p class="text-xl">1.25rem (20px)</p>
<p class="text-2xl">1.5rem (24px)</p>
<p class="text-3xl">1.875rem (30px)</p>
<p class="text-4xl">2.25rem (36px)</p>
<p class="text-5xl">3rem (48px)</p>
<p class="text-6xl">3.75rem (60px)</p>
<p class="text-7xl">4.5rem (72px)</p>
<p class="text-8xl">6rem (96px)</p>
<p class="text-9xl">8rem (128px)</p>
```

### Font Weight
```html
<p class="font-thin">100</p>
<p class="font-extralight">200</p>
<p class="font-light">300</p>
<p class="font-normal">400</p>
<p class="font-medium">500</p>
<p class="font-semibold">600</p>
<p class="font-bold">700</p>
<p class="font-extrabold">800</p>
<p class="font-black">900</p>
```

### Font Style
```html
<p class="italic">Italic</p>
<p class="not-italic">Normal</p>
```

### Text Decoration
```html
<p class="underline">Underline</p>
<p class="overline">Overline</p>
<p class="line-through">Line Through</p>
<p class="no-underline">No Underline</p>

<!-- Style de soulignement -->
<p class="decoration-solid">Solid</p>
<p class="decoration-double">Double</p>
<p class="decoration-dotted">Dotted</p>
<p class="decoration-dashed">Dashed</p>
<p class="decoration-wavy">Wavy</p>

<!-- Épaisseur -->
<p class="decoration-auto">Auto</p>
<p class="decoration-from-font">From Font</p>
<p class="decoration-0">0px</p>
<p class="decoration-1">1px</p>
<p class="decoration-2">2px</p>
<p class="decoration-4">4px</p>
<p class="decoration-8">8px</p>

<!-- Offset -->
<p class="underline-offset-auto">Auto</p>
<p class="underline-offset-0">0px</p>
<p class="underline-offset-1">1px</p>
<p class="underline-offset-2">2px</p>
<p class="underline-offset-4">4px</p>
<p class="underline-offset-8">8px</p>
```

### Text Transform
```html
<p class="uppercase">UPPERCASE</p>
<p class="lowercase">lowercase</p>
<p class="capitalize">Capitalize</p>
<p class="normal-case">Normal Case</p>
```

### Text Align
```html
<p class="text-left">Left</p>
<p class="text-center">Center</p>
<p class="text-right">Right</p>
<p class="text-justify">Justify</p>
<p class="text-start">Start (RTL-aware)</p>
<p class="text-end">End (RTL-aware)</p>
```

### Vertical Align
```html
<span class="align-baseline">Baseline</span>
<span class="align-top">Top</span>
<span class="align-middle">Middle</span>
<span class="align-bottom">Bottom</span>
<span class="align-text-top">Text Top</span>
<span class="align-text-bottom">Text Bottom</span>
<span class="align-sub">Sub</span>
<span class="align-super">Super</span>
```

### Line Height
```html
<p class="leading-none">1</p>
<p class="leading-tight">1.25</p>
<p class="leading-snug">1.375</p>
<p class="leading-normal">1.5</p>
<p class="leading-relaxed">1.625</p>
<p class="leading-loose">2</p>
<p class="leading-3">0.75rem</p>
<p class="leading-10">2.5rem</p>
```

### Letter Spacing
```html
<p class="tracking-tighter">-0.05em</p>
<p class="tracking-tight">-0.025em</p>
<p class="tracking-normal">0</p>
<p class="tracking-wide">0.025em</p>
<p class="tracking-wider">0.05em</p>
<p class="tracking-widest">0.1em</p>
```

### Text Color
```html
<p class="text-black">Black</p>
<p class="text-white">White</p>
<p class="text-gray-500">Gray 500</p>
<p class="text-red-500">Red 500</p>
<p class="text-blue-500">Blue 500</p>
<p class="text-inherit">Inherit</p>
<p class="text-current">Current</p>
<p class="text-transparent">Transparent</p>

<!-- Avec opacité -->
<p class="text-blue-500/50">Blue 500 à 50%</p>
<p class="text-blue-500/75">Blue 500 à 75%</p>
```

### Text Overflow
```html
<p class="truncate">Truncate (ellipsis)</p>
<p class="text-ellipsis">Text Ellipsis</p>
<p class="text-clip">Text Clip</p>
```

### White Space
```html
<p class="whitespace-normal">Normal</p>
<p class="whitespace-nowrap">No Wrap</p>
<p class="whitespace-pre">Pre</p>
<p class="whitespace-pre-line">Pre Line</p>
<p class="whitespace-pre-wrap">Pre Wrap</p>
<p class="whitespace-break-spaces">Break Spaces</p>
```

### Word Break
```html
<p class="break-normal">Normal</p>
<p class="break-words">Break Words</p>
<p class="break-all">Break All</p>
<p class="break-keep">Keep</p>
```

### Text Indent
```html
<p class="indent-0">0</p>
<p class="indent-4">1rem</p>
<p class="indent-8">2rem</p>
<p class="-indent-4">-1rem</p>
```

### Hyphens
```html
<p class="hyphens-none">None</p>
<p class="hyphens-manual">Manual</p>
<p class="hyphens-auto">Auto</p>
```

### Text Wrap
```html
<p class="text-wrap">Wrap</p>
<p class="text-nowrap">No Wrap</p>
<p class="text-balance">Balance (v3.4+)</p>
<p class="text-pretty">Pretty (v3.4+)</p>
```

---

## Backgrounds

### Background Color
```html
<div class="bg-black">Black</div>
<div class="bg-white">White</div>
<div class="bg-gray-500">Gray 500</div>
<div class="bg-red-500">Red 500</div>
<div class="bg-blue-500">Blue 500</div>
<div class="bg-inherit">Inherit</div>
<div class="bg-current">Current</div>
<div class="bg-transparent">Transparent</div>

<!-- Avec opacité -->
<div class="bg-blue-500/50">Blue 500 à 50%</div>
<div class="bg-blue-500/75">Blue 500 à 75%</div>
```

### Background Image
```html
<!-- Gradients -->
<div class="bg-gradient-to-r">Gradient to right</div>
<div class="bg-gradient-to-l">Gradient to left</div>
<div class="bg-gradient-to-t">Gradient to top</div>
<div class="bg-gradient-to-b">Gradient to bottom</div>
<div class="bg-gradient-to-tr">Gradient to top-right</div>
<div class="bg-gradient-to-tl">Gradient to top-left</div>
<div class="bg-gradient-to-br">Gradient to bottom-right</div>
<div class="bg-gradient-to-bl">Gradient to bottom-left</div>

<!-- None -->
<div class="bg-none">No background image</div>
```

### Gradient Color Stops
```html
<div class="from-blue-500">From Blue 500</div>
<div class="via-purple-500">Via Purple 500</div>
<div class="to-pink-500">To Pink 500</div>

<!-- Exemple complet -->
<div class="bg-gradient-to-r from-blue-500 via-purple-500 to-pink-500">
  Gradient
</div>

<!-- Avec positions -->
<div class="from-blue-500 from-0%">From 0%</div>
<div class="from-blue-500 from-10%">From 10%</div>
<div class="via-purple-500 via-50%">Via 50%</div>
<div class="to-pink-500 to-100%">To 100%</div>
```

### Background Size
```html
<div class="bg-auto">auto</div>
<div class="bg-cover">cover</div>
<div class="bg-contain">contain</div>
```

### Background Position
```html
<div class="bg-center">center</div>
<div class="bg-top">top</div>
<div class="bg-right">right</div>
<div class="bg-bottom">bottom</div>
<div class="bg-left">left</div>
<div class="bg-top-right">top right</div>
<div class="bg-top-left">top left</div>
<div class="bg-bottom-right">bottom right</div>
<div class="bg-bottom-left">bottom left</div>
```

### Background Repeat
```html
<div class="bg-repeat">repeat</div>
<div class="bg-no-repeat">no-repeat</div>
<div class="bg-repeat-x">repeat-x</div>
<div class="bg-repeat-y">repeat-y</div>
<div class="bg-repeat-round">round</div>
<div class="bg-repeat-space">space</div>
```

### Background Attachment
```html
<div class="bg-fixed">fixed</div>
<div class="bg-local">local</div>
<div class="bg-scroll">scroll</div>
```

### Background Clip
```html
<div class="bg-clip-border">border-box</div>
<div class="bg-clip-padding">padding-box</div>
<div class="bg-clip-content">content-box</div>
<div class="bg-clip-text">text (pour gradient text)</div>
```

### Background Origin
```html
<div class="bg-origin-border">border-box</div>
<div class="bg-origin-padding">padding-box</div>
<div class="bg-origin-content">content-box</div>
```

---

## Borders

### Border Width
```html
<div class="border">1px (tous côtés)</div>
<div class="border-0">0</div>
<div class="border-2">2px</div>
<div class="border-4">4px</div>
<div class="border-8">8px</div>

<!-- Côtés individuels -->
<div class="border-t">top</div>
<div class="border-r">right</div>
<div class="border-b">bottom</div>
<div class="border-l">left</div>
<div class="border-x">horizontal</div>
<div class="border-y">vertical</div>

<!-- Start / End -->
<div class="border-s">start</div>
<div class="border-e">end</div>
```

### Border Color
```html
<div class="border-black">Black</div>
<div class="border-white">White</div>
<div class="border-gray-500">Gray 500</div>
<div class="border-red-500">Red 500</div>
<div class="border-blue-500">Blue 500</div>
<div class="border-inherit">Inherit</div>
<div class="border-current">Current</div>
<div class="border-transparent">Transparent</div>

<!-- Avec opacité -->
<div class="border-blue-500/50">Blue 500 à 50%</div>

<!-- Côtés individuels -->
<div class="border-t-red-500">Top red</div>
<div class="border-r-blue-500">Right blue</div>
<div class="border-b-green-500">Bottom green</div>
<div class="border-l-yellow-500">Left yellow</div>
```

### Border Style
```html
<div class="border-solid">Solid</div>
<div class="border-dashed">Dashed</div>
<div class="border-dotted">Dotted</div>
<div class="border-double">Double</div>
<div class="border-hidden">Hidden</div>
<div class="border-none">None</div>
```

### Border Radius
```html
<div class="rounded-none">0</div>
<div class="rounded-sm">0.125rem</div>
<div class="rounded">0.25rem</div>
<div class="rounded-md">0.375rem</div>
<div class="rounded-lg">0.5rem</div>
<div class="rounded-xl">0.75rem</div>
<div class="rounded-2xl">1rem</div>
<div class="rounded-3xl">1.5rem</div>
<div class="rounded-full">9999px (cercle)</div>

<!-- Coins individuels -->
<div class="rounded-t">top</div>
<div class="rounded-r">right</div>
<div class="rounded-b">bottom</div>
<div class="rounded-l">left</div>
<div class="rounded-tl">top-left</div>
<div class="rounded-tr">top-right</div>
<div class="rounded-br">bottom-right</div>
<div class="rounded-bl">bottom-left</div>

<!-- Start / End -->
<div class="rounded-s">start</div>
<div class="rounded-e">end</div>
<div class="rounded-ss">start-start</div>
<div class="rounded-se">start-end</div>
<div class="rounded-ee">end-end</div>
<div class="rounded-es">end-start</div>
```

### Divide (between elements)
```html
<!-- Width -->
<div class="divide-y">Divise verticalement</div>
<div class="divide-x">Divise horizontalement</div>
<div class="divide-y-2">2px vertical</div>
<div class="divide-x-reverse">Reverse horizontal</div>

<!-- Color -->
<div class="divide-gray-500">Gray divider</div>
<div class="divide-red-500/50">Red à 50%</div>

<!-- Style -->
<div class="divide-solid">Solid</div>
<div class="divide-dashed">Dashed</div>
<div class="divide-dotted">Dotted</div>
<div class="divide-double">Double</div>
<div class="divide-none">None</div>
```

### Outline
```html
<!-- Width -->
<div class="outline-0">0</div>
<div class="outline-1">1px</div>
<div class="outline-2">2px</div>
<div class="outline-4">4px</div>
<div class="outline-8">8px</div>

<!-- Color -->
<div class="outline-black">Black</div>
<div class="outline-white">White</div>
<div class="outline-red-500">Red 500</div>
<div class="outline-blue-500/50">Blue à 50%</div>

<!-- Style -->
<div class="outline-solid">Solid</div>
<div class="outline-dashed">Dashed</div>
<div class="outline-dotted">Dotted</div>
<div class="outline-double">Double</div>
<div class="outline-none">None</div>

<!-- Offset -->
<div class="outline-offset-0">0</div>
<div class="outline-offset-1">1px</div>
<div class="outline-offset-2">2px</div>
<div class="outline-offset-4">4px</div>
<div class="outline-offset-8">8px</div>
```

### Ring (focus rings)
```html
<!-- Width -->
<div class="ring">3px (default)</div>
<div class="ring-0">0</div>
<div class="ring-1">1px</div>
<div class="ring-2">2px</div>
<div class="ring-4">4px</div>
<div class="ring-8">8px</div>
<div class="ring-inset">inset</div>

<!-- Color -->
<div class="ring-blue-500">Blue 500</div>
<div class="ring-red-500/50">Red à 50%</div>

<!-- Offset -->
<div class="ring-offset-0">0</div>
<div class="ring-offset-1">1px</div>
<div class="ring-offset-2">2px</div>
<div class="ring-offset-4">4px</div>
<div class="ring-offset-8">8px</div>

<!-- Offset Color -->
<div class="ring-offset-white">White offset</div>
<div class="ring-offset-gray-500">Gray offset</div>
```

---

## Effects

### Box Shadow
```html
<div class="shadow-sm">Small shadow</div>
<div class="shadow">Default shadow</div>
<div class="shadow-md">Medium shadow</div>
<div class="shadow-lg">Large shadow</div>
<div class="shadow-xl">Extra large shadow</div>
<div class="shadow-2xl">2XL shadow</div>
<div class="shadow-inner">Inner shadow</div>
<div class="shadow-none">No shadow</div>

<!-- Shadow Color -->
<div class="shadow-red-500">Red shadow</div>
<div class="shadow-blue-500/50">Blue shadow à 50%</div>
```

### Opacity
```html
<div class="opacity-0">0%</div>
<div class="opacity-5">5%</div>
<div class="opacity-10">10%</div>
<div class="opacity-20">20%</div>
<div class="opacity-25">25%</div>
<div class="opacity-30">30%</div>
<div class="opacity-40">40%</div>
<div class="opacity-50">50%</div>
<div class="opacity-60">60%</div>
<div class="opacity-70">70%</div>
<div class="opacity-75">75%</div>
<div class="opacity-80">80%</div>
<div class="opacity-90">90%</div>
<div class="opacity-95">95%</div>
<div class="opacity-100">100%</div>
```

### Mix Blend Mode
```html
<div class="mix-blend-normal">normal</div>
<div class="mix-blend-multiply">multiply</div>
<div class="mix-blend-screen">screen</div>
<div class="mix-blend-overlay">overlay</div>
<div class="mix-blend-darken">darken</div>
<div class="mix-blend-lighten">lighten</div>
<div class="mix-blend-color-dodge">color-dodge</div>
<div class="mix-blend-color-burn">color-burn</div>
<div class="mix-blend-hard-light">hard-light</div>
<div class="mix-blend-soft-light">soft-light</div>
<div class="mix-blend-difference">difference</div>
<div class="mix-blend-exclusion">exclusion</div>
<div class="mix-blend-hue">hue</div>
<div class="mix-blend-saturation">saturation</div>
<div class="mix-blend-color">color</div>
<div class="mix-blend-luminosity">luminosity</div>
<div class="mix-blend-plus-lighter">plus-lighter</div>
```

### Background Blend Mode
```html
<div class="bg-blend-normal">normal</div>
<div class="bg-blend-multiply">multiply</div>
<div class="bg-blend-screen">screen</div>
<div class="bg-blend-overlay">overlay</div>
<div class="bg-blend-darken">darken</div>
<div class="bg-blend-lighten">lighten</div>
<div class="bg-blend-color-dodge">color-dodge</div>
<div class="bg-blend-color-burn">color-burn</div>
<div class="bg-blend-hard-light">hard-light</div>
<div class="bg-blend-soft-light">soft-light</div>
<div class="bg-blend-difference">difference</div>
<div class="bg-blend-exclusion">exclusion</div>
<div class="bg-blend-hue">hue</div>
<div class="bg-blend-saturation">saturation</div>
<div class="bg-blend-color">color</div>
<div class="bg-blend-luminosity">luminosity</div>
```

---

## Filters

### Blur
```html
<div class="blur-none">0</div>
<div class="blur-sm">4px</div>
<div class="blur">8px</div>
<div class="blur-md">12px</div>
<div class="blur-lg">16px</div>
<div class="blur-xl">24px</div>
<div class="blur-2xl">40px</div>
<div class="blur-3xl">64px</div>
```

### Brightness
```html
<div class="brightness-0">0</div>
<div class="brightness-50">0.5</div>
<div class="brightness-75">0.75</div>
<div class="brightness-90">0.9</div>
<div class="brightness-95">0.95</div>
<div class="brightness-100">1</div>
<div class="brightness-105">1.05</div>
<div class="brightness-110">1.1</div>
<div class="brightness-125">1.25</div>
<div class="brightness-150">1.5</div>
<div class="brightness-200">2</div>
```

### Contrast
```html
<div class="contrast-0">0</div>
<div class="contrast-50">0.5</div>
<div class="contrast-75">0.75</div>
<div class="contrast-100">1</div>
<div class="contrast-125">1.25</div>
<div class="contrast-150">1.5</div>
<div class="contrast-200">2</div>
```

### Drop Shadow
```html
<div class="drop-shadow-sm">Small</div>
<div class="drop-shadow">Default</div>
<div class="drop-shadow-md">Medium</div>
<div class="drop-shadow-lg">Large</div>
<div class="drop-shadow-xl">Extra large</div>
<div class="drop-shadow-2xl">2XL</div>
<div class="drop-shadow-none">None</div>
```

### Grayscale
```html
<div class="grayscale-0">0</div>
<div class="grayscale">100%</div>
```

### Hue Rotate
```html
<div class="hue-rotate-0">0deg</div>
<div class="hue-rotate-15">15deg</div>
<div class="hue-rotate-30">30deg</div>
<div class="hue-rotate-60">60deg</div>
<div class="hue-rotate-90">90deg</div>
<div class="hue-rotate-180">180deg</div>
<div class="-hue-rotate-60">-60deg</div>
```

### Invert
```html
<div class="invert-0">0</div>
<div class="invert">100%</div>
```

### Saturate
```html
<div class="saturate-0">0</div>
<div class="saturate-50">0.5</div>
<div class="saturate-100">1</div>
<div class="saturate-150">1.5</div>
<div class="saturate-200">2</div>
```

### Sepia
```html
<div class="sepia-0">0</div>
<div class="sepia">100%</div>
```

### Backdrop Filters
```html
<!-- Backdrop Blur -->
<div class="backdrop-blur-sm">Small</div>
<div class="backdrop-blur">Default</div>
<div class="backdrop-blur-lg">Large</div>

<!-- Backdrop Brightness -->
<div class="backdrop-brightness-50">50%</div>
<div class="backdrop-brightness-100">100%</div>
<div class="backdrop-brightness-150">150%</div>

<!-- Backdrop Contrast -->
<div class="backdrop-contrast-50">50%</div>
<div class="backdrop-contrast-100">100%</div>

<!-- Backdrop Grayscale -->
<div class="backdrop-grayscale">100%</div>

<!-- Backdrop Hue Rotate -->
<div class="backdrop-hue-rotate-90">90deg</div>

<!-- Backdrop Invert -->
<div class="backdrop-invert">100%</div>

<!-- Backdrop Opacity -->
<div class="backdrop-opacity-50">50%</div>

<!-- Backdrop Saturate -->
<div class="backdrop-saturate-150">150%</div>

<!-- Backdrop Sepia -->
<div class="backdrop-sepia">100%</div>
```

---

## Tables

### Border Collapse
```html
<table class="border-collapse">Collapse</table>
<table class="border-separate">Separate</table>
```

### Border Spacing
```html
<table class="border-spacing-0">0</table>
<table class="border-spacing-1">0.25rem</table>
<table class="border-spacing-2">0.5rem</table>
<table class="border-spacing-x-2">X: 0.5rem</table>
<table class="border-spacing-y-2">Y: 0.5rem</table>
```

### Table Layout
```html
<table class="table-auto">Auto</table>
<table class="table-fixed">Fixed</table>
```

### Caption Side
```html
<table class="caption-top">Top</table>
<table class="caption-bottom">Bottom</table>
```

---

## Transitions & Animation

### Transition Property
```html
<div class="transition-none">none</div>
<div class="transition-all">all</div>
<div class="transition">background, border, color, opacity, shadow, transform</div>
<div class="transition-colors">colors</div>
<div class="transition-opacity">opacity</div>
<div class="transition-shadow">shadow</div>
<div class="transition-transform">transform</div>
```

### Transition Duration
```html
<div class="duration-75">75ms</div>
<div class="duration-100">100ms</div>
<div class="duration-150">150ms</div>
<div class="duration-200">200ms</div>
<div class="duration-300">300ms</div>
<div class="duration-500">500ms</div>
<div class="duration-700">700ms</div>
<div class="duration-1000">1000ms</div>
```

### Transition Timing Function
```html
<div class="ease-linear">linear</div>
<div class="ease-in">ease-in</div>
<div class="ease-out">ease-out</div>
<div class="ease-in-out">ease-in-out</div>
```

### Transition Delay
```html
<div class="delay-75">75ms</div>
<div class="delay-100">100ms</div>
<div class="delay-150">150ms</div>
<div class="delay-200">200ms</div>
<div class="delay-300">300ms</div>
<div class="delay-500">500ms</div>
<div class="delay-700">700ms</div>
<div class="delay-1000">1000ms</div>
```

### Animation
```html
<div class="animate-none">None</div>
<div class="animate-spin">Spin (rotation infinie)</div>
<div class="animate-ping">Ping (pulse)</div>
<div class="animate-pulse">Pulse (opacité)</div>
<div class="animate-bounce">Bounce</div>
```

---

## Transforms

### Scale
```html
<div class="scale-0">0</div>
<div class="scale-50">50%</div>
<div class="scale-75">75%</div>
<div class="scale-90">90%</div>
<div class="scale-95">95%</div>
<div class="scale-100">100%</div>
<div class="scale-105">105%</div>
<div class="scale-110">110%</div>
<div class="scale-125">125%</div>
<div class="scale-150">150%</div>

<!-- X & Y -->
<div class="scale-x-50">X: 50%</div>
<div class="scale-y-50">Y: 50%</div>
```

### Rotate
```html
<div class="rotate-0">0deg</div>
<div class="rotate-1">1deg</div>
<div class="rotate-3">3deg</div>
<div class="rotate-6">6deg</div>
<div class="rotate-12">12deg</div>
<div class="rotate-45">45deg</div>
<div class="rotate-90">90deg</div>
<div class="rotate-180">180deg</div>
<div class="-rotate-45">-45deg</div>
```

### Translate
```html
<div class="translate-x-0">X: 0</div>
<div class="translate-x-4">X: 1rem</div>
<div class="translate-x-1/2">X: 50%</div>
<div class="translate-x-full">X: 100%</div>
<div class="-translate-x-4">X: -1rem</div>

<div class="translate-y-0">Y: 0</div>
<div class="translate-y-4">Y: 1rem</div>
<div class="translate-y-1/2">Y: 50%</div>
<div class="translate-y-full">Y: 100%</div>
<div class="-translate-y-4">Y: -1rem</div>
```

### Skew
```html
<div class="skew-x-0">X: 0deg</div>
<div class="skew-x-3">X: 3deg</div>
<div class="skew-x-6">X: 6deg</div>
<div class="skew-x-12">X: 12deg</div>
<div class="-skew-x-12">X: -12deg</div>

<div class="skew-y-0">Y: 0deg</div>
<div class="skew-y-3">Y: 3deg</div>
<div class="skew-y-6">Y: 6deg</div>
<div class="skew-y-12">Y: 12deg</div>
<div class="-skew-y-12">Y: -12deg</div>
```

### Transform Origin
```html
<div class="origin-center">center</div>
<div class="origin-top">top</div>
<div class="origin-top-right">top right</div>
<div class="origin-right">right</div>
<div class="origin-bottom-right">bottom right</div>
<div class="origin-bottom">bottom</div>
<div class="origin-bottom-left">bottom left</div>
<div class="origin-left">left</div>
<div class="origin-top-left">top left</div>
```

---

## Interactivity

### Cursor
```html
<div class="cursor-auto">Auto</div>
<div class="cursor-default">Default</div>
<div class="cursor-pointer">Pointer</div>
<div class="cursor-wait">Wait</div>
<div class="cursor-text">Text</div>
<div class="cursor-move">Move</div>
<div class="cursor-help">Help</div>
<div class="cursor-not-allowed">Not Allowed</div>
<div class="cursor-none">None</div>
<div class="cursor-context-menu">Context Menu</div>
<div class="cursor-progress">Progress</div>
<div class="cursor-cell">Cell</div>
<div class="cursor-crosshair">Crosshair</div>
<div class="cursor-vertical-text">Vertical Text</div>
<div class="cursor-alias">Alias</div>
<div class="cursor-copy">Copy</div>
<div class="cursor-no-drop">No Drop</div>
<div class="cursor-grab">Grab</div>
<div class="cursor-grabbing">Grabbing</div>
<div class="cursor-all-scroll">All Scroll</div>
<div class="cursor-col-resize">Col Resize</div>
<div class="cursor-row-resize">Row Resize</div>
<div class="cursor-n-resize">N Resize</div>
<div class="cursor-e-resize">E Resize</div>
<div class="cursor-s-resize">S Resize</div>
<div class="cursor-w-resize">W Resize</div>
<div class="cursor-ne-resize">NE Resize</div>
<div class="cursor-nw-resize">NW Resize</div>
<div class="cursor-se-resize">SE Resize</div>
<div class="cursor-sw-resize">SW Resize</div>
<div class="cursor-ew-resize">EW Resize</div>
<div class="cursor-ns-resize">NS Resize</div>
<div class="cursor-nesw-resize">NESW Resize</div>
<div class="cursor-nwse-resize">NWSE Resize</div>
<div class="cursor-zoom-in">Zoom In</div>
<div class="cursor-zoom-out">Zoom Out</div>
```

### Pointer Events
```html
<div class="pointer-events-none">None</div>
<div class="pointer-events-auto">Auto</div>
```

### Resize
```html
<div class="resize-none">None</div>
<div class="resize">Both</div>
<div class="resize-y">Vertical</div>
<div class="resize-x">Horizontal</div>
```

### Scroll Behavior
```html
<div class="scroll-auto">Auto</div>
<div class="scroll-smooth">Smooth</div>
```

### Scroll Margin
```html
<div class="scroll-m-0">0</div>
<div class="scroll-m-4">1rem</div>
<div class="scroll-mx-4">X: 1rem</div>
<div class="scroll-my-4">Y: 1rem</div>
<div class="scroll-mt-4">Top: 1rem</div>
<div class="scroll-mr-4">Right: 1rem</div>
<div class="scroll-mb-4">Bottom: 1rem</div>
<div class="scroll-ml-4">Left: 1rem</div>
```

### Scroll Padding
```html
<div class="scroll-p-0">0</div>
<div class="scroll-p-4">1rem</div>
<div class="scroll-px-4">X: 1rem</div>
<div class="scroll-py-4">Y: 1rem</div>
<div class="scroll-pt-4">Top: 1rem</div>
<div class="scroll-pr-4">Right: 1rem</div>
<div class="scroll-pb-4">Bottom: 1rem</div>
<div class="scroll-pl-4">Left: 1rem</div>
```

### Scroll Snap
```html
<!-- Container -->
<div class="snap-none">None</div>
<div class="snap-x">X axis</div>
<div class="snap-y">Y axis</div>
<div class="snap-both">Both</div>
<div class="snap-mandatory">Mandatory</div>
<div class="snap-proximity">Proximity</div>

<!-- Child -->
<div class="snap-start">Start</div>
<div class="snap-end">End</div>
<div class="snap-center">Center</div>
<div class="snap-align-none">None</div>
```

### Touch Action
```html
<div class="touch-auto">Auto</div>
<div class="touch-none">None</div>
<div class="touch-pan-x">Pan X</div>
<div class="touch-pan-left">Pan Left</div>
<div class="touch-pan-right">Pan Right</div>
<div class="touch-pan-y">Pan Y</div>
<div class="touch-pan-up">Pan Up</div>
<div class="touch-pan-down">Pan Down</div>
<div class="touch-pinch-zoom">Pinch Zoom</div>
<div class="touch-manipulation">Manipulation</div>
```

### User Select
```html
<div class="select-none">None</div>
<div class="select-text">Text</div>
<div class="select-all">All</div>
<div class="select-auto">Auto</div>
```

### Will Change
```html
<div class="will-change-auto">Auto</div>
<div class="will-change-scroll">Scroll</div>
<div class="will-change-contents">Contents</div>
<div class="will-change-transform">Transform</div>
```

---

## SVG

### Fill
```html
<svg class="fill-current">Current color</svg>
<svg class="fill-red-500">Red 500</svg>
<svg class="fill-blue-500/50">Blue à 50%</svg>
<svg class="fill-none">None</svg>
```

### Stroke
```html
<svg class="stroke-current">Current color</svg>
<svg class="stroke-red-500">Red 500</svg>
<svg class="stroke-blue-500/50">Blue à 50%</svg>
<svg class="stroke-none">None</svg>
```

### Stroke Width
```html
<svg class="stroke-0">0</svg>
<svg class="stroke-1">1</svg>
<svg class="stroke-2">2</svg>
```

---

## Accessibility

### Screen Readers
```html
<div class="sr-only">Visible seulement pour les lecteurs d'écran</div>
<div class="not-sr-only">Visible pour tous</div>
```

### Forced Color Adjust
```html
<div class="forced-color-adjust-auto">Auto</div>
<div class="forced-color-adjust-none">None</div>
```

---

## Responsive Design

### Breakpoints
```
sm: 640px   @media (min-width: 640px)
md: 768px   @media (min-width: 768px)
lg: 1024px  @media (min-width: 1024px)
xl: 1280px  @media (min-width: 1280px)
2xl: 1536px @media (min-width: 1536px)
```

### Utilisation
```html
<!-- Mobile first -->
<div class="w-full md:w-1/2 lg:w-1/3">
  Pleine largeur sur mobile, 50% sur tablette, 33% sur desktop
</div>

<!-- Exemples -->
<div class="text-sm sm:text-base md:text-lg lg:text-xl">
  Taille de texte responsive
</div>

<div class="flex flex-col md:flex-row">
  Colonne sur mobile, ligne sur tablette+
</div>

<div class="hidden md:block">
  Caché sur mobile, visible sur tablette+
</div>

<div class="block md:hidden">
  Visible sur mobile, caché sur tablette+
</div>

<div class="p-4 md:p-8 lg:p-12">
  Padding adaptatif
</div>
```

### Max Width Responsive
```html
<div class="max-w-sm sm:max-w-md md:max-w-lg lg:max-w-xl">
  Max width responsive
</div>
```

---

## Dark Mode

### Configuration (tailwind.config.js)
```javascript
module.exports = {
  darkMode: 'class', // ou 'media'
  // ...
}
```

### Utilisation avec class
```html
<!-- Ajouter 'dark' à la balise html ou body -->
<html class="dark">

<!-- Utilisation -->
<div class="bg-white dark:bg-gray-800">
  Fond blanc en mode clair, gris foncé en mode sombre
</div>

<p class="text-black dark:text-white">
  Texte noir en clair, blanc en sombre
</p>

<button class="bg-blue-500 hover:bg-blue-600 dark:bg-blue-600 dark:hover:bg-blue-700">
  Bouton avec états hover en mode clair et sombre
</button>
```

### Exemples complets
```html
<!-- Card responsive dark mode -->
<div class="bg-white dark:bg-gray-900 rounded-lg shadow-lg dark:shadow-2xl p-6">
  <h2 class="text-2xl font-bold text-gray-900 dark:text-white">
    Titre
  </h2>
  <p class="text-gray-600 dark:text-gray-300">
    Description
  </p>
</div>

<!-- Input avec dark mode -->
<input 
  class="bg-white dark:bg-gray-800 border border-gray-300 dark:border-gray-600 text-gray-900 dark:text-white"
  type="text"
/>
```

---

## Pseudo-classes

### Hover
```html
<div class="hover:bg-blue-500">Background au hover</div>
<div class="hover:text-red-500">Texte rouge au hover</div>
<div class="hover:scale-110">Scale au hover</div>
```

### Focus
```html
<input class="focus:ring-2 focus:ring-blue-500" />
<input class="focus:border-blue-500" />
<input class="focus:outline-none" />
<button class="focus-visible:ring-2">Focus visible</button>
<div class="focus-within:ring-2">Focus within</div>
```

### Active
```html
<button class="active:bg-blue-700">Background au click</button>
<button class="active:scale-95">Scale au click</button>
```

### Disabled
```html
<button class="disabled:opacity-50 disabled:cursor-not-allowed">
  Bouton désactivé
</button>
```

### Visited
```html
<a class="visited:text-purple-600" href="#">Lien visité</a>
```

### Checked
```html
<input type="checkbox" class="checked:bg-blue-500" />
```

### Required / Optional
```html
<input class="required:border-red-500" required />
<input class="optional:border-gray-300" />
```

### Invalid / Valid
```html
<input class="invalid:border-red-500" />
<input class="valid:border-green-500" />
```

### Empty
```html
<div class="empty:hidden">Caché si vide</div>
```

### First / Last / Only
```html
<li class="first:font-bold">Premier élément</li>
<li class="last:border-b-0">Dernier élément</li>
<li class="only:text-center">Seul élément</li>
```

### Odd / Even
```html
<tr class="odd:bg-gray-100">Ligne impaire</tr>
<tr class="even:bg-white">Ligne paire</tr>
```

### Group Hover
```html
<div class="group">
  <div class="group-hover:text-blue-500">
    Change quand le parent est hover
  </div>
</div>
```

### Peer
```html
<input type="checkbox" class="peer" />
<div class="peer-checked:bg-blue-500">
  Change quand l'input peer est checked
</div>
```

### Has
```html
<div class="has-[:checked]:bg-blue-100">
  Style si contient un élément :checked
</div>
```

---

## Pseudo-elements

### Before / After
```html
<div class="before:content-['→'] before:mr-2">
  Texte avec flèche avant
</div>

<div class="after:content-['*'] after:text-red-500 after:ml-1">
  Champ requis
</div>

<div class="before:block before:h-px before:bg-gray-300">
  Ligne avant
</div>
```

### First Letter / First Line
```html
<p class="first-letter:text-4xl first-letter:font-bold">
  Première lettre stylisée
</p>

<p class="first-line:uppercase first-line:tracking-widest">
  Première ligne en majuscules
</p>
```

### Placeholder
```html
<input 
  class="placeholder:text-gray-400 placeholder:italic" 
  placeholder="Entrez votre texte"
/>
```

### Selection
```html
<p class="selection:bg-blue-200 selection:text-blue-900">
  Texte avec sélection stylisée
</p>
```

### Marker (listes)
```html
<ul>
  <li class="marker:text-blue-500">Item 1</li>
  <li class="marker:text-red-500">Item 2</li>
</ul>
```

### File Input
```html
<input 
  type="file" 
  class="file:mr-4 file:py-2 file:px-4 file:rounded file:border-0 file:bg-blue-50 file:text-blue-700"
/>
```

---

## Colors

### Palette complète (50-950)
Chaque couleur existe en 11 nuances : 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950

```html
<!-- Slate -->
<div class="bg-slate-50">Slate 50 (le plus clair)</div>
<div class="bg-slate-500">Slate 500 (moyen)</div>
<div class="bg-slate-950">Slate 950 (le plus foncé)</div>

<!-- Gray -->
<div class="bg-gray-500">Gray</div>

<!-- Zinc -->
<div class="bg-zinc-500">Zinc</div>

<!-- Neutral -->
<div class="bg-neutral-500">Neutral</div>

<!-- Stone -->
<div class="bg-stone-500">Stone</div>

<!-- Red -->
<div class="bg-red-500">Red</div>

<!-- Orange -->
<div class="bg-orange-500">Orange</div>

<!-- Amber -->
<div class="bg-amber-500">Amber</div>

<!-- Yellow -->
<div class="bg-yellow-500">Yellow</div>

<!-- Lime -->
<div class="bg-lime-500">Lime</div>

<!-- Green -->
<div class="bg-green-500">Green</div>

<!-- Emerald -->
<div class="bg-emerald-500">Emerald</div>

<!-- Teal -->
<div class="bg-teal-500">Teal</div>

<!-- Cyan -->
<div class="bg-cyan-500">Cyan</div>

<!-- Sky -->
<div class="bg-sky-500">Sky</div>

<!-- Blue -->
<div class="bg-blue-500">Blue</div>

<!-- Indigo -->
<div class="bg-indigo-500">Indigo</div>

<!-- Violet -->
<div class="bg-violet-500">Violet</div>

<!-- Purple -->
<div class="bg-purple-500">Purple</div>

<!-- Fuchsia -->
<div class="bg-fuchsia-500">Fuchsia</div>

<!-- Pink -->
<div class="bg-pink-500">Pink</div>

<!-- Rose -->
<div class="bg-rose-500">Rose</div>
```

### Couleurs spéciales
```html
<div class="bg-black">Black</div>
<div class="bg-white">White</div>
<div class="bg-transparent">Transparent</div>
<div class="bg-current">Current Color</div>
<div class="bg-inherit">Inherit</div>
```

---

## Customization

### Extend dans tailwind.config.js
```javascript
module.exports = {
  theme: {
    extend: {
      colors: {
        'brand': '#ff6b6b',
        'brand-dark': '#ee5a52',
      },
      spacing: {
        '128': '32rem',
        '144': '36rem',
      },
      borderRadius: {
        '4xl': '2rem',
      },
      fontFamily: {
        'custom': ['Custom Font', 'sans-serif'],
      },
      fontSize: {
        'xxs': '0.625rem',
      },
      maxWidth: {
        '8xl': '88rem',
        '9xl': '96rem',
      },
      screens: {
        '3xl': '1920px',
      },
      animation: {
        'spin-slow': 'spin 3s linear infinite',
        'wiggle': 'wiggle 1s ease-in-out infinite',
      },
      keyframes: {
        wiggle: {
          '0%, 100%': { transform: 'rotate(-3deg)' },
          '50%': { transform: 'rotate(3deg)' },
        }
      }
    },
  },
}
```

### Remplacer des valeurs
```javascript
module.exports = {
  theme: {
    // Remplace complètement
    colors: {
      blue: '#1fb6ff',
      purple: '#7e5bef',
      pink: '#ff49db',
    },
    // Étendre (recommandé)
    extend: {
      colors: {
        custom: '#123456',
      }
    }
  }
}
```

---

## Plugins

### Plugins officiels
```bash
npm install @tailwindcss/forms
npm install @tailwindcss/typography
npm install @tailwindcss/aspect-ratio
npm install @tailwindcss/container-queries
```

### Configuration
```javascript
module.exports = {
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
    require('@tailwindcss/aspect-ratio'),
    require('@tailwindcss/container-queries'),
  ],
}
```

### Utilisation

#### Forms Plugin
```html
<input type="text" class="form-input" />
<textarea class="form-textarea"></textarea>
<select class="form-select"></select>
<input type="checkbox" class="form-checkbox" />
<input type="radio" class="form-radio" />
```

#### Typography Plugin
```html
<article class="prose lg:prose-xl">
  <h1>Titre</h1>
  <p>Contenu avec styles par défaut...</p>
</article>

<article class="prose prose-slate">Thème slate</article>
<article class="prose prose-lg">Grande taille</article>
<article class="prose dark:prose-invert">Dark mode</article>
```

#### Aspect Ratio Plugin
```html
<div class="aspect-w-16 aspect-h-9">
  <iframe src="..."></iframe>
</div>
```

---

## Best Practices

### Organisation des classes
```html
<!-- Ordre recommandé -->
<div class="
  /* Layout */
  flex items-center justify-between
  /* Spacing */
  p-4 m-2
  /* Sizing */
  w-full h-auto
  /* Typography */
  text-lg font-bold
  /* Visual */
  bg-blue-500 border border-gray-300 rounded-lg
  /* Effects */
  shadow-lg
  /* Interactions */
  hover:bg-blue-600 transition-colors
">
  Contenu
</div>
```

### Composants réutilisables
```html
<!-- Utilisez @apply dans CSS pour les composants -->
<style>
  .btn-primary {
    @apply bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 transition-colors;
  }
</style>
```

### Patterns communs

#### Card
```html
<div class="bg-white rounded-lg shadow-md p-6 hover:shadow-xl transition-shadow">
  <h3 class="text-xl font-bold mb-2">Titre</h3>
  <p class="text-gray-600">Description</p>
</div>
```

#### Button
```html
<button class="bg-blue-500 hover:bg-blue-600 active:bg-blue-700 text-white font-semibold px-6 py-3 rounded-lg transition-colors disabled:opacity-50 disabled:cursor-not-allowed">
  Cliquez-moi
</button>
```

#### Input
```html
<input 
  type="text"
  class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
  placeholder="Entrez votre texte"
/>
```

#### Modal
```html
<div class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4">
  <div class="bg-white rounded-lg shadow-xl max-w-md w-full p-6">
    <h2 class="text-2xl font-bold mb-4">Modal</h2>
    <p class="text-gray-600 mb-6">Contenu du modal</p>
    <div class="flex justify-end space-x-2">
      <button class="px-4 py-2 border border-gray-300 rounded hover:bg-gray-50">
        Annuler
      </button>
      <button class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
        Confirmer
      </button>
    </div>
  </div>
</div>
```

#### Navigation
```html
<nav class="bg-white shadow-lg">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="flex justify-between h-16">
      <div class="flex items-center">
        <a href="#" class="text-xl font-bold">Logo</a>
      </div>
      <div class="flex items-center space-x-4">
        <a href="#" class="text-gray-700 hover:text-blue-500 transition-colors">
          Accueil
        </a>
        <a href="#" class="text-gray-700 hover:text-blue-500 transition-colors">
          À propos
        </a>
        <a href="#" class="text-gray-700 hover:text-blue-500 transition-colors">
          Contact
        </a>
      </div>
    </div>
  </div>
</nav>
```

---

## Nouveautés Tailwind v3+

### Couleurs étendues (50-950)
Toutes les couleurs ont maintenant 11 nuances au lieu de 9, avec l'ajout de 50 et 950.

### Dynamic viewport units
```html
<div class="h-svh">Small Viewport Height</div>
<div class="h-lvh">Large Viewport Height</div>
<div class="h-dvh">Dynamic Viewport Height</div>
```

### Colored box shadows
```html
<div class="shadow-lg shadow-blue-500/50">Shadow colorée</div>
<div class="shadow-xl shadow-red-500/30">Shadow rouge</div>
```

### Multi-column layout
```html
<div class="columns-2">Deux colonnes</div>
<div class="columns-3 md:columns-4">Responsive</div>
<div class="columns-xs">Colonnes auto (xs)</div>
<div class="columns-sm">Colonnes auto (sm)</div>
<div class="columns-md">Colonnes auto (md)</div>
```

### Scroll snap utilities
```html
<div class="snap-x snap-mandatory">
  <div class="snap-center">Item 1</div>
  <div class="snap-center">Item 2</div>
</div>
```

### Accent color
```html
<input type="checkbox" class="accent-blue-500" />
<input type="radio" class="accent-red-500" />
```

### Caret color
```html
<input class="caret-blue-500" />
<textarea class="caret-red-500"></textarea>
```

### Text decoration thickness
```html
<p class="underline decoration-1">Thin underline</p>
<p class="underline decoration-4">Thick underline</p>
```

### Text underline offset
```html
<p class="underline underline-offset-4">Offset underline</p>
<p class="underline underline-offset-8">More offset</p>
```

### Arbitrary variants
```html
<div class="[&>*]:p-4">Tous les enfants ont p-4</div>
<div class="[&:nth-child(3)]:bg-blue-500">3e enfant bleu</div>
```

### Native form controls
Les inputs, selects et autres ont maintenant des styles par défaut améliorés.

---

## Arbitrary Values

### Syntaxe
```html
<!-- Dimensions -->
<div class="w-[32rem]">Width personnalisée</div>
<div class="h-[calc(100vh-4rem)]">Height avec calc</div>
<div class="top-[117px]">Position exacte</div>

<!-- Couleurs -->
<div class="bg-[#1da1f2]">Couleur hex</div>
<div class="text-[rgb(123,45,67)]">RGB</div>
<div class="border-[hsl(210,100%,50%)]">HSL</div>

<!-- Spacing -->
<div class="p-[17px]">Padding exact</div>
<div class="m-[3.25rem]">Margin exact</div>

<!-- Font -->
<p class="text-[length:14px]">Taille exacte</p>
<p class="leading-[1.7]">Line height exact</p>

<!-- Grid -->
<div class="grid-cols-[200px_minmax(0,1fr)_100px]">
  Grid template custom
</div>

<!-- Autres -->
<div class="rotate-[23deg]">Rotation exacte</div>
<div class="rounded-[13px]">Border radius exact</div>
```

### Propriétés CSS arbitraires
```html
<div class="[mask-type:luminance]">
  Propriété CSS quelconque
</div>

<div class="[--my-custom-property:10px] p-[var(--my-custom-property)]">
  Variables CSS custom
</div>
```

---

## Container Queries

### Installation
```bash
npm install @tailwindcss/container-queries
```

### Utilisation
```html
<div class="@container">
  <div class="@lg:text-2xl @md:p-4">
    Change selon la taille du conteneur
  </div>
</div>

<!-- Breakpoints de container -->
@xs: 320px
@sm: 384px
@md: 448px
@lg: 512px
@xl: 576px
@2xl: 672px
@3xl: 768px
@4xl: 896px
@5xl: 1024px
@6xl: 1152px
@7xl: 1280px
```

### Container nommé
```html
<div class="@container/sidebar">
  <div class="@lg/sidebar:col-span-2">
    Responsive au conteneur "sidebar"
  </div>
</div>
```

---

## Dynamic Classes

### Attention avec les classes dynamiques
```html
<!-- ❌ Ne fonctionnera PAS (Tailwind ne peut pas voir ces classes) -->
<div class="text-{{ color }}-500">Wrong</div>
<div class="bg-{{ background }}">Wrong</div>

<!-- ✅ Utilisez des classes complètes -->
<div class="{{ isActive ? 'text-blue-500' : 'text-gray-500' }}">Correct</div>

<!-- ✅ Ou utilisez safelist dans config -->
```

### Safelist dans config
```javascript
module.exports = {
  safelist: [
    'text-red-500',
    'text-blue-500',
    {
      pattern: /bg-(red|green|blue)-(100|200|300)/,
    },
  ],
}
```

### Classes dynamiques avec template literals
```html
<!-- ✅ Correct -->
<div class="${active ? 'bg-blue-500' : 'bg-gray-500'} p-4 rounded">
  Content
</div>

<!-- ✅ Avec librairie clsx/classnames -->
<div class={clsx(
  'p-4 rounded',
  active && 'bg-blue-500',
  !active && 'bg-gray-500'
)}>
  Content
</div>
```

---

**Ressources supplémentaires:**
- Documentation officielle: https://tailwindcss.com/docs
- Tailwind UI: https://tailwindui.com/
- Tailwind Components: https://tailwindcomponents.com/
- Play CDN: https://tailwindcss.com/docs/installation/play-cdn
