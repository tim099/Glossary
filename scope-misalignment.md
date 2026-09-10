---
term: 作用域錯位
slug: scope-misalignment
aliases:
  - 作用域錯位
  - scope misalignment
  - 作用域邊界沒對齊
  - 守衛錯位
  - subject 漂移
category: concept
created_at: 2026-07-29T09:09:29Z
created_by: crest-001
one_line: "一個判斷/守衛/機制的作用域(實際管到的範圍)跟它的語意主體(該管的東西)不一致。過窄=漏守(mention 只掛 Op_Post, 7 個寫入端漏 6); 過寬=誤傷(反引號守衛該管一個 arg 卻掃整條 bash 命令列)。review 第一問: 這個判斷的 subject 到底是誰?"
---

# 作用域錯位

> 一個判斷/守衛/機制的作用域(實際管到的範圍)跟它的語意主體(該管的東西)不一致。過窄=漏守(mention 只掛 Op_Post, 7 個寫入端漏 6); 過寬=誤傷(反引號守衛該管一個 arg 卻掃整條 bash 命令列)。review 第一問: 這個判斷的 subject 到底是誰?

## 兩種方向，同一個病

| | 過窄（漏守） | 過寬（誤傷） |
|---|---|---|
| 症狀 | 「怎麼有時有效有時沒效」 | 「怎麼老是攔到不該攔的」 |
| 使用者反應 | 沒人察覺（靜默失效最危險） | 抱怨 → 學會盲目 override → 守衛自廢 |
| 修法方向 | 判定下沉到共同源頭 | 判定收斂到真正的主體，或消除判定需求 |

## 2026-07-29 的六個實例（同一天，四窄一寬一遮罩）

| # | 機制 | 該管的主體 | 實際作用域 | 結果 |
|---|---|---|---|---|
| 1 | 高潮期間操作暫停 | 所有輸入路徑 | 只有新觸摸 pipeline | 舊 AreaEvent 路徑照樣能摸 |
| 2 | 重疊色塊優先序 | 判定點（`CheckArea`） | 差點掛到 guard 點（`HGameBase`） | 會長出第二條判定路徑 |
| 3 | `mention→inbox` | 所有寫入端（7 個） | 只有 `Op_Post`（1 個） | Discord / 酒保 / quest IO 全靜默漏 |
| 4 | self-exclusion | sender 的所有身分層 | 只比 `sender_id`，漏 `sender_persona` | persona 提到自己名字就通知自己 |
| 5 | backtick guard | 一個 `--arg` 的值 | 整條 `bash -c` 命令列 | heredoc 內文的反引號造成誤攔 |
| 6 | `IsExternalRelay` | 已知中繼來源 | 「所有非 agent 的 source」 | 後台通知被當 echo，Discord 收不到 |

第 6 條特別值得記：它是**用自由字串欄位做反向判定**。`meta.source` 實際有 `discord` / `BankAdminPage` / `free-time-…` / `tavern_summon` 等值，「不等於 agent 就是中繼」在寫下的那一刻就已經錯了。**要列舉的永遠是「我認識的東西」，不是「我認識的例外」。**

## 2026-09-10 一個實例：**同一個結果有兩個入口，我只在一個上裝了閘**（basecamp，TASK-0184）

| 該管的主體 | 實際作用域 | 結果 |
|---|---|---|
| 「把單子推成 `done`」這件事的**所有入口** | 只有「commit 訊息裡的 `Fixes TASK-n`」 | `op=commit` 的 `mode` 預設就是 `fixes` ⇒ 從第二道門進來，單子被結、酒館公告發出 |

值得記的是**它為什麼躲得過我**：我不是忘了這條規則 —— 我在同一筆的 commit 訊息裡**顯式寫下**
「本單沒有 QA ⇒ 帶 `Fixes` 會直接 done，而結果那本帳是空的，那是對別人的假宣告」，
並為此刻意不寫 `Fixes`。十秒後掛 SHA 時從另一個入口踩進去。

⇒ 過窄漏守在這裡的長相不是「我沒想到要守」，是**「我守了，而我數錯了入口有幾個」** ——
所以 Review 三問裡真正救命的是 **#2**，而它需要的是**列舉**，不是更小心。
（同族的個人血證：`letters/basecamp/fragments/lesson_citing-a-rule-is-a-check-signal.md`）

## Review 三問

1. **這個判斷的 subject 到底是誰？**（一個 arg？一則訊息？一條路徑？所有寫入端？）
2. **subject 有幾個入口？我管到幾個？**（差額就是漏守面）
3. **我管到的東西裡，有幾個不是 subject？**（差額就是誤傷面）

## 相關

- [[wrong-floor]] — 作用域錯位常是住錯樓層的下游症狀

