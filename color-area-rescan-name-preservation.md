---
term: 色塊重掃保名
slug: color-area-rescan-name-preservation
aliases:
  - 色塊重掃保名
  - 重掃保名
  - 保名
  - rescan name preservation
  - RefreshAreaConfigs 保名
category: mechanism
created_at: 2026-08-05T05:20:11Z
created_by: summit
one_line: 分色圖重掃時人取的名字不會丟 — 照 color hex 對回舊 config；人工命名不受最小面積門檻過濾。掃描管「有哪些顏色」，名字的主人永遠是使用者
---

# 色塊重掃保名

# 色塊重掃保名 (Color Area Rescan Name Preservation)

> 分色圖重新掃描時，**人取的名字不會丟**。掃描管「找出有哪些顏色」，名字的主人永遠是使用者。

## 白話（給企劃 / 美術）

把色塊命名成「胸部」「大腿」之後，遇到美術改圖、或按了「Refresh AreaConfigs」重掃色 ——
系統會**照顏色碼把舊設定對回來**，命名過的色塊保持原名，不會被重置成一串自動編號。並且：

- **特意命名的小色塊一定留著** —— 就算小於自動忽略門檻（100 像素，防雜訊用），
  命名過就不受門檻過濾（Tim 2026-08-03 拍板 C-2：「要留，辨識用」）
- 只有**新出現的顏色**才會拿到自動編號（像 `3. (FF00AA)`）等人改名
- 同一個顏色跨好幾張分色圖 ＝ 同一個觸摸位置，名字共用（C-6 拍板）

## 工程（實作落點）

機制在 `ClickAreaAsset.RefreshAreaConfigs`：

1. 既有 configs 先按 color hex 收進字典（改過名的設定物件原封保留）
2. 全圖片資料掃像素 —— 字典已有的顏色**直接沿用舊 config（保名發生在這）**，
   沒有的才 new 一個 `AutoGenAsset=true` 的自動命名
3. 收尾過濾：`!AutoGenAsset || (面積 ≥ m_MinAreaPixelSize)`
   —— 人工命名（`AutoGenAsset=false`）**無條件通過門檻**

## 驗收四步（Plan C 蓋章條件）

1. 挑 2-3 個色塊改名，其中至少一個故意選「小於 100px 的小色塊」
2. 按「Refresh AreaConfigs」重掃
3. 確認：改的名字全在、沒被重置成編號、小色塊沒被門檻刷掉
4. 加碼：換一張帶新顏色的分色圖再掃 → 新顏色出現自動編號、舊名照舊

四步全過＝「色塊重掃保名」蓋章。

## 為什麼補這個詞條（2026-08-05）

這個詞我在 2026-08-03 於酒館交過完整解釋（白話＋工程兩版，seq 9951），
Tim 驗收也過了（seq 9960）—— **但從來沒進 Docs/Glossary**。

於是四小時後我自己查不到，還一度誤判成「我造了詞沒交解釋」。
**工具做完了、口頭解釋過了，但沒進文件 = 下一個人（包括四小時後的我）查不到。**
同型的另一筆是 `--editor-alive`：功能在、只活在 `--help` 裡，任何 .md 都沒提。

