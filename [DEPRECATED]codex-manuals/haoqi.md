# 昊祁 — Gradio UI 與 Demo Flow 手冊

## Role

- Branch：`feat/gradio-ui`
- Ownership：`src/arena/ui/`、Gradio layout、render、event controls、charts、demo flow。
- 依賴：Neo 的 `DemoState`、于喬的 RGB render/WorkloadState、家紳的 EvaluationResult/SkillRecord。

## 賽前順序

1. 閱讀 `Specification.md` 的 Gradio UI、Demo Runbook、DemoState sections。
2. 確認 contract gate 已合併並 rebase。
3. 先用 deterministic mock state 建立 layout，再接 live orchestration。
4. 保持四區資訊在單一畫面可讀。
5. 最後加入 retry、rollback、fallback 與 artifact links。

## Task 1 — Four-zone layout

### Task

建立 Gradio 四區：Environment、Policy、Metrics、Adaptation。

### Why

評審必須一眼理解 event、policy change、KPI movement 與 accept/rollback decision。

### Codex prompt

```text
Read Specification.md UI Contract and DemoState. Build a Gradio interface under
src/arena/ui with four visible zones: MiniGrid environment, policy/code diff,
live metrics, and adaptation/Skill Library. Start with deterministic mock
DemoState data so layout tests do not require an OpenAI API key.
```

### Allowed commands

```text
python app.py
pytest tests/ui/test_layout.py -q
git diff
```

### Expected files

- `src/arena/ui/layout.py`
- `src/arena/ui/state.py`
- `tests/ui/test_layout.py`

### Verification

- 四區在常見視窗尺寸內可讀。
- Current/candidate policy、code diff、rationale 同時可見。
- KPI 顯示 baseline comparison。

### Failure handling

UI layout 問題不得改動 shared contracts；缺資料時顯示 `N/A` 或提出 contract request。

### Handoff checklist

- Layout screenshot。
- `DemoState` fields consumed。
- Mock startup command。

## Task 2 — Environment controls and state binding

### Task

接 MiniGrid RGB render、Flash Sale button、queue/worker state 與 phase updates。

### Why

Demo 需要一個明確操作觸發 event，並即時看到狀態變化。

### Codex prompt

```text
Bind Gradio controls to AdaptiveSchedulerEnv and DemoState. Expose a Flash Sale
control, MiniGrid RGB render, queue length, worker state, current event, and
ArenaPhase. Do not run per-job LLM logic in the UI; send only documented
macro-actions to orchestration.
```

### Allowed commands

```text
python app.py
pytest tests/ui/test_controls.py -q
```

### Expected files

- `src/arena/ui/handlers.py`
- `tests/ui/test_controls.py`

### Verification

- Flash Sale control produces `FLASH_SALE` state。
- UI does not directly mutate job queue。
- Phase transitions are visible。

### Failure handling

Backend 尚未 ready 時使用 mock adapter 並標示 mock；不要假裝 live environment 已連線。

### Handoff checklist

- Handler input/output。
- Event control test。
- Backend dependency list。

## Task 3 — Metrics, diff, and adaptation panels

### Task

顯示 P95 latency、throughput、deadline miss、fairness、score、baseline comparison、code diff、reflection、hard-gate result 與 Skill Library。

### Why

UI 必須展示「為何 policy 改變」以及「改變是否通過 evaluator」。

### Codex prompt

```text
Implement read-only renderers for EvaluationResult, PolicyCandidate diff,
Reflection output, SkillRecord retrieval, gate results, rollback, and fallback.
Use green for healthy/recovered, yellow for pressure/evaluation, and red for
critical/rejected/fallback. Always show source phase and artifact path.
```

### Allowed commands

```text
pytest tests/ui/test_metrics.py -q
python app.py
```

### Expected files

- `src/arena/ui/metrics.py`
- `src/arena/ui/policy_diff.py`
- `src/arena/ui/adaptation.py`
- `tests/ui/test_metrics.py`

### Verification

- 四個 hard gates individually visible。
- Rejected candidate never appears as deployed。
- Skill reuse displays matching event signature and artifact。
- Fallback is explicit。

### Failure handling

缺失 metrics 顯示 unknown，不補造數值；artifact 讀取失敗顯示 error state。

### Handoff checklist

- UI field mapping。
- Color/state mapping。
- Screenshot or recorded smoke output。

## Task 4 — 90-second demo and smoke test

### Task

串成固定 runbook：0–20 baseline、20 Flash Sale、30 degradation、35 diagnosis、45 candidate、60 recovery、70 persistence、80–90 reuse。

### Why

Demo 必須讓 judge 看見可重現因果鏈，而不是只看到最後成功畫面。

### Codex prompt

```text
Create a deterministic UI smoke flow for the documented 90-second nominal demo.
At every phase show event, current policy, KPI movement, candidate result,
artifact, and next action. If Planner/Reflection retries, show a waiting state.
If all attempts fail, clearly label deterministic Hybrid Priority-SJF fallback.
```

### Allowed commands

```text
python app.py
pytest tests/ui -q
git diff --check
```

### Expected files

- `src/arena/ui/demo_flow.py`
- `tests/ui/test_demo_flow.py`

### Verification

- Fresh launch works with mock and live-config modes。
- 90-second nominal sequence reaches stored/reuse state。
- Fallback sequence remains understandable。

### Failure handling

LLM waiting 不應讓 UI 靜默切換 policy；retry、error、fallback 都要有狀態文字。

### Handoff checklist

- Demo recording or step-by-step screenshot。
- Required environment variables。
- Known timing/network risk。
- Neo integration request。

## Hackathon 當天 checklist

- `python app.py` clean launch。
- 四區 layout 可讀。
- Flash Sale control working。
- Mock demo rehearsal completed。
- Live API failure fallback visible。
- 最後一次整合後跑 `pytest tests/ui -q`。
