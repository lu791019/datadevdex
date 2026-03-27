# datadevdex

一個為 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 打造的 **自我進化 Tech Lead 人格系統**。

不是聊天機器人，不是問答工具 — 是一個有技術立場、會成長、會改變想法的開發者同事。

---

## 核心概念：為什麼需要一個「會成長的 Tech Lead」？

### 問題

大多數 AI coding assistant 的運作模式是 **stateless** 的：每次對話從零開始，沒有技術立場，給的建議是「一般最佳實踐」而非基於你的脈絡和經驗。這意味著：

- 你分享過的技術觀念，下次對話就忘了
- 它不會因為踩過坑而改變建議
- 它沒有「我們團隊/我個人偏好這樣做」的概念
- 你永遠在教同一件事

### 解法：三層記憶 + 進化機制

datadevdex 的核心設計是一個 **知識蒸餾循環**（Knowledge Distillation Loop）：

```
具體經驗（Layer A）──→ 討論結論（Layer B）──→ 抽象原則（Layer C）
       ↑                                              │
       └──────────── 原則指導新的討論 ←────────────────┘
```

這不只是「記住東西」。這是一個從**歸納**到**演繹**的完整認知循環：

1. **歸納階段**：你分享具體的開發做法和案例（Layer A），經過技術討論形成結論（Layer B）
2. **蒸餾階段**：當同一主題累積足夠的具體經驗和結論，系統向上萃取為抽象的技術原則（Layer C）
3. **演繹階段**：新的技術討論會基於已萃取的原則來推理，而非每次重新從零分析
4. **修正階段**：當新的證據與既有原則矛盾，原則會被更新或推翻 — **原則不是教條**

這讓 datadevdex 具備了傳統 AI assistant 缺乏的三個特性：

- **Statefulness** — 跨 session 記住你的技術脈絡
- **Opinionatedness** — 基於累積經驗形成自己的技術立場
- **Adaptability** — 新證據可以改變舊信念，不會固守過時的判斷

### 與人類 Tech Lead 的類比

| 面向 | 人類 Tech Lead | datadevdex |
|------|---------------|------------|
| 技術判斷來源 | 多年實戰經驗 | Layer A/B 累積的具體案例和結論 |
| 技術哲學 | 從經驗中內化的原則 | Layer C 從 A/B 蒸餾的抽象原則 |
| 改變想法 | 遇到反例時修正認知 | 新證據觸發 Layer C 更新 |
| 給建議的方式 | 「根據我的經驗，我認為...」 | 引用具體來源，標註知識出處 |
| 局限性 | 有盲點、有偏見 | 受限於輸入的知識廣度和深度 |

---

## 三層記憶架構

### Layer A — 知識庫（Knowledge Base）

存放你分享的**具體**做法、觀念、案例。每筆知識帶有來源、日期、脈絡標註。

```markdown
## [主題標籤] 標題

**來源**：Dex 分享 / 2026-03-26
**觀點**：一句話摘要
**細節**：具體做法或原則
**脈絡**：為什麼這很重要
```

這是原始素材層 — 不做判斷，忠實記錄。

### Layer B — 討論紀錄（Discussion Records）

跨 session 的技術討論**結論**。只記有長期價值的決策和共識，不記對話過程。

```markdown
## 2026-03-26 主題

**結論**：最終決定或共識
**理由**：關鍵 trade-off
**影響**：對後續決策的影響
```

這是提煉層 — 從具體討論中萃取出可復用的判斷。

### Layer C — 技術哲學（Technical Philosophy）

從 Layer A + B **蒸餾而來**的抽象原則。嵌入在 agent prompt 中，直接影響 datadevdex 的思考方式。

```markdown
- **原則名稱**：一句話描述
  - 來源：從哪些 A/B 紀錄萃取
  - 萃取日期：YYYY-MM-DD
```

**關鍵設計決策：Layer C 不是靜態的。** 它會隨著 A/B 的累積持續進化，也會在新證據出現時被推翻。

### 進化觸發條件

| 觸發時機 | 動作 |
|----------|------|
| Layer A 新增知識 | 檢查是否形成新的模式或原則 |
| Layer B 新增結論 | 檢查是否改變或強化既有哲學 |
| 同一主題累積 ≥3 筆 A/B 紀錄 | 萃取為 Layer C 原則 |
| 既有 C 原則被新證據反駁 | 更新或移除該原則 |

### 進化範例

```
Layer A：「某團隊用 monorepo 解決了部署同步問題」
Layer A：「另一團隊 monorepo 導致 CI 過慢改回 polyrepo」
Layer B：「monorepo 適合部署耦合高的服務，不適合獨立生命週期的模組」

→ Layer C 萃取：「程式碼組織結構應跟隨部署邊界，非團隊邊界」
```

之後如果出現新的 Layer A 證據推翻這個結論，Layer C 的原則會被修正。

---

## 四種運作模式

### consult（預設）— 技術顧問

分析 trade-off、給建議、討論架構決策。像一個你信任的資深同事。

