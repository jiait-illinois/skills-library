# Overlay Components

Components that appear on top of page content.

> **Accessibility note (required):** All modal overlays in this file (slideovers, `<ilw-modal>`, and the alert dialog documented in `FEEDBACK.md`) MUST trap focus while open and close on `Escape`. These behaviors are defined authoritatively in `ACCESSIBILITY.md` §1 — consult it before generating any overlay. The `open*()` helpers initialize focus traps automatically; when authoring overlay markup manually, implement the focus trap and `Escape` handler yourself. The close-button markup shown in the examples below is not sufficient on its own.

## 1. Tooltips
The `[<ilw-tooltip>](https://builder3.toolkit.illinois.edu/component/ilw-tooltip/index.html)` component is a popup that provides additional information about a user interface element when it is hovered over or focused.

**Attributes:**
- `theme`: Color theme used. Supported value: `blue` (defaults to standard style if omitted).
- `arrow`: Position of the arrow and tooltip. Supported values:
  - `top-left`, `top-center`, `top-right`
  - `left`, `right`
  - `bottom-left`, `bottom-center`, `bottom-right`

**Slots:**
- `trigger`: The interactive element (e.g., a link `<a>` or button `<button>`) that triggers the tooltip.
- `content`: The text or HTML content shown inside the tooltip (e.g., supporting `<strong>`, `<br>`).

The trigger must be a genuinely interactive element: a real link (an `<a>` with a meaningful `href`) or a `<button>`. Never use `href="javascript:void(0)"` — a link that navigates nowhere is an accessibility anti-pattern; use a `<button type="button">` instead when there is no destination.

**Example:**
```html
<!-- Default tooltip with a link trigger -->
<ilw-tooltip>
    <a href="/glossary/provost" slot="trigger">Provost</a>
    <span slot="content">The chief academic officer of the university.</span>
</ilw-tooltip>

<!-- Button trigger with theme and arrow configuration -->
<ilw-tooltip theme="blue" arrow="top-center">
    <button slot="trigger" class="ilw-button">Hover over me</button>
    <span slot="content">
        <strong>Some content in bold.</strong><br>This is additional information.
    </span>
</ilw-tooltip>
```

## 2. Modal
The [`<ilw-modal>`](https://builder3.toolkit.illinois.edu/component/ilw-modal/index.html) component provides a dialog box displayed on top of the page for critical information or user input.

**Attributes:**
- `id` (Required): Unique identifier used for triggering.
- `size`: `small` (35% width), `medium` (Default, 50%), `large` (1200px).
- `theme`: `white` (Default), `gray`.
- `align`: `left` (Default), `center`.

**Slots:**
- `title`: The heading of the modal.
- `image`: Optional hero image displayed at the top.
- Default slot: The main body content.

**Triggering:**
Place the `data-modal-target` attribute on any button or link, set to the `id` of the modal.

**Example:**
```html
<!-- Trigger -->
<button class="ilw-button" data-modal-target="myModal">Open Modal</button>

<!-- Modal -->
<ilw-modal id="myModal" size="small" theme="white" align="center">
    <img src="image.jpg" alt="" slot="image">
    <h2 slot="title">Subscription</h2>
    <p>Please enter your email to subscribe to our newsletter.</p>
    <form>
        <input class="" type="email" placeholder="email@illinois.edu">
        <button class="ilw-button" type="submit">Submit</button>
    </form>
</ilw-modal>
```