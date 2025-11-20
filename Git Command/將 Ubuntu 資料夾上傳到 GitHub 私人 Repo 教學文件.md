# 將 Ubuntu 資料夾上傳到 GitHub 私人 Repo 教學文件

本文件整理今日完整教學流程，協助你將 Ubuntu 內 **任意資料夾** 正確、安全地上傳到 GitHub 私人 repository。

---

## 📌 目標
將 Ubuntu 的資料夾：

```
~/文件/server_backup/20251119_1/medical
```

上傳到 GitHub 私人 repo：

```
https://github.com/afukuDev/medical
```

---

# 1️⃣ 在 Ubuntu 開啟 Terminal 並進入專案資料夾

```bash
cd ~/文件/server_backup/20251119_1/medical
```

---

# 2️⃣ 初始化 Git Repo

如果是第一次 push，必須讓這個資料夾變成 Git repository：

```bash
git init
```

---

# 3️⃣ 設定遠端 GitHub Repo

```bash
git remote add origin https://github.com/afukuDev/medical
```

如果 origin 已存在，改用：

```bash
git remote set-url origin https://github.com/afukuDev/medical
```

---

# 4️⃣ 新增所有檔案到 Git

```bash
git add .
```

---

# 5️⃣ 建立 Commit

```bash
git commit -m "Initial upload from Ubuntu"
```

---

# 6️⃣ GitHub 不再允許密碼登入 → 必須建立個人存取 Token (PAT)

前往：

👉 https://github.com/settings/tokens

選擇：

- **Tokens (classic)**
- Note：Ubuntu_push
- Expiration：可自訂
- ✔ 勾選：`repo`（即可管理私人 repo）

按下 **Generate Token**  
複製產生的 token（僅顯示一次）。

---

# 7️⃣ 當 GitHub repo 不是空的 → 必須先 `git pull`

因為 GitHub 預設有 README.md，因此你 push 會被擋住：

```
! [rejected] main -> main (fetch first)
```

正確做法：

### 設定 pull 使用 merge（避免 rebase 複雜化）

```bash
git config pull.rebase false
```

### 拉取遠端內容，允許不同歷史合併

```bash
git pull origin main --allow-unrelated-histories
```

若進入 merge 編輯器（nano），按：

```
Ctrl + O
Enter
Ctrl + X
```

---

# 8️⃣ 合併成功後 → 推送到 GitHub

```bash
git push -u origin main
```

登入時：

```
Username: afukuDev
Password: <剛剛的 GitHub Token>
```

---

# 9️⃣ （可選）讓 Git 記住 Token，不再重複輸入

```bash
git config --global credential.helper store
```

只需 push 一次，即會永久記錄。

---

# 🔟 最終成果（成功畫面）

完成後 GitHub 會出現：

- da/
- image/
- include/
- config_1.php
- README.md

並且顯示 commit：

```
Initial upload from Ubuntu
Merge branch 'main'
```

---

# 🎉 完成！

你已成功將 Ubuntu 的整個 medical 專案上傳到 GitHub 私人 repo。

若之後需要：

- 自動化 push script  
- 製作 .gitignore  
- 檢查 repo 內敏感資訊  
- 建立 CI/CD  
- 清理專案結構  

都可以再告訴我，我可以為你生成。

---

# 作者
此文件由 ChatGPT 教學協助產生。
