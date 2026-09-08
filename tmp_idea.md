# 環境配置
 - 將 docs/playbook/files_for_teamates/ 內我調整好的 AGENTS.md 複製到 ~/.codex/AGENTS.md (全域 AGENTS.md)；task-closeout 複製到 ~/.codex/skills/task-closeout 底下
 - 安裝各個開發用的 skills，如caveman, matt pocock skills, context mode, context7
 - 確定自己的 skills 或 plugins 沒有太多此競賽用不到導致浪費 context window 的問題


# 使用 codex cli 而不是 chatgpt desktop ui
Codex CLI 的 bug 比起 chatgpt desktop ui 來得少。之前遇過某個 mcp 在 CLI 端才能啟用，但是在 UI 端卻失效

# /compact
在一個任務完成或是長對話後，執行 /compact。

# $task-closeout
在一個任務完成後，先執行 /compact ，再執行 $task-closeout

# /quit
建議在做完該session任務後執行 /quit，然後開新的 terminal 分頁輸入 'codex' 執行新的任務。
創建新的分頁執行可以減少該分頁的歷史紀錄太長問題，要回去看歷史紀錄會往上滑很久
執行完該session要執行 /quit 是為了避免有進程佔用問題，如果直接關掉該分頁有可能不會關掉該進程，之後要 resume 有可能遇上該進程仍在執行的 bug，所以 /quit 是結束一個 session 任務最保險的做法

# /fork
當需要在同個歷史 context 下執行不同任務時用的
/fork 完會給 session ID （例如：01a0800e-2166-7e60-b542-d1ffddfb686d），複製下來並貼上新的 terminal 分頁
該 session /fork 完後要馬上 /quit，不然會被父進程佔據而無法開啟/fork，這算是codex沒處理好的地方
/quit 完會生成復原指令（例如：codex resume 01a08010-1d6c-7940-a670-0696d166f493）
開兩個新的 terminal 分頁：一個分頁輸入 /fork 復原指令 (codex resume 01a0800e-2166-7e60-b542-d1ffddfb686d)；一個分頁輸入 /quit 復原指令 (codex resume 01a08010-1d6c-7940-a670-0696d166f493)
這樣才能基於同個歷史context分開開發