# Deep code scan (mandatory when code is available)

Enter the codebase. Do not conclude from filenames or guesses alone. Trace real entrypoints, guards, and sinks, then cite evidence in the full report.

## When this applies

- Local/workspace repo is open
- User pasted code, a tree, or key modules
- Config, IaC, CI, or dependency manifests are present

If code access exists, refuse a shallow report: keep scanning until entrypoints and high-risk sinks are covered, or document explicit coverage gaps.

## Procedure

### 1. Inventory

Map:

- App entrypoints (HTTP servers, serverless handlers, CLIs, workers)
- Route/controller layers and middleware order
- AuthN / AuthZ modules (sessions, JWT, RBAC/ABAC, tenancy)
- Data access (ORM, raw SQL, caches, object storage)
- Uploads, webhooks, SSRF-capable HTTP clients
- Jobs/queues, admin panels, debug/actuator endpoints
- Infra-as-code, Docker/K8s, CI secrets, `.env*` samples

### 2. Trace trust boundaries

For each external entry:

`request/event → auth middleware → handler → service → DB/filesystem/network`

Ask at every hop:

- Is identity established?
- Is authorization enforced server-side for this object/action?
- Is input still trusted here?
- Can this hop reach metadata, internal nets, or secrets?

### 3. Hunt high-risk sinks

Search and open matching code. Adapt patterns to the stack.

| Class | What to search for (examples) |
|-------|-------------------------------|
| AuthZ gaps | handlers that load by id without ownership/role checks; admin flags from client |
| Injection | string-built SQL, shell, LDAP, template renders with user data |
| XSS | `innerHTML`, `dangerouslySetInnerHTML`, unescaped templates, SVG uploads |
| SSRF | user-controlled URL to `fetch`/`axios`/`request`/`http.get` |
| Path traversal | `path.join` with user segments; zip slip; file download by name |
| Deserialization | `pickle`, `yaml.load`, `unserialize`, permissive JSON revivers |
| Dangerous exec | `eval`, `Function(`, `exec(`, `spawn` with shell + user input |
| Secrets | API keys, tokens, private keys in source, clients, CI logs |
| Weak crypto | MD5/SHA1 passwords, hardcoded IVs, disabled TLS verify |
| Mass assignment | binding full request body into models/roles/isAdmin |
| CORS / cookies | `*` + credentials; missing Secure/HttpOnly/SameSite |

### 4. Follow data flows end-to-end

Before writing a finding:

1. Locate the sink
2. Prove (or refute) that untrusted data can reach it
3. Check whether a guard actually runs on that path
4. Note blast radius (tenant, role, data class)

### 5. Cite evidence

Every code finding needs:

- File path and line (or closest anchor)
- Function / route name
- Short evidence description (what the code does wrong)
- Conceptual abuse path (no payloads)

### 6. Tooling

**In Cursor / agent environments**

Use available tools aggressively:

- List directories; open entry files first
- Search the repo for sink patterns and auth keywords
- Read auth middleware, route tables, and data-access layers fully
- Run safe checks when present (e.g. dependency audit, lint security plugins, `git grep`) — not exploit tooling against third parties

**In other AIs (no repo tools)**

Ask for or reason over:

- Project tree
- Auth + routing modules
- Data-access layer
- Upload/SSRF/admin code
- Env/CI/Docker/IaC snippets

Do not invent file contents; label assumptions.

## Completion gate

Do not finalize the ethical-hacking report until:

- [ ] Entrypoints inventoried
- [ ] AuthN/AuthZ path reviewed
- [ ] High-risk sink classes searched
- [ ] Top findings traced end-to-end
- [ ] Coverage gaps documented in the report
- [ ] Full report template filled (not a short summary)

## Boundaries

- Authorized ethical assessment only
- Conceptual attacker methodology — no exploit PoCs, payloads, or intrusion playbooks
- Prefer safe validation steps that prove a fix works
