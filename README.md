# CyberSec Reporting Engine

> Generate professional cybersecurity reports from structured findings.

---

## What It Does

CyberSec Reporting Engine is a CLI tool that transforms raw security findings (YAML or JSON) into polished reports in multiple formats.

**Supported outputs:**
- **Markdown** — Version-control friendly, editable
- **HTML** — Interactive, browser-ready
- **PDF** — Print-ready via Puppeteer
- **DOCX** — Microsoft Word via Python

**Supported assessment types:**
Penetration Testing, Red Team, Vulnerability Assessment, Cloud Security, Active Directory, Digital Forensics, Incident Response, Threat Hunting, Compliance, Hardening.

---

## Quick Start

### Prerequisites

- Node.js 18+
- Python 3.10+ (for DOCX output only)

### Install

```bash
npm install
# Optional: for DOCX generation
pip install -r requirements.txt
```

### Generate Your First Report

```bash
# Initialize a new project
node cli.js init -d ./my-project

# Edit ./my-project/findings/sample-findings.yaml with your data

# Generate report
node cli.js generate -i ./my-project/findings/sample-findings.yaml -o ./my-project/output/
```

### CLI Commands

```bash
node cli.js generate -i <file>          # Generate report from findings
node cli.js validate -i <file>          # Validate findings against schema
node cli.js skills list                 # List all available skills
node cli.js skills info <name>          # Show metadata for a skill
node cli.js theme list                  # List available themes
node cli.js template list               # List available templates
node cli.js init -d <dir>               # Initialize a new project
node cli.js serve -p 3000               # Start local report viewer
```

---

## Project Structure

```
cybersec-reporting-engine/
├── cli.js                          # CLI entry point
├── package.json                    # Node.js dependencies
├── requirements.txt                # Python dependencies (DOCX)
├── .cybersecrc.yaml                # Default configuration
│
├── AGENTS.md                       # Universal AI agent entry point
├── SKILLS_INDEX.yaml               # Machine-readable skill catalog
├── SKILLS_INDEX.md                 # Human-readable skill table
│
├── skills/                         # 11 AI agent skill definitions
│   ├── universal-report-engine/
│   ├── pentest-report/
│   ├── redteam-report/
│   ├── vulnerability-assessment-report/
│   ├── cloud-security-report/
│   ├── ad-assessment-report/
│   ├── hardening-report/
│   ├── forensics-report/
│   ├── incident-response-report/
│   ├── threat-hunting-report/
│   └── compliance-report/
│       └── Each contains: SKILL.md, skill.yaml
│
├── scripts/
│   └── sync-skills.js              # Auto-generates all skill indices
│
├── templates/
│   └── finding-schema/             # Canonical finding schema
│       └── finding-schema.yaml     # Validation schema for findings
│
├── outputs/                        # Output generators
│   ├── markdown/                   # Markdown generator + template engine
│   ├── html/                       # HTML generator (marked + jsdom)
│   ├── pdf/                        # PDF generator (Puppeteer)
│   └── docx/                       # DOCX generator (python-docx)
│
├── examples/                       # Sample reports (markdown)
│   ├── pentest/
│   ├── redteam/
│   ├── cloud/
│   ├── ad/
│   ├── ir/
│   ├── forensics/
│   └── compliance/
│
├── knowledge/                      # Framework and methodology references
│   ├── frameworks/                 # CVSS, CWE, MITRE ATT&CK, NIST, OWASP, etc.
│   └── methodologies/              # Offensive, defensive, DFIR, GRC
│
└── docs/                           # Documentation
    ├── user-guide/                 # User documentation
    └── agents/                     # AI agent integration guides
```

---

## Findings Schema

All findings follow a standardized YAML schema defined in `templates/finding-schema/finding-schema.yaml`.

**Key fields:**

```yaml
metadata:
  title: "Assessment Report"
  author: "Consultant Name"
  classification: "CONFIDENTIAL"

sections:
  - id: "executive-summary"
    title: "Executive Summary"
    content: "..."

  - id: "findings"
    title: "Technical Findings"
    findings:
      - id: "FIND-001"
        title: "SQL Injection"
        severity: "critical"
        cvss_v4:
          base_score: 9.8
          vector: "CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H"
        description: "..."
        remediation: "..."
```

Run `node cli.js validate -i findings.yaml` to validate against the schema.

---

## Skill Auto-Discovery for AI Agents

This repository implements a universal skill auto-discovery protocol so AI agents (Claude Code, OpenCode, Cursor, etc.) can discover available capabilities without manual configuration.

**Discovery entry points:**
- **`AGENTS.md`** — Universal instructions for any AI agent entering this repository
- **`SKILLS_INDEX.yaml`** — Machine-readable canonical list of all 11 skills
- **`SKILLS_INDEX.md`** — Human-readable skill table

Each skill in `skills/` includes:
- **`SKILL.md`** — Full system prompt and workflow guide
- **`skill.yaml`** — Structured metadata, validation rules, and schemas

**Adding or modifying a skill?** Regenerate all index files:

```bash
npm run sync:skills
```

This updates:
- `SKILLS_INDEX.yaml`
- `SKILLS_INDEX.md`
- `.agents/skills/skills-index.yaml`
- `.claude/skills/skills-index.yaml`
- `.opencode/skills/skills-index.yaml`

---

## License

Apache 2.0 — see [LICENSE](LICENSE) for details.
