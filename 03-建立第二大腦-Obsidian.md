---
title: 'Claude Code 懶人包 #03：建立第二大腦（Obsidian）'
date: '2026-04-04'
type: 懶人包
version: v0.5
status: 實測修正版
tags:
  - Claude-Code
  - 懶人包
  - Obsidian
  - MCP
  - 第二大腦
video: EP06
---
# Claude Code 懶人包 #03：建立第二大腦（Obsidian）

> 版本：v0.5
> 更新日期：2026-04-06
> 對應影片：Claude基本功 EP06

> 📌 **本懶人包可獨立執行**：會自動檢查並安裝所需工具，不需要先看過其他懶人包。你只要確認下方「先備條件」即可開始。

---

## 這個懶人包會幫你做什麼？

幫你的 Claude Code 裝上「第二大腦」，完成後 Claude 可以：
- 讀取你 Obsidian 筆記庫裡的所有筆記
- 幫你搜尋、新增、編輯筆記
- 記住你的教學紀錄、素材、想法，變成你的專屬助理

---

## 先備條件

在使用這個懶人包之前，請確認：

- [ ] Claude Code 桌面版已安裝且能正常使用（Pro 方案以上）
- [ ] 已有 Google 帳號（用於 Google Drive 同步）
- [ ] 電腦有網路連線
- [ ] Node.js 已安裝（mcpvault 需要 npx 指令）— 沒裝也沒關係，步驟零會自動安裝

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

## 階段一：安裝 Obsidian 並建立 Vault

### 步驟零：環境檢查

> 請 Claude 在開始前，先自動確認以下所有項目。
> 如果有任何一項不符合，請先告知使用者問題所在，並引導解決後再繼續。
> **不要跳過任何一項檢查，不要假設環境正常。**

1. **確認作業系統**：執行系統指令確認是 Windows / macOS / Linux，後續所有指令請根據實際的作業系統選擇正確版本執行
2. **確認網路連線正常**
3. **檢查 Node.js 是否已安裝**：
   - ⚠️ **Windows 重要提醒**：Claude Code 桌面版的 bash 環境可能找不到 `node`，即使已安裝。請用以下方式檢查：
     - 先嘗試 `node --version`
     - 若失敗，嘗試 `export PATH="/c/Program Files/nodejs:$PATH" && node --version`
     - 若仍失敗，代表未安裝，請安裝：
       - Windows：`winget install --id OpenJS.NodeJS --accept-source-agreements --accept-package-agreements`
       - macOS：`brew install node`
       - Linux：`sudo apt update && sudo apt install nodejs npm -y`
   - 安裝完成後，後續所有 node/npm/npx 指令都需要先加上 `export PATH="/c/Program Files/nodejs:$PATH"`（僅 Windows）
