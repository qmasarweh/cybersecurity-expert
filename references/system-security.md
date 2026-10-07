# System, network & cloud security checklist

Use for hosts, servers, endpoints, networks, IAM, and cloud configs.

## Analysis order

1. **Inventory**: hosts, OS, services, open ports, identities, roles, storage, secrets stores.
2. **Exposure**: what is internet-facing vs internal; management planes; debug endpoints.
3. **Identity**: users, service accounts, keys, MFA, password policy, key age, privilege tiers.
4. **Segmentation**: VLANs/VPCs/subnets, security groups/firewalls, east-west paths.
5. **Hardening**: unused services, defaults, patch level, filesystem permissions, kernel/base images.
6. **Data & secrets**: where credentials live, encryption at rest/in transit, backup access.
7. **Observability**: auth logs, process/network telemetry, alerting gaps.

## High-priority classes

| Class | What to look for | Secure direction |
|-------|------------------|------------------|
| Exposed services | SSH/RDP/DB/admin UIs on public nets; anonymous protocols | Private access; VPN/ZTNA; IP allowlists; disable unused |
| Weak auth | Password-only admin; shared accounts; long-lived keys | MFA; SSO; short-lived credentials; rotate |
| Privilege escalation | World-writable paths, sudoers gaps, container escape paths, overly privileged pods | Least privilege; immutable images; drop caps |
| Flat network | Any-to-any security groups; no tiering | Segment by trust; deny-by-default egress/ingress |
| IAM overreach | `*` actions/resources; unused wildcards; admin for apps | Scoped roles; permission boundaries; review |
| Public storage | Open buckets/blobs; world-readable backups | Block public ACLs; encryption; access logging |
| Secret mismanagement | Secrets in env files, images, CI logs, user data | Secret manager; inject at runtime; scan pipelines |
| Patch / supply chain | Known-vuln packages; unpinned images; unsigned artifacts | Patch SLAs; SBOM; pin digests; verify signatures |
| Logging gaps | No auth failure logs; no egress visibility | Centralize logs; alert on anomalies; retain evidence |

## Cloud-specific focus

- Metadata service reachability from workloads (SSRF → credential theft class).
- Overly permissive security groups / NSGs / firewall rules.
- Public load balancers fronting admin or debug paths.
- Cross-account / cross-project trust that is broader than needed.
- CI/CD identities that can deploy to production without review.

## Host-specific focus

- Listening services and bind addresses (`0.0.0.0` vs localhost).
- Local privilege paths (scheduled tasks, services, setuid, weak ACLs).
- Remote management (SSH keys vs passwords, agent forwarding risks).
- Container/K8s: privileged pods, hostPath, cluster-admin bindings.

## Attacker narrative style (conceptual)

Frame paths, e.g.: "Internet-exposed admin panel with weak auth → session on jump host → flat network to DB subnet → backup share with readable credentials → domain/cloud privilege expansion."

Stay conceptual; no exploit steps or payloads.

## Remediation priority

1. Internet-exposed admin / remote access without strong auth
2. Credential and secret exposure
3. Over-privileged identities and privilege escalation paths
4. Flat or over-permissive network paths to crown jewels
5. Missing logging that would hide the above
