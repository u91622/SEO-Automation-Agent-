# Google Docs SEO & Grammar Automation Agent

這是一個由 Antigravity IDE 製作的 n8n 自動化工作流。它會監控 Google Drive 指定資料夾中的新文件，使用 Cerebras AI (`gpt-oss-120b`) 進行 SEO 與文法檢查，並根據分數自動移動檔案與發送 Telegram 通知。

## 功能流程
1.  **監控**: 每分鐘檢查 `Drafts` 資料夾是否有新建立的 Google Doc。
2.  **分析**: 抓取文件純文字內容，傳送給 AI 分析。
3.  **判定**:
    - **合格 (Score >= 80)**: 將檔案移動至 `Ready` 資料夾，Telegram 發送 慶祝通知 🎉。
    - **不合格 (Score < 80)**: 在文件底部自動寫入修改建議，Telegram 發送 修正通知 ⚠️。

## 前置準備

### 1. Google Drive 資料夾
請在你的 Google Drive 建立以下兩個資料夾：
- `Drafts`: 用來存放草稿（n8n 會監控這裡）。
- `Ready`: 用來存放合格的文章（n8n 會把好的文章搬過來）。

### 2. Telegram Setup
- 透過 [@BotFather](https://t.me/BotFather) 取得 **Token**。
- 取得 **Chat ID**。

### 3. Cerebras API
- 確保你有有效的 API Key。

## 安裝步驟
1.  匯入 `workflow.json` 到 n8n。
2.  設定 **Credentials**:
    - **Google Drive Trigger**: 選擇 `Google Drive OAuth2 API`。
    - **Google Docs**: 選擇 `Google Docs OAuth2 API` (通常跟 Drive 用同一個)。
    - **Cerebras AI Analysis**: 選擇 `Header Auth`，填入 `Authorization`: `Bearer csk-...`。
    - **Telegram Nodes**: 選擇 `Telegram API`，填入 Bot Token。
3.  設定 **Node 參數**:
    - **Google Drive Trigger**: 在 `Folder to Watch` 選擇你的 `Drafts` 資料夾 ID。
    - **Move to Ready**: 在 `Parent ID` 選擇你的 `Ready` 資料夾 ID。
    - **Telegram Nodes**: 填入你的 `Chat ID`。
4.  啟動工作流 (Active)。

## 常見問題
- **Trigger 沒反應?**: Google Drive Trigger 是輪詢機制 (Default: 1 min)，檔案丟進去後可能要等一下才會觸發。
- **文件沒移動?**: 請確認 n8n 的 Google Credential 有足夠的權限 (通常需要 `drive.file` 或 `drive` scope)。

---
*Created using Antigravity IDE*
