# PRD — User Authentication (Email + Password Login)
_Date: 2026-05-28_
_Stack: {{TECH_STACK}}_

## 1. Background & Goals

The system currently has no authentication. All users can access all resources.
Goal: add basic email + password login so only verified users can access core features.

## 2. User Stories

- As a **visitor**, I want to **register** with email and password, so I can start using the service
- As a **registered user**, I want to **log in**, so I can access my data and features
- As a **logged-in user**, I want to **log out**, so I can protect my account on shared devices
- As a user who **forgot my password**, I want to **reset it via email**, so I can regain access

## 3. Functional Requirements

### 3.1 Must Have
- Register with email + password (email format validated, password ≥ 8 chars)
- Log in and receive a JWT token (24-hour expiry)
- Log out (token invalidated)

### 3.2 Should Have
- Password reset flow (send reset link to email)
- Login failure rate limiting (lock after 5 attempts for 15 minutes)

### 3.3 Nice to Have
- Google / GitHub OAuth
- Remember me (30-day token)

## 4. Non-Functional Requirements

- Login API response time < 300ms (P95)
- Passwords stored as bcrypt hash (cost ≥ 12) — never plaintext
- Tokens signed with RS256

## 5. Out of Scope

- OAuth / social login
- Multi-factor authentication (MFA)
- Single Sign-On (SSO)

## 6. Acceptance Criteria

- [ ] `POST /auth/register` returns 201 with `userId` on success
- [ ] `POST /auth/login` returns 200 with `accessToken` on success
- [ ] Accessing a protected endpoint with expired/invalid token returns 401
- [ ] After 5 failed login attempts, 6th attempt returns 429 with lock duration
- [ ] Passwords stored as bcrypt hash (verifiable in DB)

## 7. Technical Constraints

- Backend: {{TECH_STACK}} (existing stack)
- Database: PostgreSQL (existing)
- Email: to be decided by Architect Agent
