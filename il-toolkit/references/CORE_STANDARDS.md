# Core Standards & Branding

This component library follows University of Illinois branding and strict accessibility guidelines.

## 1. CDN & Assets
Production Illinois pages load the v3 toolkit and associated assets.

```html
<link rel="icon" href="https://cdn.brand.illinois.edu/favicon.ico"> 
<link rel="apple-touch-icon" href="https://cdn.brand.illinois.edu/touch-icon-40x40.png">
<link rel="apple-touch-icon" sizes="152x152" href="https://cdn.brand.illinois.edu/touch-icon-152x152.png">
<link rel="apple-touch-icon" sizes="167x167" href="https://cdn.brand.illinois.edu/touch-icon-167x167.png">
<link rel="apple-touch-icon" sizes="180x180" href="https://cdn.brand.illinois.edu/touch-icon-180x180.png">
<link rel="dns-prefetch" href="//cdn.toolkit.illinois.edu"> 
<link rel="dns-prefetch" href="//cdn.disability.illinois.edu"> 
<link rel="stylesheet" href="//cdn.toolkit.illinois.edu/3/toolkit.css">
<script type="module" src="//cdn.toolkit.illinois.edu/3/toolkit.js"></script>
<script src="//cdn.disability.illinois.edu/skipto.min.js"></script>
<script>var SkipToConfig = { 'settings': { 'skipTo': { colorTheme: 'illinois' } } };</script>
```

### Versioning

The Toolkit URL pinning above is deliberate to reflect development — but be aware of it means:

- `3/` is a **floating major**. It picks up minor and patch releases automatically, so a page that works today can change behavior without a code change.

For production deployments, prefer pinning a minor version over the floating `3/` so releases are reproducible. The latest version will always be posted on the https://builder3.toolkit.illinois.edu/ site with the text "Latest version is x.xx"

Example: 
```html
<link rel="stylesheet" href="//cdn.toolkit.illinois.edu/3.14/toolkit.css">
<script type="module" src="//cdn.toolkit.illinois.edu/3.14/toolkit.js"></script>
```

## 2. Naming Conventions
- **`ilw-` prefix**: Standardized Layout/Page components and Web Toolkit components.

## 3. Layout Standards
The mandatory skeleton for a compliant page uses the `<ilw-page>` component. This handles margins, fonts, and standard slots for header/footer.

```html
<body style="margin: 0;">
  <ilw-page class="ilw-font">
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