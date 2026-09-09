# Core Standards & Branding

This component library follows University of Illinois branding and strict accessibility guidelines.

## 1. CDN & Assets
Production Illinois pages load the v3 toolkit.

<!-- TODO: Add header contents from generic HTML template link -->
```html
<!-- Toolkit v3 (required for ilw- web components) -->
<link rel="stylesheet" href="https://cdn.toolkit.illinois.edu/3/toolkit.css" />
<script src="https://cdn.toolkit.illinois.edu/3/toolkit.js" type="module"></script>
```

> **Load order:** The snippets in this skill load the v3 stylesheet before the v2 stylesheet, which means that where the two define equal-specificity rules for the same element, v2 wins the cascade tie. Whether this matches the officially recommended order has not been verified — keep whatever order the consuming site already uses consistent across its pages rather than reordering.

### Versioning

<!-- TODO: One -->
The two URLs above pin differently, and this is deliberate to reflect production — but be aware of what each means:

<!-- TODO: Pin major for continuous maintenance, pin to minor for no maintenance -->
- `3/` is a **floating major**. It picks up minor and patch releases automatically, so a page that works today can change behavior without a code change.

Not every `ilw-` component is necessarily bundled in `3/toolkit.js`, and the bundle's contents drift over time because `3/` floats. Some components are (or were) published at their own independently versioned per-component paths — e.g. `cdn.toolkit.illinois.edu/ilw-tooltip/1.2/ilw-tooltip.js` (see `OVERLAYS.md` §3). **As of August 2026, `ilw-tooltip` is registered by the `3/toolkit.js` bundle itself**, so the standalone asset is only needed when the bundle is not loaded — and loading both risks double registration of the custom element. Before adding a per-component asset, check whether the component is already defined by the bundle; before assuming a component is in the bundle, check whether its reference file names a separate CDN path.

For production deployments, prefer pinning an exact version over the floating `3/` so releases are reproducible.

## 2. Naming Conventions
- **`ilw-` prefix**: Standardized Layout/Page components and Web Toolkit components.

## 3. Layout Standards
The mandatory skeleton for a compliant page uses the `<ilw-page>` component. This handles margins, fonts, and standard slots for header/footer.

```html
<body style="margin: 0;">
  <ilw-page class="ilw-margin ilw-font">
    <ilw-header slot="header">
        <!-- Header Content -->
    </ilw-header>

    <main>
        <!-- Page Content -->
    </main>

    <ilw-footer slot="footer">
        <!-- Footer Content -->
    </ilw-footer>
  </ilw-page>
</body>
```
## 4. Accessibility Mandates

> **Note:** This section is a quick-reference summary only. `ACCESSIBILITY.md` is the authoritative source and takes precedence — consult it before implementing any of the rules below.

- **Focus Traps**: Mandatory for modal components (Alerts, Slideovers, and the `<ilw-modal>`).
- **Keyboard Navigation**: 
    - `Escape` must close overlays.
    - Arrow keys for Treeview and Dropdown navigation.
- **ARIA Attributes**: Always include `role`, `aria-expanded`, `aria-controls`, and `aria-labelledby` as specified in component docs.