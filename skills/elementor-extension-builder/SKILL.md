---
name: elementor-extension-builder
description: Build or extend an isolated custom WordPress plugin that adds reusable Elementor widgets, editable fields, templates, assets, and site behavior to an existing Elementor website without replacing its theme or page builder. Use when integrating supplied HTML/CSS/JS or design references, creating client-specific Elementor components, adding dynamic internal or external links, supporting WPML, or packaging an ongoing-site redesign for simple deployment. Do not use for a content-only edit that existing Elementor widgets already handle cleanly.
---

# Elementor Extension Builder

Add the requested design as a self-contained plugin while preserving the site's current theme, Elementor content, third-party plugins, and production behavior. Make the result editable in Elementor and deployable as one plugin folder or ZIP.

## Start with discovery

1. Inspect the repository and read any `AGENTS.md` instructions before editing.
2. Identify WordPress, PHP, Elementor, Elementor Pro, active theme, WPML, and relevant plugin versions.
3. Locate the existing header, footer, Theme Builder templates, page templates, post types, global kit styles, fonts, and asset-loading pattern.
4. Compare the supplied reference against the rendered site at desktop, tablet, and mobile sizes. Inventory every component, state, animation, field, link, and asset before implementation.
5. Check for existing work from an earlier plugin version. Preserve content editors have already changed.

Read [architecture.md](references/architecture.md) before choosing where each feature belongs. For Elementor APIs, fields, links, CSS, JavaScript, WPML, and editor behavior, read [elementor-implementation.md](references/elementor-implementation.md).

## Choose the least invasive architecture

- Put reusable page sections in custom Elementor widgets.
- Put global header/footer designs in Elementor Theme Builder templates containing plugin widgets when Elementor Pro is available.
- Put cross-page runtime behavior such as loaders or scroll indicators in plugin hooks, but keep visible/translatable values in Elementor controls or an Elementor template.
- Keep all PHP, CSS, JavaScript, fonts, icons, images, migrations, and defaults inside the custom plugin.
- Never edit WordPress core. Do not change the active theme or third-party plugins unless the user explicitly requests it and no isolated solution is viable.
- Never hardcode page, template, attachment, or post IDs that will differ between environments.

## Build the plugin safely

1. Use a unique client/project prefix and PHP namespace for functions, handles, classes, options, meta keys, and DOM roots.
2. Register a dedicated Elementor category and one widget class per independently reusable component.
3. Expose content, repeaters, responsive settings, images, colors, typography, spacing, animation toggles, and display conditions as appropriate controls. Supply design-reference values as defaults while keeping them editable.
4. For every destination link, support internal content selection across public post types plus an optional custom URL. Preserve target, `rel`, and accessibility attributes.
5. Scope styles beneath a unique root class or data attribute. Account for Elementor kit and `elementor-frontend-inline-css` specificity without leaking styles into the rest of the site.
6. Make JavaScript idempotent and compatible with Elementor frontend/editor initialization. Prevent duplicate listeners and sliders after editor refreshes.
7. Escape rendered output, sanitize stored data, check capabilities and nonces for mutations, and avoid secrets or environment URLs in code.
8. Add accessible keyboard, focus, reduced-motion, semantic, and responsive behavior.

## Make installation and upgrades predictable

- Use activation only for first-install setup. Also run a versioned, idempotent upgrade check so replacing the plugin folder applies new schema/setup work without requiring deactivate/reactivate.
- Create pages/templates as drafts unless publishing or assignment is explicitly requested.
- Mark seeded content with a stable plugin-owned key or meta value and look it up by that key. Do not identify it by title alone.
- Do not recreate an item after an editor renames it, and do not overwrite user-edited Elementor data during routine upgrades.
- Resolve the front page from WordPress settings or explicit plugin settings instead of a development-site ID.
- Store display scope as `all public pages` or selected object IDs, and translate/resolve those selections for the current environment where needed.
- Use plugin versioning for cache busting and document any migration that changes stored data.

## Verify before handing off

Follow [validation-and-deployment.md](references/validation-and-deployment.md). At minimum:

1. Run PHP syntax checks, JavaScript syntax checks, XML validation where applicable, and whitespace/diff checks.
2. Verify the Elementor editor and frontend at desktop, tablet, and mobile breakpoints.
3. Test every control, repeater, internal/external link, hover/focus state, animation, dynamic post query, template condition, and WPML language.
4. Confirm assets load once, later Elementor inline CSS does not override the component, and editor previews are never blocked by fullscreen/fixed UI.
5. Test upgrade behavior against an existing configured copy so no duplicate templates, menus, pages, or widgets are created.
6. Package the plugin with the plugin directory as the ZIP root and give concise install/update/cache-clear instructions.

If direct access to WordPress admin is available, use the existing `wordpress-elementor` skill for browser/WP-CLI editing and verification. Keep this skill responsible for the isolated plugin architecture and implementation.
