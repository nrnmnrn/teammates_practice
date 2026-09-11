# build-handoff_final 兩階段 Session

本目錄提供兩個相互隔離的 Agent Session。第一個 Session 建立候選 handoff；第二個 Session 完成後，才判斷候選是否可取代現有 `build-handoff/`。

## 執行順序

1. 開啟新的 Session，貼上 [`01-create-prompt.md`](01-create-prompt.md) 全文。
2. 等候該 Session 完成，確認 `build-handoff_final/` 已建立且 `git diff --check` 通過。
3. 結束第一個 Session。
4. 開啟另一個新的 Session，貼上 [`02-validate-prompt.md`](02-validate-prompt.md) 全文。
5. 依驗收報告決定是否需要另開修正 Session。

兩個 Session 不可同時執行。驗收 Session 全程唯讀，不得順手修正第一個 Session 的成果。

## 共用資料

兩個提示詞都會要求 Agent 完整閱讀 [`DECISIONS.md`](DECISIONS.md)。該檔只整理已確認的執行決策及未決邊界，不取代總 PRD、子 PRD 或正式流程文件。

## 預期成果

- Session 1：建立 `build-handoff_final/`，保留最新版產品內容及正式工作流程。
- Session 2：提出可追溯的產品保真與流程合規報告，給出是否可取代 `build-handoff/` 的判定。
