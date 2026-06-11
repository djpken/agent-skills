# Agent Skills

**為 AI 程式代理人設計的生產等級工程技能集。**

Skills（技能）將資深工程師在建構軟體時所使用的工作流程、品質關卡與最佳實踐封裝成可重用的格式，讓 AI 代理人在每個開發階段都能一致地遵循這些規範。

![Addy 的 Agent Skills](https://addyosmani.com/assets/images/addys-agent-skills.jpg)

```
  定義          規劃           建構          驗證         審查          發布
 ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐
 │ 想法 │ ───▶ │ 規格 │ ───▶ │ 程式 │ ───▶ │ 測試 │ ───▶ │ 品管 │ ───▶ │ 上線 │
 │ 精煉 │      │ 文件 │      │ 實作 │      │ 除錯 │      │ 關卡 │      │ 部署 │
 └──────┘      └──────┘      └──────┘      └──────┘      └──────┘      └──────┘
  /spec          /plan          /build        /test         /review       /ship
```

---

## 指令

7 個斜線指令對應開發生命週期的各個階段，每個指令都會自動啟用對應的技能。

| 你正在做什麼 | 指令 | 核心原則 |
|------------|------|---------|
| 定義要建構什麼 | `/spec` | 先規格後程式碼 |
| 規劃如何建構 | `/plan` | 小而原子化的任務 |
| 逐步建構 | `/build` | 一次一個切片 |
| 證明它能運作 | `/test` | 測試即證明 |
| 合併前審查 | `/review` | 提升程式碼健康度 |
| 簡化程式碼 | `/code-simplify` | 清晰優先於聰明 |
| 發布到生產環境 | `/ship` | 越快越安全 |

規格建立後想減少手動步驟？**`/build auto`** 會在一次批准通過中自動生成計劃並實作每個任務——你只需批准計劃一次，之後自動運行。這減少了任務間的人工介入，但不會省略驗證：每個任務仍然是測試驅動且單獨提交的，遇到失敗或有風險的步驟時會暫停。

技能也會根據你的操作自動啟用——設計 API 觸發 `api-and-interface-design`，建構 UI 觸發 `frontend-ui-engineering`，以此類推。

---

## 快速開始

<details>
<summary><b>Claude Code（推薦）</b></summary>

**Marketplace 安裝：**

```
/plugin marketplace add addyosmani/agent-skills
/plugin install agent-skills@addy-agent-skills
```

> **SSH 錯誤？** Marketplace 透過 SSH 複製儲存庫。若你沒有在 GitHub 設定 SSH 金鑰，可以[新增 SSH 金鑰](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)，或使用完整的 HTTPS URL 強制以 HTTPS 複製：
> ```bash
> /plugin marketplace add https://github.com/addyosmani/agent-skills.git
> /plugin install agent-skills@addy-agent-skills
> ```

**本地 / 開發模式：**

```bash
git clone https://github.com/addyosmani/agent-skills.git
claude --plugin-dir /path/to/agent-skills
```

</details>

<details>
<summary><b>Cursor</b></summary>

將任何 `SKILL.md` 複製到 `.cursor/rules/`，或引用完整的 `skills/` 目錄。請參閱 [docs/cursor-setup.md](docs/cursor-setup.md)。

</details>

<details>
<summary><b>Antigravity CLI</b></summary>

以原生插件形式安裝，支援技能、子代理人和斜線指令。請參閱 [docs/antigravity-setup.md](docs/antigravity-setup.md)。

**從儲存庫安裝：**

```bash
agy plugin install https://github.com/addyosmani/agent-skills.git
```

**從本地複本安裝：**

```bash
git clone https://github.com/addyosmani/agent-skills.git
agy plugin install ./agent-skills
```

</details>

<details>
<summary><b>Gemini CLI</b></summary>

以原生技能安裝用於自動探索，或加入 `GEMINI.md` 作為持久化上下文。請參閱 [docs/gemini-cli-setup.md](docs/gemini-cli-setup.md)。

**從儲存庫安裝：**

```bash
gemini skills install https://github.com/addyosmani/agent-skills.git --path skills
```

**從本地複本安裝：**

```bash
gemini skills install ./agent-skills/skills/
```

</details>

<details>
<summary><b>Windsurf</b></summary>

將技能內容加入你的 Windsurf 規則設定。請參閱 [docs/windsurf-setup.md](docs/windsurf-setup.md)。

</details>

<details>
<summary><b>OpenCode</b></summary>

透過 AGENTS.md 和 `skill` 工具使用代理人驅動的技能執行。

請參閱 [docs/opencode-setup.md](docs/opencode-setup.md)。

</details>

<details>
<summary><b>GitHub Copilot</b></summary>

使用 `agents/` 中的代理人定義作為 Copilot 人格，並在 `.github/copilot-instructions.md` 中使用技能內容。請參閱 [docs/copilot-setup.md](docs/copilot-setup.md)。

</details>

<details>
  <summary><b>Kiro IDE & CLI</b></summary>
  Kiro 的技能存放在 ".kiro/skills/" 下，可存放在專案或全域層級。Kiro 也支援 Agents.md。請參閱 Kiro 文件 https://kiro.dev/docs/skills/
</details>

<details>
<summary><b>Codex / 其他代理人</b></summary>

技能是純 Markdown 格式——可與任何接受系統提示或指令檔案的代理人配合使用。請參閱 [docs/getting-started.md](docs/getting-started.md)。

</details>

---

## 全部 24 個技能

上述指令是進入點。這個技能包共包含 24 個技能——23 個生命週期技能加上 `using-agent-skills` 元技能。每個技能都是包含步驟、驗證關卡和反合理化表格的結構化工作流程。你也可以直接引用任何技能。

### 元技能 - 找出適用的技能

| 技能 | 功能 | 使用時機 |
|-----|------|---------|
| [using-agent-skills](skills/using-agent-skills/SKILL.md) | 將傳入的工作映射到正確的技能工作流程，並定義共享操作規則 | 開始一個工作階段或決定應用哪個技能時 |

### 定義 - 釐清要建構什麼

| 技能 | 功能 | 使用時機 |
|-----|------|---------|
| [interview-me](skills/interview-me/SKILL.md) | 一次一個問題的訪談，提取用戶真正想要的而非他們認為應該要的，直到約 95% 信心度 | 需求不明確，或用戶說「訪談我」/「詳細詢問我」 |
| [idea-refine](skills/idea-refine/SKILL.md) | 結構化的發散/收斂思考，將模糊想法轉化為具體提案 | 你有一個需要探索的粗略概念 |
| [spec-driven-development](skills/spec-driven-development/SKILL.md) | 在任何程式碼之前，撰寫涵蓋目標、指令、結構、程式碼風格、測試和邊界的 PRD | 開始新專案、功能或重大變更時 |

### 規劃 - 分解任務

| 技能 | 功能 | 使用時機 |
|-----|------|---------|
| [planning-and-task-breakdown](skills/planning-and-task-breakdown/SKILL.md) | 將規格分解為帶有驗收標準和依賴排序的小型可驗證任務 | 你有規格需要分解成可實作的單元時 |

### 建構 - 撰寫程式碼

| 技能 | 功能 | 使用時機 |
|-----|------|---------|
| [incremental-implementation](skills/incremental-implementation/SKILL.md) | 薄型垂直切片——實作、測試、驗證、提交。功能旗標、安全預設值、可回滾的變更 | 任何涉及多個檔案的變更 |
| [test-driven-development](skills/test-driven-development/SKILL.md) | 紅-綠-重構，測試金字塔（80/15/5），測試大小，DAMP 優於 DRY，Beyoncé 規則，瀏覽器測試 | 實作邏輯、修復 bug 或改變行為時 |
| [context-engineering](skills/context-engineering/SKILL.md) | 在正確的時機提供代理人正確的資訊——規則檔案、上下文封裝、MCP 整合 | 開始一個工作階段、切換任務，或輸出品質下降時 |
| [source-driven-development](skills/source-driven-development/SKILL.md) | 將每個框架決策都建立在官方文件上——驗證、引用來源、標記未驗證的內容 | 你想要任何框架或函式庫的權威、來源引用的程式碼時 |
| [doubt-driven-development](skills/doubt-driven-development/SKILL.md) | 對每個進行中的非平凡決策進行對抗性的新鮮上下文審查——CLAIM → EXTRACT → DOUBT → RECONCILE → STOP，支援可選的用戶授權跨模型升級 | 風險高（生產環境、安全性、不可逆）、在不熟悉的程式碼中工作，或現在驗證比後來除錯更便宜時 |
| [frontend-ui-engineering](skills/frontend-ui-engineering/SKILL.md) | 元件架構、設計系統、狀態管理、響應式設計、WCAG 2.1 AA 無障礙設計 | 建構或修改面向用戶的介面時 |
| [api-and-interface-design](skills/api-and-interface-design/SKILL.md) | 契約優先設計、Hyrum 定律、一版本規則、錯誤語義、邊界驗證 | 設計 API、模組邊界或公共介面時 |

### 驗證 - 證明它能運作

| 技能 | 功能 | 使用時機 |
|-----|------|---------|
| [browser-testing-with-devtools](skills/browser-testing-with-devtools/SKILL.md) | Chrome DevTools MCP 用於即時執行期資料——DOM 檢查、主控台日誌、網路追蹤、效能分析 | 建構或除錯任何在瀏覽器中執行的內容時 |
| [debugging-and-error-recovery](skills/debugging-and-error-recovery/SKILL.md) | 五步驟分類：重現、定位、縮小、修復、防護。停線規則，安全回退 | 測試失敗、建構中斷或行為不預期時 |

### 審查 - 合併前的品質關卡

| 技能 | 功能 | 使用時機 |
|-----|------|---------|
| [code-review-and-quality](skills/code-review-and-quality/SKILL.md) | 五軸審查、變更大小（約 100 行）、嚴重性標籤（Nit/Optional/FYI）、審查速度規範、拆分策略 | 合併任何變更之前 |
| [code-simplification](skills/code-simplification/SKILL.md) | Chesterton 柵欄、500 行規則，在保留確切行為的同時降低複雜度 | 程式碼可運作但比應有的更難閱讀或維護時 |
| [security-and-hardening](skills/security-and-hardening/SKILL.md) | OWASP Top 10 防護、認證模式、密鑰管理、依賴審計、三層邊界系統 | 處理用戶輸入、認證、資料存儲或外部整合時 |
| [performance-optimization](skills/performance-optimization/SKILL.md) | 先測量的方法——核心網頁指標目標、效能分析工作流程、套件分析、反模式偵測 | 存在效能要求或懷疑有效能退化時 |

### 發布 - 有信心地部署

| 技能 | 功能 | 使用時機 |
|-----|------|---------|
| [git-workflow-and-versioning](skills/git-workflow-and-versioning/SKILL.md) | 主幹開發、原子化提交、變更大小（約 100 行）、提交即存檔點模式 | 進行任何程式碼變更時（始終適用） |
| [ci-cd-and-automation](skills/ci-cd-and-automation/SKILL.md) | 左移、越快越安全、功能旗標、品質關卡流水線、失敗回饋迴圈 | 設定或修改建構和部署流水線時 |
| [deprecation-and-migration](skills/deprecation-and-migration/SKILL.md) | 程式碼即負債思維、強制性與建議性棄用、遷移模式、殭屍程式碼移除 | 移除舊系統、遷移用戶或停用功能時 |
| [documentation-and-adrs](skills/documentation-and-adrs/SKILL.md) | 架構決策記錄、API 文件、內聯文件標準——記錄「為什麼」 | 做出架構決策、修改 API 或發布功能時 |
| [observability-and-instrumentation](skills/observability-and-instrumentation/SKILL.md) | 結構化日誌、RED 指標、OpenTelemetry 追蹤、基於症狀的警報——邊建構邊做儀表化 | 新增遙測，或發布任何在生產環境中執行的內容時 |
| [shipping-and-launch](skills/shipping-and-launch/SKILL.md) | 發布前清單、功能旗標生命週期、分段推出、回滾程序、監控設定 | 準備部署到生產環境時 |

---

## 代理人人格

用於針對性審查的預設定專業人格：

| 代理人 | 角色 | 視角 |
|-------|------|------|
| [code-reviewer](agents/code-reviewer.md) | 資深工程師 | 五軸程式碼審查，採用「資深工程師是否會批准這個？」標準 |
| [test-engineer](agents/test-engineer.md) | QA 專家 | 測試策略、覆蓋率分析和 Prove-It 模式 |
| [security-auditor](agents/security-auditor.md) | 安全工程師 | 漏洞偵測、威脅建模、OWASP 評估 |
| [web-performance-auditor](agents/web-performance-auditor.md) | 網頁效能工程師 | 核心網頁指標審計，提供快速/深度模式和指標誠實規則；透過 `/webperf` 執行 |

---

## 參考清單

技能在需要時引用的快速參考資料：

| 參考資料 | 涵蓋範圍 |
|---------|---------|
| [testing-patterns.md](references/testing-patterns.md) | 測試結構、命名、模擬、React/API/E2E 範例、反模式 |
| [security-checklist.md](references/security-checklist.md) | 提交前檢查、認證、輸入驗證、標頭、CORS、OWASP Top 10 |
| [performance-checklist.md](references/performance-checklist.md) | 核心網頁指標目標、前後端清單、測量指令 |
| [accessibility-checklist.md](references/accessibility-checklist.md) | 鍵盤導航、螢幕閱讀器、視覺設計、ARIA、測試工具 |

---

## 技能運作原理

每個技能都遵循一致的結構：

```
┌─────────────────────────────────────────────────┐
│  SKILL.md                                       │
│                                                 │
│  ┌─ 前置資料 ──────────────────────────────────┐  │
│  │ name: lowercase-hyphen-name               │  │
│  │ description: 引導代理人完成 [任務]。        │  │
│  │              Use when…                    │  │
│  └───────────────────────────────────────────┘  │
│  概覽           → 此技能的功能                  │
│  使用時機        → 觸發條件                     │
│  流程           → 逐步工作流程                  │
│  合理化理由      → 藉口 + 反駁                  │
│  危險信號        → 出錯的跡象                   │
│  驗證           → 證據要求                     │
└─────────────────────────────────────────────────┘
```

**關鍵設計選擇：**

- **流程，而非散文。** 技能是代理人遵循的工作流程，而非閱讀的參考文件。每個都有步驟、檢查點和退出標準。
- **反合理化。** 每個技能都包含一個代理人用來跳過步驟的常見藉口表格（例如，「我稍後再加測試」）及已記錄的反論。
- **驗證是不可協商的。** 每個技能都以證據要求結束——測試通過、建構輸出、執行期資料。「看起來對」永遠不夠。
- **漸進式揭露。** `SKILL.md` 是進入點。支援參考資料只在需要時載入，保持最小 token 使用量。

---

## 專案結構

```
agent-skills/
├── skills/                            # 24 個技能（23 個生命週期 + 1 個元技能）
│   ├── interview-me/                  #   定義
│   ├── idea-refine/                   #   定義
│   ├── spec-driven-development/       #   定義
│   ├── planning-and-task-breakdown/   #   規劃
│   ├── incremental-implementation/    #   建構
│   ├── context-engineering/           #   建構
│   ├── source-driven-development/     #   建構
│   ├── doubt-driven-development/      #   建構
│   ├── frontend-ui-engineering/       #   建構
│   ├── test-driven-development/       #   建構
│   ├── api-and-interface-design/      #   建構
│   ├── browser-testing-with-devtools/ #   驗證
│   ├── debugging-and-error-recovery/  #   驗證
│   ├── code-review-and-quality/       #   審查
│   ├── code-simplification/          #   審查
│   ├── security-and-hardening/        #   審查
│   ├── performance-optimization/      #   審查
│   ├── git-workflow-and-versioning/   #   發布
│   ├── ci-cd-and-automation/          #   發布
│   ├── deprecation-and-migration/     #   發布
│   ├── documentation-and-adrs/        #   發布
│   ├── observability-and-instrumentation/ # 發布
│   ├── shipping-and-launch/           #   發布
│   └── using-agent-skills/            #   元技能：如何使用此技能包
├── agents/                            # 4 個專業人格
├── references/                        # 4 份補充清單
├── hooks/                             # 工作階段生命週期鉤子
├── .claude/commands/                  # 7 個斜線指令（Claude Code）
├── .gemini/commands/                  # 7 個斜線指令（Gemini CLI）
├── commands/                          # 8 個斜線指令（Antigravity CLI）
├── plugin.json                        # Antigravity 插件清單
└── docs/                              # 各工具的設定指南
```

---

## 為什麼需要 Agent Skills？

AI 程式代理人預設走最短路徑——這通常意味著跳過規格、測試、安全審查，以及那些讓軟體可靠的實踐。Agent Skills 提供結構化工作流程，讓代理人遵循與資深工程師在生產程式碼中相同的紀律。

每個技能都編碼了得之不易的工程判斷：*何時*撰寫規格、*測試什麼*、*如何*審查，以及*何時*發布。這些不是通用提示——它們是區分生產品質工作和原型品質工作的有主見、流程驅動的工作流程。

技能融入了 Google 工程文化的最佳實踐——包括來自 [Software Engineering at Google](https://abseil.io/resources/swe-book) 和 Google [工程實踐指南](https://google.github.io/eng-practices/) 的概念。你會在 API 設計中發現 Hyrum 定律，在測試中發現 Beyoncé 規則和測試金字塔，在程式碼審查中發現變更大小和審查速度規範，在簡化中發現 Chesterton 柵欄，在 git 工作流程中發現主幹開發，在 CI/CD 中發現左移和功能旗標，以及一個專門將程式碼視為負債的棄用技能。這些不是抽象原則——它們直接嵌入到代理人遵循的逐步工作流程中。

---

## 貢獻

技能應該**具體**（可操作的步驟，而非模糊建議）、**可驗證**（具有證據要求的清晰退出標準）、**經過實戰測試**（基於真實工作流程），以及**最小化**（只包含引導代理人所需的內容）。

請參閱 [docs/skill-anatomy.md](docs/skill-anatomy.md) 了解格式規範，以及 [CONTRIBUTING.md](CONTRIBUTING.md) 了解貢獻指南。

---

## 授權條款

MIT——在你的專案、團隊和工具中自由使用這些技能。
