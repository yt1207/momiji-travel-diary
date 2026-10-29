# もみじ旅日記｜2026 日本秋日旅行互動網站

京都・大阪・伊根｜四天三夜（2026/11/26–11/29）。純靜態網站（一個 `index.html` ＋圖片），不需要安裝任何東西、沒有後端、沒有金鑰。

- 目前版本：V2.2.1（由 Claude Artifact 原樣移植，只把圖片網址改成相對路徑 `assets/…`）
- 預計網址：`https://yt1207.github.io/momiji-travel-diary/`（要等 GitHub 部署成功後才算數）

## 資料夾說明

| 檔案 | 用途 |
|---|---|
| `index.html` | 整個網站（程式與文字都在這裡） |
| `assets/img/*.webp` | 網站實際使用的六張正式插畫（尺寸與原圖相同，只有壓縮格式不同） |
| `assets/img/original/*.png` | 原始高畫質插畫（備份與日後替換用，網站點圖可開啟） |
| `.github/workflows/deploy.yml` | 自動部署設定（推送到 main 就自動更新網站） |
| `docs/遷移盤點.md` | 從 Claude Artifact 移植時的相依性盤點與測試結果 |

## 第一次部署（不需要打指令）

> 提醒：GitHub Pages 在「免費帳號」只支援 **公開（Public）** 儲存庫；私人（Private）儲存庫要付費方案（Pro／Team）才能用 Pages。請依你的方案決定。公開儲存庫代表任何人都能看到程式碼與行程文字，但**看不到**任何人的個人筆記（見下方「資料保存」）。

1. 登入 GitHub（帳號 `yt1207`）→ 右上角 **+** → **New repository**。
2. Repository name 填 `momiji-travel-diary`，選 Public（或你有付費方案時選 Private），**不要**勾選 README，按 **Create repository**。
3. 在新頁面點 **uploading an existing file**，把解壓縮後資料夾內的**所有內容**拖進去，按 **Commit changes**。
   - 要連 `.github` 資料夾一起上傳。Mac 看不到隱藏資料夾時，按 `Cmd + Shift + .`；Windows 需開啟「顯示隱藏的項目」。
   - 如果 `.github` 傳不上去：在儲存庫按 **Add file → Create new file**，檔名輸入 `.github/workflows/deploy.yml`，把本專案同名檔案的內容貼進去再 Commit。
   - 檔案較大（約 20 MB），上傳要等一下。如果網頁上傳失敗，可分兩批：先 `index.html`、`.nojekyll` 與 `.github`，再傳 `assets`。
4. 進入 **Settings → Pages**，**Source** 選 **GitHub Actions**。
5. 到 **Actions** 分頁，看到 *Deploy to GitHub Pages* 變成綠色勾勾（約 1–2 分鐘），網址就會顯示在 Pages 設定頁。
6. 用手機開網址檢查：首頁插畫、Day 1–4、地圖、小叮嚀。

**備用做法（不用 Actions）**：Settings → Pages → Source 選 *Deploy from a branch*，Branch 選 `main`、資料夾 `/ (root)`，按 Save。

## 把舊網站的資料搬到新網址（很重要）

新舊網站網域不同，瀏覽器的儲存資料**不會自動同步**。每支手機或電腦都要各做一次：

1. 開啟**舊的 Claude Artifact 網站** → 底部「工具箱」→ **匯出 JSON**。檔案內容包含：行程勾選、各景點心得、行李清單、伴手禮、花費紀錄、小叮嚀進度。
   - 若沒有開始下載，請複製畫面下方出現的文字框內容，貼到備忘錄，存成 `.json` 檔。
2. 開啟**新網站** → 工具箱 → **匯入 JSON**，選剛剛的檔案。
3. 確認資料都在之後，才考慮停用舊網站。**請先不要刪除原本的 Artifact。**

注意：匯入是「取代」新網站上同類資料，不是合併；新網站如果已有資料，請先匯出一份備份。格式不對的檔案會被拒絕，原有資料不會被改動。

## 資料保存方式

- 所有進度與筆記只存在**每個人自己的瀏覽器**（localStorage，鍵名 `momiji-2026-v1`、`momiji-guide-v1`），不會上傳到 GitHub，也不會與別人同步。
- 無痕／私人瀏覽模式關閉後資料會消失；換手機、清除瀏覽資料前請先匯出 JSON。
- `yt1207.github.io` 底下你的其他網站共用同一個瀏覽器儲存空間；這兩個鍵名很特別，不會互相衝突。

## 之後想修改網站怎麼辦

**Claude 與 GitHub 不會自動同步。** 在 Claude 裡改好網站，並不會自動出現在 GitHub，一定要手動換檔。流程：

1. 在 Claude 說你要改什麼，並要求：「請輸出 GitHub 版 index.html（圖片路徑用 `assets/img/…`），不要用 `/_blob/` 網址」。
2. 下載 Claude 給你的新 `index.html`。
3. 到 GitHub 儲存庫 → 點 `index.html` → 右上角鉛筆（或 **Add file → Upload files** 覆蓋同名檔）→ 貼上或上傳 → **Commit changes**。
4. 到 **Actions** 等綠色勾勾，網址不變。
5. 換圖片：把新圖用**同樣的檔名**上傳到 `assets/img/`。

安全機制：
- 每次更新都會留下歷史版本，不會被蓋掉。
- 部署前會自動檢查：如果 `index.html` 還含有 Claude 專用的圖片網址（`/_blob/`），或缺少六張插畫，部署會**停止**並顯示紅色叉叉，線上網站維持上一個可用版本。
- 請不要修改程式裡的儲存鍵名（`momiji-2026-v1`、`momiji-guide-v1`），否則訪客原有進度會讀不到。

## 回復舊版

GitHub → **Code** → **Commits** → 找到想回去的那次提交 → 右側 **…** → **Revert**（或進入那次提交，把舊的 `index.html` 內容換回來）→ 等 Actions 綠色勾勾。

## 已知限制

- 字體（Zen Maru Gothic、Zen Old Mincho）從 Google Fonts 載入，沒網路時會改用系統字體。
- 沒有離線快取（不是 PWA）。「小叮嚀」頁有「複製離線小抄」可存進備忘錄。
- 資料不跨裝置同步，只有 JSON 匯入匯出。
- 內含的官方連結（海關、觀光廳、領事局等）與法規資料查核日為 2026-10-28，出發前請再確認。
