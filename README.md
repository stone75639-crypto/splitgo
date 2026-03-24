# SplitGo ⚡️ 極速分帳 App

> 旅遊、聚餐分帳神器，支援多幣種匯率，3 秒完成一筆記帳

---

## 🎯 專案目標

以手機操作為優先，打造一個「最少點擊次數就能記帳」的分帳工具。支援多人旅遊或聚餐情境，讓記帳不再是負擔。

---

## ✅ 已完成功能

### 🏠 首頁（Home）
- 活動 Banner：顯示活動名稱、總花費、每人均攤、待結清筆數
- 成員收支卡片（每人盈虧一目了然）
- 最近 5 筆帳目快覽
- 若無活動則顯示引導建立頁面

### ⚡️ 極速記帳（Quick Add FAB）
- 主畫面右下角大型 FAB 按鈕
- 彈出 Bottom Sheet 表單，包含：
  - **大字金額顯示** + 數字鍵盤（類計算機體驗）
  - 幣種快速 Chips（TWD/JPY/USD/EUR/KRW/HKD/GBP）
  - TWD 即時換算提示
  - 類別快速標籤（🍜食物/🚗交通/🏨住宿/🎭娛樂/🛒購物/📦其他）
  - 備註輸入（可選）
  - 付款人快速選擇
  - 分帳成員勾選（預設全選）
- 一鍵複製上筆記帳再修改
- 支援編輯已存在記帳

### 📋 記帳列表（Expenses）
- 所有記帳按時間倒序排列
- 點擊查看詳細資訊（金額、幣種換算、分帳明細）
- 可從詳情頁編輯或刪除

### ⚖️ 結算（Settlement）
- 每人收支總覽（付出 / 應付 / 淨額）
- **最優化轉帳計算**：用最少筆數達到平帳（貪心演算法）
- 每筆可標記「已付」
- 支援活動預設幣種顯示

### 📊 分析報表（Analytics）
- **類別圓餅圖**：各消費類別佔比
- **時間軸長條圖**：每日花費趨勢
- **每人排行**：誰付出最多的水平進度條

### 🗂️ 活動管理
- 建立/編輯/刪除活動
- 每個活動有唯一邀請碼（6碼）
- 活動設定：名稱、預設幣種、成員（最多6人）
- 匯率設定：7種幣種對 TWD 的匯率（支援手動設定或自動更新）
- 快速切換多個活動

### 🌐 分享與匯入
- 生成活動分享連結（將活動資料編碼在 URL hash 中）
- 原生分享 API（行動裝置）
- 邀請碼匯入
- 完整備份匯出（JSON）
- 從備份 JSON 匯入

### 🌙 其他
- **Dark Mode** 深色模式切換（自動記憶偏好）
- **PWA 支援**：可加入手機主畫面，像原生 App 一樣開啟
- **Service Worker** 離線快取
- 示範資料預載（東京旅遊 3 人 5 筆帳）
- 所有資料存於 localStorage（無需後端）

---

## 📁 檔案結構

```
index.html      # 主應用程式（Single Page App）
manifest.json   # PWA Manifest
sw.js           # Service Worker（離線快取）
README.md       # 本文件
```

---

## 🔗 功能入口

| 路徑 | 說明 |
|------|------|
| `index.html` | 主頁（首頁/記帳/結算/分析） |
| `index.html#import=<base64>` | 從分享連結匯入活動 |

### URL 參數
- `#import=<base64_encoded_event>` — 從朋友分享的連結自動匯入活動

---

## 💾 資料模型

### State（存在 localStorage `splitgo_data_v2`）
```json
{
  "events": [
    {
      "id": "string",
      "name": "東京旅遊",
      "currency": "JPY",
      "members": ["小明", "小華", "小美"],
      "rates": { "TWD": 1, "JPY": 0.22, "USD": 32.5 },
      "inviteCode": "ABC123",
      "createdAt": 1700000000000,
      "expenses": [
        {
          "id": "string",
          "amount": 3200,
          "currency": "JPY",
          "category": "hotel",
          "desc": "新宿酒店",
          "payer": "小明",
          "splits": ["小明", "小華", "小美"],
          "date": 1700000000000
        }
      ],
      "settlements": []
    }
  ],
  "currentEventId": "string",
  "settings": { "darkMode": false }
}
```

---

## 🎨 設計規格

- **主色調**：`#00BCD4` (Cyan) + `#0097A7` (Dark Cyan)
- **強調色**：`#FF6B6B` (Coral Red)
- **字體**：Inter (Google Fonts)
- **圖示**：Font Awesome 6
- **圖表**：Chart.js
- **設計風格**：Material Design 圓角卡片
- **響應式**：Mobile-first，支援桌面側邊欄模式

---

## 🚧 尚未實作的功能

- [ ] 雲端同步（多裝置即時同步）
- [ ] 成員照片 / 大頭貼上傳
- [ ] 不等比例分帳（按百分比或固定金額）
- [ ] 帳單掃描 OCR
- [ ] 推播通知提醒結清
- [ ] 匯率自動更新（目前使用 exchangerate-api.com，需網路）
- [ ] 帳目留言/備註照片
- [ ] 多語系（英語等）

---

## 🔮 建議下一步

1. **雲端同步**：整合 Firebase Realtime Database 或 Supabase，讓多人可以即時看到對方記的帳
2. **掃描收據**：整合 OCR API（如 Google Cloud Vision）自動辨識金額
3. **不等比例分帳**：讓使用者輸入每人分攤的比例或固定金額
4. **匯率自動更新**：在開啟活動時自動抓取最新匯率
5. **Native App**：使用 Capacitor 或 React Native 封裝成真正的 iOS/Android App

---

## 📱 如何加入主畫面（PWA）

### iOS Safari
1. 點選下方分享按鈕
2. 選擇「加入主畫面」
3. 點選「新增」

### Android Chrome
1. 點選右上角選單（⋮）
2. 選擇「新增到主畫面」
3. 點選「新增」

---

*SplitGo v1.0 — 2025 Built with ❤️*
