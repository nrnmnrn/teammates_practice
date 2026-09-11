# Hackathon Shared Contract 討論稿

以下內容可以直接轉貼給組員的 AI，作為 Hackathon shared contract 的討論稿。

## 一、專有名詞定義

### Policy

Policy 是 worker 選擇下一筆 job 的排程規則，例如 FIFO、SJF、Priority、EDF，以及由多個規則組合而成的 Hybrid。Hybrid 的演算法函式可以預先存在於程式中，但它不是初始 Skill。

- FIFO：最早到達的 job 優先
- SJF：處理時間最短的 job 優先
- Priority：優先度最高的 job 優先
- EDF：deadline 最早的 job 優先

### Skill

Skill 是已經登錄在 Skill Library、可以被系統直接使用的 policy。每次 Reset 的初始 Skill 只有 FIFO、SJF、Priority、EDF；Hybrid 必須通過 evaluator 後才會成為 Skill。

Skill 應包含：

- `skill_id`
- 名稱
- 排程規則
- 可執行的 policy code
- 來源
- 建立時間
- 使用紀錄
- 是否經過 evaluator 驗證

### Candidate

Candidate 是 Planner 提出的待評估 policy 候選，尚未成為正式 Skill。

Candidate 可能包含候選名稱、policy code、產生原因、適用 workload、預期改善方向、`candidate_id`、版本、evaluator 結果，以及 evaluator 的通過或拒絕狀態。

Candidate 通過 evaluator 後才會轉成 Skill；未通過的 Candidate 不得被當成正式可用 policy。

### Run

Run 是從一次 Reset 開始，到下一次 Reset 結束的一局模擬。每個 Run 都有獨立的模擬時間、jobs、metrics、policy switching 紀錄、candidate 狀態與 Skill Library 狀態。

### Policy segment

Policy segment 是某一個 policy 連續生效的時間區段。例如：

```text
segment-1：FIFO，t=0～12
segment-2：Priority，t=12～25
segment-3：Hybrid，t=25～40
```

## 二、對各項問題的決定

### 1. Reset 後是否可以立即有 running job 與新事件？

可以。Reset 的行為是：

1. 清除上一局的 jobs、歷史、metrics 與事件。
2. 建立新的 `run_id`。
3. 恢復固定 seed。
4. 載入初始 jobs。
5. 恢復預設 policy，例如 FIFO。
6. 恢復初始 Skill Library。
7. 產生初始 arrival 事件。

Reset 後不必強制進入 idle。只要初始 job 已經到達且 worker 可以派工，就可以立即出現 running job、dispatch event 與 worker busy。但 Reset 不應自動推進模擬時間；時間應從新的 Run 的 `t=0` 開始，等待使用者按播放或單步。

### 2. Candidate 通過 evaluator 後是否自動切換？

Candidate 通過 evaluator 後自動註冊並切換到新 Skill：

```text
Planner propose candidate
→ Evaluator 在 sandbox evaluate
→ 通過 gate
→ Hybrid 加入 Skill Library
→ Agent 自動切換 current policy
→ 下一次 dispatch 使用 Hybrid
```

如果當下已有 job 正在處理，不取消、不重新派工，也不中斷 worker。該 job 完成後，下一次 dispatch 才使用 Hybrid。UI 應顯示「Evaluator 通過 Hybrid」、「目前策略：Hybrid」及「生效時機：下一次派工」。

### 3. Hybrid 通過 evaluator 後是否禁止再次提出候選？

不禁止。必須區分 policy switching 與 candidate proposal。

Hybrid 通過 evaluator 並登錄後，Developer mode 仍可自由切換所有已存在的 Skill；Demo mode 則由 Agent 自動切換：

```text
FIFO ↔ SJF ↔ Priority ↔ EDF ↔ Hybrid
```

切換 policy 不需要 Reset。之後也可以再次提出新的 Candidate。每次 Candidate 都必須使用新的 `candidate_id` 或版本，例如 `hybrid-v1`、`hybrid-v2`、`hybrid-v3`。

如果新 Candidate 與既有 Skill 完全相同，可以由 evaluator 判定為重複並拒絕登錄；但不能因為 Hybrid 已通過並登錄，就禁止未來提出新的版本。

Reset 的用途只有開始新的 Run、清除本局狀態、清除本局 metrics、清除本局歷史，以及恢復初始 Skill Library。Reset 不是解除 policy switching 或 candidate proposal 限制的必要條件。

### 4. 切換 policy 後，metrics 如何保留？

Metrics 同時保留三個層次。

#### A. 整局混合 metrics

從 Run 開始累計，跨越所有 policy：總完成數、總逾期數、整局吞吐量與整局 P95 完成延遲。這反映整場 workload 的真實結果。

#### B. Policy segment metrics

每次切換 policy 時建立新的 segment。每個 segment 記錄 `segment_id`、`policy_id`、開始與結束模擬時間、完成數、逾期數、吞吐量、完成延遲資料及 policy 切換原因。

