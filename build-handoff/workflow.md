# sub-PRD 工作流程

## 開始

1. 依 [INDEXER.md](INDEXER.md) 選讀文件；開始 Ticket 時必須讀已核可 sub-PRD、GitHub parent issue、目前 Ticket 與 `main`。
2. 核對非 `main` branch 對應一份 sub-PRD，且 parent issue 已記 branch 與唯一的 Draft Pull Request。branch 表示歸屬；狀態與角色指派以 parent issue 為準。
3. 核對範圍、驗收、owner 與跨 sub-PRD 依賴已核可。跨 sub-PRD 依賴須已在 `main` 驗證，或有 main branch 負責人明示例外。

本流程中的 main branch 負責人，是 parent issue 指定、負責跨 sub-PRD 決策與整合審查的人類角色；AI 不得自行擔任、核可或 merge。

## Ticket 執行

每個 session 只完成一張目前 Ticket，並產生可獨立驗證的結果。原範圍遺漏步驟可補 Ticket，記錄理由；一份 sub-PRD 最多 5 張。影響交付結果、範圍、驗收、跨 sub-PRD 依賴或 owner 的變更，先成為提案版本，待 main branch 負責人核可才可執行。

每張 Ticket 完成時，在 parent issue 留下結果、證據與 branch／PR 連結。不得自動 push、merge 或 deploy。

## 阻塞

未知原因、衝突或兩次有證據嘗試仍無法前進時，記錄症狀、嘗試與證據，標記 Blocked 並通知 main branch 負責人。可重現的程式錯誤使用診斷流程；產品決策若仍依賴主辦方回覆，維持暫停而不自行決定。

## 完成

Ticket 完成前：通過相應驗收與測試、留證據、完成所需 review。全部 Tickets 完成後：owner 做整體驗收、完整測試與最後 review；至少一位其他隊員共同確認，高風險結果由 parent issue 指定的審核者核對。main branch 負責人完成範圍、架構與整合審查，取得人類合併授權後才可 merge；再於 `main` 驗證，才可在 parent issue 標示 Done。

## 整合

整合前在 GitHub parent issue 核對核可規格版本、所有 Ticket 證據、實際 branch、Pull Request 與依賴。ADR 只保存決策及取捨；操作規則在本檔；易變狀態只在 parent issue。

## Skill routing

| 觸發情境 | 呼叫方式 | 預期結果 | 不可用時的人工替代流程 |
| --- | --- | --- | --- |
| 不確定該用哪條工作流程 | `$ask-matt` | 選出適合的流程與下一步。 | 依 INDEXER 比對情境，於 parent issue 記錄選擇理由。 |
| 原因未知、行為間歇或證據不足 | `$caveman:investigate-first` | 以證據排序假設，指出原因或精確 blocker。 | 追蹤輸入、狀態轉換、所有權與失敗輸出；未有可信機制前不修改。 |
| 可重現的程式錯誤或效能回歸 | `$diagnosing-bugs` | 建立 tight、可重現的 red loop，修正並加回歸測試。 | 先建立單一可重跑的失敗命令；最小化案例、排序假設、一次驗證一項。 |
| 已知檔案的小範圍定位、修改或 diff 審查 | `$caveman:cavecrew` | 以 investigator、builder 或 reviewer 取得壓縮結果。 | 用 `rg` 定位；小改後重讀 diff，逐項核對驗收。 |
| 修改 agent 指引、流程、模板或其他 agent 文件 | `$writing-for-agents` | 依 context pointer、漸進揭露與完成標準重寫。 | 保留單一權威；只內嵌每次必需內容，其他以明確觸發連結指向。 |
| 實作與驗證已全數完成 | `$task-closeout` | 核對狀態、證據、session 變更與下一步；取得 commit 授權。 | 核對 basis 文件、所有驗收、完整 diff 與 Git 狀態；不自行 stage 或 commit。 |
| 新 session、換目錄或交給同事 | `$handoff`（pending：未確認已安裝） | 產生只引用既有證據的可攜交接。 | 在 OS 暫存目錄寫短交接，引用檔案、issue 與證據，不複製原件。 |

pending 的 skill 不得假裝可呼叫；使用人工流程時，將結果與證據留在 parent issue。
