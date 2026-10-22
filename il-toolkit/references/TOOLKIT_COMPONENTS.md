# Toolkit Web Components

Reference for semantic UI components provided by the Web Toolkit.

> **Accessibility note:** For modal/overlay components, keyboard and focus requirements (focus traps, `Escape` to close) are defined authoritatively in `ACCESSIBILITY.md` §1. Where this file shows ARIA attributes inline, they illustrate structure only — the authoritative behavior lives in `ACCESSIBILITY.md`.
>
> Two exceptions, because these are web components that own their own behavior: for **`<ilw-tabs>`** author the static markup here and add no key handlers (§2), and for **`<ilw-carousel>`** do not enable auto-play (§2). Those two rules are stated in this file and are not overridden by the general patterns in `ACCESSIBILITY.md` §1, which describe what you must build when *not* using these components.

> **Heading hierarchy note:** Several components below hardcode an `<h3>` (e.g. Carousel slides, Tab panels). Per `ACCESSIBILITY.md` §3, headings must nest sequentially without skipping levels. Authors are responsible for ensuring these components do not create a skip relative to the surrounding page. Do not place an `<h3>`-bearing component directly under an `<h1>` without an intervening `<h2>`.

> **Coverage note:** Some components in this file have a one-line description only and **no verified slot/attribute spec or snippet**: `<ilw-call-to-action>`, `<ilw-statistic>`, `<ilw-quote>`, `<ilw-card>`, `<ilw-accordion>`/`<ilw-accordion-panel>`, `<ilw-grid>`, and `<ilw-pagination>`. Do not invent markup for these — consult the component's page in the official Toolkit documentation before generating anything beyond the element name.

## 1. Interaction & CTA
- **`<ilw-call-to-action>`**: Grabs user attention for a primary task (Apply, Join, etc.).
- **`.ilw-button`**: The v3 button class. Applied directly to an `<a>` or `<button>`. Use for **any standalone button**, including icon-only controls.
- **`.ilw-buttons`**: Container class for a **group of buttons and/or links**. Applied to a `<ul>`.

> **Note there is no `<ilw-button>` custom element** — `ilw-button` is a class, not a tag.

### Button Styling & Structure

**Scope — when to use which:**
- **Single button, anywhere** (including an icon-only search toggle in a header): put `.ilw-button` directly on the `<a>` or `<button>`. No list wrapper. Pair icon-only controls with an `.ilw-sr-only` label — see `ACCESSIBILITY.md` §7.
- **Group of buttons or links** (e.g. "Apply" / "Learn More" pairs, carousel CTAs): use the `.ilw-buttons` list structure below, which supplies the flex row layout and spacing.

**Standalone Structure:**
```html
<a class="ilw-button ilw-theme-blue-solid" href="/apply">Apply Now</a>

<button type="button" class="ilw-button ilw-theme-blue-outline">Save Draft</button>
```

**Group Structure (buttons/links):**
```html
<ul class="ilw-buttons ilw-theme-blue-solid">
    <li><a href="#">Link 1</a></li>
    <li><button type="button">Button 1</button></li>
</ul>
```

Inside `ul.ilw-buttons`, the child `<a>` and `<button>` elements are styled by the container and **do not need their own `.ilw-button` class**.

**Themes:**

| Theme | Effect |
| :-- | :-- |
| `ilw-theme-blue` / `ilw-theme-orange` | Brand primary colors |
| `ilw-theme-blue-solid` / `ilw-theme-orange-solid` | Solid background variants |
| `ilw-theme-blue-outline` / `ilw-theme-orange-outline` | Outlined variants |
| `ilw-theme-blue-inverse` / `ilw-theme-orange-inverse` | Inverted colors for dark backgrounds |

Themes work in **both** placements:
- On a standalone button, alongside `.ilw-button` on the same element.
- On the `ul.ilw-buttons` container, where it applies to every child. This works because a theme class sets the `--ilw-color--control` and `--ilw-color--control-text` custom properties, which inherit down to the children that consume them. Setting the theme on an individual child inside the list also works and overrides the container.

**Sizes:**
- `ilw-button-large`: Larger padding and font size.
- `ilw-button-small`: Compact buttons for tight spaces.

Apply to the `ul.ilw-buttons` container, or alongside `.ilw-button` on a standalone button.

The stylesheet also defines an `.ilw-buttons-left` modifier for the `ul.ilw-buttons` container (an alignment variant; exact behavior unverified — confirm against the live toolkit before use).

## 2. Impactful Content
- **`<ilw-statistic>`**: Displays numbers/data in large, accessible formats.
- **`<ilw-quote>`**: Styled blockquotes for testimonials or impactful statements.
- **`<ilw-video>`**: Auto-sizing container for accessible video embeds (YouTube, Vimeo, etc.).
- **`<ilw-content>`**: Apply consistent Illinois styling, spacing, alignment, and color themes to headings, paragraphs, lists, tables, and other page content.

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
- **`<ilw-carousel>`**: A component for displaying a sequence of slides with images, text, and call-to-action buttons.

