---
name: industry-alignment-review
description: "Monthly RevOps review of account industry and subindustry alignment. Use when: reviewing account industry classifications, flagging SFDC vs AI vs Final industry mismatches, checking industry leader approvals, auditing industry alignment data. Triggers: industry review, industry alignment, account industry, subindustry review, SFDC industry, RevOps review, monthly review, industry mismatch, industry approval, industry classification review."
---

# Industry Alignment Review

Monthly RevOps workflow for reviewing account industry/subindustry classification alignment. Analyzes pasted account data, flags approval gaps, and produces an AI-driven assessment of whether each Final classification is correct.

## Supported Taxonomy

All recommended classifications must use values from this list (Industry Leader shown for reference):

| Code | Industry | Subindustry | Industry Leader |
|------|----------|-------------|----------------|
| 1.1 | Public Sector | Federal & National Government | Glenn Powell |
| 1.2 | Public Sector | State & Local Government | Glenn Powell |
| 1.3 | Public Sector | K-12 & Higher Education | Glenn Powell |
| 2.1 | Healthcare & Life Sciences | Healthcare Providers | Jesse Cugliotta |
| 2.2 | Healthcare & Life Sciences | Life Sciences | Jesse Cugliotta |
| 2.3 | Healthcare & Life Sciences | Health Insurance Payers | Jesse Cugliotta |
| 2.4 | Healthcare & Life Sciences | Health Tech | Jesse Cugliotta |
| 2.5 | Healthcare & Life Sciences | Life Sciences Tech | Jesse Cugliotta |
| 2.6 | Healthcare & Life Sciences | HCLS Other | Jesse Cugliotta |
| 3.1 | Financial Services | Global Banks | Rinesh Patel |
| 3.2 | Financial Services | Regional Commercial & Retail Banks | Rinesh Patel |
| 3.3 | Financial Services | Wealth & Asset Management | Rinesh Patel |
| 3.4 | Financial Services | Insurance | Rinesh Patel |
| 3.5 | Financial Services | Fintech & Payments | Rinesh Patel |
| 3.6 | Financial Services | FinServ Data Providers | Rinesh Patel |
| 3.7 | Financial Services | Exchanges, Clearing & Services | Rinesh Patel |
| 4.1 | Media & Entertainment | Media & Streaming | Dennis Bucheim |
| 4.2 | Media & Entertainment | Music | Dennis Bucheim |
| 4.3 | Media & Entertainment | Sports & Betting | Dennis Bucheim |
| 4.4 | Media & Entertainment | Video & Online Gaming | Dennis Bucheim |
| 4.5 | Media & Entertainment | Agencies | Dennis Bucheim |
| 4.6 | Media & Entertainment | AdTech & MarTech | Dennis Bucheim |
| 4.7 | Media & Entertainment | Entertainment Other | Dennis Bucheim |
| 5.1 | Telecom | Network Service Provider | Dennis Bucheim |
| 5.2 | Telecom | Equipment & Infrastructure Provider | Dennis Bucheim |
| 5.3 | Telecom | Digital and Service Layer Provider | Dennis Bucheim |
| 6.1 | Retail & Consumer Goods | Retail, General Merchandising | Paul Winsor |
| 6.2 | Retail & Consumer Goods | Brands, CPG | Paul Winsor |
| 6.3 | Retail & Consumer Goods | Brands, Hardlines & Softlines | Paul Winsor |
| 6.4 | Retail & Consumer Goods | Retail, Specialty | Paul Winsor |
| 6.5 | Retail & Consumer Goods | Pure-play eCommerce | Paul Winsor |
| 6.6 | Retail & Consumer Goods | Technology, Services, & Data | Paul Winsor |
| 7.1 | Travel & Hospitality | QSR, Restaurants & Food Service | Paul Winsor |
| 7.2 | Travel & Hospitality | Travel Tech, OTAs and TMCs | Paul Winsor |
| 7.3 | Travel & Hospitality | Airlines, Car, Rail, Cruise, Bus | Paul Winsor |
| 7.4 | Travel & Hospitality | Hotels and Accommodations | Paul Winsor |
| 7.5 | Travel & Hospitality | Other Travel and Hospitality | Paul Winsor |
| 7.6 | Travel & Hospitality | Activities, Parks, Attractions | Paul Winsor |
| 8.1 | Manufacturing & Industrial | Technology Hardware | Tim Long |
| 8.2 | Manufacturing & Industrial | Automotive | Tim Long |
| 8.3 | Manufacturing & Industrial | Aerospace & Defense | Tim Long |
| 8.4 | Manufacturing & Industrial | Industrial Goods | Tim Long |
| 8.5 | Manufacturing & Industrial | Agriculture | Tim Long |
| 8.6 | Manufacturing & Industrial | Logistics, Transportation & Supply Chain | Tim Long |
| 8.7 | Manufacturing & Industrial | Power & Utilities | Tim Long |
| 8.8 | Manufacturing & Industrial | Oil & Gas, Chemicals, and Mining | Tim Long |
| 8.9 | Manufacturing & Industrial | Construction | Tim Long |
| 9.1 | Technology | Software - B2B | Prabhath Nanisetty |
| 9.2 | Technology | Software - B2C | Prabhath Nanisetty |
| 9.3 | Technology | Technology Development Services | Prabhath Nanisetty |
| 10.1 | Consulting & Professional Services | Accounting | Consulting |
| 10.2 | Consulting & Professional Services | Consulting | Consulting |
| 10.3 | Consulting & Professional Services | Legal | Consulting |
| 10.4 | Consulting & Professional Services | Professional Services | Consulting |
| 10.5 | Consulting & Professional Services | Nonprofit Organization | Consulting |
| 10.6 | Consulting & Professional Services | Other Private Service | Consulting |

