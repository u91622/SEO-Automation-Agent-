# Notion SEO & Grammar Automation Agent

這是一個由 Antigravity IDE 製作的 n8n 自動化工作流。它會監控 Notion 資料庫中的新文章，使用 Cerebras AI (`gpt-oss-120b`) 進行 SEO 與文法檢查，並根據分數自動更新狀態與發送 Slack 通知。

## 功能流程
1.  **監控**: 每分鐘檢查 Notion 指定資料庫是否有新頁面。
2.  **分析**: 抓取頁面內容，傳送給 AI 分析文法與 SEO。
3.  **判定**:
    - **合格 (Score >= 80)**: Notion 狀態更新為 `Ready`，Slack 發送慶祝通知 🎉。
    - **不合格 (Score < 80)**: Notion 狀態更新為 `Needs Revision`，在頁面底部自動寫入修改建議，Slack 發送警示通知 ⚠️。

## 前置準備

### 1. Notion Setup
- 建立一個新的 Integration: [Notion My Integrations](https://www.notion.so/my-integrations)
- 在你的 Notion 資料庫頁面，點擊右上角 `...` > `Connections` > `Connect to` > 選擇你的 Integration。
- **資料庫欄位需求**:
    - `Status` (Select 類型): 選項需包含 `Ready` 和 `Needs Revision`。
    - `SEO Score` (Number 類型)。
    - `Name` (Title 類型，預設就有)。

### 2. Slack Setup
- 前往 [Slack API](https://api.slack.com/apps) 建立一個新的 App。
- 進入 **OAuth & Permissions** 設定頁面。
- 在 **Bot Token Scopes** 新增 `chat:write` 權限。
- 點擊 **Install to Workspace** 並複製 **Bot User OAuth Token**。
- **重要**: 記得把你的 Bot 邀請到你想發送通知的 Channel (在 Slack Channel 裡打 `/invite @YourBotName`)。

### 3. Cerebras API
- 確保你有有效的 API Key。

## 安裝步驟
1.  匯入 `workflow.json` 到 n8n。
2.  設定 **Credentials**:
    - **Notion Trigger & Nodes**: 選擇 `Notion API`，填入 Integration Token。
    - **Cerebras AI Analysis**: 選擇 `Header Auth`，填入 `Authorization`: `Bearer csk-...`。
    - **Slack Nodes**: 選擇 `Slack API`，填入 Bot User OAuth Token。
3.  設定 **Node 參數**:
    - **Notion Trigger**: 選擇你要監控的 Database。
    - **Slack Nodes**: 選擇或輸入你的 `Channel ID` (或名稱 如 `#general`, 但 ID 較穩定)。
4.  啟動工作流 (Active)。

## 常見問題
- **Notion 抓不到資料庫?**: 請確認該資料庫頁面有 "Add connection" 給你的 Integration。
- **Slack 發不出去?**: 請確認 Bot 已經被邀請進該 Channel (`/invite @BotName`)，並且 Bot Token 擁有 `chat:write` 權限。

---
*Created using Antigravity IDE*
