# Final Results Summary — Gemma 4 26B A4B

Independent Interactive Cognitive-Age Benchmark (NOT ChildAgentEval),
`google/gemma-4-26b-a4b-it`, conditions L0-L4, one run each, temperature 0. Full detail:
`benchmark_report.md`.

## Reference results (contextual only, different model variant)

The paper did not test Gemma 4 26B A4B. Its Gemma-4-31B baseline (low confidence):

| Age | Reference total composite score |
|---:|---:|
| 7 | 0.28 |
| 10 | 0.29 |
| 13 | 0.27 |
| 16 | 0.25 |

## Results by condition

| Condition | Correct | Accuracy | Alignment | Interventions | Steps | Tokens | Cost |
|---|---:|---:|---:|---:|---:|---:|---:|
| L0 (no guardrail) | 19/32 | 59.38% | 0.534375 | 0 | 152 | 110,330 | $0.008856 |
| L1 (output) | 20/32 | 62.50% | 0.525000 | 0 | 152 | 109,398 | $0.008784 |
| L2 (action) | 17/32 | 53.13% | 0.528125 | 0 | 152 | 109,253 | $0.008773 |
| L3 (dynamic, wait on repeat) | 19/32 | 59.38% | 0.534375 | 64 (44.8%) | 143 | 102,369 | $0.008221 |
| L4 (dynamic, finish on repeat) | 18/32 | 56.25% | 0.518750 | 30 (41.7%) | 72 | 51,216 | $0.004113 |

## Accuracy and alignment changes

Small differences in both directions (−2 to +1 tasks vs L0), **none attributable to a
guardrail**. L1 and L2 never intervened, so their differences from L0 (1 and 2 tasks) are
pure run-to-run variability. The one task L4 lost (`reasoning_age13_v1`) had zero L4
interventions. L3 matched L0 on all 32 tasks.

## Run-to-run variability (new with Gemma)

Unlike Qwen, Gemma is not reproducible at temperature 0: it sometimes switches answers
mid-episode, and differently on each run. Across the three effectively unguarded runs (L0,
L1, L2), 3 tasks changed outcome and accuracy ranged 17-20/32. That is the noise floor for a
single run, and every observed difference sits inside it.

## Efficiency

L3: −5.9% steps, −7.2% cost vs L0. **L4: −52.6% steps, −53.6% cost vs L0; −49.7% steps,
−50.0% cost vs L3.** Same pattern as Qwen (L4 −53.8% steps vs L3).

## Cost

Gemma benchmark spend: **$0.039367** for all five conditions, including a $0.000619 two-task
smoke test (0.5% of the $8.00 cap). Project lifetime spend: $0.196188 over 1,673 API calls.

## Statistical evidence

Paired, n=32 tasks: L0 vs L1 +0.031 (p=0.80), L0 vs L2 −0.063 (p=0.62), L0 vs L3 0.000
(p=1.00), L0 vs L4 −0.031 (p=0.80), L3 vs L4 −0.031 (p=0.80). Bonferroni-adjusted p = 1.0 for
all. No effect detected.

## Comparison with Qwen3.5-27B (same benchmark, same conditions)

| | Qwen3.5-27B | Gemma 4 26B A4B |
|---|---|---|
| L0 accuracy | 18/32 | 19/32 |
| Accuracy range across L0-L4 | 18-18 (identical every task) | 17-20 |
| Planning / working memory | 0% / 0% | 0% / 0% |
| Reasoning (L0) | 0% | 50% |
| Malformed outputs | 1-2 per condition | 0 |
| L4 steps vs L3 | −53.8% | −49.7% |
| L0 cost | $0.023440 | $0.008856 |

## Limitations

Single run per condition on a model that isn't reproducible at temperature 0; 32 tasks;
L1/L2 untested because they never fired; original, unvalidated benchmark; the reference is
a different Gemma variant. Full list: `benchmark_report.md` §24-25.
