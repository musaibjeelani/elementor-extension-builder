# Changelog

All notable changes to this skill are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and releases use [Semantic Versioning](https://semver.org/).

## [1.1.0] - 2026-10-30

### Added

- Portable Agent Plugins manifest for ChatGPT and Codex distribution.
- Claude Code-compatible plugin manifest and self-hosted marketplace catalog.
- Cross-platform installation, invocation, validation, and publishing instructions.

### Changed

- Moved the skill into the portable `skills/elementor-extension-builder/` package layout.
- Marked the OpenAI package as desktop-only because its workflow requires local project access and code execution.
- Renamed the unpublished package and skill to `elementor-extension-builder` for clearer action-based discovery and to avoid a redundant `Plugin` suffix.

## [1.0.0] - 2026-10-30

### Added

- Initial public skill for building isolated Elementor extension plugins on existing WordPress sites.
- Architecture guidance for widgets, Theme Builder templates, global behavior, and environment-safe template seeding.
- Elementor implementation guidance covering controls, dynamic links, scoped CSS, idempotent JavaScript, editor safety, accessibility, and WPML.
- Validation and deployment checklist covering syntax, responsive layouts, visual fidelity, upgrades, packaging, and production handoff.
- Public installation, usage, contribution, security, and publishing documentation.