4. **檢查 npx 是否可用**：執行 `npx --version`（Windows 記得加 PATH）
5. **檢查 Google Drive 桌面版是否已安裝**：
   - Windows：確認 `G:\` 或 `C:\Users\[使用者]\Google Drive\` 路徑是否存在
   - macOS：確認 `/Users/[使用者]/Google Drive/` 或 `~/Library/CloudStorage/GoogleDrive-*/` 路徑是否存在
   - 如果沒有安裝 Google Drive 桌面版，告知使用者需要先安裝
6. **檢查 Obsidian 是否已安裝**：嘗試確認 Obsidian 的安裝路徑

> 全部通過後，告知使用者環境狀態。
> 如果有不通過的項目，列出問題清單並逐一引導解決。

---

### 步驟一：安裝 Obsidian（如果未安裝）

> 如果步驟零確認 Obsidian 已安裝，跳過此步驟。

> 🖐️ **需要手動操作**：
> 1. 請使用者開啟瀏覽器，到 https://obsidian.md 下載安裝檔
> 2. 執行安裝檔，按照指示完成安裝
> 3. 安裝完成後先不要開啟 Obsidian，等下一步設定好資料夾再開

---

### 步驟二：安裝 Google Drive 桌面版（如果未安裝）

> 如果步驟零確認 Google Drive 桌面版已安裝，跳過此步驟。

Google Drive 桌面版會在你的電腦上建立一個同步資料夾，Obsidian 的筆記存在這裡就能自動同步到雲端，換電腦也不會遺失。

> 🖐️ **需要手動操作**：請依照以下步驟操作。
>
> **下載與安裝：**
> 1. 開啟瀏覽器，前往 https://www.google.com/drive/download/
> 2. 點擊「下載電腦版雲端硬碟」按鈕
> 3. 執行下載的安裝檔（`GoogleDriveSetup.exe`）
> 4. 安裝過程中按照指示完成（一路點「下一步」即可）
>
> **登入與設定：**
> 5. 安裝完成後會自動開啟登入畫面，用你的 Google 帳號登入
> 6. 登入後，系統匣（右下角）會出現 Google Drive 的雲朵圖示
> 7. 點擊雲朵圖示 → 齒輪 ⚙ → 「偏好設定」
> 8. 確認「Google 雲端硬碟」分頁中的同步方式：
>    - 選擇「串流檔案」（預設，推薦）— 檔案存在雲端，需要時才下載
>    - 或選擇「鏡射檔案」— 所有檔案都下載到本機（佔空間但離線也能用）
>
> **確認同步資料夾位置：**
> 9. 安裝完成後，檔案總管左側會出現「Google Drive」磁碟機（通常是 `G:\`）
>    - Windows：通常是 `G:\我的雲端硬碟\` 或 `G:\My Drive\`
>    - macOS：通常是 `~/Library/CloudStorage/GoogleDrive-你的信箱/My Drive/`
> 10. 等待同步完成（系統匣的雲朵圖示顯示 ✅ 打勾）
>
> 確認能看到 Google Drive 資料夾後，告訴 Claude「完成了」即可繼續。

---

### 步驟三：在 Google Drive 中建立 Vault 資料夾

請在使用者的 Google Drive 資料夾內建立 Obsidian vault 的目錄結構：

> 🖐️ **需要手動操作**：請詢問使用者想要的 vault 名稱（預設建議：`secondbrain`）

根據作業系統，在 Google Drive 資料夾內建立：

```
Google Drive/
  └── [vault名稱]/          ← 這就是 Obsidian 的 vault
      ├── 教學素材/
      ├── 影片筆記/
      ├── 每日筆記/
      └── Templates/
```

建立完成後，記錄 vault 的完整路徑（後續步驟會用到）。

---

### 步驟四：用 Obsidian 開啟 Vault

> 🖐️ **需要手動操作**：
> 1. 請使用者開啟 Obsidian
> 2. 選擇「Open folder as vault」（開啟資料夾作為筆記庫）
> 3. 選擇剛才在 Google Drive 建立的 vault 資料夾
> 4. Obsidian 會開啟並顯示空的筆記庫

確認使用者已成功開啟 vault 後，繼續下一步。

---

## 階段二：連接 MCP（讓 Claude Code 能讀寫筆記）

### 步驟五：安裝 mcpvault MCP Server

mcpvault 是主力 MCP，讓 Claude Code 能搜尋、讀取、編輯你的筆記。
**不需要 Obsidian 開著就能運作。不需要在 Obsidian 中安裝任何外掛。**

> ⚠️ **實測踩坑紀錄**：
> - `claude mcp add` 指令在 Claude Code **桌面版**可能無法使用（找不到 `claude` CLI）
> - 用 `npx` 當 MCP command 啟動時，Windows 環境可能因為 PATH 問題導致 Claude Code 找不到 npx
> - 解法：**先全域安裝 mcpvault，再手動寫入設定檔，使用完整路徑**

#### 5-1：全域安裝 mcpvault

```bash
# Windows（記得加 PATH）
export PATH="/c/Program Files/nodejs:$PATH" && npm install -g @bitbonsai/mcpvault