**Structure:**
- `<ilw-carousel>`: Root container.
- `<ilw-slide>`: Individual slide container.
    - `<h3>`: Slide title.
    - `<p>`: Slide description.
    - `<div slot="buttons">`: Wrapper for the CTA button list. Place a `<ul class="ilw-buttons">` inside it.
    - `<img slot="image">`: Background image for the slide.

**Attributes:**
- `controls`: `compact` (smaller navigation arrows).
- `theme`: `blue`, `gray` (background color for the text area).
- `playing`: `true`/`false` (enables/disables auto-play). **Leave this off** — see the auto-play rule below.
- `width`: `full` (expands to viewport width).
- `overlay`: `true` (places text content over the image).

> **Accessibility — auto-play / motion (rule):** **Do not set `playing="true"`.** Ship carousels paused: omit the attribute or set `playing="false"`, and let the user advance slides with the `controls` arrows. Auto-advance triggers WCAG 2.2 SC 2.2.2, which requires a visible pause/stop control, pause on hover/focus, and `prefers-reduced-motion` handling — see `ACCESSIBILITY.md` §1 ("Auto-moving content"). A paused carousel satisfies 2.2.2 unconditionally, so this is the compliant default and needs no verification.
>
> Enable auto-play only when a stakeholder explicitly requires it. In that case treat the three requirements above as your responsibility: verify against the live component that each is met, and supply whatever is missing before shipping.

**Example:**
```html
<ilw-carousel controls="compact" theme="blue">
    <ilw-slide>
        <h3>Innovation at Illinois</h3>
        <p>Exploring the frontiers of technology and research.</p>
        <div slot="buttons">
            <ul class="ilw-buttons ilw-theme-blue-solid">
                <li><a href="/research">Learn More</a></li>
            </ul>
        </div>
        <img src="slide1.jpg" alt="Campus research lab" slot="image">
    </ilw-slide>
</ilw-carousel>
```
- **`<ilw-card>`**: Container for image/icon + title + text (usually used in a `<ilw-grid>`).
- **`<ilw-tabs>`**: A component for organizing related content into switchable panels.

**Usage Guidelines:**
- **Do use**: To display groups of related information in a compact area.
- **Do not use**: When a user needs to read the entire page content sequentially, or for a multi-step process. In those cases, use separate pages or standard headings.

**Layouts:**
- **Vertical (Default)**: Recommended for most use cases, especially with more than 3-4 tabs.
- **Horizontal**: Triggered by `horizontal="true"`. Use only for 2-3 tabs to avoid truncation.
- **Mobile Behavior**: On mobile devices, all layouts collapse into an **accordion-like view**.

**Structure & Accessibility:**
- `<ilw-tabs>`: Root container. Label it with `aria-labelledby` pointing to a section header.
- `<div slot="tabs" role="tablist">`: Contains the trigger buttons.
    - `<button role="tab" id="tab1" aria-controls="panel1">`: Individual triggers. The active tab carries `aria-selected="true"`; inactive tabs carry `aria-selected="false"`.
- Content Panels: `div` elements with `role="tabpanel"` and `aria-labelledby` pointing to the controlling button's ID.
    - **Tip**: Use `<ilw-content mode="inset">` inside panels for proper formatting.

> **Accessibility — keyboard navigation (rule):** `<ilw-tabs>` owns the tablist keyboard behavior at runtime. **Author the static markup exactly as shown in the example below — `role`, `aria-controls`, `aria-labelledby`, and the initial `aria-selected` / `tabindex` state — and do not attach your own `keydown` handlers or roving-`tabindex` script inside the component.** Adding custom key handling double-manages state the component already drives and will break tab switching.
>
> The full ARIA tablist pattern in `ACCESSIBILITY.md` §1 ("Tabs (ARIA tablist) keyboard pattern") applies when you build a tablist **without** `<ilw-tabs>` — hand-rolled tabs must implement it in full.
>
> If keyboard navigation does not work in a browser test, that is a component bug: report it to the Web Toolkit owners rather than patching around it with local script.

**Attributes:**
<!-- TODO: Update according to https://github.com/web-illinois/ilw-tabs/blob/main/builder/versions/ilw-tabs.1.1.json -->
- `theme`: Color theme (`blue`, `gray`, `white`).
- `width`: 
    - (default): Constrained to the container.
    - `full`: Full width of the viewport.
    - `auto`: Full width background, but text is constrained to the container.
    - `page`: Text is constrained to the standard page width.
- `horizontal`: Set to `true` for a horizontal layout.
- `default`: ID of the panel to open on load (defaults to the first item).

**CSS Variables:**
- `--ilw-tabs--display`: Controls root layout (flex/block).
- `--ilw-tabs--tablist--display`: Controls button list layout.
- `--ilw-tabs--tablist--width`: Width of the button list in vertical layout (default `350px`).

