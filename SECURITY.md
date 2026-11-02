# Security policy

## Supported versions

Security fixes are applied to the latest released version of the skill.

## Reporting a vulnerability

Do not publish credentials, private client files, exploitable target details, or a working proof of concept in a public issue.

Use GitHub's private vulnerability reporting feature when enabled. Otherwise, contact the repository owner privately and include:

- the affected skill version;
- the risky instruction or generated-code pattern;
- a minimal generic reproduction;
- the expected safe behavior;
- any suggested mitigation.

This skill generates project-specific code, so users must review and test generated plugins before production deployment. Never place live credentials, database dumps, private keys, or production secrets in a Codex prompt, generated plugin, test fixture, or public issue.
