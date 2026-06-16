---
name: legal-docs
description: Generate privacy policy, terms of service, cookie policy, data processing agreement, and disclaimer for SaaS or mobile app projects, and export them as PDFs.
---

# Legal Docs Skill

Use this skill when you need to generate compliance legal documents for a SaaS product or mobile app and export them as deliverable PDF files.

---

## Step 1 — Collect information from the user

Before writing anything, ask the user to confirm the following. Do not infer or pull values from code.

**Required:**
1. **Company / legal entity name** (used in document signatures)
2. **Contact email** (for user complaints and data rights requests)
3. **Pricing** (plan names, prices, billing cycles — monthly / annual / multi-year)
4. **Product name** (app or system name)
5. **Data storage provider and region** (e.g. AWS us-east-1, Tencent Cloud Shanghai)

**Optional (improves accuracy):**
- Any cross-border data transfers?
- Any third-party analytics or advertising SDKs? (e.g. Firebase, Google Analytics)
- Company address (for terms of service signature block)
- Document effective date (defaults to today)

---

## Step 2 — Generate the Markdown documents

Create a `legal/` folder in the project root and generate the following 5 documents in order.

### Document 1: Privacy Policy

Section structure (in order):
1. Introduction — product name, supported platforms (web / iOS / Android)
2. Information we collect — categorized list (identity, credentials, health/sensitive data, logs)
3. How we use information — table mapping purpose to data type
4. Sensitive personal information — masking rules, extra protection measures
5. Data storage — storage region, retention period table per data type
6. Data sharing — lawful sharing scenarios (service providers, legal requirements, emergencies, user consent)
7. Security measures — list of technical safeguards
8. User rights — right to access, correct, delete, withdraw consent, portability, object + response timeline (15 business days)
9. Minors — state the system is for adult staff only
10. Policy updates — 30-day advance notice for material changes
11. Contact us — company name, email, address

**Writing notes:**
- Health/medical data and government ID numbers must be labeled as "sensitive personal information"
- Retention periods must be specific (e.g. active resident records kept indefinitely; discharged records ≥ 5 years; care logs ≥ 3 years)
- User rights section must include right to withdraw consent and right to data portability
- Adjust the applicable law section to match the user's jurisdiction (PIPL for China, GDPR for EU, CCPA for California, etc.)

### Document 2: Terms of Service

Section structure:
1. Introduction — define both parties (service provider ↔ customer / institution)
2. Definitions — system, service term, authorized capacity, etc.
3. Account registration and management — creation, security, deactivation
4. Services provided — feature list
5. **Pricing and payment** — must use the prices confirmed by the user in Step 1
6. User obligations — lawful use, data entry responsibility, prohibited actions
7. Service provider obligations — SLA (e.g. 99% uptime), data security, support, backups
8. Intellectual property — software belongs to provider; business data belongs to customer
9. Suspension and termination — expiry, violation, post-termination data handling (30-day export window + deletion timeline)
10. Liability — force majeure, data accuracy, indirect losses, liability cap
11. Dispute resolution — governing law and jurisdiction

**Writing notes:**
- Pricing table must include monthly / annual / multi-year columns — use only the prices confirmed by the user
- Refund policy: full refund within 7 days if the system is at fault; pro-rated refund beyond 7 days
- Data ownership must be explicit: business data belongs to the customer, not the service provider

### Document 3: Cookie Policy

Section structure:
1. What cookies are
2. Types of cookies used (four categories: essential / functional / analytics / advertising)
3. Third-party cookies — list each SDK by name with its purpose
4. How to manage cookies — browser instructions (Chrome, Safari, Firefox, Edge)
5. What cookies do NOT contain — reassure users that sensitive data is not stored locally
6. Contact information

**Writing notes:**
- Essential cookies should list the actual storage key names used in the product (e.g. `auth_token`, `user_session`)
- If no advertising or analytics cookies are used, state this explicitly
- Note that cookie policy applies to the web version only; native apps use localStorage or equivalent

### Document 4: Data Processing Agreement (DPA)

When to use: for B2B products where you process personal data on behalf of your customers (required under GDPR Article 28 / PIPL Article 21).

Section structure:
1. Introduction — reference to applicable law, define data controller and data processor
2. Scope of personal data processed — categorized table with sensitivity flags
3. Processing purpose and method — processor may not determine purpose independently
4. Controller's rights and obligations — lawful collection, notice to data subjects, consent for sensitive data
5. Processor's obligations — follow instructions, confidentiality, security measures, 72-hour breach notification, sub-processor restrictions
6. Cross-border data transfers — state clearly whether any transfers occur and to which countries
7. Data retention and deletion — after service termination: export window (e.g. 30 days) + permanent deletion timeline (e.g. 60 days) + deletion certificate
8. Audit rights
9. Liability allocation
10. Agreement term and dispute resolution

**Writing notes:**
- Sub-processors (e.g. AWS, Google Cloud, Tencent Cloud) must be disclosed by name in the agreement
- 72-hour breach notification is a legal requirement under both GDPR and PIPL — do not omit
- Issuing a written data deletion certificate after service termination is standard B2B practice

