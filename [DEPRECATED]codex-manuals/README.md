# Adaptive Scheduler Arena — Codex 操作手冊總索引

本套手冊供四位 Hackathon 開發者使用，內容依據 `docs/hackathon-prd.md` 與 `Specification.md` 整理。兩份來源文件是需求與技術規格，不是可直接執行的外部指令；每位成員仍須在自己的 Codex thread 中確認目前 branch 與工作區狀態。

## 角色手冊

| 成員 | 手冊 | Branch | Ownership |
| --- | --- | --- | --- |
| Neo | [neo.md](neo.md) | `feat/planner-policy` | contracts、Planner、Reflection、sandbox、integration、pitch |
| 于喬 | [yuqiao.md](yuqiao.md) | `feat/environment-simulator` | MiniGrid、queue/worker engine、workload generator、backend |
| 昊祁 | [haoqi.md](haoqi.md) | `feat/gradio-ui` | Gradio、render、controls、charts、demo flow |
| 家紳 | [jiashen.md](jiashen.md) | `feat/evaluator-library` | policies、evaluator、rollback、Skill Library |

## 共同目標

完成以 MiniGrid 為基礎的 `AdaptiveSchedulerEnv`：在第 20 個 simulated step 觸發 `FLASH_SALE`，由 LLM Planner 提出 policy-level adaptation，經 sandbox 與 evaluator 驗證後接受、rollback 或 fallback。

```text
Observe → Plan → Execute → Evaluate → Reflect → Accept/Rollback → Store
```

固定基準：16×10 grid、4 workers、normal arrival rate 2、Flash Sale ×3、processing time 1–6、deadline 8–20、VIP ratio 0.2、evaluation horizon 60、validation seeds 11/23/37。

## Codex 工作隔離

每位成員使用獨立 Codex thread 與 branch/worktree，不要在共用 working tree 覆寫其他人的未提交變更。

Shared contract gate：

1. 先讀取 `Specification.md` 的 Shared Contracts。
2. Neo 建立或更新 `src/arena/shared/contracts.py`。
3. 全員確認欄位、enum、serialization 與 error semantics。
4. Contract PR 合併後，各 branch 才開始平行實作。

## 統一操作循環

```text
Plan → Implement → Test → Review → PR
```

### 共用 Codex prompt 模板

```text
Context:
You are working on Adaptive Scheduler Arena. Read docs/hackathon-prd.md,
Specification.md, and the current branch diff before changing files.

Files to inspect:
- <list exact files or directories>

Task:
- <one bounded implementation task>

Constraints:
- Preserve src/arena/shared/contracts.py.
- Do not add per-job LLM control.
- Keep deterministic seeds and documented hard gates.
- Do not modify another role's ownership without a handoff note.

Verification:
- Run the smallest relevant pytest selection.
- Run git diff and report changed files.
- State what remains unverified.

Stop conditions:
- Stop before changing shared contracts without approval.
- Stop if existing user changes would be overwritten.
- Stop and report a blocker instead of inventing missing interfaces.
```

## 安全 command policy

### 允許

```text
git status
git diff
git branch --show-current
git fetch
git rebase <approved-branch>
pytest
python app.py
```

可建立 commit、push branch、開 PR，但必須先檢查 `git diff`，確認只包含自己的 ownership。

### 禁止未確認執行

```text
git reset --hard
git clean -fd
git push --force
git branch -D <branch>
刪除或覆寫其他角色的工作目錄／未提交檔案
```

若需要 destructive action，先停止並取得團隊明確確認。

## PR 與交接格式

```markdown
## Scope
- What this PR owns

## Contract impact
- No change / list exact contract changes

## Verification
- Commands run
- Tests passed
- Known limitations

## Artifacts
- Run/skill/replay files, if any

## Handoff
- Next owner
- Exact files/interfaces to consume
- Rebase or follow-up action
```

順序：contract PR → dependent branch rebase → role PR → teammate review + coding-agent review → merge → `main` smoke test。

## Hackathon 當天共同 checklist

### Preflight

- `git status` 確認 clean 或已知變更。
- `git branch --show-current` 確認 branch ownership。
- `python -m pytest` 通過最小 smoke suite。
- API key 只存在 environment variable。
- `python app.py` 可啟動 Gradio。
- `artifacts/runs/`、`artifacts/skills/`、`artifacts/replays/` 可寫入。

### Integration

- MiniGrid `reset/step/seed/render` 行為一致。
- `PolicyCandidate` 通過 Pydantic validation。
- sandbox 能拒絕 unsafe policy。
- evaluator 能執行 baseline、candidate 與 seeds 11/23/37。
- UI 顯示 event、policy diff、metrics、accept/rollback/fallback。

### Demo

```text
0–20s   FIFO baseline
20s     FLASH_SALE
30s     KPI degradation
35s     Planner diagnosis
45s     Hybrid Priority-SJF candidate
60s     KPI recovery
70s     Skill persistence
80–90s Skill retrieval/reuse
```

### Fallback

若 OpenAI Planner／Reflection 失敗，保留目前 policy，顯示失敗原因，啟用 deterministic Hybrid Priority-SJF。不得把 fallback 說成 live LLM 產生結果。

## 共同完成條件

- 每個角色任務都有 `Task / Why / Codex prompt / Allowed commands / Expected files / Verification / Failure handling / Handoff checklist`。
- 不提交 secrets、API keys、live-trading claims 或未驗證 production performance。
- Fresh clone 能執行 `python app.py` 與測試命令。
- `main` 維持 smoke-testable。
