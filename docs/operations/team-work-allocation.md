# 團隊協作：用「總 PRD、子 PRD、Ticket」把工作做完整

> 狀態：**流程已確認；工作依認領動態分工，仍須完成一次演練。**
>
> 本頁只說明合作方式，不新增產品需求。產品決策仍待主辦方回覆題目變更請求。

## 先懂三個名詞

把整個比賽想成蓋一間房子。

- **總 PRD** 是全屋藍圖：產品要解決什麼問題、大家共用什麼規則、最後怎樣才算完成。
- **子 PRD** 是其中一個完整房間的藍圖：例如「模擬結果可以正確展示」。團隊把這個房間做到可用。
- **Ticket** 是房間裡的一個施工步驟：例如先做好輸入資料，再做好計算，再做畫面驗證。一次 agent session 只做一張。

規則很簡單：**總 PRD > 子 PRD > Ticket**。下層內容與上層不一致時，先停下來，不猜、不自行改需求，由當下隊員協調並記錄。

本 planning repo 不寫比賽程式。官方 coding window 開始後，才在未來的 competition implementation repo 開發；`build-handoff/` 是交給該 repo 的唯一文件介面。以最新的 [README](../../build-handoff/README.md) 與 [ACCEPTANCE](../../build-handoff/ACCEPTANCE.md) 為交接與驗收依據。

## 工作認領與權限

- 工作無預設固定 owner。未認領工作可自行登記並開工；已有隊員處理時，先協調。
- 子 PRD 內可多人分工；同一檔案先協調，避免同時修改造成衝突。
- 子 PRD 是最小驗收與合併單位。完整功能與必要檢查完成後，作者可先自行驗算並標示待共同確認；至少另一位隊員共同確認、經既有授權合併後，該子 PRD 才通過。
- 不強制影音證據或固定獨立 reviewer。最終展示由全隊確認。
- 動態認領不改變權限：于喬保留既有的最終合併角色；合併或發布仍須使用者授權。

範圍縮減須由團隊明確決定並更新規格；不可自行刪除或跳過必要檢查。修改後重驗受影響項與相關串接。

## 從想法到完成的流程

總 PRD、新功能或仍模糊的工作，先由隊員共同釐清，依序使用：

`$grill-me` 或 `$grill-with-docs`（找出不清楚處）→ `$to-spec`（寫清楚規格）→ `$to-tickets`（拆施工步驟）→ `$implement`（完成工作）。

隊員承接已核准且內容完整的子 PRD 時，不必重做完整訪談。子 PRD 本身就是已寫清楚的 spec。先執行一次 `$to-tickets`，再按順序讓每張 Ticket 各執行一次 `$implement`。每張 Ticket 確認完成後，先執行 `/compact`，再執行 `$task-closeout`，留下狀態、證據與下一步。

`$implement` 會要求開發、測試、`/code-review` 與 commit。

拆 Ticket 時，子 PRD 至少要有：要交付什麼、做什麼與不做什麼、和總 PRD 的關係、輸入與輸出約定、依賴什麼、怎樣驗收、還有哪些未解問題。缺少會影響決定的內容時，不開工。

## 子 PRD 每次工作怎麼做

1. 讀總 PRD 的共用規則、完整子 PRD、目前 Ticket、前置工作證據、最新 `main` 狀態與未解紀錄。
2. 只完成目前一張 Ticket，並依真正依賴順序做，不只看票號。
3. 完成該 Ticket 所列工作者自驗與相關測試，執行 `$implement`，留下清楚 commit、結果與未解問題。
4. Ticket 已真正完成時，先執行 `/compact`，再執行 `$task-closeout`。它核對完成狀態、改動範圍與下一張優先工作；`$implement` 已 commit 時，closeout 不需要再產生第二個 commit。
5. 下一次 session 再讀同一份子 PRD，接著完成下一張可做 Ticket。
6. 全部 Tickets 完成後，驗收整個子 PRD，而不是只看「每張票都關掉了」。

一張 Ticket 的完成紀錄只記：工作者自驗、相關測試、`$implement` 的 AI `/code-review`、清楚 commit，以及限制或 blocker。這不代表子 PRD 驗收；不另設 Ticket 的共同批准、驗收或合併流程。

若同一個 blocker 花超過 15 分鐘，或已做兩次有證據的嘗試，標記為 Blocked 並回報隊員。不要為了繼續做而自行創造另一套規則。

## Git 怎麼使用

一份子 PRD 用：**一條 branch + 一個 Draft Pull Request + 每張 Ticket 一個 commit**。

這樣既不會為每張小票反覆開 branch，也能在子 PRD 完成前看見進度。遇到重要共用介面或整合點，先同步 `main`，由當下隊員協調。下游工作原則上等前置成果已 merge 並在 `main` 驗證後再開始。

子 PRD 完成時，才做較完整關卡：完整測試、作者自查、實際功能結果確認與至少另一位隊員共同確認。通過後才由既有授權角色正式 PR、merge；合併即完成該子 PRD 驗收。AI 可以找問題，不能核准或 merge。

## 新問題怎麼處理

- 原本答應的功能做錯了，或少拆了一步：在原子 PRD 補一張 Ticket。
- 文件內容模糊或彼此矛盾：停下來，由隊員釐清。
- 真正多出一個可獨立交付的新功能：由隊員新增一份子 PRD，再為它拆 Tickets。

不要把每個小發現都開成新的子 PRD；否則房間會被切成太多碎片，最後沒人知道哪一份文件才是完整說明。

## 開始前的短演練

正式開始前，隊員要在獨立、可丟棄的非比賽 toy／sandbox repo，用一個假的小子 PRD 演練：拆票、認領、開發、Draft PR、review、merge、demo。記錄花多少時間、卡在哪裡。

演練 repo 不可放比賽產品程式、可重用實作、RL pipeline、Agent loop 或 reward logic；也不在本 planning repo 或未來競賽 repo 演練。CI 只在演練後且使用者明確同意才導入；CD 不列為預設。

整體完成時，所有子 PRD 都必須在同一份 `main` 正常運作，測試、demo、fallback 均可用，並由全隊確認整體串接與最終展示。
