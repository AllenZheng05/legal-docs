# legal-docs — Claude Code Skill

A Claude Code skill that guides you through writing and exporting professional legal documents for SaaS products and mobile apps — privacy policy, terms of service, cookie policy, data processing agreement, and disclaimer — all exported as print-ready PDFs.

---

## What it does

When you invoke `/legal-docs`, Claude will:

1. **Ask you** for the required information (company name, contact email, pricing, product name, data storage provider) before writing anything
2. **Write** all 5 legal documents in Markdown, tailored to your product
3. **Export** each document to a styled A4 PDF using Chrome headless — no extra tools required

---

## Documents generated

| File | Purpose |
|------|---------|
| `Privacy Policy` | Data collection, usage, retention, and user rights (PIPL / GDPR aligned) |
| `Terms of Service` | B2B or B2C service agreement with pricing, SLA, and refund policy |
| `Cookie Policy` | Cookie types with actual storage key names, third-party disclosures |
| `Data Processing Agreement` | Defines controller/processor responsibilities for sensitive data handling |
| `Disclaimer` | Health data, system availability, and user operation liability limits |

---

## Requirements

- **Claude Code** (this is a Claude Code skill, not a standalone script)
- **Python 3** with `markdown` library: `pip3 install markdown`
- **Google Chrome** (macOS: `/Applications/Google Chrome.app`, Linux: `google-chrome`)

---

## Installation

Copy `SKILL.md` into your project at:

```
your-project/.claude/skills/legal-docs/SKILL.md
```

Then in Claude Code, run:

```
/legal-docs
```

---

## What Claude will ask before writing

- Company / legal entity name
- Contact email (for user requests and complaints)
- Pricing tiers and billing cycles (monthly / annual / multi-year)
- Product name and supported platforms
- Data storage provider and region (e.g. AWS us-east-1, Tencent Cloud China)
- Any third-party SDKs that collect data (analytics, crash reporting, ads)

> Claude will **never** fill in pricing or company details on its own — it always asks first.

---

## PDF export

Documents are exported using Chrome's built-in headless PDF renderer. No Pandoc, LaTeX, or paid services needed.

The PDF style uses a clean two-color theme (teal `#0D9488` + dark blue `#0369A1`). To change the color scheme, update the two hex values in the CSS block inside `SKILL.md`.

---

## Updating a document later

Just edit the relevant `.md` file and ask Claude to re-export it:

> "Re-export the Terms of Service as PDF"

Claude will convert only that file without touching the others.

---

## Notes

- Built and tested for **China market compliance** (PIPL, Cybersecurity Law, Data Security Law), but the structure maps well to GDPR contexts too
- The Data Processing Agreement follows **PIPL Article 21** (controller / processor distinction)
- All documents include `【placeholder】` tags for fields that must be confirmed before finalizing
