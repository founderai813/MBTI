# CLAUDE.md

## 專案概述

「雀雀認證 MBTI」是一個單頁式（SPA）MBTI 人格測驗網站。使用者透過 20 題雀雀設計的情境題，測出自己的 MBTI 型態，並獲得 AI 個人化分析、配對相容性比較、即時統計圖表等功能。

網站為**單一 index.html 檔案**，無框架、無 build 步驟，純 HTML + CSS + vanilla JavaScript。

## 架構

### 前端（index.html，約 1460 行）

| 區塊 | 說明 |
|---|---|
| CSS（約 650 行） | CSS variables、nav、hero blob 動畫、quiz/result/compat/stats/instructor/course 各頁樣式、modal、toast、RWD |
| HTML（約 350 行） | SPA 結構：nav + home hero + 7 個 page div + share modal + toast + footer |
| JS 資料（約 200 行） | TYPES（16型）、DIMS（4維度）、QS（20題）、COMPAT_SCORES（16×16矩陣）、T（中英翻譯 66 key） |
| JS 邏輯（約 270 行） | 頁面切換、語言切換、測驗流程、計分、結果渲染、AI 分析、配對計算、Chart.js 圖表、表單送出、分享 modal |

### 外部依賴（CDN）

- Google Fonts：Nunito + Noto Sans TC
- Chart.js 4.4.0（bar chart + doughnut chart）

### 後端 API

| 端點 | 方法 | 用途 | 失敗行為 |
|---|---|---|---|
| `https://api.futurestarai.com/gemini` | POST `{prompt}` | AI 人格分析（轉接 Gemini） | fallback 到硬編碼文字（6 型有預設） |
| `https://api.futurestarai.com/mbti-stats` | GET `?action=get` | 取得各型態人數統計 | fallback 到 mock 資料 |
| `https://api.futurestarai.com/mbti-stats` | POST `{type}` | 記錄測驗結果 | 靜默失敗 |
| Google Apps Script（FORM_EP） | POST no-cors | 課程報名寫入 Google Sheet | toast 顯示錯誤 |

> FORM_EP 目前是 placeholder `YOUR_SCRIPT_ID`，上線前需替換。

### JS 語法規則

- 所有變數用 `var`（不用 let/const）
- 函式用 `function` 關鍵字（不用箭頭函數）
- 字串拼接用 `+`（不用 template literal）
- JS 字串外層用單引號，HTML 屬性用雙引號
- 所有事件用 `addEventListener`（不用 onclick 屬性）

## 部署

| 項目 | 值 |
|---|---|
| 平台 | Cloudflare Pages |
| GitHub repo | `founderai813/MBTI` |
| 生產分支 | `claude/mbti-quiz-webpage-qO6lp` |
| Pages 網址 | `https://mbti-c4n.pages.dev` |
| 自訂網域 | `mbti.futurestarai.com`（待綁定） |
| Build command | （無，直接部署 HTML） |
| Build output | `/`（根目錄） |

推送到生產分支後，Cloudflare Pages 會在 30-60 秒內自動重新部署。

## 工作流程

1. 在 `claude/mbti-quiz-webpage-qO6lp` 分支修改 `index.html`
2. 語法驗證：`node -e "const fs=require('fs');const h=fs.readFileSync('index.html','utf8');const m=[...h.matchAll(/<script(?![^>]*src)[^>]*>([\s\S]*?)<\/script>/g)];m.forEach((b,i)=>{try{new Function(b[1]);console.log('Block',i,'OK')}catch(e){console.log('Block',i,'ERROR:',e.message)}})"`
3. commit + push → Cloudflare 自動部署
4. 驗證：打開 https://mbti-c4n.pages.dev（Ctrl+Shift+R 清快取）

## 品牌

- 主品牌網域：`futurestarai.com`
- 子網域格式：`<project>.futurestarai.com`
- 本專案：`mbti.futurestarai.com`
- API 統一入口：`api.futurestarai.com`
- 主色：`#FF6B9D`（粉）、`#9B59F5`（紫）、`#FFD93D`（黃）、`#4ECDC4`（青）
- 背景：`#FFF9F5`、卡片 `#FFFFFF`、文字 `#1A1A2E`
- 字體：Nunito + Noto Sans TC
