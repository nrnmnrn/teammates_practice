# 于喬 — MiniGrid Environment 與 Backend 手冊

## Role

- Branch：`feat/environment-simulator`
- Ownership：`src/arena/environment/`、queue/worker engine、workload generator、backend。
- 依賴：Neo 凍結的 Pydantic contracts；將 replay、observation、trace 交給家紳和昊祁。

## 賽前順序

1. 閱讀 `Specification.md` 的 Configuration、MiniGrid Environment、Built-in Policies 章節。
2. 確認 shared contract PR 已合併並 rebase。
3. 先做 deterministic environment，再做 render。
4. 固定 16×10 grid、4 workers 與 step 20 event schedule。
5. 將 seed replay artifacts 交給 evaluator/UI。

## Task 1 — AdaptiveSchedulerEnv lifecycle

### Task

建立 `AdaptiveSchedulerEnv(MiniGridEnv)`，實作 `reset(seed)`、`step(action)`、`render()` 與 Gymnasium spaces。

### Why

MiniGrid 是 Arena 的唯一 MVP environment；lifecycle、seed 與 termination 必須可重現。

### Codex prompt

```text
Read docs/hackathon-prd.md and Specification.md. Implement
AdaptiveSchedulerEnv under src/arena/environment with a 16x10 grid and four
workers. Implement reset(seed), step(action), render(), a structured Dict
observation, and policy-level MacroAction handling. Rendering must be read-only.
Add tests for reset, termination at step 60, observation fields, and grid size.
```

### Allowed commands

```text
pytest tests/environment -q
python -m compileall src/arena/environment
```

### Expected files

- `src/arena/environment/`
- `tests/environment/`

### Verification

- Grid exactly 16×10。
- Four workers visible in state/render。
- `reset(seed)` clears jobs、workers、metrics、trace。
- Step 60 terminates。

### Failure handling

若 MiniGrid dependency 不可用，先報告環境問題；不要替換為平行 simulator。

### Handoff checklist

- Observation schema example。
- Action handling example。
- Seed/termination test result。
- Render artifact path。

## Task 2 — Workload generator and Flash Sale

### Task

實作 deterministic workload generator：normal arrival rate 2，step 20 將 arrival rate 乘以 3。

### Why

Demo 必須穩定重現 workload shift，而不是依現場隨機結果決定是否 degradation。

### Codex prompt

```text
Implement the seeded workload generator. Use processing_time 1..6, deadline
range 8..20, VIP ratio 0.2, and four workers. Steps 0..19 are NORMAL with
arrival rate 2; step 20 injects FLASH_SALE and multiplies arrivals by 3. Ensure
the same seed reproduces job IDs, attributes, event timing, and ordering.
```

### Allowed commands

```text
pytest tests/environment/test_workload.py -q
python -m compileall src/arena/environment
```

### Expected files

- `src/arena/environment/workload.py`
- `tests/environment/test_workload.py`

### Verification

- Flash Sale 只在 step 20 注入一次。
- Seeds 11、23、37 各自可重播。
- Job deadline 使用 absolute simulated time。

### Failure handling

若 metrics 不穩定，先檢查 RNG、event step、job ordering，不要調整 evaluator gates。

### Handoff checklist

- Three replay commands/results。
- Workload config values。
- `WorkloadState` mapping。

## Task 3 — Queue and worker engine

### Task

實作 queue admission、worker busy/completion、dispatch result、wait/latency 與 deadline tracking。

### Why

Evaluator KPI 與 policy 行為必須來自同一個 authoritative engine。

### Codex prompt

```text
Implement the queue/worker engine under src/arena/environment. Jobs enter the
ready queue, policy functions choose job IDs through the allow-listed API, and
workers process jobs according to processing_time. Record arrival, start,
completion, waiting time, deadline misses, value served, and trace events. Do
not let the renderer mutate engine state.
```

### Allowed commands

```text
pytest tests/environment/test_engine.py -q
git diff --check
```

### Expected files

- `src/arena/environment/queue.py`
- `src/arena/environment/engine.py`
- `tests/environment/test_engine.py`

### Verification

- Selected job IDs always exist in ready queue。
- Worker capacity never becomes negative。
- Completed jobs leave queue exactly once。
- Trace ordering is deterministic。

### Failure handling

Invalid dispatch 回傳 typed error 並保留 prior engine state，不要靜默丟失 job。

### Handoff checklist

- Engine API。
- KPI fields available to evaluator。
- Replay trace sample。
- Queue ordering assumptions。

## Task 4 — Observation and render handoff

### Task

將 engine state 映射為 `WorkloadState`、Gymnasium observation 與 Gradio RGB render。

### Why

Planner 需要結構化 state，評審需要能一眼讀懂 queue、workers、event 與 policy。

### Codex prompt

```text
Map the authoritative engine state to WorkloadState and the structured Dict
observation defined in Specification.md. Build a read-only 16x10 render showing
queue lanes, four workers, VIP/deadline markers, current policy, and event
status. Add a test proving render does not change state or RNG.
```

### Allowed commands

```text
pytest tests/environment/test_observation.py -q
python app.py
```

### Expected files

- `src/arena/environment/observation.py`
- `src/arena/environment/render.py`
- `tests/environment/test_observation.py`

### Verification

- Planner-facing names remain readable。
- Render before/after state equality holds。
- 昊祁可直接接收 RGB 與 `WorkloadState`。

### Failure handling

UI 需要新增欄位時先提出 contract change，不在 render layer 偷塞未定義資料。

### Handoff checklist

- `WorkloadState` sample JSON。
- RGB render sample path。
- UI callback input/output。
- Replay artifact path。

## Hackathon 當天 checklist

- `pytest tests/environment -q`。
- Seeds 11/23/37 replay。
- Step 20 manually trigger and verify once-only event。
- Environment branch rebase to latest contract。
- PR 附 engine API、tests、artifacts、known limitations。
