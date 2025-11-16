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
