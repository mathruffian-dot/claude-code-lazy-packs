---
name: cc-notebooklm
description: Claude Code 連接 NotebookLM MCP。說「連接 NotebookLM」時載入。
---

# 連接 NotebookLM（Claude Code 版）

## 步驟

### 1. 安裝
```bash
uv tool install notebooklm-mcp-cli
# 或 pip install notebooklm-mcp-cli
nlm --version
```

### 2. 登入
```bash
nlm login
```
（瀏覽器會開啟 Google 登入頁面）

驗證：
```bash
nlm doctor
```

### 3. 註冊 MCP
```bash
nlm setup add claude-code
```
（自動寫入 `~/.claude/settings.json`）

### 4. 驗證
重啟 Claude Code 後，問：「請列出我所有的 NotebookLM 筆記本。」

回報格式：nlm 版本、登入狀態（nlm doctor）、MCP 設定、筆記本讀取測試結果。

## 如果失敗
- 手動編輯 `~/.claude/settings.json` 的 `mcpServers` 區塊
- 移除：`nlm setup remove claude-code`；`uv tool uninstall notebooklm-mcp-cli`；`nlm logout`
- 從步驟 1 重來
