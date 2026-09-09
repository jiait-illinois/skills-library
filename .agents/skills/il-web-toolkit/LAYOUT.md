# Page Layout & Structure

Guidelines for assembling a page using the v3+ Toolkit components.

## 1. Page Construction Sequence
Follow these steps to build a compliant page:
1. **Framework**: Initialize `head` and `body`.
2. **Skeleton**: Wrap content in `<ilw-page class="ilw-margin ilw-font">`.
3. **Branding**: Add `<ilw-header slot="header">` and `<ilw-footer slot="footer">`.
<!-- TODO: Does this exist, is it necessary? -->
4. **Title/Hero**: Use `<ilw-hero>`, `<ilw-page-title>`, or `<ilw-content>` with an `h1`.
<!-- TODO: Does this exist, is it necessary? -->
5. **Navigation**: Add `<ilw-breadcrumbs>`, `<ilw-section-nav>`, and `<ilw-back-to-top>`.
<!-- TODO: Does this exist, is it necessary? -->
6. **CTA**: Insert `<ilw-call-to-action>`, or apply the `.ilw-button` class to an `<a>` or `<button>` for a standard button. (Note: `.ilw-button` is a class, not an element — there is no `<ilw-button>` tag.)
7. **Content**: Use `<ilw-content>` and `<ilw-columns>`.

## 2. Header & Footer Details
Standard branding components for the `header` and `footer` slots of `<ilw-page>`.

### ilw-header
Slots:
- `primary-unit`: Top-level organization name/link.
- `site-name`: Current site title/link.
- `links`: Utility navigation (usually a `nav` with a `ul`).
- `navigation`: Main site navigation (usually contains `<ilw-header-menu>`).
- `search`: Slot for the site search component.

### ilw-footer
Slots:
- `primary-unit`: Top-level organization name/link.
- `site-name`: Current site title/link.
- `social`: Container for social media links.
- `contact`: Address and contact information.
- Default slot: General text or supplemental links.

### Complete Page Example

This is the reference skeleton for a standard sub-page: both CDNs loaded, header and footer slotted into `<ilw-page>`, and a single `<main>` landmark holding the content. Copy this structure rather than reconstructing it from the slot lists above.

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Graduate Programs | Department of Example | University of Illinois</title>

    <!-- Toolkit v3 (required for ilw- web components) -->
    <link rel="stylesheet" href="https://cdn.toolkit.illinois.edu/3/toolkit.css">
    <script src="https://cdn.toolkit.illinois.edu/3/toolkit.js" type="module"></script>

    <!-- Toolkit v2 / 2.16 (base toolkit; loaded alongside v3 in production) -->
    <link rel="stylesheet" href="https://cdn.toolkit.illinois.edu/2.16/toolkit.css">
    <script src="https://cdn.toolkit.illinois.edu/2.16/toolkit.js"></script>
</head>
<body style="margin: 0;">
<ilw-page class="ilw-margin ilw-font">

    <ilw-header slot="header">
        <a slot="primary-unit" href="https://illinois.edu">University of Illinois</a>
        <a slot="site-name" href="/">Department of Example</a>

        <nav slot="links" aria-label="Utility">
            <ul>
                <li><a href="/directory">Directory</a></li>
                <li><a href="/give">Give</a></li>
            </ul>
        </nav>

        <div slot="navigation">
            <ilw-header-menu>
                <ul>
                    <li><a href="/about">About</a></li>
                    <li>
                        <ilw-header-menu-section>
                            <span slot="label">Academics</span>
                            <ul>
                                <li><a href="/academics/undergraduate">Undergraduate</a></li>
                                <li><a href="/academics/graduate">Graduate</a></li>
                            </ul>
                        </ilw-header-menu-section>
                    </li>
                </ul>
            </ilw-header-menu>
        </div>

        <div slot="search">
            <!-- Site search. For an icon-only search control, use the standalone-button
                 pattern in TOOLKIT_COMPONENTS.md §1 with an .ilw-sr-only label
                 (ACCESSIBILITY.md §7). -->
        </div>
    </ilw-header>

    <main>
        <ilw-breadcrumbs>
            <ul>
                <li><a href="/">Home</a></li>
                <li><a href="/academics">Academics</a></li>
                <li><a href="/academics/graduate" aria-current="page">Graduate</a></li>
            </ul>
        </ilw-breadcrumbs>

        <ilw-page-title>
            <h1>Graduate Programs</h1>
        </ilw-page-title>

        <ilw-columns>
            <ilw-section-nav mode="manual" collapse="true">
                <ul>
                    <li><a href="/academics" class="ilw-section-nav--root">Academics</a></li>
                    <li><a href="/academics/undergraduate">Undergraduate</a></li>
                    <li><a href="/academics/graduate" aria-current="page">Graduate</a></li>
                </ul>
            </ilw-section-nav>

            <div>
                <ilw-content mode="introduction">
                    <p>Our graduate programs span research and professional degrees.</p>
                </ilw-content>

                <ilw-content>
                    <h2>Degrees Offered</h2>
                    <p>Program details go here.</p>

                    <ul class="ilw-buttons ilw-theme-blue-solid">
                        <li><a href="/apply">Apply Now</a></li>
                        <li><a href="/visit">Schedule a Visit</a></li>
                    </ul>
                </ilw-content>
            </div>
        </ilw-columns>

        <ilw-spacer></ilw-spacer>
    </main>

    <ilw-back-to-top></ilw-back-to-top>

    <ilw-footer slot="footer">
        <a slot="primary-unit" href="https://illinois.edu">University of Illinois</a>
        <a slot="site-name" href="/">Department of Example</a>

        <div slot="contact">
            <p>1234 Example Hall, Urbana, IL 61801</p>
            <p><a href="mailto:example@illinois.edu">example@illinois.edu</a></p>
        </div>

        <nav slot="social" aria-label="Social media">
            <ul>
                <li><a href="https://x.com/illinois1867">X</a></li>
                <li><a href="https://www.instagram.com/illinois1867">Instagram</a></li>
            </ul>
        </nav>

        <p>&copy; 2026 The Board of Trustees of the University of Illinois</p>
    </ilw-footer>

