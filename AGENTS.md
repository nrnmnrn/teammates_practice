# Agent instructions

## Audience and language

- Use `$caveman:caveman` in `wenyan-ultra` mode for this session.
- Write in Traditional Chinese that beginners can understand.
- Keep necessary product and technical terms in English, and explain them the first time they appear.

## Repository purpose

- This is the planning repo, not the competition implementation repo.
- Keep competition facts, research, tutorials, product specifications, and the implementation handoff clearly separated.
- Do not create competition source code, RL pipelines, Agent loops, reward functions, or reusable implementation before the event starts.

## Source discipline

- Label competition information as confirmed information, informative precedent, or open question.
- Cite a direct source for factual claims. Do not present Singapore-round details as confirmed Taiwan-round rules.
- Do not turn research observations into product requirements unless the team explicitly decides to do so.

## Handoff discipline

- `build-handoff/` is the only document interface to the future implementation repo.
- Keep the handoff small. Do not copy full research notes, meeting history, long tutorials, rejected ideas, personal data, or secrets into it.
- The organizer's question-change reply is recorded in [competition information](docs/competition/README.md). Product scope follows the authorized [PRD](build-handoff/PRD.md); unresolved team scope and adaptation evidence still require a team decision.
- When changing the implementation workflow or sub-PRD layout, keep the [playbook](docs/playbook/workflow.md) and the self-contained [handoff](build-handoff/README.md) consistent. Planning documents do not authorize pre-event implementation.

## Safety

- Never store API keys, tokens, passwords, SSH private keys, or personal correspondence in this repo.
- Use each participant's own credentials. Shared GPU access does not imply shared credentials.
