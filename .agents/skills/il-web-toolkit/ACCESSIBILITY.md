# Accessibility Guidelines & Standards

The application aims for **WCAG 2.2 AA** compliance to ensure inclusivity for all users.

## 1. Core Mandates
- **Focus Traps**: Mandatory for all modal components — alert dialogs, slideovers, and the v3 `<ilw-modal>`. Use the `openAlert()` or `openSlideover()` functions, which initialize these traps automatically. `<ilw-modal>` (triggered via `data-modal-target`) traps focus internally; when authoring any modal markup manually, implement the trap yourself.
- **Keyboard Navigation**: 
    - `Escape`: Must close any open overlay or menu.
    - `Tab`: Must navigate through interactive elements in a logical order.
    - `Arrow Keys`: Mandatory for `Treeview` and `Dropdown` menu navigation, and for moving between tabs in a `<ilw-tabs>` tablist (see Tabs pattern below).
- **Focus Indicators**: Never disable the default browser focus ring unless replacing it with a more visible, brand-compliant alternative (e.g., `outline-il-orange`).
- **Tab Index**: Do not use positive `tabindex` values — they alter the natural tab order.
- **Tabs (ARIA tablist) keyboard pattern**: Any tablist must support the standard interaction below. **`<ilw-tabs>` implements this itself** — author its markup per `TOOLKIT_COMPONENTS.md` §2 and do not add your own key handlers. Implement the pattern by hand only for a tablist built **without** `<ilw-tabs>`:
    - `Left`/`Right` (horizontal) or `Up`/`Down` (vertical) Arrow keys move focus between tab triggers.
    - `Home`/`End` move to the first/last tab.
    - Use a roving `tabindex` (active tab `tabindex="0"`, inactive tabs `tabindex="-1"`); the active tab carries `aria-selected="true"`.
    - `Tab` moves focus out of the tablist and into the active panel.
- **Auto-moving content (carousels, etc.)**: Any content that auto-advances MUST provide a visible pause/stop control, MUST pause on hover/focus, and MUST respect the user's `prefers-reduced-motion` setting by disabling auto-play when reduced motion is requested. This satisfies WCAG 2.2 SC 2.2.2. **The standing rule is to not auto-advance at all**: ship `<ilw-carousel>` paused (omit `playing`, or set `playing="false"`) so SC 2.2.2 is met unconditionally. See `TOOLKIT_COMPONENTS.md` §2 for the carousel rule and the narrow exception.

## 2. Semantic Structure & Landmarks
- **Skip link**: Provide a "Skip to main content" link as the first focusable element on the page, targeting an `id` on the `<main>` element. It may be visually hidden until focused, but it MUST become visible on `:focus` — `.ilw-sr-only` alone is not sufficient for this, since it hides content unconditionally (whether the toolkit reveals it on focus is unverified). Check whether `<ilw-page>`/`<ilw-header>` already renders a skip link before adding a duplicate.

Use HTML5 landmark elements to provide a map for screen reader users:
- `<header>`: Site-wide branding and utility links.
- `<nav>`: Major navigation blocks (use `aria-label` to distinguish between multiple navs).
- `<main>`: The unique content of the current page.
- `<footer>`: Site-wide copyright, contact, and legal links.
- `<section>`: Use only when a thematic grouping of content has a heading.

## 3. Heading Hierarchy
Headings must be nested sequentially without skipping levels:
- `h1`: The primary page title (only one per page).
- `h2`: Major sections.
- `h3` through `h6`: Sub-sections within the hierarchy.
- **Note**: Never use heading tags for visual styling only; use CSS classes instead.

## 4. ARIA Attributes
- Build the UI using semantic HTML5 elements and native HTML functionality whenever possible. Avoid ARIA unless it is genuinely necessary.
- Use the WAI-ARIA 1.2 specification as the authoritative reference when ARIA is required. Use only valid WAI-ARIA 1.2 roles, states, and properties.

Always use the following attributes as defined in the component documentation:
- `role`: Define the element type (`dialog`, `tablist`, `menuitem`, etc.).
- `aria-expanded`: Toggle `true`/`false` for collapsible elements.
- `aria-controls`: Link a trigger to the ID of the element it affects.
- `aria-labelledby`: Point to a heading or label that describes the element.
- `aria-current="page"`: Identify the active link in a navigation list.

## 5. Media & Color
- **Alt Text**: 
    - Meaningful images: Provide concise, descriptive text.
    - Decorative images: Use an empty `alt=""` attribute.
