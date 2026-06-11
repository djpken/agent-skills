# Scenario Routing — 情境技能路由指南

這份文件以真實開發情境為出發點，幫助你快速找到對應的技能或指令組合。

每個情境包含：**觸發訊號 → 推薦技能鏈 → 跳過警示**。

---

## 目錄

1. [從零開始一個新功能](#1-從零開始一個新功能)
2. [修復生產環境 Bug](#2-修復生產環境-bug)
3. [重構既有程式碼](#3-重構既有程式碼)
4. [設計新的 API / 模組邊界](#4-設計新的-api--模組邊界)
5. [建構 UI 元件](#5-建構-ui-元件)
6. [準備程式碼審查](#6-準備程式碼審查)
7. [部署到生產環境](#7-部署到生產環境)
8. [安全性稽核](#8-安全性稽核)
9. [效能問題排查](#9-效能問題排查)
10. [棄用舊系統或遷移](#10-棄用舊系統或遷移)
11. [建立 CI/CD 流水線](#11-建立-cicd-流水線)
12. [需求模糊，要從想法開始](#12-需求模糊要從想法開始)

---

## 情境路由快速參照表

| 情境 | 主要指令 | 核心技能鏈 |
|------|---------|-----------|
| 新功能（需求明確） | `/spec` → `/plan` → `/build` → `/test` → `/review` → `/ship` | 完整生命週期 |
| 新功能（需求模糊） | `interview-me` → `/spec` → ... | 先訪談再規格 |
| 生產環境 Bug | `/test` → `/review` | debugging → TDD → review |
| 重構 | `/code-simplify` → `/review` | simplification → review |
| 設計 API | `/spec` → `/plan` | api-and-interface-design |
| 建構 UI | `/build` | frontend-ui-engineering |
| 程式碼審查 | `/review` | code-review-and-quality |
| 部署 | `/ship` | shipping-and-launch |
| 安全稽核 | `/review` | security-and-hardening |
| 效能調優 | `/review` | performance-optimization |
| 棄用/遷移 | `/plan` → `/build` | deprecation-and-migration |
| CI/CD 設定 | `/plan` → `/build` | ci-cd-and-automation |

---

## 1. 從零開始一個新功能

### 觸發訊號

- 新的 issue / ticket 或 PRD 出現
- 用戶說「幫我實作 X」、「新增功能 Y」
- 需要跨多個檔案的變更

### 推薦技能鏈

```
spec-driven-development
        │
        ▼
planning-and-task-breakdown
        │
        ▼
incremental-implementation  ←──── test-driven-development
        │                               （每個任務）
        ▼
code-review-and-quality
        │
        ▼
git-workflow-and-versioning
        │
        ▼
shipping-and-launch
```

### 指令序列

```bash
/spec       # 寫出 PRD，定義目標與邊界
/plan       # 將 PRD 分解為可實作的原子任務
/build      # 逐步實作，每個切片都測試
/test       # 確保每個行為都有測試覆蓋
/review     # 合併前五軸審查
/ship       # 部署前最終確認清單
```

> **自動模式：** 規格確定後，`/build auto` 可自動完成規劃 + 實作，無需手動介入每個任務。

### 跳過警示

- 跳過 `/spec` → 代理人會假設需求，導致事後大量重工
- 跳過 `/test` → 無法證明行為正確，審查也會缺少依據

---

## 2. 修復生產環境 Bug

### 觸發訊號

- 測試失敗、線上告警、用戶回報異常
- 行為不符預期，原因未知

### 推薦技能鏈

```
debugging-and-error-recovery
        │  （重現 → 定位 → 縮小 → 修復 → 防護）
        ▼
test-driven-development
        │  （先寫能重現 bug 的失敗測試，再修復）
        ▼
code-review-and-quality
        │  （確認修復不引入新問題）
        ▼
git-workflow-and-versioning
```

### 指令序列

```bash
/test       # 先寫重現 bug 的失敗測試（Prove-It 模式）
/review     # 修復後審查，確認邏輯正確
```

### Prove-It 模式

1. 寫一個能重現 bug 的測試（預期失敗）
2. 提交：`fix: add regression test for <issue>`
3. 修復 bug，確認測試轉為通過
4. 提交修復

### 跳過警示

- 直接修復不寫測試 → 同一個 bug 可能在未來回歸
- 不縮小重現範圍 → 修復了錯誤的地方

---

## 3. 重構既有程式碼

### 觸發訊號

- 程式碼可運作，但閱讀或維護困難
- 函式超過 500 行（Rule of 500）
- 發現重複的抽象或不一致的命名

### 推薦技能鏈

```
code-simplification
        │  （Chesterton 柵欄：先理解再移除）
        ▼
test-driven-development
        │  （確保行為在重構前後完全相同）
        ▼
code-review-and-quality
```

### 指令序列

```bash
/code-simplify   # 找出可簡化的地方，保留行為
/test            # 確認測試在重構前後都通過
/review          # 審查重構是否引入了新的複雜度
```

### Chesterton 柵欄原則

> 在理解為何存在之前，不要移除任何東西。

重構前先問：
- 這段程式碼為何這樣寫？
- 移除它會破壞什麼隱含的約定？
- 有測試覆蓋這個行為嗎？

### 跳過警示

- 在沒有測試覆蓋的情況下重構 → 行為靜默改變無法發現
- 順手「順便修復」無關的程式碼 → 審查範圍爆炸

---

## 4. 設計新的 API / 模組邊界

### 觸發訊號

- 需要定義 REST/GraphQL 端點
- 設計模組的公開介面或型別契約
- 建立前後端之間的邊界

### 推薦技能鏈

```
api-and-interface-design
        │  （契約優先設計，考慮 Hyrum 定律）
        ▼
spec-driven-development
        │  （文件化 API 契約與錯誤語義）
        ▼
source-driven-development
        │  （驗證框架的官方用法）
        ▼
doubt-driven-development
        │  （對設計決策進行對抗性審查）
        ▼
documentation-and-adrs
```

### 關鍵決策清單

```
□ 已定義錯誤型別與 HTTP 狀態碼語義
□ 版本策略確定（URL 版本 / 標頭版本 / 無版本）
□ 已考慮 Hyrum 定律：隱式行為也是契約
□ 邊界驗證只在系統入口執行
□ 重大變更的向後相容策略已記錄在 ADR 中
```

### 跳過警示

- 跳過 ADR → 三個月後沒人知道為什麼這樣設計
- 不做邊界驗證 → 內部錯誤洩漏到外部呼叫者

---

## 5. 建構 UI 元件

### 觸發訊號

- 新增或修改面向用戶的介面
- 需要回應式設計或無障礙設計支援

### 推薦技能鏈

```
frontend-ui-engineering
        │  （元件架構、設計系統、狀態管理）
        ▼
browser-testing-with-devtools
        │  （Chrome DevTools MCP 即時驗證）
        ▼
test-driven-development
        │  （元件行為測試）
        ▼
performance-optimization
        │  （Core Web Vitals 確認）
        ▼
code-review-and-quality
```

### 指令序列

```bash
/build      # 實作 UI，同步使用 browser-testing-with-devtools 驗證
/test       # 元件行為測試
/webperf    # 執行 Core Web Vitals 審計（使用 web-performance-auditor）
/review     # 五軸審查，包含無障礙設計
```

### 跳過警示

- 不在真實瀏覽器中驗證 → 無法發現渲染問題和 DevTools 才能看到的錯誤
- 跳過無障礙設計檢查 → WCAG 2.1 AA 合規問題上線後才發現

---

## 6. 準備程式碼審查

### 觸發訊號

- Pull Request 準備合併
- 需要確認程式碼品質達到標準

### 推薦技能鏈

```
code-review-and-quality
        │  （五軸：正確性、可讀性、架構、安全性、效能）
        ▼
security-and-hardening  ←── 若涉及用戶輸入或認證
        │
        ▼
git-workflow-and-versioning
        │  （確認提交歷史乾淨，變更大小合理）
```

### 五軸審查框架

| 軸向 | 關鍵問題 | 嚴重性門檻 |
|-----|---------|----------|
| **正確性** | 這段程式碼做了它應該做的事嗎？ | 必須修復 |
| **可讀性** | 未來的維護者能理解嗎？ | FYI / Optional |
| **架構** | 這符合系統的設計模式嗎？ | Optional / 必須修復 |
| **安全性** | 有引入新的攻擊面嗎？ | 必須修復 |
| **效能** | 有明顯的效能退化嗎？ | FYI / 必須修復 |

### 嚴重性標籤

- **Nit** — 風格偏好，可選修復
- **Optional** — 改善但不阻礙合併
- **FYI** — 資訊性，無需行動
- **必須修復** — 阻礙合併

### 跳過警示

- PR 超過 500 行 → 要求拆分（變更大小規範）
- 沒有測試的變更 → 缺乏正確性證明

---

## 7. 部署到生產環境

### 觸發訊號

- 功能開發完成，準備上線
- 需要確認部署清單

### 推薦技能鏈

```
shipping-and-launch
        │  （發布前清單、功能旗標、回滾程序）
        ▼
observability-and-instrumentation
        │  （結構化日誌、RED 指標、追蹤就緒）
        ▼
ci-cd-and-automation
        │  （確認品質關卡流水線通過）
        ▼
documentation-and-adrs
```

### 指令序列

```bash
/ship       # 執行發布前清單確認
```

### 發布前必確認清單

```
□ 所有測試通過（單元、整合、E2E）
□ 效能基準沒有退化
□ 功能旗標就緒（支援快速關閉）
□ 回滾程序已定義並測試
□ 監控警報已設定（錯誤率、延遲、SLO）
□ 資料庫遷移是向後相容的
□ 密鑰/環境變數已在目標環境設定
□ 分段推出策略已確認
```

### 跳過警示

- 沒有功能旗標直接全量上線 → 問題發生無法快速關閉
- 沒有監控就上線 → 問題可能數小時後才被發現

---

## 8. 安全性稽核

### 觸發訊號

- 功能涉及用戶輸入、認證或敏感資料
- 即將上線的功能需要安全審查
- 發現潛在安全漏洞

### 推薦技能鏈

```
security-and-hardening
        │  （OWASP Top 10、三層邊界系統）
        ▼
code-review-and-quality
        │  （安全性軸向重點審查）
        ▼
documentation-and-adrs
        │  （記錄安全決策）
```

### 三層邊界系統

| 層級 | 範圍 | 驗證內容 |
|-----|------|---------|
| **第一層：外部邊界** | 系統入口（API、表單） | 所有用戶輸入、認證憑證 |
| **第二層：服務邊界** | 微服務/模組間呼叫 | 服務身份、權限範圍 |
| **第三層：資料邊界** | 資料庫、外部服務 | 參數化查詢、輸出編碼 |

### OWASP Top 10 快速檢查

```
□ A01 存取控制：每個端點都有授權檢查
□ A02 加密失敗：敏感資料不明文儲存/傳輸
□ A03 注入：使用參數化查詢，不拼接 SQL
□ A04 不安全設計：威脅模型已建立
□ A05 安全設定錯誤：生產環境關閉除錯模式
□ A06 易受攻擊元件：依賴審計通過
□ A07 認證失敗：密碼政策、MFA、會話管理
□ A08 軟體完整性：CI/CD 流水線無未驗證步驟
□ A09 日誌監控不足：安全事件有記錄和告警
□ A10 SSRF：外部 URL 有白名單驗證
```

---

## 9. 效能問題排查

### 觸發訊號

- Core Web Vitals 指標退化
- 用戶回報頁面緩慢
- 效能測試失敗

### 推薦技能鏈

```
performance-optimization
        │  （先測量，後優化）
        ▼
browser-testing-with-devtools
        │  （Chrome DevTools 效能分析）
        ▼
web-performance-auditor（/webperf）
        │  （Core Web Vitals 全面審計）
        ▼
code-review-and-quality
        │  （效能軸向審查）
```

### 指令序列

```bash
/webperf    # 執行 Core Web Vitals 審計
/review     # 效能軸向深度審查
```

### 先測量原則

> 永遠不要在沒有測量資料的情況下優化。

```
1. 建立基準：記錄當前 LCP / FID / CLS / TTFB
2. 找到瓶頸：Flame Graph、Network Waterfall
3. 形成假設：「這個 N+1 查詢導致 TTFB 高」
4. 實施修復：一次改一個變數
5. 驗證改善：對比前後指標
```

### Core Web Vitals 目標值

| 指標 | 良好 | 需改善 | 差 |
|-----|------|-------|---|
| LCP | ≤ 2.5s | 2.5s–4s | > 4s |
| FID / INP | ≤ 100ms | 100ms–300ms | > 300ms |
| CLS | ≤ 0.1 | 0.1–0.25 | > 0.25 |
| TTFB | ≤ 800ms | 800ms–1800ms | > 1800ms |

---

## 10. 棄用舊系統或遷移

### 觸發訊號

- 需要移除舊版 API 或系統
- 將用戶從舊功能遷移到新功能
- 有「殭屍程式碼」（只被棄用的東西呼叫）

### 推薦技能鏈

```
deprecation-and-migration
        │  （程式碼即負債思維、強制性 vs 建議性棄用）
        ▼
planning-and-task-breakdown
        │  （將遷移分解為安全的原子步驟）
        ▼
incremental-implementation
        │  （平行執行期：新舊同時存在）
        ▼
documentation-and-adrs
        │  （記錄棄用決策和遷移路徑）
        ▼
observability-and-instrumentation
```

### 棄用類型決策

| 場景 | 棄用類型 | 說明 |
|-----|---------|------|
| 外部 API，有外部呼叫者 | 強制性棄用 | 必須提供遷移期和文件 |
| 內部模組，只有自己維護 | 建議性棄用 | 可以直接刪除 |
| 有活躍用戶的功能 | 強制性棄用 | 需要功能旗標和公告 |
| 已無人使用的程式碼 | 直接刪除 | 先確認無呼叫者 |

---

## 11. 建立 CI/CD 流水線

### 觸發訊號

- 新專案需要建立自動化流水線
- 現有流水線需要改進品質關卡

### 推薦技能鏈

```
ci-cd-and-automation
        │  （左移、Faster is Safer 原則）
        ▼
planning-and-task-breakdown
        │  （將流水線設定分解為階段）
        ▼
security-and-hardening
        │  （確認流水線中沒有密鑰洩漏）
        ▼
observability-and-instrumentation
        │  （建構失敗的回饋迴圈）
```

### 標準品質關卡流水線

```
Push
  │
  ▼
Lint + Type Check  （< 2 分鐘）
  │
  ▼
Unit Tests  （< 5 分鐘）
  │
  ▼
Integration Tests  （< 10 分鐘）
  │
  ▼
Security Scan  （Dependabot / SAST）
  │
  ▼
Build + Bundle Analysis
  │
  ▼
E2E Tests（主要路徑）
  │
  ▼
Deploy to Staging
  │
  ▼
Performance Test（Core Web Vitals）
  │
  ▼
Promote to Production
```

---

## 12. 需求模糊，要從想法開始

### 觸發訊號

- 用戶說「我想做某件事但不確定怎麼做」
- 需求不明確，有多種解讀方式
- 想探索多種可能方案

### 推薦技能鏈（需求非常模糊）

```
interview-me
        │  （一次一個問題，挖掘真實需求，到 ~95% 信心度）
        ▼
idea-refine
        │  （發散/收斂思考，產生具體提案）
        ▼
spec-driven-development
        │  （將提案轉化為正式 PRD）
        ▼
（繼續完整生命週期）
```

### 推薦技能鏈（有粗略概念）

```
idea-refine
        │
        ▼
spec-driven-development
        │
        ▼
（繼續完整生命週期）
```

### interview-me 模式觸發條件

啟用 `interview-me` 當用戶說了以下任何一句：
- 「訪談我」/ 「interview me」
- 「幫我想想」/ 「不知道要做什麼」
- 「需求還不清楚」
- 「先討論一下」

### 跳過警示

- 在模糊需求下直接跳到 `/build` → 建了錯誤的東西
- 不做 `interview-me` 就寫規格 → 規格基於假設而非真實需求

---

## 組合情境範例

### 情境 A：全新 SaaS 功能（從零到上線）

```
interview-me → idea-refine → /spec → /plan → /build → /test → /webperf → /review → /ship
                                                   ↑
                              (api-and-interface-design 在設計 API 時自動觸發)
                              (security-and-hardening 在處理認證時自動觸發)
```

### 情境 B：緊急生產 Bug 修復

```
debugging-and-error-recovery → /test（Prove-It 模式）→ /review → git push（hotfix 分支）
```

### 情境 C：技術債清理 Sprint

```
code-simplification → /code-simplify → /test（確認行為不變）→ /review → deprecation-and-migration（若有棄用）
```

### 情境 D：安全性強化

```
security-and-hardening → /review（安全軸向）→ documentation-and-adrs（記錄決策）→ /ship
```

---

## 相關文件

- [using-agent-skills/SKILL.md](../skills/using-agent-skills/SKILL.md) — 技能發現與調用的完整規則
- [skill-anatomy.md](skill-anatomy.md) — 如何撰寫新技能
- [README.md](../README.md) — 專案總覽（英文）
- [README.zh-tw.md](../README.zh-tw.md) — 專案總覽（繁體中文）