# macOS / Linux
npm install -g @bitbonsai/mcpvault
```

安裝完成後，確認全域安裝的路徑：
- Windows：通常在 `C:\Users\[使用者]\AppData\Roaming\npm\mcpvault.cmd`
- macOS / Linux：通常在 `/usr/local/bin/mcpvault` 或 `~/.npm-global/bin/mcpvault`

用 `which mcpvault` 或 `where.exe mcpvault` 確認實際路徑。

#### 5-2：寫入 MCP 設定檔

> ⚠️ **重要**：為了確保 Claude Code 能正確載入 MCP，請在**三個位置**都寫入設定。
> 這是因為不同版本的 Claude Code（桌面版、CLI、Web）讀取設定的位置不同。

**位置 1：使用者全域設定** `~/.claude/settings.json`

```json
{
  "mcpServers": {
    "obsidian": {
      "command": "C:\\Users\\[使用者]\\AppData\\Roaming\\npm\\mcpvault.cmd",
      "args": [
        "G:\\我的雲端硬碟\\[vault名稱]"
      ]
    }
  }
}
```

**位置 2：專案設定** `[工作目錄]/.claude/settings.local.json`

（內容同上）

**位置 3：專案根目錄** `[工作目錄]/.mcp.json`

（內容同上）

> 📝 **macOS / Linux 的設定範例**：
> ```json
> {
>   "mcpServers": {
>     "obsidian": {
>       "command": "mcpvault",
>       "args": [
>         "/Users/[使用者]/Library/CloudStorage/GoogleDrive-xxx/My Drive/[vault名稱]"
>       ]
>     }
>   }
> }
> ```
> macOS/Linux 通常不需要完整路徑，直接用 `mcpvault` 即可。

> ⚠️ 路徑中如果有中文或空格，在 JSON 中用雙反斜線跳脫。
> 例如：`"G:\\我的雲端硬碟\\secondbrain"`

---

### 步驟六：重啟 Claude Code 並驗證

> 🖐️ **需要手動操作**：請使用者完全關閉 Claude Code 桌面版，然後重新開啟。
>
> ⚠️ **只需要重啟一次**：如果步驟五的三個設定檔都正確寫入，重啟一次就夠了。

重新開啟後，驗證 MCP 是否成功載入：

#### 驗證方式 1：檢查工具清單
嘗試使用 `mcp__obsidian__get_vault_stats` 或 `mcp__obsidian__list_directory` 工具。
如果能回傳 vault 的資料夾結構，代表連接成功。

#### 驗證方式 2：如果工具不存在
如果找不到 `mcp__obsidian__` 開頭的工具，代表 MCP 未成功載入，請依序排查：
1. 確認 `~/.claude/settings.json` 中的 `command` 路徑是否正確（路徑中的檔案確實存在）
2. 確認 vault 路徑確實存在且可存取
3. 在終端機手動測試 MCP server 是否能啟動：
   ```bash
   echo '{"jsonrpc":"2.0","id":1,"method":"tools/list"}' | mcpvault "[vault路徑]"
   ```
   如果回傳 JSON 工具清單，代表 mcpvault 本身正常，問題在 Claude Code 的設定讀取
4. 確認 `.mcp.json` 在 Claude Code 開啟的工作目錄下
5. 再次重啟 Claude Code

#### 驗證方式 3：新增測試筆記
連接成功後，在 vault 中新增一篇測試筆記：
- 路徑：`測試筆記.md`
- 內容：「這是 Claude Code 透過 MCP 自動建立的筆記。連接成功！」
- 含 frontmatter：title、date

> 全部測試通過後，告知使用者：
> 「✅ 全部完成！Claude Code 已成功連接你的 Obsidian 第二大腦。」

---

## 階段三：建立基礎知識結構

### 步驟七：建立 CLAUDE.md

在 vault 根目錄建立 `CLAUDE.md`，這是 Claude Code 的「班規」，每次對話都會自動讀取。

請詢問使用者以下資訊，用來填入 CLAUDE.md：

> 🖐️ **需要手動操作**：請詢問使用者：
> 1. 你教什麼科目？什麼年級？
> 2. 你希望 Claude 用什麼語言回答？（繁體中文）
> 3. 有沒有其他偏好？（例如：回答要簡潔、要附教學建議等）

根據使用者回答建立 CLAUDE.md，範例結構：

```markdown
# 我的 Obsidian — CLAUDE.md

## 關於我
- 我是[科目][年級]老師
- 這個 vault 是我的教學第二大腦

## 語言偏好
- 所有回應請使用繁體中文

