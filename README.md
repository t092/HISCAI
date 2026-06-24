# 國中社會科歷史 CAI 互動學習平台

戰後臺灣歷史課程的互動輔助教材，含學生端互動挑戰與教師後台成績管理。

## 專案結構

```
├── index.html                     # CAI 主入口頁（年級/學期/章節總覽）
├── 7B/                            # 七年級下學期課程目錄
│   ├── l04-postwar-politics.html  # L04 戰後臺灣的政治
│   ├── l05cross-strait-relations.html # L05 戰後臺灣的外交與兩岸關係
│   ├── l06 economy-society.html   # L06 戰後臺灣的經濟與社會
│   └── images/                    # 課程圖片素材
├── teacher.html                   # 教師後台（Firebase Auth 登入）
└── .gitignore
```

## 技術棧

- **前端**：HTML5/CSS3/原生 JavaScript（無前端框架依賴）
- **資料庫**：Firebase Firestore（Compat SDK）
- **教師驗證**：Firebase Authentication（Email/Password）
- **部署**：任何靜態網頁伺服器，或 Firebase Hosting

## 功能概覽

### 學生端
- 三關依序解鎖互動挑戰（排序、分類配對、翻翻樂）
- 扣分機制（每錯一題扣 5 分，最低 0 分）
- 通關證書彈窗，成績自動上傳 Firestore
- 觸控拖曳與點選配對備援

### 教師端
- Email/Password 登入驗證
- 統計卡片（總人數、平均分、滿分及格人數）
- 章節/班級/姓名即時篩選
- 表頭點擊排序、CSV 匯出
- 單筆刪除、批次清空

## 新增章節

複製現有章節 `.html`，修改三個位置：

1. `<title>` 與 `<h1>` 標題
2. `CHAPTER_CODE`（如 `"L07"`）與 `CHAPTER_TITLE`
3. `localStorage` key（如 `'l07_student_info'`）

教師後台無需修改，自動顯示新章節紀錄。

## Firestore 安全規則

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /completions/{docId} {
      allow read: if request.auth != null;
      allow create: if true;
      allow update, delete: if request.auth != null;
    }
  }
}
```