- **Color Contrast**: 
  - Normal/smaller text (less than 18pt / 24px, or less than 14pt / 18.67px bold) must have a minimum contrast ratio of 4.5:1.
  - Large text (at least 18pt / 24px, or 14pt / 18.67px bold) must have a minimum contrast ratio of 3:1.
    - **Note**: The shipped Toolkit components' *default* color combinations are designed for compliance, but the brand palette is not universally safe to compose freely: Illini Orange (#FF5F05) on white is ≈2.9:1 and **fails** the 4.5:1 requirement for normal text. Never use orange for body text on light backgrounds; verify contrast for any color combination you assemble yourself rather than assuming the palette is pre-vetted.
- **Color as Meaning**: Never use color as the only method of conveying information (e.g., use an icon or text label alongside a red error border).

## 6. Forms
- Make all form controls accessible — every `<input>`, `<select>`, `<textarea>`, and custom control must have a programmatically determinable accessible name.
- **Labels**: Prefer visible `<label>` elements and associate them with controls using native HTML mechanisms (e.g., `<label for="id">`).
- Ensure required fields, errors, instructions, and validation messages are programmatically associated with their relevant controls.
- **Validation**: Error messages must be linked to inputs using `aria-describedby`.
  - For dynamically validated forms, avoid using `aria-live` for every small validation change — this can become disruptive for screen-reader users.
- **Grouping**: Use `<fieldset>` and `<legend>` for groups of related controls (radio buttons, checkboxes).

## 7. Tables

Use native semantic table markup -  Use `<table>` for **actual tabular data**. Do not use tables for page layout.
- Use native HTML elements such as `<caption>`, `<thead>`, `<tbody>`, `<tfoot>`, `<tr>`, `<th>`, and `<td>`.
- Do not recreate tables using `<div>`, `<span>`, CSS grids, or ARIA roles when a native HTML table can be used.
- Do not add `role="table"`, `role="row"`, `role="cell"`, or similar ARIA roles to a native table unless there is a specific, documented reason.
- Avoid ARIA unless native HTML cannot provide the required semantics or behavior.

**Provide an accessible table name**
- Provide a meaningful `<caption>` that identifies the purpose or subject of the table.
- The caption should be concise and understandable without requiring the user to inspect the surrounding page.
- Do not use an empty or generic caption such as "Table" or "Data."

**Identify column and row headers**
- Use `<th>` for all header cells instead of using `<td>` with bold text or CSS to visually represent a header.
- Use the `scope` attribute when appropriate:

Do not rely on visual styling alone to identify headers.

If a column can be sorted:
- Make the sorting control keyboard accessible.
- Use a native `<button>` for the sorting action when appropriate.
- Clearly communicate the current sorting state to assistive technologies.
- Do not make an entire `<th>` behave like a clickable control using a `<div>` or `<span>`.
- Ensure the accessible name identifies what is being sorted and, where appropriate, the current sort direction.

For table filters:
- Provide visible labels for filter controls.
- Ensure filter controls have accessible names.
- Clearly communicate the relationship between filters and the table.
- Ensure users can determine when filters have been applied.
- Provide an accessible indication of the number of matching results when appropriate.

Ensure tables remain usable at different viewport sizes and zoom levels.
- Do not allow table content to become inaccessible at 200% zoom.
- Do not hide essential table information solely because the viewport is narrow.
- If horizontal scrolling is required, ensure the table remains fully operable using keyboard and assistive technologies.
- Do not use CSS truncation that prevents users from accessing essential information.
- Consider whether a complex table should transform into a more accessible presentation on small screens rather than simply shrinking the table.

Ensure pagination, selection, and dynamic updates, are fully keyboard and screen-reader accessible.

## 8. Error Messages
If a user submits a form containing errors, consider moving focus to:
- The first invalid field, **or**
- An error summary at the beginning of the form.

Ensure error messages are clear, specific, and available to screen readers. Identify the field with the error and provide instructions for correcting it. Do not rely on color alone to communicate errors.

Ensure error messages remain visible until the user has corrected the error or the validation state changes.

## 9. Status Messages
- Screen readers should be able to detect important status changes without requiring the user to move focus to the message.
- A status update should generally not steal keyboard focus from the user's current location
- Make messages meaningful- Don't announce generic messages such as: "Success"
- Don't rely on color alone - Dont just show green color for success, instead show text for e.g- success : Record was saved 

## 10. Screen Reader Only Content
Use the `.ilw-sr-only` utility class to provide information that is only accessible to screen readers. This is essential for providing context to links, labeling icon-only buttons, or offering instructions that are visually apparent but need explicit description for non-visual users.

**Example:**
```html
<button type="button" class="ilw-button">
    <ilw-icon icon="search"></ilw-icon>
    <span class="ilw-sr-only">Search the site</span>
</button>
```

## 11. Responsive Design

- Ensure content remains usable at 200% zoom and meets WCAG 2.2 requirements for reflow and text spacing.
- Do not allow fixed-position elements, sticky controls, banners, or other UI components to obscure meaningful content or keyboard focus.