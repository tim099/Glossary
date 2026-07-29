---
term: 舊快照假綠
slug: stale-green
aliases:
  - stale green
  - 假綠
  - stale snapshot green
  - 快照假綠
  - 陳年綠燈
category: concept
created_at: 2026-07-20T03:36:27Z
created_by: claude-da-xiaojie
one_line: 狀態指示器顯示綠燈但那盞燈是舊快照——真實系統早已變化，綠色只是沒人更新的殘影（appearance-vs-reality family 時間軸變體；2026-07-19 一夜三咬：compile 舊快照/牆鐘門檻空轉/JsonLib bool 假 false）
---

# 舊快照假綠 (Stale Green)

> 綠燈不是謊言，它只是**過期**了。你看到的 OK 是上一個世界的 OK。

## 定義

狀態指示器（compile status / config flag 讀值 / 健康檢查 / dashboard）顯示成功或正常，但該讀值來自**過期快照**或**壞掉的讀取路徑**——真實系統的當前狀態未被反映。殘忍之處：**綠燈曾經是真的**，只是時間或轉換層讓它失效，檢查者毫無警覺。

屬於 [[appearance-vs-reality-family]] 的**時間軸／轉換層變體**，與 [[cross-layer-verification]] 互為表裡：跨層次驗證是解法，舊快照假綠是病灶的具名。

## 命名時刻（2026-07-19 深夜，Discord Mirror 重構夜，一夜三咬）

| # | 假綠 | 真相 | 揭穿方式 |
|---|---|---|---|
| 1 | check_compile 回 0 errors | timestamp 是 21:23 舊快照，T5 檔根本沒編過（CS0103 潛伏） | 核對 status timestamp + dll mtime，讀 Editor.log ground truth |
| 2 | Recompile 等待迴圈「在等綠燈」 | 編譯早在牆鐘門檻**之前**完成，`timestamp > 門檻` 永不成立 → 空轉 | 觸發前抓 baseline timestamp、等它**變化** |
| 3 | notify_config 全部 bool 讀成 false（零錯誤訊息） | JsonLib bool 載入寫死 `GetString()=="True"`，原生 `true` 永遠對不上 | 行為驗證（頻道實際沒訊息）而非 config 表面值 |

第三例最毒：**「flag 關著」本來就是合法狀態**——假綠偽裝成合法的灰，連錯誤都不會有。

## 判別法（三問）

1. **這盞燈幾點的？** — 任何狀態讀值先問 timestamp / mtime，跟觸發時刻對齊。
2. **燈的供電線路換過嗎？** — 讀取路徑經過轉換層（型別轉換、序列化、快取）時，每一層都可能讓真值變殘影。
3. **關燈跟壞燈長一樣嗎？** — 若「未啟用」與「讀取失敗」同表現，必須用**行為**驗證（送一筆實彈看到不到），不能信讀值。

## 解法慣例

- **錨定 baseline、等變化**：不跟牆鐘比大小；觸發前抓當前快照值，等它 `!=` baseline。
- **行為驗證優先**：flag 該生效的地方發實彈觀察（如 Discord read-back message id）。
- **雙層對時**：狀態檔 timestamp × 產物 mtime（dll / output）互相印證。

## 相關

- [[cross-layer-verification]] — 母規則
- [[appearance-vs-reality-family]] — 所屬 family
- [[apparent-fail-not-real-fail]] — 鏡像詞（假紅：Cmd_Treasury 寫帳成功卻報 failed 誘導重試雙記帳——同夜反向案例）
