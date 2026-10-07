# Cybersecurity Expert — portable skill pack

Strong red-team-heavy expert for **authorized ethical hacking**. Deep-scans application code, configs, APIs, hosts, and cloud — then always delivers a **full ethical-hacking engagement report** with evidence, attack paths, remediation, and detection.

Works in **Cursor**, **local AIs**, and **cloud AIs**.

## What's inside

| File | Use |
|------|-----|
| `SKILL.md` | Cursor Agent Skill (auto-discovered under `~/.cursor/skills/`) |
| `SYSTEM.md` | Paste as system prompt for any other AI |
| `references/code-scan.md` | Mandatory deep code-scan procedure |
| `references/report-template.md` | Full ethical-hacking report (mandatory deliverable) |
| `references/web-security.md` | Web/API checklist |
| `references/system-security.md` | Host/network/cloud checklist |
| `references/red-team.md` | Attacker methodology |
| `references/blue-team.md` | Detection and hardening pairing |

## Install for Cursor

```bash
git clone https://github.com/qmasarweh/cybersecurity-expert.git
```

**Windows (PowerShell):**

```powershell
git clone https://github.com/qmasarweh/cybersecurity-expert.git "$env:USERPROFILE\.cursor\skills\cybersecurity-expert"
```

Or copy an existing clone:

```powershell
Copy-Item -Recurse -Force .\cybersecurity-expert\* "$env:USERPROFILE\.cursor\skills\cybersecurity-expert\"
```

**macOS / Linux:**

```bash
git clone https://github.com/qmasarweh/cybersecurity-expert.git ~/.cursor/skills/cybersecurity-expert
```

### Cursor prompts

- "Use the cybersecurity expert skill. Deep-scan this repo and produce the full ethical-hacking report."
- "Ethically hack this app: enter the code, trace auth and sinks, full report with attack paths."
- "Red-team this API — code-level evidence and remediation roadmap."

## Use with any other AI (local or cloud)

1. Open `SYSTEM.md` and paste it as the **system prompt** / custom instructions.
2. For stronger runs, also paste `references/code-scan.md` and `references/report-template.md`.
3. Give the model your project tree, auth modules, and sensitive handlers (or the whole repo if the tool allows).
4. Ask for the **full ethical-hacking report**.

Works with Ollama, LM Studio, Open WebUI, Claude Projects, ChatGPT custom instructions, etc.

## What you get

Every engagement should end with a full report including:

- Executive summary + overall risk rating
- Scope, methodology, attack surface
- Threat model / kill-chain
- Findings table + detailed evidence (`file:line` when possible)
- Chained attack-path narratives
- Code coverage notes
- Remediation roadmap + retest checklist

## Boundaries

- Authorized ethical assessment only.
- High-level attacker methodology — no exploit PoCs, payloads, or unauthorized intrusion playbooks.
- Always includes remediation and detection notes.
