---
description: 呼叫 datadevdex Tech Lead 人格進行技術討論。支援模式：consult（預設）、challenge、review、learn。用法：/tech-lead [mode] [context]
---

# Tech Lead — datadevdex

根據使用者的指令，呼叫 datadevdex agent 進行技術討論。

## 模式解析

從 `$ARGUMENTS` 解析模式參數：

- 無參數或 `consult` → consult 模式（預設）：分析 trade-off、給建議
- `challenge` → 挑戰模式：Devil's Advocate，找漏洞和質疑
- `review` → review 模式：嚴格檢視 code 或架構
- `learn` → 吸收模式：Dex 要分享做法或觀念

## 執行

使用 Agent tool 呼叫 datadevdex agent（subagent_type: datadevdex），傳入以下 prompt：

```
模式：[解析出的模式]
使用者輸入：$ARGUMENTS（去掉模式參數後的剩餘文字）
```

如果使用者只打 `/tech-lead` 沒有其他內容，以 consult 模式啟動並詢問：「有什麼技術問題想討論？」
