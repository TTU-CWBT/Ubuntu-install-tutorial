# Ubuntu-install-tutorial (USB bootable version)

```
    製作：Timothy_Su 蘇翰庭
    特別感謝：ChatGPT & Ubuntu-TW
```

## 目標

本教材將引導你如何在 Linux（如 Ubuntu）或 macOS 系統上，製作可開機的 Ubuntu 安裝 USB，並透過該 USB 安裝 Ubuntu 至目標主機。

---

## 目錄

1. [環境需求](#1-環境需求)
2. [下載 Ubuntu ISO](#2-下載 Ubuntu ISO)
3. [製作 USB 開機碟](#3-製作 USB 開機碟)
4. [BIOS / UEFI 設定](#4-BIOS / UEFI 設定)
5. [安裝 Ubuntu 系統](#5-安裝 Ubuntu 系統)
6. [安裝完成後的設定](#6-)

---

## 1. 環境需求

- 一台運行 Linux 或 macOS 的電腦
- 至少 8GB 容量的 USB 隨身碟
- 目標安裝的電腦（x86_64 架構）
- 穩定的網路連線

---

## 2. 下載 Ubuntu ISO

請至官方網站下載你需要的 Ubuntu ISO 映像檔（請注意你選擇的版本）：

https://ubuntu.com/download/server

---

## 3. 製作 USB 開機碟

###  macOS 系統

1. **插入 USB 隨身碟**（請先備份資料）
    > 你使用的USB會被格式化，所以請先備份
2. 開啟終端機，列出磁碟：
   ```bash
   diskutil list
   ```
3. 找到 USB 的裝置編號，例如 `/dev/disk2`
    >  如果不知道你的USB是哪一個，你可以插拔USB觀察消失的是哪一個disk
4. 卸載 USB：
   ```bash
   diskutil unmountDisk /dev/disk2
   ```
5. 將 ISO 寫入 USB：
   ```bash
   sudo dd if=~/Downloads/ubuntu-22.04-server-amd64.iso of=/dev/rdisk2 bs=4m status=progress
   ```
   > 注意：`rdisk2`（非 `disk2`）速度更快；`if` 為 ISO 路徑，請依實際情況修改。
   > 改為rdisk的話有機率會出錯，如果有錯誤，可以改回disk

6. 完成後安全退出：
   ```bash
   sudo sync
   ```

---

### 📌 Linux 系統（如 Ubuntu）

1. 插入 USB，確認裝置名稱（如 `/dev/sdb`）：
   ```bash
   lsblk
   ```
2. 寫入 ISO：
   ```bash
   sudo dd if=~/Downloads/ubuntu-22.04-server-amd64.iso of=/dev/sdX bs=4M status=progress
   ```
   > 請將 `/dev/sdX` 替換為正確的 USB 裝置名稱。
3. 寫入完成後同步緩衝區並拔除：
   ```bash
   sync
   ```

---

## 4. BIOS / UEFI 設定

在安裝 Ubuntu 前，請依下列步驟設定目標電腦：

- 開機時按下 `F2`、`F12`、`DEL`、`ESC` 等進入 BIOS 設定
- 啟用 USB 開機選項（Boot from USB）
- 如有 Secure Boot 選項，建議暫時 **關閉**
- 儲存並重新啟動，選擇從 USB 裝置開機

---

## 5. 安裝 Ubuntu 系統

1. 開機後選擇「Try or Install Ubuntu」
2. 選擇語言與鍵盤配置
3. 點選「Install Ubuntu」
4. 選擇安裝方式：
   - 與現有系統共存（可選）
   - **清除整個磁碟安裝 Ubuntu**（⚠️ 會清除全部資料）
5. 如果有SSH需求，記得開啟Install OpenSSH server
6. 設定：
   - 區域時間
   - 使用者名稱與密碼
7. 開始安裝，在安裝階段會很久，如果想確認有沒有在正常執行，可以點選`View full log`查看情況
8. 退出USB

---

## 6. 安裝完成後的設定建議

- **更新系統**：
  ```bash
  sudo apt update && sudo apt upgrade
  ```
- **安裝常用工具**：
  ```bash
  sudo apt install git vim curl build-essential htop tmux
  ```
- **安裝Ubuntu Desktop GUI**
  ```bash
  sudo apt install ubuntu-desktop
  ```
  ---

## 附註

- 建議安裝 LTS 版本（如 22.04 或 24.04），穩定且支援時間長
- 若有雙系統需求，請先備份現有系統，並手動分割磁碟
- macOS 的 `dd` 操作不可中途中斷，否則 USB 可能損壞

---



