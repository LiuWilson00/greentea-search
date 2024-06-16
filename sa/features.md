# GREENTEA SEARCH API 和模組設計

## API 設計

### 用戶管理 API

- **User Registration API** (`POST /api/register`)

  - 功能：註冊新用戶，根據角色（客戶或律師）進行不同的註冊流程。
  - 對應模組：User Management Module

- **User Login API** (`POST /api/login`)

  - 功能：用戶登入驗證，返回授權令牌。
  - 對應模組：Authentication Module

- **User Profile API** (`GET /api/profile`, `PUT /api/profile`)

  - 功能：獲取和更新用戶基本資料。
  - 對應模組：User Management Module

- **User Verification API** (`POST /api/verify`)
  - 功能：用戶身份認證流程。
  - 對應模組：User Verification Module

### 搜尋功能 API

- **Basic Search API** (`GET /api/search`)

  - 功能：根據用戶輸入的案情進行基本搜索，返回匹配的 Token。
  - 對應模組：Search Engine Module
  - 可以參考 [feature-search](./feature-search.md)
  - MVP

- **Search Result API** (`GET /api/search/results/{token}`)

  - 功能：根據 Token 回傳律師列表。
  - 對應模組：Search Engine Module
  - 可以參考 [feature-search](./feature-search.md)
  - MVP

- **Chatbot Start API** (`GET /api/chatbot/start`)

  - 功能：啟動聊天機器人，引導用戶進行進階搜索。
  - 對應模組：Chatbot Module
  - 可以參考 [feature-search](feature-advanced-search.md)
  - MVP

- **Chatbot Socket** (`ws://{domain}/socket`)

  - 功能：接收用戶的初始輸入和詳細輸入，進行初步分類和範圍縮小。
  - 對應模組：Chatbot Module
  - 可以參考 [feature-search](feature-advanced-search.md)
  - MVP

### 律師管理 API

- **Lawyer Profile API** (`GET /api/lawyer/{id}`, `PUT /api/lawyer/{id}`)

  - 功能：獲取和更新律師個人資料。
  - 對應模組：Lawyer Management Module

- **Lawyer Rating API** (`POST /api/lawyer/{id}/rating`)

  - 功能：用戶對律師進行評分。
  - 對應模組：Rating Module

- **Lawyer Comment API** (`POST /api/lawyer/{id}/comment`)
  - 功能：用戶對律師進行評論。
  - 對應模組：Comment Module

### 發案管理 API

- **Case Management API** (`POST /api/case`, `GET /api/case/{id}`, `PUT /api/case/{id}`)

  - 功能：用戶提交新案件，查看案件詳情和更新案件狀態。
  - 對應模組：Case Management Module

- **Case Assignment API** (`POST /api/case/{id}/assign`)
  - 功能：將案件分配給律師。
  - 對應模組：Assignment Module

## 模組設計

### User Management Module

- 功能：處理用戶的註冊、登入、資料管理和認證。

### Authentication Module

- 功能：處理用戶的登入驗證，生成和管理授權令牌。

### User Verification Module

- 功能：處理用戶的身份認證流程。

### Search Engine Module

- 功能：處理基本和進階搜索請求。調用 OPEN AI 將用戶的回應分類，根據分類用類別距離計算的演算法，算出最合適的律師，並返回匹配結果。

### Advanced Search Module

- 功能：處理進階搜索功能，包括階段式詢問和表單搜索。

### Lawyer Management Module

- 功能：處理律師的個人資料管理和更新。

### Rating Module

- 功能：處理用戶對律師的評分功能。

### Comment Module

- 功能：處理用戶對律師的評論功能。

### Case Management Module

- 功能：處理用戶的案件提交、管理和狀態更新。

### Assignment Module

- 功能：處理案件分配給律師的流程。

### Crawler Engine Module

- 功能：爬取律師和法律文檔資料，並將其分類存入資料庫。

### Chatbot Module

- 功能：引導用戶進行進階搜索，逐步縮小範圍，彙整回應並提交搜索請求。
- **WebSocket 消息類型**：
  - `initial_prompt`: 向用戶發送初始提示。
  - `user_input`: 接收用戶輸入。
  - `follow_up_prompt`: 根據用戶回應進一步詢問。
  - `final_submission`: 通知用戶案情已提交。
  - `search_results`: 向用戶展示搜索結果。
