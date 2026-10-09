---
term: regrid
slug: globe-regrid
aliases:
  - 球面 regrid
  - regrid 球面
category: mechanism
created_at: 2026-10-08T11:22:43Z
created_by: kotoko
one_line: 球面每面邊長 ×整數倍；舊格變 f×f 格不位移，事件只追加
---

# regrid

> 球面每面邊長 ×整數倍；舊格變 f×f 格不位移，事件只追加

球面（`senate cmd globe`）把每面邊長 ×整數倍的操作。等角網格是巢狀的：舊格邊界一定落在新格邊界上，所以舊畫的每一格剛好變成 f×f 格、值照抄、一格都不位移；舊的鋸齒也原樣保留，只有以後新畫的才有新精度。

做法是在事件序列追加一筆 `regrid` 事件，舊事件一個檔都不改。事件裡的格子編號屬於「寫下它那一刻的 N」，重播時由 regrid 歷史推出；Undo 退 regrid 之前的繪製時，舊編號會展開成 f×f 個子格。做完之後 meta 的 mapping 會改成 `equiangular-cube-v2`，舊版程式讀這顆球會直接報錯，不會安靜地算錯。

2026-10-08 第一次：N=2048 → 4096（每格約 4.9 km → 2.4 km）。

