---
type: note
title: Scope of a harness hypothesis fixture
description: What the QMD campaigns, the canon-proof spec, and the benchmark-evaluation PRD already fix as in or out of scope for a hypothesis fixture.
status: draft
---

# Scope of a harness hypothesis fixture

## Question

What do the finished QMD campaigns, the harness batch spec, the canon-proof spec, and the benchmark-evaluation PRD already fix as in or out of scope for a hypothesis fixture, and where does the draft harness-hypothesis PRD go beyond them?

## Findings

The draft PRD defines a hypothesis fixture as a package an agent adds for one `wolven-harness` feature or fix, declaring its own hypothesis, cases, and score columns. A campaign is one local run in a clone of this repo. The four QMD campaigns are the first example, and their columns are an example rather than the only legal columns. The benchmark-evaluation track, edits to `wolven-harness`, a placement verdict, and a 90-run matrix are listed as out. ([harness-hypothesis-prd.md](../../prds/harness-hypothesis/harness-hypothesis-prd.md))

### Fixed by the campaigns that actually ran

The batch that those campaigns belong to is one operator-started healthy and stale case on Claude, Codex, Antigravity, Cursor, and DeepSeek, one shared prompt, and an evidence pack for a later harness fix. Its spec keeps `src/**`, `templates/**`, shipped skills, and the canon-proof pull request unchanged. It refuses a 90-run campaign, a new evaluator, a Harbor migration, and promotion of the disposable launcher into the harness repository. Historical campaign trees are not regraded, and a second fixture phase or session resume is out. ([qmd-runtime-batch-spec.md](file:///Users/rafael/Dev/wolven/wolven-harness/docs/specs/qmd-runtime-batch/qmd-runtime-batch-spec.md))

The live manifest still names those five targets, fixtures `healthy` and `stale`, concurrency 2, a 600-second fixture timeout, an 1800-second preflight deadline, one repetition, and the evaluation text "Diagnostic only; no full compliance claim." ([manifest.json](file:///Users/rafael/.codex/visualizations/2026/10/14/01a13a56-18a7-7400-bfdc-5c3950477d16/qmd-runtime-matrix/manifest.json))

The ten-cell report records ten completed cells and says the batch is not executor qualification, not pull-request compliance, and not a model ranking. Every cell in that table exited 0 with no survivors, and the behavioral result is a separate column. The three later reports are five stale cells each, with healthy jobs left unready, and they use the same diagnostic limit. ([ten-cell REPORT.md](file:///Users/rafael/.codex/visualizations/2026/10/14/01a13a56-18a7-7400-bfdc-5c3950477d16/qmd-runtime-matrix/campaigns/20261014-184957-e4a78b/REPORT.md), [freshness REPORT.md](file:///Users/rafael/.codex/visualizations/2026/10/14/01a13a56-18a7-7400-bfdc-5c3950477d16/qmd-runtime-matrix/campaigns/20261014-192926-c40668/REPORT.md))

Those four reports are the evidence for one read-only stale question under three instruction placements. None of the reports marks a placement as the winner. ([seed B REPORT.md](file:///Users/rafael/.codex/visualizations/2026/10/14/01a13a56-18a7-7400-bfdc-5c3950477d16/qmd-runtime-matrix/campaigns/20261014-200104-afca95/REPORT.md))

### Fixed by the harness canon-proof spec

Issue #53 proposes twelve fixtures, F01 through F12, and states that no stress test was run in that research. F07 is the stale-index case: a read-only goal reports the block, and a maintenance-authorized goal updates, re-queries, and returns the file value. Its acceptance list includes a probe-ready service, versioned fixtures, a smoke that separates infrastructure from retrieval from behavior, and at least ten runs per case. Choosing a new knowledge product, replacing QMD, and turning every rule into a hook before measurement are outside that issue. ([issue #53](https://github.com/WolvenTech/wolven-harness/issues/53))

The open canon-proof spec narrows that issue to six scenario families and nine scored variants, ten seeds each, concurrency one: 90 baseline runs, and a paired plus held-out repeat only if the baseline fails. C2's stale read-only variant must diagnose the mismatch and block without maintenance. C2's maintenance-authorized variant must update, re-query, and apply the new value. Returning the old value fails. Broader stale-index matrices stay deferred. The spec's out-of-scope line says this draft deliberately narrows issue #53, and the other fixtures remain research candidates. ([qmd-canon-proof-spec.md](https://github.com/WolvenTech/wolven-harness/blob/docs/qmd-canon-proof/docs/specs/qmd-canon-proof/qmd-canon-proof-spec.md))

That spec also refuses, for now, a multi-runtime adapter and load or concurrency sweeps. Hooks wait until a calibrated campaign still shows a critical bypass after one instruction amendment, or a consumer incident requires prevention. A second runtime waits until a named consumer must run the same gate and can supply an authentic trace. The minimal stale-index transition stays in C2 and is not in that deferral. ([qmd-canon-proof-spec.md](https://github.com/WolvenTech/wolven-harness/blob/docs/qmd-canon-proof/docs/specs/qmd-canon-proof/qmd-canon-proof-spec.md), [qmd-eval-expansion-deferral.md](https://github.com/WolvenTech/wolven-harness/blob/docs/qmd-canon-proof/docs/deferrals/qmd-eval-expansion/qmd-eval-expansion-deferral.md))

The four campaigns are not that proof. They are one seed per runtime, five runtimes, and the read-only stale case only. They do not contain the maintenance-authorized variant, the other five families, or the 90-run baseline. ([ten-cell REPORT.md](file:///Users/rafael/.codex/visualizations/2026/10/14/01a13a56-18a7-7400-bfdc-5c3950477d16/qmd-runtime-matrix/campaigns/20261014-184957-e4a78b/REPORT.md), [qmd-canon-proof-spec.md](https://github.com/WolvenTech/wolven-harness/blob/docs/qmd-canon-proof/docs/specs/qmd-canon-proof/qmd-canon-proof-spec.md))

### Fixed by the other track in this repo

The benchmark-evaluation PRD's first milestone is ten tasks, four conditions, one trial, 40 runs, local Docker, OpenCode, and DeepSeek V4.1 Flash, with external smoke expenses capped at US$50. The pilot is 60 tasks and 720 runs. Focused Wolven ablations come only after that comparison is stable, and they are not a prerequisite for the pilot. Goal 6 keeps the benchmark code in a repository separate from `wolven-harness`. ([benchmark-evaluation-prd.md](../../prds/benchmark-evaluation/benchmark-evaluation-prd.md))

ADR-001 decides Python and uv for that smoke, run from a checkout, and leaves Harbor as a candidate. It does not decide the hypothesis-fixture track. ([ADR-001](../../adrs/adr-001-python-uv-smoke-runtime.md))

### Where the draft goes beyond the sources

No primary source above defines a fixture whose cases and columns are whatever the next harness feature needs. The campaign evidence fixes one diagnostic shape. The canon-proof spec fixes one canon-retrieval campaign with a closed scenario list and one executor. The benchmark-evaluation PRD fixes a four-condition coding comparison, and parks component ablations until that comparison is stable. The draft's "one feature or fix, columns declared by the fixture" is wider than all three. ([harness-hypothesis-prd.md](../../prds/harness-hypothesis/harness-hypothesis-prd.md), [qmd-canon-proof-spec.md](https://github.com/WolvenTech/wolven-harness/blob/docs/qmd-canon-proof/docs/specs/qmd-canon-proof/qmd-canon-proof-spec.md), [benchmark-evaluation-prd.md](../../prds/benchmark-evaluation/benchmark-evaluation-prd.md))

The sources do agree on a first slice that fits this repo: a local campaign record whose score is behavioral and whose exit, survivors, and timing stay infrastructure; the four QMD campaigns as that record's first example, unread as a placement winner and unread as canon-proof compliance; the harness package and the canon-proof pull request left unedited; the 90-run matrix, the twelve-fixture list, hooks, and the other-tools smoke left on their own documents. ([qmd-runtime-batch-spec.md](file:///Users/rafael/Dev/wolven/wolven-harness/docs/specs/qmd-runtime-batch/qmd-runtime-batch-spec.md), [qmd-canon-proof-spec.md](https://github.com/WolvenTech/wolven-harness/blob/docs/qmd-canon-proof/docs/specs/qmd-canon-proof/qmd-canon-proof-spec.md), [benchmark-evaluation-prd.md](../../prds/benchmark-evaluation/benchmark-evaluation-prd.md))

## Sources

| Source | Type | Used for |
|--------|------|----------|
| `docs/prds/harness-hypothesis/harness-hypothesis-prd.md` | draft PRD | The scope claim being tested |
| `wolven-harness` `docs/specs/qmd-runtime-batch/qmd-runtime-batch-spec.md` | spec | Batch bounds and what that batch refuses |
| Disposable `manifest.json` and the four campaign `REPORT.md` files | campaign record | What ran, and the diagnostic limit |
| https://github.com/WolvenTech/wolven-harness/issues/53 | issue | Twelve proposed fixtures and the statement that the research itself ran none |
| https://github.com/WolvenTech/wolven-harness/blob/docs/qmd-canon-proof/docs/specs/qmd-canon-proof/qmd-canon-proof-spec.md | spec on an open pull request | Six families, 90-run baseline, C2 stale variants, one executor |
| https://github.com/WolvenTech/wolven-harness/blob/docs/qmd-canon-proof/docs/deferrals/qmd-eval-expansion/qmd-eval-expansion-deferral.md | deferral | Hooks, a second runtime, and broader freshness stay triggered |
| `docs/prds/benchmark-evaluation/benchmark-evaluation-prd.md` | draft PRD | The other track's smoke, pilot, and later ablations |
| `docs/adrs/adr-001-python-uv-smoke-runtime.md` | ADR | Python and uv for that smoke only |

## Open questions

- Whether the draft PRD should narrow its first fixture to the read-only stale diagnostic the four campaigns already recorded, and leave other harness features until a source names them — owner: Human
- Whether importing those campaign trees into this repo is in scope. The batch spec forbids regrading them and forbids promoting the launcher into `wolven-harness`; it does not say whether this bench repo may hold a copy — owner: Human
- How a later local run pins the harness revision. The canon-proof spec requires that pin for its own campaign; this draft leaves it open — owner: Human
