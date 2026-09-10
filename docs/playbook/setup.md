# 賽前安裝與檢查

## 每台電腦都要完成

- [ ] 可登入自己的 GitHub 帳號。
- [ ] Git 可執行，並已設定自己的姓名與 email。
- [ ] Codex CLI 可登入自己的帳號並完成一個無敏感資料的測試請求。
- [ ] Python 及團隊選定的 package manager 可執行。
- [ ] 能 clone 練習 repo、建立 branch、commit、push 和建立 Pull Request。
- [ ] 團隊選定並完成驗證後，依 [Matt Pocock 官方安裝說明](https://github.com/mattpocock/skills)安裝需要的 skills。
- [ ] 團隊完成驗證並凍結 manifest 後，記錄其來源 commit；凍結至比賽結束前不自行更新。
- [ ] 未把任何 secrets 寫入 shell history、repo 或共用文件。

## Skills 版本紀錄

團隊完成驗證並決定凍結後填寫：

| Skill | 用途 | 來源 commit | 四人驗證 |
|---|---|---|---|
| `setup-matt-pocock-skills` | repo 初始設定 | 待填 | ☐ |
| `grill-with-docs` | 需求對齊與文件 | 待填 | ☐ |
| `tdd` | 測試先行開發 | 待填 | ☐ |
| `diagnosing-bugs` | 系統化除錯 | 待填 | ☐ |
| `code-review` | 修改檢查 | 待填 | ☐ |
| 規格／issue skill | 題目確認後選定 | 待填 | ☐ |

## A6000 遠端環境

不要在文件中保存 SSH private key 或密碼。每位需要操作 GPU 的成員應各自完成：

- [ ] 從比賽場地可能使用的網路環境測試 SSH。
- [ ] 確認兩張 RTX A6000 可被系統辨識。
- [ ] 能啟動、查看及停止一個無關參賽題目的測試工作。
- [ ] 知道 logs、outputs 與 checkpoints 存放位置。
- [ ] 知道 GPU 忙碌、連線中斷與磁碟不足時要通知誰。
- [ ] 確認個人 OpenAI 憑證沒有留在共用主機。
