---
title: 'Claude Code 懶人包 #03：建立第二大腦（Obsidian）'
date: '2026-04-04'
type: 懶人包
version: v0.2
status: 初版（實作後更新）
tags:
  - Claude-Code
  - 懶人包
  - Obsidian
  - MCP
  - 第二大腦
video: EP06
---
# Claude Code 懶人包 #03：建立第二大腦（Obsidian）

> 版本：v0.2
> 更新日期：2026-04-04
> 對應影片：Claude基本功 EP06

> 📌 **本懶人包可獨立執行**：會自動檢查並安裝所需工具，不需要先看過其他懶人包。你只要確認下方「先備條件」即可開始。

---

## 這個懶人包會幫你做什麼？

幫你的 Claude Code 裝上「第二大腦」，完成後 Claude 可以：
- 讀取你 Obsidian 筆記庫裡的所有筆記
- 幫你搜尋、新增、編輯筆記
- 知道你目前正在看哪篇筆記
- 記住你的教學紀錄、素材、想法，變成你的專屬助理

---

## 先備條件

在使用這個懶人包之前，請確認：

- [ ] Claude Code 桌面版已安裝且能正常使用（Pro 方案以上）
- [ ] 已有 Google 帳號（用於 Google Drive 同步）
- [ ] 電腦有網路連線
- [ ] Node.js 已安裝（mcpvault 需要 npx 指令）

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
3. **檢查 Node.js 是否已安裝**：執行 `node --version`，如果未安裝：
   - Windows：`winget install --id OpenJS.NodeJS --accept-source-agreements --accept-package-agreements`
   - macOS：`brew install node`
   - Linux：`sudo apt update && sudo apt install nodejs npm -y`
4. **檢查 npx 是否可用**：執行 `npx --version`
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

> 🖐️ **需要手動操作**：
> 1. 請使用者開啟瀏覽器，到 https://www.google.com/drive/download/ 下載 Google Drive 桌面版
> 2. 安裝並登入 Google 帳號
> 3. 等待 Google Drive 同步完成（狀態列顯示 ✅）

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
**不需要 Obsidian 開著就能運作。**

請執行以下指令（將 vault 路徑替換為步驟三記錄的實際路徑）：

**Windows**：
```bash
claude mcp add obsidian --scope user -- npx @bitbonsai/mcpvault "[vault的完整路徑]"
```

**macOS / Linux**：
```bash
claude mcp add obsidian --scope user -- npx @bitbonsai/mcpvault "/path/to/vault"
```

> ⚠️ 路徑中如果有中文或空格，務必用引號包住。
> 例如：`"C:\Users\王老師\Google Drive\secondbrain"`

---

### 步驟六：安裝 obsidian-claude-code-mcp 插件（可選但建議）

這個插件讓 Claude Code 能感知你目前在 Obsidian 開啟的筆記。

> 🖐️ **需要手動操作**：以下步驟需要使用者在 Obsidian 中操作。

#### 6a. 下載插件檔案

在 vault 的 `.obsidian/plugins/` 資料夾下建立 `obsidian-claude-code-mcp` 資料夾，並下載三個檔案：

```bash
# 建立插件資料夾
mkdir -p "[vault路徑]/.obsidian/plugins/obsidian-claude-code-mcp"

# 下載插件檔案
cd "[vault路徑]/.obsidian/plugins/obsidian-claude-code-mcp"
curl -sLO "https://github.com/iansinnott/obsidian-claude-code-mcp/releases/download/1.1.8/main.js"
curl -sLO "https://github.com/iansinnott/obsidian-claude-code-mcp/releases/download/1.1.8/manifest.json"
curl -sLO "https://github.com/iansinnott/obsidian-claude-code-mcp/releases/download/1.1.8/styles.css"
```

#### 6b. 在 Obsidian 中啟用插件

> 🖐️ **需要手動操作**：
> 1. 重新開啟 Obsidian（或按 Ctrl+R 重新載入）
> 2. 到「設定 → 社群外掛」
> 3. 先關閉「安全模式」（第一次使用社群外掛需要）
> 4. 找到「Claude Code MCP」並啟用
> 5. 確認設定頁面中顯示 `Running`，port 預設 `22360`

#### 6c. 設定 Claude Code 的 MCP 連接

```bash
claude mcp add obsidian-ide --scope user -- npx mcp-remote http://localhost:22360/sse
```

> 注意：使用 obsidian-ide 時，Obsidian 必須先開著。

---

### 步驟七：重啟 Claude Code 並驗證

> 🖐️ **需要手動操作**：請使用者完全關閉 Claude Code 桌面版，然後重新開啟。

重新開啟後，逐一測試：

#### 測試 1：搜尋筆記
嘗試搜尋 vault 中的筆記（例如搜尋「教學」或任何已建立的資料夾名稱）。
如果能回傳結果，代表 mcpvault 連接成功。

#### 測試 2：新增筆記
在 vault 中新增一篇測試筆記：
- 路徑：`測試筆記.md`
- 內容：「這是 Claude Code 透過 MCP 自動建立的筆記。連接成功！」
- 含 frontmatter：title、date

#### 測試 3：感知當前筆記（如果有安裝步驟六的插件）
請使用者在 Obsidian 中打開剛才的測試筆記，然後嘗試讀取當前開啟的檔案。

> 全部測試通過後，告知使用者：
> 「✅ 全部完成！Claude Code 已成功連接你的 Obsidian 第二大腦。」

---

## 階段三：建立基礎知識結構

### 步驟八：建立 CLAUDE.md

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

### 步驟九：建立第一篇正式筆記

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
| 「我現在開著的這篇，幫我延伸出教學點子」 | 感知當前筆記，給出延伸建議 |
| 「幫我把今天的對話重點存到筆記」 | 整理對話並存入 vault |

---

## 如果安裝失敗，如何重來

對 Claude Code 說：
「Obsidian 懶人包執行失敗了，幫我檢查哪裡出問題，重新處理。」

Claude 會自動：
1. 檢查 mcpvault 是否正常運作
2. 檢查 vault 路徑是否正確
3. 檢查 obsidian-claude-code-mcp 插件是否啟用
4. 找出問題並修復

如果需要完全重置 MCP 連接：
```bash
claude mcp remove obsidian
claude mcp remove obsidian-ide
```
然後從步驟五重新開始。

---

## 常見問題

| 問題 | 解法 |
|------|------|
| mcpvault 搜尋不到筆記 | 確認 vault 路徑正確，路徑中有中文或空格需用引號包住 |
| obsidian-ide 連不上 | 確認 Obsidian 有開啟且插件已啟用，然後重啟 Claude Code |
| 社群插件找不到 Claude Code MCP | 這個插件需手動下載安裝（步驟六），尚未上架官方插件庫 |
| Google Drive 同步衝突 | 避免在兩台裝置同時編輯同一篇筆記，等同步完成再操作 |
| `npx: command not found` | 確認 Node.js 已安裝，重啟 Claude Code 桌面版 |
| （實作後持續補充） | |

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

---

## 相關連結

- [mcpvault GitHub](https://github.com/bitbonsai/mcpvault)
- [obsidian-claude-code-mcp GitHub](https://github.com/iansinnott/obsidian-claude-code-mcp)
- [Obsidian 官網](https://obsidian.md)
- [[00-環境建置|懶人包 #00：環境建置]]
- [[Claude基本功EP06 - Obsidian第二大腦懶人包]]
- [[README|Claude Code 懶人包索引]]
