# Master Dataset Reference

This document describes every dataset in the `exercises/` directory, its purpose, column definitions, and baked-in patterns.

---

## Dataset Index

| Dataset | Chapter(s) | Rows | Purpose |
|---------|-----------|------|---------|
| transcript-batch1.csv | Ch 4, Ch 5 | 8 | Error analysis: open and axial coding |
| prompt-iteration-data.csv | Ch 5, Ch 7 | 14 | Prompt iteration results and LLM judge calibration |
| eval-strategy-costs.csv | Ch 6 | 6 | Evaluation strategy cost/coverage comparison |
| launch-timeseries.csv | Ch 8, Ch 9 | 42 | Post-launch: spike, decay, and confounds |
| ship-signals.csv | Ch 9 | 12 | Mixed signals for ship/hold/conditional decision |
| hackathon-scoping.md | Ch 2 | Template | Hackathon validation exercise |
| derisking-template.md | Ch 3 | Template | Tech derisking exercise |

---

## transcript-batch1.csv

**Purpose:** Learners practice error analysis by performing open coding and axial coding on real conversation logs from Babbel Speak's alpha.

**Columns:**
- `conversation_id` — Unique identifier
- `language` — Target language (Spanish, French, German, Italian)
- `user_level` — Learner's stated CEFR level (all A1)
- `transcript_snippet` — AI-user conversation exchange (4-8 turns)
- `failure_type` — Ground truth label (hidden from learner during open coding)

**Baked-in patterns:**
- 3 clean conversations (CONV-002, CONV-003, CONV-004): Appropriate for A1, natural flow
- 2 level_mismatch (CONV-001, CONV-005): AI vocabulary/grammar too advanced for A1
- 1 coherence_failure (CONV-006): AI repeats itself, especially noticeable in the "pique-nique" repetition
- 1 language_switch (CONV-007): AI initially uses formal "Sie" then switches to informal "du" after user feedback — subtle failure where the AI corrected formality but the taxonomy flags it as language quality concern
- 1 language_quality (CONV-008): AI corrects learner's grammar mid-conversation AND uses overly complex sentences — dual failure that tests whether learners catch both issues

**Difficulty note:** Some failures are subtle. CONV-007 (language_switch) may be debated — some learners will see the AI's formality switch as good adaptation, not a failure. This is intentional: it mirrors real open coding disagreements.

---

## prompt-iteration-data.csv

**Purpose:** Learners analyze whether prompt iterations are improving quality. Tests ability to detect regressions, identify rollback candidates, and calibrate LLM judge trust.

**Columns:**
- `iteration` — Sequential iteration number (1-14)
- `failure_mode` — Which of the four failure modes this iteration targets
- `prompt_version` — Semantic version of the prompt change
- `quality_score_before` — Quality score (0-1) before this iteration
- `quality_score_after` — Quality score (0-1) after this iteration
- `human_agreement_rate` — LLM judge vs. human label agreement rate
- `rolled_back` — Whether this iteration was ultimately rolled back

**Baked-in patterns:**
- Iteration 4 (communication_breakdown v1.3): Quality drops from 0.88 to 0.64. Clear regression — should be rolled back. LLM agreement rate also drops to 0.72 (below 85% threshold).
- Iteration 8 (level_appropriateness v2.3): Quality drops from 0.85 to 0.79. Regression, but less severe. Rolled back. Agreement rate at 0.83 (borderline).
- Iterations 1-3: Progressive improvement on communication_breakdown. Classic three-iteration fix pattern from Chapter 5.
- Iterations 5-7: Progressive improvement on level_appropriateness.
- Iterations 9-12: Coherence improvement plateaus at v3.2 (0.87). v3.3 showed no additional gain — likely hit the ceiling.
- Iterations 13-14: Conversation ending improvements. Reaches 0.84 but still below the 0.85+ best iterations.

**Calibration exercise note:** Human agreement rates below 0.85 (iterations 1, 4, 8, 13) mean the LLM judge should not be trusted at those stages. The relationship between quality scores and agreement rates is non-linear — some high-quality iterations (v3.2, 0.87 quality, 0.88 agreement) are well-calibrated, while others (v2.3, 0.79 quality, 0.83 agreement) show misalignment.

---

## eval-strategy-costs.csv

**Purpose:** Learners compare evaluation strategies across cost, coverage, and latency. Tests ability to make trade-off decisions under budget constraints.

**Columns:**
- `strategy` — Name of the evaluation strategy
- `eval_type` — Underlying evaluation method(s)
- `cost_per_1000_runs` — Dollar cost to evaluate 1,000 conversations
- `coverage_pct` — Percentage of total failure types covered
- `failure_modes_covered` — Which specific failure modes this strategy catches
- `latency_ms` — Time to get evaluation results (86400000 ms = 24 hours for human review)
- `human_review_needed` — Whether this strategy requires human involvement

**Baked-in patterns:**
- code_only is free but catches only 35% of failures. Lacks nuance.
- llm_judge_only catches 72% at $25 — reasonable coverage for the cost.
- human_only catches 95% but costs $2,500 — 100x more expensive than LLM judge.
- code_plus_llm gives 78% coverage at same $25 cost as llm_judge_only — code rules are essentially free to add.
- code_plus_llm_plus_human gives 94% coverage at $52.50 — best balance for production.
- llm_plus_human gives 96% coverage at $127.50 — diminishing returns. The extra 2% costs 2.4x more.

