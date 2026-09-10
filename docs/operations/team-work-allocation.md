# 四人 child PRD 協作政策

> 狀態：**作業設計已由規劃者確認；四位成員的角色確認與演練仍待完成。**
>
> 本頁是協作政策權威。產品決策仍暫停，直到主辦方回覆題目變更請求；本頁不新增產品需求，也不取代 parent PRD（總產品需求文件）。

competition implementation workflow 只在官方 coding window 開始後的 future competition implementation repo 進行。本 planning repo 在賽前不建立任何比賽 source code；`build-handoff/` 是交給未來實作 repo 的唯一介面。賽前四人演練的 branch、Draft PR、review、merge 只在獨立、可丟棄的非比賽 toy／sandbox repo 進行，不在本 planning repo 或 future competition implementation repo 進行。

## 權威與交付單位

決策權威依序為：**parent PRD > 已核准 child PRD > Ticket**。三者若矛盾，立即停止受影響工作；林于喬更新受影響規格與 Ticket。人或 agent 不得默默選擇一種解讀，或自行擴大範圍。

- 最終認領與交付單位是 child PRD。每份 child PRD 恰有一位 primary owner／writer，負責其全部 Ticket 與最終交付。
- 一個 agent session 只執行一張 Ticket。Ticket 依 blocking edge（必須先完成的依賴）排序，不依票號排序。
- child PRD 的唯一真相來源是 versioned repo Markdown。GitHub parent Issue 只連到它；Ticket 是 child Issue，或必須寫 Parent reference。GitHub Project 只追蹤狀態，不重複規格正文。

## child PRD 何時可拆 Ticket

child PRD 至少要寫明：使用者結果、範圍／不做範圍、與 parent 的關係、輸入／輸出／contracts（介面約定）、跨 child 依賴、驗收／demo、owner／reviewer、未解問題。缺少會影響決策的資訊時，不可拆 Ticket。

新 parent 工作或內容有歧義時，依序使用 `$grill-me` 或 `$grill-with-docs`（質問假設與查既有文件）、`$to-spec`（整理規格）、`$to-tickets`（拆可執行工作）、`$implement`（完成一張工作單）。已核准且完整的 child PRD，先 `$to-tickets`，再每張 Ticket 各跑一次 `$implement`。若拆票時發現歧義，退回澄清，不帶著猜測實作。

## 狀態、認領與工作時限

狀態只有：Draft、Ready、Active、Blocked、Review、Done。林于喬確認派工；尚未認領者只能提議承接一整份未認領 child PRD，不可任意拿其中一張 Ticket。

child PRD 通常拆為 2–5 張 Ticket，或縮至可在 integration cutoff（整合截止點）前完成。若既有文件有指定，Ticket 目標為 30–60 分鐘。同一阻礙最多花 15 分鐘或提出兩次有證據的嘗試；之後標為 Blocked，附證據回報。活動最後 25% 時間保留給整合、review、demo 與 fallback。

## 每個 session 的輸入與輸出

開始時，讀取：parent 的共同規則、完整 child PRD、目前 Ticket、已完成 blocker 的證據、最新 `main`／integration 狀態、未解紀錄。

結束時，留下：Ticket 識別、驗收／測試證據、已做決策、未解問題、下一張可執行 Ticket。無法完成時也要留下同等證據。

## Ticket 完成與 Git 工作方式

Ticket 完成門檻：驗收條件達成、相關測試通過、`$implement-required` 的輕量 AI `/code-review` 完成、只有一個清楚 commit、限制／blocker 已記錄。每張 Ticket 不要求人類 review 或 merge；AI 永遠不可核准或 merge。

一份 child PRD 使用一條 branch 與一個 Draft Pull Request，不是每張 Ticket 各一條／一個。每張 Ticket 一個 commit，且只有 primary writer 能修改該 child PRD 的實作。重要共用介面可請林于喬快速確認；在依賴或整合節點同步 `main`。協助若變成實作，須正式轉交 owner，或使用隔離的例外 branch；不可並行改同一份工作。

child PRD 完成門檻：完整測試、最後 AI review、owner 自查、林于喬的架構／範圍／整合 review、高風險時咏宸（Neo）的領域結果 review、正式 PR、merge、merge 後整合驗收。依賴方僅能在 blocker 已 merge 並於 `main` 驗證後開工；只有林于喬明確核准 stable contract exception（穩定介面例外）才可提前。

## 風險、變更與審查

高風險包括：題意、演算法結果、共用資料 contract、核心架構、跨 child 整合、主要 demo。高風險由林于喬加上咏宸（與領域相關時）審查。低風險為隔離的文案／樣式／小型測試／明確修復，由指定人類 reviewer 審查。

- 已承諾行為有誤，或漏了內部工作：在原 child PRD 下新增 Ticket。
- 規格模糊或矛盾：停止並澄清；咏宸負責領域核對。
- 真正新增、可獨立交付的成果：林于喬建立新 child PRD 與其 Tickets。

## 檢查、故障備援與驗證演練

本地檢查一律必要。CI（持續整合，自動檢查）僅在演練後且使用者明確同意才導入；CD（持續部署）不是預設。

GitHub 故障時，暫以每張 Ticket 一個本地檔案記錄，沿用相同識別與欄位；服務恢復後同步一次。不得讓本地與 GitHub 維持兩份分歧真相。

正式開始前，四人須在獨立、可丟棄的非比賽 toy／sandbox repo，以小型假 child PRD 走完 spec、Tickets、認領、sessions、Draft PR、review、merge、demo，並記錄時間與 blocker；不得在本 planning repo 或 future competition implementation repo 演練。演練不得含比賽 product code、可重用 implementation、RL pipeline、Agent loop 或 reward logic。competition implementation workflow 仍須待官方 coding window 開始。此演練與全員角色確認尚未完成，不可宣稱四人已確認。

整體完成條件：所有 child PRD 都在同一份已整合的 `main` 為 Done、測試通過、demo 可用、fallback 已準備；林于喬核准整合，咏宸確認領域正確性。