| Segment | Policy | 時間 | 完成 | 逾期 |
|---|---|---:|---:|---:|
| segment-1 | FIFO | 0–12 | 3 | 1 |
| segment-2 | Priority | 12–25 | 4 | 0 |
| segment-3 | Hybrid | 25–40 | 5 | 2 |

#### C. Job-level attribution

每筆 job 應記錄它是在什麼 policy 下被派工：

```json
{
  "job_id": "job-08",
  "dispatch_policy": "priority",
  "dispatch_segment_id": "segment-2",
  "completion_time": 18.0
}
```

Metrics 應以派工當下的 policy 歸屬。若 t=10 由 FIFO 派工 job-03、t=11 切換成 Priority、t=12 job-03 完成，job-03 仍歸入 FIFO 統計。

切換 policy 不應清除既有 metrics。UI 應同時顯示「整局結果：包含 FIFO、Priority、Hybrid」與「目前區段：Hybrid，t=25～40」。單一 Run 的不同 segment 可能面對不同 workload，因此不能直接宣稱某個 policy 一定比較好。若要公平比較，應使用相同 seed、相同 workload，建立獨立 Run 交給 evaluator 比較。

### 5. 再次提出候選時是否要新的 ID？

要。每次提出都建立不可重複的 `candidate_id`，並保留版本與來源：

```json
{
  "candidate_id": "hybrid-v2",
  "parent_skill_id": "hybrid-v1",
  "created_at": 32.0,
  "reason": "Flash Sale workload",
  "status": "proposed"
}
```

Candidate 狀態可包含 `proposed`、`evaluating`、`passed`、`rejected` 與 `superseded`。通過 Candidate 後可以新增 Skill；未通過 Candidate 則不改變目前 policy，也不新增 Skill。

Candidate evaluator 結果與 policy switching 是兩個獨立概念。通過 Candidate 會自動啟用該 Skill，但之後仍可任意切換其他已驗證 Skill；Demo 中由 Agent 執行切換，User 不介入。

## 三、建議的最小 shared contract

### Job

```json
{
  "job_id": "job-08",
  "arrival": 3.0,
  "processing_time": 2.0,
  "priority": 3,
  "deadline": 8.0,
  "status": "waiting",
  "start_time": null,
  "completion_time": null,
  "dispatch_policy": null,
  "dispatch_segment_id": null
}
```

### Skill

```json
{
  "skill_id": "fifo",
  "name": "FIFO",
  "rule": "arrival_time ASC, job_id ASC",
  "source": "builtin",
  "verified": true,
  "usage_count": 4
}
```

### Candidate

```json
{
  "candidate_id": "hybrid-v2",
  "name": "Hybrid",
  "code": "...",
  "reason": "Flash Sale workload",
  "status": "proposed",
  "evaluator_result": null
}
```

### Event

```json
{
  "event_id": "event-19",
  "time": 12.0,
  "type": "policy_switch",
  "job_id": null,
  "policy_id": "priority",
  "reason": "user_selected"
}
```

### Snapshot

```json
{
  "run_id": "run-03",
  "snapshot_version": 27,
  "time": 12.0,
  "current_policy": "priority",
  "jobs": [],
  "worker": {},
  "metrics": {},
  "policy_segments": [],
  "events": []
}
```

## 四、最後共識

## 五、最新 adaptation 共識：優先重用既有 Skill

正式 Demo 中 User 不介入 policy switching，也不按 Candidate accept。指標惡化後，Agent 必須先讓 Evaluator 在相同 seed、workload、初始狀態與 evaluation window 下測試 Skill Library 內所有已驗證 Skills。

- 目前 Skill 已能解決問題：維持目前 policy，不提出新的 Candidate。
- 其他既有 Skill 通過：依 expired、completed、P95、目前 policy、skill ID 的固定順序選出最佳 Skill，記錄 `existing_skill_reused`，並於下一次 dispatch 自動啟用。
- 所有既有 Skills 都失敗：Planner 才能產生 `hybrid-v1` 等 Candidate，每次依 Critic feedback 修正，最多 5 個版本。
- Candidate 通過：自動登錄 Skill Library 並啟用；全部失敗：維持原 policy。

Demo UI 顯示診斷、sandbox evaluation、Critic feedback、重用或建立結果與自動切換事件。Developer mode（`uv run python app.py --mode developer`）才顯示手動 policy switch 與手動套用控制，僅供開發測試。

核心流程應為：

```text
Workload 改變
→ Planner 提出 Candidate
→ Evaluator 評估
→ Evaluator 通過
→ Candidate 成為 Skill
→ Agent 自動切換到新 Skill
→ 執行期間仍可自由切換所有既有 Skill
→ 未來可以再次提出新版本 Candidate
```

因此：

- Evaluator 通過 Hybrid 不會鎖住 policy switching。
- 通過 Hybrid 不代表必須 Reset 才能提出下一個 Candidate。
- Reset 只負責開始新 Run。
- Metrics 同時保留整局混合結果、各 policy segment 結果，以及依派工 policy 歸屬的 job 結果。
- 隊友後端若實作不同介面，應由 adapter 相容層轉換，不能讓 UI 直接依賴後端內部資料結構。
