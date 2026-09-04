---
name: claude-code-lazy-packs
description: Claude Code 懶人包全集 — 環境建置、MCP 串接、技能安裝。說「Claude Code 懶人包」「安裝懶人包」時載入。
---

# Claude Code 懶人包 — AI Agent 自動安裝入口

當使用者給你這個 repo 網址並說要安裝時：

## 步驟一：列出可用懶人包

> 編號對應 README.md 的懶人包編號，方便使用者跨文件溝通。

| 編號 | Skill 名稱 | 說明 |
|------|-----------|------|
| 00 | `00-env-setup` | 安裝 uv 等基礎工具 |
| 01 | `01-notebooklm` | 連接 NotebookLM MCP |
| 02 | `02-github` | 連接 GitHub CLI + push 驗證 |
| 03 | `03-obsidian` | 連接 Obsidian MCPVault |
| 03+ | `04-second-brain` | 第二大腦三層結構 |
| 04 | `04-supabase` | 連接 Supabase 資料庫 |
| 04.5 | `05-firebase` | 連接 Firebase 資料庫 |
| 05 | `06-ollama` | 安裝本地 AI Ollama |
| 06 | `07-gemini` | 設定 Gemini 免費 API |
| 07 | `08-workspace` | 老師建專案工作模式 |
| 08 | `09-draw` | 安裝 gpt-image-2 生圖 skill |
| 全部 | `00-install-all` | 一次安裝全部 |

## 步驟二：讓使用者選擇

問：「你要安裝哪些？輸入全部或編號組合（例如 00, 01, 03）。」

## 步驟三：依序安裝

```bash
npx skills add mathruffian-dot/claude-code-lazy-packs --skill <名稱> -g -y
```

若無法使用 `npx skills add`，改手動讀取 `skills/<名稱>/SKILL.md` 執行。

## 步驟四：回報

每項回報 ✅/⚠️/❌，最後列總表。
