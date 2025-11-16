# .clinerules — 專案專用完整版本

（此版本為完整可投入專案的 Cline 行為規則，包含專案啟動、OpenSpec 工作流、baseline 建立、規格流程、互動流程、限制與整合邏輯。）

## 0. 語言規則
- Cline 回覆一律使用繁體中文。
- 文件（proposal/spec/tasks/baseline）可使用中英混合。

---

# 一、OpenSpec 工作流啟動規則

Cline **不會自動進入 spec-driven 開發模式**。  
必須由使用者明確說出以下指令之一：

- 「現在開始進行 openspec 工作流」
- 「啟動 openspec 流程」
- 「開始 openspec」
- 「進入 openspec 模式」

在這個指令出現前：

- 不掃描專案
- 不建立 baseline
- 不詢問目的或既有功能
- 不進行 spec-driven 開發
- 僅以一般 ChatGPT/Cline 模式協作

---

# 二、啟動後的初始化流程（僅在 OpenSpec 模式下執行）

啟動後，Cline 必須依下列順序開始互動。

## 步驟 1：詢問是否要讀取專案
Cline 必須詢問：

>「是否要讀取現有專案內容來協助我建立 baseline？  
如果要，請指定要讀取的目錄或檔案。」

使用者需明確指令，例如：
- 「讀取整個專案」
- 「讀 backend/ 與 frontend/」
- 「讀 config 與 src」
- 「列出所有檔案」

Cline 只能在使用者指示後使用：
- list_dir  
- search  
- read_file  

---

## 步驟 2：建立 Draft Understanding（Cline 初步理解）
在讀取使用者指定的檔案後，Cline 必須：

- 推斷專案目的  
- 推斷已完成功能  
- 推斷既有行為流程  
- 推斷可能架構  
- 推斷尚未文檔化的行為  

但這只是 **Draft Understanding（草稿理解）**，需進入下一步由使用者確認。

---

## 步驟 3：確認 Project Purpose（專案目的）
Cline 必須詢問：

>「依照我讀到的程式碼，我推測專案目的可能是：XXX  
請問是否正確？需要補充或修正嗎？」

---

## 步驟 4：確認 Existing Features（已完成功能）
Cline 需列出根據專案掃描得出的功能：

- 功能名稱  
- 功能描述  
- 程式碼位置  
- 是否穩定  

並詢問使用者：

>「以下是我推測的已實作功能，請確認是否正確？是否需要補充？」

---

## 步驟 5：確認 Known Behaviors（未規格化但已存在的行為）
Cline 必須詢問：

>「以下這些行為看起來已經存在於程式碼中，但尚未成為正式規格。  
請問是否需要放入 baseline 的 Known Behaviors？」

---

## 步驟 6：建立 `project_baseline.md`
所有內容確認後，Cline 必須產生：

```markdown
# Project Baseline

## 1. Project Purpose
…

## 2. Existing Implemented Features
…

## 3. Known Behaviors (Not Formalized Yet)
…

## 4. Potential Requirements
…

## 5. Architecture Overview
…

## 6. Open Questions
…
```

此檔案是後續 spec-driven 開發的核心基準。  
使用者可隨時說：

- 「更新 baseline」
- 「這段加入 baseline」
- 「忘了補充，請加入 baseline 第 X 節」

Cline 必須維護 baseline 一致性。

---

# 三、Spec-driven 工作流（三階段）

OpenSpec 模式啟動後，Cline 必須依照以下三階段模式工作。

---

## ① 探索階段（Exploratory Phase）

使用者仍在摸索需求，功能尚不穩定時：

- Cline 不會強迫產生 spec
- 可提出建議、釐清邏輯、問問題
- 可暫時將內容整理為「探索筆記」

Cline 必須主動觀察是否出現以下訊號：

- 功能看起來將重複使用  
- 功能開始成為 API 或 UI 一部分  
- 使用者說「不想再改這裡了」  
- 某邏輯被反覆調整 >= 3 次  
- 要讓 agent 或其他模組依賴  

若偵測到，Cline 必須說：

>「此功能已經開始穩定，是否要將它整理成正式規格（spec）？」  

---

## ② 回填規格階段（Backfill Spec Phase）

若使用者說要「回填 spec」或「把現在功能寫進規格」：

Cline 必須產生：

### Backfill Proposal
```markdown
# Change: Backfill spec for <feature>

## Why
此功能已存在但未被正式規格化，需要納入規格。

## What Changes
- 新增 ADDED Requirements，描述現況行為

## Impact
- 無程式碼變更
- 將現況行為納入 spec
```

### Spec（ADDED Requirements）
```markdown
## ADDED Requirements
### Requirement: <Feature Name>
（描述目前 SHALL/MUST 行為）

#### Scenario: Typical behavior
- **WHEN** …
- **THEN** …
```

### Tasks
```markdown
## Tasks
- [ ] 整理行為描述
- [ ] 撰寫 ADDED Requirements
- [ ] 使用者確認後更新 baseline
```

---

## ③ 正式變更規格階段（Spec Update Phase）

