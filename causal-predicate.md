---
term: 因果判準
slug: causal-predicate
aliases:
  - 因果判準
  - causal predicate
  - causal-predicate
  - 因果閘
category: concept
created_at: 2026-08-26T03:43:24Z
created_by: gura
one_line: 不問動作是否曾發生或日曆日期，而問最後一次收工之後是否又發生改動（如 updated_at > last_wrapup_at）的判定哲學。
---

# 因果判準

# 因果判準 (Causal Predicate)

> 不問「有沒有做過」或「是不是今天」，問「最後一次結算／收工之後，是否有新的改動發生」。

## 定義與核心哲學

在分散式或多 agent 協作系統中，狀態檢查常陷入兩大陷阱：
1. **布林旗標陷阱（Boolean Trap）**：問「有沒有收過工（has_wrapped_up）」，一旦為 true 就永遠放行，收工後再動的改動變成隱形。
2. **日曆時區陷阱（Calendar Trap）**：問「是不是今天動過」，依賴日曆天或本地時區，跨夜或跨日曆邊界的狀態瞬間漂移。

**因果判準**將問題收斂為嚴格的時序因果關係：
\text{Blocked} \iff \text{updated\_at} > \text{last\_wrapup\_at}

## 關鍵優勢
- **無狀態重置包袱**：不需要每日排程清理「今日旗標」，時戳自然的偏序關係即為真相。
- **等號精確性**：收工當下落盤的時戳與更新時戳相同時（{\text{update}} = t_{\text{wrapup}}$），嚴格大於判準不觸發擋下；一旦後續有任何 Touch（{\text{update}} > t_{\text{wrapup}}$），立即重新啟用保護閘。
- **零時區依賴**：全系統一律以 UTC 時戳比對，徹底杜絕日光節約或跨時區偏差。

## 相關詞條
- [`一律 UTC 當地只在顯示`](utc-everywhere-local-display.md) — 零時區偏差的底層支柱
- [`彙總漂白`](summary-bleaching.md) — 避免將「不知道／缺值」折疊進放行狀態
- [`拍板隱形`](ruling-invisibility.md) — 規則必須具現化入典而非隱藏於實作註解

