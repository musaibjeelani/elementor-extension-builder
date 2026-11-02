# Elementor Extension Builder

A portable AI-agent plugin for building project-specific WordPress Elementor extensions without replacing an existing site's theme or page builder.

It packages one reusable skill for:

- ChatGPT and Codex through the Agent Plugins format;
- Claude Code through its compatible plugin and marketplace manifests;
- direct standalone skill installation when a full plugin marketplace is not needed.

> This repository does not contain a universal WordPress plugin. It gives the coding agent a production-oriented workflow for inspecting each website and creating the correct isolated WordPress plugin for that project.

## What the skill helps build

- Reusable Elementor widgets for new page sections
- Elementor content, repeater, style, responsive, and animation controls
- Page, post, and public custom-post-type link selection
- Optional external URLs with target and relationship settings
- Elementor Theme Builder-compatible headers and footers
- Loaders, progress indicators, sticky navigation, and other global behavior
- Dynamic post queries, tabs, sliders, menus, testimonials, and content grids
- Scoped CSS resistant to Elementor Kit and inline-style conflicts
- Idempotent JavaScript compatible with Elementor frontend and editor rendering
- WPML-ready fields, templates, translated links, and language switchers
- Safe activation, versioned upgrades, template seeding, and production packaging

## Supported hosts

| Host | Supported | Invocation |
| --- | --- | --- |
| Codex | Yes | `$elementor-extension-builder` |
| ChatGPT with local Codex execution | Yes | Select the installed plugin/skill, then describe the task |
| Claude Code | Yes | `/elementor-extension-builder:elementor-extension-builder` |
| Other Agent Skills-compatible hosts | Usually | Install the folder under `skills/elementor-extension-builder/`; host-specific metadata may be ignored |

The core workflow is provider-neutral. `agents/openai.yaml`, root `plugin.json`, and `.claude-plugin/plugin.json` provide host-specific discovery and presentation metadata without duplicating the skill instructions.

## Requirements

- A local coding-agent environment with filesystem and command execution access
- An accessible WordPress project using Elementor
- A design reference, screenshots, HTML/CSS/JS files, or a written component specification
- Elementor Pro only when requested features require Pro functionality such as Theme Builder
- WPML only when multilingual behavior is required

This package is marked desktop-only for OpenAI because its core workflow inspects and modifies local project files.

## Repository structure

```text
elementor-extension-builder/
├── plugin.json
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── skills/
│   └── elementor-extension-builder/
│       ├── SKILL.md
│       ├── agents/
│       │   └── openai.yaml
│       └── references/
│           ├── architecture.md
│           ├── elementor-implementation.md
│           └── validation-and-deployment.md
├── README.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── SECURITY.md
├── LICENSE
└── .gitignore
```

## Install in Codex

### Plugin marketplace installation

After publishing this folder to GitHub, replace `OWNER/REPOSITORY` below:

```bash
codex plugin marketplace add OWNER/REPOSITORY
```

Restart the ChatGPT desktop app, open the Plugins Directory, choose the added marketplace, and install **Elementor Extension Builder**.

### Standalone skill installation

Users who only want the skill can copy this directory:

```text
skills/elementor-extension-builder
```

to:

```text
~/.codex/skills/elementor-extension-builder
```

For a Git checkout on Windows PowerShell:

```powershell
git clone YOUR_REPOSITORY_URL elementor-extension-builder-source
$skillDestination = Join-Path $HOME '.codex\skills\elementor-extension-builder'
New-Item -ItemType Directory -Force -Path (Split-Path $skillDestination) | Out-Null
Copy-Item -Recurse '.\elementor-extension-builder-source\skills\elementor-extension-builder' $skillDestination
```

Open a new Codex chat after installation.

## Install in Claude Code

### Install through the bundled marketplace

Replace `OWNER/REPOSITORY` with the GitHub shorthand:

```bash
claude plugin marketplace add OWNER/REPOSITORY
claude plugin install elementor-extension-builder@elementor-extension-tools
```

Or install from inside Claude Code:

```text
/plugin install elementor-extension-builder --marketplace OWNER/REPOSITORY
```

### Test a local checkout without installing

```bash
claude --plugin-dir ./elementor-extension-builder
```

Inside Claude Code, invoke the skill with:

```text
/elementor-extension-builder:elementor-extension-builder
```

Claude can also select the skill automatically when the request matches its description.

## Use the skill

### Codex prompt

```text
Use $elementor-extension-builder to integrate the supplied design into my existing Elementor website as a separate custom plugin. Preserve the current theme, Elementor pages, and third-party plugins.
```

### Claude Code prompt