</ilw-page>
</body>
</html>
```

**Notes on this example:**
- Exactly one `<h1>` (in `<ilw-page-title>`); section headings start at `<h2>`. See `ACCESSIBILITY.md` §3.
- Each `<nav>` carries a distinguishing `aria-label`, since the page has several. See `ACCESSIBILITY.md` §2.
- `aria-current="page"` marks the active link in both the breadcrumbs and the section nav.
- `<ilw-back-to-top>` sits outside `<main>` because it is page furniture, not page content.
- The `<ilw-breadcrumbs>` child markup is inferred from its "nested list of parent pages" description and has not been verified against the live component — confirm the expected inner structure before relying on it.
- The v3 stylesheet is loaded before the v2 stylesheet, so where the two define equal-specificity rules, v2 wins the cascade tie. Keep the order consistent site-wide; see the load-order note in `CORE_STANDARDS.md` §1.

## 3. Header & Banner Options
<!-- TODO: Does this exist, is it necessary? -->
- **`<ilw-hero>`**: Large text + background image + CTA buttons. Use for high-impact landing pages.
- **`<ilw-page-title>`**: Centered title with patterned background. Use for standard sub-pages.
- **`<ilw-content mode="introduction">`**: For ledes and introductory text.
- **`<ilw-header-menu>`**: The primary navigation widget (see Navigation Components).

> **Coverage note:** `<ilw-hero>`, `<ilw-columns>`, `<ilw-spacer>`, and `<ilw-content>` (beyond the `introduction` and `inset` modes shown in passing) have **no full slot/attribute spec in this skill**. Use them only as the Complete Page Example (§2) shows; for anything beyond that — hero slots, column configuration, other content modes — consult the official Toolkit documentation rather than inventing markup.

## 4. In-Page Navigation
<!-- TODO: Does this exist, is it necessary? -->
- **`<ilw-breadcrumbs>`**: Nested list of parent pages.
- **`<ilw-section-nav>`**: List of sibling/child pages (usually placed in a left column).

**Manual Mode (`mode="manual"`)**:
Used when the navigation structure is provided manually in the HTML.

**Structure:**
- Root `<ul>`: Contains the navigation items.
- Root Link: Add class `ilw-section-nav--root` to the top-level section link.
- Current Page: Add `aria-current="page"` to the active link.
- Hierarchy: Use nested `<ul>` elements for sub-pages.

**Attributes:**
- `collapse`: Set to `true` to enable collapsible sections.
- `open`: Set to `true` to have the navigation open by default on mobile.

**Example:**
```html
<ilw-section-nav mode="manual" collapse="true">
    <ul>
        <li><a href="/section" class="ilw-section-nav--root">Section Home</a></li>
        <li><a href="/page-1">Parent Page</a>
            <ul>
                <li><a href="/sub-1" aria-current="page">Active Sub-page</a></li>
                <li><a href="/sub-2">Other Sub-page</a></li>
            </ul>
        </li>
    </ul>
</ilw-section-nav>
```
- **`<ilw-back-to-top>`**: Floating button for long pages.

<!-- TODO: Does this exist, is it necessary? -->
## 5. Layout Helpers
- **`<ilw-columns>`**: Flexible grid container.
- **`<ilw-grid>`**: Standardized grid for cards/statistics.
- **`<ilw-spacer>`**: Standardized vertical padding between components.
- **`<ilw-accordion>`**: Collapsible content sections for dense information.

## 6. Global Utility Classes
<!-- TODO: Does this exist, is it necessary? -->
### Image Cover
Use these classes on a container `div` to make its child `img` cover the entire area (similar to `object-fit: cover`).

- `.ilw-image-cover`: Standard cover (centered).
- `.ilw-image-cover-top`: Align to the top.
- `.ilw-image-cover-bottom`: Align to the bottom.
- `.ilw-image-cover-left`: Align to the left.
- `.ilw-image-cover-right`: Align to the right.

### Screen Reader Only
- `.ilw-sr-only`: Hides content visually but leaves it available for screen readers. See `ACCESSIBILITY.md` for usage guidelines.

**Example:**
```html
<div class="ilw-sr-only">Hidden instruction for screen readers.</div>
```