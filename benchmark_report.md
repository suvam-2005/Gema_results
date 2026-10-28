# Benchmark Report — Independent Interactive Cognitive-Age Benchmark, L0-L4, Gemma 4 26B A4B

**Model:** `google/gemma-4-26b-a4b-it` via OpenRouter. **Conditions:** L0, L1, L2, L3, L4,
all run and analysed together (the Qwen3.5-27B study ran L0-L3 first and L4 later as a
separate comparison; here all five share one pipeline, one set of figures and one set of
tables). **This is NOT ChildAgentEval** — see §4. Every number in this report is read from
`data/gemma_benchmark/processed/` and `data/gemma_benchmark/statistics/` (produced by
`scripts/analyze_gemma_benchmark.py`), and every figure from `scripts/generate_gemma_figures.py`.

Run date: 2026-10-28. Config: `configs/gemma_independent_benchmark.yaml`.

## 1. Abstract

The same 32-task, 8-category, 4-age benchmark and the same five guardrail conditions used
for Qwen3.5-27B were run on Gemma 4 26B A4B (one run per condition, temperature 0). Accuracy
ranged from 17/32 to 20/32 across conditions (L0 19, L1 20, L2 17, L3 19, L4 18) and
alignment from 0.51875 to 0.534375. **None of these differences is attributable to a
guardrail.** L1 and L2 never intervened, so they are behaviourally identical to L0, yet they
still differed from L0 on 1 and 2 tasks. That spread is Gemma's run-to-run
non-determinism at temperature 0, and it is the same size as every other difference
observed. The one task L4 lost relative to L0 (`reasoning_age13_v1`) had zero L4
interventions. As with Qwen, the guardrails' real effect was on efficiency: L3 cut steps by
5.9% and cost by 7.2%, and L4 cut steps by 52.6% and cost by 53.6% relative to L0 (49.7% /
50.0% relative to L3). No paired comparison approached significance (all p ≥ 0.62;
Bonferroni-adjusted p = 1.0). Total spend for all five conditions: $0.0394.

## 2. Research question

Do external guardrails (L1 output, L2 action, L3/L4 trajectory-level) change the task
accuracy or cognitive-age alignment of an interactive browser agent relative to an
unguarded control (L0), and do the Qwen3.5-27B findings carry over to a second,
architecturally different open-weight model (a 26B-parameter mixture-of-experts model with
~4B active parameters)?

## 3. Reference study

*Evaluating Cognitive Age Alignment in Interactive AI Agents* (arXiv:2605.17894,
ChildAgentEval). The paper's open-weight baselines are Qwen-3.5-27B and **Gemma-4-31B**. It
did **not** evaluate Gemma 4 26B A4B, so there is no reference result for the model run here.
Gemma-4-31B's reported baseline composite scores (age 7: 0.28, 10: 0.29, 13: 0.27,
16: 0.25) are low confidence (single extraction pass, `reference/reference_results.json`)
and are shown for context only (§14).

## 4. Why an independent benchmark was necessary

Unchanged from the Qwen study: the official ChildAgentEval repository contains no tasks,
scoring code or data; the evaluation is administered by the authors on request because the
items derive from copyrighted WISC materials (`docs/phase0_findings.md`). This study uses the
project's own original benchmark (`docs/independent_benchmark_specification.md`).

## 5. Benchmark methodology

Identical to the Qwen run: real headless Chromium (Playwright), DOM-assisted observations
(accessibility tree + list of interactive elements), generic click/type/select/scroll/
keypress/wait/finish actions, binary scoring done after the episode against
`success_condition` (never shown to the agent), and the project's own linear age-alignment
formula (target 0.35 at age 7 → 0.95 at age 16, Euclidean distance).

## 6. Task design

The same 32 original tasks: 8 categories × ages 7/10/13/16 × 1 task. Task files are unchanged
since they were first committed (`git log -- benchmark/tasks`: one commit, `c5ced43`; clean
working tree). **Same-benchmark check:** the step-0 observation (accessibility tree and
element list) Gemma received was byte-identical to what Qwen received on all 32 of 32 tasks.
SHA-256 of the 32 task files concatenated in sorted path order:
`31ccc0b035de3fde4483f1ff6609e5de3a59293d7f908e47ba8122c4f323748c`. This does **not** match the
fingerprint recorded in the Qwen freeze (`4e6ae675…`). That value was hand-entered, and the
method used to compute it isn't in the repository. Since the files haven't changed since their
only commit, the mismatch is a difference in hashing method, not in content; the
observation-identity check above is the direct evidence.

