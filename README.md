# AuditZero — India-First Compliance Dataset

> **The open compliance knowledge base for Indian regulatory frameworks.**
> Machine-readable rules, obligations, and gap indicators — built to power AI-driven compliance engines for Indian web apps, mobile apps, SaaS, fintech, and more.

---

## What This Is

AuditZero is a structured compliance dataset extracted from 25 Indian and international regulatory frameworks. It is the **knowledge base layer** of a compliance engine — the part that answers: *"Given a product, which regulations apply, and what exactly must the product do to be compliant?"*

Every rule, obligation, definition, penalty, and right from each framework has been extracted, cleaned, and structured into a machine-readable JSON format with:
- A plain-English description (no legal jargon)
- A yes/no test question for automated gap detection
- Severity rating and penalty exposure
- Evidence required to prove compliance
- Tags for fast lookup
- Cross-references to related controls

This dataset does **not** require re-reading the original PDFs at query time. Your compliance engine queries these JSON files — fast, consistent, and maintainable.

---

## The Problem This Solves

Indian companies face a uniquely complex compliance landscape:
- **DPDP Act 2023** (effective 2027) — India's comprehensive data protection law, with penalties up to ₹250 crore per violation
- **Multi-regulator overlap** — a fintech must simultaneously satisfy RBI, SEBI or IRDAI, CERT-In, PMLA, and DPDP obligations
- **63 million+ MSMEs** that can't afford compliance consultants
- **No structured, machine-readable dataset** of Indian compliance controls existed — until now

---

## Frameworks Covered

### 🇮🇳 Universal (applies to all Indian digital products)

| Framework | Regulator | Controls | Key Obligation |
|-----------|-----------|----------|----------------|
| DPDP Act 2023 | MeitY | 19 | Data protection, consent, breach notification |
| DPDP Rules 2025 | MeitY | 49 | Consent Manager, DPIA, security safeguards |
| IT Act 2000 | MeitY | 109 | Cybercrime, electronic records, liability |
| IT Rules 2011 | MeitY | 9 | Sensitive personal data, privacy policy |
| CERT-In Directions 2022 | CERT-In | 6 | 6-hour incident reporting, point of contact |

### 💰 Financial Services

| Framework | Regulator | Controls | Applies To |
|-----------|-----------|----------|------------|
| RBI KYC Master Directions | RBI | 113 | All RBI-regulated entities |
| RBI Digital Lending 2025 | RBI | 31 | Digital lenders, LSPs |
| RBI Cybersecurity Framework | RBI | 61 | Banks, NBFCs |
| RBI IT Outsourcing Directions | RBI | 36 | RBI entities outsourcing IT |
| RBI Account Aggregator Framework | RBI | 35 | AA ecosystem (AA, FIP, FIU) |
| PMLA / AML-CFT | FIU-IND | 58 | Financial businesses |
| PCI DSS v4.0 | PCI SSC | 358 | Card data environments |
| SEBI AML/CFT Guidelines | SEBI | 43 | Securities intermediaries |
| SEBI Cybersecurity Framework 2024 | SEBI | 177 | All SEBI-regulated entities |
| SEBI Investment Adviser Regs 2013 | SEBI | 59 | Investment advisers |
| SEBI Algo Trading Circular 2025 | SEBI | 7 | Algo/HFT brokers |
| IRDAI Cyber Security Guidelines 2023 | IRDAI | 384 | All insurers |
| IRDAI Web Aggregator Regs 2017 | IRDAI | 98 | Insurance web platforms |
| IRDAI AML/CFT Guidelines | IRDAI | 32 | Insurance entities |

### 🏢 Corporate & Sectoral

| Framework | Regulator | Controls | Applies To |
|-----------|-----------|----------|------------|
| Companies Act 2013 | MCA | 744 | All incorporated companies |
| Consumer Protection (E-Commerce) Rules 2020 | MoCA | 7 | E-commerce platforms |
| Labour Codes 2020 | MoLE | 26 | All employers |
| Telemedicine Guidelines 2020 | NMC | 75 | Digital health platforms |

### 🌍 Global Standards

| Framework | Body | Controls | Applies To |
|-----------|------|----------|------------|
| GDPR 2018 | EU DPA | 283 | Products with EU users |
| ISO 27001:2022 | ISO | 46 | SaaS seeking enterprise customers |
| SOC 2 Type II | AICPA | — | *(placeholder — paid standard)* |

