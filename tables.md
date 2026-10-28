# Publication Tables — Gemma 4 26B A4B, L0-L4

All values are read from `data/gemma_benchmark/processed/` and
`data/gemma_benchmark/statistics/`, none hand-typed. All five conditions are reported in
one table set (the Qwen tables covered L0-L3, with L4 reported separately).

## Table 1 — Model and configuration

| model | provider | temperature | max_output_tokens | response_format | reasoning |
|---|---|---:|---:|---|---|
| google/gemma-4-26b-a4b-it | openrouter | 0 | 256 | json_object | disabled |

## Table 2 — Reference methodology vs this run

Official ChildAgentEval access remains unavailable; this run used the Independent
Interactive Cognitive-Age Benchmark (`docs/independent_benchmark_specification.md`),
identical tasks and harness to the Qwen run.

## Table 3 — Reference results (Gemma-4-31B, paper-reported; NOT the model run here)

| age | total_composite_score | confidence | status |
|---:|---:|---|---|
| 7 | 0.28 | low | unverified_single_pass |
| 10 | 0.29 | low | unverified_single_pass |
| 13 | 0.27 | low | unverified_single_pass |
| 16 | 0.25 | low | unverified_single_pass |

Source: `reference/reference_results.json`.

## Table 4 — Independent-benchmark results (L0-L4)

| condition | n | n_correct | mean_accuracy | alignment | identical outcomes to L0 |
|---|---:|---:|---:|---:|---:|
| L0 | 32 | 19 | 0.59375 | 0.534375 | 32/32 |
| L1 | 32 | 20 | 0.62500 | 0.525000 | 31/32 |
| L2 | 32 | 17 | 0.53125 | 0.528125 | 30/32 |
| L3 | 32 | 19 | 0.59375 | 0.534375 | 32/32 |
| L4 | 32 | 18 | 0.56250 | 0.518750 | 31/32 |

## Table 5 — Paired comparisons (same 32 tasks)

| comparison | task_performance_diff | discordant | incorrect→correct | correct→incorrect |
|---|---:|---:|---|---|
| L0 vs L1 | +0.03125 | 1 | visual_spatial_age7_v1 | — |
| L0 vs L2 | −0.06250 | 2 | — | integrated_age7_v1, reasoning_age13_v1 |
| L0 vs L3 | 0.00000 | 0 | — | — |
| L0 vs L4 | −0.03125 | 1 | — | reasoning_age13_v1 |
| L3 vs L4 | −0.03125 | 1 | — | reasoning_age13_v1 |

None of the discordant tasks had a guardrail intervention in the differing condition.

## Table 6 — Statistical significance / effect sizes

| comparison | n | mean_diff 95% CI | welch_t | p_value | bonferroni_p | cohens_d_paired |
|---|---:|---|---:|---:|---:|---:|
| L0 vs L1 | 32 | [−0.030, 0.093] | 0.252 | 0.801 | 1.000 | 0.177 |
| L0 vs L2 | 32 | [−0.148, 0.023] | −0.497 | 0.619 | 1.000 | −0.254 |
| L0 vs L3 | 32 | [0.000, 0.000] | 0.000 | 1.000 | 1.000 | undefined (zero variance) |
| L0 vs L4 | 32 | [−0.093, 0.030] | −0.249 | 0.803 | 1.000 | −0.177 |
| L3 vs L4 | 32 | [−0.093, 0.030] | −0.249 | 0.803 | — | −0.177 |

## Table 7 — Per-age accuracy (correct of 8)

| condition | age 7 | age 10 | age 13 | age 16 |
|---|---:|---:|---:|---:|
| L0 | 5 | 4 | 5 | 5 |
| L1 | 6 | 4 | 5 | 5 |
| L2 | 4 | 4 | 4 | 5 |
| L3 | 5 | 4 | 5 | 5 |
| L4 | 5 | 4 | 4 | 5 |

## Table 8 — Per-category accuracy (n=4 each)

| category | L0 | L1 | L2 | L3 | L4 |
|---|---:|---:|---:|---:|---:|
| abstraction | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 |
| integrated_reasoning | 1.00 | 1.00 | 0.75 | 1.00 | 1.00 |
| language | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 |
| planning | 0.00 | 0.00 | 0.00 | 0.00 | 0.00 |
| processing_behavior | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 |
| reasoning | 0.50 | 0.50 | 0.25 | 0.50 | 0.25 |
| visual_spatial | 0.25 | 0.50 | 0.25 | 0.25 | 0.25 |
| working_memory | 0.00 | 0.00 | 0.00 | 0.00 | 0.00 |

## Table 9 — Interventions

| condition | interventions | steps | rate | repeated_action | excessive_steps |
|---|---:|---:|---:|---:|---:|
| L0 | 0 | 152 | 0.000 | 0 | 0 |
| L1 | 0 | 152 | 0.000 | 0 | 0 |
| L2 | 0 | 152 | 0.000 | 0 | 0 |
| L3 | 64 | 143 | 0.448 | 57 | 7 |
| L4 | 30 | 72 | 0.417 | 30 | 0 |

## Table 10 — Token/cost analysis

| condition | input_tokens | output_tokens | api_calls | cost_usd | errors | recoveries |
|---|---:|---:|---:|---:|---:|---:|
| L0 | 108,003 | 2,327 | 152 | $0.008856 | 0 | 0 |
| L1 | 107,071 | 2,327 | 152 | $0.008784 | 0 | 0 |
| L2 | 106,926 | 2,327 | 152 | $0.008773 | 0 | 0 |
| L3 | 100,184 | 2,185 | 143 | $0.008221 | 0 | 0 |
| L4 | 50,122 | 1,094 | 72 | $0.004113 | 0 | 0 |

## Table 11 — Failure analysis

Not populated: the per-step failure taxonomy (`evaluation/failure_taxonomy.py`) wasn't run,
same as the Qwen tables. The dominant failure modes are described qualitatively in
`benchmark_report.md` §19 and §21.
