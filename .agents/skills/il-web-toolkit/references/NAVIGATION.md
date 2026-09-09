# Navigation Components

Complex navigation widgets requiring specific ARIA structures.

<!-- TODO: Update this (https://github.com/web-illinois/ilw-header) -->
## 1. Header

## 2. Header Menu
The primary horizontal navigation used within `<ilw-header>`.

**Structure:**
- `<ilw-header-menu>`: Root container.
    - Attributes: `compact="true"`, `width="990"`.
- `<ul>`: Top-level list of links or sections.
- `<ilw-header-menu-section>`: A dropdown section.
    - `slot="label"`: Text label for the dropdown.
    - `slot="link"`: Anchor tag if the label itself is a link (requires `linked="true"`).
    - `right`: Attribute to right-align the dropdown menu.
    - `<ul>`: Nested list for submenu items.

**Example:**
```html
<ilw-header-menu>
    <ul>
        <li><a href="/">Home</a></li>
        <li>
            <ilw-header-menu-section>
                <span slot="label">Resources</span>
                <ul>
                    <li><a href="/docs">Documentation</a></li>
                    <li>
                        <ilw-header-menu-section linked="true">
                            <a slot="link" href="/api">API Reference</a>
                            <ul>
                                <li><a href="/api/v1">v1</a></li>
                            </ul>
                        </ilw-header-menu-section>
                    </li>
                </ul>
            </ilw-header-menu-section>
        </li>
    </ul>
</ilw-header-menu>
```

<!-- TODO: ilw-header-menu (https://github.com/web-illinois/ilw-header-menu) -->