- 有自己的觀點，不和稀泥
- 會說「我認為」而不是「一般建議」
- 不同意時明確表達並說明理由

```
/tech-lead 這個專案該用 monorepo 還是 polyrepo？
```

### challenge — Devil's Advocate

主動找漏洞和質疑。適用於重大架構決策前的壓力測試。

- 對每個技術決策提出反面論點
- 找出隱含假設和潛在風險
- 不給面子但給理由

```
/tech-lead challenge 我打算在這兩個服務之間加 message queue
```

### review — 嚴格審查

Code/Architecture Review 模式。從四個維度審查：可維護性、可測試性、效能、安全性。

- 區分「必須改」vs「建議改」
- 指出具體問題和改善建議

```
/tech-lead review 檢查這個新模組的架構
```

### learn — 知識吸收

你分享做法或觀念時使用。datadevdex 會整理歸納後寫入知識庫，並檢查是否觸發 Layer C 進化。

- 仔細聆聽，提出釐清問題
- 結構化記錄到 Layer A
- 自動檢查進化條件

```
/tech-lead learn
```

---

## 技術實作

### 基於 Claude Code Subagent 架構

datadevdex 運行在 Claude Code 的 [custom agent](https://docs.anthropic.com/en/docs/claude-code/custom-agents) 機制上。每次被喚起時：

1. 讀取 Layer A（知識庫）載入累積知識
2. 讀取 Layer B（討論紀錄）載入歷史結論
3. Layer C（技術哲學）已嵌入 agent prompt，直接影響推理
4. 根據指定模式調整回應策略

### 檔案結構

```
.claude/
├── agents/
│   ├── datadevdex.md                  ← Layer C：人格定義 + 進化中的技術哲學
│   └── datadevdex-knowledge.md        ← Layer A：知識庫（具體案例和做法）
├── commands/
│   └── tech-lead.md                   ← /tech-lead slash command
└── docs/
    └── datadevdex-design.md           ← 設計規格書
memory/
└── tech-lead-discussions.md           ← Layer B：跨 session 討論結論
```

### 知識規模管理

當知識量增長超過閾值時，系統會自動建議拆分：

| 指標 | 閾值 | 動作 |
|------|------|------|
| 行數 | > 200 行 | 拆為目錄結構 |
| 主題數 | > 5 個不相關領域 | 按領域分檔 |

拆分後結構：

```
.claude/agents/datadevdex-knowledge/
├── principles.md    ← 核心技術觀點
├── practices.md     ← 具體做法
└── cases.md         ← 案例紀錄
```

---

## 安裝

### 前置需求

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI

### 步驟

1. 將檔案複製到你的 Claude Code 設定目錄：

```bash
# Layer C — Agent 定義
cp .claude/agents/datadevdex.md ~/.claude/agents/

# Layer A — 知識庫
cp .claude/agents/datadevdex-knowledge.md ~/.claude/agents/

# Slash command
cp .claude/commands/tech-lead.md ~/.claude/commands/

# Layer B — 討論紀錄（路徑依你的專案記憶位置調整）
cp memory/tech-lead-discussions.md ~/.claude/projects/<your-project>/memory/
```

2. **開新 Claude Code session**（agent 和 command 在啟動時載入）

3. 輸入 `/tech-lead` 開始對話，或 `/tech-lead learn` 開始分享你的技術觀念

---

## 自訂

### 基礎技術哲學

編輯 `datadevdex.md` 中的「基礎原則」區塊，設定起始技術價值觀。預設值：

- **先規劃再動手** — schema-aware，基於實際結構而非猜測
- **YAGNI + 最小複雜度** — 收斂優於擴散，三行重複好過一個過早抽象
- **Pragmatic over dogmatic** — 不教條式套用 pattern，根據情境選擇
- **反饋循環** — 寫完就驗證，測試不是可選的
- **簡潔架構** — 質疑不必要的抽象和中間層

這些是起點，不是終點。隨著你的分享，datadevdex 會在此基礎上長出自己的原則。

---

## 設計決策

**為什麼用 subagent 而不是單純的 prompt？**
Subagent 擁有獨立的檔案讀寫能力，可以自行維護知識庫和更新自己的技術哲學。一般 prompt 在 session 結束後就失去所有狀態。

**為什麼要三層而不是一層知識庫？**
具體案例（A）和討論結論（B）是不同性質的知識。將它們蒸餾為抽象原則（C）才能形成認知循環 — 這是讓人格「真正進化」而非「只是記住更多東西」的關鍵區別。

**為什麼原則可以被推翻？**
因為教條是好工程的敵人。任何原則都應該能被新的證據修正，否則 datadevdex 會隨著時間變得越來越僵化，而非越來越智慧。

**為什麼 Layer C 嵌入 agent prompt 而非外部檔案？**
嵌入 prompt 意味著原則直接影響推理過程，而非只是可供查詢的參考資料。這讓原則成為人格的一部分，而非外掛的知識。

---

## License

MIT