**Exercise scenario note:** With a $500/month budget and ~8,000 conversations/month, the math forces trade-offs. code_plus_llm costs $25 × 8 = $200/month, leaving $300 for human spot-checking. Human_only would cost $2,500 × 8 = $20,000/month — impossible under budget.

---

## launch-timeseries.csv

**Purpose:** Learners analyze real post-launch data to distinguish novelty effects from sustainable behavior change. Tests ability to use DiD, identify confounds, and draw honest conclusions.

**Columns:**
- `day` — Days since launch (1-42)
- `date` — Calendar date
- `conversation_starts` — Number of conversations started that day
- `second_conversation_rate` — % of users who completed a conversation and returned for another within 7 days
- `active_users_7d` — 7-day rolling active users
- `quality_score` — Average LLM judge PASS rate across conversations
- `support_tickets` — Number of support tickets filed related to Speak
- `marketing_active` — Whether a marketing campaign promoting the new tab was active (TRUE/FALSE)

**Baked-in patterns:**
- Days 1-3: Pre-launch baseline. conversation_starts = 0 (feature not yet live).
- Days 4-10: Launch + marketing campaign. Spike to 8,120, then decline begins. Marketing is a confound — the spike is partly novelty, partly marketing push.
- Days 11-42: Marketing ends. conversation_starts decays from ~4,880 to ~1,600. About 60% decay from peak.
- second_conversation_rate: Flat at ~0.10-0.12 throughout. This is the key insight — even during the spike, return rate didn't improve. The spike was trial, not behavior change.
- active_users_7d: Rises from ~14,230 (baseline) to ~17,480 (peak at day 7), then decays back to ~14,150 by day 42. Temporary bump, back to baseline.
- quality_score: Stable at 0.71-0.74. No launch effect; no degradation.
- support_tickets: Spikes during marketing campaign (days 4-10), then settles. Expected — more users = more tickets.
- The daily jitter in conversation_starts (ups and downs within the overall decay) is intentional. Real data isn't smooth.

**Analysis guidance:** The marketing campaign (days 4-10) is a confound. To estimate the true treatment effect, compare days 11-42 (post-marketing) to the baseline (days 1-3 projected forward). The gap between the projected baseline (~0-500) and actual post-marketing levels (~1,500-2,000) is the real effect. Everything above that was marketing + novelty.

---

## ship-signals.csv

**Purpose:** Learners make a ship/hold/conditional decision based on conflicting signals from multiple stakeholders. Tests decision-making under ambiguity.

**Columns:**
- `metric` — Metric name
- `current_value` — Current value
- `threshold` — Target threshold for this metric
- `direction` — Whether current value is above or below threshold
- `is_guardrail` — Whether this is a guardrail metric (failure = immediate action)
- `trend` — Direction of change (improving, declining, stable, flat)
- `severity` — How concerning the gap is (low, medium, high)

**Baked-in patterns:**
- All 3 guardrails are green: safety_incidents (0/0), language_coverage (5/5), regulatory_compliance (100/100). Safety says "ship."
- Among optimization metrics, 7 of 9 are below threshold. Product says "hold."
- The key tensions:
  - second_conversation_rate is 0.11 vs 0.20 threshold — 45% below target. High severity. Flat trend.
  - b1_plus_quality is 0.64 vs 0.80 threshold — 20% below target. High severity. Flat trend.
  - daily_active_speakers is 1,680 vs 3,000 — 44% below target. High severity. Declining.
  - support_tickets weekly 35 vs threshold 20 — 75% above threshold. Medium severity. Increasing.
- The pattern forces a conditional ship: guardrails are green but product metrics are far below thresholds with no improving trend. The right answer synthesizes these into a conditional ship with explicit criteria.
- Note the asymmetry: some metrics are below threshold but stable (quality_score), while others are below and declining (daily_active_speakers). This makes the conditional criteria more nuanced — different metrics need different targets.

---

## hackathon-scoping.md

**Purpose:** Learners design a hackathon to validate their riskiest assumption. The template forces specificity: one hypothesis, one success criterion, everything else is explicitly excluded.

**Key design choices in the template:**
- Forces the learner to name what they WON'T build (most teams under-scope hackathons)
- Success criteria must be observable (what the user says, does, or feels) — not a metric
- The critical unknown question forces honesty about what the hackathon can't answer
- The reflection section connects back to the Babbel Speak story

---

## derisking-template.md

**Purpose:** Learners design cheap experiments following the "working with water" framework. Tests ability to scope experiments tightly, set pass/fail thresholds, and make go/no-go decisions.

**Key design choices in the template:**
- Three experiments forces learners to consider multiple dimensions, not just one
- The pass/fail threshold distinction forces clear success criteria for each experiment
- The "what would kill the idea" field forces thinking about worst-case outcomes
- The stop/go section trains the decision framework from Chapter 3
- The reflection connects back to Babbel Speak's capability map
