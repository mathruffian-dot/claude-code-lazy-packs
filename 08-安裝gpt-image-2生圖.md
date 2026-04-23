# 懶人包 08：把 ChatGPT Image 2.0 裝進 Claude Code

> 對應影片：**Claude 基本功 EP11 — 把 ChatGPT Image 2.0 裝進 Claude Code**
> 目標：五分鐘內完成安裝，讓 Claude Code 能夠用一句話生圖
> 費用：第一次需要儲值 OpenAI 最少 US$5；每張圖約 **NT$0.3**（low 品質）

---

## 你會得到什麼

裝好後，在 **任何** 專案資料夾裡對 Claude Code 說：

> 「畫一隻穿西裝的龍蝦」

Claude Code 會自動呼叫 OpenAI 的 **gpt-image-2** 生圖，存到當前資料夾的 `slides/generated/`（或 `./generated/`）裡。

---

## 你需要手動準備的三件事

Claude 沒辦法幫你做這三件事（需要本人身份驗證與付款）。做完之後把剩下的全部交給 Claude。

### ① OpenAI 帳號 + 儲值（US$5 起跳）

1. 到 [platform.openai.com](https://platform.openai.com)，用你的 ChatGPT 帳號登入（**同一個帳號**）
2. 右上角頭像 → **Billing** → **Add to credit balance** → 至少儲值 **US$5**（約 NT$160）
3. 可以綁信用卡或用 PayPal

> **為什麼要儲值？** API 是按張計費，跟 ChatGPT 訂閱分開計算。US$5 足夠畫 500 張以上（low 品質）。

### ② 組織 Individual 驗證

gpt-image-2 需要身份驗證才能用。

1. 到 [platform.openai.com/settings/organization/general](https://platform.openai.com/settings/organization/general)
2. 找到 **Verifications** 區塊，點 **Verify Organization**
3. 上傳身分證正反面 + 手持身分證自拍
4. 等 **15 分鐘** 左右驗證通過（會收到 Email）

> **沒做這步會怎樣？** 生圖時會跳 `403 Organization must be verified` 錯誤。

### ③ 取得 API Key

1. 到 [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. **Create new secret key** → 名稱填 `claude-code-draw`（隨便取）
3. **Permissions** 選 **All** → **Create**
4. **複製那串 `sk-...` 開頭的 Key**（只會顯示一次，關掉視窗就看不到了）
5. 暫時貼到記事本

---

## 把以下內容整份丟給 Claude Code

> 打開 Claude Code 桌面版（Pro 以上），貼上這整個檔案內容。Claude 會自動執行下列步驟並提示你貼 API Key。

---

# 🤖 Claude 執行區（以下由 AI 自動完成）

你是 Claude Code 助手。請依序完成以下任務。遇到需要使用者提供資訊時，用 AskUserQuestion 詢問。

## 任務 1：確認環境

檢查使用者環境：

```bash
python --version
pip --version
```

若沒有 Python，提示使用者安裝 Python 3.10+ 後再繼續。

## 任務 2：安裝必要套件

```bash
pip install openai --break-system-packages
```

Windows 使用者跳過 `--break-system-packages`；macOS/Linux 需加上。

## 任務 3：儲存 API Key

用 AskUserQuestion 問使用者：

> 請把你剛剛從 OpenAI 複製的 API Key（`sk-...` 開頭）貼在下方：

拿到 Key 之後，寫入使用者 home 的 `~/.openai.env`（**全域可用**，任何專案都吃得到）：

```
OPENAI_API_KEY=sk-使用者貼的那串
```

Windows 路徑：`C:/Users/<使用者>/.openai.env`
macOS/Linux：`~/.openai.env`

**注意**：
- 這個檔案要有 600 權限（Unix）或加上隱藏屬性（Windows）
- `.gitignore` 不會追這個路徑（在 home 底下），安全

## 任務 4：建立全域 draw Skill

建立資料夾：

- Windows：`C:/Users/<使用者>/.claude/skills/draw/`
- macOS/Linux：`~/.claude/skills/draw/`

在這個資料夾裡寫入兩個檔案。

### 4-1：`SKILL.md`

```markdown
---
name: draw
description: OpenAI gpt-image-2 生圖技能（全域可用）。當使用者要求「畫一張」、「生一張圖」、「做一張圖」、「產生圖片」、「畫個封面」、「畫插圖」、「畫示意圖」、「畫分鏡」等任何需要 AI 生成圖像的情境時，請一定要使用此技能。此技能會呼叫本地腳本以 gpt-image-2 模型生圖，自動判斷 quality 等級（預設 low），存檔至當前專案的 slides/generated/ 目錄（若無則建於當下工作目錄）。
---

# 小克生圖技能（gpt-image-2）

## 觸發情境
使用者說出：
- 「畫一張 XX」「生一張圖」「做一張圖」
- 「畫個封面／插圖／示意圖／分鏡」
- 「產生圖片」「幫我生圖」
- 「改這張圖」「修改圖片」「把背景換成 XX」（→ 改圖模式，需提供圖片路徑）

## 腳本位置
- Windows：`C:/Users/<使用者>/.claude/skills/draw/draw.py`
- macOS/Linux：`~/.claude/skills/draw/draw.py`

## 使用方式
```bash
python <SKILL路徑>/draw.py "要畫的內容" --name 檔名前綴
```

### 參數
- `prompt`（必填）：要畫什麼
- `--size`：`1024x1024`（方，預設）/ `1536x1024`（橫）/ `1024x1536`（直）
- `--quality`：`low`（預設，NT$0.3）/ `medium`（NT$1.3）/ `high`（NT$5.5）
- `--n`：生成張數 1–8
- `--name`：檔名前綴
- `--outdir`：輸出目錄
- `--edit IMAGE_PATH`：改圖模式（指定來源圖）
- `--mask MASK_PATH`：遮罩圖片（搭配 --edit 使用）

## 判斷 quality 等級的原則
**預設永遠用 `low`**（省錢 + 速度優先）

- **low**（NT$0.3）：**99% 情境**。演講簡報、教學插圖、封面、demo 都夠。
- **medium**（NT$1.3）：通常不用。
- **high**（NT$5.5）：實體印刷、跨語言文字零錯才用。

不確定就 **low**，不要自作主張升級。

## 錯誤處理
- `403 Organization must be verified` → 到 platform.openai.com/settings/organization/general 做 Individual 驗證
- `401 Invalid API key` → 檢查 `~/.openai.env`
- `429 Rate limit` → 額度用完，到 Billing 儲值

## 輸出
PNG 檔，格式：`<name>_<YYYYMMDD_HHMMSS>.png`
```

### 4-2：`draw.py`

```python
"""
小克全域生圖腳本（OpenAI gpt-image-2 版）

用法：
  python draw.py "一隻穿西裝的龍蝦，寫實風格"
  python draw.py "演講海報" --size 1536x1024 --name poster
  python draw.py "把背景換成海底" --edit ./image.png --name edited
  python draw.py "加一頂帽子" --edit ./image.png --mask ./mask.png --name masked

會自動讀取以下來源的 OPENAI_API_KEY（依序）：
  1. 當前 shell 環境變數
  2. 當前工作目錄的 .env
  3. 使用者 home 的 ~/.openai.env（全域備援）

輸出：
  預設放在「當前工作目錄/slides/generated/」
  若該目錄不存在，會建立「./generated/」
"""

import os
import sys
import base64
import argparse
from pathlib import Path
from datetime import datetime

MODEL = "gpt-image-2"
DEFAULT_SIZE = "1024x1024"
DEFAULT_QUALITY = "low"
DEFAULT_N = 1


def load_env_from_file(path: Path):
    if not path.exists():
        return
    with open(path, encoding="utf-8") as f:
        for line in f:
            line = line.strip()
            if line and not line.startswith("#") and "=" in line:
                key, value = line.split("=", 1)
                os.environ.setdefault(key.strip(), value.strip().strip('"').strip("'"))


def load_env():
    load_env_from_file(Path.cwd() / ".env")
    load_env_from_file(Path.home() / ".openai.env")


def resolve_outdir(user_outdir):
    if user_outdir:
        return Path(user_outdir)
    cwd = Path.cwd()
    slides_dir = cwd / "slides"
    if slides_dir.exists():
        return slides_dir / "generated"
    return cwd / "generated"


def _save_results(result, name, n, outdir):
    stamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    saved = []
    for i, item in enumerate(result.data):
        suffix = f"_{i + 1}" if n > 1 else ""
        out_path = outdir / f"{name}_{stamp}{suffix}.png"
        png_bytes = base64.b64decode(item.b64_json)
        out_path.write_bytes(png_bytes)
        saved.append(out_path)
        print(f"  [OK] {out_path}")
    return saved


def draw(prompt, size, quality, n, name, outdir):
    from openai import OpenAI
    if not os.getenv("OPENAI_API_KEY"):
        print("錯誤：找不到 OPENAI_API_KEY", file=sys.stderr)
        sys.exit(1)
    outdir.mkdir(parents=True, exist_ok=True)
    client = OpenAI()
    print(f"畫圖中（{MODEL}, {size}, {quality}, n={n}） -> {outdir}", file=sys.stderr)
    result = client.images.generate(model=MODEL, prompt=prompt, size=size,
                                     quality=quality, n=n)
    return _save_results(result, name, n, outdir)


def edit(prompt, image_path, mask_path, size, quality, n, name, outdir):
    from openai import OpenAI
    if not os.getenv("OPENAI_API_KEY"):
        print("錯誤：找不到 OPENAI_API_KEY", file=sys.stderr)
        sys.exit(1)
    if not image_path.exists():
        print(f"錯誤：找不到來源圖片 {image_path}", file=sys.stderr)
        sys.exit(1)
    outdir.mkdir(parents=True, exist_ok=True)
    client = OpenAI()
    mode = "遮罩改圖" if mask_path else "全圖改圖"
    print(f"改圖中（{mode}, {MODEL}, {size}, {quality}） -> {outdir}", file=sys.stderr)
    kwargs = dict(model=MODEL, image=open(image_path, "rb"),
                  prompt=prompt, size=size, quality=quality, n=n)
    if mask_path:
        if not mask_path.exists():
            print(f"錯誤：找不到遮罩圖片 {mask_path}", file=sys.stderr)
            sys.exit(1)
        kwargs["mask"] = open(mask_path, "rb")
    result = client.images.edit(**kwargs)
    return _save_results(result, name, n, outdir)


def main():
    load_env()
    parser = argparse.ArgumentParser()
    parser.add_argument("prompt", nargs="+")
    parser.add_argument("--edit", default=None)
    parser.add_argument("--mask", default=None)
    parser.add_argument("--size", default=DEFAULT_SIZE)
    parser.add_argument("--quality", default=DEFAULT_QUALITY,
                        choices=["low", "medium", "high", "auto"])
    parser.add_argument("--n", type=int, default=DEFAULT_N)
    parser.add_argument("--name", default="image")
    parser.add_argument("--outdir", default=None)
    args = parser.parse_args()
    prompt = " ".join(args.prompt)
    outdir = resolve_outdir(args.outdir)
    if args.edit:
        edit(prompt, Path(args.edit), Path(args.mask) if args.mask else None,
             args.size, args.quality, args.n, args.name, outdir)
    else:
        draw(prompt, args.size, args.quality, args.n, args.name, outdir)


if __name__ == "__main__":
    main()
```

## 任務 5：驗證安裝

測試生成一張圖：

```bash
python <SKILL路徑>/draw.py "一隻可愛的黑貓，扁平插畫風格" --name test
```

預期結果：
- 終端機顯示 `[OK] .../generated/test_<時間戳>.png`
- 開啟該檔案能看到黑貓圖

若遇到錯誤：
- `403 Organization must be verified` → 使用者沒做 Individual 驗證，引導他到 platform.openai.com/settings/organization/general
- `401 Invalid API key` → 檢查 `~/.openai.env` 裡的 Key 有沒有寫對
- `429 Rate limit` → 使用者沒儲值，提示到 Billing

## 任務 6：回報結果

驗證成功後，告訴使用者：

> 🎉 安裝完成！
>
> 以後在任何專案裡，對我說「畫一張 XX」就能生圖。
>
> - 預設存在 `slides/generated/` 或 `./generated/`
> - 每張約 NT$0.3（low 品質）
> - 生圖模型：gpt-image-2（OpenAI 最新）
>
> 想看實戰應用？請看 EP12：gpt-image-2 的三大教學應用

---

# 📚 原理延伸（影片觀眾自己看）

## 為什麼用 Skill 而不是 MCP？

| 方式 | 優點 | 缺點 |
|------|------|------|
| **Skill（本篇）** | 零安裝門檻、按需呼叫、不佔資源、整合費用保護邏輯 | 不支援圖片局部修改（需另寫） |
| MCP Server | 支援 inpainting、多輪對話 | 要裝 Node.js、改 settings.json、常駐 |

對老師觀眾來說，**Skill 方式門檻最低**，而且你的 `draw.py` 已經內建 `--edit` 能做全圖改圖。

## 安全性

- API Key 存在 `~/.openai.env`，跟專案 repo 完全分離
- 不會被 git 追蹤
- 所有生圖都在本機執行，不經過第三方伺服器

## 成本控制

三個防護機制內建在 `draw.py`：

1. **預設 low 品質**：一張 NT$0.3
2. **SKILL.md 規則**：Claude 不會自作主張升級 quality
3. **透明計費**：你可以到 [platform.openai.com/usage](https://platform.openai.com/usage) 隨時查用量

---

# 🔗 相關資源

- **EP11 腳本**：`創作庫/Claude基本功EP11 - 把ChatGPT Image 2.0裝進Claude Code.md`
- **EP12 腳本**（應用篇）：`創作庫/Claude基本功EP12 - gpt-image-2的三個教學應用.md`
- **OpenAI API 文件**：https://platform.openai.com/docs/guides/images
- **gpt-image-2 模型介紹**：https://platform.openai.com/docs/models/gpt-image-2
- **相關懶人包**：
  - [00 環境建置](00-環境建置.md)（Python 等基礎工具）
  - [09 SOIL 教學簡報生成](09-SOIL教學簡報.md)（下一集的實戰應用，規劃中）

---

**作者**：三師爸（宋睿偉）
**頻道**：[三師爸 Sense Bar](https://youtube.com/@SenseBar)
**最後更新**：2026-04-23
