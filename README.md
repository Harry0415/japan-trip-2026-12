# 東京・富士・箱根 九日（2026/12/21–12/29）

這是一個單頁的行程網站，整個網站只有 `index.html` 一個檔案，不需要安裝或編譯。

## 放上 GitHub Pages

1. 在 GitHub 建一個新的 repository，例如 `japan-trip-2026`。要用 GitHub Pages 免費方案，repository 必須是 Public。
2. 在 repository 頁面按 **Add file → Upload files**，把 `index.html` 和這份 `README.md` 拖進去，按 **Commit changes**。
3. 到 **Settings → Pages**，Source 選 **Deploy from a branch**，Branch 選 `main`、資料夾選 `/ (root)`，按 **Save**。
4. 等一到兩分鐘，網址會出現在同一頁，格式是 `https://你的帳號.github.io/japan-trip-2026/`。

> 網站是公開的，任何人拿到網址都看得到。不要放護照號碼、訂位代號、電話或信用卡資料。

## 修改行程

用 GitHub 網頁版打開 `index.html`，按右上角的鉛筆圖示就能直接改；改完按 **Commit changes**，一分鐘左右網站就會更新。

所有行程都在檔案上方的 `TRIP` 資料裡，畫面程式在下面，一般不用動。

### 每天的時間表 `plan`

| 想加的內容 | 寫法 |
| --- | --- |
| 一般行程 | `{ t: "14:00–16:00", what: "做什麼", tag: "指定", note: "補充說明", map: "Google 地圖搜尋字" }` |
| 我的建議（會顯示「建議」標籤） | 同上，`tag: "建議"` |
| 留白讓自己填 | `{ t: "12:00–13:00", tag: "待填", hint: "午餐" }` |
| 一般乘車 | `{ ride: { dep: "10:00", from: "淺草", to: "銀座", name: "銀座線", pay: "ic" } }` |
| 要準時搭的班次（車票樣式） | `{ ride: { key: true, dep: "08:30", arr: "10:26", from: "新宿", to: "河口湖", name: "富士回遊 7 號", price: 4200, stamp: "book", check: "提醒文字" } }` |

- 填好「待填」格子：補上 `what`，把 `tag` 改成 `"指定"`。
- `stamp` 是車票上的圓章：`book` 要預約、`pass` 周遊券、`ic` 西瓜卡。
- 每一列結尾要有逗號，最後一列可以不加。

### 交通費 `cost`

`{ leg: "淺草 → 銀座", how: "銀座線", yen: 209, pay: "ic" }`

`pay` 可以是 `ic`（西瓜卡）、`buy`（單買或預約）、`pass`（箱根周遊券）、`free`（免費或已包含）。改了 `yen` 之後，每日小計、富士・箱根三天合計和九天總額都會自動重算。

### 餐廳 `restaurants`

把 `{ blank: true }` 換成：

`{ name: "店名", where: "地點", hours: "營業時間", plan: "排在哪一天", note: "備註", map: "Google 地圖搜尋字" }`

## 改壞了怎麼辦

畫面變成空白，通常是少了逗號或引號。到 repository 的 **History** 可以看到每一次修改，打開上一個版本就能複製回來。
