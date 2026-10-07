# Web & API security checklist

Use for websites, SPAs, backends, and HTTP APIs.

## Analysis order

1. **Asset inventory**: origins, routes, APIs, auth flows, admin surfaces, file upload, webhooks, third parties.
2. **Trust boundaries**: browser ↔ app ↔ API ↔ DB ↔ internal services ↔ cloud metadata.
3. **AuthN / AuthZ**: session, JWT, OAuth/OIDC, MFA, password reset, role checks on every sensitive action.
4. **Input/output handling**: injection sinks, XSS contexts, SSRF-capable fetchers, deserialization, path traversal.
5. **Config & transport**: TLS, cookies, CORS, CSP, HSTS, cache, error verbosity, secrets in clients.
6. **Business logic**: IDOR, mass assignment, race conditions, coupon/payment abuse, workflow skips.

## High-priority classes (OWASP-oriented)

| Class | What to look for | Secure direction |
|-------|------------------|------------------|
| Broken access control | Missing object/function checks; predictable IDs; admin by obscurity | Deny-by-default; server-side authZ on every action |
| Injection | SQL/NoSQL/OS/LDAP/template sinks with untrusted input | Parameterized queries; allowlists; least privilege DB |
| XSS | Unescaped HTML/JS contexts; dangerous sinks (`innerHTML`, `eval`) | Context-aware encoding; CSP; avoid unsafe sinks |
| Auth / session | Weak reset, long-lived tokens, missing rotation, insecure cookies | Short-lived tokens; HttpOnly/Secure/SameSite; MFA for sensitive ops |
| SSRF | User-controlled URLs to internal hosts/metadata | Allowlist destinations; block link-local/metadata ranges |
| CSRF | Cookie auth + state-changing GET/missing tokens | SameSite; anti-CSRF tokens; prefer Authorization headers |
| Misconfiguration | Default creds, verbose errors, open CORS `*`, missing headers | Harden defaults; security headers; least exposure |
| Sensitive data | Secrets in JS bundles, PII in logs/URLs, weak crypto | Server-side secrets; minimize data; modern crypto |
| API flaws | BOLA/BFLA, mass assignment, excessive data, rate-limit gaps | Per-object authZ; DTO allowlists; pagination + rate limits |
| Upload / file | Unrestricted type/path; executable storage; XSS via SVG | Type allowlist; random names; separate domain; scan |

## Quick evidence checks (authorized)

- Trace each privileged action to a server-side authorization check.
- Confirm cookies: `Secure`, `HttpOnly`, appropriate `SameSite`, tight `Path`/`Domain`.
- Review CORS: no `*` with credentials; explicit origins.
- Search client bundles and repos for keys, tokens, internal URLs.
- Review error paths for stack traces / schema leakage.
- Map every outbound fetch that accepts user-controlled URLs.

## Attacker narrative style (conceptual)

Frame chains, e.g.: "Public endpoint leaks object IDs → missing ownership check → read/modify another tenant's record → escalate to admin function if role check is client-only."

Do not supply exploit payloads. Describe the class of input and the sink.

## Remediation priority

1. AuthZ failures and auth bypasses
2. Injection / RCE-class sinks
3. SSRF to cloud metadata / internal networks
4. Session fixation / token theft paths
5. XSS on sensitive origins
6. Misconfig and information disclosure
