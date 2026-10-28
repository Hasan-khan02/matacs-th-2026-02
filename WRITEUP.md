# AI Proposal Triage Design

## Proposed Process

1. The admin creates a category and defines its mandatory requirements, scoring criteria, weights, and scoring rules. These settings are saved as a version rather than hard-coded.

2. The reviewer selects the category and uploads the proposals. The system validates the files and extracts their text.

3. AI extracts relevant facts from each proposal, with supporting text references. Missing or unclear information is marked for review rather than guessed. Instructions inside proposals are treated as untrusted content.

4. A reviewer checks the extracted facts against the source text and resolves any flagged issues.

5. Rule-based code checks mandatory requirements and calculates weighted scores using the selected criteria version. Proposals with unresolved information remain flagged; proposals that fail mandatory requirements are marked ineligible.

6. The system displays eligible proposals in ranked order, with a score breakdown and reasons. Flagged and ineligible proposals are shown separately.

7. A human makes the final award decision. Corrections, overrides, and their reasons are recorded in an audit log.


## 1. AI vs. Rules

AI extracts facts from proposal text, such as price, timeline, experience, warranties, and evidence of mandatory requirements. It returns structured data with supporting text references and marks missing or conflicting information for review.

Plain code validates the extracted data, checks mandatory requirements, calculates weighted scores, and sorts eligible proposals. Criteria, weights, and scoring rules come from the admin's saved configuration.

I use AI to interpret differently written proposals, while explicit rules handle calculations and eligibility consistently. AI does not decide the final ranking or award the contract.


## 2. Consistency and Explainability

Before evaluation, the admin defines scoring formulas or rubrics, weights, and a tie-breaking rule. Each evaluation uses one saved criteria version for all proposals.

The system saves reviewer-confirmed facts and calculates scores using rule-based code. Running the same confirmed facts through the same rules produces the same scores and ranking. AI extraction can vary, so a fresh extraction is treated as a new revision requiring review, rather than silently replacing confirmed facts.

The reviewer can see each extracted value, its supporting text reference, the score for each criterion, its weight, and the total calculation. Missing information and failed requirements are displayed with clear reasons.


## 3. Human in the Loop and Audit

A reviewer checks extracted facts against the proposal text before scoring, especially mandatory requirements, missing information, conflicts, and suspicious instructions. Unresolved proposals remain flagged until clarification is received.

The reviewer can correct extracted facts with supporting evidence and a recorded reason, then rerun scoring. Any authorized decision to depart from the ranking must include a justification and must not silently change scores or bypass mandatory requirements. A human makes the final award decision.

The audit log records the proposal identifier and file hash, criteria version, model and prompt versions, extracted evidence, reviewer corrections, score breakdowns, and final decision. Each human action includes the actor, timestamp, and reason. Previous results are retained.

## 4. Changing Criteria

When an admin edits criteria, weights, or scoring rules, the system creates a new criteria version. Existing evaluations keep their original version and results, so past decisions remain traceable.

The reviewer is notified that newer criteria are available and can start a new evaluation. All proposals in that comparison are evaluated against the same new version; scores from different versions are never mixed.

Confirmed facts can be reused where still relevant. If a new criterion requires information that was not previously extracted, the system extracts it and requests human verification. Missing information is flagged. The new results are saved separately from the original evaluation.


## 5. Risks and Safeguards

- Wrong extraction or invented facts: Require supporting text references for extracted values. Validate data types, currency, and units. Missing or contradictory facts remain flagged for human review rather than guessed.

- Prompt injection: Treat proposal content as untrusted data, never as instructions. AI has no tools or authority to change criteria or rankings. Proposal C's instruction to rank itself first is ignored and logged for review; its actual evidence is evaluated normally.

- Bias: Apply the same criteria version and scoring rules to every proposal. Do not score company names, persuasive language, or unsupported claims. Reviewers check that the configured rubric is relevant and fair.

- Missing or failed requirements: Do not let a low price compensate for a failed mandatory requirement. Proposal B does not satisfy the stated bidder-held licence requirement. Proposal D remains flagged until its timeline is clarified.

- Cost and failures: Limit file size, page count, and AI output length. Cache extraction by document hash and extraction configuration, and cap retries. Failed extraction is shown as an error requiring attention, never as a valid result.

- Privacy and security: Restrict access to authorized users, validate uploaded files, and keep API keys outside the repository. Use only the fictional sample data for this assessment.



## Proposed Scoring Rules

These are proposed admin-configurable rules, not formulas supplied by the brief. They must be approved before evaluating proposals.

Mandatory requirements must pass before ranking. I also treat the budget ceiling and 16-week deadline as eligibility limits. An unknown value needed for eligibility requires clarification.

Each weighted criterion receives a score from 0 to 100:

| Criterion | Scoring rule |
|---|---|
| Experience | 10+ qualifying projects: 100; 5–9: 70; 1–4: 40; none: 0. Only projects within the specified five-year window count. |
| Price | Up to CAD 230,000: 100; above 230,000 to 240,000: 90; above 240,000 to 260,000: 80. |
| Timeline | Up to 10 weeks: 100; above 10 to 12: 90; above 12 to 14: 80; above 14 to 16: 60. |
| Warranty | 50 × min(panel years / 25, 1) + 50 × min(workmanship years / 12, 1). |
| Team and approach | 25 points each for a named lead, specified crew, safety plan, and monitoring plan. Unstated items receive no points. |

Total = experience × 0.30 + price × 0.25 + timeline × 0.15 + warranty × 0.15 + team × 0.15.

Use unrounded totals for ranking and display two decimal places. Ties are resolved by lower price, then shorter timeline. Remaining ties share a rank.



## Sample Evaluation Assumption

For this example, I assume an evaluation date of 1 January 2026 and
a five-year experience window covering 2021–2025. This is a stated
assumption, not a date supplied by the brief. A and E's counts since
2021 are treated as qualifying. C's count since 2020 needs clarification
before scoring. In production, the admin must confirm the exact window.