## 7. Model configuration

| field | value |
|---|---|
| model | `google/gemma-4-26b-a4b-it` (paid endpoint, not `:free`) |
| provider | OpenRouter |
| temperature | 0 |
| max_output_tokens | 256 |
| response_format | `json_object` |
| reasoning | disabled |
| per-step prompt | identical to Qwen: placeholder age prompt + action-schema instructions + current observation only (no action history) |
| pricing (live, run date) | $0.0765 / M input tokens, $0.255 / M output tokens |

The only differences from the Qwen config are `model`, `run_id`
(`independent_benchmark_v1_gemma4_26b_a4b`), and where results and ledgers are written
(`data/gemma_benchmark/`, `.state/gemma/`). The Qwen data and ledgers were not touched.

## 8. Experimental conditions

| condition | guardrail | version | what it does |
|---|---|---|---|
| L0 | none | — | control |
| L1 | output | L1-v1 | repairs/rejects malformed JSON output |
| L2 | action | L2-v1 | rejects finish-at-step-0 and targets not on the page |
| L3 | dynamic | L3-v1 | repeat → substitute `wait`; ≥5 steps → force `finish` |
| L4 | dynamic | L4-v2 | ≥5 steps → force `finish`; repeat → force `finish` immediately |

Conditions were run serially, L0 → L4, each over all 32 tasks, with the same task order.

## 9. Budget

Gemma spend, all five conditions: **$0.039367**. That includes a 2-task L0 smoke test
($0.000619, `data/gemma_benchmark/smoke/`) that was charged to the Gemma L0 ledger before the
full run. That is 0.5% of the $8.00 Gemma benchmark cap. Whole-project lifetime spend after this
run: $0.196188 over 1,673 API calls (of the $10.00 hard cap).

## 10. L0 results (no guardrail)

19/32 correct (59.375%). Alignment 0.534375. 152 steps, 110,330 tokens, $0.008856. Zero
malformed-output errors.

## 11. L1 results (output guardrail)

20/32 (62.5%). Alignment 0.525. **0 interventions.** 152 steps, 109,398 tokens, $0.008784.
Differs from L0 on one task (`visual_spatial_age7_v1`, incorrect → correct). Because L1
never fired, this difference cannot have come from L1.

## 12. L2 results (action guardrail)

17/32 (53.125%). Alignment 0.528125. **0 interventions.** 152 steps, 109,253 tokens,
$0.008773. Differs from L0 on two tasks (`integrated_age7_v1`, `reasoning_age13_v1`, both
correct → incorrect). Again with zero interventions, so not caused by L2.

## 13. L3 and L4 results (trajectory guardrails)

**L3:** 19/32 (59.375%), alignment 0.534375, identical per-task outcomes to L0 on all 32
tasks. 64 interventions (57 repeated-action → `wait`, 7 excessive-steps → `finish`; 44.8% of
143 steps). 143 steps (−5.9%), 102,369 tokens, $0.008221 (−7.2%).

**L4:** 18/32 (56.25%), alignment 0.51875. 30 interventions, all repeated-action →
immediate `finish` (41.7% of 72 steps). 72 steps (−52.6% vs L0, −49.7% vs L3), 51,216 tokens
(−50.0% vs L3), $0.004113 (−53.6% vs L0, −50.0% vs L3). Differs from L0 and L3 on one task,
`reasoning_age13_v1` (correct → incorrect). Its L4 trajectory was `c, b, c, b`: the model
never proposed the same action twice in a row, so L4 never fired on that task. The loss is
model variability, not an L4 effect.

## 14. Reference vs L0

| age | reference (Gemma-4-31B, paper) | our L0 (Gemma 4 26B A4B) |
|---:|---:|---:|
| 7 | 0.28 | 0.625 (5/8) |
| 10 | 0.29 | 0.500 (4/8) |
| 13 | 0.27 | 0.625 (5/8) |
| 16 | 0.25 | 0.625 (5/8) |

Not a comparison in any statistical sense: different model variant (31B dense vs 26B MoE),
different items, different scoring scale, and a low-confidence extraction of the reference
values. Figures 1-2 and outcome figures 07-08 show them side by side for context only.

