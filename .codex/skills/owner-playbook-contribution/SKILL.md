---
name: owner-playbook-contribution
description: Verify an owner's Codex practice or skill-routing guidance before adding it to this planning repo's docs/playbook. Use when maintaining team playbook guidance, not for ordinary coding or product decisions.
---

# Owner Playbook Contribution

Turn an owner's practical experience into a safe, reusable playbook entry without presenting personal preference, a past outcome, or another event's rules as universal fact.

## Inputs and modes

Collect the available facts: the situation, the owner's approach, the observed result, relevant tool or skill version, and any failure cases or limits. Never request or record credentials, tokens, passwords, private correspondence, or private chain-of-thought.

Use **review mode** by default: show the proposed entry and wait for the owner to say it is approved before editing files. Use **direct-write mode** only when the owner explicitly authorizes writing verified guidance in the same request.

## Verify before drafting

1. Read the repository's `AGENTS.md`, `CONTEXT.md`, and the relevant `docs/playbook/` files. Respect their source, product-decision, and safety rules.
2. If the contribution recommends a skill, read that skill's complete `SKILL.md`. Read authoritative documentation when the claim depends on a current or technical fact.
3. Check for conflict with existing playbook guidance. Preserve the owner's experience as evidence, but distinguish it from documented behavior.
4. Identify the narrowest appropriate destination:
   - Codex workflow, prompting, review, or recovery practice → `docs/playbook/codex-practices.md`.
   - Choosing, avoiding, or sequencing a skill → `docs/playbook/skill-routing.md`.
   - Setup, standard workflow, or troubleshooting only when it directly changes that document's purpose.

## Draft and write

For each entry, state:

- **適用情境** (when it helps)
- **做法** (short, repeatable steps)
- **限制／不要使用的情況**
- **驗證方式** (what a teammate can observe)
- **狀態**: `已核可操作指引` only when the guidance is consistent and sufficiently supported; otherwise `待驗證` with the unresolved question.

Keep product requirements, competition rules, and historical-case claims out of the playbook. Link or cite a direct source for factual claims, and label other competitions as `參考先例`.

In review mode, report the target file, verification result, and draft; do not edit. In direct-write mode, make the smallest focused edit with `apply_patch`, update the playbook index if a new document is created, then report the files changed and any remaining uncertainty.