```text
/elementor-extension-builder:elementor-extension-builder

Integrate the supplied design into my existing Elementor website as a separate custom plugin. Preserve the current theme, Elementor pages, and third-party plugins.
```

Provide as much of the following as is available:

- local WordPress project path or repository;
- design URL, design files, screenshots, or HTML/CSS/JS reference;
- pages and global templates that must change;
- required Elementor fields and dynamic content sources;
- supported devices and browsers;
- whether Elementor Pro, WPML, ACF, or custom post types are active;
- whether the agent should only build widgets or also create and assign draft templates;
- staging URL and access only when browser verification is requested.

### Example: new page components

```text
Use the Elementor Extension Builder skill for this existing Elementor site.

Project: D:\sites\client-wordpress
Reference design: D:\designs\homepage-v2

Build the hero, services, testimonials, and contact sections as reusable Elementor widgets. Make text, images, colors, spacing, tabs, and links editable. Internal links must support pages, posts, and public custom post types, with an optional external URL mode. Package everything as one installable WordPress plugin and do not modify the active theme.
```

### Example: global templates and WPML

```text
Use the Elementor Extension Builder skill to create a plugin-owned header, mega menu, footer, and loader for this existing Elementor Pro website. Keep visible content editable through Elementor templates, support WPML translations, use current WPML language-switcher output, and avoid hardcoded template or page IDs between staging and production.
```

### Example: update an existing generated plugin

```text
Use the Elementor Extension Builder skill to compare the updated design with the current custom Elementor plugin. Implement only changed components. Preserve editor-modified content, run a versioned idempotent upgrade, and verify that no duplicate pages, templates, menus, or widgets are created.
```

## Expected workflow

1. Inspect repository instructions, WordPress setup, Elementor versions, theme/templates, post types, global styles, and existing custom code.
2. Compare every component and state against the supplied reference at representative viewport sizes.
3. Decide which features belong in widgets, Theme Builder templates, plugin hooks, or plugin settings.
4. Implement a uniquely prefixed and scoped custom WordPress plugin.
5. Add Elementor controls, dynamic links, accessibility, responsive states, animations, WPML support, and safe upgrades as required.
6. Run syntax, editor, frontend, responsive, visual, multilingual, and upgrade checks.
7. Deliver the WordPress plugin folder or ZIP with deployment and assignment instructions.

## Validate

### Validate the skill for Codex

Use the validator bundled with Codex's `skill-creator` skill:

```text
python <codex-skills>/.system/skill-creator/scripts/quick_validate.py ./skills/elementor-extension-builder
```

The validator requires PyYAML:

```text
python -m pip install PyYAML
```

### Validate the Claude plugin

With Claude Code installed:

```bash
claude plugin validate --strict .
```

Then run a clean local session:

```bash
claude --plugin-dir .
```

## Publish on GitHub

Before the first public release, replace the generic contributor name in both manifests with your name or organization. Add final `homepage` and `repository` URLs after the GitHub repository exists.

From this folder:

```bash
git init
git add .
git commit -m "feat: publish cross-platform Elementor extension skill"
git branch -M main
git remote add origin YOUR_REPOSITORY_URL
git push -u origin main
git tag -a v1.1.0 -m "Release 1.1.0"
git push origin v1.1.0
```

Recommended GitHub topics:

```text
agent-skills, codex-skill, claude-code-plugin, wordpress, elementor, wordpress-plugin, wpml
```

## Publish to plugin directories

### OpenAI Plugin Directory

1. ZIP the repository with `plugin.json` at the archive root or inside its single top-level folder.
2. Open the OpenAI Plugins dashboard and create a skills-only submission.
3. Upload the ZIP, resolve automated findings, complete verified developer metadata, and submit for review.
4. Because the workflow needs local project access, keep `requires_local_executor` enabled and follow the desktop-only submission requirements.

### Anthropic directory

1. Run `claude plugin validate --strict .`.
2. Push the plugin to a public GitHub repository.
3. Open Anthropic's plugin directory developer portal with an eligible paid Claude plan.
4. Submit the repository and resolve review findings.

The included `.claude-plugin/marketplace.json` also lets users install directly from your GitHub repository without waiting for directory approval.

## Scope and limitations

- Generated WordPress plugins remain project-specific and require testing on the target website.
- Activation does not automatically replace live pages unless the implementation explicitly assigns and verifies page/template conditions.
- Elementor Pro, WPML, ACF, and similar plugins are optional dependencies only when requested functionality needs them.
- Host-specific metadata or invocation syntax may differ, but the core `SKILL.md` workflow stays the same.
- Never commit client credentials, database dumps, private design assets, production URLs, or client-specific generated plugins to this public repository.

## License

Released under the [MIT License](LICENSE).