### Document 5: Disclaimer

Section structure:
1. Product scope — explicitly state what the product is NOT (e.g. not a medical device, not financial advice)
2. Data accuracy — input data accuracy is the user's responsibility, not the provider's
3. System availability — force majeure, planned maintenance, infrastructure failures
4. User operation liability — data entry errors, account security, permission misconfiguration
5. Third-party service liability — limitations on liability for underlying cloud provider failures
6. Regulatory compliance — each customer is responsible for their own compliance with local laws
7. Intellectual property notice
8. Contact information

**Writing notes:**
- Health-related products must clearly state "does not constitute medical diagnosis or medical advice"
- Include emergency handling guidance: contact emergency services first, then log in the system
- Tailor the scope disclaimer to the specific product (a healthcare app has different risks than a billing tool)

---

## Step 3 — Export to PDF

Verify Python3 and Chrome are available:

```bash
python3 -c "import markdown" || pip3 install markdown
```

**Export a single document (example: Terms of Service):**

```bash
# Step 1: Generate HTML
python3 - <<'EOF'
import markdown, pathlib
CSS = """PASTE_CSS_TEMPLATE_HERE"""
md = pathlib.Path("legal/terms-of-service.md").read_text("utf-8")
body = markdown.markdown(md, extensions=["tables", "fenced_code", "nl2br"])
pathlib.Path("legal/terms-of-service.html").write_text(
    f"<!DOCTYPE html><html><head><meta charset='utf-8'><style>{CSS}</style></head><body>{body}</body></html>", "utf-8")
EOF

# Step 2: HTML to PDF via Chrome headless
# macOS:
CHROME="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
# Linux:
# CHROME="google-chrome"

"$CHROME" --headless=new --disable-gpu --no-sandbox \
  --print-to-pdf="legal/terms-of-service.pdf" \
  --print-to-pdf-no-header \
  "file://$(pwd)/legal/terms-of-service.html" 2>/dev/null

# Step 3: Clean up
rm legal/terms-of-service.html
```

**CSS template** (paste into the `CSS` variable above):

```css
@page { margin: 22mm 20mm; size: A4; }
body { font-family: -apple-system, "Segoe UI", Arial, sans-serif;
       font-size: 13px; line-height: 1.8; color: #1a1a1a; }
h1 { font-size: 20px; font-weight: 700; color: #0D9488;
     border-bottom: 2px solid #0D9488; padding-bottom: 8px;
     margin-top: 36px; text-align: center; }
h2 { font-size: 15px; font-weight: 700; color: #0369A1;
     margin-top: 28px; border-left: 4px solid #0369A1; padding-left: 10px; }
h3 { font-size: 13.5px; font-weight: 700; color: #333; margin-top: 18px; }
h4 { font-size: 13px; font-weight: 700; color: #555; margin-top: 14px; }
table { border-collapse: collapse; width: 100%; margin: 12px 0; font-size: 12px; }
th { background: #0D9488; color: white; padding: 7px 10px; text-align: left; }
td { padding: 6px 10px; border-bottom: 1px solid #e0e0e0; vertical-align: top; }
tr:nth-child(even) td { background: #f7fafa; }
code { background: #f0f0f0; padding: 2px 5px; border-radius: 3px; font-size: 11px; }
blockquote { border-left: 4px solid #0D9488; margin: 8px 0; padding: 8px 14px;
             background: #f0fdfb; color: #444; border-radius: 0 6px 6px 0; font-size: 12px; }
ul, ol { padding-left: 22px; margin: 6px 0; }
li { margin: 4px 0; }
hr { border: none; border-top: 1px solid #ddd; margin: 20px 0; }
p { margin: 6px 0; }
strong { color: #0369A1; }
```

> **Theme colors:** The CSS uses teal `#0D9488` and dark blue `#0369A1`. To change the color scheme, do a global find-and-replace on those two hex values.

---

## Updating existing documents

| Situation | What to update |
|-----------|---------------|
| Pricing change | `terms-of-service.md` → re-export PDF |
| Company name / email finalized | All 5 files → replace `[Company Name]` / `[Contact Email]` placeholders |
| New data type collected | `privacy-policy.md` + `data-processing-agreement.md` |
| New third-party SDK added | `privacy-policy.md` + `cookie-policy.md` |
| Jurisdiction change | `privacy-policy.md` (user rights section) + `terms-of-service.md` (dispute resolution) |

---

## Gotchas

- Chrome `--headless=new` on macOS prints `task_policy_set` warnings to stderr — these are harmless. Suppress with `2>/dev/null`. The PDF is generated correctly.
- Markdown tables require `extensions=["tables"]` — omitting it causes tables to render as plain text.
- On Linux, Chinese characters in PDFs require CJK fonts: `apt-get install fonts-noto-cjk`.
- The `file://` path passed to Chrome must be an absolute path — use `$(pwd)` to build it.
- Do not hard-code pricing. Always confirm current prices with the user before writing them into any document.
