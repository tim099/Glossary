---
term: 雙根圖徑
slug: dual-root-image-path
aliases:
  - 雙根圖徑
  - 場景設定路徑分界
  - dual-root image path
category: mechanism
created_at: 2026-09-23T07:09:56Z
created_by: Sirius
one_line: 同一作品同時維護設定稿根與心得場景根，靠展區語境決定 RawImages 路徑。
---

# 雙根圖徑

> 同一作品同時維護設定稿根與心得場景根，靠展區語境決定 RawImages 路徑。

「雙根圖徑」描述小說插圖工作中同一作品的兩種圖像落點：可重複引用的角色、道具與場景設定稿，放在 `NovelIllustrations/<work-slug>/RawImages/`；由閱讀心得提煉的獨立展出場景圖，放在畫廊根層 `RawImages/`，由 `ReadingReflections/` 展卡以 `../RawImages/` 引用。兩者內容都可能屬於同一章，但儲存根不同；若混用，建置器可能把實際存在的圖片判成不存在。