**Total: 2,865 controls across 25 frameworks**

---

## Dataset Statistics

```
Total controls       : 2,865
  ├── Critical       :   155  (licence revocation / ₹10 Cr+ penalty)
  ├── High           : 1,497  (mandatory obligations, major penalties)
  ├── Medium         : 1,006  (standard compliance requirements)
  └── Low / Info     :   207  (recommended / definitions)

Content types:
  ├── OBLIGATION     : 1,650  (what you MUST do)
  ├── PROCEDURE      :   670  (how to do it)
  ├── DEFINITION     :   153  (legal term definitions)
  ├── PENALTY        :   116  (consequences of non-compliance)
  ├── PROHIBITION    :   139  (what you MUST NOT do)
  ├── TIMELINE       :    87  (deadlines and time limits)
  ├── RIGHT          :    24  (user/data principal rights)
  └── EXEMPTION      :    26  (who is exempt)

Glossary terms       :   136
Searchable tags      :    62

Industry verticals   :    14  (fintech, healthtech, edtech, ecommerce, saas, hrtech, logistics, ...)
Sub-types            :    28  (payment_gateway, lending, neobank, wealthtech, telemedicine, ...)
```

---

## Repository Structure

```
Compliances/
│
├── frameworks/                        # Source PDFs + metadata
│   ├── universal/                     # DPDP Act, DPDP Rules, IT Act, IT Rules, CERT-In
│   │   └── DPDP_ACT_2023/
│   │       ├── original/              # Original PDF
│   │       ├── metadata.json          # Framework metadata
│   │       └── README.md              # Plain-English summary
│   ├── financial/                     # RBI, SEBI, IRDAI, PCI DSS, PMLA
│   ├── corporate/                     # Companies Act, Consumer Protection, Labour, Telemedicine
│   └── global/                        # GDPR, ISO 27001, SOC 2
│
└── dataset/                           # ← Compliance engine knowledge base
    ├── all_controls.json              # MASTER FILE — all 2,865 controls merged
    ├── framework_selector.json        # Maps product profile → applicable frameworks (condition-based rules)
    ├── industry_framework_map.json    # Maps industry vertical + sub-type → applicable frameworks
    ├── tags_index.json                # Tag → [control_ids] inverted index
    ├── glossary.json                  # 136 legal term definitions
    ├── dataset_stats.json             # Counts and coverage statistics
    ├── validation_report.json         # Quality audit results
    ├── ocr_required.json              # Frameworks pending OCR enhancement
    ├── overlaps/
    │   └── cross_framework_overlaps.json   # Controls that overlap across frameworks
    └── controls/                      # Per-framework control files
        ├── DPDP_ACT_2023_controls.json
        ├── RBI_KYC_MASTER_DIRECTIONS_controls.json
        └── ... (25 files total)
```

---

## Control Record Schema

Every control in the dataset follows this schema:

```json
{
  "control_id":       "CERT_IN_DIRECTIONS_2022-001",
  "framework_id":     "CERT_IN_DIRECTIONS_2022",
  "regulator":        "CERT-In",
  "section_ref":      "Direction 3",
  "content_type":     "OBLIGATION",
  "obligation_type":  "mandatory",

  "title":            "Designate a Point of Contact for CERT-In",
  "plain_english":    "All service providers, intermediaries, data centres, and body corporates must designate a Point of Contact to interface with CERT-In and keep that information updated.",
  "original_text":    "...the service provider/intermediary/data centre/body corporate...",

  "applies_to": {
    "product_types":          ["web_app", "mobile_app", "saas", "fintech", "healthtech", "ecommerce"],
    "has_indian_users":       true,
    "handles_personal_data":  false,
    "conditional_flag":       null
  },

  "test_question":    "Does your organisation designate a Point of Contact to interface with CERT-In?",

  "gap_indicators": [
    "No designated Point of Contact exists in the organisation's records.",
    "No evidence of POC details being submitted to CERT-In in the required format."
  ],

  "evidence_required": [
    { "name": "CERT-In POC registration (Annexure II format)", "type": "document",   "mandatory": true  },
    { "name": "Audit log / system log",                        "type": "log",        "mandatory": true  }
  ],

  "timeline": {
    "exists":        true,
    "value":         "6 hours",
    "trigger_event": "breach/incident occurrence"
  },

  "penalty": {
    "exists":      true,
    "amount_inr":  0,
    "description": "Non-compliance treated as direction violation",
    "type":        "licence_revocation"
  },

  "severity":    "high",
  "keywords":    ["breach_notification", "cert_in", "incident_response", "vendor_management"],
  "tags":        ["breach_notification", "cert_in", "incident_response", "vendor_management"],

  "remediation_steps": [
    "Assign a named individual as the CERT-In Point of Contact.",
    "Submit POC details to CERT-In using the Annexure II format.",
    "Set up a calendar reminder to update POC information whenever personnel change."
  ],

  "cross_references": ["Direction 4", "Annexure II"]
}
```

