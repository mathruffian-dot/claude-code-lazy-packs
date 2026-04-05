---
title: 'Claude Code 懶人包 #02：連接 GitHub'
date: '2026-04-04'
type: 懶人包
version: v0.2
status: 初版（實作後更新）
tags:
  - Claude-Code
  - 懶人包
  - GitHub
  - GitHub-Pages
video: EP04
---
# Claude Code 懶人包 #02：連接 GitHub

> 版本：v0.2
> 更新日期：2026-04-04
> 對應影片：Claude基本功 EP04

> 📌 **本懶人包可獨立執行**：會自動檢查並安裝所需工具，不需要先看過其他懶人包。你只要確認下方「先備條件」即可開始。

---

## 這個懶人包會幫你做什麼？

讓你的 Claude Code 桌面版能夠直接操控 GitHub，包括：
- 建立 GitHub 儲存庫（repo）
- 把 Claude 做好的網頁、遊戲、教材推送上線
- 開啟 GitHub Pages，讓學生掃 QR Code 就能開啟
- 以後每次做好新教材，一句話就能上線

---

## 先備條件

在使用這個懶人包之前，請確認：

- [ ] Claude Code 桌面版已安裝且能正常使用（Pro 方案以上）
- [ ] 有可用的 email 信箱（如果還沒 GitHub 帳號，懶人包會引導你註冊）
- [ ] 電腦有網路連線

---

## 請 Claude Code 幫我執行以下步驟

> ⚠️ 以下內容是給 Claude Code 讀的操作指令。
> 你只需要把這整份 MD 檔丟給 Claude Code 桌面版的 Code 分頁，它會自動開始執行。
> 遇到需要你手動操作的地方，它會暫停並告訴你該怎麼做。
>
> **所有安裝與設定都在 Claude Code 桌面版內完成，不需要另外打開 PowerShell 或命令提示字元。**
> 如果 Claude Code 桌面版無法執行某個指令，才會引導你到終端機操作。
> 進階使用者也可以直接使用 Claude Code CLI 版本來執行本懶人包。

---

### 步驟零：環境檢查

> 請 Claude 在開始前，先自動確認以下所有項目。
> 如果有任何一項不符合，請先告知使用者問題所在，並引導解決後再繼續。
> **不要跳過任何一項檢查，不要假設環境正常。**

1. **確認作業系統**：執行系統指令確認是 Windows / macOS / Linux，後續所有指令請根據實際的作業系統選擇正確版本執行
2. **確認網路連線正常**：嘗試 ping 一個外部網站
3. **檢查 Git 是否已安裝**：執行 `git --version`，如果未安裝：
   - Windows：`winget install --id Git.Git --accept-source-agreements --accept-package-agreements`
   - macOS：`xcode-select --install`
   - Linux：`sudo apt update && sudo apt install git -y`
4. **檢查 GitHub CLI 是否已安裝**：執行 `gh --version`，如果未安裝：
   - Windows：`winget install --id GitHub.cli --accept-source-agreements --accept-package-agreements`
   - macOS：`brew install gh`
5. **檢查 Git 使用者資訊是否已設定**：執行 `git config --global user.name`

> 全部通過後，告知使用者：「環境檢查完成，所有條件都符合，開始設定。」
> 如果有不通過的項目，列出問題清單並逐一引導解決。
> 安裝完工具後如果指令找不到，請提醒使用者可能需要重啟 Claude Code 桌面版。

---

### 步驟零.五：確認使用者有 GitHub 帳號（如果還沒有,幫忙註冊）

> 🖐️ **需要手動操作**：請 Claude **先詢問使用者**：「你有 GitHub 帳號了嗎?」
>
> - **已有帳號** → 跳到步驟一
> - **還沒有** → 請 Claude 依照以下流程引導使用者註冊（約 3 分鐘）

#### 註冊 GitHub 帳號引導流程

**Step 1：打開註冊頁面**

請使用者在瀏覽器開啟：https://github.com/signup

**Step 2：填寫註冊資料（Claude 逐步引導）**

依序詢問使用者並提醒填寫：

| 欄位 | 說明 | 老師建議 |
|------|------|---------|
| **Email** | 常用的 email | 用學校 email 或個人常用 email |
| **Password** | 至少 15 字元 或 8 字元含數字+小寫 | 建議寫下來或存到密碼管理器 |
| **Username** | 全站唯一,之後網址會用到 | **建議用英文+數字,不要中文**。例如 `mathteacher2026`、`wang-math` |
| **Email preferences** | 是否接收電子報 | 可以取消勾選（N） |

> ⚠️ **Username 一旦決定就不好改**（GitHub Pages 網址會變），請慎重。
> 建議用「科目+姓名」或「暱稱」這種容易記的英文。

**Step 3：通過驗證**

- GitHub 會出現一個拼圖驗證（verify your account）
- 請使用者完成拼圖
- 完成後點「Create account」

**Step 4：Email 驗證**

- GitHub 會寄驗證碼到使用者 email
- 請使用者打開 email,複製 8 位數驗證碼
- 回到 GitHub 頁面貼上驗證碼

**Step 5：基本設定（可跳過）**

- 會出現「How many team members?」等問卷 → **可以直接捲到最下方點 Skip 跳過**
- 選擇 **Free 方案**（免費即可,不要選付費）

**Step 6：確認註冊成功**

- 登入後會看到 GitHub 首頁（Dashboard）
- 告知使用者：「✅ GitHub 帳號註冊完成!接下來幫你連接 Claude Code。」

