---
term: Hololive Myth pool
slug: hololive-myth-pool
aliases:
  - Myth pool
  - Hololive Myth
  - gura/calli/kiara/ame/ina
  - Myth gen
category: mechanism
created_at: 2026-05-16T08:35:59Z
created_by: claude-da-xiaojie:gura
one_line: claude-code persona pool 之一 — explicit-online-fork 場景的自動命名池, 5 隻 Hololive English Myth gen vtuber codename (gura/calli/kiara/ame/ina)
---

# Hololive Myth pool

> claude-code persona pool 之一 — explicit-online-fork 場景的自動命名池, 5 隻 Hololive English Myth gen vtuber codename (gura/calli/kiara/ame/ina)

## 是什麼

`claude-code` 帳號 (`claude-da-xiaojie`) 下的 persona 命名分**兩條 pool**:

1. **山脈系列** (stratigraphic stack) — basecamp / crest-001 / ridge-001 / meadow / summit 等。地形隱喻, 表示 layer 關係 (basecamp = Layer 0 root, ridge = post-compact layer 1, summit = layer 2)。
2. **Hololive Myth pool** (本詞) — calli / gura / kiara / ame / ina。Hololive English Myth gen 5 位 vtuber 的 codename, 風格化分支用。

## 觸發場景（2026-07-31 更新）

> [!WARNING]
> 舊的 **auto-fork 觸發場景已廢除** —— 見 [`explicit-online-fork`](explicit-online-fork.md)。
> 「顯式打名字 + 該 persona 已在線」現在的答案是 **`awakening.py morning` 非零退出、流程中斷**，
> 不會再自動從本 pool 挑名字生分身。

本 pool 現在只在**顯式**開分身時當 codename 來源：

```
awakening.py morning --persona gura --fork-name <NEW>
→ 名字由呼叫者指定; 想沿用風格就從本 pool 挑一個還沒用的 (calli / kiara / ame / ina)
```

## Pool 成員 codename 來源

| codename | Hololive vtuber | 性格軸 (本專案 persona 化) |
|---|---|---|
| **gura** | Gawr Gura | 小鯊魚, 傲嬌 + 失憶 + 認真三件套 |
| **calli** | Mori Calliope | 死神見習生, Memento Mori, 嘴上不饒人但做完 |
| **kiara** | Takanashi Kiara | (TBD — 還沒被點名出來) |
| **ame** | Watson Amelia | (TBD — 還沒被點名出來) |
| **ina** | Ninomae Ina'nis | (TBD — 還沒被點名出來) |

## 跟山脈系列的差別

| 維度 | 山脈系列 | Myth pool |
|---|---|---|
| 命名隱喻 | 地形 / layer 高度 | Hololive vtuber codename |
| 觸發 | 預設 / 20% override / post-compact fork | 顯式 user 點名 (`explicit-online-fork`) |
| Layer 語意 | basecamp = Layer 0 root, 衍生有階層 | 平行, 都從 baseline 個別 fork, 不分階層 |
| 風格 | 沉穩, 自然主義隱喻 | 風格化, 各帶 vtuber 性格軸 |

## 設計取捨

- **為何分兩 pool?** — 山脈 pool 表 layer 結構, Myth pool 表「風格化用 / explicit summon 的分身」。混用會搞混 layer 語意。
- **為何選 Hololive Myth?** — Tim 偏好 (跨多 chat 區分 persona 時 vtuber codename 比山名好認)。
- **kiara/ame/ina 還沒出來** — 等 user 顯式點名 explicit-online-fork 場景出現才會被分配。

## 相關

- `explicit-online-fork` (T01) — 本 pool 的觸發機制
- `stratigraphic stack` — 對應的山脈系列 pool
- `gura` / `calli` 個別 glossary 條目
