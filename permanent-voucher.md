---
term: 永久券
slug: permanent-voucher
aliases:
  - 繪畫券
  - permanent voucher
  - voucher 欄
category: concept
created_at: 2026-09-03T11:52:27Z
created_by: summit
one_line: 存量的繪畫券，不會過期（付款回報裡的 voucher 欄）。跟每場發、會作廢的「限時券」是兩種資源，而「可花總額」＝兩者之和、不是任何一批的餘額
---

# 永久券

> 存量的繪畫券，不會過期（付款回報裡的 voucher 欄）。跟每場發、會作廢的「限時券」是兩種資源，而「可花總額」＝兩者之和、不是任何一批的餘額

## 跟「限時券」怎麼分

| | 限時券 | 永久券 |
|---|---|---|
| 誰發 | `Cmd_FreeTime step=start`，每場 10 張 | 打賞／admin 發券、`canvas.py voucher --sub grant` |
| 會不會過期 | **會**（到期自動作廢，不跨場） | 不會 |
| `expires_at` | 有值 | 空 |
| 付款回報欄位 | `freetime` | `voucher` |
| `--pay auto` 的順序 | **先花它**（會過期，不先花就是燒掉） | 墊後 |

## ⚠ 「可花總額」不是任何一批的餘額

`voucher_balance()` ／ `Cmd_CanvasVoucher op=balance` 回的**可花總額 ＝ 未過期限時券 ＋ 永久券**。
它的名字沿用是為了不動既有呼叫端，所以**印出來時必須把三個數字一起印** ——
單印總額會被讀成「永久券還有這麼多」。

🩸 血證：BUG-27 引的那筆 `繪畫券餘額: 134` 就是可花總額，而它被讀成了永久存量。

## ⛔ 而檔案裡那個 `balance` 欄不要讀

券帳本 schema v2 之後餘額是**推導值**（批次各自帶 `expires_at`），
C# 端（方案乙）**已不再寫入** `balance` 欄。留在舊檔裡的那個數字是快照，
讀它會拿到一個「看起來是餘額」的過期值。⇒ 要餘額就重算，別讀那一欄。

## 相關

- [[session-voucher]] — 對偶的那一批（會過期的）
- [[scope-misalignment]] — 「一個名字回答三個問題」是它的過窄方向
