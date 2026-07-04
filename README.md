# Industry Alignment Review

> Monthly audit of SFDC vs AI-suggested vs Final industry classifications — flags gaps and generates correction recommendations.

A [Cortex Code Desktop](https://docs.snowflake.com/en/user-guide/cortex-code/cortex-code) skill for Michael Lemke's RevOps workflows at Snowflake.

## What It Does

The skill pulls the current state of industry classifications for all accounts, compares the SFDC-recorded industry, the AI-suggested industry, and the Final approved industry, then flags approval gaps (Final set without Industry Leader sign-off) and mismatches (SFDC and AI disagree). It generates a 5-section report with a summary of total accounts reviewed, flagged accounts, recommended corrections, and the accounts requiring Industry Leader action.

## Business Value

| Metric | Impact |
|--------|--------|
| Time saved | 2–4 hours/month |
| Systems integrated | Salesforce, Snowflake account tables, Industry Leader roster |
| Manual steps eliminated | Cross-referencing three classification columns, approval gap detection, correction drafting |

Maintains data integrity for Snowflake's industry-based revenue attribution and quota plans. Replaces a manual monthly audit spreadsheet exercise with a structured, repeatable, automated report.

## How It Works

1. Skill queries Snowflake for all accounts with their SFDC industry, AI-suggested industry, and Final industry classification.
2. Applies rule-based logic to flag: (a) accounts where Final ≠ SFDC or AI, (b) accounts where Final was set without an Industry Leader approval record, and (c) accounts where SFDC and AI disagree without a Final resolution.
3. Groups flagged accounts by Industry Leader for easy routing.
4. Uses LLM analysis to summarize patterns and draft recommended corrections.
5. Outputs a 5-section markdown report covering: Summary, Approval Gaps, SFDC/AI Mismatches, Correction Recommendations, and Accounts Needing Action.

## Prerequisites

- Cortex Code Desktop (CoCo)
- Snowflake connection with read access to the account classification and Industry Leader tables
- Role with visibility into SFDC sync tables in Snowflake

## Usage

Invoke by typing the skill name in CoCo, or with natural language triggers listed in the skill description (e.g., "run industry alignment review", "monthly industry audit", "check industry classification gaps").

## Technologies

- **Snowflake Cortex Code Desktop** — AI-powered IDE and skill runtime
- **Snowflake SQL** — multi-column classification comparison and approval gap queries
- **LLM-powered analysis** — pattern detection, correction drafting, and report generation

## Value Realization

This skill is part of a broader **AI-driven RevOps automation system** that eliminates manual work across the Snowflake Sales Engineering and RevOps teams. Industry classification accuracy is directly tied to quota attribution and pipeline analytics — this skill ensures the monthly audit happens consistently, catches gaps before they distort reporting, and surfaces the right accounts to the right Industry Leaders for sign-off.
