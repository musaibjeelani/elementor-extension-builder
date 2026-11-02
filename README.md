# Elementor Extension Builder

Elementor Extension Builder is a reusable AI-agent skill for adding project-specific designs and functionality to an existing WordPress Elementor website as an isolated custom plugin.

It helps an agent inspect the current site, convert supplied designs into editable Elementor components, preserve the active theme and existing pages, and package the result for predictable deployment.

> This repository is an agent skill, not a universal WordPress plugin. It teaches the coding agent how to build the correct plugin for each website.

## What it is designed for

- Converting HTML, CSS, JavaScript, screenshots, or design references into reusable Elementor widgets
- Adding editable content, repeater, style, responsive, link, and animation controls
- Building Elementor Theme Builder-compatible headers and footers
- Supporting internal page, post, and public custom-post-type links plus optional external URLs
- Adding loaders, sticky navigation, sliders, tabs, menus, testimonials, and dynamic content grids
- Isolating CSS from Elementor Kit and inline-style conflicts
- Keeping JavaScript safe across Elementor frontend and editor rerenders
- Supporting WPML-managed fields, translated content, links, and language switchers
- Creating environment-safe setup and upgrade routines without hardcoded page or template IDs

## What it does not do

- It does not replace Elementor, the active theme, or existing page content automatically.
- It does not provide one generic WordPress plugin that fits every website.
- It does not make production assignments or publish content unless the user explicitly requests those actions.
- It does not require an MCP server. The package is a skills-only plugin.

## How it works

```text
Existing WordPress + Elementor site
                |
                v
Agent inspects the theme, plugins, templates, content model, and design reference
                |
                v
Agent chooses the least invasive implementation
                |
                +--> Reusable sections: custom Elementor widgets
                +--> Header/footer: Theme Builder templates when available
                +--> Global behavior: isolated plugin hooks and settings
                |
                v
Agent builds and verifies one project-specific WordPress plugin
                |
                v
Developer installs the generated plugin and assigns widgets/templates as documented
```

The workflow keeps every generated WordPress plugin client-specific. This public repository contains only generic implementation guidance.

## Supported hosts

| Host | Support | Invocation |
| --- | --- | --- |
| ChatGPT desktop / Codex | Yes | `$elementor-extension-builder` |
| Claude Code | Yes | `/elementor-extension-builder:elementor-extension-builder` |
| Agent Skills-compatible hosts | Usually | Install `skills/elementor-extension-builder/` using the host's skill mechanism |

## Requirements

- A local coding-agent environment with filesystem and command execution access
- An accessible WordPress project using Elementor
- A design reference, screenshots, HTML/CSS/JS files, or a written component specification
- Elementor Pro only when the requested implementation needs Pro features such as Theme Builder
- WPML only when multilingual behavior is required

The OpenAI manifest marks this package as desktop-only because the workflow must inspect and modify local project files.

## Install in ChatGPT desktop or Codex

Add this repository as a plugin marketplace:

```bash
codex plugin marketplace add musaibjeelani/elementor-extension-builder
```

Restart the ChatGPT desktop app, open the Plugins Directory, select the `elementor-extension-tools` marketplace, and install **Elementor Extension Builder**.

Start a new chat after installation.

## Install as a standalone Codex skill

Clone the repository:

```bash
git clone https://github.com/musaibjeelani/elementor-extension-builder.git
```

Copy `skills/elementor-extension-builder/` into:

```text
~/.codex/skills/elementor-extension-builder
```

Windows PowerShell example:

```powershell
git clone https://github.com/musaibjeelani/elementor-extension-builder.git elementor-extension-builder-source
$destination = Join-Path $HOME '.codex\skills\elementor-extension-builder'
New-Item -ItemType Directory -Force -Path (Split-Path $destination) | Out-Null
Copy-Item -Recurse -Force '.\elementor-extension-builder-source\skills\elementor-extension-builder' $destination
```

Start a new Codex chat after copying the skill.

## Install in Claude Code

Add the bundled marketplace and install the plugin:

