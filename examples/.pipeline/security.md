# Security Audit Report — User Authentication
_Date: 2026-05-28_
Overall Risk: MEDIUM

## 🔴 Critical (0 issues)

None found.

## 🟡 High (1 issue)

### [S-001] JWT secret falls back to hardcoded string
- OWASP Category: A02 — Cryptographic Failures
- Location: `src/backend/auth/token.service.ts:12`
- Attack vector: If `JWT_SECRET` env var is not set, the service uses `"dev-secret"` as fallback. In production this would allow any attacker knowing the default to forge tokens.
- Fix: Remove the fallback entirely. Throw an error at startup if `JWT_SECRET` is not set.

## 🟢 Medium (logged to tech-debt-tracker.md)

### [S-002] No rate limiting on `/auth/register`
- OWASP Category: A04 — Insecure Design
- Location: `src/backend/auth/auth.controller.ts:8`
- Risk: Allows unlimited account creation (spam/enumeration)
- Logged to `docs/tech-debt-tracker.md` with priority P2
