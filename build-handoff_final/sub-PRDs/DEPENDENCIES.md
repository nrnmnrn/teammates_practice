# 子 PRD 靜態依賴索引

本文件只索引跨子 PRD 的依賴。每份子 PRD 是其直接依賴與理由的權威；若本索引衝突，停止相關整合，交由 main branch 負責人處理後再更新規格。

方向固定為「Consumer depends on Provider」。本文件不記錄 Owner、狀態、Issue、Ticket、進度或測試結果。

## 類型

| 類型 | 意義 |
| --- | --- |
| Hard | Provider 的指定成果未完成前，Consumer 不能安全開始。 |
| Contract | 契約核定後，Consumer 可用 deterministic adapter 或 fixture 平行準備；真正整合仍受 Provider 限制。 |
| Integration | 雙方可各自完成；僅聯合驗收受彼此限制。 |
| None | 無直接依賴。 |

## 子 PRD 索引

| ID | 子 PRD | 完整交付能力 | 首個工作 |
| --- | --- | --- | --- |
| SP-01 | [Scheduling Arena](scheduling-arena.md) | 可操作、可觀察的基礎排程與 UI shell。 | 可開始；無上游子 PRD。 |
| SP-02 | [Existing Skill Adaptation](existing-skill-adaptation.md) | 退化後評估、重用與自動啟用既有 Skill。 | 可在 SP-01 契約凍結後，以 fixture／adapter 開始。 |
| SP-03 | [Candidate Evaluation Loop](candidate-evaluation-loop.md) | 安全產生、sandbox 評估與迭代 Candidate。 | 可在 SP-01／SP-02 契約凍結後，以 fixture／adapter 開始。 |
| SP-04 | [Skill Promotion and Recovery](skill-promotion-and-recovery.md) | 通過 Candidate 的正式晉升、啟用、復原。 | 可在 SP-01／SP-03 契約凍結後，以 fixture／adapter 開始。 |

## 依賴邊

| Edge ID | Consumer | Provider | Type | Required output / contract | Start condition | Integration condition | If unavailable | Exception approval |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DEP-01 | SP-02 | SP-01 | Contract | SchedulerBackendAdapter、Snapshot、metrics、events、已驗證 Skills、Policy activation、pause／resume seam、adaptation UI 插入點。 | 契約已核定；使用 deterministic Snapshot／adapter fixture。 | SP-01 的實際 Arena 成果可提供同一契約，且 team mode 聯合驗收 pause／resume。 | 僅完成 fixture 路徑；不得宣稱與 Arena 已整合。 | main branch 負責人明確批准穩定契約例外。 |
| DEP-02 | SP-03 | SP-02 | Contract | `(run_id, workload_window.id)` 唯一 TriggerResult、既有 Skill EvaluationResult、all-skills-failed 判定、active adaptation context。 | 契約已核定；使用 deterministic all-failed context 與 fake adapter。 | SP-02 的實際輸出可驅動同一 adaptation Candidate loop。 | 僅驗證 Candidate loop fixture；不得繞過 trigger 去重規則。 | main branch 負責人明確批准穩定契約例外。 |
| DEP-03 | SP-03 | SP-01 | Contract | sandbox、baseline Snapshot、metrics、events、pause／resume、手動恢復 UI 插入點。 | 契約已核定；使用可重現 baseline fixture。 | SP-01 的實際 scheduler 能證明 sandbox／live 隔離及 clock pause。 | 只做 isolated sandbox fixture；不得宣稱 Live Run 隔離或恢復已完成。 | main branch 負責人明確批准穩定契約例外。 |
| DEP-04 | SP-04 | SP-03 | Contract | gate-passed Candidate、EvaluationResult、Candidate lineage。 | 契約已核定；使用不可變 fixed passed result。 | SP-03 實際 gate 輸出可驅動 register。 | 僅驗證 promotion fixture；不得接受未證實 Candidate。 | main branch 負責人明確批准穩定契約例外。 |
| DEP-05 | SP-04 | SP-01 | Contract | Skill Library、dispatch attribution、Snapshot、events、segments、pause／resume、reset、UI shell。 | 契約已核定；使用 deterministic scheduler adapter。 | SP-01 實際 dispatch、pause／resume、reset 與三 tabs 可呈現 promotion evidence。 | 只做 adapter fixture；不得宣稱正式啟用或 UI 整合。 | main branch 負責人明確批准穩定契約例外。 |
| DEP-06 | SP-02 | SP-01 | Integration | Arena 的實際 Snapshot、pause／resume 與 UI shell。 | SP-02 可依 DEP-01 先行。 | team mode 中，Arena、metrics、events、pause／resume 與 adaptation evidence 使用同一 Snapshot。 | 保留 fixture 證據，等待聯合驗收。 | 不適用；不可跳過聯合驗收。 |
| DEP-07 | SP-03 | SP-01、SP-02 | Integration | 真實 trigger、all-failed context、pause／resume、retry、sandbox／live 比較。 | SP-03 可依 DEP-02、DEP-03 先行。 | 真實 all-failed 路徑以同一 adaptation 進入 loop，失敗保持暫停且只可安全手動恢復。 | 保留 fixture 證據，等待聯合驗收。 | 不適用；不可跳過聯合驗收。 |
| DEP-08 | SP-04 | SP-01、SP-03 | Integration | 真實 passed Candidate、registration、activation、resume、dispatch attribution 與 retry／reset evidence。 | SP-04 可依 DEP-04、DEP-05 先行。 | team mode 中由 passed Candidate 走到 Skill Library、resume、下次 dispatch、segment attribution 與恢復。 | 保留 fixture 證據，等待聯合驗收。 | 不適用；不可跳過聯合驗收。 |

## 全域整合閘門

這些是產品 closeout 的整合條件，不是新的子 PRD：

- 四份子 PRD 使用相容的 Snapshot、Event、EvaluationResult、Policy activation 與錯誤契約。
- 三個 tabs、metrics、Skill Library 與 events 對應同一 Snapshot。
- Live Run 與 Sandbox Run 隔離。
- 正常 workload、Flash Sale、trigger pause／去重、既有 Skill 重用、Candidate 成功、resume 與五版／外部錯誤失敗後的手動恢復都有驗收證據。
- 90 秒短展示可導覽可追溯的 team-mode Run 證據，不以外部 AI 在 90 秒內完成為驗收條件；不固定時長 progression run 實際驗證多輪 baseline gate、activation 與 segment attribution。
