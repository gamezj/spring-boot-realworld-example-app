# AGENTS.md — agent conventions for this repo

- PR titles MUST start with the Jira issue key: `PDLC-123: short description` (D5).
  Branch names are never checked; vendor prefixes like `copilot/*` are accepted as-is.
- Self-review your diff and post evidence (test output, screenshots) on the PR BEFORE
  requesting human review; the critic gate must show `ADVERSARIAL-REVIEW: PASS`.
- Never touch CI files, deployment config, or anything under `.github/` unless the work
  item's risk tier is R3 and a human approved the plan (D8).
- Full program conventions are managed centrally (Claude plugin / Cursor Team Rules /
  Copilot org instructions) — this file carries only repo-specific facts.

## Repo-specific facts

<!-- build/test commands, module layout, deploy targets — keep this current -->
