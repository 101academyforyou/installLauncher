# installLauncher — WatchLaterHub 電腦小幫手安裝檔

[WatchLaterHub](https://github.com/101academyforyou/WatchLaterHub) 新分頁的「開啟」功能，可以點一下就開啟電腦上的軟體。
Chrome 擴充功能基於安全不能直接開啟程式，所以要裝一個「電腦小幫手」（Chrome Native Messaging host）。
這個 repo 負責把小幫手做成**雙擊就能安裝**的檔案，不需要 Rust 或終端機。

## 安裝（Mac）

1. 下載最新版：[WatchLaterHub-Launcher.pkg](https://github.com/101academyforyou/installLauncher/releases/latest/download/WatchLaterHub-Launcher.pkg)
   （新分頁「開啟」面板沒偵測到小幫手時，也會顯示「下載小幫手」按鈕）
2. 雙擊安裝檔，照指示按「繼續」
3. 回到新分頁，在「開啟」按「重新偵測」

若出現「無法驗證開發者」：到「系統設定 → 隱私權與安全性」，按「強制打開」。

移除：

```bash
sudo "/Library/Application Support/WatchLaterHub/uninstall.sh"
```

## 安裝內容

- 小幫手程式：`/Library/Application Support/WatchLaterHub/launcher`（Apple 晶片與 Intel 通用）
- 瀏覽器設定檔 `com.watchlaterhub.launcher.json`：
  - 整台電腦：Chrome、Chromium、Edge
  - 目前使用者：Chrome、Chrome Beta、Chromium、Edge、Brave（有安裝的才設定）
- 只允許 WatchLaterHub 擴充功能（ID `lnokcijoconplpecgocjgkhhagkdmcce`）呼叫
- 小幫手只會列出與開啟電腦上已安裝的軟體，不會執行其他指令

## Linux

```bash
git clone https://github.com/101academyforyou/installLauncher
cd installLauncher
./install-launcher.sh   # 需要 Rust：https://rustup.rs
```

## 產生安裝檔

- **自動**：推送到 `main`（或在 Actions 手動執行「release」）會在 Mac 環境編譯，發佈到 Release `v<版本>`。
  版本號在 `launcher/Cargo.toml`，改版本號就會產生新的 Release。
- **手動**（在 Mac 上）：`./build-pkg.sh` → `dist/WatchLaterHub-Launcher.pkg`

### 簽署與公證（選用，免除「無法驗證開發者」警告）

需要 Apple Developer 帳號。在 repo 的 Settings → Secrets 設定：

| Secret | 內容 |
|---|---|
| `MAC_INSTALLER_CERT_P12` | Developer ID Installer 憑證（.p12）的 base64 |
| `MAC_INSTALLER_CERT_PASSWORD` | .p12 的密碼 |
| `MAC_INSTALLER_SIGN_ID` | 憑證名稱，例如 `Developer ID Installer: Your Name (TEAMID)` |
| `APPLE_ID`、`APPLE_TEAM_ID`、`APPLE_APP_PASSWORD` | 公證用的 Apple ID、Team ID、App 專用密碼 |

## 原始碼

- `launcher/`：小幫手（Rust，與 WatchLaterHub repo 的 `launcher/` 相同）
- `pkg/postinstall`：安裝後設定瀏覽器；`pkg/uninstall.sh`：移除
- `build-pkg.sh`：產生 .pkg
- `install-launcher.sh`：Linux／開發者從原始碼安裝