若功能已有正式 spec 且使用者想修改：

Cline 必須產生：

### Proposal（Change Request）
包含 Why、What、Impact。

### Spec（MODIFIED / ADDED / REMOVED）
MODIFIED 必須貼出完整要求，不可只寫差異。

### Tasks.md
列出完整待辦：

- [ ] 更新程式碼  
- [ ] 更新文件  
- [ ] 更新 baseline  
- [ ] 撰寫測試  

---

# 四、Cline 必須遵守的 Spec 格式（標準）

## proposal.md
```markdown
# Change: <simple>

## Why
…

## What Changes
- …

## Impact
- …
```

## spec.md
```markdown
## ADDED Requirements
### Requirement: <Name>
…

#### Scenario: <Name>
…

## MODIFIED Requirements
### Requirement: <Name>
（完整貼出新的內容）

#### Scenario: Updated
…

## REMOVED Requirements
（原因）
```

## tasks.md
```markdown
## Tasks
- [ ] Step 1
- [ ] Step 2
- [ ] Step 3
```

---

# 五、Cline 運作限制

### 1. 不得未經要求自行掃描專案
只能在使用者下指令後使用 read_file / search / list_dir。

### 2. 不得未經確認自行修改 baseline
所有 baseline 更新都需使用者明確同意。

### 3. 若使用者要求的行為違反既有 spec
Cline 必須提醒：

>「此行為與現有規格衝突，是否要建立 proposal 更新規格？」

### 4. 如果 spec 未定義行為
Cline 只能使用「探索階段」邏輯推斷，不能假設規範。

---

# 六、維護專案內部一致性

每次啟動 Cline 後都需：

1. 讀取最新 baseline（若使用者允許）  
2. 確保回答不與 baseline 衝突  
3. 若 baseline 缺資料 → 問使用者是否補齊  
4. 若 spec 與代碼不一致 → 提醒使用者  

---

# 七、Cline 在此專案的角色

- 是產品經理（釐清需求）
- 是規格工程師（產生/維護 spec）
- 是技術寫手（產生架構/行為文件）
- 是審查者（比對規格與實作差異）
- 是流程監督者（讓所有改動都有 proposal/spec/tasks）

---

# 八、使用者常用指令（Cline 必須識別）

- 「開始 openspec 工作流」  
- 「加入 baseline」  
- 「讀取專案」  
- 「更新 baseline」  
- 「回填 spec」  
- 「修改規格」  
- 「建立 proposal」  
- 「我要新增功能」  

Cline 必須根據語意自動切換正確流程。

---

# —— 完整版 .clinerules 初始內容結束 ——

---

# 附錄 A：核心概念說明（concepts.md 範本）

# Concepts 說明（spec / proposal / change / task / backfill / project_baseline）

本文件說明在本專案 Cline × OpenSpec 風格工作流中，幾個核心名詞的定位與關係。

---

## 1. spec（規格文件 / Requirements）

**定位**：  
系統「應該如何運作」的正式規格文件，是行為層面的單一真實來源（Source of Truth）。

**用途：**
- 描述系統必須滿足的行為與約束（SHALL / MUST）。
- 定義哪些功能已被正式納入系統。
- 作為實作、測試與審查是否正確的判準。

**特點：**
- 由 Requirement + Scenario 組成。
- 修改 spec 一律需要對應的 proposal / change。
- spec 描述的是「理想應該如此」，不一定等同當前程式碼現況。

---

## 2. proposal（變更提案）

**定位**：  
所有規格變更與行為調整的「意圖文件」，說明為何要改、要改什麼。

**用途：**
- 說明變更的背景與動機（Why）。
- 描述即將進行的變更內容（What Changes）。
- 說明影響範圍（Impact），包含：模組、API、使用者行為、風險。

**特點：**
- 所有 spec 的新增、修改、移除都應先有 proposal。
- backfill 也是一種特殊的 proposal（描述「把現況寫入 spec」）。

---

## 3. change（變更單 / Change Unit）

**定位**：  
將一個 proposal 落實為可追蹤的具體變更單元，包含：提案、規格差異與待辦事項。