## Workflow

### Step 1: Ingest Data

**Ask** the user to paste the full tab-separated dataset including the header row. It should have 23 columns in this order:

```
FINAL INDUSTRY AND SUBINDUSTRY | PREVIOUS_UPDATE_FLAG | SFDC Industry Leader | SFDC Industry Leader Approval | Final Industry Lead | Final Industry Leader Approval | Erin F Commentary | ID | ACCOUNT_NAME | TYPE | SUBTYPE | AE | CLOSE_DATE | CAP1_ACV | SFDC_INDUSTRY | SFDC_SUBINDUSTRY | AI_CLAUDE_INDUSTRY | AI_CLAUDE_SUBINDUSTRY | AI_COMPANY_DESCRIPTION | PATCH | DISTRICT | REGION | THEATER
```

**If data is already in the conversation**, skip asking and proceed directly to Step 2.

Confirm intake: total data rows, total CAP1_ACV sum, close date range.

---

### Step 2: Analyze Discrepancies

Parse each row. For each account:

1. **Resolve Final Industry and Subindustry** by splitting column 0 on ` || `. Flag `NO_FINAL_CLASSIFICATION` if blank.

2. **Apply 7 discrepancy checks:**

| Flag Code | Condition |
|-----------|-----------|
| `SFDC_MISMATCH` | SFDC_INDUSTRY or SFDC_SUBINDUSTRY does not match Final, AND SFDC fields are not blank |
| `AI_MISMATCH` | AI_CLAUDE_INDUSTRY or AI_CLAUDE_SUBINDUSTRY does not match Final, AND AI fields are not blank |
| `PENDING_SFDC_APPROVAL` | SFDC Leader Approval is not `"Yes"` AND SFDC Leader is not `#N/A` or blank |
| `PENDING_FINAL_APPROVAL` | Final Lead Approval is not `"Yes"` |
| `NO_AI_CLASSIFICATION` | Both AI_CLAUDE_INDUSTRY and AI_CLAUDE_SUBINDUSTRY are blank |
| `NO_SFDC_CLASSIFICATION` | Both SFDC_INDUSTRY and SFDC_SUBINDUSTRY are blank |
| `NO_LEADER_ASSIGNED` | SFDC Industry Leader is `#N/A` or blank |

3. **Assign severity:**
   - **High**: `PENDING_SFDC_APPROVAL` or `PENDING_FINAL_APPROVAL` with ACV ≥ $100K
   - **Medium**: `SFDC_MISMATCH` or `AI_MISMATCH`
   - **Low**: `NO_AI_CLASSIFICATION`, `NO_SFDC_CLASSIFICATION`, `NO_LEADER_ASSIGNED`
   - **Clean**: No flags

`PREVIOUS_UPDATE_FLAG` is informational only — do not treat it as a discrepancy.

---

### Step 3: AI Classification Assessment

