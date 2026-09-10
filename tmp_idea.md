> 此檔案照著 docs/playbook/workflow.md 跟 docs/operations/team-work-allocation.md 的流程為大綱進行設計

# 暫定待做項目

待創建skills:
 - 創建子 PRD 用：但可能有大需求跟小需求，大需求可能是多個子 PRD 組成，小需求可能就少數或單個，這部分還不確定該如何執行
 - 承接子 PRD 任務時用：選擇確認的 
 - 承接子 PRD 任務方發生有疑慮的錯誤或問題時，交接給于喬的 agent 解決用：產生資訊，讓于喬的 agent 接收後處理，還不確定如何傳遞資訊，也許也可以用群組的社交軟體直接發送訊息
 - 于喬的 agent 解決其他人遇到的錯誤或問題用：此技能需要接收到訊息後，找出精準的解決方法，不能影響到其他子 PRD 的開發，或是跟其他子 PRD 功能有重疊。若要解決但一定會影響到的話，這部分先不處理，因為流程一定很複雜，先假設不會遇到，到比賽時遇到在講，現在也沒時間開發這詳細的功能



AGENTS.md（repo端）：
 - 在執行承接的子 PRD 任務時，導入操作手冊文件，目的是其他組員不熟悉 vibe coding 操作流程，這個文件可以把遇到什麼問題用相對應正確的skills解決
 - 提及 CONTEXT.md, codebase 架構檔
 - 子PRD建立在 github 父issue 上；ticket 建立在 github 子 issue 上
 - 環境為 uv


承接子 PRD 操作手冊文件：
使用者提出需求後；或是 tickets 狀態卡住，無法通過的時候 -> $caveman:investigate-first （避免有缺失資訊的問題，這是最容易發生的錯誤；以及找出 tickets 卡住的原因）
執行、修改、創建、刪除、$implement 等操作 -> $caveman:cavecrew （使用 caveman 團隊執行比較能減少 token 消耗，又能增加正確性）
遇到程式錯誤 -> $diagnosing-bugs
執行完 $task-closeout -> 問使用者是否執行 $handoff（用於傳遞給下一個 session 的 agent，使其對上一輪的進度有所了解）
當使用者對某些概念不清楚或是看不懂的時候 -> 用 ELI5 的方式詳細回覆使用者

子PRD的命名是否需要跟領域相關？例如 Simulator-....md，因為我們一開始有分配領域工作，相關的可以持續進行，而且一看就知道跟什麼相關
子PRD是否集中放在某個資料夾底下，然後該資料夾要有導覽，知道什麼子PRD跟什麼功能相關，或是跟其他子PRD相關，以及判斷是否有順序執行關係，避免其他人拿了不該先做的子PRD進行開發



# 待做項目完成後
咏宸已完成 Gradio UI，所以我認為：
咏宸先把目前完成的 Gradio UI 帶進新 repo，並確認它是否符合 PRD.md （總PRD）、與驗收條件；確認後，合併到 main，我審查通過後再建立 UI 相關的子 PRD。

```
如果大家沒有問題，我會先開一個新的開發 repo，建立以下共同文件：

- 總 PRD：整個產品要完成什麼。
- 驗收條件：怎樣才算功能真的完成。
- AGENTS.md（repo端的）：讓開發 agent 共同遵守的規則。
- Codebase 架構：資料、模組與介面的共同約定。
- 團隊 skills：適合我們流程的 skills。會和上次開會提及的 task-closeout skill 一樣，安裝到各自的全域
skills 中，讓比賽時不用再在 repo 內安裝。

接著，

然後我會依總 PRD 與目前可用的基礎，建立其他四個範圍小、彼此盡量沒有依賴的子 PRD，讓四個人各自認領一個，包含我自己也會完整跑一次「拆 tickets → 逐張 implement → task-closeout → 子 PRD 驗收 → PR 審查與整合」的流程。

這一輪的目的不是盡快做出大量功能，而是確認每個人都能使用同一套流程完成一個小功能。若流程、文件、skills 或整合方式出現問題，就先修正，再擴大建立與認領後續子 PRD。
```

# 非必需，視情況決定
創建比賽時所需 agents (.toml)
創建 mermaid 流程圖說明，讓組員知道該如何進行
創建 github CI