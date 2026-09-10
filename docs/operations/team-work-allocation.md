# 四人協作：用「總 PRD、子 PRD、Ticket」把工作做完整

> 狀態：**流程已確認；四位成員仍須確認角色，並完成一次演練。**
>
> 本頁只說明合作方式，不新增產品需求。產品決策仍待主辦方回覆題目變更請求。

## 先懂三個名詞

把整個比賽想成蓋一間房子。

- **總 PRD** 是全屋藍圖：產品要解決什麼問題、大家共用什麼規則、最後怎樣才算完成。
- **子 PRD** 是其中一個完整房間的藍圖：例如「模擬結果可以正確展示」。一人負責把這個房間做到可用。
- **Ticket** 是房間裡的一個施工步驟：例如先做好輸入資料，再做好計算，再做畫面驗證。一次 agent session 只做一張。

規則很簡單：**總 PRD > 子 PRD > Ticket**。下層內容與上層不一致時，先停下來，不猜、不自行改需求。由于喬更新文件；涉及題目理解或結果正確性時，請咏宸確認。

本 planning repo 不寫比賽程式。官方 coding window 開始後，才在未來的 competition implementation repo 開發；`build-handoff/` 是交給該 repo 的唯一文件介面。

## 每個人的責任

- **于喬** 是總設計師與整合者：確認總 PRD、切出子 PRD、指派 owner、處理跨子 PRD 問題、審查並合併最後成果。
- **咏宸（Neo）** 是題目與結果的核對者：檢查演算法結果、題意、高風險決定與 demo 是否合理。
- **子 PRD owner** 是一個完整功能的負責人：依序完成自己子 PRD 的所有 Tickets，最後證明整個子 PRD 可用。
- **其他隊員** 可提議承接尚未有人負責的完整子 PRD；不可隨意抽走別人的單張 Ticket。

一份子 PRD 只有一位主要寫作者。這像一位廚師負責一道菜：別人可幫忙試味道，但不能同時各自往同一鍋加料。

## 工作總覽（overview）

| 誰 | 平常負責什麼 | 使用哪段流程 | Overcooked! 比喻 |
| --- | --- | --- | --- |
| 于喬 | 把總 PRD 的模糊想法變成可分配的子 PRD，分派工作，處理跨組問題與最後整合。 | `$grill-me` 或 `$grill-with-docs` → `$to-spec` → `$to-tickets` → `$implement` | 主廚：決定菜單，確認整桌菜能一起端出。 |
| 子 PRD owner | 承接一份已核准的子 PRD，按依賴順序完成所有 Tickets。 | 先 `$to-tickets`；每張 Ticket 執行 `$implement`，完成後 `/compact` 再 `$task-closeout` | 一道菜的負責廚師：從備料到擺盤。 |
| 咏宸 | 核對題意、演算法結果與高風險 demo；需要時協助于喬釐清問題。 | 以 review、核對與必要的完整流程為主 | 試菜與品質檢查者。 |
| 其他隊員 | 提議承接未認領的完整子 PRD，或依于喬安排協助核對。 | 承接子 PRD 時，依子 PRD owner 流程 | 等待接下一道完整料理的廚師。 |

這是正常分工，不是限制。若出現少見或特殊情況，于喬明確指派某位組員從頭負責一項完整功能時，該組員可參考于喬的完整流程，先釐清需求、寫清楚規格，再拆 Tickets。未獲指派時，不自行重寫總 PRD 或另開產品方向。

## 從想法到完成的流程

于喬通常處理總 PRD、新功能或仍模糊的工作，依序使用：

`$grill-me` 或 `$grill-with-docs`（找出不清楚處）→ `$to-spec`（寫清楚規格）→ `$to-tickets`（拆施工步驟）→ `$implement`（完成工作）。

其他組員承接已核准且內容完整的子 PRD 時，不必重做完整訪談。子 PRD 本身就是已寫清楚的 spec。先執行一次 `$to-tickets`，再按順序讓每張 Ticket 各執行一次 `$implement`。每張 Ticket 確認完成後，先執行 `/compact`，再執行 `$task-closeout`，留下狀態、證據與下一步。

`$implement` 會要求開發、測試、`/code-review` 與 commit。

拆 Ticket 時，子 PRD 至少要有：要交付什麼、做什麼與不做什麼、和總 PRD 的關係、輸入與輸出約定、依賴什麼、怎樣驗收、owner、reviewer、還有哪些未解問題。缺少會影響決定的內容時，不開工。

## 子 PRD owner 每次工作怎麼做

1. 讀總 PRD 的共用規則、完整子 PRD、目前 Ticket、前置工作證據、最新 `main` 狀態與未解紀錄。
2. 只完成目前一張 Ticket，並依真正依賴順序做，不只看票號。
3. 通過該 Ticket 的驗收條件與相關測試，執行 `$implement`，留下清楚 commit、結果與未解問題。
4. Ticket 已真正完成時，先執行 `/compact`，再執行 `$task-closeout`。它核對完成狀態、改動範圍與下一張優先工作；`$implement` 已 commit 時，closeout 不需要再產生第二個 commit。
5. 下一次 session 再讀同一份子 PRD，接著完成下一張可做 Ticket。
6. 全部 Tickets 完成後，驗收整個子 PRD，而不是只看「每張票都關掉了」。

一張 Ticket 的完成標準只有：驗收條件通過、相關測試通過、`$implement` 的 AI `/code-review` 已完成、commit 清楚、限制或 blocker 已記錄。每張 Ticket 不必人工 review 或 merge，避免過度拖慢速度。

若同一個 blocker 花超過 15 分鐘，或已做兩次有證據的嘗試，標記為 Blocked 並回報于喬。不要為了繼續做而自行創造另一套規則。

## Git 怎麼使用

一份子 PRD 用：**一條 branch + 一個 Draft Pull Request + 每張 Ticket 一個 commit**。

這樣既不會為每張小票反覆開 branch，也能在子 PRD 完成前看見進度。遇到重要共用介面或整合點，先同步 `main`，由于喬快速確認。下游工作原則上等前置成果已 merge 並在 `main` 驗證後再開始。

子 PRD 完成時，才做較完整關卡：完整測試、最後 AI review、owner 自查、于喬的架構與整合 review；高風險結果再由咏宸核對。通過後才正式 PR、merge，並在 merge 後驗收整合結果。AI 可以找問題，不能核准或 merge。

## 新問題怎麼處理

- 原本答應的功能做錯了，或少拆了一步：在原子 PRD 補一張 Ticket。
- 文件內容模糊或彼此矛盾：停下來，請于喬澄清；題意問題請咏宸核對。
- 真正多出一個可獨立交付的新功能：于喬新增一份子 PRD，再為它拆 Tickets。

不要把每個小發現都開成新的子 PRD；否則房間會被切成太多碎片，最後沒人知道哪一份文件才是完整說明。

## 開始前的短演練

正式開始前，四人要在獨立、可丟棄的非比賽 toy／sandbox repo，用一個假的小子 PRD 演練：拆票、認領、開發、Draft PR、review、merge、demo。記錄花多少時間、卡在哪裡。

演練 repo 不可放比賽產品程式、可重用實作、RL pipeline、Agent loop 或 reward logic；也不在本 planning repo 或未來競賽 repo 演練。CI 只在演練後且使用者明確同意才導入；CD 不列為預設。

整體完成時，所有子 PRD 都必須在同一份 `main` 正常運作，測試、demo、fallback 均可用，並由于喬核准整合、咏宸確認結果合理。
