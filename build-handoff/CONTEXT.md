# 排程展示詞彙

本檔只定義領域名詞；權限、流程與實作細節見 INDEXER 指向的文件。

## 協作

**總 PRD**：全產品需求與共同接受條件。

**sub-PRD**：一項完整功能的規格，可為草稿或已核可版本。

**Ticket**：一次工作 session 可交付且可獨立驗證的最小工作。

**owner**：承接一份 sub-PRD、統整其交付結果的負責人。

**功能依賴**：一項 sub-PRD 須待另一功能可用，方能交付的關係。

## 排程

**Job**：一筆可由 Worker 排程的訂單。PRD 既有的 `Request` 只是同義舊稱。

**Run**：自初始化或 reset 起至下次 reset 前的一局模擬。

**Policy**：決定下一筆 Job 的排程規則。

**產品 Skill**：技能庫中可查看、選擇與套用某個 Policy 的項目。

**agent 工作 skill**：協助 agent 執行文件、診斷或交接工作的工具；不是產品 Skill，也不參與排程。

**Worker**：每次只處理一筆 Job 且不搶占的唯一處理者。

## 資料來源

**team backend**：隊友提供的排程後端。

**local backend**：本機模擬排程的後端。

## 模擬提案

**Mock proposal**：預寫的 Policy 候選，不是資料來源；接受它不表示 AI 或 evaluator 已證明改善。
