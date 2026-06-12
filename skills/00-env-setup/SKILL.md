---
name: cc-env-setup
description: Claude Code 環境建置（Node.js, uv）。說「建置環境」「安裝開發環境」時載入。
---

# Claude Code 環境建置

## 步驟
檢查 `node --version; uv --version`，補裝缺少的工具。

| 工具 | Windows | macOS | Linux |
|------|---------|-------|-------|
| Node.js | `winget install OpenJS.NodeJS` | `brew install node` | `curl -fsSL https://deb.nodesource.com/setup_20.x \| bash - && apt install nodejs -y` |
| uv | `powershell -c "irm https://astral.sh/uv/install.ps1 \| iex"` | `curl -LsSf https://astral.sh/uv/install.sh \| sh` |

最終驗證所有版本。回報格式：已安裝/已補裝清單 + 版本號。
