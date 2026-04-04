---
title: 'Claude Code 懶人包 #01：連接 NotebookLM'
date: '2026-04-04'
type: 懶人包
version: v0.2
status: 初版（實作後更新）
tags:
  - Claude-Code
  - 懶人包
  - NotebookLM
  - MCP
video: EP03
---
# Claude Code 懶人包 #01：連接 Google NotebookLM

> 版本：v0.2（加入環境檢查、復原機制、跨平台支援）
> 更新日期：2026-04-04
> 對應影片：Claude基本功 EP03

> 📌 **本懶人包可獨立執行**：會自動檢查並安裝所需工具，不需要先看過其他懶人包。你只要確認下方「先備條件」即可開始。

---

## 這個懶人包會幫你做什麼？

讓你的 Claude Code 桌面版能夠直接操控 Google NotebookLM，包括：
- 建立 notebook、上傳資料來源
- 產生教學簡報（Slide Deck）
- 產生資訊圖表（Infographic）
- 所有成品自動下載到電腦裡的指定資料夾

---

## 原理說明：這個懶人包在做什麼？

**三角關係圖**：

```
Claude Code ←(MCP 協定)→ nlm (翻譯官) ←(Google 登入)→ NotebookLM
```

這個懶人包會在你的電腦裡裝一個叫 `nlm` 的「翻譯官」，讓 Claude Code 能透過它去操控 NotebookLM。

- **為什麼需要翻譯官？**
  NotebookLM 沒有官方 API，Google 沒開放程式直接呼叫。`nlm` 是用「模擬瀏覽器操作」的方式，假裝是你在點網頁。

- **什麼是 MCP？**
  MCP（Model Context Protocol）是 Claude 跟外部工具溝通的標準接口，就像手機的 USB-C——只要工具支援 MCP，Claude 就能插上去用。

- **為什麼要登入 Google？**
  `nlm` 需要你的 Google 通行證，才能幫你去操作 NotebookLM。

**一句話記住**：你跟 Claude 講中文，Claude 叫 `nlm` 去幫你點 NotebookLM 的網頁，成品自動下載到你電腦的資料夾。

---

## 先備條件

在使用這個懶人包之前，請確認：

- [ ] Claude Code 桌面版已安裝且能正常使用（Pro 方案以上）
- [ ] 你有 Google 帳號（用來登入 NotebookLM）
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

> 請 Claude 在開始安裝前，先自動確認以下所有項目。
> 如果有任何一項不符合，請先告知使用者問題所在，並引導解決後再繼續。
> **不要跳過任何一項檢查，不要假設環境正常。**

1. **確認作業系統**：執行系統指令確認是 Windows / macOS / Linux，後續所有指令請根據實際的作業系統選擇正確版本執行
2. **檢查 Git 是否已安裝**：執行 `git --version`，如果未安裝：
   - Windows：`winget install --id Git.Git --accept-source-agreements --accept-package-agreements`
   - macOS：`xcode-select --install`
   - Linux：`sudo apt update && sudo apt install git -y`
3. **確認 uv 已安裝**：執行 `uv --version`，如果未安裝，步驟一會自動安裝
4. **確認網路連線正常**：嘗試 ping 一個外部網站
5. **確認 Claude Code 的版本**：確保是最新版本

> 全部通過後，告知使用者：「環境檢查完成，所有條件都符合，開始安裝。」
> 如果有不通過的項目，列出問題清單並逐一引導解決。

---

### 步驟一：安裝 NotebookLM MCP CLI 工具

如果步驟零確認 `uv` 尚未安裝，請先安裝：

**Windows（PowerShell）**：
```bash
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

**macOS / Linux**：
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

安裝完 uv 後（或已有 uv），安裝 NotebookLM MCP CLI：

```bash
uv tool install notebooklm-mcp-cli
```

安裝完成後，確認指令可用：

```bash
nlm --version
```

> 如果出現 `nlm: command not found`，請重開終端機再試一次。
> 如果仍然失敗，請告知使用者可能需要手動將 uv 的路徑加入系統 PATH。

---

### 步驟二：登入 Google 帳號

請執行以下指令，會自動開啟瀏覽器讓使用者登入 Google：

```bash
nlm login
```

> 🖐️ **需要手動操作**：瀏覽器會開啟 Google 登入頁面，請使用者登入他的 Google 帳號。登入成功後，CLI 會自動擷取認證資訊。

登入成功後，確認狀態：

```bash
nlm doctor
```

> 如果 `nlm doctor` 顯示未認證，請重新執行 `nlm login`。
> 如果瀏覽器沒有自動開啟，請手動開啟瀏覽器到 Google 登入頁面，登入後再回到終端機。

---

### 步驟三：設定 Claude Code 的 MCP 連接

請執行以下指令，自動完成 MCP 設定（不需要手動編輯 JSON）：

```bash
nlm setup add claude-code
```

確認設定成功：

```bash
nlm setup list
```

> 應該能看到 `claude-code` 出現在已設定的客戶端清單中。

---

### 步驟四：建立本地資料夾

請根據使用者的作業系統，在文件資料夾下建立以下目錄結構：

```
Documents/
  └── NotebookLM/
      ├── slides/          ← 簡報（Slide Deck，可匯出 .pptx）
      ├── infographics/    ← 資訊圖表（多種風格可選）
      ├── audio/           ← 音訊概覽（Audio Overview）
      ├── video/           ← 影片概覽（Video Overview，含 Cinematic / Explainer / Brief）
      ├── docs/            ← Google 文件匯出（Reports、Notes 等）
      ├── sheets/          ← Google 試算表匯出（Data Tables）
      ├── mindmaps/        ← 心智圖（Mind Map）
      └── quizzes/         ← 測驗與閃卡（Quiz、Flashcards）
