# Red team methodology (authorized)

Use an attacker mindset to prioritize and narrate risk. Stay conceptual — no exploit PoCs or unauthorized intrusion procedures.

## Mindset

- Assume a motivated adversary with time and tooling.
- Prefer **chains** over isolated findings.
- Optimize for impact on confidentiality, integrity, availability, and business objectives.
- Distinguish unauthenticated external paths from post-auth / insider paths.

## Kill-chain style framing

Map findings into stages (skip stages that do not apply):

1. **Recon / discovery** — what an attacker can learn without privileges
2. **Initial access** — first foothold class (weak auth, exposed service, injectable input, leaked secret)
3. **Execution / foothold** — how control is maintained at a high level
4. **Privilege escalation** — local admin, role elevation, tenancy escape
5. **Lateral movement** — trust abuse, credential reuse, flat network
6. **Objective** — data theft, fraud, persistence, sabotage, ransomware prep

For each finding, note which stage it enables and what it unlocks next.

## Prioritization

Score qualitatively:

| Factor | Higher severity when… |
|--------|------------------------|
| Exploitability | Unauthenticated, reliable, common class, low skill |
| Impact | Auth bypass, data breach, RCE-class, full tenancy compromise |
| Blast radius | Affects many users/tenants/environments |
| Detectability | Silent / no logs / looks like normal traffic |
| Chaining | Combines with other findings into a path |

Default rank order:

1. Unauthenticated critical impact
2. Authenticated low-priv → high-priv / cross-tenant
3. Post-foothold lateral / persistence enablers
4. Defense-evasion / logging blind spots
5. Hardening nits without clear abuse path

## Narrative rules

For every finding, write a short **attacker methodology** paragraph:

- Preconditions (who, from where, what access)
- Abuse pattern (class of action, not a payload)
- Resulting capability
- Likely next move in the chain

Example tone:

> An unauthenticated caller who can hit `/api/orders/{id}` and guess or enumerate IDs can read other customers' orders because ownership is not enforced server-side. Combined with an admin export endpoint that only checks a client-supplied role claim, this becomes a path to bulk PII extraction.

## Engagement hygiene (authorized)

- Scope: only assets the user is allowed to assess.
- Prefer static/config/code review and safe validation over destructive tests.
- Call out risky live tests the user would need a formal pentest engagement for; do not run or detail exploit procedures.
- Document assumptions and evidence gaps.

## Anti-patterns

- Laundry lists of CVE names without tying to the target's stack
- Severity inflation for theoretical issues with no path
- Payload dumps or "copy-paste this exploit"
- Ignoring business context (what actually matters if broken)
