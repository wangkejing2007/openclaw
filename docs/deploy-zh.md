---
title: 部署指南 (Deploy Guide)
---

# 如何部署 Moltbot (OpenClaw)

OpenClaw 是一個需要後端運行的 AI 助手。您不能僅使用 GitHub Pages 運行它，因為它需要一個伺服器來處理 WebSocket 連接和代理運行時。

有兩種主要的部署方式：

## 1. 使用 GitHub Codespaces (快速體驗)

這是最快體驗 Moltbot 的方式，直接在您的瀏覽器中運行。

1.  確保您已登錄 GitHub。
2.  在項目主頁點擊綠色的 **"Code"** 按鈕。
3.  切換到 **"Codespaces"** 標籤。
4.  點擊 **"Create codespace on main"**。
5.  等待環境構建完成（這可能需要幾分鐘）。
    - 系統會自動安裝依賴並構建項目。
6.  在終端中運行以下命令來啟動 Moltbot:
    ```bash
    # 安裝並啟動服務
    pnpm openclaw onboard --install-daemon
    
    # 或者手動啟動
    pnpm gateway
    ```
7.  VS Code 會提示您轉發端口 `18789`。點擊 "Open in Browser" 即可訪問控制面板。

## 2. 部署到 Render (長期運行)

如果您希望機器人 24/7 在線，建議部署到 Render（有免費層）。

### 部署步驟

1.  **Fork 本倉庫** 到您的 GitHub 帳戶。
2.  點擊下方的按鈕直接部署：
    
    [![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/openclaw/openclaw)

3.  **配置 Render 服務**:
    - Render 會讀取 `render.yaml` 藍圖。
    - 您需要設置 `SETUP_PASSWORD`（設置嚮導的密碼）。
    - 其他環境變量如 `OPENCLAW_GATEWAY_TOKEN` 會自動生成。

4.  **選擇方案**:
    - **Starter (推薦)**: 大約 $7/月，包含持久化硬盤，不會休眠。
    - **Free**: 免費，但閒置 15 分鐘後會休眠，且**沒有持久化存儲**（重啟後數據會丟失）。
      - 如果使用免費版，請在您的 Fork 中修改 `render.yaml`，將 `plan: starter` 改為 `plan: free`。

### 部署後設置

1.  等待部署完成，訪問 `https://<您的服務名>.onrender.com/setup`。
2.  輸入您設置的 `SETUP_PASSWORD`。
3.  配置您的 AI 模型（如 Anthropic 或 OpenAI）。
4.  連接您的通訊軟體（Telegram, Discord, Slack 等）。

---

## 常見問題

**Q: 我可以用 Vercel 或 Netlify 嗎？**
A: 不行。Vercel 和 Netlify 是為前端和 Serverless 設計的，不支援 OpenClaw 所需的長連接 WebSocket 服務。

**Q: 如何更新我的機器人？**
A: 在 Render 控制台中，您可以手動觸發拉取最新的代碼。如果是 Codespaces，只需 `git pull` 即可。
