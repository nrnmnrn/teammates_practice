# Hackathon Vibe Coding 操作手冊

這份手冊讓 Vibe Coding 經驗較少的成員也能安全地完成一個小任務。團隊採用 [Matt Pocock 的 skills](https://github.com/mattpocock/skills) 所強調的流程：先對齊需求、保持快速回饋、用測試證明結果、由人檢查 agent 的修改。

## 完成標準

每位成員都必須能在不靠口頭補充的情況下完成：

```text
Issue → branch → 先寫測試 → Codex 實作 → 執行檢查
      → 閱讀 diff → commit → Pull Request → owner 合併
```

若無法完成，也必須能提供指令、完整錯誤訊息、已嘗試方法和目前 diff，讓隊友快速接手。

## 閱讀順序

1. [基本觀念](concepts.md)
2. [賽前安裝與檢查](setup.md)
3. [一個任務的標準流程](workflow.md)
4. [故障與降級處理](troubleshooting.md)
5. 在獨立練習 repo 完成一次完整流程

## 團隊硬規則

- Agent 是協作者，不是最後決策者；提交前一定要看 diff。
- 一次只交給 agent 一個可驗收的小目標。
- 先描述成功條件，再要求寫程式。
- 行為改變以測試先行；不能只靠「看起來有動」。
- 不把錯誤訊息改寫成自己的猜測；完整保留原文。
- API key、SSH private key、密碼與 token 不可貼入聊天、prompt、issue 或 repo。
- 同一問題若兩次嘗試仍無新證據，停止亂試，整理證據後求助。
- 不在開賽前於規劃 repo 或演練 repo 建立可搬用的參賽程式碼。

## 精簡 skills 流程

實際 skill 名稱以 9/8 凍結的 manifest 為準。流程角色如下：

| 階段 | Skill 類型 | 產出 |
|---|---|---|
| 對齊 | `grill-with-docs` | 共同理解、名詞與重要決策 |
| 規格 | product spec／issue 拆分相關 skill | 小而可驗收的任務 |
| 開發 | `tdd` | 失敗測試、最小實作、重構 |
| 除錯 | `diagnosing-bugs` | 有證據的原因與修正 |
| 驗收 | `code-review`、驗證相關 skill | 測試結果、diff 檢查與 review |

不要因為「有很多 skills」就全部使用。每個 skill 都會增加流程與 context；只在它負責的階段呼叫它。

