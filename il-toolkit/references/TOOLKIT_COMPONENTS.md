# Illinois Toolkit Components

Use this reference with `SKILL.md` when creating, editing, or reviewing Illinois Toolkit v3 pages. Consult the [official component index](https://builder3.toolkit.illinois.edu/component/index.html) for the full catalog.

## Contents

1. [Content and forms](#1-content-and-forms)
2. [Actions and buttons](#2-actions-and-buttons)
3. [Featured content](#3-featured-content)
4. [Organization and layout](#4-organization-and-layout)
5. [Navigation and context](#5-navigation-and-context)
6. [Icons and icon panels](#6-icons-and-icon-panels)
7. [Profiles](#7-profiles)
8. [Review checklist](#8-review-checklist)

## Implementation rules

- Target Toolkit v3 and use production documentation unless the user requests beta features. Match examples to the component version already loaded by the project.
- Read the linked component documentation before introducing attributes, slots, classes, or behavior not covered here. Do not infer a component API from its name or from another component.
- Use this file for component selection and recurring patterns; use the documentation for complete APIs. If documentation is unavailable, use only known patterns and explain any unresolved implementation details.
- Load the required component assets and global styles through the project's established Toolkit setup. Avoid adding duplicate imports.
- Follow `ACCESSIBILITY.md` for accessibility requirements and `LAYOUT.md` for page structure and navigation placement. Those companion files are referenced here, not reproduced.
- Let Toolkit components manage their built-in interaction. For [`<ilw-tabs>`](https://builder3.toolkit.illinois.edu/component/ilw-tabs/index.html), do not add competing keyboard handlers or a roving-tabindex script. For [`<ilw-carousel>`](https://builder3.toolkit.illinois.edu/component/ilw-carousel/index.html), explicitly start paused.
- Choose heading levels according to the page hierarchy. The `h3` elements in examples are author-supplied examples, not fixed heading levels imposed by the components. Use `h2`, `h3`, or another appropriate level without skipping levels.
- For components summarized without a complete example, consult their documentation before generating markup.

## 1. Content and forms

- **[`<ilw-content>`](https://builder3.toolkit.illinois.edu/component/ilw-content/index.html)**: Wrap standard HTML content and apply consistent Illinois styling.
- **[`ilw-input`](https://builder3.toolkit.illinois.edu/component/ilw-input/index.html) styles**: Apply CSS classes to native form elements inside [`<ilw-content>`](https://builder3.toolkit.illinois.edu/component/ilw-content/index.html) and `<form>`. This is a styling package, not an [`<ilw-input>`](https://builder3.toolkit.illinois.edu/component/ilw-input/index.html) wrapper.

### Instructions

- Wrap related HTML content in a single [`<ilw-content>`](https://builder3.toolkit.illinois.edu/component/ilw-content/index.html) block.
- Include related headings, paragraphs, links, lists, tables, and native form markup within the block.
- Preserve semantic HTML, logical heading levels, and accessible form labels.
- Prefer Toolkit styling over custom CSS that duplicates its appearance.
- Keep other Toolkit components outside the wrapper unless their documentation explicitly supports nesting them within [`<ilw-content>`](https://builder3.toolkit.illinois.edu/component/ilw-content/index.html).
- Consult the component documentation before adding optional attributes or specialized modes: https://builder3.toolkit.illinois.edu/component/ilw-content/index.html
- If documentation is unavailable, follow these core instructions without inventing attributes or supported options.
- Place all form elements inside both [`<ilw-content>`](https://builder3.toolkit.illinois.edu/component/ilw-content/index.html) and `<form>`.
- Use native HTML controls with the classes provided by [`ilw-input`](https://builder3.toolkit.illinois.edu/component/ilw-input/index.html); do not invent an [`<ilw-input>`](https://builder3.toolkit.illinois.edu/component/ilw-input/index.html) wrapper.
- Wrap each label-and-control pair in `<div class="ilw-input-entry">`.
- Associate each label's `for` attribute with its control's unique `id`.
- For required controls, add `required` to the control and `class="ilw-label-required"` to its label.
- Preserve control names, validation, and form submission behavior.
- Ensure the project's Toolkit setup includes the [`ilw-input`](https://builder3.toolkit.illinois.edu/component/ilw-input/index.html) styles.
- Consult the input documentation for additional supported patterns: https://builder3.toolkit.illinois.edu/component/ilw-input/index.html
- If documentation is unavailable, use the patterns below without inventing additional classes, attributes, or options.

### Content inside other components

- Place [`<ilw-content>`](https://builder3.toolkit.illinois.edu/component/ilw-content/index.html) inside other Toolkit components when their content areas contain standard HTML, such as headings, paragraphs, lists, or forms.
- When nested content needs headings displayed at a uniform size, use [`<ilw-content mode="inset">`](https://builder3.toolkit.illinois.edu/component/ilw-content-inset/index.html).
- Preserve logical heading levels (`h2`, `h3`, etc.). Use inset mode to normalize their visual size without changing their semantic level.
- For forms, continue using the documented [`ilw-input`](https://builder3.toolkit.illinois.edu/component/ilw-input/index.html) classes on native form markup inside both [`<ilw-content>`](https://builder3.toolkit.illinois.edu/component/ilw-content/index.html) and `<form>`.
- For [`<ilw-tabs>`](https://builder3.toolkit.illinois.edu/component/ilw-tabs/index.html), place [`<ilw-content>`](https://builder3.toolkit.illinois.edu/component/ilw-content/index.html) inside each panel, outside the tab-button list. Keep buttons and panels in the structure required by the tabs component.
- Follow the containing component's documented structure and slots. Keep slotted headings, icons, and controls at the required level rather than moving them into a content wrapper.
- See the [inset-mode documentation](https://builder3.toolkit.illinois.edu/component/ilw-content-inset/index.html).

### Example

```html
<ilw-content>
  <h2>Request information</h2>
  <p>Enter your email address to learn more.</p>

  <form action="/request-information" method="post">
    <div class="ilw-input-entry">
      <label for="email" class="ilw-label-required">Email address</label>
      <input type="email" id="email" name="email" autocomplete="email" required>
    </div>
    <button type="submit" class="ilw-button">
      Request information
    </button>
  </form>
</ilw-content>
```

### Review

Before completing a change, confirm that related HTML content, including form elements, is inside [`<ilw-content>`](https://builder3.toolkit.illinois.edu/component/ilw-content/index.html), that form controls have accessible labels, and that custom styles do not unnecessarily duplicate Toolkit styling.

## 2. Actions and buttons

- **[`<ilw-call-to-action>`](https://builder3.toolkit.illinois.edu/component/ilw-call-to-action/index.html)**: Grabs user attention for a primary task (Apply, Join, etc.).
- **[`.ilw-button`](https://builder3.toolkit.illinois.edu/component/ilw-global-button-anchor/index.html)**: The v3 button class. [Native button details](https://builder3.toolkit.illinois.edu/component/ilw-global-button-button/index.html). Applied directly to an `<a>` or `<button>`. Use for **any standalone button**, including icon-only controls.
- **[`.ilw-buttons`](https://builder3.toolkit.illinois.edu/component/ilw-global-buttons/index.html)**: Container class for a **group of buttons and/or links**. Applied to a `<ul>`.

> **Note there is no [`<ilw-button>`](https://builder3.toolkit.illinois.edu/component/ilw-global-button-anchor/index.html) custom element** — [`ilw-button`](https://builder3.toolkit.illinois.edu/component/ilw-global-button-anchor/index.html) is a class, not a tag.

### Button Styling & Structure

**Scope — when to use which:**

- **Single button, anywhere** (including an icon-only search toggle in a header): put [`.ilw-button`](https://builder3.toolkit.illinois.edu/component/ilw-global-button-anchor/index.html) directly on the `<a>` or `<button>`. No list wrapper. Pair icon-only controls with an [`.ilw-sr-only`](https://builder3.toolkit.illinois.edu/component/ilw-global-sr-only/index.html) label — see `ACCESSIBILITY.md` §7.
- **Group of buttons or links** (e.g. "Apply" / "Learn More" pairs, carousel CTAs): use the [`.ilw-buttons`](https://builder3.toolkit.illinois.edu/component/ilw-global-buttons/index.html) list structure below, which supplies the flex row layout and spacing.

**Standalone Structure:**

```html
<a class="ilw-button ilw-theme-blue-solid" href="/apply">Apply Now</a>

<button type="button" class="ilw-button ilw-theme-blue-outline">Save Draft</button>
```

**Group Structure (buttons/links):**

```html
<ul class="ilw-buttons">
    <li><a class="ilw-theme-blue-solid" href="/apply">Apply</a></li>
    <li><button class="ilw-theme-blue-solid" type="button">Open details</button></li>
</ul>
```

Inside `ul.ilw-buttons`, the child `<a>` and `<button>` elements are styled by the container and **do not need their own [`.ilw-button`](https://builder3.toolkit.illinois.edu/component/ilw-global-button-anchor/index.html) class**.

**Themes:**

| Theme | Effect |
| :-- | :-- |
| `ilw-theme-blue` / `ilw-theme-orange` | Brand primary colors |
| `ilw-theme-blue-solid` / `ilw-theme-orange-solid` | Solid background variants |
| `ilw-theme-blue-outline` / `ilw-theme-orange-outline` | Outlined variants |
| `ilw-theme-blue-inverse` / `ilw-theme-orange-inverse` | Inverted colors for dark backgrounds |

Apply theme and size classes directly to individual links or buttons when customizing them. Use one theme and one size per control; consult the linked global-style documentation before relying on inheritance from a group container.

**Sizes:** Use `ilw-button-large` or `ilw-button-small` on the individual control.

**Alignment:** Use `ul.ilw-buttons` for a centered group or `ul.ilw-buttons-left` for a left-aligned group.

Use an `<a href="…">` for navigation and a native `<button>` for an action. Set `type="button"` for non-submit buttons inside forms.

## 3. Featured content

- **[`<ilw-statistic>`](https://builder3.toolkit.illinois.edu/component/ilw-statistic/index.html)**: Displays numbers/data in large, accessible formats.
- **[`<ilw-quote>`](https://builder3.toolkit.illinois.edu/component/ilw-quote/index.html)**: Styled blockquotes for testimonials or impactful statements.
- **[`<ilw-video>`](https://builder3.toolkit.illinois.edu/component/ilw-video/index.html)**: Auto-sizing container for accessible video embeds (YouTube, Vimeo, etc.).
- **[`<ilw-content mode="introduction">`](https://builder3.toolkit.illinois.edu/component/ilw-content-introduction/index.html)**: Style an introductory block at the beginning of a story or content section. [Introduction details](https://builder3.toolkit.illinois.edu/component/ilw-content-introduction/index.html)
- **[`<ilw-content mode="lede">`](https://builder3.toolkit.illinois.edu/component/ilw-content-lede/index.html)**: Emphasize a short opening or summary paragraph. [Lede details](https://builder3.toolkit.illinois.edu/component/ilw-content-lede/index.html)
- **[`<ilw-carousel>`](https://builder3.toolkit.illinois.edu/component/ilw-carousel/index.html)**: A component for displaying a sequence of slides with images, text, and call-to-action buttons.
- **[`<ilw-card>`](https://builder3.toolkit.illinois.edu/component/ilw-card/index.html)**: Container for image/icon + title + text (usually used in a [`<ilw-grid>`](https://builder3.toolkit.illinois.edu/component/ilw-grid/index.html)).
- **[`<ilw-icon-panel>`](https://builder3.toolkit.illinois.edu/component/ilw-icon-panel/index.html)**: A content block featuring an icon, a linked heading, and descriptive text.

### Video details

**Attributes:**

- `aspectratio`: `16/9` (Default), `9/16` (Recommended for YouTube Shorts/vertical video).
- `width`: CSS value (e.g., `467px`).
- `height`: CSS value (e.g., `831px`).

**Structure:**
Wrap the video `<iframe>` inside the component. **Note**: The iframe must have a descriptive `title` attribute for accessibility. Replace the placeholder IDs below with your actual video ID.

**Example (Standard):**

```html
<ilw-video>
    <iframe src="https://www.youtube.com/embed/VIDEO_ID" title="Introduction Video" allowfullscreen></iframe>
</ilw-video>
```

**Example (YouTube Short):**

```html
<ilw-video aspectratio="9/16" width="467px">
    <iframe src="https://www.youtube.com/embed/SHORT_VIDEO_ID" title="Campus Summer Short" allowfullscreen></iframe>
</ilw-video>
```

### Carousel details

**Structure:**

- [`<ilw-carousel>`](https://builder3.toolkit.illinois.edu/component/ilw-carousel/index.html): Root container.
- [`<ilw-slide>`](https://builder3.toolkit.illinois.edu/component/ilw-carousel/index.html): Individual slide container.
    - `<h3>`: Slide title.
    - `<p>`: Slide description.
    - `<div slot="buttons">`: Wrapper for the CTA button list. Place a `<ul class="ilw-buttons">` inside it.
    - `<img slot="image">`: Background image for the slide.

**Attributes:**

- `controls`: `compact` (smaller navigation arrows).
- `theme="blue"`: Gives the carousel controls a blue background.
- `playing`: `true`/`false` (enables/disables auto-play). Set `playing="false"` explicitly.
- `width`: `full` (expands to viewport width).
- `overlay`: `true` (places text content over the image).

> **Project motion rule:** Start carousels paused with `playing="false"`; do not rely on an omitted attribute to select the initial state. Enable autoplay only when explicitly requested. If enabled, verify pause/resume controls, focus and hover behavior, and reduced-motion behavior in the loaded version against `ACCESSIBILITY.md` §1. Starting paused does not remove the need to check keyboard navigation, labels, and other accessibility requirements.

**Example:**

```html
<h2>Research highlights</h2>
<ilw-carousel controls="compact" theme="blue" playing="false" label="Research highlights">
    <ilw-slide>
        <h3>Innovation at Illinois</h3>
        <p>Exploring the frontiers of technology and research.</p>
        <div slot="buttons">
            <ul class="ilw-buttons">
                <li><a href="/research">Learn More</a></li>
            </ul>
        </div>
        <img src="slide1.jpg" alt="Campus research lab" slot="image">
    </ilw-slide>
</ilw-carousel>
```

## 4. Organization and layout

- **[`<ilw-accordion>`](https://builder3.toolkit.illinois.edu/component/ilw-accordion/index.html)**: Wrapper for multiple [`<ilw-accordion-panel>`](https://builder3.toolkit.illinois.edu/component/ilw-accordion/index.html) elements.
- **[`<ilw-accordion-panel>`](https://builder3.toolkit.illinois.edu/component/ilw-accordion/index.html)**: A collapsible section inside [`<ilw-accordion>`](https://builder3.toolkit.illinois.edu/component/ilw-accordion/index.html). Put a heading in `slot="summary"`; choose its level for the surrounding hierarchy and use the same level across peer panels.
- **[`<ilw-columns>`](https://builder3.toolkit.illinois.edu/component/ilw-columns/index.html)**: Arrange content in responsive columns; consult its documented layout options.
- **[`<ilw-grid>`](https://builder3.toolkit.illinois.edu/component/ilw-grid/index.html)**: Manages alignment for responsive sets of cards or stats.
- **[`<ilw-tabs>`](https://builder3.toolkit.illinois.edu/component/ilw-tabs/index.html)**: A component for organizing related content into switchable panels.
- **[`<ilw-pagination>`](https://builder3.toolkit.illinois.edu/component/ilw-pagination/index.html)**: Accessible page navigation for lists.

### Tabs details

Use the linked documentation for the current version. The example below includes explicit initial ARIA states; the component manages subsequent selection and keyboard behavior.

**Usage Guidelines:**

- **Do use**: To display groups of related information in a compact area.
- **Do not use**: When a user needs to read the entire page content sequentially, or for a multi-step process. In those cases, use separate pages or standard headings.

**Layouts:**

- **Vertical (Default)**: Recommended for most use cases, especially with more than 3-4 tabs.
- **Horizontal**: Triggered by `horizontal="true"`. Use only for 2-3 tabs to avoid truncation.
- **Mobile Behavior**: On mobile devices, all layouts collapse into an **accordion-like view**.

**Structure & Accessibility:**

- [`<ilw-tabs>`](https://builder3.toolkit.illinois.edu/component/ilw-tabs/index.html): Root container. Label it with `aria-labelledby` pointing to a section header.
- `<div slot="tabs" role="tablist">`: Contains the trigger buttons.
    - `<button role="tab" id="tab1" aria-controls="panel1">`: Individual triggers. The active tab carries `aria-selected="true"`; inactive tabs carry `aria-selected="false"`.
- Content Panels: `div` elements with `role="tabpanel"` and `aria-labelledby` pointing to the controlling button's ID.
    - **Tip**: Use [`<ilw-content mode="inset">`](https://builder3.toolkit.illinois.edu/component/ilw-content-inset/index.html) inside panels for proper formatting.

> **Accessibility — keyboard navigation (rule):** [`<ilw-tabs>`](https://builder3.toolkit.illinois.edu/component/ilw-tabs/index.html) owns the tablist keyboard behavior at runtime. **Author the static markup exactly as shown in the example below — `role`, `aria-controls`, `aria-labelledby`, and the initial `aria-selected` / `tabindex` state — and do not attach your own `keydown` handlers or roving-`tabindex` script inside the component.** Adding custom key handling double-manages state the component already drives and will break tab switching.
>
> The full ARIA tablist pattern in `ACCESSIBILITY.md` §1 ("Tabs (ARIA tablist) keyboard pattern") applies when you build a tablist **without** [`<ilw-tabs>`](https://builder3.toolkit.illinois.edu/component/ilw-tabs/index.html) — hand-rolled tabs must implement it in full.
>
> If keyboard navigation fails in a browser test, first check asset loading, markup, and the component version. If the failure persists with documented markup, report it to the Web Toolkit owners; avoid competing local keyboard handlers.

**Attributes:**

- `theme`: Color theme (`blue`, `gray`, `white`).
- `width`:
    - (default): Constrained to the container.
    - `full`: Full width of the viewport.
    - `auto`: Full width background, but text is constrained to the container.
    - `page`: Text is constrained to the standard page width.
- `horizontal`: Set to `true` for a horizontal layout.
- `default`: ID of the tab to open on load (defaults to the first tab); check the loaded version before configuring this option.

**Example:**

```html
<h2 id="tabs-heading">Department Information</h2>
<ilw-tabs theme="blue" aria-labelledby="tabs-heading">
    <div slot="tabs" role="tablist" aria-labelledby="tabs-heading">
        <button type="button" role="tab" id="btn1" aria-controls="panel1" aria-selected="true" tabindex="0">Overview</button>
        <button type="button" role="tab" id="btn2" aria-controls="panel2" aria-selected="false" tabindex="-1">Details</button>
    </div>
    <div id="panel1" role="tabpanel" aria-labelledby="btn1">
        <ilw-content mode="inset">
            <h3>Overview</h3>
            <p>Main content goes here.</p>
        </ilw-content>
    </div>
    <div id="panel2" role="tabpanel" aria-labelledby="btn2" hidden>
        <ilw-content mode="inset">
            <h3>Details</h3>
            <p>Detailed information.</p>
        </ilw-content>
    </div>
</ilw-tabs>
```

## 5. Navigation and context

- **[`<ilw-breadcrumbs>`](https://builder3.toolkit.illinois.edu/component/ilw-breadcrumbs/index.html)**: Helps users understand site hierarchy. Placement guidance is in `LAYOUT.md` §4 (In-Page Navigation); prefer `LAYOUT.md` for where and how to position breadcrumbs within a page. Use this file only for the component's identity/role.
- **[`<ilw-section-nav>`](https://builder3.toolkit.illinois.edu/component/ilw-section-nav/index.html)**: Contextual navigation for the current site branch. Full markup, manual mode, and placement are documented in `LAYOUT.md` §4; prefer `LAYOUT.md`.

## 6. Icons and icon panels

### Icons

- **[`<ilw-icon>`](https://builder3.toolkit.illinois.edu/component/ilw-icon/index.html)**: Wrapper for University of Illinois brand icons.

**Attributes:**

- `icon`: A supported name from the linked brand icon catalog.
- `alt`: Alternative text. If omitted, the icon is hidden from screen readers (`aria-hidden="true"`). When an icon is the only content of an interactive control, omit `alt` and supply an [`.ilw-sr-only`](https://builder3.toolkit.illinois.edu/component/ilw-global-sr-only/index.html) label on the control instead (see `ACCESSIBILITY.md` §7).
- `size`: CSS value and unit (e.g., `24px`, `2rem`).

**Example:**

```html
<ilw-icon icon="calendar" size="1.5rem" alt="Event Date"></ilw-icon>
```


### Icon panels

Use [`<ilw-icon-panel>`](https://builder3.toolkit.illinois.edu/component/ilw-icon-panel/index.html) to combine an icon with a linked heading or short message.

**Slots:**

- `icon`: The icon to display (usually an [`<ilw-icon>`](https://builder3.toolkit.illinois.edu/component/ilw-icon/index.html) or `<svg>`).
- `link`: An `<a>` tag for the panel's heading.
- Default slot: Descriptive text/paragraph.

**Attributes:**

- `theme`: Use a background. Consult the documentation before selecting another theme.
- `accent`: Icon/heading accent color (`blue`, `orange`).

**Example:**

```html
<ilw-icon-panel theme="white" accent="orange">
    <ilw-icon slot="icon" icon="handbook"></ilw-icon>
    <a href="/handbook" slot="link">Student Handbook</a>
    <p>Policies and requirements for all graduate students.</p>
</ilw-icon-panel>
```

**Icon names:** Choose a supported name from the [Illinois brand icon catalog](https://cdn.brand.illinois.edu/icons.html), then confirm it is available in the loaded component version. Avoid maintaining a copied list that can become outdated. The icon component must not contain child elements or text.

## 7. Profiles

- **[`<ilw-profile>`](https://builder3.toolkit.illinois.edu/component/ilw-profile/index.html)**: A comprehensive layout for faculty, staff, or student profiles.

**Slots:**

- `name`: Person's name (usually an `h1`).
- `title`: Job title and department (usually a `<p>`).
- `image`: Profile photo (`<img slot="image">`).
- `contact`: Container for contact information.
    - Use `<div class="ilw-profile--icon">` for rows (e.g., address, phone).
- `left`: Optional sidebar content.
- Default slot: Main body content (Biography, Appointments, etc.).

**Attributes:**

- `theme`: `gray`.
- `width`: `full`, `auto`, `page`.
- `circle`: `true` (clips profile image to a circle).
- `right`: `true` (swaps the layout to right-aligned image).

> **Version check:** These slot names are documented in the Builder. Match optional attributes to the loaded version and check the rendered layout in a browser.

**Example:**

```html
<ilw-profile theme="gray" circle="true">
    <img slot="image" src="jdoe.jpg" alt="Dr. Jane Doe">
    <h1 slot="name">Dr. Jane Doe</h1>
    <p slot="title">Professor, Department of Computer Science</p>
    <div slot="contact">
        <div class="ilw-profile--icon">
            <ilw-icon icon="location"></ilw-icon>
            <span>1234 Siebel Center, Urbana, IL 61801</span>
        </div>
        <div class="ilw-profile--icon">
            <ilw-icon icon="phone"></ilw-icon>
            <a href="tel:2175551234">(217) 555-1234</a>
        </div>
        <div class="ilw-profile--icon">
            <ilw-icon icon="email"></ilw-icon>
            <a href="mailto:jdoe@illinois.edu">jdoe@illinois.edu</a>
        </div>
    </div>
    <ilw-content slot="left" mode="inset">
        <h2>Office Hours</h2>
        <p>Tuesdays &amp; Thursdays, 2–4 PM.</p>
    </ilw-content>
    <ilw-content>
        <h2>Biography</h2>
        <p>Dr. Doe's research focuses on distributed systems and accessibility.</p>
    </ilw-content>
</ilw-profile>
```

- **[`<ilw-profile-card>`](https://builder3.toolkit.illinois.edu/component/ilw-profile-card/index.html)**: A compact card variant for displaying profile information in grids.

**Slots:**

- `image`: Profile photo (`<img slot="image">`).
- `name`: Person's name (usually an `h2`).
- `title`: Job title or description (usually a `<p>`).
- `address`: Postal address or office location.
- `phone`: Phone number link.
- `email`: Email address link.
- `hours`: Office hours.
- `linkedin`, `x`, `facebook`: Social media links.
- `news`: Link to related news articles.
- `hr`: Reporting information or manager link.

**Attributes:**

- `theme`: `gray`.
- `round`: Clips the profile image into a round shape.
- `aspectratio`: `1/1`, `2/1`, `16/10`, `10/16` (sets the image container ratio).

> **Version check:** These slot names are documented in the Builder. Match optional attributes to the loaded version and check the rendered layout in a browser.

**Example:**

```html
<ilw-profile-card theme="gray" round="true" aspectratio="1/1">
    <img slot="image" src="jdoe.jpg" alt="Dr. Jane Doe">
    <h2 slot="name">Dr. Jane Doe</h2>
    <p slot="title">Professor, Computer Science</p>
    <div slot="address">1234 Siebel Center, Urbana, IL 61801</div>
    <a slot="phone" href="tel:2175551234">(217) 555-1234</a>
    <a slot="email" href="mailto:jdoe@illinois.edu">jdoe@illinois.edu</a>
    <div slot="hours">Tue &amp; Thu, 2–4 PM</div>
    <a slot="linkedin" href="https://linkedin.com/in/example">LinkedIn</a>
</ilw-profile-card>
```

- **[`<ilw-profile-list>`](https://builder3.toolkit.illinois.edu/component/ilw-profile-list/index.html)**: A compact list-item variant for high-density profile directories.

**Slots:**

- `image`: Profile photo (`<img slot="image">`).
- `name`: Person's name link (`<a slot="name">`).
- `title`: Department or job title. Do not substitute an undocumented `subtitle` slot.
- Default slot: Contact links (email, phone).

**Attributes:**

- `circle`: Clips the profile image into a circle; use a roughly square source image.

**Example:**

```html
<ilw-profile-list circle>
    <img slot="image" src="avatar.jpg" alt="Jane Smith">
    <a href="/profile/jane" slot="name">Smith, Jane</a>
    <div slot="title">Department of Redundancy</div>
    <div>
        <a href="mailto:jsmith@illinois.edu">jsmith@illinois.edu</a> |
        <a href="tel:2175551234">(217) 555-1234</a>
    </div>
</ilw-profile-list>
```

## 8. Review checklist

- Confirm every chosen component and optional setting against its linked documentation and the loaded production version.
- Wrap standard content in [`<ilw-content>`](https://builder3.toolkit.illinois.edu/component/ilw-content/index.html); use `mode="inset"` inside other components when headings need a uniform visual size.
- Keep native form controls inside both [`<ilw-content>`](https://builder3.toolkit.illinois.edu/component/ilw-content/index.html) and `<form>`, use documented input classes, and associate labels with unique control IDs.
- Preserve the containing component's required slots and direct-child structure.
- Choose semantic heading levels and descriptive links; label icon-only controls and embedded video frames.
- Start carousels paused and let tabs manage their own keyboard interaction. Check runtime behavior in a browser.
- Follow `ACCESSIBILITY.md` and `LAYOUT.md` for their respective requirements.
