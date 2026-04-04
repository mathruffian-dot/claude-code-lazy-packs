---
title: 'Claude Code 懶人包 #04：連接 Supabase 資料庫'
date: '2026-04-04'
type: 懶人包
version: v0.2
status: 初版（實作後更新）
tags:
  - Claude-Code
  - 懶人包
  - Supabase
  - 資料庫
  - MCP
video: EP08
---
# Claude Code 懶人包 #04：連接 Supabase 資料庫

> 版本：v0.2
> 更新日期：2026-04-04
> 對應影片：Claude基本功 EP08

> 📌 **本懶人包可獨立執行**：會自動檢查並安裝所需工具，不需要先看過其他懶人包。你只要確認下方「先備條件」即可開始。

---

## 這個懶人包會幫你做什麼？

讓你的 Claude Code 桌面版能夠直接操控 Supabase 雲端資料庫，包括：
- 用自然語言建立資料表（不需要學 SQL）
- 新增、查詢、修改、刪除資料
- 讓你做的網頁工具能「記住」資料（關掉瀏覽器再開，資料還在）
- 支援多人同時使用同一份資料

---

## 先備條件

在使用這個懶人包之前，請確認：

- [ ] Claude Code 桌面版已安裝且能正常使用（Pro 方案以上）
- [ ] 已有 GitHub 帳號（Supabase 可用 GitHub 登入）
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

## 階段一：建立 Supabase 帳號與專案

### 步驟零：環境檢查

> 請 Claude 在開始前，先自動確認以下所有項目。
> 如果有任何一項不符合，請先告知使用者問題所在，並引導解決後再繼續。
> **不要跳過任何一項檢查，不要假設環境正常。**

1. **確認作業系統**：執行系統指令確認是 Windows / macOS / Linux，後續所有指令請根據實際的作業系統選擇正確版本執行
2. **確認網路連線正常**
3. **檢查 Node.js 是否已安裝**：執行 `node --version`，如果未安裝：
   - Windows：`winget install --id OpenJS.NodeJS --accept-source-agreements --accept-package-agreements`
   - macOS：`brew install node`
   - Linux：`sudo apt update && sudo apt install nodejs npm -y`
4. **檢查 npx 是否可用**：執行 `npx --version`
5. **檢查 GitHub CLI 是否已登入**：執行 `gh auth status`（Supabase 用 GitHub 帳號登入）

> 全部通過後，告知使用者環境狀態並繼續下一步。

---

### 步驟一：註冊 Supabase 帳號

> 🖐️ **需要手動操作**：
> 1. 請使用者開啟瀏覽器，到 https://supabase.com
> 2. 點擊「Start your project」
> 3. 選擇「Continue with GitHub」用 GitHub 帳號登入
> 4. 授權 Supabase 存取 GitHub 帳號

等使用者確認已登入後，繼續下一步。

---

### 步驟二：建立 Supabase 專案

> 🖐️ **需要手動操作**：
> 1. 在 Supabase Dashboard 中，點擊「New Project」
> 2. 設定以下資訊：
>    - **Project name**：建議用 `my-teaching-tools`（或使用者自訂）
>    - **Database Password**：設定一個密碼（請記住，之後會用到）
>    - **Region**：選擇離你最近的地區（東亞建議選 `Northeast Asia (Tokyo)`）
> 3. 點擊「Create new project」
> 4. 等待 1-2 分鐘，專案建立完成

---

### 步驟三：取得專案憑證

> 🖐️ **需要手動操作**：
> 1. 在 Supabase Dashboard 中，點擊左側「Project Settings」（齒輪圖示）
> 2. 點擊「API」
> 3. 複製以下兩個值：
>    - **Project URL**（格式像 `https://xxxxxxxx.supabase.co`）
>    - **service_role key**（在「Project API keys」區塊，點擊 `service_role` 旁的「Reveal」並複製）
>
> ⚠️ **安全提醒**：service_role key 有完整的資料庫權限，不要分享給他人或放在公開的程式碼中。

請使用者將這兩個值提供給 Claude（貼在對話中即可）。

---

## 階段二：連接 MCP

### 步驟四：安裝 Supabase MCP Server

使用使用者提供的 Project URL 和 service_role key，執行以下指令：

```bash
claude mcp add supabase --scope user -- npx -y @supabase/mcp-server-supabase@latest --supabase-url [使用者的Project URL] --supabase-service-role-key [使用者的service_role_key]
```

> 將 `[使用者的Project URL]` 和 `[使用者的service_role_key]` 替換為使用者提供的實際值。
> 注意：URL 和 key 都不需要加引號。

---

### 步驟五：重啟 Claude Code 並驗證

> 🖐️ **需要手動操作**：請使用者完全關閉 Claude Code 桌面版，然後重新開啟。

