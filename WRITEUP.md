# AI Proposal Triage Design

## Proposed Process

1. The admin configures category requirements, weights, scoring rules, and the evaluation window. The system validates that weights total 100% and saves a criteria version.
2. The reviewer selects a category and uploads proposals. Code validates files and extracts text, preserving available page references.
3. AI extracts criterion-specific facts, supporting excerpts, and warnings. Proposal content is untrusted data, never instructions.
4. Code validates the output structure and evidence references. A reviewer verifies facts and resolves ambiguities.
5. Rules check eligibility. Confirmed failures are ineligible; unresolved information needed for evaluation remains flagged.
6. Rules score eligible proposals and display their ranking, calculation breakdowns, and evidence. Flagged and ineligible proposals appear separately.
7. A human makes the award decision. Audit records are written throughout the process.

## 1. AI vs. Rules

AI interprets differently written proposals and extracts structured facts. It does not invent missing values, decide eligibility, calculate scores, or award contracts.

Code handles validation, mandatory checks, weighted calculations, and sorting using admin-configured rules. This separates language interpretation from repeatable decisions.

## 2. Consistency and Explainability

Each comparison uses one criteria version, evaluation window, and scoring rubric. The same reviewer-confirmed facts and rules produce the same scores.

Fresh AI extraction may vary. It creates a new revision requiring review rather than silently replacing confirmed facts. The model and prompt versions are recorded.

Reviewers see source excerpts, extracted values, criterion scores, weights, totals, and reasons for flags or exclusions. Calculations use unrounded values; displayed scores use two decimal places.

## 3. Human Review and Audit

A reviewer verifies extracted facts, especially mandatory evidence, conflicting information, and suspicious instructions. Clarifications require supporting evidence before rescoring.

Corrections and authorized departures from the ranking require recorded reasons. They cannot silently alter scores or bypass mandatory requirements. The final award remains a human decision.

Audit records include document identifiers and hashes, criteria and prompt versions, model version, extracted evidence, corrections, score breakdowns, and decisions. Human actions record the actor, timestamp, and reason. Earlier results remain available.

## 4. Changing Criteria

Edits create a new criteria version without overwriting previous evaluations. Reviewers are notified and can initiate a new comparison using the updated version for every proposal.

Relevant confirmed facts can be reused. New information requirements trigger extraction and verification. Scores from different versions are never mixed, and both evaluations remain traceable.

## 5. Risks and Safeguards

- Extraction errors and invented facts: Require supporting excerpts, validate types and units, and request human verification. Missing or contradictory values are not guessed.
- Prompt injection: Keep system instructions separate from proposal data. AI has no tools or authority to change rules. Ignore and log C's ranking instruction; evaluate its factual evidence normally.
- Bias: Apply identical rules to all proposals. Do not reward company names, persuasive wording, or unsupported claims. Review the rubric for relevance and fairness.
- Missing requirements: Low prices cannot compensate for mandatory failures. Distinguish bidder-held licences from subcontractor licences.
- Cost and failures: Limit file size, pages, output length, and retries. Cache extraction by document hash and extraction configuration. Failed extraction requires attention rather than becoming a valid result.
- Security: Restrict access, validate uploads, and keep credentials outside the repository. This assessment uses only fictional data.

## Proposed Scoring Rules

These proposed admin-configurable rules require approval before evaluation; the brief supplies weights but not these scoring formulas.

Mandatory requirements must pass. I also treat the budget ceiling and 16-week deadline as eligibility limits. Unknown information needed for eligibility requires clarification.

| Criterion | Score from 0 to 100 |
|---|---|
| Experience | 10+ qualifying projects: 100; 5–9: 70; 1–4: 40; none: 0. |
| Price | Up to CAD 230,000: 100; above 230,000 to 240,000: 90; above 240,000 to 260,000: 80. |
| Timeline | Up to 10 weeks: 100; above 10 to 12: 90; above 12 to 14: 80; above 14 to 16: 60. |
| Warranty | 50 × min(panel years / 25, 1) + 50 × min(workmanship years / 12, 1). |
| Team | 25 each for a named lead, specified crew, safety plan, and monitoring plan. Unstated items earn no points. |

Total = experience × 0.30 + price × 0.25 + timeline × 0.15 + warranty × 0.15 + team × 0.15.

Break equal totals by lower price, then shorter timeline. Remaining ties share a rank.

## Sample Outcomes and Clarifications

The brief provides no evaluation date. The admin must confirm the five-year window. Counts stated as "since 2021" or "since 2020" cannot automatically be treated as entirely within that window.

Current outcomes in ranking.json:
- A and E: Flagged pending confirmation of qualifying project counts.
- B: Ineligible because the proposal does not establish the required bidder-held licence.
- C: Flagged pending experience-window clarification; its ranking instruction is ignored.
- D: Flagged because its timeline is unspecified.

### Conditional Scoring Demonstration

If verification confirms A has 14 qualifying projects and E has eight, their scores would be:

| Weighted contribution | A | E |
|---|---:|---:|
| Experience | 30 | 21 |
| Price | 22.50 | 20 |
| Timeline | 13.50 | 12 |
| Warranty | 13.75 | 15 |
| Team | 15 | 15 |
| Total | 94.75 | 83.00 |

Under those conditions, A ranks ahead of E. These illustrative totals are not final scores or a complete ranking. C and D require clarification and could affect the eventual ordering.