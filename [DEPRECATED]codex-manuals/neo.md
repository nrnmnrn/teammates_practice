# Neo — Planner、Contracts、Sandbox 與 Integration 手冊

## Role

- Branch：`feat/planner-policy`
- Ownership：`src/arena/shared/`、`src/arena/planner/`、`src/arena/sandbox/`、integration、pitch。
- 依賴：于喬的 `WorkloadState`、家紳的 evaluator、昊祁的 `DemoState`。

## 賽前順序

1. 讀取 `docs/hackathon-prd.md` 與 `Specification.md` 的 Shared Contracts、Planner、Sandbox、FSM 章節。
2. 執行 `git status`、`git branch --show-current`、`git diff`。
3. 凍結 Pydantic models，先建立 contract PR。
4. Contract merge 後通知其他三人 rebase。
5. 依序完成 Planner adapter、sandbox、FSM integration、fallback。

## Task 1 — Shared contracts

### Task

在 `src/arena/shared/contracts.py` 實作 `Job`、`WorkloadState`、`MacroAction`、`SchedulingDecision`、`PolicyCandidate`、`ExecutionResult`、`EvaluationResult`、`SkillRecord`、`DemoState` 與 enums。

### Why

所有角色的資料交換必須有單一 schema source of truth。

### Codex prompt

```text
Read docs/hackathon-prd.md and Specification.md, especially Shared Contracts.
Implement the Pydantic models in src/arena/shared/contracts.py exactly from the
documented fields and validation rules. Add focused pytest tests for required
fields, enum values, numeric bounds, and JSON round-trips. Do not change the
public contract without stopping and reporting it.
```

### Allowed commands

```text
git status
git diff
pytest tests/shared -q
```

### Expected files

- `src/arena/shared/contracts.py`
- `tests/shared/`

### Verification

- Invalid `PolicyCandidate` JSON is rejected。
- Hard-gate fields cannot be silently removed。
- JSON round-trip preserves enums and numeric values。

### Failure handling

需要新增欄位時，在 PR description 列出 contract impact；不要直接改 schema 配合單一模組。

### Handoff checklist

- Contract PR URL。
- 欄位與 validation summary。
- Tests command 與結果。
- 通知其他三人 rebase。

## Task 2 — Planner and Reflection

### Task

實作 `Planner.plan(state, skills)` 與 `Reflection.revise(candidate, result)`。

### Why

LLM 只能做 policy-level diagnosis 與 candidate generation，不得逐 job 控制或更改 evaluator 規則。

### Codex prompt

```text
Implement the Planner and Reflection adapters under src/arena/planner.
Return only Pydantic-validated PolicyCandidate values. Enforce at most three
attempts per phase and a 15-second timeout per OpenAI call. Reject malformed
JSON, unsupported skills, unsafe policy instructions, per-job control, and any
attempt to change hard gates. Add a deterministic mock adapter for tests.
```

### Allowed commands

```text
pytest tests/planner -q
python -m compileall src/arena/planner
```

### Expected files

- `src/arena/planner/`
- `tests/planner/`

### Verification

- Valid response becomes `PolicyCandidate`。
- Malformed response retries with format correction。
- Exhausted attempts produce a typed failure for FSM fallback。

### Failure handling

API timeout、invalid JSON 或 schema error 必須回傳 typed error，交由 FSM 決定 retry 或 fallback。

### Handoff checklist

- Planner input/output example。
- Mock adapter usage。
- Timeout/retry behavior。
- Neo-to-Haoqi `DemoState` fields。

## Task 3 — Sandbox and fallback

### Task

建立 AST allowlist、isolated subprocess、timeout/memory/dispatch limits，以及 deterministic Hybrid Priority-SJF fallback。

### Why

Code-as-Policy 必須可驗證、可 rollback，不能取得 host filesystem、network 或 credentials。

### Codex prompt

```text
Implement the policy sandbox under src/arena/sandbox. Parse policy_code with
AST, reject imports, dynamic execution, filesystem/network/subprocess/system
access, unbounded loops, and global mutation. Execute only the allow-listed
SchedulerAPI in an isolated subprocess with bounded resources. Add tests for
safe code, every unsafe category, timeout, invalid job_id, and deterministic
fallback to hybrid-priority-sjf.
```

### Allowed commands

```text
pytest tests/sandbox -q
python -m compileall src/arena/sandbox
```

### Expected files

- `src/arena/sandbox/`
- `src/arena/policies/hybrid_priority_sjf.py`
- `tests/sandbox/`

### Verification

- Unsafe code is rejected before subprocess execution。
- Timeout produces `ExecutionError`。
- Fallback produces valid decisions and trace metadata。

### Failure handling

不要為了讓 candidate 通過而放寬 allowlist；不安全 policy 必須 rejection + fallback。

### Handoff checklist

- Sandbox API。
- Error codes。
- Fallback policy ID。
- UI 要顯示的 fallback fields。

## Task 4 — FSM and integration

### Task

將 `NORMAL → DEGRADED → PLANNING → EXECUTING → EVALUATING → RECOVERED/STORED` 與 failure paths 接起來。

### Why

UI、Planner、evaluator 與 artifacts 需要同一個可觀察 lifecycle。

### Codex prompt

```text
Implement the orchestration state machine using ArenaPhase. Every transition
must emit a timestamped trace event with correlation_id, previous phase, next
phase, reason, and optional error. Wire MiniGrid state, Planner, sandbox,
Evaluator, Reflection, SkillStore, and deterministic fallback without allowing
a candidate to bypass evaluation.
```

### Allowed commands

```text
pytest tests/integration -q
python app.py
git diff --check
```

### Expected files

- `src/arena/` orchestration module
- `app.py` wiring only
- `tests/integration/`

### Verification

- Mock run reaches `STORED` after acceptance。
- Rejected candidate reaches Reflection or fallback。
- `python app.py` starts Gradio。

### Failure handling

若 integration 需要修改他人 contract，停止並建立 contract change note。

### Handoff checklist

- End-to-end test command。
- FSM transition diagram。
- Known live API limitations。
- Demo fallback rehearsal result。

## Judge-facing demo checklist

- Explain that the LLM chooses a policy strategy, not individual jobs。
- Show Flash Sale causing KPI degradation。
- Show candidate rationale and code diff。
- Show hard-gate result before deployment。
- Show Skill Library persistence/reuse。
- If fallback is active, label it explicitly。
