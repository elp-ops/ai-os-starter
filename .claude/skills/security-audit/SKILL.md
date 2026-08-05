---
name: security-audit
description: Run a comprehensive security audit on a web app codebase before any public deployment. Triggers on "audit this", "security check", "is this safe to deploy", or /security-audit. MUST be run before pushing any app or website public.
---

# Security Audit

Run this before any app or website goes live. No exceptions.

This audit covers the five most common vibe-coded app vulnerabilities, plus CORS, rate limiting, and file upload security.

**The five critical areas (80% of all security issues):**
1. Exposed environment variables and API keys
2. Missing or broken Row Level Security (RLS) on the database
3. No server-side validation (trusting the frontend)
4. Outdated or hallucinated packages
5. Missing authentication middleware

---

## How to Run

Read the full codebase first. Build a mental model of the architecture before making any findings. Then work through every checklist item below. Each item gets one of:

- ✅ PASS: Handled correctly. Cite the file and line.
- ❌ FAIL: Vulnerability exists. Document fully (see format below).
- ⚠️ PARTIAL: Some coverage but gaps remain. Explain what is missing.
- N/A: Not applicable. State why briefly.

Do not skip items. Do not group items together.

---

## Finding Format

For every ❌ FAIL:

```
FINDING #[number]
Severity: CRITICAL / HIGH / MEDIUM / LOW
Category: e.g. Secret Exposure, Missing RLS, etc.
Location: file/path.ts:line_number
CWE: CWE-XXX (Name)

What's wrong: [plain English description]
Why it matters: [what an attacker could actually do]
Vulnerable code: [exact snippet]
The fix: [corrected code, ready to copy/paste]
Effort: ~[X] minutes
```

---

## Audit Checklist

### Section 1: Environment Variables and Secret Management

Search every file including source, config, scripts, and any .env files committed to the repository.

- 1.1 Hardcoded secrets: search for API keys, tokens, passwords, connection strings, webhook URLs embedded directly in source code. Patterns to check: `sk_live_`, `sk_test_`, `Bearer`, `eyJ` (JWT prefix), `ghp_`, `AKIA` (AWS), any 32+ character alphanumeric strings in quotes.
- 1.2 .gitignore coverage: verify `.env`, `.env.local`, `.env.production`, `.env*.local` are all in `.gitignore`. Check git history for previously committed .env files.
- 1.3 Public prefix leaks: check that server-only secrets do NOT use `NEXT_PUBLIC_`, `VITE_`, or `REACT_APP_` prefixes. Keys that must never be public: database service role keys, Stripe secret keys, OpenAI/Anthropic API keys, SMTP credentials, any key granting write/admin access.
- 1.4 Console/error leaks: search for `console.log`, `console.error`, and error components that print environment variables or secrets to the browser.
- 1.5 Build artifact exposure: check if source maps are enabled in production (`productionBrowserSourceMaps`, Vite `sourcemap` config). Source maps let anyone reconstruct original source code.
- 1.6 Startup validation: verify the app fails fast if required environment variables are missing, rather than silently running with undefined values.

### Section 2: Database Security

If using Supabase, Firebase, or any database with client-side access, this section is critical.

- 2.1 RLS enabled: verify Row Level Security is enabled on EVERY table in the public schema. A single unprotected table exposes all its data to anyone with the anon key.
- 2.2 RLS policies exist: a table with RLS enabled but no policies silently returns empty results. Verify every RLS-enabled table has at least SELECT and INSERT policies.
- 2.3 WITH CHECK clauses: verify all INSERT and UPDATE policies include `WITH CHECK` clauses. Without it, a user can insert rows with any `user_id`.
- 2.4 Policy identity source: ensure RLS policies use `auth.uid()`, NOT `auth.jwt()->'user_metadata'`. User metadata can be modified by end users.
- 2.5 Service role key isolation: the `service_role` key bypasses all RLS. Verify it is never used in client-side code, only in server-side admin operations.
- 2.6 Storage bucket policies: if using Supabase Storage, verify buckets have RLS policies. By default, storage buckets are publicly accessible.
- 2.7 SQL injection: check for raw SQL queries using string concatenation or template literals instead of parameterised queries.
- 2.8 SECURITY DEFINER functions: check for database functions marked `SECURITY DEFINER`. These run with superuser privileges. Verify they do not bypass RLS inappropriately.

### Section 3: Authentication and Session Management