> 🖐️ **常見註冊問題**：
> | 問題 | 解法 |
> |------|------|
> | Username 被搶走了 | 在後面加數字或連字號,例如 `wang-math-2026` |
> | 沒收到驗證信 | 檢查垃圾郵件匣,或點「Resend code」重寄 |
> | 拼圖驗證一直失敗 | 重新整理頁面再試一次 |
> | 想改 username | 到 Settings → Account → Change username(但會影響既有連結) |

---

### 步驟一：登入 GitHub

檢查是否已登入：
```bash
gh auth status
```

如果尚未登入，請執行：
```bash
gh auth login --web --git-protocol https
```

> 🖐️ **需要手動操作**：
> 1. 終端機會顯示一組驗證碼（例如 ABCD-1234）
> 2. 請告知使用者這組驗證碼
> 3. 瀏覽器會自動開啟 GitHub 授權頁面
> 4. 如果瀏覽器沒有自動開啟，請使用者手動開啟 https://github.com/login/device
> 5. 使用者輸入驗證碼並點擊「Authorize」
> 6. 授權成功後，終端機會顯示「Logged in as [使用者名稱]」

登入成功後確認：
```bash
gh auth status
```

---

### 步驟二：設定 Git 使用者資訊

檢查是否已有設定：
```bash
git config --global user.name
git config --global user.email
```

如果沒有設定：

> 🖐️ **需要手動操作**：請詢問使用者他想顯示的姓名和 email。

```bash
git config --global user.name "使用者的姓名"
git config --global user.email "使用者的email"
```

---

### 步驟三：建立第一個測試 repo 並驗證

為了確認一切正常，建立一個測試用的 repo：

1. 在使用者的文件資料夾下建立一個臨時資料夾 `github-test`
2. 初始化 Git repo
3. 建立一個簡單的 `index.html`，內容為「Hello！GitHub 連接成功！」
4. 推到 GitHub（repo 名稱：`github-test`，公開）
5. 開啟 GitHub Pages（使用 main branch）

```bash
gh repo create github-test --public --source=. --push
gh api repos/{owner}/github-test/pages -X POST -f build_type=workflow -f source.branch=main -f source.path=/
```

> 等待約 1 分鐘讓 GitHub Pages 部署完成。

6. 取得 GitHub Pages 網址並告知使用者
7. 請使用者在瀏覽器開啟確認

> 🖐️ **需要手動操作**：請使用者開啟瀏覽器確認網頁顯示「Hello！GitHub 連接成功！」

---

### 步驟四：清除測試 repo（可選）

確認成功後，詢問使用者是否要刪除測試 repo：

> 🖐️ **需要手動操作**：詢問使用者「測試成功了！要保留這個測試 repo 還是刪除它？」

如果要刪除：
```bash
gh repo delete github-test --yes
```

並刪除本地的 `github-test` 資料夾。

告知使用者：「✅ 全部完成！你的 Claude Code 已成功連接 GitHub。以後只要說『幫我推到 GitHub 上線』，Claude 就會自動幫你做好。」

---

## 完成！接下來你可以這樣用

連接成功後，你隨時可以在 Claude Code 裡用自然語言操控 GitHub：

| 你說的話 | Claude + GitHub 會做的事 |
|----------|------------------------|
| 「幫我做一個互動遊戲並推到 GitHub 上線」 | 產生 HTML → 建 repo → 推送 → 開啟 Pages → 給你網址 |
| 「幫我更新這個網頁的內容」 | 修改檔案 → 推送 → 自動更新 |
| 「幫我把這個網頁的 QR Code 產生出來」 | 產生 QR Code 圖片 |
| 「幫我把這個 repo 刪除」 | 刪除 GitHub 上的 repo |

---

## 如果安裝失敗，如何重來

對 Claude Code 說：
「GitHub 懶人包執行失敗了，幫我檢查哪裡出問題，重新處理。」

Claude 會自動：
1. 重新執行環境檢查
2. 確認 Git 和 GitHub CLI 是否正常
3. 確認 GitHub 登入狀態
4. 找出問題並修復

如果需要完全重置 GitHub CLI 登入：
```bash
gh auth logout
gh auth login --web --git-protocol https
```

---

## 常見問題

| 問題 | 解法 |
|------|------|
| `gh: command not found` | 重啟 Claude Code 桌面版，或確認安裝路徑已加入 PATH |
| `git: command not found` | 重啟 Claude Code 桌面版，Windows 需確認 Git for Windows 已安裝 |
| gh auth login 瀏覽器沒開啟 | 手動開啟 https://github.com/login/device 並輸入驗證碼 |
| GitHub Pages 網頁顯示 404 | 等待 1-2 分鐘再重新整理，Pages 部署需要時間 |
| push 被拒絕 | 確認 `gh auth status` 有顯示 `repo` 權限 |
| （實作後持續補充） | |

---

## 更新紀錄

| 日期 | 版本 | 更新內容 |
|------|------|---------|
| 2026-04-04 | v0.1 | 初版 |
| 2026-04-04 | v0.2 | 加入環境檢查、復原機制、跨平台支援、測試 repo 驗證 |

---

## 相關連結

- [[00-環境建置|懶人包 #00：環境建置]]
- [[01-連接 NotebookLM|懶人包 #01：連接 NotebookLM]]
- [[Claude基本功EP05 - GitHub懶人包與教學網頁上線]]
- [[README|Claude Code 懶人包索引]]
