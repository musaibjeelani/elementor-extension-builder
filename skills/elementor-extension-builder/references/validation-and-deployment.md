# Validation and deployment

## Static checks

Run the checks available in the target environment:

```powershell
php -l path\to\changed-file.php
node --check path\to\frontend.js
git diff --check
```

Repeat PHP lint for every changed PHP file. Validate `wpml-config.xml` and any other XML with an available XML parser. Run the project's existing build command if Sass or compiled assets are part of the plugin.

## Functional matrix

Verify at least:

- plugin activation with Elementor active and graceful behavior when an optional dependency is absent;
- Elementor editor insertion, control changes, responsive preview, save, reload, duplicate, and delete;
- frontend desktop, tablet, and phone layouts at representative widths;
- keyboard navigation, focus-visible states, reduced motion, alt text, headings, labels, and contrast;
- every internal page/post/CPT link and every external URL mode;
- empty, single-item, long-content, missing-image, and maximum repeater/query states;
- hover/open/active animations and stable layout during tabs, sliders, fonts, and image loads;
- Theme Builder conditions and global header/footer behavior;
- WPML original, translated, and RTL output when present;
- no PHP warnings, JavaScript errors, duplicate network requests, or duplicate event handlers.

## Upgrade regression

Test against a configured copy of the previous plugin version:

1. Change an Elementor field and rename any seeded page/template.
2. Replace the plugin files without deactivating.
3. Load admin and frontend so the versioned upgrade runs.
4. Confirm the changed content remains, the renamed object is reused, and no duplicates are created.
5. Confirm new defaults appear only for newly inserted widgets or through an explicit migration.

## Visual comparison

Compare the implementation with the supplied reference at matching viewport sizes. Check typography, line breaks, widths, spacing, alignment, crop, colors, border radii, shadows, z-index, hover states, animation timing, and section-to-section rhythm. Prefer screenshots or browser inspection over memory.

## Packaging

- Increment the plugin version and asset cache key.
- Exclude source references, local notes, credentials, development URLs, caches, and build-only dependencies.
- Ensure the ZIP contains exactly one top-level plugin directory.
- Test installing the ZIP on staging, then activate and clear Elementor generated CSS/files plus site/CDN caches as needed.
- Provide the final version, ZIP/folder location, created templates/pages, assignment steps, and any content or plugin dependency.

Do not promise that activation alone will replace production pages unless the plugin deliberately implements and verifies that assignment. State clearly whether editors must insert widgets or assign Theme Builder templates.
