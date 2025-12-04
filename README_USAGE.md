# 🎿 雪國旅記 - Web App 使用指南

## 📱 快速開始

你的 Web App 已完全配置，可立即使用！

### 訪問方式

**本地開發環境:**
```
http://localhost:8000/index.html
```

**GitHub Pages 部署 (推薦):**
1. 推送到 GitHub
2. 在 Settings → Pages 中啟用 GitHub Pages
3. 選擇 main branch 為來源
4. 訪問: `https://你的用戶名.github.io/trip.8713/index.html`

---

## 📲 在 iPhone 上安裝為 App

### 步驟 1: 打開 Web App
- 用 **Safari** 打開你的 Web App 地址

### 步驟 2: 新增到主屏幕
1. 點擊底部分享按鈕 **[↗️]**
2. 向下滑，點擊 **「Add to Home Screen」**
3. 確認 App 名稱 (預設: 雪國旅記)
4. 點擊 **「Add」**

### 步驟 3: 完成 ✨
- Chiikawa 圖示現在會出現在你的 iPhone 主屏幕上！
- 點擊即可以 App 模式打開（全屏，無 Safari 導航欄）

---

## 🤖 在 Android 上安裝為 App

### 步驟 1: 打開 Web App
- 用 **Chrome** 打開你的 Web App 地址

### 步驟 2: 安裝應用
1. 點擊右上角菜單 **[⋮]**
2. 點擊 **「安裝應用」** 或 **「Install app」**
3. 確認並完成安裝

### 步驟 3: 完成 ✨
- App 已安裝到你的手機主屏幕
- 點擊即可開啟

---

## 🌐 在線部署選項

### 選項 A: GitHub Pages (免費 & 推薦)
```bash
git add .
git commit -m "Add PWA travel guide"
git push origin main
```
然後在 GitHub Settings 啟用 Pages。

### 選項 B: Netlify (免費 & 簡單)
1. 訪問 [netlify.com](https://netlify.com)
2. 連接你的 GitHub 倉庫
3. 自動部署完成！

### 選項 C: Vercel (免費 & 快速)
1. 訪問 [vercel.com](https://vercel.com)
2. 導入 GitHub 項目
3. 自動部署完成！

---

## 📋 Web App 功能

✅ **完整行程** - 16 天詳細日程規劃
✅ **實時天氣** - 每個城市當前天氣
✅ **日期選擇** - 輕鬆在不同日期間切換
✅ **準備清單** - 可勾選的行前準備清單
✅ **住宿彙整** - 自動整理所有酒店信息
✅ **Google Maps 導航** - 每個景點一鍵導航
✅ **緊急聯絡** - 駐日代表處急難救助電話
✅ **時間軸設計** - 清晰的日程時間軸展示

---

## 🎨 Chiikawa 圖示說明

- **圖示檔案**: `chiikawa-icon.png` (192x192px)
- **格式**: PNG 格式（支援透明背景）
- **使用場景**: 
  - iPhone "Add to Home Screen"
  - PWA manifest icon
  - 瀏覽器標簽頁 icon

### 自定義圖示
如果想更換圖示，只需將新的 PNG 檔案（192x192px）替換 `chiikawa-icon.png` 即可。

---

## 🔧 常見問題

**Q: Chiikawa 圖示在 iPhone 上不顯示？**
A: 
1. 確保使用 Safari 打開
2. 等待頁面完全加載
3. 清除 Safari 緩存：設置 → Safari → 清除歷史記錄和網站數據

**Q: 如何更新 Web App 內容？**
A: 編輯 `index.html`，保存後刷新即可。

**Q: 可以離線使用嗎？**
A: 需要添加 Service Worker。可選功能。

**Q: 在哪兒找到訪問地址？**
A: 
- 本地: `http://localhost:8000`
- 部署後: 查看 GitHub Pages/Netlify 的公開 URL

---

## 📝 文件結構

```
/workspaces/trip.8713/
├── index.html              # 主要 Web App (HTML 完整應用)
├── README.md               # 原始 HTML 文件
├── chiikawa-icon.png       # Chiikawa 圖示 (PWA icon)
└── README_USAGE.md         # 本文檔
```

---

## 🚀 後續建議

1. **部署到 GitHub Pages** - 獲得永久的線上訪問地址
2. **配置 HTTPS** - GitHub Pages 自動提供
3. **分享給朋友** - 複製 URL 分享給旅伴
4. **定期更新** - 行程有變化時隨時修改

---

## 📞 需要幫助？

如有問題，檢查：
1. ✅ 伺服器是否運行 (`http://localhost:8000`)
2. ✅ `index.html` 檔案是否存在
3. ✅ `chiikawa-icon.png` 檔案是否存在
4. ✅ 網路連接是否正常

祝你旅途愉快！🎿❄️🗻
