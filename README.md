# installLauncher — WatchLaterHub 電腦小幫手

[WatchLaterHub](https://github.com/101academyforyou/WatchLaterHub) 新分頁的「開啟」功能，可以點一下就開啟電腦上的軟體。
Chrome 擴充功能基於安全不能直接開啟程式，所以要裝一個「電腦小幫手」（Chrome Native Messaging host）。
這個 repo 提供安裝腳本 `install-launcher.sh` 與已編譯好的小幫手，**不需要 Rust**。

## 安裝（macOS／Linux）

打開「終端機」，貼上這行後按 Enter：

```bash
curl -fsSL https://raw.githubusercontent.com/101academyforyou/installLauncher/main/install-launcher.sh | bash
```

或下載這個 repo（Code → Download ZIP）解壓後，在資料夾裡執行：

```bash
./install-launcher.sh
```

裝好後回到新分頁，在「開啟」按「重新偵測」。（新分頁沒偵測到小幫手時，「開啟」面板也會顯示這行指令，可直接複製）

用終端機下載與安裝，不會出現 macOS「無法驗證開發者」的警告。

## 移除

```bash
curl -fsSL https://raw.githubusercontent.com/101academyforyou/installLauncher/main/install-launcher.sh | bash -s -- --uninstall
# 或在資料夾裡：./install-launcher.sh --uninstall
```

## 安裝了什麼

- 小幫手程式（只放在你的使用者資料夾，不需要管理員密碼）
  - macOS：`~/Library/Application Support/WatchLaterHub/launcher`（Apple 晶片與 Intel 通用）
  - Linux：`~/.local/share/watchlaterhub/launcher`
- 瀏覽器設定檔 `com.watchlaterhub.launcher.json`：Chrome、Chrome Beta、Chromium、Edge、Brave（有安裝的才設定）
- 只允許 WatchLaterHub 擴充功能（ID `lnokcijoconplpecgocjgkhhagkdmcce`）呼叫
- 小幫手只會列出與開啟電腦上已安裝的軟體，不會執行其他指令
- 下載的檔案會用 Release 裡的 `.sha256` 核對

## 從原始碼安裝

需要 [Rust](https://rustup.rs)：`./install-launcher.sh --from-source`
（下載失敗時也會自動改用原始碼編譯，前提是有 Rust 與 `launcher/` 資料夾）

## 發佈新版

推送到 `main`（或在 Actions 手動執行「release」）會編譯 macOS 通用版與 Linux x86_64，發佈到 Release `v<版本>`。
版本號在 `launcher/Cargo.toml`，改版本號就會產生新的 Release；安裝腳本固定下載最新版。

## 檔案

- `install-launcher.sh`：安裝／移除腳本
- `launcher/`：小幫手原始碼（Rust，與 WatchLaterHub repo 的 `launcher/` 相同）
- `.github/workflows/release.yml`：編譯與發佈