## 工作規則
- 新增筆記時自動加上 frontmatter（title、date、tags）
- 搜尋筆記時優先搜尋教學素材相關內容
```

### 步驟八：建立第一篇正式筆記

幫使用者建立一篇有意義的筆記作為起點，例如：
- 「本學期教學計畫」
- 「我的教學工具清單」
- 或使用者自己想記錄的任何內容

> 🖐️ **需要手動操作**：詢問使用者想建立什麼主題的第一篇筆記。

---

## 完成！接下來你可以這樣用

| 你說的話 | Claude + Obsidian 會做的事 |
|----------|--------------------------|
| 「搜尋我的筆記有沒有跟 XXX 相關的」 | 用 BM25 搜尋 vault，回傳相關筆記 |
| 「幫我新增一篇筆記，紀錄今天的教學反思」 | 在 vault 中建立新筆記，含 frontmatter |
| 「幫我整理這篇筆記的重點」 | 讀取筆記內容並摘要 |
| 「幫我看 XXX 這篇筆記，延伸出教學點子」 | 讀取指定筆記，給出延伸建議 |
| 「幫我把今天的對話重點存到筆記」 | 整理對話並存入 vault |

---

## 如果安裝失敗，如何重來

對 Claude Code 說：
「Obsidian 懶人包執行失敗了，幫我檢查哪裡出問題，重新處理。」

Claude 會自動：
1. 檢查 mcpvault 是否正常運作
2. 檢查 vault 路徑是否正確
3. 找出問題並修復

如果需要完全重置 MCP 連接：
```bash
# 如果有 claude CLI
claude mcp remove obsidian

# 如果沒有 claude CLI（桌面版），手動刪除以下檔案中的 obsidian 設定：
# - ~/.claude/settings.json
# - [工作目錄]/.claude/settings.local.json
# - [工作目錄]/.mcp.json
```
然後從步驟五重新開始。

---

## 常見問題

| 問題 | 解法 |
|------|------|
| mcpvault 搜尋不到筆記 | 確認 vault 路徑正確，路徑中有中文或空格需用引號包住 |
| Google Drive 同步衝突 | 避免在兩台裝置同時編輯同一篇筆記，等同步完成再操作 |
| `npx: command not found` | 確認 Node.js 已安裝，重啟 Claude Code 桌面版 |
| `claude: command not found` | Claude Code 桌面版不一定有 CLI。改用手動寫入設定檔的方式（見步驟五） |
| 重啟後 MCP 工具仍不存在 | 確認設定檔中 `command` 使用**完整路徑**（如 `C:\\Users\\...\\mcpvault.cmd`），不要只寫 `npx` |
| Windows bash 找不到 node | 新裝的 Node.js 可能不在 bash PATH 中，需要 `export PATH="/c/Program Files/nodejs:$PATH"` |
| 設定檔寫了但沒生效 | 確認在三個位置都寫入（`~/.claude/settings.json`、`.claude/settings.local.json`、`.mcp.json`） |
| Obsidian 需要裝外掛嗎？ | **不需要**。mcpvault 直接讀寫 vault 資料夾的檔案，不經過 Obsidian app |

---

## 同步方案說明

本懶人包預設使用 **Google Drive**（免費），如果你使用其他方案：

| 方案 | 差異 |
|------|------|
| **Google Drive**（本懶人包預設） | vault 建在 Google Drive 資料夾內即可 |
| **Obsidian Sync**（付費 $4/月） | vault 建在任意位置，Obsidian 內設定同步 |
| **iCloud**（macOS / iOS） | vault 建在 iCloud Drive 資料夾內 |

> 差別只在 vault 資料夾的位置不同，MCP 連接方式完全相同。

---

## 更新紀錄

| 日期 | 版本 | 更新內容 |
|------|------|---------|
| 2026-04-04 | v0.1 | 初版 |
| 2026-04-04 | v0.2 | 加入環境檢查、復原機制、跨平台支援、Google Drive 同步方案 |
| 2026-04-06 | v0.3 | 移除 obsidian-ide（obsidian-claude-code-mcp），mcpvault 已涵蓋所需功能 |
| 2026-04-06 | v0.4 | 補充 Google Drive 桌面版完整安裝教學、修正步驟編號 |
| 2026-04-06 | v0.5 | 實測修正：步驟五改為全域安裝 + 手動寫設定檔（解決桌面版無 CLI、npx PATH 問題），三處設定確保一次重啟就成功，補充 Windows 踩坑常見問題 |

---

## 相關連結

- [mcpvault GitHub](https://github.com/bitbonsai/mcpvault)
- [Obsidian 官網](https://obsidian.md)
- [[00-環境建置|懶人包 #00：環境建置]]
- [[Claude基本功EP07 - Obsidian第二大腦懶人包]]
- [[README|Claude Code 懶人包索引]]
