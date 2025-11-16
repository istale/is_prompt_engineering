# .clinerules — 通用模板版（適用於多個專案）

此版本為可套用到任何專案的「OpenSpec 工作流規格驅動」通用模板。
不包含任何專案特有邏輯，只包含框架規則與標準流程。

---

# 0. 語言規則
- 回覆一律使用繁體中文。
- 文件可中英混用。

---

# 一、OpenSpec 工作流啟動規則（必須由使用者明確觸發）

Cline 不得自動啟動規格驅動流程。  
必須由使用者明確說出下列語句之一才啟動：

- 「現在開始進行 openspec 工作流」
- 「開始 openspec」
- 「啟動 openspec」
- 「進入 openspec 模式」

啟動前：
- 不讀取專案
- 不建立 baseline
- 不進入 spec-driven
- 僅以一般助手模式回應

---

# 二、OpenSpec 啟動後的初始化流程（通用）

啟動後，Cline 必須依序進行以下互動：

---

## 步驟 1：詢問是否要讀取專案
Cline 必須詢問使用者：

>「要不要讀取專案目錄／程式碼來建立 baseline？若要請指定目錄。」

僅能在使用者指令後使用：
- list_dir  
- search  
- read_file  

---

## 步驟 2：建立 Draft Understanding（草稿理解）
當使用者指定讀取專案後，Cline 必須整理：

- 推測專案目的（purpose）
- 已完成功能（implemented features）
- 已存在行為（current behaviors）
- 架構概念（architecture overview）
- 未文檔化的行為（unknown or hidden behaviors）

此為草稿，必須交由使用者確認。

---

## 步驟 3：確認 Purpose
Cline 必須詢問：

>「以下是我推測的專案目的，請確認是否正確？」

---

## 步驟 4：確認 Existing Implemented Features
Cline 必須列出：

- 功能名稱  
- 描述  
- 程式碼位置  

並詢問使用者確認與補充。

---

## 步驟 5：確認 Known Behaviors
Cline 必須詢問：

>「以下這些行為已存在但未成為規格，是否要加入 baseline？」

---

## 步驟 6：建立 baseline 檔案（project_baseline.md）
Cline 必須建立下列結構：

```markdown
# Project Baseline

## 1. Purpose
（使用者確認）

## 2. Existing Implemented Features
（使用者確認）

## 3. Known Behaviors
（未規格化但存在的部分）

## 4. Potential Requirements
（推測）

## 5. Architecture Overview
（由程式碼推斷）

## 6. Open Questions
```

此 baseline 必須可持續更新。

---

# 三、Spec-driven 工作流（模板通用）

在 OpenSpec 模式中，Cline 必須遵循三階段：

---

## ① 探索階段（Exploratory Phase）

- 使用者仍在摸索  
- 不立即產生 spec  
- 先釐清需求  

當偵測到需求變得穩定時，提醒：

>「此功能似乎已穩定，是否要將其加入 spec？」

---

## ② 回填規格階段（Backfill Phase）

當功能已存在但未規格化：

### Backfill Proposal
```markdown
# Change: Backfill for <feature>

## Why
（功能已存在但缺乏規格）

## What Changes
（加入 ADDED Requirements）

## Impact
（僅文件與規格變動）
```

### Spec（ADDED Requirements）
```markdown
## ADDED Requirements
### Requirement: <Name>

#### Scenario: Default
- **WHEN** …
- **THEN** …
```

---

## ③ 正式規格更新（Spec Update）

當功能已有規格並需修改：

### Proposal
- Why  
- What Changes  
- Impact  

### Spec 更新  
使用：
- ADDED  
- MODIFIED（需貼完整要求）  
- REMOVED  

### Tasks.md
```markdown
## Tasks
- [ ] 程式碼更新
- [ ] 測試更新
- [ ] baseline 更新
```

---

# 四、標準文件格式（模板）

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
- [ ] 1
- [ ] 2
```

---

# 五、限制（模板通用）

- 不得未經使用者批准擅自讀取專案  
- 不得未經使用者批准修改 baseline  
- 若使用者要求的行為與 baseline 不符，需提醒建立 proposal  
- 若行為未定義於 spec，需進入探索階段詢問使用者  

---

# 六、Cline 在此模板中的角色

- 協助釐清需求  
- 協助建立 baseline  
- 協助建立 spec  
- 協助維護變更一致性  
- 協助審查 spec 與程式碼是否一致  

---

# —— 通用模板 .clinerules 內容結束 ——
