<!-- name: cline_refactor_main_to_thin_orchestrator_dict_style -->
你現在要協助我「重構 main / 入口函式」，目標是讓：

1. main（或 API handler 等入口）變得很薄，只負責流程編排（orchestrator）
2. 商業邏輯拆成具語意的 `handle_xxx()`、`dispatch_xxx()` 等函式
3. 資料結構雖然是 `list` / `dict` / `list[dict]` / JSON，但欄位語意要整理清楚（有「欄位 schema」的感覺），而不是到處亂塞 key

---

## 🎯 任務背景

我目前有一個「主流程函式」，例如：

- `main()`（CLI 入口）
- 某個 API handler（例如 `run_command()`）
- 或其他類似的入口函式

這些入口函式裡，常常混在一起：

- 流程控制（先做 A、再做 B…）
- 商業邏輯（判斷、計算、業務規則）
- 資料結構組裝 / 拆解（dict / list / JSON 處理）

你要幫我把它們重構成：

- 薄的 orchestrator 函式
- 一組 `handle_xxx()` / `dispatch_xxx()` 等子函式
- 清楚定義的「dict / JSON 欄位 schema 說明」

---

## 🧩 Step 0：理解現狀（請先做）

當我選取並貼上一段 main 或 handler 程式碼時，請先：

1. 用自然語言說明：
   - 這個 main / handler 的角色與責任
   - 它大致包含哪些類型的動作（例如：讀輸入、解析、呼叫外部服務、寫 log、回傳結果）

2. 幫我「粗略分段」目前這段程式碼的邏輯區塊，例如：
   - 解析 CLI / HTTP 請求
   - 根據輸入建立一個 dict / JSON 結構
   - 決定走哪條路（if/elif 大 switch）
   - 實際執行動作（呼叫其他函式或模組）
   - 收集結果並輸出

先不要改 code，只要讓我跟你都對現狀有共同理解。

---

## 🧩 Step 1：設計重構後的函式結構（先設計，不直接改碼）

接著請你幫我設計「理想中的薄 main / handler」長相，用文字＋函式名稱表示即可，例如：

```python
# name: refactor_main_example_orchestrator
def main():
    raw_input = load_input_from_cli()
    request_dict = parse_and_normalize_input(raw_input)
    result_dict = dispatch_request(request_dict)
    render_output(result_dict)
```

請幫我完成：

1. 建議一個新的 orchestrator 骨架（main 或 handler），用 pseudo-code 形式寫出「呼叫哪些子函式」。
2. 列出你建議拆出的子函式清單，例如：
   - `load_input_from_cli()`
   - `parse_request(raw_input: str) -> dict`
   - `dispatch_request(request: dict) -> dict`
   - `handle_open_app(request: dict) -> dict`
   - `handle_run_macro(request: dict) -> dict`
   - `render_output(result: dict)`

這一階段只需要設計圖，不直接動現有程式碼。請把設計圖貼出來給我確認。

---

## 🧩 Step 2：整理資料結構（以 dict / JSON 欄位為主）

我不打算在程式裡大量使用 class / dataclass，  
但仍然需要讓資料結構有明確欄位定義、語意清楚。

你要做的是：

1. 從這段程式碼中，找出幾個「核心 dict / JSON 結構」，例如：
   - command / intent / request / result / context 等
2. 幫我用文字整理每個結構的欄位 schema，例如：

   ```text
   Request(dict)：
     - type: str        # 指令類型，例如 "open_app" / "search"
     - raw_text: str    # 原始使用者輸入
     - target_app: str? # 要打開的應用程式（可選）
     - extra: dict      # 其他參數（視情況）
   ```

3. 說明這些 dict 在流程中的流向：
   - 從哪裡建立（哪個函式）
   - 傳給哪些函式使用
   - 大致包含哪些欄位（欄位名與用途）

如果你覺得有必要，也可以建議「把某些 dict 的欄位改名更有語意」，但先以描述現況為主。

重要：  
這一步只需要「欄位設計與文檔」，不需要真的改動原始程式碼裡的 dict 結構。先讓我看清楚資料長什麼樣。

---

## 🧩 Step 3：提出實際重構方案（分階段修改）

在我同意 Step 1＋Step 2 的設計後，再開始改 code。請依這個順序：

1. **第一階段：函式切割**
   - 從 main / handler 中，把明顯的邏輯區塊剪出來變成子函式（例如 `parse_request_dict()`、`dispatch_request()`、`handle_xxx()`）
   - main / handler 只保留「呼叫子函式」的流程，不在裡面寫細節
   - 行為盡量保持不變（功能等價）

2. **第二階段：dict / JSON 結構清理**
   - 在不破壞功能的前提下，調整：
     - 不必要的重複欄位
     - 不一致的 key 命名
   - 優先從「接近邊界」的地方開始（例如一開始解析輸入時就做 normalization）

3. **第三階段：錯誤處理與命名優化**
   - 將錯誤處理整理成統一風格（例如在某一層集中處理）
   - 將子函式命名改成「商業語意導向」

在每一階段，請：

- 列出將修改的函式與大致變更內容
- 再給出建議的程式碼（可以用 patch 或完整函式形式）

---

## 🧩 Step 4：同步更新 PROJECT_NAVIGATOR.md（可選但強烈建議）

當這個 main / handler 重構完之後，請你協助：

1. 依照重構後的流程，為它建立或更新一條 Flow（Input / Execution / Output）描述。
2. 為該 Flow 補上一段 `Logic Flow（pseudo-code）`，例如：

   ```md
   #### Logic Flow（pseudo-code）

   - Step 1：從 CLI 讀取輸入字串
   - Step 2：呼叫 parse_request_dict() 解析成標準化 dict
   - Step 3：呼叫 dispatch_request() 決定處理路徑
   - Step 4：針對不同 type 呼叫對應 handle_xxx()
   - Step 5：整理結果 dict，呼叫 render_output() 輸出
   ```

3. 先把建議寫入內容貼給我看，等我同意後再實際修改 `PROJECT_NAVIGATOR.md`。

---

## 🧩 回應風格總結

- 任何時候都要先「描述設計」再「提改動程式碼」：
  - 先講新的 main/handler 長什麼樣
  - 再列出子函式與責任
  - 再整理 dict / JSON 的欄位 schema
- 你的描述要讓一個幾週沒碰這個專案的「未來的我」，看完就知道：
  - main 的流程調整成什麼樣
  - 哪些 handle_xxx / dispatch_xxx 負責哪段邏輯
  - 主要 dict / JSON 的欄位大致長什麼樣、被誰傳來傳去

---

當我說：「請依 refactor main（dict 版）的規則幫我處理這段程式碼」，  
請從 **Step 0** 開始完整執行流程。
