# 🧱 Apache 網站安全設定與43禁止訪問說明

> 說明：如何在 Apache 封鎖 /medical/da/ 目錄外部存取，同時允許子資料夾運作。
> 
> 適用版本：Ubuntu 24.04 LTS (Noble Numbat)  
> 作者：afuku  
> 更新日期：2025-11-09


---

## 📌 一、目標說明

本文件說明如何設定 Apache：
- 🔒 封鎖外部直接瀏覽某目錄 `/medical/da/`
- 🚫 禁止目錄列出（防止被掃目錄）
- ✅ 允許子資料夾（例如 `/medical/da/admin/`）正常運行
- 🧩 阻擋敏感副檔名（如 `.sql`, `.log`, `.txt` 等）
- 🧠 強化整體伺服器安全性

---

## 📂 二、修改 Apache VirtualHost 設定

編輯設定檔：
```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

在 `<VirtualHost *:80>` 區塊內加入以下內容：

```apache
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    # 禁止列目錄，但允許 PHP 正常執行
    <Directory /var/www/html/medical/da/>
        Options -Indexes
        Require all granted
    </Directory>
</VirtualHost>
```

儲存後檢查語法：
```bash
sudo apachectl configtest
```
若顯示：
```
Syntax OK
```
即可重啟 Apache：
```bash
sudo systemctl restart apache2
```

---

## 🧾 三、設定 .htaccess 文件

在 `/var/www/html/medical/da/` 內建立或編輯 `.htaccess`：

```bash
sudo nano /var/www/html/medical/da/.htaccess
```

填入以下內容：

```apache
# 禁止列出目錄內容
Options -Indexes

# 允許 index 頁面執行
<FilesMatch "^(index\.html|index\.php)?$">
    Require all granted
</FilesMatch>

# 封鎖敏感副檔名
<FilesMatch "\.(sql|log|txt|ini|bak|conf)$">
    Require all denied
</FilesMatch>
```

---

## 🧱 四、權限設定建議

確保檔案權限正確：

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

---

## ✅ 五、測試結果驗證

| 測試網址 | 預期結果 |
|-----------|------------|
| `http://120.117.xx.xx/medical/da/` | 顯示「403 Forbidden」 |
| `http://120.117.xx.xx/medical/da/admin/index.php` | 正常顯示 PHP 頁面 |
| `.sql`, `.txt`, `.log` 檔 | 無法被外部存取 |

---

## 🧪 六、可選進階安全加強（建議）

可在 `.htaccess` 中額外加入以下段落：

```apache
# 強制使用 UTF-8
AddDefaultCharset UTF-8

# 隱藏 Apache 版本資訊
ServerSignature Off
ServerTokens Prod

# 開啟壓縮（需啟用 mod_deflate）
<IfModule mod_deflate.c>
    AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css application/javascript
</IfModule>

# 若伺服器支援 HTTPS，可自動導向
# RewriteEngine On
# RewriteCond %{HTTPS} !=on
# RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

---

## 🦯 七、常見錯誤排除

| 錯誤訊息 | 原因 | 解法 |
|------------|------|------|
| `500 Internal Server Error` | `.htaccess` 中出現 `<Directory>` | 刪此區塊 |
| `403 Forbidden` | `Require all denied` 導致整層封鎖 | 改為允許 `FilesMatch` index |
| `AH00526` 語法錯誤 | Apache 語法錯誤 | 執行 `sudo apachectl configtest` |
| `.sql` 檔仍可下載 | `.htaccess` 沒套用 | 檢查 `AllowOverride All` 是否啟用 |

---

## 🧩 八、結論

完成以上設定後：
- 外部使用者將無法列出 `/medical/da/` 目錄內容。
- 子目錄（例如 `/admin/`）仍可正常使用。
- Apache 不會顯示版本號與不必要的錯誤資訊。
- 整體安全性提升，減少被掃描與入侵的風險。

---

> 🔒 **作者註：** 此設定適用於 Ubuntu 20.04~24.04 的 Apache 2.4.x 系列版本。

