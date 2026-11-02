# Contributing

Contributions should make the skill more reliable across existing Elementor websites without embedding one client's design, content, naming, credentials, or environment details.

## Before you start

1. Open an issue for substantial workflow or architecture changes.
2. Fork the repository and create a short-lived branch from `main`.
3. Keep each pull request focused on one logical improvement.
4. Do not include generated client plugins, private designs, credentials, production URLs, or environment-specific IDs.

## Development process

1. Update `skills/elementor-extension-builder/SKILL.md` only for instructions that should apply whenever the skill is invoked.
2. Put conditional or detailed guidance in `skills/elementor-extension-builder/references/` and link it from `SKILL.md`.
3. Keep examples generic and remove private URLs, paths, assets, IDs, and credentials.
4. Run the available maintainer validations described below.
5. Test it in a new Codex chat and a clean Claude Code plugin session with at least one realistic existing-site request.
6. Update `CHANGELOG.md` when behavior or public guidance changes.
7. Open a focused pull request explaining the user-visible improvement and how it was tested.

## Maintainer validation

These checks are for contributors changing this repository. End users do not need them to install or invoke the skill.

### Codex skill validation

Codex installations that include the built-in `skill-creator` skill also include `quick_validate.py`. The script checks skill metadata, naming, and unfinished scaffold placeholders; it is not a runtime dependency of Elementor Extension Builder.

Windows PowerShell:

```powershell
python "$HOME\.codex\skills\.system\skill-creator\scripts\quick_validate.py" `
  ".\skills\elementor-extension-builder"
```

macOS or Linux:

```bash
python ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py \
  ./skills/elementor-extension-builder
```

If the validator reports a missing `yaml` module, install its Python dependency:

```bash
python -m pip install PyYAML
```

If the local Codex installation does not provide `skill-creator`, skip this host-specific check and review the `SKILL.md` frontmatter manually.

### Claude plugin validation

With Claude Code installed:

```bash
claude plugin validate . --strict
```

Then load the checkout in a clean session:

```bash
claude --plugin-dir .
```

## Quality requirements

- Preserve existing themes, builders, content, and unrelated plugins by default.
- Avoid hardcoded IDs, domains, filesystem paths, and environment-specific assumptions.
- Keep generated code compatible with WordPress escaping, sanitization, capabilities, and nonces.
- Keep CSS and JavaScript scoped to plugin-owned component roots.
- Treat Elementor editor behavior, responsive layouts, accessibility, WPML, upgrades, and duplicate prevention as testable concerns.
- Do not add generic advice that Codex already knows unless it prevents a demonstrated failure.

## Commit style

Use concise conventional subjects, for example:

```text
docs: clarify direct-clone installation
fix: preserve renamed seeded templates during upgrades
feat: add multilingual link resolution guidance
```

Write commit messages and pull request descriptions in English.

## Pull request checklist

- [ ] The Codex skill validator passes when it is available.
- [ ] The Claude plugin validator passes in strict mode when Claude Code is available.
- [ ] No client-specific names, designs, URLs, credentials, or IDs are included.
- [ ] New references are linked from `SKILL.md` where relevant.
- [ ] A realistic invocation was tested.
- [ ] The changelog reflects user-visible changes.

## Release process

1. Confirm the working tree is clean and `main` is up to date.
2. Update versions in both plugin manifests when the release version changes.
3. Add a human-readable entry to `CHANGELOG.md`.
4. Run the available validations and test installation in clean Codex and Claude Code sessions.
5. Create and push an annotated semantic-version tag, for example:

```bash
git tag -a v1.1.0 -m "Release 1.1.0"
git push origin v1.1.0
```

6. Create a GitHub Release from the tag and summarize user-visible changes.
