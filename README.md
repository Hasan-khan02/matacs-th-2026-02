# AI Proposal Triage Design

- **Candidate:** Muhammad Hassan Javed Khan
- **Assessment reference:** MATACS-TH-2026-02
- **Time actually spent:** 3 hours

## Overview

This submission proposes an AI-assisted workflow for reviewing contractor
proposals. AI extracts facts and evidence, configurable rules determine
eligibility and scores, and a human makes the final award decision.

## Deliverables

- [Design write-up and scoring rules](WRITEUP.md)
- [AI extraction prompt](PROMPTS.md)
- [Expected sample ranking](ranking.json)
- [Process-flow diagram](diagram/process-flow.png)

## AI Use

I used ChatGPT to help interpret the brief, draft the process and extraction
prompt, and work through the scoring calculations.

I challenged ChatGPT's suggestion to assume an evaluation date of
1 January 2026, because the brief did not specify that date and the
assumption affected which projects qualified within the five-year window.
I decided that the evaluation date and qualifying project counts should
be confirmed before finalizing experience scores and rankings.

## Scope

This is a design submission. No application or optional prototype is included.
The sample outcomes identify proposals requiring clarification before final
ranking. WRITEUP.md includes a conditional scoring demonstration for A and E.