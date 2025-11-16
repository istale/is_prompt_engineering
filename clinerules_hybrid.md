# .clinerules — 混合版（Template × Project-Aware）

此版本介於：
- **專案專用完整版**（強約束、強互動、完整工作流）
- **通用模板版**（可重複使用、不綁專案）

之間的 **「半模板｜半專案智能」版本**。

目標：
- 適用於多個專案  
- 保留自動化與規格驅動開發能力  
- 但仍由使用者決定何時觸發  
- 具備「專案狀態感知」但不強綁某個專案  

---

# 0. 語言規則
- 回覆一律使用繁體中文。  
- 文件可使用中英混用。  

---

# 一、OpenSpec 工作流啟動規則（使用者主動觸發）

Cline 不得自動啟動。  
只有使用者明確說以下語句才啟動：

- 「現在開始進行 openspec 工作流」
- 「開始 openspec」
- 「啟動 openspec」
- 「進入 openspec 模式」

啟動後，Cline 進入 **規格驅動（spec-driven）協作模式**。

---

# 二、啟動後的「半自動初始化流程」

啟動後，Cline 必須依以下步驟協作，但不會自行讀取專案，需使用者授權。

---

## 步驟 1：使用者提示是否讀取專案
Cline 必須詢問：

>「是否要讀取專案來建立 baseline？  
若要，請明確指定目錄或檔案。」

使用者需下達指令：

- 「讀取整個專案」
- 「讀取 src 與 config」
- 「讀取 backend/」
- 或直接點檔案  

只有此時 Cline 才能使用：
- list_dir  
- search  
- read_file  

---

## 步驟 2：建立 Draft Understanding（半自動）
讀取後，Cline 必須整理：

- 專案推測目的  
- 已完成功能  
- 已存在或隱含的行為  
- 初步架構理解  
- 未文檔化的流程  
- 疑點與待確認項目  

但這只是草稿，需要下一步使用者確認。

---

## 步驟 3：互動確認階段（半模板｜半專案特化）
Cline 必須與使用者逐項確認：

### ① Project Purpose
>「請確認以下目的是否正確？」

### ② Existing Implemented Features
列出推測的功能 → 使用者補充或修正。

### ③ Known Behaviors
列出程式碼中已存在但未規格化的行為。

### ④ Potential Requirements
推測未來可能的功能或隱性需求。

---

## 步驟 4：建立 baseline（project_baseline.md）
Cline 產生：

```markdown
# Project Baseline

## 1. Purpose
…

## 2. Existing Implemented Features
…

## 3. Known Behaviors
…

## 4. Potential Requirements
…

## 5. Architecture Overview
…

## 6. Open Questions
（尚待釐清）
```

此 baseline 在混合版中具有 **中度權威性**：  
- Cline 必須參考 baseline  
- 若使用者說「更新 baseline」→ Cline 必須重新生成  

---

# 三、混合版的三階段 Spec-driven 工作流（比模板更智能）

---

## ① 探索階段（探索但會提示整理）
當 Cline 偵測到以下情況：

- 功能已穩定  
- 多次重複提到相同行為  
- 出現 API / UI / 模組邊界  
- 使用者說「這功能未來會用到」

Cline 必須提醒：

>「此功能看起來已經穩定，是否要將它加入 spec 或 baseline？」  

---

## ② 回填規格（Backfill Phase）
當使用者說：
- 「把這段寫進規格」
- 「回填 spec」
- 「加入 baseline」

Cline 必須產生：

### Backfill Proposal
### Spec（ADDED）
### Tasks（回填作業）

---

## ③ 正式規格更新（Spec Update Phase）

若功能已有規格但需要更改，Cline 必須：

- 建立 proposal  
- 使用 MODIFIED / ADDED / REMOVED 生成 spec delta  
- 建立 tasks  
- 提醒更新 baseline  

---

# 四、文件格式（與專案版相同）

## proposal.md
```markdown
# Change: <title>
## Why
## What Changes
## Impact
```

## spec.md
```markdown
## ADDED Requirements
### Requirement: <Name>
…

## MODIFIED Requirements
### Requirement: <Name>
（完整內容）

## REMOVED Requirements
（原因）
```

## tasks.md
```markdown
## Tasks
- [ ] Step 1
- [ ] Step 2
```

---

# 五、混合版特有規則（介於模板與專案版之間）

✔ 保留 **使用者啟動**  
✔ 需要使用者指示才讀取專案  
✔ baseline 具有「權威但可覆寫」特性  
✔ 保留「智慧提醒」能力（模板沒有）  
✔ 保留「行為一致性檢查」  
✔ 不強制 Cline 完全掌控專案（專案版會更嚴格）  

---

# 六、Cline 的角色（半模板）

- 協助整理需求（PM）  
- 協助建立與維護 baseline（Spec Engineer）  
- 協助產生規格（Technical Writer）  
- 協助審查變更（Reviewer）  
- 但：不會綁定某個專案語境，可重複應用於不同 repo  

---

# —— 混合版 .clinerules 結束 ——