- 3.1 Auth middleware exists: verify authentication middleware exists and runs on protected routes. Check the matcher config.
- 3.2 Default-deny routing: check whether middleware protects routes by default (allowlist of public routes) vs. by exception (blocklist of protected routes). Default-deny is significantly safer.
- 3.3 getUser() vs getSession(): for Supabase apps, verify server-side security operations use `supabase.auth.getUser()` (validates JWT against Supabase servers), not `supabase.auth.getSession()` (only reads local JWT without verification).
- 3.4 Auth callback handler: verify the `/auth/callback` route properly exchanges auth codes for sessions, handles errors, and does not expose tokens in URLs or logs.
- 3.5 Session storage: verify session tokens are in `httpOnly` cookies, NOT `localStorage` or `sessionStorage` (accessible to any JavaScript including XSS payloads).
- 3.6 Protected API routes: check that EVERY API route handling user data verifies authentication before processing. AI-added routes often skip this.
- 3.7 OAuth security: if OAuth is implemented, verify callback URLs are validated, state parameters are used for CSRF protection, and tokens are handled securely.
- 3.8 Password reset flows: verify reset tokens expire, are single-use, and are transmitted securely.

### Section 4: Server-Side Validation

- 4.1 Schema validation: verify all API routes and server actions validate input using a schema validation library (Zod, Yup, Valibot, etc.) on the server. Frontend validation is UX, not security.
- 4.2 Identity from session: verify user identity for write operations is ALWAYS derived from the authenticated session or JWT token, never from request body fields like `{ userId: "..." }`.
- 4.3 Input sanitisation: check that user-generated content rendered in HTML is properly sanitised to prevent XSS. Look for `dangerouslySetInnerHTML`, `v-html`, `[innerHTML]`, or unescaped template literals.
- 4.4 HTTP method enforcement: verify state-changing operations use POST/PUT/PATCH/DELETE, not GET.
- 4.5 Error information leaks: verify error responses do not leak stack traces, SQL errors, file paths, or environment variable names to the client.
- 4.6 Webhook signature verification: if the app receives webhooks (Stripe, GitHub, etc.), verify it validates the webhook signature before processing.

### Section 5: Dependency and Package Security

- 5.1 Audit results: run `npm audit` (or `pnpm audit`, `yarn audit`, `bun audit`) and report vulnerabilities by severity.
- 5.2 Hallucinated packages: check for installed packages with suspiciously low download counts, very recent publish dates, or names that do not match well-known packages.
- 5.3 Lockfile committed: verify a lockfile (`package-lock.json`, `pnpm-lock.yaml`, `yarn.lock`, `bun.lockb`) is committed. Without it, installs can pull different (potentially compromised) versions.
- 5.4 Outdated packages: check for outdated packages with known CVEs. Focus on auth libraries, crypto libraries, and framework versions.
- 5.5 Unused dependencies: check for packages in `package.json` that are not imported anywhere. Each unused package is unnecessary attack surface.

### Section 6: Rate Limiting

- 6.1 Expensive operations: identify all API routes that call external paid APIs (OpenAI, Anthropic, Stripe, email/SMS providers) and verify they have rate limiting.
- 6.2 Auth endpoints: verify login, signup, password reset, and OTP endpoints have rate limiting.
- 6.3 Implementation check: if rate limiting exists, verify it is server-side (not frontend debouncing) and uses a reliable backing store (Redis, Upstash, etc.), not in-memory storage that resets on deploy.

### Section 7: CORS Configuration

- 7.1 API route CORS: if the app exposes API routes intended only for its own frontend, verify CORS headers restrict access to the app's own domain. Check for `Access-Control-Allow-Origin: *` on sensitive endpoints.
- 7.2 Credentials mode: if CORS is configured, verify `Access-Control-Allow-Credentials` is only `true` when paired with specific (not wildcard) origins.

### Section 8: File Upload Security

- 8.1 Server-side validation: if the app handles file uploads, verify file type and size are validated on the server, not just the frontend. Check MIME type, not just file extension.
- 8.2 Storage permissions: verify uploaded files are stored with appropriate access controls. Public and private uploads need different policies.
- 8.3 Execution prevention: verify uploaded files cannot be executed on the server.

---

## Final Report Structure

After completing all checklist items, output:

**1. Security Posture Rating**
- CRITICAL: active data exposure or auth bypass. Stop and fix now.
- NEEDS WORK: significant gaps that would be exploitable.
- ACCEPTABLE: minor issues, no immediate data exposure risk.
- STRONG: well-secured with only informational findings.

Include a one-paragraph summary explaining the rating.

**2. Critical and High Findings** (stop everything and fix these)

**3. Quick Wins** (under 10 minutes each, meaningful improvement)

**4. Prioritised Remediation Plan** (all findings ordered by severity then effort)

**5. What Is Already Done Right** (what not to accidentally break)

**6. Checklist Summary** (compact view of every item and its verdict)

---

## Notes

- This audit was sourced from a comprehensive vibe-coded app security guide covering OWASP Top 10 and LLM-specific vulnerability patterns.
- AI security reviews catch common patterns but can miss complex cross-file vulnerabilities or business logic issues. If the stakes are high, do a manual review as well.
- No app is 100% secure. This audit targets the 80% of issues that cause 80% of breaches.
