<!-- name: cline_logic_flow_from_navigator_flows -->
你現在要幫我做一件「文件補強」工作：

目標：
從目前 `PROJECT_NAVIGATOR.md` 裡已經存在的所有 Flow 描述（Input / Execution / Output），
為每個 Flow 補上一段「Pseudo-code Logic Flow」，讓未來的我可以快速掌握實作邏輯走向。

---

## 任務說明

1. 打開專案根目錄的 `PROJECT_NAVIGATOR.md`。
2. 尋找裡面所有代表「流程」的小節，例如標題：
   - `### Flow 1：...`
   - `### Flow 2：...`
3. 對於每一個 Flow 區塊，在不修改原內容的前提下，新增一段：

#### Logic Flow（pseudo-code）
- Step 1：...
- Step 2：...
- Step 3：...

---

## Logic Flow 產生規則（Pseudo-code）

1. **一層抽象原則**  
   每個 Step 必須是高階動作，例如：解析 → 驗證 → 分派 → 執行 → 整理結果。

2. **允許引用函式名稱，但不可貼程式碼**  
   例如：  
   - Step 2：呼叫 `parse_intent()` 產生 Intent 物件。

3. **可以參考程式碼內容，但產出要保持在 pseudo-code 層級。**

4. **若 Flow 描述過於模糊，無法產生合理 pseudo-code**  
   請在該 Flow 底下注記：  
   `> TODO: 此 Flow 的程式邏輯需要從程式碼補強後才能建立 Logic Flow。`

---

## 修改流程

1. 依序為所有 Flow 產生 pseudo-code。
2. 將「更新後的 Flow 區塊（含 Logic Flow）」全部貼出來給我確認。
3. 等我回覆「OK，可以寫檔」後，再實際修改 `PROJECT_NAVIGATOR.md`。

---

## 回應風格

- 請完整貼出更新後每個 Flow 的 Markdown（原 Flow + 新增 Logic Flow）。
- 不要只貼 diff。
- 若不確定某部分資訊，請以「推測」標註。