For **every account**, use the AI_COMPANY_DESCRIPTION (and account name as context) to assess whether the Final Industry & Subindustry is the best fit from the supported taxonomy. Assign one of four verdicts:

| Verdict | Meaning |
|---------|---------|
| `AGREE` | Final classification is correct and matches the taxonomy |
| `NEEDS_REVIEW — INDUSTRY` | Top-level industry is wrong; recommend a different industry entirely |
| `NEEDS_REVIEW — SUBINDUSTRY` | Industry is correct but subindustry is wrong or imprecise |
| `INSUFFICIENT_DATA` | No AI description and account name alone is ambiguous |

**Assessment rules:**
- Always recommend a value from the supported taxonomy above
- When the Final classification is not in the taxonomy (e.g. non-standard subindustry values), flag it and recommend the closest supported value
- Base the assessment on what the company **primarily does and sells**, not on who its customers are
- Manufacturers, distributors, and processors → Manufacturing & Industrial (not the industry of their customers)
- Software companies → Technology, regardless of the vertical they serve (unless the company is exclusively a fintech payments processor or healthcare provider)
- Payment gateway/fintech companies → Financial Services || Fintech & Payments
- Nonprofits, charities, foundations → CPS || Nonprofit Organization (not Healthcare Providers)
- Law firms → CPS || Legal
- Tax/compliance software companies → Technology || Software-B2B (not Financial Services)

---

### Step 4: Generate Report

Output the following seven sections in order:

---

#### Section 1: Executive Summary

| Metric | Count |
|--------|-------|
| Total accounts reviewed | N |
| Clean (no flags) | N |
| Needs attention (any flag) | N |
| High severity | N |
| Medium severity | N |
| Low severity | N |
| Previously updated in prior cycle | N |
| Total ACV reviewed | $X |
| ACV at risk (flagged accounts) | $X |
| AI Assessment — Agree | N |
| AI Assessment — Needs Review (Industry) | N |
| AI Assessment — Needs Review (Subindustry) | N |
| AI Assessment — Insufficient Data | N |

---

#### Section 2: Discrepancy Breakdown

For each of the 7 flag types: count, total ACV, top 3 accounts by ACV. Sort by count descending.

---

#### Section 3: High-Priority Accounts

Top 20 flagged accounts sorted by ACV descending.

Columns: Account Name | ACV | Theater | Final Industry | SFDC Industry | AI Industry | Flags | Approvals Status

---

#### Section 4: Pending Approvals by Leader

Group accounts with `PENDING_SFDC_APPROVAL` or `PENDING_FINAL_APPROVAL` by leader name. For each leader: count pending, list accounts with ACV (max 10).

---

#### Section 5: Theater Breakdown

| Theater | Total Accounts | Clean | Flagged | Pending Approval | Total ACV |

Sort by Total ACV descending.

---

#### Section 6: AI Assessment — Industry Disagreements

All accounts with verdict `NEEDS_REVIEW — INDUSTRY`, sorted by ACV descending.

| Final Industry & Subindustry | Account Name | Company Website | Recommended Industry & Subindustry | Justification |

Justification should be 1–2 sentences explaining why the Final classification is wrong and what the company actually does.
Company Website should be a markdown hyperlink inferred from the account name and AI company description, e.g. `[acme.com](https://www.acme.com)`. Use best judgment; if uncertain, omit rather than guess.

---

#### Section 7: AI Assessment — Agreed Classifications

All accounts with verdict `AGREE`, sorted by Final Industry then Account Name.

| Final Industry & Subindustry | Account Name | Company Website |

Company Website formatted as a markdown hyperlink inferred from account name and description. Omit if uncertain.

---

## Stopping Points

None — runs end-to-end and outputs the full report.

## Notes

- **SFDC mismatch comparison is case-insensitive** — normalize both sides before comparing.
- **ACV parsing**: Strip `$` and commas before numeric comparison.
- **Empty vs. blank**: Treat empty string, whitespace-only, and `#N/A` as "not provided".
- **Accounts with no FINAL classification**: Flag as `NO_FINAL_CLASSIFICATION` and exclude from mismatch checks.
- Skip rows where all key fields (ID, ACCOUNT_NAME, SFDC_INDUSTRY, AI_CLAUDE_INDUSTRY) are blank.
- Sections 6 and 7 always appear even if empty (show "No accounts in this category").
