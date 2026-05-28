# Code Review Report — User Authentication
_Date: 2026-05-28_
Overall Grade: B

## 🔴 Critical (0 issues)

None found.

## 🟡 Important (1 issue)

### [C-001] Missing await on async repository call
- Location: `src/backend/auth/auth.service.ts:34`
- Problem: `this.repo.findByEmail(email)` is called without `await`. The function returns a Promise that is compared directly to `null`, always evaluating as truthy.
- Fix: Add `await`: `const user = await this.repo.findByEmail(email)`
- Confidence: 98

## 🟢 Minor (2 issues)

### [M-001] Error message reveals whether email exists
- Location: `src/backend/auth/auth.controller.ts:52`
- Current: `"No account found for this email"`
- Better: `"Invalid email or password"` (prevents email enumeration)
- Logged to tech-debt-tracker.md

### [M-002] `TokenService` not injected via interface
- Location: `src/backend/auth/auth.service.ts:10`
- Hard-coded dependency makes unit testing harder — should accept interface
- Logged to tech-debt-tracker.md
