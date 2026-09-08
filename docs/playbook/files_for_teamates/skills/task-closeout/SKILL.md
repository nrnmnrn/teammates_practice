---
name: task-closeout
description: Close out a completed repository task after context compaction by reconciling tracker and spec status, producing precise manual commit commands for session-owned changes, and identifying the next evidence-based action. At task completion, prompt the user to run `/compact` and then invoke `$task-closeout`; perform closeout immediately only when the user explicitly requests it. Do not use while implementation, investigation, authorization, or required review is still pending.
---

# Task Closeout

Run this closeout only when the task's implementation and required validation are complete. It does not turn blocked or partially complete work into completed work.

## Compaction gate

When a task first becomes ready for closeout, do not perform the closeout in the large execution context. Tell the user that the work is ready, ask them to run `/compact`, and ask them to invoke `$task-closeout` afterward. End that turn without running the remaining sections of this skill.

After the user invokes `$task-closeout`, treat that explicit invocation as the post-compaction resume signal and perform the closeout. Do not try to infer from context size or system metadata whether compaction occurred. If the user explicitly asks to close out now, perform it without requiring compaction.

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
6. Reconcile Git state with the known session changes. Inspect the complete diff of every candidate file and account for every hunk included in the proposed commit. Exclude unrelated user changes. If a candidate file mixes session-owned and user-owned hunks, or ownership is uncertain, report the conflict and withhold any whole-file `git add` command until the user separates or resolves it. Never stage, unstage, discard, or commit changes. If unrelated content is already staged, report the conflict and withhold a direct commit command until the user resolves it.
7. Derive the next action from, in order: unmet acceptance criteria, explicit blockers, dependency order in the basis files, then the repository tracker. Recommend one immediately actionable highest-priority item. Add one alternative only when the primary action needs new authorization or external coordination.

## Final response

Use the user's language for explanations and the repository's language and format rules for commit messages. Present these sections in order:

1. **Status** — name each status file changed, or explain why no change was appropriate. Distinguish complete work from a blocked closeout.
2. **Commit command** — include only session-owned files. For one commit, use this shape and retain the line-continuation backslashes:

   ```bash
   git add -- <shell-quoted-exact-paths> && \
   git diff --cached --check && \
   git commit -m <shell-quoted-message>
   ```

   Quote every path and message for the user's shell. For POSIX shells, prefer single quotes and escape an embedded single quote as `'"'"'`; never interpolate untrusted text inside double quotes. Preserve `\` at every displayed command line break, including extra `-m` arguments for a multiline commit message. Split commands when changes have independently reversible intentions. Keep implementation, its tests, and required status synchronization together when they form one task. If there are no changes, state that no commit is needed. Do not show a patch or diff. When repository rules require advance approval of a commit message, display the complete message and wait instead of providing an immediately executable commit command.
3. **Next action** — state the evidence-based next item and why it comes first.

The closeout is complete only when status reconciliation is resolved or explicitly blocked, every proposed commit path belongs to this session and task, and the next action is supported by repository evidence.