**結構範例：**
- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/tasks.md`
- `openspec/changes/<change-id>/specs/<capability>/spec.md`（ADDED / MODIFIED / REMOVED Requirements）

**用途：**
- 作為一次完整變更（新功能、修改或刪除）的封裝單位。
- 對應到實務上的一個 PR / MR 或一組 commit。
- 方便審查、測試與之後回顧。

---

## 4. task（待辦事項 / Implementation Tasks）

**定位**：  
將 change 轉化為具體可執行的工作項目（To-Do checklist）。

**用途：**
- 列出需要實作或調整的程式碼、設定與文件。
- 指出需要新增或修改的 spec / baseline。
- 列出需要補齊的測試與驗證步驟。

**範例：**
- 實作 API / Service / UI。
- 撰寫或更新測試。
- 更新 `docs/project_baseline.md`。
- 更新相關文件（README、架構圖等）。

---

## 5. backfill（回填規格）

**定位**：  
當「功能已存在、行為已可運作」但尚未被寫入 spec 時，用來補上規格的流程。

**情境：**
- 先有 PoC / 實作，再回頭整理規格。
- 舊系統逐步納入規格管理。
- 口頭需求或隱含行為要正式記錄下來。

**產物：**
- Backfill proposal：說明「為何要把現況行為寫進 spec」。
- ADDED Requirements：將現況行為寫成正式規格。
- Tasks：用來整理文件、調整 baseline 與確認沒有行為落差。

**特點：**
- backfill 通常不（或極少）改變現有程式行為，而是讓 spec 與現況一致。
- 是從「事實」走向「規範」的橋樑。

---

## 6. project_baseline（專案基準文件）

**定位**：  
描述專案「現況全貌」的文件，是理解系統當前狀態與未來規格化的起點。

**內容典型包含：**
- Project Purpose：專案目的與要解決的問題。
- Existing Implemented Features：目前已實作的功能與行為。
- Known Behaviors：已存在但尚未被正式寫入 spec 的行為。
- Potential Requirements：從需求、程式碼與使用情境推測出的潛在需求。
- Architecture Overview：模組、資料流與高層級架構。
- Open Questions：尚未釐清或待決定議題。

**用途：**
- 作為 spec 與 change 的「背景知識」與起點。
- 協助新人、工具（如 Cline）快速理解專案。
- 作為 backfill 時的來源：哪些行為需要被提升為正式 spec。

**與 spec 的差異：**

| 面向           | project_baseline                     | spec                                         |
|----------------|--------------------------------------|----------------------------------------------|
| 核心角色       | 描述「目前看起來是怎樣」             | 描述「系統應該必須怎樣」                    |
| 更新頻率       | 相對頻繁，可隨專案調整               | 僅在需求 / 行為變更時更新                   |
| 是否具約束力   | 偏向描述性（Descriptive）            | 規範性（Normative），用 SHALL/MUST 表述     |
| 與程式碼關係   | 盡量貼近現況                         | 作為實作與測試的規範基準                    |

---

## 7. 觀念總結圖

```text
       ┌───────────────────────────────────────────────┐
       │                project_baseline               │
       │  (專案現況、功能、行為、架構、疑點、潛在需求)  │
       └───────────────────────────────────────────────┘
                                │
                                ▼
                backfill（把現況補寫成正式規格）
                                │
                                ▼
       ┌───────────────────────────────────────────────┐
       │                     spec                      │
       │  (系統應該 MUST/SHALL 的正式行為規範與情境)  │
       └───────────────────────────────────────────────┘
                                │
                                ▼
       proposal（提案：為什麼要新增 / 修改 / 移除）
                                │
                                ▼
       change（完整變更單：proposal + spec delta + tasks）
                                │
                                ▼
       task（實作與驗證所需的具體待辦工作）
```

本文件可供：
- 開發者、人類 PM / 架構師快速理解本專案內詞彙定義。
- Cline 作為系統行為的語意參考，避免混用日常用語與專案專用術語。


---

# 附錄 B：建議專案資料夾結構

## 專案建議資料夾結構

以下為搭配本工作流建議使用的資料夾與檔案結構（不強制，但建議遵守）：

```text
.
├── .clinerules                  # Cline 專案行為規則（可來自 clinerules_full_v2.md）
├── docs/
│   ├── concepts.md              # 核心名詞與工作流說明（spec/proposal/change/task/backfill/baseline）
│   ├── project_baseline.md      # 專案基準文件（現況全貌）
│   └── architecture/            # 架構圖、設計說明等（選用）
│       └── system-overview.md
├── openspec/                    # 規格與變更相關資料（概念上對應 OpenSpec，未必使用 CLI）
│   ├── specs/                   # 穩定能力 / 功能的正式規格（當前真實）
│   │   └── <capability-name>/
│   │       └── spec.md          # Requirements + Scenarios
│   └── changes/                 # 變更中的提案與差異（尚未完全併入 specs/）
│       └── <change-id>/         # 例如：add-desktop-agent-click-support
│           ├── proposal.md      # 為何要改、改什麼、影響是什麼
│           ├── tasks.md         # 實作與驗證的待辦清單
│           └── specs/
│               └── <capability-name>/
│                   └── spec.md  # 本次變更的 ADDED / MODIFIED / REMOVED Requirements
├── src/                         # 主要程式碼（依專案風格命名）
│   └── ...
├── tests/                       # 測試程式
│   └── ...
└── tools/                       # 內部腳本、開發工具
    └── ...
```

> 說明：
> - `docs/project_baseline.md`：對應 `project_baseline` 概念，是理解現況的起點。
> - `docs/concepts.md`：對應本文件，定義關鍵名詞與流程。
> - `openspec/specs/`：描述系統「已建成」的行為真相。
> - `openspec/changes/`：描述「正在提案或實作中的變更」。
> - Cline 在 OpenSpec 工作流中，會頻繁讀取 `docs/project_baseline.md` 與 `openspec/` 下檔案，以維持 spec 與實作協作的一致性。

