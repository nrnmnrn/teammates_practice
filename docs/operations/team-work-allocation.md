# 團隊協作：總 PRD、子 PRD、Ticket

> 狀態：已核可操作指引；仍待跨隊員演練驗證。

本頁只定協作方式，不新增產品需求。總 PRD 是產品全圖；子 PRD 是一項完整、可驗收與合併的功能；Ticket 是一次可驗收的工作。下層與上層衝突時，停止並由人釐清。

## 分工、文件與權限

- 工作採動態認領：任何人認領未認領且依賴就緒的工作。領域不設固定 owner；認領衝突或同檔修改先協調。
- 于喬保留既有 merge 授權。任何人不得因認領而取得 merge、發布或產品決定權。
- 子 PRD 的實際切分、名稱、依賴、owner 與 issues 均待團隊核可後才建立。核可後，規格文件定義規格與依賴；GitHub 父 issue 記錄子 PRD 認領、狀態與證據，子 issue 記錄 Ticket。issue 依賴連結只鏡像文件；開工前核對父子 issues 已建立、引用核可規格且沒有重複拆票。
- 一份子 PRD 用一條 branch 與一個 Draft PR；每張 Ticket 原則一個 commit，一次 agent session 一票。正常先拆二張，最多三張；更多時重檢切分，確保不是巨票且驗收未遺漏。
- 子 PRD 可多人協作，但它仍是最小驗收與合併單位。作者自驗後，至少另一位隊員共同確認；必要驗收、完整測試、AI review 與作者自查皆完成後，才可 merge。merge 與 release 須使用者授權；merge 後在 `main` 驗證整合。最終展示由全隊確認。

## 每張 Ticket 的作法

1. 短檢查總 PRD、子 PRD、Ticket、相依證據、最新 `main`、未解紀錄與既有 `uv` 配置；不自行加依賴。跨子 PRD 的前置成果須 merge 並在 `main` 驗證，不能只看 issue closed；于喬明確同意穩定介面並行並留下證據時例外。同一子 PRD 的下一張 Ticket 可在同 branch 接前一張成果。
2. 認領一張依賴已就緒的 Ticket，確認本次驗收條件與修改範圍。
3. 資訊不足、文件矛盾或原因不明時，用 `$caveman:investigate-first` 先取得可查證原因或精確 blocker。涉及產品範圍、取捨或規格的決定，交由人決定。原因不明、難重現或偶發程式錯誤用 `$diagnosing-bugs`。
4. 明確實作用 `$implement` 管理，完成實作、相關測試與 `/code-review`。`$caveman:cavecrew` 僅適合定位、至多兩檔小修、或精簡 review；它不保證 token 節省或正確率。
5. 實作、必要測試與 review 通過後，才用 `$task-closeout` 核對完成證據、規格與 Git 狀態，並申請 commit 批准。

團隊覆寫 `$implement` 的自動 commit：沒有明確授權就不 commit。可複製指令：`請依本 Ticket 執行 $implement，完成實作、測試與 /code-review；不要 commit。必要驗證通過後，執行 $task-closeout 提出核可。`已有授權時依其範圍，不重問。closeout 成功後可詢問是否 `$handoff`。換 session 或 Blocked 時也可 handoff，不能偽稱完成；handoff 寫入作業系統暫存位置、不寫 repo，摘要文件未記錄的進度、假設與 blocker，並引用已有證據。

`/compact` 由 agent 自行判斷，沒有任何強制時點或門檻。

## Blocker、驗收與交接

同一 blocker 超過 15 分鐘，或已做兩次有證據嘗試仍未解，標記 Blocked 並回報隊員。不可為繼續工作自行改寫規格或跳過驗收。

子 PRD 的所有 Tickets 完成後，確認完整功能、必要驗收、完整測試、AI review 與作者自查；作者自驗後，至少另一位隊員共同確認，再由于喬依既有權限並取得使用者授權 merge。父 issue／PR 記錄驗證版本、預期與實際結果及審查證據；日期與參與者沿用 GitHub 紀錄，目錄不另填。merge 後在共同 `main` 確認整合；這才是子 PRD 完成，不是單張 Ticket 關閉。

目前交接包的短版實作流程見 [WORKFLOW.md](../../build-handoff/WORKFLOW.md)。日後子 PRD 目錄只在拆分計畫獲團隊核可後建立。產品規格或範圍變更需團隊決定；比賽已確認、參考先例與待確認事項見[比賽資訊](../competition/README.md)。

賽前只在獨立、可丟棄的非比賽 toy／sandbox repo 演練流程，禁止競賽碼與可重用實作。CI 須獲使用者明確授權才導入；CD 非預設。

## 白話說明

把工作想成蓋房子：總 PRD 是全屋藍圖，子 PRD 是一間能使用的房間，Ticket 是一小段施工。誰有空就認領已備妥材料的工作，但只有原本有權限的人能把房間正式交屋。若施工說明不清，先查證或請人決定；卡住就留下紀錄交接，而不是把半成品說成完工。
