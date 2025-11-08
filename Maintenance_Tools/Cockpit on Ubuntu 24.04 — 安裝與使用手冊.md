#  Cockpit on Ubuntu 24.04 — 安裝與使用手冊

> 說明：安裝Cockpit on Ubuntu 24.04 與功能說明介紹。
> 
> 適用版本：Ubuntu 24.04 LTS (Noble Numbat)  
> 作者：afuku  
> 更新日期：2025-11-09

---

## 📘 一、Cockpit 介紹

### 1️⃣ 什麼是 Cockpit？
Cockpit 是 Red Hat 主導開發的 **開源網頁化 Linux 伺服器管理工具**，  
讓系統管理員能透過 Web 瀏覽器操作 Linux 伺服器，  
以圖形介面完成日常維護與監控工作。

官方網站：<https://cockpit-project.org/>

---

### 2️⃣ 主要特點
| 功能分類 | 說明 |
|-----------|------|
| 🔧 系統管理 | 管理服務 (systemd)、帳號、日誌、網路、磁碟等 |
| 📊 即時監控 | 顯示 CPU、RAM、磁碟 I/O 、網路流量等系統圖表 |
| 🤁 容器與虛擬機 | 整合 Podman、KVM/libvirt 模組 |
| 🧩 擴充性強 | 透過額外模組 (cockpit-podman、cockpit-machines 等) 延伸功能 |
| 🧠 輕量安全 | 使用 systemd socket on-demand 啟動，閒置時不占資源 |
| 🖥️ 多機管理 | 可從一台 Cockpit 主機連接與管理多台 Linux 伺服器 |

---

## ⚙️ 二、安裝步驟 (Ubuntu 24.04)

### 1️⃣ 系統更新
```bash
sudo apt update && sudo apt upgrade -y
```

### 2️⃣ 安裝 Cockpit
Ubuntu 24.04 的官方倉庫已內建 cockpit 套件：
```bash
sudo apt install cockpit -y
```

### 3️⃣ 啟用 Cockpit 服務
```bash
sudo systemctl enable --now cockpit.socket
```

- `enable` ：開機自動啟動  
- `--now` ：立即啟動服務  

確認狀態：
```bash
sudo systemctl status cockpit.socket
```

若顯示 `active (listening)` 代表成功。

---

### 4️⃣ 防火牆設定 (UFW)
Cockpit 預設使用 TCP 9090 埠：
```bash
sudo ufw allow 9090/tcp
sudo ufw reload
```

---

### 5️⃣ 瀏覽器連線
在瀏覽器輸入：
```
https://<伺服器IP>:9090
```
例如：`https://192.168.1.10:9090`

初次連線會顯示自簽憑證警告，可直接選擇 **繼續前往 (unsafe)**。

使用伺服器現有帳號登入，建議具 sudo 權限。

---

## 🦯 三、Cockpit 主要介面與功能

### 1️⃣ 系統概覽 (Overview)
- 顯示 CPU、RAM、磁碟 使用率。
- Reboot / Shutdown 按鈕。
- 顯示 hostname、OS 版本、內核版本等資訊。

### 2️⃣ 日誌 (Logs)
- 整合 journald 日誌，支援關鍵字/時間篩選。

### 3️⃣ 儲存 (Storage)
- 查看和掛載磁碟區。
- 建立 RAID、LVM、格式化磁碟等操作。

### 4️⃣ 網路 (Networking)
- 顯示 Ethernet / Wi-Fi 介面。
- 支援 Bonding 、Bridge 、VLAN 設定。
- 即時監控網路流量。

### 5️⃣ 帳號 (Accounts)
- 新增或刪除使用者。
- 管理 sudo 權限與密碼到期策略。

### 6️⃣ 服務 (Services)
- 與 systemd 整合，可啟動/停止/重啟服務。
- 可搜尋 .service 單元設定開機啟動。

### 7️⃣ 應用模組 (Applications)
| 模組 | 功能 |
|-----------|------|
| `cockpit-podman` | 管理 Podman 容器 |
| `cockpit-machines` | 管理 KVM/QEMU 虛擬機 |
| `cockpit-networkmanager` | 進階網路設定 |
| `cockpit-storaged` | 高階儲存與 LVM 操作 |

安裝：
```bash
sudo apt install cockpit-podman cockpit-machines cockpit-networkmanager -y
sudo systemctl restart cockpit.socket
```

---

## 🥉 四、使用與管理技巧

### 1️⃣ 多台伺服器管理
Cockpit 可在首頁上加入其他伺服器 IP，  
透過 SSH 連接跨機管理。

### 2️⃣ 安全建議
- 僅允許內網 IP 存取 9090 埠。  
- 若對外開放，建議配合 Nginx + Let's Encrypt 憑證。  
- 不推薦直接使用 root 登入。

### 3️⃣ 停用 Cockpit
```bash
sudo systemctl disable --now cockpit.socket
```

---

## 💡 五、故障排除
| 問題 | 解決方法 |
|------|----------------|
| 無法連線 9090 | 檢查 UFW 或 cockpit.socket 是否啟用 |
| 登入失敗 | 使用者無 sudo 權限或帳號被鎖定 |
| SSL 警告 | 自簽憑證常見，可自訂 CA 憑證 |
| 資源顯示異常 | 等待 systemd 服務資料收集完成。 |

---

## 🔚 六、結論
Cockpit 是 Ubuntu 24.04 中最輕量、直覺的伺服器管理工具，特別適合：
- 初學者認識 Linux 系統結構
- 學校或實驗室環境管理伺服器
- 家用、IoT、輔助 Web 系統操作

其 GUI 與 CLI 能完美互補，是 Linux 伺服器管理者的優秀選擇。

---

**延伸閱讀：**
- 官方文件：<https://cockpit-project.org/guide/latest/>
- Red Hat Blog：<https://www.redhat.com/en/blog/intro-cockpit>
- Ubuntu Wiki：<https://wiki.ubuntu.com/Cockpit>
