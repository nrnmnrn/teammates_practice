# 家紳 — Policies、Evaluator 與 Skill Library 手冊

## Role

- Branch：`feat/evaluator-library`
- Ownership：`src/arena/policies/`、`src/arena/evaluator/`、rollback、SkillStore。
- 依賴：Neo 的 contracts/sandbox API、于喬的 deterministic environment/replay；提供昊祁 EvaluationResult、SkillRecord、gate state。

## 賽前順序

1. 閱讀 `Specification.md` 的 Policy API、Evaluator、FSM、SkillStore、Test Specification。
2. 確認 contract gate 已合併並 rebase。
3. 先完成 deterministic built-in policies，再完成 evaluator hard gates。
4. 固定 replay artifact 格式後交給 Neo／昊祁。
5. 最後接 sandbox 與 Planner candidate，不讓 candidate 直接寫入 Skill Library。

## Task 1 — Built-in policy library

### Task

實作 FIFO/FCFS、SJF、Priority、Round Robin、EDF 與 Hybrid Priority-SJF，統一使用 allow-listed `SchedulerAPI`。

### Why

Planner 需要 initial skills，evaluator 需要穩定 baseline 與 deterministic fallback。

### Codex prompt

```text
Read Specification.md Policy API. Implement deterministic policy functions for
fifo, sjf, priority, round-robin, edf, and hybrid-priority-sjf. Policies may
only read the supplied SchedulerAPI snapshot and return a valid job_id. Use
stable tie-breaking based on policy key, arrival_time, and job_id. Add focused
pytest tests for ordering, empty queues, ties, VIP/deadline behavior, and no
mutation of input jobs.
```

### Allowed commands

```text
pytest tests/policies -q
python -m compileall src/arena/policies
```

### Expected files

- `src/arena/policies/`
- `tests/policies/`

### Verification

- Every policy returns queued job ID or explicit empty result。
- Same input snapshot yields same result。
- Input jobs are not mutated。

### Failure handling

不要調整 queue engine 來配合 policy；invalid/empty queue 使用 typed result。

### Handoff checklist

- Policy IDs and formulas。
- Unit-test output。
- SchedulerAPI usage notes。
- Hybrid fallback behavior。

## Task 2 — Metrics and evaluator

### Task

實作 throughput、P95 latency、deadline miss rate、value served、fairness、starvation penalty、aggregate score 與 hard-gate evaluator。

### Why

只有 evaluator 通過，candidate 才能成為 current policy；不能用 aggregate score 掩蓋 regression。

### Codex prompt

```text
Implement Evaluator.evaluate for the fixed 60-step Flash Sale scenario. Run the
same baseline and candidate across seeds 11, 23, and 37. Compute throughput,
P95 latency, deadline miss rate, value served, fairness, starvation penalty,
aggregate score, and independent hard-gate booleans. Accept only when P95
improvement >= 10%, throughput delta >= 0, deadline miss delta <= 0, and
fairness drop <= 0.05. Return a typed rejection reason otherwise.
```

### Allowed commands

```text
pytest tests/evaluator -q
python -m compileall src/arena/evaluator
```

### Expected files

- `src/arena/evaluator/`
- `tests/evaluator/`

### Verification

- Each gate has independent passing/failing tests。
- Baseline/candidate use identical seed scenarios。
- Aggregate result cannot hide a failed gate。

### Failure handling

Metric 缺資料回傳 typed error；不要把 missing value 當成零或自動通過。

### Handoff checklist

- `EvaluationResult` sample JSON。
- Gate calculation definitions。
- Replay commands/results。
- UI field mapping。

## Task 3 — Rollback and atomic deployment

### Task

建立 candidate evaluation、accept、rollback 與 current policy 更新的 atomic flow。

### Why

Candidate 未通過 hard gate 時，現行 policy、trace 與 Skill Library 不能被部分覆寫。

### Codex prompt

```text
Implement an atomic policy deployment boundary. A candidate becomes current
only after all evaluator gates pass for all required seeds. On rejection, retain
the previous policy, persist the rejection reason, and route to Reflection or
fallback. Add tests proving a failed candidate cannot change current policy or
write an accepted SkillRecord.
```

### Allowed commands

```text
pytest tests/evaluator/test_rollback.py -q
git diff --check
```

### Expected files

- `src/arena/evaluator/deployment.py`
- `tests/evaluator/test_rollback.py`

### Verification

- Failed candidate leaves current policy unchanged。
- Accepted candidate updates policy once。
- Rejection reason appears in result and trace。

### Failure handling

部署 transaction 中途失敗時回到 previous policy 並寫入 rollback trace，不刪除原有 artifacts。

### Handoff checklist

- Deploy/rollback API。
- Atomicity tests。
- FSM transitions expected by Neo。

## Task 4 — SkillStore and replay artifacts

### Task

實作 `SkillStore.save(record)`、`SkillStore.retrieve(state)` 與 runs/skills/replays artifact layout。

### Why

Self-evolving 的 MVP 定義是擴充 executable policy repertoire，下一次相同 regime 能 retrieval/reuse。

### Codex prompt

```text
Implement SkillStore using JSON artifacts. Accepted skills go to
artifacts/skills/<skill_id>.json, run traces to artifacts/runs/<run_id>.json,
and replay summaries to artifacts/replays/<run_id>.json. Retrieval must rank
event_signature and workload-feature matches. Never persist a skill before all
hard gates pass, and never skip evaluator validation when deploying a retrieved
skill. Add save/load/retrieve tests.
```

### Allowed commands

```text
pytest tests/evaluator/test_skill_store.py -q
Get-ChildItem artifacts -Recurse
```

### Expected files

- `src/arena/evaluator/skill_store.py`
- `tests/evaluator/test_skill_store.py`
- `artifacts/skills/`、`artifacts/runs/`、`artifacts/replays/`

### Verification

- JSON files pass Pydantic validation。
- Retrieval returns matching event signature first。
- Accepted/rejected persistence semantics are distinct。

### Failure handling

Artifact write failure 回傳 typed error 並保留 current policy；不要用 in-memory success 假裝 persistence 完成。

### Handoff checklist

- Artifact schema/sample。
- Retrieval ranking behavior。
- UI artifact paths。
- Replay seed summary。

## Hackathon 當天 checklist

- Built-in policies unit tests pass。
- Seeds 11/23/37 baseline replay pass。
- Each hard gate has visible result。
- Rollback test pass。
- SkillRecord only writes after acceptance。
- `artifacts/` paths writable and contain no secrets。
