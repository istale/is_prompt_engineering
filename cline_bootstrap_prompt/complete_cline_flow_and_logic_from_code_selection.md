<!-- name: cline_flow_and_logic_from_code_selection -->
你現在要協助我，從「選取的一段程式碼」整理出：

1. 一條新的 Flow 描述（Input / Execution / Output）
2. 對應的 pseudo-code 級「Logic Flow」
3. 並將這些內容整合進 `PROJECT_NAVIGATOR.md`

---

# 🎯 任務目的

透過選取的程式碼片段，自動推導出：

- 這段程式碼屬於哪一個「執行路徑」（Flow）
- 它的 Input / Execution / Output 是什麼
- 它的高階邏輯（Pseudo‑code Logic Flow）
- 如何將這些資訊加入 `PROJECT_NAVIGATOR.md`

這能讓未來的我在閱讀專案時，不需要看完整程式碼就能理解邏輯與資料流。

---

# 🧩 Step 1：從程式碼整理出 Flow 描述

當我選取並貼上某段程式碼時，請先完成：

## 1.1 說明這段程式碼的用途

以自然語言說明：

- 這段程式碼負責的角色與功能
- 若可推理，說明它的上游來源（CLI / API / Batch / 其他）

## 1.2 產生 Flow 描述（Markdown，不要表格）

請用以下格式產出一條 Flow：

```md
### Flow X：<給這條流程一個明確名稱>

**Input：**  
- 來源（CLI / API / Internal call / Batch / 等）  
- 輸入資料格式（字串、JSON、dataclass、物件…）

**Execution：**  
- 用自然語言描述主要步驟  
- 可以提及函式名稱、模組名稱（不可貼原始程式碼）
- 每步驟是一個明確的動作（解析 → 驗證 → 分派 → 執行 → 收集結果）

**Output：**  
- 輸出資料型態（文字 / JSON / 檔案 / 狀態…）  
- 若會寫入目錄（logs/、output/ 等），請標註  
- 若不確定請寫「推測」
```

Flow 名稱要具體，例如：

- 「CLI 遠端桌面操作流程」  
- 「API：執行單一使用者指令並回傳結果」  

---

# 🧩 Step 2：產生 Logic Flow（Pseudo‑code）

依照剛剛建立的 Flow，請加上一段高階邏輯如下：

```md
#### Logic Flow（pseudo‑code）

- Step 1：...
- Step 2：...
- Step 3：...
```

規則：

1. **一層抽象原則**  
   - 每個 Step 是一個高階概念（解析 → 分派 → 執行 → 整理結果）
   - 不可寫 if/else 細節、不可寫參數細節

2. **可引用函式名，但不可貼程式碼**  
   - e.g., `parse_intent()`, `dispatch_flow()`, `handle_open_app()`

3. **保持自然語言 + 函式語意兼具**

4. 若程式碼過度片段，無法推導完整流程  
   - 請加註：  
     `> TODO: 此程式碼片段不足以推導完整 Logic Flow，需要補充上下文。`

---

# 🧩 Step 3：整合進 PROJECT_NAVIGATOR.md

完成 Flow + Logic Flow 後：

1. 開啟 `PROJECT_NAVIGATOR.md`
2. 找到「Input → Execution → Output（主要流程）」的區域
3. 插入新的 Flow（依現有 Flow 編號往後新增）
4. Flow 名稱需與你生成的相同

---

# 🧩 Step 4：寫檔前確認

在修改前：

1. 請先把「預計寫進 Navigator 的完整段落」貼給我  
   （Flow + Logic Flow，兩者合併為完整 Markdown 區塊）

2. 等我說「OK，可以寫入」後  
   - 才能真的修改 `PROJECT_NAVIGATOR.md`

3. 寫檔後  
   - 再貼一次更新後的該 Flow，讓我確認

---

# 🧩 回應風格規範

- Flow 與 Logic Flow 必須完整貼出（不要貼 diff）
- Flow 與 Logic Flow 都只能用 Markdown 文字描述，不可貼程式碼
- 若資訊不足，請明確寫出「推測」並說明依據
- 若判斷選取程式碼是工具函式、不是 Flow 主體，請先提醒我，不要強行寫入 Navigator

---

# 🧩 當我選取一小段 code 並對你說「請依提示建立 Flow」時

請從 **Step 1** 開始執行整套流程。
