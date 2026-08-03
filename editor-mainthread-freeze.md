---
term: 主執行緒卡死
slug: editor-mainthread-freeze
aliases:
  - 主執行緒卡死
  - Editor 卡住
  - Editor 卡死
  - 編輯器凍結
  - 卡住主執行緒
  - 卡住 mainthread
  - mainthread blocking
  - editor freeze
  - Unity 無回應
category: engineering
created_at: 2026-08-03T07:00:00Z
created_by: summit
one_line: "Editor 主執行緒被同步重活（外部 process 等待 / 重 IO / OCR / 截圖）擋住 → 整個 Unity 凍結無回應影響基本操作。解法=UniTask 非同步化（Editor 模式可用, await 恢復點自動落主執行緒）。實戰模式與六條地雷已收斂在工作記憶 unitask-editor-async（work_memory.py read --topic unitask-editor-async）, 含本 repo 可抄範例: Task.Run 包阻塞呼叫 / .Forget() / 防重入 guard 要活過 async / IMGUI 繪製禁 async / out 參數消失防靜默。案例: 2026-08-03 AdminPage OCR 定位同步跑 python 子程序, Editor 凍結數十秒, Tim 全面 async 化根治。"
---

# 主執行緒卡死（editor-mainthread-freeze）

> Editor 主執行緒被同步重活擋住 → 整個 Unity 凍結、基本操作全部無回應。

## 症狀

- 按一顆按鈕，Unity 整個停住幾秒到幾十秒（轉圈／白視窗／無回應）
- 外部 process（python 腳本、OCR、截圖、網路請求）在主執行緒同步 `WaitForExit` / 同步讀
- `Thread.Sleep` 出現在 Editor 主執行緒路徑上

## 解法：UniTask 非同步化

**Editor 模式完全可用**（不需 PlayMode）——UniTask 的 continuation 走 `EditorApplication.update`，`await` 之後恢復點自動落在主執行緒，可以安全碰 Unity API。

**實戰模式與地雷（六條，含本 repo 可抄範例與踩坑史）**：
```bash
python <UCL_Core>/Tools~/AgentCommands/work_memory.py read --topic unitask-editor-async
```

速記版：阻塞呼叫包 `Task.Run` → `await`；按鈕 handler 掛 `.Forget()`；防重入旗標要活過整段 async（不是 `try{X().Forget()}finally` — 那同幀就放鎖）；**IMGUI 繪製方法禁 async**；async 化時 `out` 參數會消失，錯誤訊息必須顯式接住。

## 案例

2026-08-03：`UCL_BartenderAdminPage` 自動通知／OCR 定位流程原為同步——每次 OCR 跑 python 子程序數秒起跳，Editor 全程凍結影響基本操作。Tim 將 `RunOnce`／`RunCursorTest`／`CapturePreview`／`ListMonitors` 全面 async 化根治；summit 代修三隻 async 化回歸（guard 旗標／錯誤訊息漏接／繪製方法 async 地雷）。
