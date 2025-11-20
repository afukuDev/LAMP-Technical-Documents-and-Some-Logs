# 在另一台 Ubuntu 下載 GitHub Repo 中指定資料夾教學（方法一）

本教學說明如何從 GitHub 私人 Repo：

```
https://github.com/afukuDev/medical
```

下載其中指定資料夾：

- `da/admin_new`
- `da/person`

到另一台 Ubuntu 的：

```
~/tmp/20251119
```

此方法採用 **git clone + 複製資料夾** 的方式，最穩定、最簡單，不需額外套件。

---

# 1️⃣ 建立目標資料夾

在另一台 Ubuntu 開啟 Terminal，輸入：

```bash
mkdir -p ~/tmp/20251119
```

---

# 2️⃣ Clone GitHub Repo 到本機

這會把整個 medical 專案下載到 `~/tmp/medical`：

```bash
cd ~/tmp
git clone https://github.com/afukuDev/medical
```

成功後資料結構會變成：

```
~/tmp/
 └── medical/
      ├── da/
      ├── image/
      ├── include/
      ├── config_1.php
      └── README.md
```

---

# 3️⃣ 複製指定資料夾到目標路徑

執行：

```bash
cp -r ~/tmp/medical/da/admin_new ~/tmp/20251119/
cp -r ~/tmp/medical/da/person ~/tmp/20251119/
```

執行後會在 `~/tmp/20251119/` 出現：

```
~/tmp/20251119/
 ├── admin_new/
 └── person/
```

---

# 4️⃣ 若要更新 repo（之後只需）

```bash
cd ~/tmp/medical
git pull
```

---

# 🎉 完成！

你已成功將 GitHub Repo 的指定資料夾下載到新電腦。

如需自動化腳本、自動同步，或只下載特定資料夾（SVN 方法），我可以進一步為你製作。

---

# 作者

本文件由 ChatGPT 教學協助產生。
