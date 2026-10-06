---
type: note
title: Harness initialization 2026-10-06
description: Lean development-harness initialization with approved full entry integration and recorded deferrals.
status: stable
---

# Harness initialization 2026-10-06

## Entry integration

Recommended full integration because no AGENTS.md exists. Human selected A: full. Diff shown before writing; copy router body into AGENTS.md and remove WOLVEN.md. Human approved adding draft PRD frontmatter without moving or changing its body; diff shown and change applied. Human selected A to replace the external architecture source link with a verified pinned GitHub blob URL; content provenance preserved without asserting a local decision. Diff shown before writing. Entry validation then passed with no warnings; Human approved the entry phase local commit; no push authorized. The installed code-commit skill is used; pragmatic-guard was not consulted because it is not loaded in the runtime session skill list. No existing AGENTS.md or CLAUDE.md to preserve, no overlapping router, and no ignored harness paths reported.

## ADR migration

No legacy warnings reported; migration not needed.

## Discovery

File-based discovery: repository contains PRD/spec, local jury review, temporary Wolven 0.3.1 development dependency, pnpm lockfile, eighteen installed skills, three rules, and the seeded architecture-recording decision. No runner, tests, README, or CI exists. Confirmed evaluation tools are Codex CLI, OpenSpec, and local Docker; Harbor remains a preference subject to compatibility. QMD collection configuration exists but the qmd executable is unavailable; used repository text search. Lifecycle questions deferred, not inferred.

## Research

Repo-only; optional web research deferred by Human.

## Suggestions

Proposed benchmark-isolation and codex-openspec-evaluation skills, citing the confirmed PRD/spec isolation and runtime decisions. Human selected D: neither.

## Stubs

None selected; no additional stubs written.

## Harness score

Initial score attempt could not run. Human approved installing harness-score 1.6.5. Scoring then reported L2 Guided, 43/108 (40%). No rules changed: after score remains L2 Guided, 43/108 (40%). No credential-leak checks failed. Twenty-one gaps remain: CTX-07; SKL-03; AGT-01, AGT-02; HKS-01 through HKS-05; SNS-01 through SNS-05; CI-01 through CI-04; HYG-02, HYG-05, HYG-08. Per-dimension questions were deferred; no checks dropped or gaps built. This is development-harness maturity, not evaluation performance.

## Validate wiring

Deferred; nothing written beyond existing setup scripts.

## Deferred / skipped steps

**Mode:** lean

**Immediate goal:** "run $code-spec on it"; development harness is temporary and will be pruned before benchmark freeze.

| Step | Name | Reason | Remaining work |
| --- | --- | --- | --- |
| 2 extras | Deep discovery questions | Thin code evidence; Human approved deferral | Ask lifecycle and undocumented decisions when needed |
| 3 | Optional web research | Human approved repo-only path | Research decided tools later if requested |
| 6 part | Score-gap keep/drop questions | Human approved deferral | Record score and leave gaps open |
| 6 part | Validate-wiring question | Human approved deferral | Choose CI, chaining, or local wiring later |

## Next steps for the Human

No stubs to define. Keep score gaps open; choose CI wiring later. QMD executable/index setup remains unavailable. Continue reviewing the draft benchmark spec and resolve its open experiment contracts before planning. Prune the temporary development harness before benchmark freeze and exclude it from evaluation workspaces. Human approved the setup-phase local commit; nothing pushed. The jury review is excluded from this commit. pragmatic-guard was not consulted because it is not loaded in the runtime session skill list. Entry-phase commit: 4c90dbd.
