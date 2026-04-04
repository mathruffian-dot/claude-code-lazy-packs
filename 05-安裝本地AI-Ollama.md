---
title: 'Claude Code 懶人包 #05：安裝本地 AI（Ollama）'
date: '2026-04-04'
type: 懶人包
version: v0.2
status: 初版（實作後更新）
tags:
  - Claude-Code
  - 懶人包
  - Ollama
  - Gemma4
  - 本地AI
video: EP10
---
# Claude Code 懶人包 #05：安裝本地 AI（Ollama）

> 版本：v0.2
> 更新日期：2026-04-04
> 對應影片：Claude基本功 EP10

> 📌 **本懶人包可獨立執行**：會自動檢查並安裝所需工具，不需要先看過其他懶人包。你只要確認下方「先備條件」即可開始。

---

## 這個懶人包會幫你做什麼？

在你的電腦上安裝一個免費的本地 AI，讓你用 Claude Code 做出來的工具也能有 AI 能力：
- 完全免費，不需要 API Key 或信用卡
- 資料不離開你的電腦（隱私安全）
- 沒有網路也能運作

---

## 先備條件

- [ ] Claude Code 桌面版已安裝且能正常使用（Pro 方案以上）
- [ ] 電腦記憶體至少 8GB（建議 16GB 以上）
- [ ] 電腦有至少 10GB 可用硬碟空間

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

1. **確認作業系統**：確認是 Windows / macOS / Linux
2. **確認電腦記憶體大小**：
   - Windows：`wmic computersystem get totalphysicalmemory`
   - macOS：`sysctl -n hw.memsize`
   - Linux：`free -h`
3. **確認可用硬碟空間**：至少需要 10GB
4. **確認網路連線**：下載模型需要網路
5. **根據記憶體大小決定模型版本**：

| 記憶體 | 建議模型 | 預估下載大小 |
|--------|---------|------------|
| 8GB 以下 | ⚠️ 告知使用者建議改用 Gemini 免費 API（懶人包 #06） | — |
| 8-16GB | `gemma4:e2b` | 約 5GB |
| 16GB 以上 | `gemma4:e4b` | 約 10GB |

> 告知使用者偵測到的記憶體大小和建議的模型版本，確認後再繼續。

---

### 步驟一：安裝 Ollama

**Windows**：
```bash
winget install --id Ollama.Ollama --accept-source-agreements --accept-package-agreements
```

**macOS**：
> 🖐️ **需要手動操作**：請使用者到 https://ollama.com 下載安裝檔並執行。

**Linux**：
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

安裝完成後確認：
```bash
ollama --version
```

> 如果指令找不到，請重啟 Claude Code 桌面版再試。

---

### 步驟二：下載 AI 模型

根據步驟零決定的模型版本執行下載：

```bash
ollama pull [步驟零決定的模型名稱]
```

> ⚠️ 下載需要時間，依網路速度約 5-20 分鐘。
> 下載過程中會顯示進度條，請耐心等待。
> 告知使用者預估下載大小和時間。

下載完成後確認模型已安裝：
```bash
ollama list
```

---

### 步驟三：測試本地 AI

啟動模型並測試：

```bash
ollama run [模型名稱] "請用繁體中文回答：1+1 等於多少？"
```

> 確認模型能正常回應繁體中文。
> 如果回應正常，告知使用者：「本地 AI 安裝成功！」
> 如果回應異常（太慢、亂碼、沒回應），檢查記憶體是否足夠。

---

### 步驟四：測試從網頁呼叫本地 AI

建立一個簡單的測試網頁，確認瀏覽器能呼叫本地 Ollama：

1. 建立一個 HTML 檔案，包含一個輸入框和送出按鈕
2. 按下按鈕後，透過 Ollama 的 API（`http://localhost:11434/api/generate`）呼叫本地 AI
3. 將 AI 的回應顯示在網頁上

> 開啟測試網頁，輸入一個問題，確認能收到 AI 回應。
> 成功後告知使用者：「✅ 本地 AI 安裝完成！你做的網頁工具現在可以使用本地 AI 了。」

---

## 完成！接下來你可以這樣用

在你用 Claude Code 做網頁工具時，只要告訴 Claude：

> 「這個工具的 AI 功能用本地 Ollama 的 Gemma 4 模型。」

Claude Code 就會自動在你的工具中串接本地 AI。

| 工具功能 | 本地 AI 做的事 |
|----------|---------------|
| 學習單自動生成 | 根據主題產生題目 |
| 作文簡易回饋 | 給出基本的寫作建議 |
| 教材摘要 | 將長文本整理成重點 |
| 翻譯輔助 | 翻譯英文教材為中文 |

---

## 如果安裝失敗，如何重來

對 Claude Code 說：
「Ollama 懶人包執行失敗了，幫我檢查哪裡出問題。」

Claude 會自動：
1. 檢查 Ollama 是否正常安裝
2. 檢查模型是否下載完成
3. 檢查記憶體是否足夠
4. 找出問題並修復

如果需要完全重裝：
```bash
ollama rm [模型名稱]
```
然後從步驟二重新下載。

---

## 常見問題

| 問題 | 解法 |
|------|------|
| `ollama: command not found` | 重啟 Claude Code 桌面版 |
| 模型下載中斷 | 重新執行 `ollama pull [模型名稱]`，會從中斷處繼續 |
| 回應非常慢（超過 30 秒） | 電腦記憶體不足，建議改用較小的模型或改用 Gemini API |
| 瀏覽器呼叫 Ollama 失敗（CORS） | 需設定 Ollama 允許跨域存取：`OLLAMA_ORIGINS=* ollama serve` |
| （實作後持續補充） | |

---

## 更新紀錄

| 日期 | 版本 | 更新內容 |
|------|------|---------|
| 2026-04-04 | v0.1 | 初版 |
| 2026-04-04 | v0.2 | 加入環境檢查、自動選擇模型、復原機制 |

---

## 相關連結

- [Ollama 官網](https://ollama.com)
- [Gemma 4 官方發布](https://blog.google/innovation-and-ai/technology/developers-tools/gemma-4/)
- [[06-設定Gemini免費API|懶人包 #06：設定 Gemini 免費 API]]
- [[Claude基本功EP10 - 本地AI與免費API懶人包]]
- [[README|Claude Code 懶人包索引]]
