# Gemma 4 26B A4B — Guardrail Study Results (L0-L4)

Results for `google/gemma-4-26b-a4b-it` on the Independent Interactive Cognitive-Age
Benchmark (NOT ChildAgentEval). This folder mirrors `../ProjectCLG_frontend/` (the Qwen3.5-27B
results), with one difference: **all five conditions, L0-L4, are analysed and plotted
together**. For Qwen, L0-L3 were reported first and L4 separately.

| File | Contents |
|---|---|
| `outcome_brief.html` / `.md` | Plain-language, 12-page brief (open the HTML in a browser) |
| `benchmark_report.md` | Full technical report |
| `final_results_summary.md` | One-page summary, including a Qwen-vs-Gemma comparison |
| `tables.md` | All result tables |
| `reference_vs_ours.md` | Paper reference values vs our results (context only) |
| `figures/` | 10 research figures + `captions.json` |
| `outcome_figures/` | 10 presentation figures used by the brief |
| `outcome_data_summary.json` | Every headline number, machine-readable |
| `reproducibility_manifest.json` | Experiment IDs, git commit, config hash, timestamps |

Raw and processed data, trajectories and the scripts that produced this folder live in
`../childagenteval-research/` (`data/gemma_benchmark/`, `configs/gemma_independent_benchmark.yaml`,
`scripts/analyze_gemma_benchmark.py`, `scripts/generate_gemma_figures.py`,
`scripts/build_gemma_summary.py`).
