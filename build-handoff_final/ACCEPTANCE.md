# Adaptive Scheduler Arena 驗收矩陣

本文件承接總 PRD的每項需求、負責子 PRD與驗收證據。它也是正式流程的 closeout 證據索引，不重複改寫產品需求。所有項目目前為待驗收；`build-handoff_final` 通過獨立驗收前不可宣稱已正式取代其他 handoff。

## 證據規則

- 每列填入結果、證據連結或命令輸出、日期與驗收者；未通過須保留限制，不能以 local 結果宣稱 team mode 通過。
- 子 PRD closeout 必須有：完整功能驗收、相關測試、AI review、Owner 自查、另一位成員確認、正式 PR merge，以及 `main` 整合驗收。
- 所有子 PRD完成仍不足以宣告產品完成；必須再通過本表的整合與 90 秒 demo 項目。

| ID | 總 PRD需求 | 負責子 PRD | 驗收證據 | 結果／日期／驗收者 |
| --- | --- | --- | --- | --- |
| R01 | Job 驗證、狀態、單 worker、不可搶占、deadline 與同時刻事件順序正確。 | SP-01 | 可手算 Policy／deadline 案例與契約測試。 | 待填 |
| R02 | FIFO、SJF、Priority、EDF 排序與 tie-break 正確；Hybrid 未通過 gate 前不是初始 Skill。 | SP-01 | 五種 Policy 的 deterministic 測試；reset 的初始 Skill 檢查。 | 待填 |
| R03 | `advance` 切分一致；seed、注入、20 筆上限與 reset 可重現且原子。 | SP-01 | 大步／分段、RNG、無效批次、名額、reset 測試。 | 待填 |
| R04 | 三個 tabs、Arena 控制、動畫、Metrics & Code、初始 Skill Library 同一 Snapshot 且可讀。 | SP-01 | controller／revision 測試；1440×900、1280×720 瀏覽器證據。 | 待填 |
| R05 | Demo mode 只允許播放、暫停、單步、reset、倍速與 workload；Developer mode 才可手動套用已驗證 Skill。 | SP-01 | 兩種 mode 的控制可見性與操作測試。 | 待填 |
| R06 | provider-neutral 的 team backend 與外部 Agent adapter 可用，或明確標示為 local／Mock；team 載入失敗不 fallback。 | SP-01、SP-03 | `--backend team --factory`、外部 adapter、Mock 標示與失敗 factory 的啟動／整合證據。 | 待填 |
| R07 | trigger 未成立不 adaptation；成立後以相同 baseline 評估所有已驗證 Skills。 | SP-02 | trigger、相同 workload／seed／state／window、sandbox fixture 測試。 | 待填 |
| R08 | 目前或其他既有 Skill 通過時，依固定順序重用，自動於下次 dispatch 啟用，不建立 Candidate。 | SP-02 | 目前 Skill、多 Skill tie-break、無 Planner／Candidate、running Job 保留測試。 | 待填 |
| R09 | UI 顯示 degradation、既有 Skill 比較、選擇原因、`existing_skill_reused` 與 `policy_activated`。 | SP-02 | 同一 Snapshot 的 event、metrics、瀏覽器證據。 | 待填 |
| R10 | Candidate 只在全部既有 Skills 失敗後建立；ID／父版本／原因／code／workload／feedback 可追溯。 | SP-03 | all-failed gate、Candidate lineage 與 UI 證據。 | 待填 |
| R11 | Candidate sandbox 評估與 evaluator gate 正確；sandbox 不修改 Live Run。 | SP-03 | baseline／Candidate metrics、契約檢查、Live Snapshot before/after 測試。 | 待填 |
| R12 | Critic feedback 最多推動五版；拒絕、契約錯誤、第五版失敗皆維持 Live Policy。 | SP-03 | 版本上限、failure reason、`adaptation_failed` 測試與畫面。 | 待填 |
| R13 | 外部 Planner／Evaluator／Critic／LLM 錯誤停止 loop、保留最後有效 Snapshot、顯示錯誤，不自動轉 Mock。 | SP-03 | timeout／schema／服務錯誤 adapter 測試與 UI 證據。 | 待填 |
| R14 | 僅 gate-passed Candidate 可註冊為 verified Skill，並在 Skill Library 顯示來源與使用記錄。 | SP-04 | passed／rejected input、register、Library Snapshot／瀏覽器測試。 | 待填 |
| R15 | 註冊後自動啟用；新 Policy 只在下次 dispatch 生效；建立 Policy segment 與 `policy_activated`。 | SP-04 | running Job、next dispatch、segment／event 測試。 | 待填 |
| R16 | promotion／activation／同步錯誤時暫停、保留最後有效 Snapshot；reset 恢復四個初始 Skills。 | SP-04 | state-uncertain、resync／reset、Skill／segment 清理測試。 | 待填 |
| R17 | Live、segment、existing evaluation、Candidate evaluation metrics 分開，不能以 sandbox 或混合 metrics 作錯誤宣稱。 | SP-01、SP-02、SP-03、SP-04 | 契約測試、UI 標示與人工資料解讀檢查。 | 待填 |
| R18 | Snapshot、Event、EvaluationResult、adapter timeout／錯誤與 session revision 契約一致。 | SP-01、SP-02、SP-03、SP-04 | contract tests、兩個 session、舊回應拒收、錯誤恢復。 | 待填 |
| R19 | 90 秒 Demo 依正常 workload、Flash Sale、既有重用、Candidate 成功、live／sandbox 區分完整走通。 | SP-01、SP-02、SP-03、SP-04 | team mode 操作紀錄、測試輸出與必要截圖。 | 待填 |
| R20 | 不含未核定 scope：Round Robin、搶占、worker failure、多 worker、長期 evaluator、登入、部署、未驗證動態 code。 | SP-01、SP-02、SP-03、SP-04 | code／設定人工審查與最終 AI review。 | 待填 |

## Closeout 總檢

| ID | 條件 | 證據 | 結果／日期／驗收者 |
| --- | --- | --- | --- |
| C01 | 每個子 PRD 已依流程完成完整功能驗收、測試、AI review、Owner 自查、另一位成員確認、正式 PR merge 與 `main` 整合驗收。 | 四份子 PRD 的 closeout 連結與 `main` 驗收結果。 | 待填 |
| C02 | `DEPENDENCIES.md` 與每份子 PRD的直接依賴一致；所有 Contract／Integration 邊完成真正整合。 | 依賴逐邊核對紀錄。 | 待填 |
| C03 | team mode 的所有必要測試、lint、format check 與瀏覽器驗收通過；local 證據未被混作 team 證據。 | 命令輸出與來源標示。 | 待填 |
| C04 | 尚待決定的 trigger overlap、segment attribution、額外 Skill metadata、錯誤 event／retry 行為，已由團隊作決定或不影響已驗收行為。 | 決定紀錄或受影響驗收列。 | 待填 |
| C05 | 交接包可由獨立 implementation repository 讀取並執行，不需要 planning repository、會議紀錄、AO 草案或預先建立 Ticket。 | 獨立讀取／rehearsal 證據。 | 待填 |
