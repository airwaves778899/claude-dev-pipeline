# claude-dev-pipeline

> 一個 Claude Code Plugin，協調 **7 個專業 AI Agent**，從需求分析到生產部署的完整開發流水線。每個關鍵節點都有人工確認機制，確保你隨時掌控方向。

[English README](./README.md)

---

## 為什麼需要這個？

開發一個功能遠不只是寫程式碼：

- 確認所有人理解的需求規格
- 寫程式前先做架構決策
- 後端與前端真正契合
- 有測試證明功能可運作
- 程式碼審查抓出安全問題
- 部署設定能真正執行

`claude-dev-pipeline` 將這個工作流程封裝成 Claude Code Plugin。每個 Agent 都是專家，你在每個關卡保持掌控。

---

## 7 個 Agent

| # | Agent | 職責 | 輸出 |
|---|-------|------|------|
| 0 | **Discovery** | 透過對話釐清模糊需求 | 確認後的需求 |
| 1 | **Exploration** | 平行掃描現有程式碼庫 | `.pipeline/exploration.md` |
| 2 | **PM** | 撰寫含 User Story 與驗收標準的結構化 PRD | `.pipeline/pm.md` |
| 3 | **Architect** | 提出 2–3 個架構方案並說明取捨 | `.pipeline/architect.md` |
| 4a | **Backend** | 以 TypeScript 實作 REST API、Service、Repository | `src/backend/` |
| 4b | **Frontend** | 以 TypeScript/React 實作 UI、Hook、API Client | `src/frontend/` |
| 5 | **QA** | 撰寫並執行單元、整合、E2E 測試 | `tests/` |
| 6 | **Reviewer** | 審查安全、Bug、程式碼品質（信心值 ≥ 80 才回報） | `.pipeline/review.md` |
| 7 | **DevOps** | 建立 Dockerfile、docker-compose、GitHub Actions CI/CD | `deploy/` |

Phase 4a（後端）與 4b（前端）**並行執行**。

---

## 安裝

### 前置條件

- 已安裝 [Claude Code](https://claude.ai/code) CLI 或 VS Code Extension

### 安裝 Plugin

```bash
# 1. Clone 這個倉庫
git clone https://github.com/your-username/claude-dev-pipeline.git

# 2. 註冊為本地 Marketplace
claude plugin marketplace add ./claude-dev-pipeline

# 3. 安裝
claude plugin install claude-dev-pipeline

# 4. 確認
claude plugin list
# 應顯示：✓ claude-dev-pipeline  enabled
```

安裝完成後，CLI 與 VS Code Extension 都可直接使用，不需要額外的參數。

---

## 使用方式

### 啟動完整流水線

```
/claude-dev-pipeline:dev-pipeline start "新增使用者驗證功能，支援 Email + 密碼登入"
```

### 單獨執行指定 Agent

```
/claude-dev-pipeline:dev-pipeline run --agent pm
/claude-dev-pipeline:dev-pipeline run --agent architect
/claude-dev-pipeline:dev-pipeline run --agent backend
```

### 從指定 Phase 繼續

```
/claude-dev-pipeline:dev-pipeline run --from qa
```

### 其他指令

```
/claude-dev-pipeline:dev-pipeline status   # 顯示目前進度
/claude-dev-pipeline:dev-pipeline reset    # 清除 .pipeline/ 重新開始
```

---

## 流水線流程

```
使用者需求
    │
[Discovery]  ← 需求模糊時主動追問
    │ 確認後的需求
[Exploration] ← 平行掃描程式碼（2 個子任務）
    │ exploration.md
   [PM] ← 你審閱並確認 PRD
    │ pm.md
[Architect] ← 你從 3 個方案中選 1 個
    │ architect.md
 ┌──┴──┐
[後端] [前端]  ← 並行執行
 └──┬──┘
   [QA]
    │ 測試全綠
[Reviewer] ← Critical 問題 > 3 條時暫停流水線
    │ review.md 無 Critical
 [DevOps]
    │
🎉 完成
```

---

## 中間產物

所有 Agent 都寫入你專案根目錄的 `.pipeline/`：

```
.pipeline/
├── pm.md           # 產品需求文件（PRD）
├── architect.md    # 架構決策 + 詳細設計
├── exploration.md  # 程式碼庫分析
└── review.md       # 程式碼審查報告
```

---

## 貢獻

歡迎 Pull Request！請先開 Issue 討論你想做的變更。

---

## 授權

[MIT](./LICENSE)
