# CSS Fundamentals - Complete Styling Guide

> Master CSS layouts, flexbox, grid, animations, responsive design, and common interview concepts

## 📋 Table of Contents

- [What is CSS?](#what-is-css)
- [CSS Syntax & Selectors](#css-syntax--selectors)
- [Box Model](#box-model)
- [Layout Systems](#layout-systems)
  - [Flexbox](#flexbox)
  - [Grid](#grid)
- [Positioning](#positioning)
- [Responsive Design](#responsive-design)
- [Animations & Transitions](#animations--transitions)
- [Common Interview Questions](#common-interview-questions)
- [Best Practices](#best-practices)

---

## What is CSS?

**CSS (Cascading Style Sheets)** controls the visual presentation of HTML elements. It separates content from design.

### Three Ways to Add CSS

```html
<!-- 1. Inline (avoid for maintainability) -->
<p style="color: blue;">Text</p>

<!-- 2. Internal (in <head>) -->
<style>
    p { color: blue; }
</style>

<!-- 3. External (recommended) -->
<link rel="stylesheet" href="styles.css">
```

---

## CSS Syntax & Selectors

### Basic Syntax

```css
selector {
    property: value;
    another-property: value;
}
```

### Common Selectors

```css
/* Element selector */
p { color: black; }

/* Class selector */
.button { background: blue; }

/* ID selector (higher specificity) */
#header { width: 100%; }

/* Descendant selector */
nav a { color: white; }

/* Direct child */
ul > li { list-style: none; }

/* Adjacent sibling */
h1 + p { margin-top: 0; }

/* Attribute selector */
input[type="email"] { border: 1px solid gray; }

/* Pseudo-class */
a:hover { color: red; }
li:first-child { font-weight: bold; }
li:nth-child(2) { color: blue; }

/* Pseudo-element */
p::before { content: "→ "; }
p::first-letter { font-size: 2em; }
```

### Specificity (Most to Least Specific)

1. Inline styles: `style="..."`
2. IDs: `#header`
3. Classes, attributes, pseudo-classes: `.button`, `[type]`, `:hover`
4. Elements: `p`, `div`

**Example**: `#nav .button:hover` beats `.button`

---

## Box Model

Every element is a box with 4 parts:

```
┌─────────────────────────────┐
│        Margin               │  ← Outside spacing (transparent)
│  ┌──────────────────────┐   │
│  │     Border           │   │  ← Border line
│  │  ┌───────────────┐   │   │
│  │  │   Padding     │   │   │  ← Inside spacing
│  │  │  ┌────────┐   │   │   │
│  │  │  │Content │   │   │   │  ← Actual content
│  │  │  └────────┘   │   │   │
│  │  └───────────────┘   │   │
│  └──────────────────────┘   │
└─────────────────────────────┘
```

### Example

```css
.box {
    width: 200px;           /* Content width */
    height: 100px;          /* Content height */
    padding: 20px;          /* Space inside */
    border: 2px solid black; /* Border */
    margin: 10px;           /* Space outside */
}
/* Total width = 200 + (20*2) + (2*2) + (10*2) = 264px */
```

### Box-Sizing

```css
/* Default: width = content only */
.box { box-sizing: content-box; }

/* Better: width = content + padding + border */
.box { box-sizing: border-box; }

/* Apply to all elements (recommended) */
* {
    box-sizing: border-box;
}
```

---

## Layout Systems

### Flexbox

**Best for**: One-dimensional layouts (rows OR columns)

#### Container Properties

```css
.container {
    display: flex;

    /* Direction */
    flex-direction: row;  /* row | column | row-reverse | column-reverse */

    /* Main axis alignment (horizontal if row) */
    justify-content: center; /* flex-start | flex-end | center | space-between | space-around | space-evenly */

    /* Cross axis alignment (vertical if row) */
    align-items: center; /* flex-start | flex-end | center | stretch | baseline */

    /* Wrap items */
    flex-wrap: wrap; /* nowrap | wrap | wrap-reverse */

    /* Gap between items */
    gap: 20px;
}
```

#### Item Properties

```css
.item {
    /* Grow factor */
    flex-grow: 1;   /* Take available space */

    /* Shrink factor */
    flex-shrink: 1; /* Shrink if needed */

    /* Base size */
    flex-basis: 200px;

    /* Shorthand */
    flex: 1 1 200px; /* grow shrink basis */

    /* Override alignment */
    align-self: flex-end;
}
```

#### Common Patterns

```css
/* Center anything */
.center {
    display: flex;
    justify-content: center;
    align-items: center;
}

/* Equal width columns */
.column {
    flex: 1;
}

/* Responsive navigation */
.nav {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
}
```

---

### Grid

**Best for**: Two-dimensional layouts (rows AND columns)

#### Basic Grid

```css
.grid {
    display: grid;

    /* Define columns */
    grid-template-columns: 200px 1fr 1fr; /* 3 columns: fixed, equal, equal */
    grid-template-columns: repeat(3, 1fr); /* 3 equal columns */
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); /* Responsive */

    /* Define rows */
    grid-template-rows: 100px auto 50px;

    /* Gap between items */
    gap: 20px;
    row-gap: 10px;
    column-gap: 20px;
}
```

#### Placing Items

```css
.item {
    /* Start at column 1, span 2 columns */
    grid-column: 1 / 3;
    /* or */
    grid-column: 1 / span 2;

    /* Start at row 2, span 1 row */
    grid-row: 2 / 3;
}
```

#### Named Areas

```css
.grid {
    display: grid;
    grid-template-areas:
        "header header header"
        "sidebar main main"
        "footer footer footer";
    grid-template-columns: 200px 1fr 1fr;
}

.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main { grid-area: main; }
.footer { grid-area: footer; }
```

---

## Positioning

```css
/* Static (default) - normal flow */
position: static;

/* Relative - offset from normal position */
position: relative;
top: 10px;
left: 20px;

/* Absolute - positioned relative to nearest positioned ancestor */
position: absolute;
top: 0;
right: 0;

/* Fixed - positioned relative to viewport */
position: fixed;
bottom: 20px;
right: 20px;

/* Sticky - switches between relative and fixed */
position: sticky;
top: 0; /* Sticks when scrolled to top */
```

### Z-Index

```css
/* Controls stacking order (only works with positioned elements) */
.modal {
    position: fixed;
    z-index: 100;
}

.overlay {
    position: fixed;
    z-index: 99;
}
```

---

## Responsive Design

### Media Queries

```css
/* Mobile-first approach (recommended) */

/* Base styles for mobile */
.container {
    width: 100%;
    padding: 10px;
}

/* Tablet and up */
@media (min-width: 768px) {
    .container {
        width: 750px;
        margin: 0 auto;
    }
}

/* Desktop and up */
@media (min-width: 1024px) {
    .container {
        width: 960px;
    }
}

/* Large desktop */
@media (min-width: 1200px) {
    .container {
        width: 1140px;
    }
}
```

### Common Breakpoints

```css
/* Phone */
@media (max-width: 767px) { }

/* Tablet */
@media (min-width: 768px) and (max-width: 1023px) { }

/* Desktop */
@media (min-width: 1024px) { }
```

### Responsive Units

```css
/* Relative to parent */
width: 50%;

/* Relative to viewport */
width: 50vw;  /* 50% of viewport width */
height: 100vh; /* 100% of viewport height */

/* Relative to font size */
padding: 1em;  /* 1x current font size */
padding: 1rem; /* 1x root font size (usually 16px) */

/* Clamp (responsive sizing) */
font-size: clamp(16px, 4vw, 24px); /* min, preferred, max */
```

---

## Animations & Transitions

### Transitions

```css
.button {
    background: blue;
    transition: background 0.3s ease;
}

.button:hover {
    background: darkblue;
}

/* Multiple properties */
.box {
    transition: all 0.3s ease-in-out;
    /* or specific */
    transition:
        background 0.3s ease,
        transform 0.2s ease-out;
}
```

### Animations

```css
/* Define animation */
@keyframes slideIn {
    from {
        transform: translateX(-100%);
        opacity: 0;
    }
    to {
        transform: translateX(0);
        opacity: 1;
    }
}

/* Apply animation */
.element {
    animation: slideIn 0.5s ease-out;
}

/* Advanced */
.element {
    animation-name: slideIn;
    animation-duration: 1s;
    animation-timing-function: ease-in-out;
    animation-delay: 0.5s;
    animation-iteration-count: infinite;
    animation-direction: alternate;
}
```

### Transform

```css
/* Move */
transform: translateX(100px);
transform: translateY(50px);
transform: translate(100px, 50px);

/* Scale */
transform: scale(1.5);
transform: scale(2, 0.5); /* width, height */

/* Rotate */
transform: rotate(45deg);

/* Combine */
transform: translateX(50px) rotate(45deg) scale(1.2);
```

---

## Common Interview Questions

### 1. What is the box model?
**Answer**: The box model describes how element dimensions are calculated: content + padding + border + margin. Use `box-sizing: border-box` to include padding/border in width.

### 2. Flexbox vs Grid - when to use each?
**Answer**:
- **Flexbox**: One-dimensional (row or column), content-first, dynamic sizing
- **Grid**: Two-dimensional (rows and columns), layout-first, fixed grid structure

### 3. Explain CSS specificity
**Answer**: Determines which style wins when multiple rules target same element:
Inline (1000) > ID (100) > Class/Attribute (10) > Element (1)

### 4. What is the cascade in CSS?
**Answer**: When multiple rules apply, CSS uses specificity, source order, and importance (`!important`) to determine which rule wins.

### 5. How do you center a div?
**Answer**: Multiple ways:
```css
/* Flexbox (preferred) */
.parent {
    display: flex;
    justify-content: center;
    align-items: center;
}

/* Grid */
.parent {
    display: grid;
    place-items: center;
}

/* Absolute + Transform */
.child {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

### 6. What are pseudo-classes and pseudo-elements?
**Answer**:
- **Pseudo-class** (`:hover`, `:first-child`): Targets element state
- **Pseudo-element** (`::before`, `::after`): Targets part of element or creates virtual element

### 7. Explain `em` vs `rem`
**Answer**:
- `em`: Relative to parent font size (compounds)
- `rem`: Relative to root (`<html>`) font size (consistent)

### 8. What is mobile-first design?
**Answer**: Writing base CSS for mobile, then using `min-width` media queries to add styles for larger screens. Results in better performance and progressive enhancement.

### 9. How does CSS Grid differ from tables?
**Answer**: Grid is for layout, tables for tabular data. Grid is flexible, responsive, and doesn't require HTML structure changes. Tables are semantic for data.

### 10. What is the difference between `display: none` and `visibility: hidden`?
**Answer**:
- `display: none`: Removes from layout, no space taken
- `visibility: hidden`: Hidden but space remains

---

## Best Practices

### 1. Use CSS Reset or Normalize
```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
```

### 2. Use CSS Variables
```css
:root {
    --primary-color: #007bff;
    --spacing: 16px;
    --border-radius: 4px;
}

.button {
    background: var(--primary-color);
    padding: var(--spacing);
    border-radius: var(--border-radius);
}
```

### 3. BEM Naming Convention
```css
/* Block */
.card { }

/* Element */
.card__title { }
.card__image { }

/* Modifier */
.card--featured { }
```

### 4. Avoid `!important`
Only use when absolutely necessary (utility classes, overriding inline styles)

### 5. Mobile-First Responsive Design
```css
/* Base (mobile) */
.container { width: 100%; }

/* Larger screens */
@media (min-width: 768px) {
    .container { width: 750px; }
}
```

---

## Resources

- [CSS-Tricks](https://css-tricks.com/)
- [MDN CSS Reference](https://developer.mozilla.org/en-US/docs/Web/CSS)
- [Flexbox Froggy](https://flexboxfroggy.com/) - Learn Flexbox
- [Grid Garden](https://cssgridgarden.com/) - Learn Grid
- [Can I Use](https://caniuse.com/) - Browser support

---

**Keywords**: CSS tutorial, flexbox, CSS grid, responsive design, CSS animations, CSS interview questions, web design, CSS layout, mobile-first, CSS best practices
