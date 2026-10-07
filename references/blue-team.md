# Blue team pairing

Every red-team finding must end with prevention, detection, and response notes.

## Pairing model

For each finding provide:

1. **Prevent** — control that removes or shrinks the vulnerability
2. **Detect** — log/signal that would show abuse attempts or success
3. **Respond** — containment steps if the issue were exploited

## Prevention patterns

| Risk class | Control themes |
|------------|----------------|
| Access control | Deny-by-default authZ, central policy, object-level checks |
| Injection | Parameterization, allowlists, WAF as defense-in-depth only |
| Auth | MFA, short-lived tokens, secure cookie flags, phishing-resistant factors for admins |
| Network | Segmentation, least-privilege egress, private service endpoints |
| Secrets | Manager + rotation + scan CI/repos/images |
| Misconfig | IaC policy-as-code, CIS baselines, remove defaults |
| Supply chain | Pin versions/digests, signed artifacts, dependency review |

## Detection patterns

Suggest concrete signals, not vague "monitor the system":

- Auth: burst failures, impossible travel, new device + privilege use
- AuthZ: repeated 403s then success on object IDs; cross-tenant resource access
- Web: unusual parameterized query errors; SSRF-like outbound to metadata/link-local
- Host: new listening ports, unexpected privileged process trees, scheduled task changes
- Cloud: IAM `*`, public ACL changes, anomalous `AssumeRole`, disabled logging
- Data: bulk export/download spikes; off-hours admin API use

Prefer existing stacks when known (SIEM, EDR, CSPM, cloud audit logs). If unknown, name the log source type.

## Response patterns

Keep high-level and defensive:

- Revoke sessions/keys; rotate secrets
- Isolate host or revoke security-group path
- Disable abused endpoint or feature flag
- Preserve logs; snapshot for forensics if warranted
- Patch/config-fix; add regression test or policy gate

## Hardening quick wins (common)

- Security headers: CSP, HSTS, `X-Content-Type-Options`, frame protections as appropriate
- Cookie flags and token binding to context
- Rate limiting + lockout/backoff on auth endpoints
- Remove debug/admin from production exposure
- Turn on access logs for storage, API gateways, and identity providers
- Enforce MFA for all admin and remote access paths

## Verification after fix

Describe how to confirm the fix safely:

- Unit/integration test that unauthorized access is denied
- Config assertion (cookie flags, CORS, IAM policy)
- Negative test: low-priv user cannot reach high-priv action
- Log test: abuse attempt produces an alertable event

Do not provide exploit payloads to "prove" the bug.
