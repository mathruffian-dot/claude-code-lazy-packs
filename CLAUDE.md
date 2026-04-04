# CLAUDE.md — claude-code-lazy-packs

## 這個資料夾是什麼

這是公開發布到 GitHub 的 Claude Code 懶人包倉庫，對應 repo：
**mathruffian-dot/claude-code-lazy-packs**

## ⚠️ 雙倉同步規則（重要）

懶人包在 Obsidian vault 裡有**兩份**：

| 路徑 | 用途 |
|------|------|
| `Claude Code 懶人包/` | Obsidian 閱讀用（含雙向連結、frontmatter） |
| `claude-code-lazy-packs/` | 推送到 GitHub 的公開版本 |

**每次更新懶人包內容時，必須兩份同步修改**，然後把 `claude-code-lazy-packs/` 的變更 commit + push 到 GitHub。

### 同步流程

1. 修改 `Claude Code 懶人包/XX-xxx.md`（Obsidian 內部版）
2. 同步修改 `claude-code-lazy-packs/XX-xxx.md`（GitHub 版，檔名格式可能略異）
3. 在 `claude-code-lazy-packs/` 執行：
   ```bash
   git add <檔案>
   git commit -m "<說明>"
   git push origin master
   ```

### 檔名對照

| Obsidian 版 | GitHub 版 |
|-------------|-----------|
| `01-連接 NotebookLM.md` | `01-連接-NotebookLM.md` |
| （空格） | （連字號） |

## 檔案結構

```
claude-code-lazy-packs/
├── 00-環境建置.md
├── 01-連接-NotebookLM.md
├── 02-連接-GitHub.md
├── 03-建立第二大腦-Obsidian.md
├── 04-連接-Supabase-資料庫.md
├── 05-安裝本地AI-Ollama.md
├── 06-設定Gemini免費API.md
├── LICENSE
└── README.md
```

## 提醒 Claude

- 使用者說「更新懶人包」時，**預設要同步兩份**並推 GitHub
- 若使用者只說「更新 Obsidian 版」或「更新 GitHub 版」才分開處理
- Push 前務必確認 commit message 清楚描述變更內容
