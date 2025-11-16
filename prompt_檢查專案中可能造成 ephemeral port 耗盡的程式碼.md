# 提示詞：檢查專案中可能造成 ephemeral port 耗盡的程式碼

你現在是一位資深系統工程師 + 資深後端/前端開發者，幫我檢查整個專案中，哪些程式碼可能會導致在 macOS 上出現這類錯誤：

- Python: OSError / ConnectionError: [Errno 49] Can't assign requested address
- Browser: ERR_ADDRESS_INVALID / ERR_ADDRESS_UNREACHABLE / 連不到 localhost backend
- 本機 frontend 連不到 backend，但 backend 沒有任何 request log
- netstat 顯示大量 TIME_WAIT (例如 2000~30000 條)

請你掃描整個 repo（包含 backend、frontend、各種 script），列出「所有有風險」的地方，並且對每個案例做詳細分析與修改建議。

特別要幫我注意下列幾種情況：

1. **Python / backend / 爬蟲相關**
   - 任何直接使用 `requests.get` / `requests.post` / `requests.request` 的地方。
   - 尤其是：在 for 迴圈、while 迴圈、thread pool、async 任務裡大量呼叫 requests，但沒有共用 `requests.Session`。
   - 任何使用 `aiohttp` 或 `httpx` 的地方，如果：
     - 沒有共用 Client / Session
     - 沒有限制 `limit`（最大連線數）
     - 沒有適當的 timeout
   - 任何「下載大量圖片/漫畫/檔案」的程式（例如爬蟲），如果：
     - 每張圖片都開一個新的 TCP 連線
     - `asyncio.gather` 或 `ThreadPoolExecutor` 同時開非常多請求
     - 沒有 sleep / rate limit。
   - 列出所有「在迴圈中做 HTTP 請求」的地方，標記為高風險。

2. **Node / frontend / React / Vite / Next.js 等**
   - 檢查 Vite / webpack / dev server 的 proxy 設定：
     - 如果有把 `/` 或非常廣的路徑 proxy 到 backend（例如 proxy: { "/": "http://localhost:8000" }），請判斷是否會造成「前端 HTML 也被 proxy 回 backend，導致循環請求」。
     - 檢查 `/api` proxy 是否會對靜態資源或頁面本身造成 loop。
   - 在 React component 中：
     - 使用 `useEffect` 發 HTTP 請求但 **依賴陣列寫錯**（例如沒寫 `[]`，變成每次 render 都打 API）。
     - 使用 `setInterval` / `setTimeout` 重複打 API 但沒有適當清除。
     - 使用 `Promise.all` 對巨大陣列做 fetch / axios 請求。
   - 任何輪詢(polling)邏輯，如果：
     - 間隔太短（例如每 100~500 ms 就打一次）
     - 沒有在 component unmount 時停止
     - 或有多層嵌套輪詢。

3. **其他可能造成大量連線的情況**
   - 使用 WebSocket / SSE 但：
     - 每次操作都重新建立連線而不是重用
     - 沒有正確關閉。
   - 任何會在本機大量建立「短生命週期 TCP 連線」的地方。

請你輸出時，用下面格式整理結果：

1. 先給出一個「總結」，說明你找到幾個高風險點、分別在 backend / frontend / scripts。
2. 然後逐一列出：
   - 檔案路徑
   - 可能有問題的程式碼區塊（節錄重點就好）
   - 這段程式碼為什麼可能造成 ephemeral ports 耗盡 / TIME_WAIT 暴增
   - 具體修改建議（例如：
     - 改用 `requests.Session`，示範如何共用 session
     - 限制並發數量（例：最多同時 5~10 個請求）
     - 在 React 中修正 `useEffect` 的依賴陣列
     - 修正 Vite proxy 設定避免 proxy `/`
     - 增加 timeout、retry、sleep、backoff 等）

最後，請在答案中直接給出「建議後的範例程式碼片段」，方便我複製修改。