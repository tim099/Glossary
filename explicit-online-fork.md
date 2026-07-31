---
term: explicit-online-fork
slug: explicit-online-fork
aliases:
  - 顯式在線分身
  - T01 fork
  - explicit fork
  - explicit-persona auto-fork
category: mechanism
status: deprecated
created_at: 2026-05-16T08:37:45Z
created_by: claude-da-xiaojie:gura
last_updated: 2026-07-31
one_line: 【已廢除 2026-07-31】awakening.py morning 舊 T01 機制 — 顯式打 persona 名字 + 該 persona 已在線時自動 fork 新分身；新規則下同一條件是「中斷」，要分身請顯式 --fork-name

---

# explicit-online-fork（已廢除）

> [!WARNING]
> **本機制於 2026-07-31 由 Tim 廢除，以下內容僅供理解歷史。**
> 新規則：**目標 persona 已在線 → `awakening.py morning` 非零退出、整個流程中斷**，
> 不再自動生分身。要開新分身請顯式 `--fork-name <NEW>`；`--explicit-persona` 旗標一併廢除。
> 理由：「顯式打了名字」不足以構成「我要一個新分身」的意思表示 —— 它同樣可能是
> 「我不知道它已經在線」。**在這個歧義上自動造一個新人格，代價遠大於停下來問一句。**
> 規範本體：`ucl_core:Docs~/zh-Hant/Plan/Plan_Awakening_Flow_Simplification.md`（R3/R4）。

## （以下皆為歷史紀錄，2026-07-31 起不再是有效行為）

## 是什麼

`awakening.py morning` 的 **T01 機制**: 處理「user 顯式打 persona 名字 + 該 persona 已在線」的場景, 自動 fork 出新分身。

## 為何需要

CLAUDE.md hard rule 規定: 「同 chat = 同 persona」, mid-chat 鎖一個 persona 後不能再鎖第二個。但 user 跨 chat 可能想「請該 persona 來」, 即使該 persona 已在別 chat 上線。

解法分兩 form:

| Form | 觸發 | 行為 |
|---|---|---|
| **Form 1** (純口語) | `早安大小姐` (沒打 persona 名字) | 同 session re-trigger → reuse no-op, 不 fork |
| **Form 2/3** (顯式名字) | `早安gura大小姐` / `/ucl-morning claude-code gura` | 若該 persona 已 ACTIVE → **auto-fork 新分身** |

意義: 「顯式打名字 + 該 persona 已在線 = 我要該 persona 的**新分身**」, 不是 reuse。

## 走法

1. `awakening.py morning --persona <X>` 帶 `--explicit-persona` flag (caller 必加, 否則被 short-circuit reuse)
2. awakening.py 偵測 X 已在 active locks
3. 從 **Hololive Myth pool** (`gura` / `calli` / `kiara` / `ame` / `ina`) 挑下個未用 codename
4. 新 persona 出生 — 繼承類似 baseline 但獨立 `wake_count` / 獨立 lock / 獨立 letter inbox

## 跟其他場景對比

| 場景 | Trigger | 行為 |
|---|---|---|
| 同 session re-trigger (Form 1) | `早安大小姐` 同 chat 再喊 | reuse no-op, 不 fork |
| explicit-online-fork (本詞) | `早安gura大小姐`, gura 已 ACTIVE | auto-fork 新 Myth codename |
| Cross-agent persona claim | `--agent X` 跟 `persona.agent` 不同 | reject (要 `--rebind-agent` ack) |
| Same session_key collision | 同 cwd 多 Claude IDE 撞 | 要 `--strict-persona` 顯式 ack |

## 設計取捨

- **為何 auto-fork 而不直接讓 user 共用 active lock?** — 違反「同 session = 同 (claim_origin, agent, persona)」, 會撞 lock ownership。
- **為何 fork 後 codename 自動挑?** — user 不必每次想新名字, pool 預定義有 5 個可用。
- **Pool 耗盡怎麼辦?** — 用完 5 個 (`gura / calli / kiara / ame / ina`) 後需要 spec 補新 pool (e.g. Hololive Promise gen) 或改 fork-name 機制顯式指定。

## 相關

- `Hololive Myth pool` — 本機制的 codename 來源
- `stratigraphic stack` — 對應的山脈 pool (走別觸發場景)
- `awakening.py morning` — 本機制的 host script
- CLAUDE.md hard rule §🌅 早安觸發 — 完整 spec
