# legal-docs — Claude Code Skill

A Claude Code skill that generates compliance legal documents for SaaS products and mobile apps, and exports them as PDFs.

## What it generates

| Document | File |
|----------|------|
| Privacy Policy | `legal/privacy-policy.md` |
| Terms of Service | `legal/terms-of-service.md` |
| Cookie Policy | `legal/cookie-policy.md` |
| Data Processing Agreement (DPA) | `legal/data-processing-agreement.md` |
| Disclaimer | `legal/disclaimer.md` |

Supports **PIPL** (China), **GDPR** (EU), and **CCPA** (California). Each document is structured for real legal use — not boilerplate templates.

## Installation

Copy the skill folder into your project:

```bash
mkdir -p your-project/.claude/skills
cp -r legal-docs your-project/.claude/skills/
```

Your project structure should look like this:

```
your-project/
  .claude/
    skills/
      legal-docs/
        SKILL.md
```

## Usage

In any Claude Code session inside your project, type:

```
/legal-docs
```

Claude will ask you a few questions (company name, pricing, data storage region, etc.) and then generate all 5 documents in a `legal/` folder at your project root.

## PDF Export

The skill includes a built-in PDF export workflow using Python + Chrome headless. No extra tools required beyond what most developers already have.

**Requirements:**
- Python 3 + `pip install markdown`
- Google Chrome (macOS or Linux)

After generating the Markdown files, follow the export steps in `SKILL.md` to produce print-ready A4 PDFs.

## What Claude will ask you before writing anything

The skill enforces a confirmation step before generating any document. Claude will ask for:

1. Company / legal entity name
2. Contact email
3. Pricing (plan names, prices, billing cycles)
4. Product name
5. Data storage provider and region

> Pricing is never inferred from code. Claude will always ask you to confirm the numbers first.

## Updating documents later

| Situation | What to update |
|-----------|----------------|
| Pricing change | `terms-of-service.md` → re-export PDF |
| Company details finalized | All 5 files → replace `[Company Name]` / `[Contact Email]` placeholders |
| New data type collected | `privacy-policy.md` + `data-processing-agreement.md` |
| New third-party SDK added | `privacy-policy.md` + `cookie-policy.md` |
| Native app released | `privacy-policy.md` + `cookie-policy.md` |

## License

MIT — free to use, modify, and share.
