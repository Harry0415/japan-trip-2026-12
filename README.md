# 東京・富士・箱根 九日（2026/12/21–12/29）

網站：https://harry0415.github.io/japan-trip-2026-12/

| 檔案 | 用途 |
| --- | --- |
| `index.html` | 整個網站，行程資料也在裡面 |
| `firestore.rules` | 「大家加的行程與餐廳」的資料庫權限規則 |
| `README.md` | 這份說明 |

## 網站內容

- 頂端的索引列切換區塊，一次只顯示一個：每日行程、想吃的餐廳、大家加的、住宿、出發前準備、花費與評估（九天交通費、交通與票券、資料來源）。網址會跟著變，例如 `#cost`、`#d4`，可以直接分享，也能用返回鍵回上一個區塊。
- 每日行程是可以左右捲動的卡片，一天一張，停下來時會固定在畫面正中間。上方的路線會標出目前看的日期，小列車跟著移動；旅行期間打開會直接停在今天。
- 每張卡片依序是：今日重點、時間表、當天交通、大家加的。
- 「大家加的行程與餐廳」讓旅伴用 Google 帳號登入後新增；每一筆只有新增的人能修改或刪除，管理員可以刪除不當內容。

## 開啟旅伴新增功能（Firebase，大約 15 分鐘）

GitHub Pages 只能放靜態網頁，所以旅伴新增的內容存在 Google 的 Firebase。免費的 Spark 方案就夠用。

1. **建立專案**：到 [Firebase 主控台](https://console.firebase.google.com/) 建立專案，名稱例如 `japan-trip-2026`。Google Analytics 可以關閉。
2. **新增網頁應用程式**：在專案總覽按網頁圖示 `</>`，取個暱稱後註冊，不用勾 Firebase Hosting。畫面會出現 `firebaseConfig`，把其中的 `apiKey`、`authDomain`、`projectId`、`appId` 四個值貼到 `index.html` 的這一行：

   ```js
   firebase: { apiKey: "", authDomain: "", projectId: "", appId: "" },
   ```

3. **開啟 Google 登入**：Authentication（驗證）→ 開始使用 → Sign-in method（登入方式）→ Google → 啟用，填支援電子郵件後儲存。
4. **加入授權網域**：Authentication → Settings（設定）→ 授權網域 → 新增 `harry0415.github.io`。少了這一步，登入時會出現「這個網址還沒加進 Firebase 的授權網域」。
5. **建立資料庫**：Firestore Database → 建立資料庫，位置選 `asia-east1`（台灣）或 `asia-northeast1`（東京），選正式版模式（production mode）。
6. **貼上規則**：Firestore Database → 規則，把這個 repo 的 `firestore.rules` 整個貼上，按「發布」。
7. **設定管理員**：在網站登入一次後，到 Firebase 主控台 Authentication → Users 找到自己的帳號，複製「使用者 UID」，填到兩個地方：
   - `index.html`：`admins: ["使用者 UID"],`
   - `firestore.rules`：`return ['使用者 UID'];`，再到 Firebase 主控台重新發布規則。

規則的內容：

- 任何打開網頁的人都看得到大家加的內容。
- 登入 Google 才能新增，而且只能用自己的帳號新增。
- 只有新增的人能修改或刪除自己的那一筆；管理員只能刪除，不能改別人的內容。
- 每個欄位都有長度限制，也不能多塞其他欄位。

> `apiKey` 這些設定值本來就會出現在網頁裡，公開沒關係，誰能讀寫是由 `firestore.rules` 決定。
>
> 大家新增的內容是公開的，請提醒旅伴不要寫電話、訂位代號這類資料。
>
> 在 LINE、Facebook 這類 App 內建的瀏覽器裡不能用 Google 登入，要改用 Safari 或 Chrome。網頁會顯示提示；在 LINE 裡還會多一個「用預設瀏覽器開啟」的連結。

## 修改行程

用 GitHub 網頁版打開 `index.html`，按右上角的鉛筆圖示就能改；改完按 **Commit changes**，一兩分鐘後網站會更新。所有行程都在檔案上方的 `TRIP` 資料裡，畫面程式在下面，一般不用動。

### 每天的時間表 `plan`

| 想加的內容 | 寫法 |
| --- | --- |
| 一般行程 | `{ t: "14:00–16:00", what: "做什麼", tag: "指定", note: "補充說明", map: "Google 地圖搜尋字" }` |
| 建議行程（會顯示「建議」標籤） | 同上，`tag: "建議"` |
| 留白讓自己填 | `{ t: "12:00–13:00", tag: "待填", hint: "午餐" }` |
| 一般乘車 | `{ ride: { dep: "10:00", from: "淺草", to: "銀座", name: "銀座線", pay: "ic" } }` |
| 要準時搭的班次（車票樣式） | `{ ride: { key: true, dep: "08:30", arr: "10:26", from: "新宿", to: "河口湖", name: "富士回遊 7 號", price: 4200, stamp: "book", check: "提醒文字" } }` |

- 填好「待填」格子：補上 `what`，把 `tag` 改成 `"指定"`。
- `key: true` 的班次會用車票樣式顯示，也會自動列進當天的「今日重點」。
- `stamp` 是車票上的圓章：`book` 要預約、`pass` 周遊券、`ic` 西瓜卡。
- 每一列結尾要有逗號，最後一列可以不加。

### 今日重點 `points`

每天的 `points` 是提醒清單，一句一行：

```js
points: [
  "西瓜卡餘額留 ¥3,000 以上。",
  "飯店接送 15:00–20:00，只接不送。"
],
```

### 交通費 `cost`

`{ leg: "淺草 → 銀座", how: "銀座線", yen: 209, pay: "ic" }`

`pay` 可以是 `ic`（西瓜卡）、`buy`（單買或預約）、`pass`（箱根周遊券）、`free`（免費或已包含）。改了 `yen` 之後，每日小計、富士・箱根三天合計和九天總額都會自動重算。

### 想吃的餐廳 `restaurants`

把 `{ blank: true }` 換成：

`{ name: "店名", where: "地點", hours: "營業時間", plan: "排在哪一天", note: "備註", map: "Google 地圖搜尋字" }`

## 改壞了怎麼辦

畫面變成空白，通常是少了逗號或引號。到 repo 的 **History** 可以看到每一次修改，打開上一個版本就能把內容複製回來。
