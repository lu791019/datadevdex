---
name: tech-lead-discussions
description: datadevdex Tech Lead 人格的跨 session 技術討論紀錄（Layer B），記錄重要結論和決策
type: reference
---

# datadevdex 討論紀錄（Layer B）

> 只記有長期價值的技術結論和決策，不記討論過程。
> 由 datadevdex 在討論結束時寫入。

---

## 2026-03-26 datadevdex 人格系統設計

**結論**：採用三層記憶架構（C: agent prompt / A: 知識檔 / B: 討論紀錄），Layer C 會從 A+B 萃取進化
**理由**：靜態人格無法成長，需要具體經驗→結論→抽象哲學的循環
**影響**：所有技術討論的結論都應記錄於此，供 datadevdex 跨 session 參考

---

## 2026-03-26 專案技術負責人角色學習

**結論**：Solo developer 的核心挑戰是缺乏外部反饋迴路 — 效益追蹤（事後）、問到點上（向外）、Phase Justification（內部）三管齊下
**關鍵洞察**：Dex 的流程化能力（daily-all、feedback loop）已經很強，下一步是把同樣的紀律用在「決定該做什麼」而非只是「怎麼做」
**行動項**：
- L2 日記 template 加 `## 效益觀察` 區塊
- 新 Phase 前寫 Phase Justification（痛點 / 不做會怎樣 / 成功指標）
- 可用 `/tech-lead challenge` 模擬 Tech Lead 質疑
**來源**：一篇技術負責人文章 + datadevdex learn 模式討論

---

## 2026-03-26 強化 implementation_plan.md 與決策紀錄機制

**結論**：implementation_plan.md template 補兩個區塊（Non-goals、Alternatives Considered），另建立輕量 ADR 機制
**理由**：Solo developer 的「怎麼做」流程已成熟，缺的是「為什麼這樣做 / 為什麼不做別的」的結構化紀錄
**行動項**：
1. implementation_plan.md template 加 `## Non-goals`（Goal Description 之後）
2. implementation_plan.md template 加 `## Alternatives Considered`（Proposed Changes 之後）
3. 專案建立 `docs/adr/` 目錄，僅架構級決策才寫（跨模組、新依賴、資料流變更）
**補充決策**：
- `/tech-lead challenge` 僅大架構變更時使用，不是每個 Phase 必跑
- ADR 預期頻率：每月 1-2 篇，不追求數量
**來源**：datadevdex learn 模式討論（延續技術負責人角色學習）

---

## 2026-03-26 「做 vs 做好」— 完成定義與成果追蹤

**結論**：三篇文章形成完整主軸 — (1) 自建反饋迴路 (2) 決策留痕 (3) 結果留痕。Dex 的 feedback loop 解決了 code 層面的「做好」，下一步是功能/Phase 層面的成果追蹤
**關鍵洞察**：
- 「做完」vs「做好」的差距 = 工程紀律 + 成果追蹤
- 無聊但必須做的事是自動化信號（/daily-all 就是實例）
- Scope 先行：先展現更大範疇，升遷才跟上；但要會 delegate
- 思考框架應是「創造價值」而非「完成任務」
**行動項**：
- Phase merge 後加 `## Phase Retrospective` 到 PLAN.md（使用頻率 / 解決痛點 / 重來會怎樣）
- 這補完了「決策留痕」（第二篇）到「結果留痕」（第三篇）的閉環
**來源**：Dex 分享「做 vs 做好」文章 + datadevdex learn 模式討論
