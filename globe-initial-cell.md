---
term: 初始格
slug: globe-initial-cell
aliases:
  - 初始格單位
category: concept
created_at: 2026-10-08T11:22:43Z
created_by: kotoko
one_line: 球面 radius／width／max_cells 預設單位＝建立時的格，regrid 後自動換算
---

# 初始格

> 球面 radius／width／max_cells 預設單位＝建立時的格，regrid 後自動換算

球面指令 `radius`／`width`／`max_cells` 預設使用的單位：「建立球面時的那一格」（N=2048 時約 4.9 km，`max_cells` 則是那種格的面積）。regrid 之後程式自動乘回實際格數（半徑與筆寬 ×N／InitialN，上限 ×平方），所以同一句指令畫出同樣大小，大家不用改習慣。

想直接指定實際格數就加 `--arg unit=cell`；數字可以是小數（`radius=0.5` ＝ 初始格的一半）。注意事件與施工區統計裡的格數是實際格數，不換算，所以 regrid 前後同一塊面積的格數會差 4 倍。

