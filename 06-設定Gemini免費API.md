---
title: 'Claude Code 懶人包 #06：設定 Gemini 免費 API'
date: '2026-04-04'
type: 懶人包
version: v0.2
status: 初版（實作後更新）
tags:
  - Claude-Code
  - 懶人包
  - Gemini
  - API
  - 免費
video: EP10
---
# Claude Code 懶人包 #06：設定 Gemini 免費 API

> 版本：v0.2
> 更新日期：2026-04-04
> 對應影片：Claude基本功 EP10

---

## 這個懶人包會幫你做什麼？

設定 Google Gemini 的免費 API，讓你用 Claude Code 做出來的工具也能有 AI 能力：
- 完全免費，不需要信用卡
- 不用安裝任何軟體（只需要一個 API Key）
- AI 能力比本地模型更強（適合複雜推理、出題、分析）

---

## 先備條件

- [ ] Claude Code 桌面版已安裝且能正常使用（Pro 方案以上）
- [ ] 已有 Google 帳號
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

1. **確認作業系統**：確認是 Windows / macOS / Linux
2. **確認網路連線正常**
3. **確認是否已有 Gemini API Key**：
   - 檢查環境變數 `GEMINI_API_KEY` 是否已存在
   - 如果已存在，跳到步驟三驗證即可

> 全部確認後繼續下一步。

---

### 步驟一：申請 Gemini API Key

> 🖐️ **需要手動操作**：
> 1. 請使用者開啟瀏覽器，到 https://aistudio.google.com/apikey
> 2. 用 Google 帳號登入
> 3. 點擊「Create API Key」
> 4. 選擇一個 Google Cloud 專案（如果沒有，系統會自動建一個）
> 5. API Key 產生後，**複製整串 Key**
>
> ⚠️ 不需要輸入信用卡。
> ⚠️ API Key 只會顯示一次，請確認已複製。

請使用者將 API Key 貼到對話中。

---

### 步驟二：安全存放 API Key

將 API Key 存到環境變數中（不要寫在程式碼裡）：

**Windows**：
```bash
setx GEMINI_API_KEY "[使用者的API Key]"
```

**macOS / Linux**：
```bash
echo 'export GEMINI_API_KEY="[使用者的API Key]"' >> ~/.bashrc
source ~/.bashrc
```

> ⚠️ **安全提醒**：
> - API Key 不要寫在 HTML 或 JavaScript 原始碼中（公開網頁看得到）
> - 不要推到 GitHub 上（會被掃描盜用）
> - 存在環境變數中是最安全的做法

---

### 步驟三：驗證 API Key

測試 API Key 是否能正常使用：

```bash
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=$GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"contents":[{"parts":[{"text":"請用繁體中文回答：1+1 等於多少？"}]}]}'
```

> 如果回傳正常的 JSON 回應，代表 API Key 設定成功。
> 如果回傳錯誤，檢查 API Key 是否正確複製（前後不能有空格）。

---

### 步驟四：測試從網頁呼叫 Gemini API

建立一個簡單的測試網頁，確認瀏覽器能呼叫 Gemini API：

1. 建立一個 HTML 檔案，包含一個輸入框和送出按鈕
2. 按下按鈕後，呼叫 Gemini API
3. 將 AI 的回應顯示在網頁上

> ⚠️ 測試階段可以直接在前端呼叫 API。
> 正式工具建議透過 Supabase Edge Functions 代理（避免 API Key 暴露）。

> 測試成功後告知使用者：「✅ Gemini 免費 API 設定完成！你做的網頁工具現在可以使用雲端 AI 了。」

---

## 完成！接下來你可以這樣用

在你用 Claude Code 做網頁工具時，只要告訴 Claude：

> 「這個工具的 AI 功能用 Gemini 免費 API。API Key 從環境變數讀取。」

Claude Code 就會自動在你的工具中串接 Gemini API。

| 工具功能 | Gemini API 做的事 |
|----------|-------------------|
| 智慧出題 | 根據單元和難度自動出題 |
| 作文批改回饋 | 分析寫作結構、給出具體建議 |
| 教材差異化 | 同一份教材產出 A/B/C 三種難度版本 |
| 學生回饋分析 | 整理全班回饋，找出共同問題 |
| 多語言翻譯 | 支援 140+ 語言的即時翻譯 |

---

## 如果安裝失敗，如何重來

對 Claude Code 說：
「Gemini API 懶人包執行失敗了，幫我檢查哪裡出問題。」

Claude 會自動：
1. 檢查環境變數是否正確設定
2. 測試 API Key 是否有效
3. 找出問題並修復

如果需要重新申請 API Key：
到 https://aistudio.google.com/apikey 重新建一個即可。

---

## 常見問題

| 問題 | 解法 |
|------|------|
| API 回傳 401 錯誤 | API Key 無效，到 Google AI Studio 重新申請 |
| API 回傳 429 錯誤 | 超過免費速率限制，等一分鐘再試 |
| 環境變數設定後讀不到 | Windows 需重啟 Claude Code；macOS 需 `source ~/.bashrc` |
| 不確定 Key 是否正確 | 到 https://aistudio.google.com/apikey 查看已建立的 Key |
| 擔心被收費 | 免費方案不需要信用卡，不會產生任何費用 |
| （實作後持續補充） | |

---

## 免費方案說明

| 項目 | 免費額度 |
|------|---------|
| 費用 | $0（完全免費） |
| 信用卡 | 不需要 |
| 可用模型 | Gemini 2.5 Flash、Flash-Lite 等 |
| 速率限制 | 每分鐘有請求上限（一般教學使用不會超過） |
| 使用期限 | 無期限 |

---

## 更新紀錄

| 日期 | 版本 | 更新內容 |
|------|------|---------|
| 2026-04-04 | v0.1 | 初版 |
| 2026-04-04 | v0.2 | 加入環境檢查、安全提醒、復原機制 |

---

## 相關連結

- [Google AI Studio](https://aistudio.google.com)
- [Gemini API 文件](https://ai.google.dev/docs)
- [[05-安裝本地AI Ollama|懶人包 #05：安裝本地 AI（Ollama）]]
- [[Claude基本功EP10 - 本地AI與免費API懶人包]]
- [[README|Claude Code 懶人包索引]]
