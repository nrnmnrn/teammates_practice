---
name: task-closeout
description: Close out a completed repository task by reconciling tracker and spec status, verifying session-owned Git changes, requesting explicit commit approval, and identifying the next evidence-based action. Use when implementation and required validation are complete; do not use while required work, authorization, or review remains pending.
---

# Task Closeout

Run this closeout only when the task's implementation and required validation are complete. It does not turn blocked or partially complete work into completed work.

## Inputs

Establish these from the current session before acting:

```text
task:
basis_files:
completion_evidence:
session_changed_files:
```

`basis_files` may be empty. Treat `session_changed_files` as candidates until Git confirms them.

## Closeout

1. Read the repository's applicable agent instructions and document-authority rules. Obey their learning preflights, ownership boundaries, review requirements, and Git conventions.
2. Identify the task's basis files from explicit paths in the request or session. If none were explicit, inspect the files referenced during the task and the session-owned changes. If no basis file can be identified, report that fact and skip status edits.
3. Reconcile completion evidence against every explicit acceptance condition in the basis files. Follow the repository's authority order. If no order exists and documents conflict, preserve the more conservative status and name the conflict.
4. Update tracker or spec status directly only when the evidence satisfies explicit criteria and the update does not create a new decision, verdict, scope, or unsupported claim. When such judgment or missing authority is required, leave the status unchanged and report the blocker.
5. Validate any status edit with checks proportional to its format and repository rules. A task requiring independent acceptance is not ready for closeout until that acceptance exists.
6. Reconcile Git state with the known session changes. Inspect the complete diff of every candidate file, account for every hunk, and inspect staged changes for conflicts. Keep only session-owned files in the proposed commit. If a candidate file mixes session-owned and user-owned hunks, or ownership is uncertain, report the conflict and leave it for the user to separate or resolve. If unrelated content is already staged, report the conflict and withhold commit approval until the user resolves it. In this closeout turn, preserve the current Git state: do not stage, unstage, discard, or commit.
7. Derive the next action from, in order: unmet acceptance criteria, explicit blockers, dependency order in the basis files, then the repository tracker. Recommend one immediately actionable highest-priority item. Add one alternative only when the primary action needs new authorization or external coordination.

## Final response

Use the user's language for explanations and the repository's language and format rules for commit messages. Present these sections in order:

1. **Status** — name each status file changed, or explain why no change was appropriate. Distinguish complete work from a blocked closeout.
2. **Next action** — state the evidence-based next item and why it comes first.
3. **Commit info** — if session-owned changes exist and Git has no ownership or staged-change conflict, list only the exact session-owned files waiting for `git add` and display the complete recommended commit message. Ask the user to reply `Y` to authorize the next turn to run `git add`, `git diff --cached --check`, and `git commit`, or `N` to leave Git unchanged. If there are no session-owned changes, state that no commit is needed and request no reply. If ownership or staged-change conflicts exist, name the conflict and request no approval until it is resolved. Do not show a patch, a commit command, or a partial commit plan.

After a `Y` reply in a later turn, recheck the proposed files and Git state, then run `git add` for only those files, `git diff --cached --check`, and `git commit` with the approved complete message. After an `N` reply, leave Git unchanged.

The closeout is complete when status reconciliation is resolved or explicitly blocked, session-owned files and staged conflicts are verified, the next action is supported by repository evidence, and either no commit is needed or the user has been asked for `Y` or `N` approval.
