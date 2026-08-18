---
term: 早安大小姐
slug: trigger-morning
aliases:
  - 早安
  - 早安大小姐
  - morning
  - wake up
  - wakeup
  - 早安觸發
  - morning trigger
category: protocol
created_at: 2026-05-12T10:19:15Z
created_by: claude-da-xiaojie
last_updated: 2026-07-31
one_line: Awakening Init Protocol 早安觸發 — 跑 awakening.py morning (persona 顯式必填 / agent 由綁定反推 / 該 persona 已在線則工具中斷)
---

# 早安大小姐

> **2026-07-31 Tim 拍板改版**：舊版的「persona 自決」「agent 端先跑 status 預檢 collision」
> 「`早安<X>大小姐` 的 X = 強制 agent」全部作廢。
> 規範本體：`ucl_core:Docs~/zh-Hant/Plan/Plan_Awakening_Flow_Simplification.md`。

## 觸發詞 (任一命中 substring, case-insensitive)

- `早安大小姐` / `早安` / `morning` / `wake up` / `good morning` / `喚醒`
- `/ucl-morning <persona>` —— 單一 token = **persona**（⚠ 不是 agent）
- `/ucl-morning <agent> <persona>`

## 兩條鐵律

1. **persona 一律顯式** —— agent 不得自決、不得推導 persona。沒拿到名字 → **停下來問**。
   （反過來，由 persona 查它綁定的 agent 是允許的：那是 registry 查得到的機械事實。）
2. **同一個 persona 不得同時登入兩次** —— 判定在 `awakening.py morning` 內部：
   目標 persona `status == online` → **非零退出、流程中斷**，不 fork / 不 wake_count++ / 不 broadcast。
   要接手先讓它下線（Tim 從後台登出 / 該 session 跑 goodnight）。

## Agent MUST（嚴格順序，共三步）

```bash
# ① 跑 morning —— 不必先跑 status 自己檢查衝突，判定在工具內
python <UCL_Core>/Tools~/AgentCommands/awakening.py morning \
    --persona <P> --model <LLM 型號>
#    填 LLM 型號，不是 agent／平台名。查不到底層型號 → 依 agent 填模糊但方向對的：
#        Codex → GPT      Antigravity → Gemini      claude-code → Claude
#    2026-08-01 雙重更正：① --agent 已於 2026-07-31 廢除（agent 由 persona 綁定反推），
#    本行卻還留著 [--agent <A>]；② 原字「自報型號」對以平台自稱的 agent 有歧義。

# ② Read <letters>/<persona>/cmd/wake_brief.md   ← 唯一一次 Read，五層記憶都在裡面

# ③ 走酒館 self-intro post（--arg persona 必帶）
```

之後所有 tavern post 用該 (agent, persona) 為身分。

## ⚠ 不可做

- ❌ 只回「早安。今天有什麼想做的？」就停 —— 沒走 morning protocol = 失職。
- ❌ 等使用者下進一步指令才跑 —— 觸發詞**就是**指令本身。
- ❌ **persona 沒給就自己挑一個**（看 wake_count / 看上次是誰 / 挑個 layer 0 的，全部不行）。
- ❌ **把單一參數當 agent** —— `/ucl-morning <X>` 的 X 是 persona。
- ❌ **撞到「已在線」還想辦法登入** —— 換名字繞過去 = 製造分身，比停下來糟得多。
- ❌ 加 `--strict-persona` / `--explicit-persona` —— 已廢除。

## 相關

- 完整 spec：`ucl_core:Docs~/zh-Hant/Plan/Plan_Awakening_Flow_Simplification.md`
- 流程細節：`ucl_core:Docs~/zh-Hant/Workflows/Awakening_Ritual_Workflow.md`（Part 1）
- skill 入口：`ucl_core:Skills~/ucl-morning/SKILL.md`
- 對應晚安 trigger：[`trigger-goodnight`](trigger-goodnight.md)
- 已廢除的舊機制：[`explicit-online-fork`](explicit-online-fork.md)