重新開啟後，測試 Supabase 連接是否成功：

1. 嘗試查詢資料庫中有哪些資料表（新專案應該是空的，這是正常的）
2. 如果能成功查詢（即使結果是空的），代表連接成功

> 如果連接失敗，請檢查：
> - Project URL 和 service_role key 是否正確
> - 是否有多餘的空格或換行
> - 重新執行步驟四

---

### 步驟六：功能測試

連接成功後，執行一個完整的測試：

1. 建立一個測試資料表：
   - 表名：`test_table`
   - 欄位：`id`（自動遞增）、`name`（文字）、`created_at`（時間戳）

2. 新增一筆測試資料：
   - name: 「Supabase 連接測試成功」

3. 查詢資料，確認能讀取到剛才新增的資料

4. 刪除測試資料表

5. 告知使用者：「連接測試成功！接下來設定自動防暫停。」

---

### 步驟七：設定自動防暫停排程

> Supabase 免費專案閒置一週會自動暫停。設定一個每週自動查詢，就能永遠防止暫停。

請使用 Claude Code 的排程功能，建立一個每週自動執行的任務：

- **任務內容**：查詢 Supabase 資料庫中的 students 表（或任意一個表），確認資料庫回應正常
- **排程頻率**：每週執行一次
- **目的**：防止 Supabase 免費專案因閒置而暫停

設定完成後，告知使用者：
「✅ 全部完成！你的 Claude Code 已成功連接 Supabase 資料庫，並設定了每週自動防暫停。」

---

## 完成！接下來你可以這樣用

| 你說的話 | Claude + Supabase 會做的事 |
|----------|--------------------------|
| 「幫我建一個學生成績的資料表」 | 自動建立資料表 + 設定欄位 |
| 「幫我新增一筆學生成績」 | 插入資料到資料庫 |
| 「幫我查詢全班數學成績的平均」 | 查詢資料庫並計算 |
| 「幫我做一個成績管理網頁，連接資料庫」 | 產生前端 + 連接 Supabase + 資料持久化 |
| 「幫我把這個工具推到 GitHub Pages」 | 上線（搭配 EP04 的 GitHub） |

---

## 如果安裝失敗，如何重來

對 Claude Code 說：
「Supabase 懶人包執行失敗了，幫我檢查哪裡出問題，重新處理。」

Claude 會自動：
1. 檢查 MCP 連接狀態
2. 確認 Project URL 和 key 是否正確
3. 找出問題並修復

如果需要完全重置 MCP 連接：
```bash
claude mcp remove supabase
```
然後從步驟四重新開始。

---

## 常見問題

| 問題 | 解法 |
|------|------|
| `npx: command not found` | 確認 Node.js 已安裝，重啟 Claude Code |
| 連接後查詢失敗 | 確認 service_role key 是否正確（不是 anon key） |
| Supabase 專案顯示「Paused」 | 懶人包已設定每週自動防暫停。如果仍然暫停，到 Dashboard 點「Restore」即可 |
| 建資料表失敗 | 確認專案沒有被暫停，網路連線正常 |
| 不確定哪個是 service_role key | 在 Project Settings → API 中，有兩個 key：用 `service_role`（不是 `anon`） |
| （實作後持續補充） | |

---

## 免費方案說明

| 項目 | 免費額度 | 老師夠用嗎？ |
|------|---------|------------|
| 資料庫儲存 | 500 MB | ✅ 一個班級的成績記錄遠遠不到 |
| 檔案儲存 | 1 GB | ✅ 足夠 |
| 月活用戶 | 50,000 | ✅ 全校師生都夠用 |
| API 請求 | 無限 | ✅ 不用擔心 |
| 專案數量 | 2 個 | ⚠️ 免費只能有 2 個專案 |

> ⚠️ 免費專案閒置一週會自動暫停，重新啟動即可，資料不會消失。

---

## 安全提醒

- **不要在公開的程式碼中放 service_role key**
- 示範時使用假資料（假名、假成績），不要放真實學生個資
- Supabase MCP 適合開發和測試，如果要正式使用需注意隱私法規

---

## 更新紀錄

| 日期 | 版本 | 更新內容 |
|------|------|---------|
| 2026-04-04 | v0.1 | 初版 |
| 2026-04-04 | v0.2 | 加入環境檢查、復原機制、安全提醒、免費方案說明 |

---

## 相關連結

- [Supabase 官網](https://supabase.com)
- [Supabase MCP 官方文件](https://supabase.com/docs/guides/getting-started/mcp)
- [Supabase MCP GitHub](https://github.com/supabase-community/supabase-mcp)
- [[02-連接 GitHub|懶人包 #02：連接 GitHub]]
- [[Claude基本功EP08 - Supabase資料庫懶人包]]
- [[README|Claude Code 懶人包索引]]
