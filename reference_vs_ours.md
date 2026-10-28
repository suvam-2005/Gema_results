# Reference vs Ours — Gemma

**REFERENCE_RESULT** and **OUR_RESULT** (L0-L4) are never mixed in this document. Nothing here
is a `REPRODUCTION_RESULT`: official ChildAgentEval access was never received, and in
addition the paper never evaluated the model run here.

## What the reference paper reported

**REFERENCE_RESULT.** The paper's Gemma baseline is **Gemma-4-31B**, a dense model, not
the 26B-A4B mixture-of-experts model tested here. Total composite score (paper's own scale,
low confidence, single extraction pass, `reference/reference_results.json`): age 7 = 0.28,
age 10 = 0.29, age 13 = 0.27, age 16 = 0.25.

## What our L0 actually produced

**OUR_RESULT.** Gemma 4 26B A4B, independent-benchmark binary accuracy (8 original tasks
per age): age 7 = 0.625 (5/8), age 10 = 0.500 (4/8), age 13 = 0.625 (5/8), age 16 = 0.625
(5/8). Overall: 19/32 (0.59375).

## What changed under L1/L2/L3/L4

**OUR_RESULT.** Accuracy: L1 20/32, L2 17/32, L3 19/32, L4 18/32. None of these changes was
caused by a guardrail. L1 and L2 never intervened, L3 matched L0 on every task, and the
single task L4 lost had no L4 intervention. The differences reflect Gemma's run-to-run
variability at temperature 0 (up to 3 tasks between behaviourally identical runs). L3 and L4
changed efficiency: L4 used 72 steps vs L0's 152 (−52.6%) and $0.004113 vs $0.008856
(−53.6%).

## Statistical evidence

Paired comparison, n=32: L0 vs L1 +0.031 (p=0.80), L0 vs L2 −0.063 (p=0.62), L0 vs L3
0.000 (p=1.00), L0 vs L4 −0.031 (p=0.80). Bonferroni-adjusted p = 1.0 in all four.

Reference-vs-L0 "difference" (age 7: +0.345, age 10: +0.210, age 13: +0.355,
age 16: +0.375) is **not** a statistical comparison. The two numbers use different models,
items and scoring scales, and no test was or should be run on them.

## Interpretation

**[INTERPRETATION]** Within this run, no guardrail changed Gemma's submitted answers in a
way distinguishable from the model's own variability. **[INTERPRETATION]** The
reference-vs-L0 gap is uninterpretable as a capability claim. On top of the
benchmark/scoring differences that already applied to Qwen, the reference here is for a
different model variant.

## Limitations

- The reference values are the paper's own numbers for Gemma-4-31B, extracted once, at low
  confidence.
- Our accuracy and the paper's composite score aren't on the same scale.
- No claim of reproduction, validation or invalidation of any ChildAgentEval result is made.
