# build-handoff_final 兩階段 Session 提示詞紀錄

## 本輪決定

- 同意用兩個彼此隔離的 Session 處理 `build-handoff_final/`。
- Session 1 建立候選 `build-handoff_final/`。
- Session 2 在 Session 1 完成後，唯讀驗收產品與流程保真。
- 兩個 Session 不同時執行。
- 使用 Sub-agent 分流，但避免多人重複閱讀相同來源。
- 先盤點全部檔案，再只完整閱讀權威來源、實際採用來源、疑似衝突及可能有獨有需求的檔案。
- 建立工作可解決有唯一證據答案的問題；會改變產品、驗收、安全或人員權責者保留為「尚待決定」。
- 驗收 Session 不順手修正；若不合格，只提出最低修正。

## 本輪建立的執行文件

- `docs/operations/build-handoff-final-sessions/README.md`
- `docs/operations/build-handoff-final-sessions/DECISIONS.md`
- `docs/operations/build-handoff-final-sessions/01-create-prompt.md`
- `docs/operations/build-handoff-final-sessions/02-validate-prompt.md`

## 設計理由

共用決策集中在 `DECISIONS.md`，兩份提示詞不重複保存同一批規則。這可降低 token 消耗及日後內容分歧。提示詞依步驟設定完成條件，讓 Agent 先證明盤點與權威判斷完成，再進入建立或驗收。

Session 1 將來源盤點與檔案撰寫分階段。唯讀 worker 分別處理舊流程 handoff 與最新版產品 handoff；writer 只處理互不重疊的檔案集合。主 Agent 保留權威判斷、子 PRD 邊界及跨文件整合責任。

Session 2 將產品保真與流程保真分成兩條獨立驗收軸。主 Agent 複核阻斷性證據並合併結論。只有兩軸發生衝突時才加入第三位 reviewer，避免固定支付重複閱讀成本。

## 尚未執行

- 尚未建立 `build-handoff_final/`。
- 尚未執行產品或流程驗收。
- 尚未決定 final 是否可取代 `build-handoff/`。
- 尚未建立 GitHub Issue 或 Ticket。