---

## Industry Vertical → Framework Map

`dataset/industry_framework_map.json` maps every product category to its applicable compliance frameworks. All verticals also inherit the **base requirements** (DPDP Act, IT Act, IT Rules, CERT-In, Companies Act) which apply to every Indian digital product.

### 💸 Fintech — Financial Technology
*Sub-types: Payment Gateway · Lending · Neobank · Wealthtech · Account Aggregator · Crypto/Web3*

| Applicability | Frameworks |
|---|---|
| Mandatory | RBI KYC Master Directions · PMLA AML/CFT · RBI Cybersecurity Framework |
| Conditional | RBI IT Outsourcing Directions *(if outsourcing IT)* |
| Recommended | ISO 27001:2022 |

### 🛡️ Insurtech — Insurance Technology
*Sub-types: Insurance Company · Insurance Web Aggregator*

| Applicability | Frameworks |
|---|---|
| Mandatory | IRDAI Cyber Security Guidelines · IRDAI AML/CFT · PMLA AML/CFT · RBI KYC |
| Conditional | IRDAI Web Aggregator Regs *(if operating as web aggregator)* |

### 🏥 Healthtech — Health Technology
*Sub-types: Telemedicine · Health Records · Wellness & Fitness*

| Applicability | Frameworks |
|---|---|
| Mandatory | DPDP Act 2023 · IT Rules 2011 |
| Conditional | Telemedicine Guidelines 2020 *(if offering doctor consultations)* |
| Recommended | ISO 27001:2022 |

### 🎓 Edtech — Education Technology
*Sub-types: K-12 Platform · Upskilling / Professional Learning*

