
## 地推業務機器人專案筆記

### 1. 目標

訓練一個專業的地推業務機器人，協助業務人員進行高效外訪推廣、客製化策略建議與持續成長。

---

### 2. 功能架構

#### A. 背景資料說明

- 蒐集目標診所相關資料  
  - 地址、主治醫師、過往拜訪紀錄  
  - 競品狀況、市場分析等
- 建立知識庫，結合 RAG（Retrieval-Augmented Generation）技術即時查詢診所背景

#### B. 每次拜訪錄音分析

- 支援錄音檔案自動上傳
- 語音轉文字及情緒/重點抽取  
  - 擷取關鍵字（需求、疑慮、競品反應、承諾等）
  - 分析對話內容（情感、成交機率、下一步建議）
- 自動生成會後報告  
  - 例如：「本次對方關注 X 功能，態度積極，但對價格仍有疑慮，建議下次帶競品比較表。」

#### C. 自動修正的閉環

- 根據錄音分析自動調整拜訪策略
- AI 學習閉環：每次拜訪都讓機器人更懂現場、建議更個人化
- 自動產生最佳戰略建議，動態優化策略

---

### 3. 技術架構與實作路徑

- **模型平台**：Hugging Face Spaces + Taide LLM，支援繁體中文及醫療領域
- **語音辨識**：Whisper 或 faster-whisper
- **資料增強**：RAG（檢索式增強生成）+ 診所背景與歷史拜訪紀錄
- **資料庫**：AkamaiDB（Postgres）存儲所有紀錄、分析結果與用戶資料
- **前端介面**：GitHub Pages 部署（React + shadcn/ui），提供使用者互動與管理界面
- **自動化閉環**：分析 → 建議 → 修正 → 再出訪，持續提升業務策略與表現

---

### 4. 預期流程

1. 蒐集背景資料，查詢診所相關資訊
2. 地推拜訪，錄音並自動上傳
3. 語音自動分析，產生會後報告與判斷狀態
4. AI 依據資料回饋最佳策略建議
5. 下次拜訪前，AI 提供客製化重點與行動建議

---

### 5. 擴充方向

- 手機 App 或 Web 整合（提升現場便利性）
- 團隊管理與績效追蹤、知識共用
- 多租戶支援，企業級擴展
- 線上 BI Dashboard 與後台分析工具

---

如需更細節的系統流程圖、範例 Prompt、API 設計或資料庫結構，歡迎參考專案其他文件，或與開發團隊聯繫。


### 專案架構

```plaintext
/
│
├── ui/                      # 前端 (GitHub Pages 靜態網站)
│   ├── public/              # 靜態資源、favicon、robots.txt
│   ├── src/                 # React/Next.js 程式碼（shadcn/ui元件等）
│   │   ├── components/      # 元件
│   │   ├── pages/           # 頁面（或 /app 依框架）
│   │   ├── api/             # 前端直接調用的 api proxy（如需要）
│   │   └── ...              # 其他前端資料夾
│   ├── package.json         # 前端專案管理
│   ├── vite.config.js       # 或 next.config.js（依前端框架）
│   └── README.md            # 前端說明
│
├── api/                     # 後端 (Hugging Face Space 主機)
│   ├── app.py               # FastAPI/Gradio主程式
│   ├── requirements.txt     # Python 套件
│   ├── hf_space.yaml        # Hugging Face Space 設定（如有）
│   ├── modules/             # 模型、音檔分析、RAG等功能模組
│   ├── db/                  # DB 連線、Schema定義、操作程式
│   ├── shared/              # （可選）前後端共用 schema/type
│   └── README.md            # 後端說明
│
├── shared/                  # 前後端共用（如有 API schema、TypeScript type 等）
│   ├── openapi.yaml         # API 規格文件
│   ├── types.ts             # TypeScript 型別定義（前後端共用）
│   ├── prompts/             # 標準 prompt、system prompt 範例
│   └── ...                  # 其他可共用資源
│
├── .github/
│   └── workflows/
│       ├── deploy-ui.yml    # 只監控 ui/，自動部署 GitHub Pages
│       └── deploy-api.yml   # 只監控 api/，自動部署 Hugging Face Space
│
├── docs/                    # 技術文件、操作教學、系統架構圖
│   ├── images/              # 架構圖/流程圖
│   ├── setup.md             # 部署教學
│   └── ...
│
├── README.md                # 專案總說明
└── LICENSE                  # 授權
```
