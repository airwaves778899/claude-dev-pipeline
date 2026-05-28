# claude-dev-pipeline

> 一個 Claude Code Plugin，協調 **9 個專業 AI Agent**，從需求分析到生產部署的完整開發流水線。每個關鍵節點都有人工確認機制，確保你隨時掌控方向。

[English README](./README.md)

---

## 為什麼需要這個？

開發一個功能遠不只是寫程式碼：確認所有人理解的需求規格、寫程式前先做架構決策、後端與前端真正契合、有測試證明功能可運作、安全審查抓出漏洞、程式碼審查抓出品質問題、部署設定能真正執行。

`claude-dev-pipeline` 將這個工作流程封裝成 Claude Code Plugin。每個 Agent 都是專家，你在每個關卡保持掌控。

---

## 9 個 Agent

| # | Agent | 職責 | 輸出 |
|---|-------|------|------|
| 0 | **Discovery** | 透過對話釐清模糊需求 | 確認後的需求 |
| 1 | **Exploration** | 平行掃描現有程式碼庫 | `.pipeline/exploration.md` |
| 2 | **PM** | 撰寫含 User Story 與驗收標準的結構化 PRD | `.pipeline/pm.md` |
| 3 | **Architect** | 提出 2–3 個架構方案並說明取捨 | `.pipeline/architect.md` |
| 4a | **Backend** | 依技術棧實作 API、Service、Repository | `src/backend/` |
| 4b | **Frontend** | 依技術棧實作 UI、Hook、API Client | `src/frontend/` |
| 5 | **QA** | 撰寫並執行單元、整合、E2E 測試 | `tests/` |
| 6 | **Security** | OWASP Top 10 安全掃描（信心值 ≥ 80） | `.pipeline/security.md` |
| 7 | **Reviewer** | 程式碼品質審查（信心值 ≥ 80） | `.pipeline/review.md` |
| 8 | **DevOps** | 建立部署設定，自動開 PR | `deploy/` |

Phase 4a（後端）與 4b（前端）**並行執行**。

---

## 安裝

### 前置條件

- 已安裝 [Claude Code](https://claude.ai/code) CLI 或 VS Code Extension

### 安裝 Plugin

```bash
# 1. Clone 這個倉庫
git clone https://github.com/airwaves778899/claude-dev-pipeline.git

# 2. 註冊為本地 Marketplace
claude plugin marketplace add ./claude-dev-pipeline

# 3. 安裝
claude plugin install claude-dev-pipeline

# 4. 確認
claude plugin list
# 應顯示：✓ claude-dev-pipeline  enabled
```

---

## 使用方式

### 啟動完整流水線

```
/claude-dev-pipeline:dev-pipeline start "新增使用者驗證功能，支援 Email + 密碼登入"
```

### 指定技術棧

```
/claude-dev-pipeline:dev-pipeline start "新增支付功能" --stack python
/claude-dev-pipeline:dev-pipeline start "建立行動端登入頁" --stack flutter
```

### 修復指定 bug

```
/claude-dev-pipeline:dev-pipeline fix "登入後 token 沒有刷新"
```

### 只跑程式碼審查（用在現有專案）

```
/claude-dev-pipeline:dev-pipeline review-only
```

### 其他指令

```
/claude-dev-pipeline:dev-pipeline run --agent pm        # 單獨執行指定 Agent
/claude-dev-pipeline:dev-pipeline run --from qa         # 從指定 Phase 繼續
/claude-dev-pipeline:dev-pipeline status                # 顯示目前進度
/claude-dev-pipeline:dev-pipeline reset                 # 清除 .pipeline/ 重新開始
```

---

## 技術棧 Profiles

| Profile | 技術棧 |
|---------|--------|
| `ts-node`（預設） | TypeScript + Node.js + Express |
| `ts-react` | TypeScript + React + Vite |
| `python` | Python + FastAPI |
| `go` | Go + gin |
| `flutter` | Flutter + Dart |

詳細說明與推薦套件請見 [`stack-profiles/`](./stack-profiles/)。

---

## Git 自動 Commit

若你的專案已有 git repo，每個 Phase 審核通過後會自動 commit：

| Phase | Commit 訊息 |
|-------|-------------|
| PM | `pipeline: PM — add PRD for <feature>` |
| Architect | `pipeline: Architect — add architecture for <feature>` |
| 實作 | `pipeline: Implement <feature> (backend + frontend)` |
| QA | `pipeline: QA — add test suite for <feature>` |
| Security | `pipeline: Security — security scan passed` |
| Reviewer | `pipeline: Reviewer — code review passed` |
| DevOps | `pipeline: DevOps — add deployment config` |

DevOps 完成後若偵測到 `gh` CLI 可用，會自動執行 `gh pr create`。

---

## 知識庫（docs/）

Architect Agent 在你的專案中自動建立 `docs/` 目錄，跨 session 持續累積：

```
docs/
├── design-docs/
│   └── index.md              # 架構決策記錄
├── tech-debt-tracker.md      # 已追蹤的技術債（可見，不藏）
└── QUALITY_SCORE.md          # QA 各層覆蓋率結果
```

模板檔案請見 [`templates/docs/`](./templates/docs/)。

---

## 流水線流程

```
使用者需求
    │
[Discovery]  ← 需求模糊時主動追問
    │
[Exploration] ← 平行掃描程式碼（2 個子任務）
    │
   [PM] ← 你審閱並確認 PRD ✅
    │
[Architect] ← 你從 3 個方案中選 1 個 ✅
    │
 ┌──┴──┐
[後端] [前端]  ← 並行執行
 └──┬──┘
   [QA]  ← 測試全綠
    │
[Security] ← OWASP 掃描
    │
[Reviewer] ← Critical > 3 時暫停 ✅
    │
 [DevOps] ← 自動開 PR
    │
🎉 完成
```

---

## 貢獻

歡迎 Pull Request！請先開 Issue 討論你想做的變更。詳見 [CONTRIBUTING.md](./CONTRIBUTING.md)。

---

## 授權

[MIT](./LICENSE)
