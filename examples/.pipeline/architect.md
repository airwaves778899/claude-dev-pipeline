# Architecture Design — User Authentication
_Date: 2026-05-28_
_Stack: TypeScript + Node.js + Express_

## Option A: Minimal Changes

**Core idea**: Add auth logic directly inside existing Express routes using `jsonwebtoken`.

**Pros**: Fewest changes, fastest to implement, no refactoring needed

**Cons**: Auth logic scattered, hard to test, difficult to extend (e.g. adding MFA later)

---

## Option B: Clean Architecture

**Core idea**: Standalone `auth` module with strict layering (Controller → Service → Repository). Token logic abstracted as an interface.

**Pros**: Clear separation of concerns, easy to unit test, extensible

**Cons**: More files upfront, slower initial development

---

## Option C: Pragmatic Balance [Recommended]

**Core idea**: `src/backend/auth/` sub-module with Controller, Service, Repository layers — but no over-abstraction. JWT logic encapsulated in `TokenService`.

**Pros**: Clean structure, testable, consistent with existing codebase style. Avoids over-engineering.

**Cons**: More upfront work than Option A, but far less than Option B

---

## Selected Option: C — Pragmatic Balance

### Tech Stack

| Purpose | Package |
|---------|---------|
| Password hashing | `bcryptjs` (cost 12) |
| JWT sign/verify | `jsonwebtoken` (RS256) |
| Email sending | `nodemailer` + SMTP |
| Input validation | `zod` |

### API Contracts

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/auth/register` | ✗ | Create account |
| POST | `/auth/login` | ✗ | Get access token |
| POST | `/auth/logout` | ✓ | Invalidate token |
| POST | `/auth/forgot-password` | ✗ | Send reset email |
| POST | `/auth/reset-password` | ✗ | Set new password via reset token |

### Database Schema

```sql
CREATE TABLE users (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email       VARCHAR(255) UNIQUE NOT NULL,
  password    VARCHAR(255) NOT NULL,  -- bcrypt hash
  created_at  TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE password_reset_tokens (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id     UUID REFERENCES users(id) ON DELETE CASCADE,
  token_hash  VARCHAR(255) NOT NULL,
  expires_at  TIMESTAMPTZ NOT NULL,
  used        BOOLEAN DEFAULT FALSE
);
```

### Directory Structure

```
src/backend/auth/
├── auth.controller.ts   # Route handlers + zod validation
├── auth.service.ts      # Business logic
├── auth.repository.ts   # DB access
├── token.service.ts     # JWT sign/verify
└── auth.middleware.ts   # requireAuth middleware
```

### Security Design

- Passwords: bcrypt cost 12, never returned or logged
- JWT: RS256, 24h expiry, logout adds token to Redis blocklist
- Login failures: Redis counter, 5 failures → 15-minute lock
- Reset tokens: SHA-256 hashed in DB, 30-minute expiry, single-use
