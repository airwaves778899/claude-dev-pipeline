# PRD — 使用者驗證功能（Email + 密碼登入）
_產出日期：2026-05-28_

## 1. 背景與目標

目前系統沒有任何身份驗證機制，所有使用者都可以存取所有資源。
本次目標是加入基本的 Email + 密碼登入功能，確保只有已驗證的使用者才能使用核心功能。

## 2. 使用者故事（User Stories）

- 作為**訪客**，我想要用 Email 和密碼**註冊帳號**，以便開始使用服務
- 作為**已註冊使用者**，我想要**登入**，以便存取我的個人資料和功能
- 作為**已登入使用者**，我想要**登出**，以便在共用裝置上保護我的帳號
- 作為**忘記密碼的使用者**，我想要透過 Email **重設密碼**，以便重新取得帳號存取權

## 3. 功能需求

### 3.1 Must Have
- 使用者可以用 Email + 密碼完成註冊
- 系統在註冊時驗證 Email 格式，密碼至少 8 字元
- 使用者可以用已註冊的 Email + 密碼登入
- 登入成功後發放 JWT Token（有效期 24 小時）
- 使用者可以登出（Token 失效）

### 3.2 Should Have
- 密碼重設流程（發送重設連結至信箱）
- 登入失敗次數限制（5 次後鎖定 15 分鐘）

### 3.3 Nice to Have
- Google / GitHub OAuth 登入
- 記住我（30 天 Token）

## 4. 非功能需求

- 登入 API 回應時間 < 300ms（P95）
- 密碼以 bcrypt（cost factor ≥ 12）雜湊儲存，絕不明文
- Token 使用 RS256 簽名

## 5. 範圍外（Out of Scope）

- 社群登入（OAuth）
- 多因素驗證（MFA）
- 單一登入（SSO）

## 6. 驗收標準

- [ ] `POST /auth/register` 成功時回傳 201，Body 包含 `userId`
- [ ] `POST /auth/login` 成功時回傳 200，Body 包含 `accessToken`
- [ ] 使用過期或無效 Token 存取受保護 API 時，回傳 401
- [ ] 密碼錯誤 5 次後，第 6 次回傳 429 並附鎖定剩餘時間
- [ ] 密碼在資料庫中以 bcrypt hash 儲存（可驗證）

## 7. 技術限制與整合需求

- 後端：Node.js + TypeScript（現有技術棧）
- 資料庫：PostgreSQL（現有）
- Email 發送：待 Architect Agent 決定（SendGrid 或 SMTP）
