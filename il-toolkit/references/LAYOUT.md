# Page Layout & Structure

Guidelines for assembling a page using the v3+ Toolkit components.

## 1. Page Construction Sequence
Follow these steps to build a compliant page:
1. **Framework**: Initialize `head` and `body`.
2. **Skeleton**: Wrap content in `<ilw-page class="ilw-font">`.
3. **Branding**: Add `<ilw-header slot="header">` and `<ilw-footer slot="footer">`.
    a. [Header details](https://builder3.toolkit.illinois.edu/component/ilw-header/index.html)
    b. [Header menu details](https://builder3.toolkit.illinois.edu/component/ilw-header-menu/index.html)
    b. [Footer details](https://builder3.toolkit.illinois.edu/component/ilw-footer/index.html)
4. **Title/Hero**: Use `<ilw-hero>`, `<ilw-page-title>`, or `<ilw-content>` with an `h1`.
    a. A Hero is needed if you want to get the user's attention with a few quick links and a large image. [Hero details](https://builder3.toolkit.illinois.edu/component/ilw-hero/index.html)
    b. A Page title is needed if you want to get the user's attention with a small image or you have a complex application that is subdivided into several sections and you want to easily distinguish those sections. [Page title details](https://builder3.toolkit.illinois.edu/component/ilw-page-title/index.html)
    c. A Content block with an `h1` is good for a simple page. [Content details](https://builder3.toolkit.illinois.edu/component/ilw-content/index.html)
5. **Navigation**: Add `<ilw-breadcrumbs>`, `<ilw-section-nav>`, and `<ilw-back-to-top>`.
    a. Breadcrumbs are needed if the website has a navigation structure more than two levels deep. [Breadcrumb details](https://builder3.toolkit.illinois.edu/component/ilw-breadcrumbs/index.html)
    b. Section Navigation is needed if the website has a navigation structure more than two levels deep and you want to reference child pages within the page. Wrap the `<ilw-section-nav>` element inside an `<ilw-columns mode="sidebar-left" width="page">` element -- this will put the Section Navigation to the left of the rest of the content. [Section Navigation details](https://builder3.toolkit.illinois.edu/component/ilw-section-nav/index.html)
    c. Back to Top should always be close as possible to the ending `</body>` tag outside the `<ilw-page>`. [Back to top details](https://builder3.toolkit.illinois.edu/component/ilw-back-to-top/index.html)
6. **Content**: Add the rest of the content. Use the TOOLKIT_COMPONENTS.md for details. 

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
        <p slot="primary-unit">
            <a href="">Unit of Example</a>
        </p>
        <p slot="site-name">
            <a href="/">Department of Example</a>
        </p>
        
        <address slot="address">
            <p>
                5678 West Example Street
                <br>
                MC-0000
                <br>
                Champaign, IL 61820
            </p>
            <p>
                Email:
                <a href="mailto:address@illinois.edu">address@illinois.edu</a>
            </p>
            <p>
                Phone:
                <a href="tel:+12175551234" aria-label="Example Site Office">(217) 555-1234</a>
            </p>
        </address>

        <nav slot="social" aria-label="Social media">
            <ul>
                <li>
                    <a href="">
                        <ilw-icon alt="Our Bluesky account" icon="bluesky" size="44px"></ilw-icon>
                    </a>
                </li>
                <li>
                    <a href="">
                        <ilw-icon alt="Our GitHub account" icon="github" size="44px"></ilw-icon>
                    </a>
                </li>
            </ul>
        </nav>
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