## 15. Alignment analysis

Alignment: L0 0.534375, L1 0.525, L2 0.528125, L3 0.534375, L4 0.51875 (Figure 3). The
range (0.0156) comes entirely from the 1-3 task-level differences in §11-13, all of which
are attributable to run-to-run variability (§21). No condition moved alignment in a way
that can be separated from that noise.

## 16. Capability analysis

Accuracy: L0 0.59375, L1 0.625, L2 0.53125, L3 0.59375, L4 0.5625 (Figure 4; SEM of binary
scores ≈ 0.088 per condition, n=32). Every condition's mean lies within about one SEM of
L0's.

## 17. Alignment-capability tradeoff

Capability retention vs L0: L1 1.053, L2 0.895, L3 1.000, L4 0.947 (Figure 5; L0 and L3
coincide exactly). There is no tradeoff to report: the points scatter in both directions
around L0, consistent with noise rather than a systematic guardrail effect.

## 18. Per-age analysis

Correct of 8 per age (Figure 6):

| condition | age 7 | age 10 | age 13 | age 16 |
|---|---:|---:|---:|---:|
| L0 | 5 | 4 | 5 | 5 |
| L1 | 6 | 4 | 5 | 5 |
| L2 | 4 | 4 | 4 | 5 |
| L3 | 5 | 4 | 5 | 5 |
| L4 | 5 | 4 | 4 | 5 |

No age trend, in any condition. Age 10 (4/8) and age 16 (5/8) are identical across all five.

## 19. Per-category analysis

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

Planning and working memory are 0% in every condition, exactly as for Qwen. In all 8 of
those tasks under L0, Gemma clicked the first required element and then re-clicked that
same element on every remaining step (e.g. `wm_age10_v1`: `target_square` × 7). The per-step
prompt contains no action history and the page doesn't visibly record which buttons were
clicked, so the model has no signal that it has already made that click. This is a property
of the harness that both models share. It's not evidence that the model can't do a 4-6 step
sequence.

## 20. Intervention analysis

| condition | interventions | rate | triggers |
|---|---:|---:|---|
| L0 | 0 | 0.0% | — |
| L1 | 0 | 0.0% | — |
| L2 | 0 | 0.0% | — |
| L3 | 64 | 44.8% | repeated_action 57, excessive_steps 7 |
| L4 | 30 | 41.7% | repeated_action 30 |

Same pattern as Qwen (L3 70, L4 31): L1/L2 have nothing to catch (Gemma produced no
malformed output and no invalid targets), and the trajectory guardrails fire on the repeated
clicks described in §19. No discordant task outcome in any condition coincided with an
intervention on that task.

## 21. Error/recovery analysis and run-to-run variability

Malformed-output errors and recoveries: **0 in every condition** (Qwen had 1-2 per condition).

**The key Gemma-specific finding is non-determinism at temperature 0.** On multiple-choice
tasks Qwen picked one answer and re-clicked it for the whole episode. Gemma sometimes
switches answers mid-episode and does so differently from run to run. Examples:
`reasoning_age13_v1` clicked `c c d c` (L0), `c d c c` (L1), `b c c b` (L2), `b c b c` (L3),
`c b c b` (L4); `visual_spatial_age7_v1` clicked `b a a b` (L0), `b a b a` (L1), `b b b b` (L2).
Even the first click differs between runs. In the 2-task smoke test before the full run,
`reasoning_age10_v1` was answered correctly (`c b d c`); in the full L0 run it was answered
incorrectly (`d d c d`). Because scoring uses the **last** click on multiple-choice tasks, this
switching translates directly into outcome changes.

L0, L1 and L2 are effectively three replicate runs of the same unguarded agent (L1/L2 had
zero interventions). Across them, 3 of 32 tasks changed outcome and accuracy ranged from 17
to 20 of 32. **That is the noise floor for a single run of this model on this benchmark**, and
every L0-vs-guardrail difference observed (≤ 2 tasks) falls inside it. 6 of 32 tasks had
Gemma click more than one distinct answer within a single episode under L0
(`data/gemma_benchmark/processed/guardrail_behavior.json`).

