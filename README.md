# 🧠 雀雀認證 MBTI 測驗

> 用 20 題情境題，找出專屬於你的 MBTI 型態。理解自己、看見他人，讓關係更自在。

🔗 **線上體驗**：https://mbti-c4n.pages.dev

---

## ✨ 功能總覽

### 核心功能
| 功能 | 說明 |
|---|---|
| 20 題情境式測驗 | 雀雀設計的生活情境題（E/I、S/N、T/F、J/P 各 5 題），選完自動跳下一題 |
| 40 題完整版測驗 | 額外 20 題可切換，更精準的結果 |
| 測驗結果頁 | MBTI 四字母 + emoji + 名稱 + 四維度百分比條 |
| AI 個人化分析 | Gemini API 產生溫暖鼓勵，失敗 fallback 預設文字 |
| 適合職業推薦 | 每型 3 個職業建議（中英） |
| 名人對照 | 每型 2-3 個同型名人 |
| 溝通秘訣 | 每型一句相處建議 |
| 16 型人格介紹 | 四維度卡片 + 16 型 grid |
| 配對相容性 | SVG 圓環分數 + 等級 + 溝通建議 |
| 即時統計圖 | Bar chart + Doughnut chart（Chart.js） |
| 課程報名表單 | POST 到 Google Apps Script |
| 講師介紹 | 預留位置 |

### 體驗功能
| 功能 | 說明 |
|---|---|
| 中英文切換 | 66 個翻譯 key，即時切換 |
| 深色模式 🌙 | localStorage 記住偏好 |
| Canvas 分享圖片 | 600×800 PNG 可下載 |
| 歷史紀錄 | 再訪顯示上次結果 banner |
| 測驗滑動動畫 | slideIn 動畫 |
| RWD 響應式 | 桌面 / 平板 / 手機 |

### 商業導流
| 功能 | 說明 |
|---|---|
| LINE 官方帳號 CTA | 結果頁綠色按鈕導流 |
| Email 報告收集 | 收集潛在客戶名單 |
| 課程報名 | Google Sheet 寫入 |

---

## 🏗 技術架構

```
index.html（單一檔案，約 1700 行）
├── <style>  CSS（約 650 行）
│   ├── CSS Variables + Reset
│   ├── Nav / Hero / Blob 動畫
│   ├── 各頁面樣式（quiz / result / compat / stats / instructor / course）
│   ├── Modal / Toast / Dark Mode
│   └── RWD @media
├── <body>  HTML（約 350 行）
│   ├── Nav + Home Hero
│   ├── 7 個 Page Div（SPA 切換 display）
│   ├── Share Modal + Toast + Footer
│   └── Canvas（動態產生）
└── <script> JS（約 700 行）
    ├── 資料：TYPES(16型) / DIMS(4維度) / QS(20題) / QS_EXTRA(20題)
    ├── 資料：COMPAT_SCORES(16×16) / T(中英翻譯66key)
    ├── 邏輯：頁面切換 / 語言切換 / 測驗流程 / 計分
    ├── 邏輯：結果渲染 / AI分析 / 配對計算 / Chart.js圖表
    └── 邏輯：Canvas繪圖 / 表單送出 / 分享Modal / 深色模式
```

### 外部依賴（CDN）
- Google Fonts：Nunito + Noto Sans TC
- Chart.js 4.4.0

### 後端 API
| 端點 | 方法 | 用途 | 失敗行為 |
|---|---|---|---|
| `api.futurestarai.com/gemini` | POST | AI 人格分析 | fallback 硬編碼文字 |
| `api.futurestarai.com/mbti-stats` | GET | 統計數據 | fallback mock 資料 |
| `api.futurestarai.com/mbti-stats` | POST | 記錄結果 | 靜默失敗 |
| Google Apps Script | POST | 課程報名 | toast 錯誤 |

---

## 🛠 技術棧 & 技能

```
HTML5 / CSS3 / Vanilla JS(ES5) / Canvas API / Chart.js /
Fetch API / localStorage / Cloudflare Pages / Git
```

| 分類 | 技能 |
|---|---|
| **前端** | SPA 架構、RWD、CSS 動畫（blob/slideIn/pulse）、Canvas 繪圖、SVG 圓環 |
| **API 整合** | Gemini AI 串接、Google Apps Script、RESTful 設計、Fallback 機制 |
| **部署** | Cloudflare Pages CI/CD、DNS 網域管理 |
| **設計** | 色彩系統（4 主色）、UX 流程設計、響應式、深色模式 |
| **產品** | 增長漏斗設計、病毒傳播機制、名單收集、$0 成本控制 |
| **領域** | MBTI 四維度理論、Keirsey 互補理論、心理測驗設計 |
| **i18n** | 中英雙語即時切換（66 key） |

---

## 🚀 部署

| 項目 | 值 |
|---|---|
| 平台 | Cloudflare Pages |
| 生產分支 | `claude/mbti-quiz-webpage-qO6lp` |
| Pages 網址 | https://mbti-c4n.pages.dev |
| 自訂網域 | `mbti.futurestarai.com`（待綁定） |
| Build command | （無） |
| Build output | `/` |

Push 到生產分支後 Cloudflare Pages 在 30-60 秒內自動重新部署。

### 開發流程
1. 修改 `index.html`
2. 語法驗證：
```bash
node -e "const fs=require('fs');const h=fs.readFileSync('index.html','utf8');const m=[...h.matchAll(/<script(?![^>]*src)[^>]*>([\s\S]*?)<\/script>/g)];m.forEach((b,i)=>{try{new Function(b[1]);console.log('Block',i,'OK')}catch(e){console.log('Block',i,'ERROR:',e.message)}})"
```
3. `git commit` + `git push` → 自動部署
4. 打開 https://mbti-c4n.pages.dev（Ctrl+Shift+R 清快取）

---

## 🎨 品牌

| 項目 | 值 |
|---|---|
| 主品牌 | futurestarai.com（未來之星 AI） |
| 子網域格式 | `<project>.futurestarai.com` |
| 本專案 | `mbti.futurestarai.com` |
| API 入口 | `api.futurestarai.com` |
| 主色 | `#FF6B9D` 粉 · `#9B59F5` 紫 · `#FFD93D` 黃 · `#4ECDC4` 青 |
| 背景 | `#FFF9F5`、卡片 `#FFFFFF`、文字 `#1A1A2E` |
| 字體 | Nunito + Noto Sans TC |

---

## 📈 商業邏輯

```
社群曝光（IG/LINE 分享卡片）
    ↓
首頁（blob 動畫 + CTA 按鈕）
    ↓
20/40 題測驗（互動留存）
    ↓
結果頁（AI 分析 + 職業 + 名人 → 驚喜感 → 分享動機）
    ↓
┌─ 📸 分享圖片 → 病毒式擴散
├─ 💚 加入 LINE → 私域流量池
├─ 📧 Email 報告 → 精準名單
├─ 💞 配對功能 → 二次傳播
└─ 📚 課程報名 → 付費轉化
```

---

## ⚠️ 待辦事項

| 優先級 | 項目 |
|---|---|
| 🔴 | 替換 `FORM_EP` 為實際 Google Apps Script URL |
| 🔴 | 建立 LINE 官方帳號 @queque-mbti |
| 🟡 | 綁自訂網域 `mbti.futurestarai.com` |
| 🟡 | 加 SEO meta（og:title / og:image） |
| 🟡 | 加 Google Analytics 追蹤 |
| 🟢 | 配對頁加免責聲明 |
| 🟢 | 更新講師真實照片和 bio |

---

## 📄 授權

© 2026 未來之星 AI — 雀雀認證 MBTI