```bash
claude plugin marketplace add musaibjeelani/elementor-extension-builder
claude plugin install elementor-extension-builder@elementor-extension-tools
```

Start a new Claude Code session, then invoke the skill with:

```text
/elementor-extension-builder:elementor-extension-builder
```

To test a local checkout without installing it:

```bash
claude --plugin-dir ./elementor-extension-builder
```

## Use the skill

### ChatGPT desktop or Codex

```text
Use $elementor-extension-builder to integrate the supplied design into my existing Elementor website as a separate custom plugin. Preserve the current theme, Elementor pages, and unrelated plugins.
```

### Claude Code

```text
/elementor-extension-builder:elementor-extension-builder

Integrate the supplied design into my existing Elementor website as a separate custom plugin. Preserve the current theme, Elementor pages, and unrelated plugins.
```

Provide the information that is available:

- local WordPress project or repository path;
- design URL, local design files, screenshots, or HTML/CSS/JS reference;
- pages and global templates that must change;
- required editable fields and dynamic content sources;
- supported viewport sizes and browsers;
- whether Elementor Pro, WPML, ACF, or custom post types are active;
- whether the agent should only build widgets or also create draft templates;
- staging access only when browser verification is required.

## Example requests

### Build reusable page sections

```text
Use $elementor-extension-builder for this existing Elementor site.

Project: D:\sites\client-wordpress
Reference design: D:\designs\homepage-v2

Build the hero, services, testimonials, and contact sections as reusable Elementor widgets. Make text, images, colors, spacing, tabs, and links editable. Internal links must support pages, posts, and public custom post types, with an optional external URL mode. Package everything as one installable WordPress plugin and do not modify the active theme.
```

### Add global templates and multilingual behavior

```text
Use $elementor-extension-builder to create a plugin-owned header, mega menu, footer, and loader for this existing Elementor Pro website. Keep visible content editable through Elementor templates, support WPML translations, use WPML's language-switcher output, and avoid hardcoded template or page IDs between staging and production.
```

### Update an existing generated plugin

```text
Use $elementor-extension-builder to compare the updated design with the current custom Elementor plugin. Implement only changed components, preserve editor-modified content, run a versioned idempotent upgrade, and verify that no duplicate pages, templates, menus, or widgets are created.
```

## Expected output

The agent should provide:

- a self-contained project-specific WordPress plugin folder or ZIP;
- reusable Elementor widgets and controls requested by the design;
- bundled and scoped frontend assets;
- safe activation and upgrade behavior where setup is required;
- validation results covering syntax, editor behavior, frontend states, responsiveness, links, accessibility, and multilingual behavior as applicable;
- concise installation, assignment, cache-clear, and deployment instructions.

Activation alone does not replace production pages unless the generated plugin deliberately implements and verifies that assignment.

## Repository structure

```text
elementor-extension-builder/
|-- plugin.json
|-- .claude-plugin/
|   |-- plugin.json
|   `-- marketplace.json
|-- skills/
|   `-- elementor-extension-builder/
|       |-- SKILL.md
|       |-- agents/
|       |   `-- openai.yaml
|       `-- references/
|           |-- architecture.md
|           |-- elementor-implementation.md
|           `-- validation-and-deployment.md
|-- .github/
|   |-- CODEOWNERS
|   `-- PULL_REQUEST_TEMPLATE.md
|-- CHANGELOG.md
|-- CONTRIBUTING.md
|-- SECURITY.md
`-- LICENSE
```

## Contributing

Suggestions and improvements are welcome through GitHub Issues and pull requests. Fork the repository, create a focused branch, make a generic change that does not expose client information, and open a pull request.

Contributor setup, validation, and release guidance belong in [CONTRIBUTING.md](CONTRIBUTING.md). End users do not need Codex's `skill-creator` validator or Claude's plugin validator to install and use this skill.

## Security and privacy

Never include client credentials, database dumps, private designs, production secrets, environment-specific IDs, or generated client plugins in a public issue or pull request. See [SECURITY.md](SECURITY.md) for private vulnerability reporting guidance.

## License

Released under the [MIT License](LICENSE).
