# Industry Alignment Review

> Monthly RevOps audit of account industry classifications — flags mismatches, missing approvals, and AI vs. SFDC divergences.

A [Cortex Code Desktop](https://docs.snowflake.com/en/user-guide/cortex-code/cortex-code) skill for Michael Lemke's RevOps workflows at Snowflake.

## What It Does

Monthly CoCo workflow that analyzes account industry/subindustry classification data, flags accounts where SFDC, AI-suggested, and Final classifications disagree, identifies Final classifications set without Industry Leader approval, and generates a structured 5-section report with recommended corrections using Snowflake's 56-subindustry taxonomy.

## Business Value

| Metric | Impact |
|--------|--------|
| Time saved | 3–4 hours/month → 20 minutes |
| Data integrity | Catches misclassifications before quota attribution errors |
| Approval tracking | Flags Final classifications missing Industry Leader sign-off |

Maintains the foundation of Snowflake's industry-based revenue attribution. A single misclassified account can shift quota targets between industry leaders.

## Report Sections

1. **Approval gaps** — Finals set without Industry Leader sign-off
2. **SFDC vs AI mismatches** — Where automated classification disagrees with SFDC entry
3. **SFDC vs Final mismatches** — Where Final doesn't match current SFDC data
4. **Recommended corrections** — AI-driven reclassification suggestions with confidence
5. **Summary metrics** — Alignment % by industry vertical

## Prerequisites

- Cortex Code Desktop (CoCo)
- Access to account classification data (MDM + SFDC tables)
- Industry taxonomy knowledge (56 subindustries, 9 verticals)

## Usage

Invoke in CoCo:
```
industry-alignment-review
```

Triggers: "industry review", "industry alignment", "monthly industry audit", "classification review", "industry mismatch report"

## Technologies

- **Snowflake Cortex Code Desktop** — AI-powered IDE and skill runtime
- **Snowflake SQL** — Account classification queries
- **LLM analysis** — Classification recommendation engine
- **56-node taxonomy** — Standardized industry/subindustry hierarchy

## Value Realization

This skill is part of a broader **AI-driven RevOps automation system**. Accurate industry classification is the foundation of Snowflake's quota plans, territory assignments, and industry-based pipeline reporting. Monthly automated reviews ensure data drifts are caught and corrected before they compound — protecting the integrity of revenue attribution across all 9 industry verticals.
