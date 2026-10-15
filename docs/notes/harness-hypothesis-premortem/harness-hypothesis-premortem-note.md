---
type: note
title: Pre-mortem of a weakness-finding harness test
description: Four ways a hypothesis fixture can ship, look rigorous, and still miss the harness weaknesses that matter.
status: draft
---

# Pre-mortem of a weakness-finding harness test

## Question

If a hypothesis fixture in this repo is treated as a real test of harness weaknesses, which failures already have a mechanism in the draft and in published agent evaluations?

## Findings

The position under test is that an agent clones this repo, runs a fixture for a harness change, and receives a score and a detailed report that expose where the harness fails. The four QMD campaigns and the canon-proof spec are the floor. ([harness-hypothesis-prd.md](../../prds/harness-hypothesis/harness-hypothesis-prd.md), [harness-hypothesis-scope-note.md](../harness-hypothesis-scope/harness-hypothesis-scope-note.md))

- The draft lets the same agent declare the hypothesis, the cases, and the score columns. A column that matches behavior the change already produces will pass. The canon-proof spec instead requires the scorer to reject deliberate bypasses, including a correct answer with no QMD call. ([harness-hypothesis-prd.md](../../prds/harness-hypothesis/harness-hypothesis-prd.md), [qmd-canon-proof-spec.md](https://github.com/WolvenTech/wolven-harness/blob/docs/qmd-canon-proof/docs/specs/qmd-canon-proof/qmd-canon-proof-spec.md))
- On the ten-cell batch, Claude's healthy answer was the on-disk number with no executed `qmd` command. A column that only checks the number would record a pass. ([ten-cell REPORT.md](file:///Users/rafael/.codex/visualizations/2026/10/14/01a13a56-18a7-7400-bfdc-5c3950477d16/qmd-runtime-matrix/campaigns/20261014-184957-e4a78b/REPORT.md))
- Published coding-agent scores already count plausible failures as passes. Re-running SWE-bench Verified patches against the rest of the developer tests found 7.8% of plausible patches incorrect. Differential tests separated 29.6% of plausible patches from the oracle patch. A later augmentation marked 176 SWE-bench Lite patches and 169 SWE-bench Verified patches as wrongly passed. ([Wang et al.](https://arxiv.org/html/2503.15223v2), [UTBoost](https://arxiv.org/html/2506.09289v1))
- One cell is the whole sample in each of the four campaigns. Ten independent runs of the same agent configuration on SWE-bench Verified moved single-run pass@1 by 2.2 to 6.0 percentage points, with standard deviation above 1.5 points even at temperature 0. The paper says a 2–3 point gain can be evaluation noise. ([On Randomness in Agentic Evals](https://arxiv.org/html/2602.07150v2))
- The freshness report's Cursor row states the indexed number, and the disagreement wording appears only inside the opened skill and rule. A scorer that searches the trace for those words would treat the instruction text as the agent's conclusion. OpenAI's audit of 138 SWE-bench Verified problems found material test or statement flaws in 59.4% of them, which is the same shape of failure: the check rejects or accepts the wrong thing. ([freshness REPORT.md](file:///Users/rafael/.codex/visualizations/2026/10/14/01a13a56-18a7-7400-bfdc-5c3950477d16/qmd-runtime-matrix/campaigns/20261014-192926-c40668/REPORT.md), [OpenAI](https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/))
- A review of benchmark practice states Goodhart's law as the reason a measure stops working once it is the target. The canon-proof spec keeps the oracle outside the agent and forbids the goal from saying to use QMD. The draft's score columns are chosen by the agent that is being measured. ([Can We Trust AI Benchmarks?](https://arxiv.org/html/2502.06559v2), [qmd-canon-proof-spec.md](https://github.com/WolvenTech/wolven-harness/blob/docs/qmd-canon-proof/docs/specs/qmd-canon-proof/qmd-canon-proof-spec.md))

What would guarantee the miss: the measured agent writes the expected cells, one seed is the verdict, the score is a substring of the trace, and there is no bypass the author did not already believe. Those conditions are already in the draft and in the four reports.

## Sources

| Source | Type | Used for |
|--------|------|----------|
| `docs/prds/harness-hypothesis/harness-hypothesis-prd.md` | draft PRD | Who declares the columns |
| `docs/notes/harness-hypothesis-scope/harness-hypothesis-scope-note.md` | draft note | The floor this pre-mortem starts from |
| Campaign `REPORT.md` files for `20261014-184957-e4a78b` and `20261014-192926-c40668` | campaign record | Correct number without QMD; disagreement words inside the instruction files |
| https://github.com/WolvenTech/wolven-harness/blob/docs/qmd-canon-proof/docs/specs/qmd-canon-proof/qmd-canon-proof-spec.md | spec | Negative controls and blind goals |
| https://arxiv.org/html/2503.15223v2 | paper | Plausible patches that fail a wider test |
| https://arxiv.org/html/2506.09289v1 | paper | Patches marked passed and later withdrawn |
| https://arxiv.org/html/2602.07150v2 | paper | Single-run variance at temperature 0 |
| https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/ | vendor report | Flawed checks in a filtered benchmark |
| https://arxiv.org/html/2502.06559v2 | review | Goodhart's law applied to benchmark scores |

## Open questions

- Which of these four failures the Human wants the fixture to close before the PRD is approved — owner: Human
- Whether ten seeds, a bypass suite, and an oracle the agent did not write are in the first version — owner: Human
