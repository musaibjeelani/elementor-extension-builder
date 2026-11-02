# Elementor implementation guide

## Registration and compatibility

- Declare minimum PHP, WordPress, Elementor, and optional Elementor Pro versions.
- Load only after Elementor is available. Show an admin notice instead of fatally failing when a dependency is missing.
- Use current Elementor registration hooks and APIs for the installed version.
- Enqueue widget assets only where needed when practical.
- Keep editor-only CSS/JS separate from frontend assets.

## Controls and fields

Use controls that match the content model:

- Text, textarea, WYSIWYG, media, icons, color, typography, dimensions, slider, switcher, URL, select, and responsive controls for single values.
- Repeaters for cards, tabs, slides, menu groups, metrics, people, testimonials, or navigation rows.
- Query controls for post type, taxonomy, ordering, limits, and manual selection.
- Style controls should emit scoped CSS through Elementor selectors where possible.

Defaults should reproduce the approved design on first insertion. Avoid treating a default as a forced value; editors must be able to change it.

## Internal and external links

For links requiring content selection, expose:

1. Link mode: internal content or custom URL.
2. Internal object: searchable selection of published entries from allowed public post types, including pages.
3. Custom URL using Elementor's URL control.
4. Optional new-tab and `nofollow`/sponsored controls where relevant.

At render time, validate that the selected object still exists, call `get_permalink()`, and apply current-language resolution when a translation plugin provides it. Render a non-link fallback when no valid destination exists. Do not cache a domain-specific permalink in widget data.

## CSS isolation and design fidelity

- Give every widget a root such as `[data-project-widget="hero"]`; use it in all selectors.
- Prefer component tokens on the root, for example `--component-accent`, and set defaults from widget controls.
- Reset only properties the component owns. Never apply broad selectors such as `h2`, `a`, `button`, `.elementor-button`, or `img` globally.
- Elementor kit inline CSS may load after plugin CSS. Increase specificity inside the component root before using `!important`. If `!important` is unavoidable, keep it scoped and explain why.
- Preserve aspect ratios, `object-fit`, line clamping, font loading, and breakpoint behavior from the reference.
- Check active, hover, focus-visible, open, disabled, and editor states separately.

## JavaScript lifecycle

Each initializer must:

- Accept a widget root, return early if already initialized, and store its instance or flag on that root.
- Work on normal `DOMContentLoaded` and Elementor's frontend element-ready hooks.
- Destroy or reuse slider/observer instances when Elementor rerenders a widget.
- Scope DOM queries to the widget root.
- Support keyboard and reduced-motion behavior.
- Avoid global mutation observers unless the feature truly requires one; disconnect observers when no longer needed.

For tabs or sliders, keep panel height stable when the approved design is stable. Synchronize text, image, pagination, and accessibility state from a single active index.

## Editor-safe global UI

Loaders, sticky headers, overlays, and fixed panels can block Elementor editing. Detect edit/preview modes and either disable blocking behavior or render a noninteractive preview. Do not hide controls from the editor merely to avoid fixing the preview.

## WPML and translations

- Add `wpml-config.xml` entries for translatable widget fields and repeaters.
- Resolve translated post/template IDs through WPML filters when available, while keeping a no-WPML fallback.
- Prefer Elementor templates for translatable global visual content.
- Use WPML's language-switcher API, shortcode, or widget output for functionality; style its existing markup rather than recreating language URLs manually.
- Test original and secondary languages, including RTL where applicable.

## Output safety

- Escape text, attributes, URLs, HTML, SVG, and JSON for their output contexts.
- Allow rich text only through a deliberate `wp_kses` policy.
- Sanitize settings and verify nonces/capabilities for AJAX or admin actions.
- Prefer bundled, reviewed SVG files or Elementor icon controls. Never print arbitrary untrusted SVG markup.
