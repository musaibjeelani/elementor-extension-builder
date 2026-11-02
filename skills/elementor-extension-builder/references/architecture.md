# Architecture decisions

Use this guide after discovery and before coding.

## Placement matrix

| Requirement | Preferred placement | Reason |
| --- | --- | --- |
| Reusable page section | Custom Elementor widget | Editors can place and configure it on existing pages. |
| Page-specific composite layout | Several plugin widgets on the existing Elementor page | Avoid a second page builder or theme template fork. |
| Global header or footer | Elementor Theme Builder template containing plugin widgets | Keeps display conditions and translations in Elementor. |
| Loader or scroll progress | Plugin runtime hook plus Elementor-managed visible content/settings | Runtime starts early; copy remains editable and translatable. |
| Content list | Widget query controls for a selected public post type/taxonomy | Reuses existing site content. |
| Small one-off style adjustment | Scoped plugin CSS | Avoid theme edits while keeping deployment atomic. |
| Site-wide data model | Plugin-owned settings/meta with an Elementor control surface | Keeps schema independent from the theme. |

## Suggested plugin structure

```text
project-elementor-extension/
  project-elementor-extension.php
  includes/
    class-plugin.php
    class-assets.php
    class-upgrades.php
    class-link-resolver.php
    class-template-manager.php
  widgets/
    class-widget-base.php
    class-hero-widget.php
    class-content-section-widget.php
  assets/
    css/frontend.css
    css/editor.css
    js/frontend.js
    images/
    icons/
    fonts/
  languages/
  wpml-config.xml
```

Adjust structure to project size. Do not create abstraction layers that only have one trivial consumer.

## Environment portability

- Generate a unique prefix from the client/project slug, not from this skill name.
- Use WordPress APIs for URLs, uploads, posts, templates, and the configured front page.
- Keep imported assets inside the plugin unless the editor must replace them through the media library.
- Store stable logical keys in plugin meta/options; resolve environment-specific IDs at runtime or during an idempotent setup pass.
- A copied plugin must activate on staging or production without editing PHP constants.
- Never include credentials, local filesystem paths, development domains, or database exports in the deliverable.

## Template creation and assignment

When automatic template creation is requested:

1. Create a draft Elementor Library document through supported WordPress/Elementor APIs.
2. Store a stable seed key and schema version in post meta.
3. Reuse the existing seeded document on future upgrades, even if its title changed.
4. Separate creation from publishing and Theme Builder assignment.
5. Preserve manually edited Elementor JSON. Apply only explicit, versioned migrations that can distinguish untouched defaults from editor changes.

If supported APIs are unavailable or unstable for the installed Elementor version, create the widget and let an editor place it manually rather than manipulating opaque `_elementor_data` blindly.
