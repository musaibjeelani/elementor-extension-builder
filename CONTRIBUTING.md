# Contributing

Contributions should make the skill more reliable across existing Elementor websites without embedding one client's design, content, naming, credentials, or environment details.

## Development process

1. Fork the repository and create a short-lived branch.
2. Update `skills/elementor-extension-builder/SKILL.md` only for instructions that should apply whenever the skill is invoked.
3. Put conditional or detailed guidance in `skills/elementor-extension-builder/references/` and link it from `SKILL.md`.
4. Keep examples generic and remove private URLs, paths, assets, IDs, and credentials.
5. Validate the skill using `quick_validate.py` and validate the Claude package using `claude plugin validate --strict .` as described in the README.
6. Test it in a new Codex chat and a clean Claude Code plugin session with at least one realistic existing-site request.
7. Update `CHANGELOG.md` when behavior or public guidance changes.
8. Open a focused pull request explaining the user-visible improvement and how it was tested.

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

## Pull request checklist

- [ ] The skill validator passes.
- [ ] The Claude plugin validator passes in strict mode.
- [ ] No client-specific names, designs, URLs, credentials, or IDs are included.
- [ ] New references are linked from `SKILL.md` where relevant.
- [ ] A realistic invocation was tested.
- [ ] The changelog reflects user-visible changes.
