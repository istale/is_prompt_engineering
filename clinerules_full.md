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

# —— 完整版 .clinerules 結束 ——