```

建立完成後，告知使用者資料夾的完整路徑。

---

### 步驟五：重啟 Claude Code 並驗證連接

> 🖐️ **需要手動操作**：請使用者完全關閉 Claude Code 桌面版，然後重新開啟。

重新開啟後，測試 NotebookLM 連接是否成功：
1. 嘗試列出使用者的 NotebookLM 筆記本清單
2. 如果成功顯示清單（即使是空的），代表連接成功

---

### 步驟六：功能測試

連接成功後，請執行一個簡單的測試：
1. 在 NotebookLM 中建立一個新的 notebook，名稱為「測試筆記本」
2. 確認建立成功
3. 建立成功後，刪除這個測試筆記本
4. 告知使用者：「✅ 全部完成！你的 Claude Code 已成功連接 NotebookLM。」

---

## 完成！接下來你可以這樣用

連接成功後，你隨時可以在 Claude Code 裡用自然語言操控 NotebookLM：

| 你說的話 | NotebookLM 會做的事 | 存放位置 |
|----------|-------------------|---------|
| 「幫我用這份 PDF 建一個 notebook」 | 建立 notebook + 上傳 PDF 作為資料來源 | — |
| 「幫我產生教學簡報」 | 生成 Slide Deck（可匯出 .pptx） | slides/ |
| 「幫我做一張資訊圖表」 | 生成 Infographic（多種風格可選） | infographics/ |
| 「幫我產生音訊概覽（Podcast）」 | 生成 Audio Overview | audio/ |
| 「幫我產生影片概覽」 | 生成 Video Overview（Cinematic / Explainer / Brief） | video/ |
| 「幫我產生報告並匯出成文件」 | 生成 Report，匯出為 Google Docs | docs/ |
| 「幫我做數據表格並匯出試算表」 | 生成 Data Table，匯出為 Google Sheets | sheets/ |
| 「幫我產生心智圖」 | 生成 Mind Map | mindmaps/ |
| 「幫我出測驗題 / 閃卡」 | 生成 Quiz / Flashcards | quizzes/ |

---

## 如果安裝失敗，如何重來

> 如果執行過程中遇到錯誤，不用緊張。
> 對 Claude Code 說以下這段話，它會幫你清除之前的設定，從頭再來：

**請對 Claude Code 說：**
「上次 NotebookLM 懶人包執行失敗了，幫我清除之前的設定，重新跑一次。」

**Claude 會自動執行以下復原步驟：**

1. 移除 MCP 設定：`nlm setup remove claude-code`
2. 移除 notebooklm-mcp-cli：`uv tool uninstall notebooklm-mcp-cli`
3. 清除登入狀態：`nlm logout`
4. 確認環境乾淨後，從步驟零重新開始

---

## 常見問題

| 問題 | 解法 |
|------|------|
| `nlm: command not found` | 重開終端機，或確認 uv 安裝路徑已加入 PATH |
| `uv: command not found` | Windows 需重開 PowerShell；macOS/Linux 需執行 `source ~/.bashrc` 或 `source ~/.zshrc` |
| 登入後 `nlm doctor` 顯示未認證 | 重新執行 `nlm login`，確認瀏覽器登入成功 |
| 瀏覽器沒有自動開啟 | 手動開啟瀏覽器登入 Google，或嘗試 `nlm login --manual` |
| Claude Code 看不到 NotebookLM 工具 | 確認有執行 `nlm setup add claude-code`，並完全關閉再重啟 Claude Code |
| Windows 上指令格式錯誤 | 確認使用 PowerShell 而非 CMD，或改用 Git Bash |
| （實作後持續補充） | |

---

## 更新紀錄

| 日期 | 版本 | 更新內容 |
|------|------|---------|
| 2026-04-04 | v0.1 | 初版，根據官方文件建立基本流程 |
| 2026-04-04 | v0.2 | 加入環境檢查、復原機制、跨平台支援、常見問題擴充 |
| | v0.3（預定） | 加入影片實作過程中遇到的問題與解法 |
| | v1.0（預定） | 經多人實測後的穩定版本 |

---

## 相關連結

- [notebooklm-mcp-cli GitHub](https://github.com/jacob-bd/notebooklm-mcp-cli)
- [NotebookLM Downloader MCP](https://lobehub.com/mcp/pmane-notebooklm-downloader)
- [[Claude基本功EP03 - 懶人包先備工作]]
- [[README|Claude Code 懶人包索引]]
