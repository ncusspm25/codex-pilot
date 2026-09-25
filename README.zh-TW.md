[English](README.md) | 繁體中文

# Codex Pilot

**讓 Codex 有更順手的預設工作方式。**

`codex-pilot` 是一份可重複使用的全域 `AGENTS.md`。它適合希望 Codex 直接完成已授權工作、不要每次遇到大型任務就先停下來確認 model 或 effort 的使用者。

它調整的是 Codex 會遵循的工作指示，不會切換 model picker、增加權限或覆蓋更高優先級的規則。它不需要安裝套件，也沒有常駐程式。

## 安裝

Codex 會從 `~/.codex/AGENTS.md` 讀取使用者層級的指示。Windows 對應路徑是 `%USERPROFILE%\.codex\AGENTS.md`。可參考 [OpenAI 的 Codex AGENTS.md 說明](https://developers.openai.com/api/docs/guides/latest-model#using-agentsmd)。

如果你已經有全域 `AGENTS.md`，請先備份，再手動合併內容；不要直接覆蓋自己的規則。只有在目標檔案不存在時，才直接複製本 repo 的 `AGENTS.md`。

macOS 或 Linux：在 repo 資料夾執行：

```sh
mkdir -p ~/.codex
if [ -e ~/.codex/AGENTS.md ]; then
  printf '%s\n' '請將 AGENTS.md 與既有的 ~/.codex/AGENTS.md 合併；未覆寫任何檔案。'
else
  cp AGENTS.md ~/.codex/AGENTS.md
fi
```

Windows PowerShell：在 repo 資料夾執行：

```powershell
$target = Join-Path $env:USERPROFILE ".codex\AGENTS.md"
New-Item -ItemType Directory -Force (Split-Path $target) | Out-Null
if (Test-Path -LiteralPath $target) {
    throw "請手動合併既有的全域 AGENTS.md；未覆寫任何檔案。"
}
Copy-Item -LiteralPath .\AGENTS.md -Destination $target
```

安裝後開啟新的 Codex session。專案內的 `AGENTS.md` 可以再補上更具體的指示。

## Model 與 effort

這份 policy 預設沿用你目前選好的 model 和 effort。一般任務直接開始做，並依工作內容調整調查深度與驗證方式。只有當目前設定很可能明顯影響正確性、完成品質或效率時，才簡短建議一個更合適的設定；只要還有可安全完成的工作，就繼續處理。

```text
收到任務 → 沿用目前 model 和 effort → 執行 → 按影響程度驗證
                         └─ 設定明顯不足？→ 建議一個調整
                                              能繼續就先繼續
```

這份 policy 不會自動切換 model picker。

## 實際差異

遇到重構時，Codex 可以沿用目前設定，檢查相關程式、完成修改並執行適合的測試；不需要先問你要不要換 model。

如果工作是刪除正式環境資料，Codex 仍然需要取得明確授權。減少 model 選擇造成的中斷，不代表放寬會影響你或其他人的操作限制。

長任務應延續原本的目標、限制、決策、未解問題和目前成果。這份 policy 不要求每個階段都輸出計畫或進度檢查點。

## Skills 與相關專案

`codex-pilot` 是全域工作 policy；Skills 則是可選的任務流程，依使用者選擇和 Codex 環境的規則使用。本 repo 不要求所有人停用 Skills。

[Opus Mode for Codex](https://github.com/ncusspm25/opus-mode-for-codex) 是獨立的長任務 Skill，並非本 repo 的依賴。

## 限制

`AGENTS.md` 是指示文字，不是權限控管機制。實際效果會受 Codex 使用介面、版本和更高優先級的指示影響。它不能授予工具權限，也無法保證模型一定採取特定行為。本 repo 沒有宣稱 benchmark 或效能提升。
