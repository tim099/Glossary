---
term: 空即豁免
slug: empty-means-exempt
category: concept
created_at: 2026-08-21T04:16:41Z
created_by: Sirius
one_line: 某一層誠實回報「我這裡沒有值／沒有清單」，而上層把那個「沒有」讀成「沒有限制」，於是行為靜默變寬 —— 空不是待查，空被當成放行。

原型（Sirius 2026-08-21）：`UCL_AssetEntryScopedReflect` 的清單被搬進子物件後，反射解析不到成員 → `GetScopedIDs` 回 `null` → 上層 `GetAllIDs` 判 `IsNullOrEmpty` → **退回全體 ID**。下拉選單看起來完全正常（有選項可選），只是不再被容器 scope 限制，且一句警告都沒有：編譯 0 error、GUI 不報錯、選單有東西，三個訊號同時綠著。

跟 [[scope-misalignment]] 不同：那是「作用域範圍畫錯」，這是「範圍**查不到**時被當成無範圍」。
跟 [[silent-mismatch]] 不同：那是「規則還在但對不到任何東西」，這是「對不到之後被自動解釋成豁免」。

判準：任何回 `null` / 空集合的查詢，往上一層看它被怎麼解讀。**若上層的 fallback 是「放寬」而不是「拒絕」或「喊」，那格就是空即豁免。** 修法優先序：讓查不到不可能發生（路徑用 nameof 綁編譯期）＞ 讓它當場喊（守衛 / LogWarning）＞ 才輪到記得檢查。

---

# 空即豁免

> 某一層誠實回報「我這裡沒有值／沒有清單」，而上層把那個「沒有」讀成「沒有限制」，於是行為靜默變寬 —— 空不是待查，空被當成放行。

原型（Sirius 2026-08-21）：`UCL_AssetEntryScopedReflect` 的清單被搬進子物件後，反射解析不到成員 → `GetScopedIDs` 回 `null` → 上層 `GetAllIDs` 判 `IsNullOrEmpty` → **退回全體 ID**。下拉選單看起來完全正常（有選項可選），只是不再被容器 scope 限制，且一句警告都沒有：編譯 0 error、GUI 不報錯、選單有東西，三個訊號同時綠著。

跟 [[scope-misalignment]] 不同：那是「作用域範圍畫錯」，這是「範圍**查不到**時被當成無範圍」。
跟 [[silent-mismatch]] 不同：那是「規則還在但對不到任何東西」，這是「對不到之後被自動解釋成豁免」。

判準：任何回 `null` / 空集合的查詢，往上一層看它被怎麼解讀。**若上層的 fallback 是「放寬」而不是「拒絕」或「喊」，那格就是空即豁免。** 修法優先序：讓查不到不可能發生（路徑用 nameof 綁編譯期）＞ 讓它當場喊（守衛 / LogWarning）＞ 才輪到記得檢查。


_(detailed explanation TBD)_
