# My Work Item
這是一個基於 .NET MVC 架構開發的 B2E (Backend to Employee) 工作項目管理系統。本專案展示了各人化狀態追蹤、權限控管 (RBAC) 以及資安加密 (Salted Hash) 的實作。

## 技術堆疊

* **Framework:** .NET 10.0 (ASP.NET Core MVC)
* **Database:** SQL Server (SQLEXPRESS)
* **ORM:** Entity Framework Core
* **Authentication:** Cookie-based Authentication with Custom Salted Hash
* **Tools:** Antigravity(or any other AI editor)

---
## 快速啟動
請依照以下步驟在本地環境啟動專案：

### 1. 下載專案
開啟終端機 (Terminal) 並執行以下指令：

```bash
git clone https://github.com/chungkai1029/MyWorkItem.git
cd MyWorkItem
```

### 2. 環境準備
確保您的環境已安裝：
- .NET 10.0 SDK
- SQL Server (SQLEXPRESS)

### 3. 資料庫初始化
本專案使用 Entity Framework Core。在首次執行前，必須執行 Migration 以建立資料庫結構並寫入種子資料 (Seed Data)。

```bash
# 還原相依套件
dotnet restore

# 執行資料庫更新與種子資料寫入 (這步會建立預設 Admin 與 User)
dotnet ef database update
```

注意： 請事先確認 `appsettings.json` 中的 `ConnectionStrings` 是否正確指向您的本地資料庫實體。

### 4. 啟動專案

```bash
dotnet run
```

專案啟動後，請至 `http://localhost:5038` 進行存取。

---
## 測試帳號
為了方便面試演示，系統在初始化時已預先寫入以下兩組帳號。密碼皆須經過 SHA256 + Salt 加密處理。

| 角色 (Role) | 帳號 (Username) | 密碼 (Password) | 權限說明 |
| --- | --- | --- | --- |
| 管理員 | admin | 1234 | 可進入後台新增、修改、刪除 Work Items |
| 使用者 | testUser | 1234 | 僅能查看列表、執行個人化確認 (Confirm) |

---
## API 路徑 (Key Features)
[Demo 路徑](http://localhost:5038)

| 功能模組 | URL 路徑 | 說明 |
| --- | --- | --- |
| 前台-登入 | [/login](http://localhost:5038/login) | 系統入口，請使用上方帳號登入。 |
| 前台-使用者相關 | [/users](http://localhost:5038/users) | (需登入) 一般使用者帳號密碼修改。 |
| 前台-WorkItems | [/work-items](http://localhost:5038/work-items) | (需登入) 一般使用者查看列表，可測試勾選與確認功能。 |
| 前台-WorkItem Detail | [/work-items/{id}](http://localhost:5038/work-items/{id}) | (需登入) 一般使用者查看詳細工作項目。 |
| 後台-登入 | [/admin](http://localhost:5038/admin) | (需 Admin 權限) 管理員登入。 |
| 後台-WorkItems 管理 | [/admin/work-items](http://localhost:5038/admin/work-items) | (需 Admin 權限) 管理員維護介面，可執行 CRUD。 |
| 後台 - WorkItem Detail | [/admin/work-items/{id}](http://localhost:5038/admin/work-items/{id}) | (需 Admin 權限) 管理員修改詳細工作項目。 |
| Swagger | [/swagger/index.html](http://localhost:5038/swagger/index.html) | (開發模式) API 文件與測試介面 (若有實作 API)。 |

---
## AI 工具應用說明
本專案在開發過程中將使用 Antigravity 作為輔助工具，主要應用於：
- 資料庫模型生成：協助生成 EF Core Model。
- 資安演算法實作：生成 `PasswordService` 中的 `Salt` 與 `Hash` 邏輯。
- 假資料生成：協助產生 `OnModelCreating` 中的 Seeding Data。
- 前端頁面生成：協助產生 Views 以及 API Controller。

---
## 專案結構與設計文件
- API 規格: [Swagger UI Link](http://localhost:5038/swagger/index.html) (啟動後可見)
- 資料庫設計: 採用正規化設計 (3NF)，將 `User` 與 `WorkItem` 透過 `UserWorkItemStatus` 進行多對多關聯，並獨立 `Role` 表格以利擴充。