One consequence for L3/L4: when the model switches answers, a trajectory guardrail that ends
an episode early *can* change which answer counts as last. That was impossible for Qwen,
whose repeats were always of the same answer. In this run no outcome change was traced to
that mechanism, but for Gemma, L3/L4 aren't guaranteed to leave outcomes unchanged the way
they were for Qwen.

## 22. Token/cost analysis

| condition | input tokens | output tokens | API calls | cost (USD) | Δ cost vs L0 |
|---|---:|---:|---:|---:|---:|
| L0 | 108,003 | 2,327 | 152 | $0.008856 | — |
| L1 | 107,071 | 2,327 | 152 | $0.008784 | −0.8% |
| L2 | 106,926 | 2,327 | 152 | $0.008773 | −0.9% |
| L3 | 100,184 | 2,185 | 143 | $0.008221 | −7.2% |
| L4 | 50,122 | 1,094 | 72 | $0.004113 | −53.6% |

L1/L2's small cost differences come from slightly different prompt lengths, not from
interventions. Gemma L0 cost 62% less than Qwen L0 ($0.008856 vs $0.023440) for the same
152 steps. Latency was ~1.3 s per step (196 s total for L0) vs ~23 s for Qwen.

## 23. Statistical analysis

Paired comparisons over the same 32 tasks (`statistics/comparisons.json`):

| comparison | mean diff | 95% CI | discordant | Welch p | Bonferroni p |
|---|---:|---|---:|---:|---:|
| L0 vs L1 | +0.031 | [−0.030, 0.093] | 1 | 0.801 | 1.000 |
| L0 vs L2 | −0.063 | [−0.148, 0.023] | 2 | 0.619 | 1.000 |
| L0 vs L3 | 0.000 | [0.000, 0.000] | 0 | 1.000 | 1.000 |
| L0 vs L4 | −0.031 | [−0.093, 0.030] | 1 | 0.803 | 1.000 |
| L3 vs L4 | −0.031 | [−0.093, 0.030] | 1 | 0.803 | — |

Welch's t-test (normal approximation) is reported for continuity with the Qwen study. With
1-2 discordant pairs no test has meaningful power. An exact McNemar test on 1 or 2
discordant pairs gives p = 1.0 or 0.5. No comparison is significant before or after
correction.

## 24. Limitations

- One run per condition, and unlike Qwen, Gemma's output isn't reproducible run to run at
  temperature 0, so a single run per condition can't separate a small guardrail effect from
  noise. Several seeds/repeats per condition would be needed.
- 32 tasks (4 per category, 8 per age): low power for any category- or age-level claim.
- L1 and L2 never fired: their effect on Gemma is untested, not disproven.
- The benchmark is original and not psychometrically validated. Planning/working-memory
  scores are dominated by the stateless prompt (§19).
- The reference values are for a different Gemma variant, at low confidence.

## 25. Threats to validity

- **Last-click scoring × answer switching**: for a model that changes its mind
  mid-episode, measured accuracy depends partly on *when* the episode stops, which is
  exactly what L3/L4 change (§21).
- **Provider routing**: OpenRouter may route requests to different backends, which is a
  plausible source of the observed non-determinism. Not verified.
- **Persona prompt**: the placeholder prompt asks the model to act like a person of the
  target age, which may itself induce deliberate errors; this is shared with the Qwen run.

## 26. Reproducibility

```bash
python scripts/run_independent_benchmark.py --config configs/gemma_independent_benchmark.yaml --condition L0   # then L1..L4
python scripts/analyze_gemma_benchmark.py
python scripts/generate_gemma_figures.py
python scripts/build_gemma_summary.py --tests-passed 297 --tests-total 297
```

`reproducibility_manifest.json` lists the experiment IDs, git commit, config hash and
timestamps per condition. The full per-step trajectories are in
`data/gemma_benchmark/raw/trajectories/{L0..L4}/`. Test suite: 297/297 passed (all mocked;
no API calls).

## 27. Conclusion

On Gemma 4 26B A4B, as on Qwen3.5-27B, no guardrail from L1 to L4 produced an accuracy or
alignment change distinguishable from noise, and L4 again roughly halved the steps and cost.
What's new with Gemma is the noise itself. Qwen's outcomes were perfectly reproducible
across conditions; Gemma's vary by up to 3 tasks between runs that are behaviourally
identical. Any future guardrail claim on this model needs repeated runs per condition to
be credible.