| Applicability | Frameworks |
|---|---|
| Conditional | DPDP Act 2023 *(if collecting children's data)* · GDPR 2018 *(if EU users)* · PCI DSS v4 *(if processing payments)* |
| Recommended | ISO 27001:2022 |

### 🛒 E-Commerce / Online Retail
*Sub-types: Marketplace · D2C Brand · Quick Commerce*

| Applicability | Frameworks |
|---|---|
| Mandatory | Consumer Protection (E-Commerce) Rules 2020 |
| Conditional | PCI DSS v4 *(if handling card data directly)* · PMLA AML/CFT *(if high-value marketplace)* |

### ☁️ SaaS — Software as a Service
*Sub-types: Fintech SaaS · Healthtech SaaS · HR SaaS · Developer Platform*

| Applicability | Frameworks |
|---|---|
| Conditional | GDPR 2018 *(if EU customers)* · RBI IT Outsourcing Directions *(if serving RBI-regulated entities)* |
| Recommended | ISO 27001:2022 |

### 👥 HRtech — HR Technology
*Sub-types: Recruitment Platform · Gig Platform · Payroll Platform*

| Applicability | Frameworks |
|---|---|
| Mandatory | Labour Codes 2020 |
| Conditional | DPDP Act 2023 *(if processing employee/candidate data)* · PCI DSS v4 *(if salary card programs)* |

### 🚚 Logistics & Supply Chain
*Sub-types: Last-Mile Delivery · Freight Platform*

| Applicability | Frameworks |
|---|---|
| Mandatory | Labour Codes 2020 |
| Conditional | DPDP Act 2023 *(if tracking individual locations)* |
| Recommended | ISO 27001:2022 |

### 🎬 Media & Entertainment
*Sub-types: OTT Platform · Gaming · Social Media*

| Applicability | Frameworks |
|---|---|
| Mandatory | IT Rules 2011 *(intermediary / SSMI obligations)* |
| Conditional | DPDP Act 2023 *(if platform has minors)* · GDPR 2018 *(if EU users)* |

### 🏛️ Govtech — Government Technology

| Applicability | Frameworks |
|---|---|
| Mandatory | CERT-In Directions 2022 · DPDP Act 2023 · ISO 27001:2022 |

### 🌾 Agritech — Agricultural Technology

| Applicability | Frameworks |
|---|---|
| Conditional | DPDP Act 2023 *(if collecting farmer data)* · RBI KYC *(if providing agri-credit)* · PMLA AML/CFT *(if large cash transactions)* |

### 🏠 Proptech — Property Technology

| Applicability | Frameworks |
|---|---|
| Mandatory | Companies Act 2013 |
| Conditional | PMLA AML/CFT *(if facilitating property transactions)* · PCI DSS v4 *(if handling card payments)* |

### ⚖️ Legaltech — Legal Technology

| Applicability | Frameworks |
|---|---|
| Conditional | DPDP Act 2023 *(if storing legal documents)* · GDPR 2018 *(if EU clients)* |
| Recommended | ISO 27001:2022 |

### 🤖 Deep Tech / AI / IoT

| Applicability | Frameworks |
|---|---|
| Conditional | DPDP Act 2023 *(if training on personal data)* · IT Rules 2011 *(if biometric data)* · CERT-In Directions *(if critical infrastructure)* · GDPR 2018 *(if EU personal data)* |
| Recommended | ISO 27001:2022 |

---

## How to Use This Dataset

### 1. Identify applicable frameworks by industry vertical

Query `dataset/industry_framework_map.json` using your product's vertical and sub-type:

```python
import json

imap    = json.load(open("dataset/industry_framework_map.json"))
controls = json.load(open("dataset/all_controls.json"))["controls"]

# Step 1 — collect framework IDs for your product
# e.g. a lending fintech with Indian + EU users
base_ids     = [f["framework_id"] for f in imap["base_requirements"]["frameworks"]
                if f["applicability"] == "mandatory"]
eu_ids       = [f["framework_id"] for f in imap["cross_border_requirements"]["eu_users"]["frameworks"]]
vertical_ids = [f["framework_id"] for f in imap["verticals"]["fintech"]["frameworks"]["mandatory"]]
subtype_ids  = [f["framework_id"] for f in imap["verticals"]["fintech"]["sub_types"]["lending"]["frameworks"]["mandatory"]]

applicable = set(base_ids + eu_ids + vertical_ids + subtype_ids)

# Step 2 — filter controls
my_controls = [c for c in controls if c["framework_id"] in applicable]
print(f"Applicable frameworks : {len(applicable)}")
print(f"Controls to check     : {len(my_controls)}")
```

**Output for a lending fintech (India + EU):**
```
Applicable frameworks : 11
Controls to check     : 872
```

---

### 2. Select applicable frameworks for a product (condition-based rules)

Query `dataset/framework_selector.json` with the product's profile:

```python
import json

selector = json.load(open("dataset/framework_selector.json"))
product  = {
    "product_types":       ["fintech"],
    "has_indian_users":    True,
    "handles_payment_data": True
}

applicable = []
for rule in selector["rules"]:
    cond = rule["condition"]
    if cond.get("has_indian_users") and product.get("has_indian_users"):
        applicable.extend(rule["applicable_frameworks"])
    if any(pt in product.get("product_types", []) for pt in cond.get("product_types", [])):
        applicable.extend(rule["applicable_frameworks"])

# Sort by priority
applicable.sort(key=lambda x: x["priority"])
for fw in applicable:
    print(f"[{fw['applicability'].upper()}] {fw['framework_id']}: {fw['reason']}")
```

**Output for a fintech product:**
```
[MANDATORY] DPDP_ACT_2023: Applies to all Digital Data Fiduciaries processing personal data of Indian residents
[MANDATORY] RBI_KYC_MASTER_DIRECTIONS: KYC/CDD required for all RBI-regulated entities
[MANDATORY] RBI_DIGITAL_LENDING_2025: Applies to all digital lending platforms and LSPs
[MANDATORY] PCI_DSS_V4: Required for any entity storing, processing or transmitting card data
[MANDATORY] PMLA_AML_CFT: AML/CFT obligations for all financial businesses
```

---

### 3. Run a gap check on a specific framework

```python
import json

# Load controls for the framework
fw_controls = json.load(open("dataset/controls/DPDP_ACT_2023_controls.json"))

for control in fw_controls["controls"]:
    if control["obligation_type"] == "mandatory":
        print(f"[{control['severity'].upper()}] {control['control_id']}")
        print(f"  Q: {control['test_question']}")
        print(f"  Evidence needed: {[e['name'] for e in control['evidence_required']]}")
        print()
```

---

### 4. Search by tag

```python
import json

tags_index = json.load(open("dataset/tags_index.json"))
all_controls_map = {c["control_id"]: c
                    for c in json.load(open("dataset/all_controls.json"))["controls"]}

# Find all consent-related controls
consent_controls = tags_index.get("consent", [])
for cid in consent_controls:
    c = all_controls_map[cid]
    print(f"{cid} [{c['framework_id']}] — {c['title']}")
```

**Available tags include:** `consent`, `breach_notification`, `data_retention`, `access_control`, `audit_log`, `vendor_management`, `incident_response`, `governance`, `kyc`, `aml`, `encryption`, `vulnerability`, `personal_data`, `data_localisation`, `children` and 47 more.

---

### 5. Get penalty exposure for a product

```python
import json

all_controls = json.load(open("dataset/all_controls.json"))["controls"]

# Filter by framework and severity
critical_gaps = [
    c for c in all_controls
    if c["framework_id"] in ["DPDP_ACT_2023", "DPDP_RULES_2025"]
    and c["severity"] == "critical"
]

total_exposure = sum(c["penalty"]["amount_inr"] for c in critical_gaps if c["penalty"]["exists"])
print(f"Critical DPDP controls : {len(critical_gaps)}")
print(f"Max penalty exposure   : ₹{total_exposure:,.0f}")
```

---

## How the Dataset Was Built

The dataset was built through a fully automated pipeline:

```
Original PDFs → Text Extraction (pdfplumber + Tesseract OCR)
             → Section Segmentation (regex pattern matching)
             → Content Classification (keyword heuristics)
                 OBLIGATION / PROHIBITION / DEFINITION /
                 PENALTY / TIMELINE / EXEMPTION / RIGHT / PROCEDURE
             → Structured Extraction
                 (plain_english, test_question, applies_to,
                  timeline, penalty, evidence, gap_indicators, tags)
             → Jargon Cleanup (deterministic text transformation)
             → Cross-framework Deduplication
             → Validation (2,865/2,865 passing all checks)
```

**Key design decisions:**
- **No LLM API calls at query time.** The engine queries structured JSON — responses are fast and deterministic.
- **`original_text` preserved.** The source legal text is always stored alongside the plain-English version for verification.
- **`llm_enhance` flag.** Controls that could benefit from further LLM enrichment are flagged — a future batch pass can improve them without rebuilding the dataset.
- **Scanned PDFs handled via OCR.** Three SEBI documents (CSCRF 206 pages, AML/CFT, Investment Adviser Regs) were processed using Tesseract OCR at 120 DPI.

---

## Adding a New Framework

To add a new regulatory framework to the dataset:

1. **Add the PDF** to `frameworks/<category>/<FRAMEWORK_ID>/original/`
2. **Create metadata.json** in the framework folder:
```json
{
  "framework_id": "NEW_FRAMEWORK_ID",
  "framework_name": "Full Official Name",
  "regulator": "Regulating Body",
  "version": "2025",
  "effective_date": "2025-01-01",
  "source_url": "https://official.source.url/document.pdf",
  "document_status": "verified_latest"
}
```
3. **Re-run the pipeline** on just the new framework — the existing dataset is unaffected
4. **Merge** the new `*_controls.json` into `all_controls.json` by re-running `stage2_master.py`

---

## Product Architecture

AuditZero is designed as a full-stack compliance automation platform. The dataset in this repository is the **Framework Knowledge Base** — the bottom-left box in the flow below.

```
┌─────────────────────────────────────────────────────┐
│                   🌐 Web Portal                      │
│              Login / Register    [🔑 API Key]        │
└────────────────────────┬────────────────────────────┘
                         │
                         ▼
            ┌────────────────────────┐
            │   Connect with GitHub  │
            │  (OAuth or manual zip) │
            └────────────┬───────────┘
                         │
                         ▼
            ┌────────────────────────┐
            │  Extract the Codebase  │
            │ files · configs · deps │
            └────────────┬───────────┘
                         │
                         ▼
            ┌────────────────────────────────┐
            │  Analyze & Classify Product    │
            │  FinTech · SaaS · HealthTech   │
            │  EdTech · eCommerce · HRTech   │
            └──────────────┬─────────────────┘
                           │
              ┌────────────┘
              │   Based on category →
              ▼   Framework Selection
┌─────────────────────┐      ┌────────────────────────────┐
│  Framework          │ ───► │  DPDP · RBI · SEBI · IRDAI │
│  Knowledge Base     │      │  CERT-In · PCI DSS · GDPR  │
│  (This repo — JSON) │      │  ISO 27001 · PMLA · ...    │
└─────────────────────┘      └────────────┬───────────────┘
                                          │
                                          ▼
                         ┌─────────────────────────────────┐
                         │         ⚡ Gap Analysis          │  ← 🤖 Agent
                         │                                  │
                         │  Code + Framework Controls + LLM │
                         │  → runs test_question per control│
                         └─────────────┬───────────────────┘
                                       │
                                       ▼
                         ┌─────────────────────────────────┐
                         │       🔍 Evidence Collection     │
                         │  logs · configs · policy docs    │
                         └─────────────┬───────────────────┘
                                       │
                                       ▼
                         ┌─────────────────────────────────┐
                         │       📄 Report Generation       │
                         │  score · gaps · penalty exposure │
                         └─────────────┬───────────────────┘
                                       │
                                       ▼
                         ┌─────────────────────────────────┐
                         │       💡 Recommendations         │
                         │  prioritised remediation plan    │
                         └─────────────────────────────────┘
```

**Current Status:** The Framework Knowledge Base (this repository) is complete — 2,865 controls across 25 frameworks. The Gap Analysis Engine is the next stage being built.

---

## Roadmap

This dataset is **Stage 1** of the AuditZero compliance platform. The planned next stages are:

- **Stage 2 — Gap Analysis Engine:** Takes a GitHub codebase, classifies the product type (FinTech / SaaS / HealthTech / etc.), queries `framework_selector.json`, and runs each applicable control's `test_question` against the code to identify gaps.
- **Stage 3 — Evidence Collection:** Automated collection of compliance evidence — screenshots, policy document hashes, configuration exports.
- **Stage 4 — Report Generation:** Structured gap reports with prioritised remediation plans, penalty exposure estimates, and compliance scores per framework.
- **Stage 5 — Regulatory Radar:** Background monitoring of RBI/SEBI/MeitY/IRDAI circulars — auto-updates the dataset when new obligations are published.

---

## Frameworks Pending Completion

Three SEBI frameworks were processed via OCR but cover only pages 1–120 of the SEBI CSCRF document (pages 121–206 are annexures and templates). Contributions to complete these are welcome:

| Framework | Status | Notes |
|-----------|--------|-------|
| `SEBI_CYBERSECURITY_FRAMEWORK_2024` | Partial | pp 121–206 (annexures) not yet extracted |
| `SOC2_TYPE2` | Placeholder | Paid AICPA standard — requires licensed copy |

---

## Contributing

Contributions are welcome, especially:
- **New frameworks** — state or sector-specific regulations (e.g. TRAI, FSSAI, SEBI PMS Regulations)
- **Control corrections** — if a `plain_english` or `test_question` is inaccurate, open a PR with the corrected text and the source section reference
- **OCR improvements** — better text extraction for scanned PDFs
- **New tags** — if a compliance domain is missing from the tag index

Please include the source section reference for any content changes.

---

## Disclaimer

This dataset is provided for informational and engineering purposes only. It is **not legal advice**. While every effort has been made to accurately represent the source regulations, organisations should consult qualified legal counsel before relying on this dataset for compliance decisions. Regulations change — always verify against the latest official source.

---

## License

The dataset structure, pipeline code, and metadata are released under the **MIT License**.

Original regulatory documents remain the property of their respective government bodies and are reproduced here for research and compliance engineering purposes only.

---

*Built with 🇮🇳 for Indian startups. Part of the [AuditZero](https://github.com/your-org/auditzero) open compliance platform.*
