# AI Agent Context — CyberSec Reporting Engine

> **Auto-Discovery Protocol:** When operating in this repository, read `SKILLS_INDEX.yaml`
> (or `SKILLS_INDEX.md`) on startup to discover all available skills. Do not wait for the
> user to tell you which skills exist.

## Quick Start for Agents

1. **Discover skills**: Read `SKILLS_INDEX.yaml` to see all 11 available skills.
2. **Load a skill**: Read `skills/<name>/SKILL.md` to get the full system prompt and schema.
3. **Generate reports**: Use the CLI (`node cli.js generate`) or call the orchestrator programmatically.

## Repository Layout

```
skills/                         — Canonical skill definitions (11 skills)
  <skill-name>/
    SKILL.md                    — Full system prompt and usage instructions
    skill.yaml                  — Structured metadata, schemas, validation rules

outputs/                        — Report generators
  markdown/                     — GFM + Mermaid output
  html/                         — Themed responsive HTML
  pdf/                          — Print-ready PDF via Puppeteer
  docx/                         — Microsoft Word output

templates/
  finding-schema/               — Canonical YAML schema for security findings

cli.js                          — CLI entry point
checks/                         — Validation rule implementations
scripts/
  sync-skills.js                — Re-generates all index files after skill changes
```

## Available Skills (Auto-Discovered)

See `SKILLS_INDEX.yaml` for the canonical list. As of last sync:

- **pentest-report** — PTES-aligned penetration testing reports
- **redteam-report** — Adversary emulation & purple team reports
- **vulnerability-assessment-report** — Risk-scored vulnerability reports
- **cloud-security-report** — AWS/Azure/GCP security assessment reports
- **ad-assessment-report** — Active Directory security assessment reports
- **hardening-report** — CIS benchmark hardening assessment reports
- **forensics-report** — DFIR investigation reports
- **incident-response-report** — NIST/SANS-aligned incident response reports
- **threat-hunting-report** — Hypothesis-driven threat hunting reports
- **compliance-report** — ISO 27001 / NIST CSF / PCI DSS compliance reports
- **universal-report-engine** — Generic structured report engine

## CLI Reference

```bash
# List discovered skills
node cli.js skills list

# Show skill metadata
node cli.js skills info pentest-report

# Generate a report from findings YAML
node cli.js generate -i findings/my-assessment.yaml -f md,html,pdf

# Validate findings against schema
node cli.js validate -i findings/my-assessment.yaml

# Start local report viewer
node cli.js serve -p 3000 -d ./output
```

## Agent Integration Conventions

- **Skills are self-describing.** Every `SKILL.md` contains the complete prompt needed to invoke that skill. When a user asks for a "penetration test report," load `skills/pentest-report/SKILL.md` and follow its instructions.
- **Schemas are canonical.** `templates/finding-schema/finding-schema.yaml` defines the universal finding structure used across all skills.
- **Indexes are auto-generated.** Run `node scripts/sync-skills.js` after adding or modifying any skill. This updates `SKILLS_INDEX.yaml` and all per-agent indices.
- **Multi-agent support.** The repository maintains copies in `.agents/skills/`, `.claude/skills/`, and `.opencode/skills/` so that agent-specific discovery mechanisms can locate skills.

## When to Use Which Skill

| User Request | Skill |
|--------------|-------|
| "Generate a pentest report" | `skills/pentest-report/SKILL.md` |
| "Red team engagement report" | `skills/redteam-report/SKILL.md` |
| "Vulnerability scan results" | `skills/vulnerability-assessment-report/SKILL.md` |
| "Cloud security assessment" | `skills/cloud-security-report/SKILL.md` |
| "Active Directory audit" | `skills/ad-assessment-report/SKILL.md` |
| "CIS hardening report" | `skills/hardening-report/SKILL.md` |
| "Incident response report" | `skills/incident-response-report/SKILL.md` |
| "Digital forensics report" | `skills/forensics-report/SKILL.md` |
| "Threat hunting report" | `skills/threat-hunting-report/SKILL.md` |
| "Compliance audit report" | `skills/compliance-report/SKILL.md` |

## Quality Gates

All reports must pass validation rules defined in each skill's `skill.yaml`. Common gates:
- Evidence required for Critical/High findings
- CVSS v4.0 vectors for all risk-scored findings
- Concrete reproduction steps (no generic "send malicious payload")
- Business-impact language in executive summaries (no raw CVE names)

## Need Help?

- Read `README.md` for human-facing documentation.
- Run `node cli.js --help` for CLI options.
- Read `SKILLS_INDEX.md` for a human-readable skill table.