**Example:**
```html
<h2 id="tabs-heading">Department Information</h2>
<ilw-tabs theme="blue" aria-labelledby="tabs-heading">
    <div slot="tabs" role="tablist">
        <button role="tab" id="btn1" aria-controls="panel1" aria-selected="true" tabindex="0">Overview</button>
        <button role="tab" id="btn2" aria-controls="panel2" aria-selected="false" tabindex="-1">Details</button>
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

## 3. Organizational Elements
- **`<ilw-accordion>`**: Wrapper for multiple `<ilw-accordion-panel>` elements.
- **`<ilw-accordion-panel>`**: Single collapsible section. Requires a `h3 slot="summary"`.
- **`<ilw-grid>`**: Manages alignment for responsive sets of cards or stats.
- **`<ilw-pagination>`**: Accessible page navigation for lists.

## 4. Metadata & Context
- **`<ilw-breadcrumbs>`**: Helps users understand site hierarchy. Placement guidance is in `LAYOUT.md` §4 (In-Page Navigation); prefer `LAYOUT.md` for where and how to position breadcrumbs within a page. Use this file only for the component's identity/role.
- **`<ilw-section-nav>`**: Contextual navigation for the current site branch. Full markup, manual mode, and placement are documented in `LAYOUT.md` §4; prefer `LAYOUT.md`.

## 5. Icons & Media
- **`<ilw-icon>`**: Wrapper for University of Illinois brand icons.

**Attributes:**
- `icon`: Name of the icon (see list below).
- `alt`: Alternative text. If omitted, the icon is hidden from screen readers (`aria-hidden="true"`). When an icon is the only content of an interactive control, omit `alt` and supply an `.ilw-sr-only` label on the control instead (see `ACCESSIBILITY.md` §7).
- `size`: CSS value and unit (e.g., `24px`, `2rem`).

**Example:**
```html
<ilw-icon icon="calendar" size="1.5rem" alt="Event Date"></ilw-icon>
```

- **`<ilw-icon-panel>`**: A content block featuring an icon, a linked heading, and descriptive text.

**Slots:**
- `icon`: The icon to display (usually an `<ilw-icon>` or `<svg>`).
- `link`: An `<a>` tag for the panel's heading.
- Default slot: Descriptive text/paragraph.

**Attributes:**
- `theme`: Background color (`white`, `gray`).
- `accent`: Icon/heading accent color (`blue`, `orange`).

**Example:**
```html
<ilw-icon-panel theme="gray" accent="orange">
    <ilw-icon slot="icon" icon="handbook"></ilw-icon>
    <a href="/handbook" slot="link">Student Handbook</a>
    <p>Policies and requirements for all graduate students.</p>
</ilw-icon-panel>
```

**Available Icon Names:**
*admissions, alumni, apply-now, audio, back, bill, bluesky, calendar, call, cancel, check, comment, directory, document, donor, download, email, employment, event, exit, facebook, faculty, family, faq, favorite, flickr, gift, give, google-plus, handbook, home, hours, hr, image, inclusive, info, instagram, intranet, job, less, link, linkedin, location, manual, menu, more, news, next, notification, parent, partners, pause, pay, pdf, phone, pinterest, plan, play, program-of-study, reddit, research, resources, safety, search, send, settings, shop, sitemap, snapchat, spotify, staff, students, submit, team, threads, tickets, tiktok, training, trash, twitter, unit, user, video, volume, website, weibo, whatsapp, workshop, x, yelp, youtube.*

## 6. Profiles
- **`<ilw-profile>`**: A comprehensive layout for faculty, staff, or student profiles.

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

> **Note:** The example below is reconstructed from the documented slot list and has not been verified against the live component. Confirm slot names and markup before relying on it.

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
    <div slot="left">
        <h2>Office Hours</h2>
        <p>Tuesdays &amp; Thursdays, 2–4 PM.</p>
    </div>
    <h2>Biography</h2>
    <p>Dr. Doe's research focuses on distributed systems and accessibility.</p>
</ilw-profile>
```

- **`<ilw-profile-card>`**: A compact card variant for displaying profile information in grids.

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
- `round`: `true` (rounded corners for the card).
- `aspectratio`: `1/1`, `2/1`, `16/10`, `10/16` (sets the image container ratio).

> **Note:** The example below is reconstructed from the documented slot list and has not been verified against the live component. Confirm slot names and markup before relying on it.

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

- **`<ilw-profile-list>`**: A compact list-item variant for high-density profile directories.

**Slots:**
- `image`: Profile photo (`<img slot="image">`).
- `name`: Person's name link (`<a slot="name">`).
- `subtitle`: Department or job title.
- Default slot: Contact links (email, phone).

**Attributes:**
- `circle`: `circle` (clips profile image to a circle).

**Example:**
```html
<ilw-profile-list circle="circle">
    <img slot="image" src="avatar.jpg" alt="Jane Smith">
    <a href="/profile/jane" slot="name">Smith, Jane</a>
    <div slot="subtitle">Department of Redundancy</div>
    <div>
        <a href="mailto:jsmith@illinois.edu">jsmith@illinois.edu</a> | 
        <a href="tel:2175551234">(217) 555-1234</a>
    </div>
</ilw-profile-list>
```
