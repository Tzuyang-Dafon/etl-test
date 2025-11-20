# PostgreSQL + pgAdmin（Docker Compose）

這個專案提供一份可快速啟動的 Docker Compose，用來建立：

* PostgreSQL 16
* pgAdmin 4（Web 管理工具）

資料與設定皆會存放在目前目錄中，方便備份與移動。

---

## 📁 專案結構

```
.
├─ docker-compose-postgre.yml
├─ postgre-data/      ← PostgreSQL 資料庫資料
└─ pgadmin-data/      ← pgAdmin 設定資料（需正確權限）
```

---

## 🚀 安裝與啟動

### 1. 建立並設定 pgAdmin 資料目錄（第一次使用時必做）

```bash
rm -rf pgadmin-data
mkdir pgadmin-data
sudo chown 5050:5050 pgadmin-data
sudo chmod 700 pgadmin-data
```

> pgAdmin Docker 映像檔使用 UID/GID 5050，因此此資料夾需要此權限才能正常啟動。

---

### 2. 啟動服務

```bash
docker compose -f docker-compose-postgre.yml up -d
```

### 3. 停止服務

```bash
docker compose -f docker-compose-postgre.yml down
```

---

## 🌐 服務資訊

### PostgreSQL

* Host：`localhost`
* Port：`5001`
* DB Name：`mydb`
* User：`myuser`
* Password：`mypassword`
* 資料儲存位置：`./postgre-data`

### pgAdmin

* URL：

  ```
  http://localhost:5000
  ```
* Login Email：`admin@example.com`
* Login Password：`admin123`
* 設定與 Session 儲存：`./pgadmin-data`

---

## 📝 在 pgAdmin 中連線 PostgreSQL

登入 pgAdmin 後：

1. 左側「Servers」右鍵 → **Create → Server…**
2. **General**

   * Name：`Local Postgres`
3. **Connection**

   * Host name/address：`postgres`
   * Port：`5432`（容器內部 port）
   * Username：`myuser`
   * Password：`mypassword`
   * Maintenance DB：`mydb`
4. 儲存後即可瀏覽資料庫。

---

## ❗ 常見問題

### 🔧 啟動 pgAdmin 時出現 504 或無法登入

請確保 `pgadmin-data` 目錄權限正確設定為：

```bash
sudo chown 5050:5050 pgadmin-data
sudo chmod 700 pgadmin-data
```

然後重新啟動：

```bash
docker compose -f docker-compose-postgre.yml down
docker compose -f docker-compose-postgre.yml up -d
```

---

如需加入自動連接的 `servers.json` 或更多設定，也可以再告訴我，我可以幫你補一份進階版 README。
