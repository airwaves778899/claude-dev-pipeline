# 架構設計 — 使用者驗證功能
_產出日期：2026-05-28_

## 方案 A：最小變更（Minimal Changes）

**核心思路**：在現有 Express 路由中直接加入驗證邏輯，使用 `jsonwebtoken` 套件。

**優點**：改動最少，最快實作，不需要重構現有程式碼

**缺點**：驗證邏輯散落各處，難以測試，日後不易擴充（如加入 MFA）

---

## 方案 B：乾淨架構（Clean Architecture）

**核心思路**：建立獨立的 `auth` 模組，嚴格分層（Controller → Service → Repository）。Token 刷新邏輯抽象為介面，便於日後替換實作。

**優點**：職責清晰，單元測試容易，支援未來擴充

**缺點**：需要建立更多檔案，短期開發速度較慢

---

## 方案 C：務實平衡（Pragmatic Balance）【推薦】

**核心思路**：建立 `src/backend/auth/` 子模組，包含 Controller、Service、Repository 三層，但不過度抽象。JWT 邏輯封裝在 `TokenService` 中。

**優點**：結構清晰、可測試，同時避免過度工程化；與現有程式碼風格一致

**缺點**：比方案 A 多一些前置工作，但遠少於方案 B

---

## 選定方案：C — 務實平衡

### 技術選型

| 用途 | 套件 |
|------|------|
| 密碼雜湊 | `bcryptjs`（cost 12） |
| JWT 簽發 / 驗證 | `jsonwebtoken`（RS256） |
| Email 發送 | `nodemailer` + SMTP |
| 輸入驗證 | `zod` |

### API 合約

| Method | Path | Auth | 說明 |
|--------|------|------|------|
| POST | `/auth/register` | ✗ | 註冊新帳號 |
| POST | `/auth/login` | ✗ | 登入取得 Token |
| POST | `/auth/logout` | ✓ | 登出（Token 加入黑名單） |
| POST | `/auth/forgot-password` | ✗ | 發送密碼重設信 |
| POST | `/auth/reset-password` | ✗ | 以重設 Token 更新密碼 |

### 資料庫 Schema

```sql
CREATE TABLE users (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email       VARCHAR(255) UNIQUE NOT NULL,
  password    VARCHAR(255) NOT NULL,  -- bcrypt hash
  created_at  TIMESTAMPTZ DEFAULT NOW(),
  updated_at  TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE password_reset_tokens (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id     UUID REFERENCES users(id) ON DELETE CASCADE,
  token_hash  VARCHAR(255) NOT NULL,
  expires_at  TIMESTAMPTZ NOT NULL,
  used        BOOLEAN DEFAULT FALSE
);
```

### 目錄結構

```
src/backend/auth/
├── auth.controller.ts   # 路由處理與輸入驗證（zod）
├── auth.service.ts      # 業務邏輯
├── auth.repository.ts   # 資料存取
├── token.service.ts     # JWT 簽發 / 驗證
└── auth.middleware.ts   # requireAuth middleware
```

### 安全設計

- 密碼：bcrypt cost 12，絕不回傳或 log
- JWT：RS256，24h 有效期，登出後加入 Redis 黑名單
- 登入失敗：Redis 計數器，5 次失敗鎖定 15 分鐘
- 重設 Token：SHA-256 雜湊後存 DB，30 分鐘有效，單次使用

### 部署架構

現有單體 Express 服務，不需要額外部署元件。Redis 用於 Token 黑名單與登入失敗計數。
