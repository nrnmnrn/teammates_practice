# SP-01：Scheduling Arena

## 交付結果

交付一個不依賴 Agent adaptation 也能完整展示與驗收的排程 Arena：使用者控制可重現的單 worker 模擬、注入 workload，並在三個 Gradio tabs 看見同一 Snapshot 的排程過程、metrics、實際 policy code 與初始 Skill Library。

預估比賽當日拆為 2–3 張 vertical-slice Ticket。Owner、parent issue 與 Ticket 在比賽當日決定。

## 範圍

- 實作 Job 驗證、五種 Policy 定義（Hybrid 只能作未登錄的可驗證候選規則）、生命週期、deadline、事件順序、單 worker、不可搶占、reset、seed、20 筆名額與 deterministic `advance`。
- 產生契約要求的 Snapshot、排程 events、整局 metrics、series、初始 Skills 與 developer-mode 的已驗證 Skill 套用入口。
- 實作 Gradio 共用 shell 與資料來源標示；Scheduling Arena 的播放、暫停、單步、倍速、reset、inject 與 SVG 狀態呈現。
- 實作 Metrics & Code 的整局 cards、趨勢、實際排序 code、Policy diff；實作 Skill Library 的初始 Skill 閱讀、預覽與來源／使用記錄呈現。
- 以 1440×900、1280×720 做可讀性與操作驗收；動畫只能插補 Snapshot，不可成為狀態來源。

## 不在範圍

不負責 trigger、既有 Skill evaluator、Candidate 生成／評估／晉升。不得預先建立 Hybrid Skill、Candidate 或 adaptation UI state。不得把 local 結果冒充 team backend。

## 介面與直接依賴

本子 PRD 提供 [後端契約](../docs/contracts/backend-contract.md) 所定義的 SchedulerBackendAdapter、Snapshot、Job、Skill、Event、metrics、segments 插入點與 UI shell。無直接上游子 PRD，可先開始。

下游 `existing-skill-adaptation` 依賴本子 PRD 的 Snapshot、metrics、events、Policy activation seam 與 adaptation UI 插入點；這是 **Contract** 依賴。契約凍結後，下游可用 deterministic adapter／fixture 平行工作。與實際 Arena 的聯合驗收另為 **Integration** 依賴，須等本子 PRD 完成。

## 必要行為

- 預設 `seed=42`、八筆可重現 Jobs、初始 FIFO；reset 新建 run 並恢復四個初始 Skills。
- 同時刻事件順序及全部 Policy tie-break 必須符合總 PRD。不同 `advance` 切分不可影響結果。
- `inject` 必須整批原子；無效資料、過去 arrival、ID 衝突、超過名額不可留下部分資料或消耗 RNG。
- pending／running／scheduled／completed／expired 的顯示與後端 Snapshot 一致。worker 閒置原因須可讀。
- Arena、Metrics & Code、Skill Library 同一 UI 更新中使用同一 `run_id` 與 `snapshot_version`；切 tab 不得新增 timer 或推進狀態。
- Demo mode 隱藏手動 Policy 切換與套用；Developer mode 才可測試已驗證 Skill。資料來源與 Mock 標籤始終可見。

## 驗收與證據

- 使用可手算資料驗證 FIFO、SJF、Priority、EDF，以及通過驗證後的 Hybrid 選擇、平手、deadline 與同時刻事件順序。
- 比較大步與小數分段 `advance` 的 Job、events、metrics；驗證 P95、throughput、名額、reset、RNG 與輸入拒絕。
- 以兩個 factory 實例驗證 session 隔離、防禦性 Snapshot、JSON 可序列化與舊 revision 拒收。
- 瀏覽器驗證控制項、20 筆捲動、tab 切換、動畫、畫面尺寸及同 Snapshot 顯示。
- team 模式必須實際呼叫指定 factory；載入失敗明確錯誤、不 fallback。local 僅可作明確開發／備援模式。

## 尚待決定

Policy segment metrics 對跨 Policy 切換的 Job 之精確歸屬尚待決定。此子 PRD 必須保留契約欄位與可驗證的 dispatch 記錄，但不得自行宣稱某一歸屬規則為產品決定。
