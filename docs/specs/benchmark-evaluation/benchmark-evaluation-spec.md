---
type: spec
title: Benchmark evaluation smoke implementation
description: Requirements and evidence contracts for comparing four Codex CLI workflows locally, with independent grading and a US$50 smoke cap.
status: draft
---

# Benchmark evaluation smoke implementation

**Source:** `docs/prds/benchmark-evaluation/benchmark-evaluation-prd.md`, updated 2026-10-06, GitHub blob `01e4dd8ad6b0c399a4b8f1cb4364e08c320b1fc7`. The PRD still says draft; the confirmed ask is “run $code-spec on it,” following the user's six configuration decisions. This spec does not relabel the PRD approved.
**Next:** Human confirms this draft; `code-plan` slices approved requirements. No executable plan or paid experiment is produced here.
**Named structural proof:** `proof-benchmark-spec-obligations`.

## Repository grounding

| Surface | Present today | Role |
| --- | --- | --- |
| Source PRD | Yes, local and GitHub main | Product requirements and confirmed choices |
| `docs/reviews/benchmark-evaluation-jury.md` | Local only | Advisory review; 4–1 proceed, with operational-protocol dissent |
| Runner, condition adapters, evaluator, tests | No | New implementation; no existing behavior to preserve |
| Package manifest, runtime choice for benchmark code | No | Language and packaging unresolved |
| `AGENTS.md`, skills, standing rules, ADRs in this repository | No | No local policy or architectural decision records to reuse |
| `harness:validate` in this repository | No | Validation wiring unresolved; never report it passed |
| Wolven `code-spec` and `pragmatic-guard` skills | Read from sibling `wolven-harness/templates/.agents/skills/` | Authoring instructions only; not installed in the benchmark repository |
| Harbor integration | Preferred by PRD; not installed or validated here | Compatibility must be demonstrated, not assumed |

The remote root was inspected and contains only `docs/`. The local benchmark directory is a document staging directory, not a Git checkout. Search used `rg` over existing documentation; no repository knowledge-query tool is configured. There are no existing ADRs to resolve these decisions against.

## Term challenge

| Term | Resolution |
| --- | --- |
| Condition | PRD's A default, B lightweight checklist, C Wolven, D OpenSpec; same Codex CLI/model/settings |
| Smoke | Ten tasks, one trial, four conditions: forty intended agent runs; a budget-limited partial experiment is not a completed smoke |
| Trial | Independent task/condition execution; subsequent infrastructure attempts are retries, not extra successful trials |
| Accepted task | Acceptance and regression checks pass, mandatory constraints hold, and execution finishes within budget |
| Run / attempt | A logical task-condition-trial has immutable attempt IDs; retried attempts retain provenance |
| Factual equivalence | All conditions can access the same task-relevant facts; workflow-specific instructions may differ |
| Resume | Runner recovery skips completed attempts; handoff recovery starts a fresh agent conversation with permitted workspace state |
| Hidden checks | Evaluator-owned assets unavailable to the agent; public task exposure is disclosed separately |
| Pilot | Separate sixty-task, three-trial experiment; neither its spending nor launch is authorized by this smoke specification |

## Surface walk

**In scope:** benchmark repository documents, future local runner and its configuration, four workflow adapters, task-package interfaces, protected grading, artifact capture, recovery, budget accounting, and reports. Exact code paths and CLI surface are unresolved until language/integration choices are made; they are not represented as existing paths.

**Out of mutate scope:** `wolven-harness` source; Codex CLI, OpenSpec, and public benchmark upstream code; external accounts and repository visibility; hosted services; authored task content and grading judgments owned by the user's team. This initiative supplies interfaces for team-owned task packages, not authority to invent their final acceptance criteria.

## Design contract

Consume pinned versions of Codex CLI, Wolven, OpenSpec, and any selected public dataset/evaluator. Prefer the existing Harbor agent/environment interfaces if the compatibility check succeeds. No alternative runner architecture or implementation language is selected by this draft.

Keep initialization separate from timed task execution and report its costs separately. Normalize facts across initialized snapshots before freezing them. Preserve workflow-specific tools, instructions, and legitimate task-generated records as the treatment.

A task package declares its starting snapshot, request, facts, constraints, allowed actions, resource limits, reference solution, known-failing controls, protected acceptance/regression checks, reviewer rubric, and provenance. Synthetic fixtures and public tasks are both included; exact counts and IDs are a team decision. Synthetic fixtures supply coverage missing from public tasks across the six PRD families.

Run grading outside the agent workspace in a clean environment. The agent receives neither reference solutions nor evaluator credentials. Network/cache isolation and public-task exposure must be recorded; public availability is not equivalent to demonstrated contamination resistance.

The runner persists logical-run identity, attempt identity, status, timestamps, stop reason, and artifact references. The exact state vocabulary and crash-recovery transition table are unresolved; they must distinguish completed outcomes from interrupted execution and evaluator failure. Never interpret a missing terminal record as success.

## Requirements: obligation ↔ proof

Every proof below is an observable check against a future delivered artifact, not a claim that an implementation or test exists today. Each proof is unique. Executable product-check commands must be added to the implementation contract before wave acceptance; see `U-commands`.

| ID | Obligation | Named proof | Evidence shape |
| --- | --- | --- | --- |
| R01 | Deliver exactly A/B/C/D on the same pinned Codex/model/reasoning configuration; D uses OpenSpec | `proof-benchmark-conditions` | Resolve one task across four setups; inspect versions, shared facts, and condition-specific differences |
| R02 | Reject incomplete or inconsistent manifests before agent dispatch | `proof-benchmark-manifest` | Invalid-version, missing-pin, and inconsistent-budget fixtures dispatch zero agents; archive a valid resolved manifest |
| R03 | User/team-owned ten-task suite includes synthetic and public sources, six-family coverage, provenance, and validated grading | `proof-benchmark-task-packages` | Team-approved inventory; references pass and known-failing controls fail repeatedly under declared validation protocol |
| R04 | Preserve factual equivalence while isolating hidden checks, answers, credentials, and prior trials | `proof-benchmark-isolation` | Snapshot fact inventory plus boundary probes across all four conditions; report denied evaluator access and any permitted public-source exposure |
| R05 | Distinguish pre-initialization/setup evidence and costs from task execution | `proof-benchmark-onboarding` | Initialization ledger and normalized frozen snapshot identities; separate setup and execution subtotals with cap treatment recorded |
| R06 | Execute in fresh local Docker workspaces with equal resources and a reproducible randomized order | `proof-benchmark-scheduling` | Same seed reproduces schedule; contamination probes fail; active runs never exceed declared concurrency |
| R07 | Never exceed the US$50 total smoke spending cap; enforce task limits and count failures, retries, setup, and both handoff phases | `proof-benchmark-budget` | Budget-boundary simulation reserves all potential in-flight costs; unaffordable dispatch is rejected; unknown pricing/metering cannot silently permit spending |
| R08 | Persist distinct completed, unsuccessful, input-blocked, budget-stopped, condition-failed, and infrastructure-failed outcomes without false success | `proof-benchmark-outcomes` | Fault/input/timeout fixtures produce specified terminal records and reason evidence under the approved transition table |
| R09 | Resume interrupted runner execution without duplicate completed runs; retain and link every allowed retry | `proof-benchmark-recovery` | Interrupt/restart and concurrent-resume checks preserve IDs/artifacts; completed attempts are not redispatched; retries obey frozen policy |
| R10 | Apply declared factual-answer and approval rules equally; log interventions and stop unanswerable requests as `needs_input` | `proof-benchmark-human-policy` | Scripted question/approval fixtures record responses without implementation hints; unsupported questions retain blocked outcomes |
| R11 | Handoff tasks enforce declared interruption boundaries, retain only permitted state, drop prior conversation, and account for both phases | `proof-benchmark-handoff` | Team-approved task protocol; inspect retained/discarded state and fresh-session identity, cost, outcome, and rework evidence |
| R12 | Independent protected grading separates functional success from acceptance and checks tests, constraints, budget, and evidence-backed claims | `proof-benchmark-grading` | Reference, regression, constraint-violation, falsified-check-claim, and honest-failure controls receive correct distinct assessments |
| R13 | Quarantine invalid/flaky tasks through a predefined symmetric rule; condition failures are never silently excluded | `proof-benchmark-quarantine` | Inject invalid environment; all conditions receive the same eligibility disposition with original denominator and reason retained |
| R14 | Capture manifests, pins, environment identity, transcript, commands, diff, grading, usage/cost, timing, interactions, stop reasons, and retry links; redact secrets | `proof-benchmark-artifacts` | Audit artifact bundle, credential-redaction control, missing-usage marker, and local storage/Git exclusion |
| R15 | Produce JSON and Markdown with traceable outcomes, exclusions, paired differences, family breakdowns, and complete cost accounting | `proof-benchmark-report` | Deterministic synthetic-result controls verify denominators, failed-attempt cost, zero-success undefined cost, and artifact links; no automatic publication |
| R16 | Preserve task-level pairing in 95% uncertainty estimates; identify C/A primary and C/B,C/D secondary; report unknown or unmeasured quantities honestly | `proof-benchmark-analysis` | Statistical controls retain repeated trials as a task cluster; pins for analysis settings; missing measurements never become zero |
| R17 | A clean-environment smoke reproduction and independent operational audit establish readiness before pilot progression | `proof-benchmark-smoke-gate` | Forty completed logical runs plus approved audits of facts, task validity, grading, handoffs, failures/retries, artifacts, recovery, and actual cost; a partial smoke fails readiness |
| R18 | Freeze pilot threshold, task holdout, analysis policy, versions, and separate approved budget before pilot launch | `proof-benchmark-pilot-boundary` | Pilot admission rejects missing decisions/authorization; smoke report may inform threshold but may not select final tasks by condition wins |

### Budget boundary

The confirmed hard cap takes precedence over the earlier PRD allowance to disclose overshoot. Disclosure alone does not satisfy R07. A strict cap requires credible pre-dispatch upper bounds including active requests; if the selected provider/adapter cannot enforce those bounds, paid dispatch remains blocked under `U-budget-metering`. This is an explicit interpretation for Human confirmation, not an assertion that Codex already exposes suitable metering. Do not assume the US$50 funds forty runs.

### Analysis boundary

A one-trial, ten-task smoke establishes operational feasibility and approximate cost; it does not demonstrate general superiority or adequate statistical power. Report limitations. Multitrial-capable analysis may be validated with deterministic result fixtures before real pilot data exists. Human review minutes remain unmeasured unless reviewers actually record them.

## Nine-dimension landings

| Dimension | Landing kind | Landing |
| --- | --- | --- |
| validation | obligation ↔ proof | R02 / `proof-benchmark-manifest`; R03 / `proof-benchmark-task-packages` |
| failure modes | obligation ↔ proof + Unresolved | R08 / `proof-benchmark-outcomes`; `U-failure-policy` freezes attribution |
| idempotency and retry | obligation ↔ proof + Unresolved | R09 / `proof-benchmark-recovery`; `U-retry-policy` fixes eligible failures and limits |
| authorization | obligation ↔ proof | R10 / `proof-benchmark-human-policy`; R18 / `proof-benchmark-pilot-boundary`; writing this spec authorizes no paid run |
| concurrency and ordering | obligation ↔ proof + Unresolved | R06 / `proof-benchmark-scheduling`; `U-limits` fixes concurrency and resume exclusion mechanism |
| data lifecycle | obligation ↔ proof + Unresolved | R14 / `proof-benchmark-artifacts`; `U-storage` fixes retention, cleanup, and redaction review |
| external-dependency failure | obligation ↔ proof + Unresolved | R08 / `proof-benchmark-outcomes`; R13 / `proof-benchmark-quarantine`; `U-integration` and `U-failure-policy` |
| state transitions | obligation ↔ proof + Unresolved | R08 / `proof-benchmark-outcomes`; R09 / `proof-benchmark-recovery`; `U-state-contract` fixes transition table |
| observability | obligation ↔ proof | R14 / `proof-benchmark-artifacts`; R15 / `proof-benchmark-report` |

## Unresolved

| Identifier | Type | Decision needed | Owner | Blocking effect | Disposition |
| --- | --- | --- | --- | --- | --- |
| U-integration | Technical contract | Harbor/Codex/OpenSpec compatibility and benchmark implementation language/packaging | Implementer + Human | Blocks frozen code design | Read-only compatibility investigation before selecting architecture; no substitute framework silently |
| U-model | Experiment configuration | Exact model, reasoning level, runtime/framework versions and price schedule | Human with implementer evidence | Blocks live dispatch | Pin after compatibility/cost investigation |
| U-tasks | Product inputs | Ten task IDs, public/synthetic split, fixtures, reviewers, licensed provenance, reference repetition/flakiness threshold | User/team | Blocks smoke acceptance and dispatch | Team supplies validated packages; preserve six-family coverage |
| U-facts | Validity protocol | Auditable task-fact inventory and snapshot normalization checks | User/team + implementer | Blocks snapshot freeze | Inventory required facts and reviewer sign-off; do not infer equivalence from equal file counts |
| U-budget-metering | Financial contract | Enforceable upper bounds, setup charging, provider usage/pricing, cancellation and in-flight cost treatment | Implementer + Human | Blocks paid dispatch | Hard cap controls all smoke-attributable costs; unsupported bounds fail closed |
| U-limits | Experiment configuration | Per-task time/cost/token limits, resource limits, concurrency, network policy, resume exclusion | Human + implementer | Blocks live dispatch and scheduler acceptance | Set within hard cap and equally across conditions |
| U-failure-policy | Validity protocol | Evidence-based infrastructure/condition/evaluator failure classification and disputed cases | User/team | Blocks outcome acceptance | Freeze before runs; classification cannot depend on which condition wins |
| U-retry-policy | Experiment configuration | Retry eligibility, maximum attempts, whether interrupted in-flight requests can be retried safely | Human + implementer | Blocks automatic retries | No automatic paid retry before policy is frozen; retain attempts |
| U-human-policy | Interaction contract | Exact answer bank, approval boundaries, stopping rule | User/team | Blocks unattended workflow comparison | Freeze before dispatch; log all responses |
| U-handoff | Task contract | Per-task interruption boundary, retained state, fresh-session evidence, rework measure | User/team + implementer | Blocks handoff grading | Apply same external interruption protocol across conditions |
| U-state-contract | Technical contract | Concrete persisted state vocabulary, allowed transitions, artifact-write and crash reconciliation rules | Implementer | Blocks runner recovery acceptance | Design from PRD outcomes before code-plan freezes affected units |
| U-storage | Data lifecycle | Local artifact destination, retention/cleanup, secret-redaction review | Human + implementer | Blocks artifact acceptance | No remote backend or silent deletion; retain failed attempts for audit |
| U-commands | Verification contract | Real runner/check commands and harness-validation wiring | Implementer | Blocks wave acceptance | Add commands only when actual repository surfaces exist; this spec's embedded gate works now |
| U-analysis | Statistical policy | Interval algorithm/settings, repository clustering treatment, secondary-comparison adjustment | User/team + implementer | Blocks pilot freeze; smoke limitations report allowed | Predeclare before pilot results |
| U-pilot | Product/financial decision | Minimum worthwhile effect, acceptable cost/review tradeoff, independent holdout, pilot budget | Human | Blocks pilot launch only | Decide from smoke evidence before final pilot; no automatic budget inheritance |

## Waves

These are acceptance boundaries, not ordered work units or batch packing. `code-plan` must preserve the stops after approval. A failed gate aborts advancement; fix and rerun before continuing.

| Boundary | Scope | Gate |
| --- | --- | --- |
| Wave 1: verifiable local evaluation contracts | Resolve architecture/commands and deliver manifest, condition, task, grading, state, cost, and artifact contracts without paid runs | All relevant R01–R16 proof checks; actual commands supplied under U-commands; structural gate below |
| Wave 2: smoke readiness and controlled execution | Approved team inputs, live-launch authorization, full independent smoke audit | R17 and R18 boundary checks; structural gate below; US$50 cap |

Wave-specific executable product gates are unresolved under U-commands; this draft does not invent commands for a nonexistent runner. No wave may be marked accepted using the document-only gate. The sixty-task pilot is a later initiative requiring U-pilot resolution.

## Acceptance

All unchecked: no implementation evidence exists.

- [ ] R01 — `proof-benchmark-conditions` PASS.
- [ ] R02 — `proof-benchmark-manifest` PASS.
- [ ] R03 — `proof-benchmark-task-packages` PASS.
- [ ] R04 — `proof-benchmark-isolation` PASS.
- [ ] R05 — `proof-benchmark-onboarding` PASS.
- [ ] R06 — `proof-benchmark-scheduling` PASS.
- [ ] R07 — `proof-benchmark-budget` PASS.
- [ ] R08 — `proof-benchmark-outcomes` PASS.
- [ ] R09 — `proof-benchmark-recovery` PASS.
- [ ] R10 — `proof-benchmark-human-policy` PASS.
- [ ] R11 — `proof-benchmark-handoff` PASS.
- [ ] R12 — `proof-benchmark-grading` PASS.
- [ ] R13 — `proof-benchmark-quarantine` PASS.
- [ ] R14 — `proof-benchmark-artifacts` PASS.
- [ ] R15 — `proof-benchmark-report` PASS.
- [ ] R16 — `proof-benchmark-analysis` PASS.
- [ ] R17 — `proof-benchmark-smoke-gate` PASS.
- [ ] R18 — `proof-benchmark-pilot-boundary` PASS.

## Eval / gates

| Gate | Real command / check | When | PASS | Abort |
| --- | --- | --- | --- | --- |
| Spec obligations | Embedded Python command below, named `proof-benchmark-spec-obligations` | Before spec handoff and after edits | Unique proof per obligation, acceptance bijection, nine landings, typed unresolved entries | Do not call draft structurally ready |
| Product proofs | Direct artifact/control inspections defined separately in each R row; executable bindings unresolved in U-commands | Relevant wave acceptance | Each obligation's own evidence passes; no reused proof | Do not advance on document validation alone |
| Harness integrity | `harness:validate` is absent; U-commands must supply actual repository invocation before it can be required or run | Once validation surface exists | Real command exits 0 | Never claim an absent gate passed |
| Smoke-to-pilot | Independent R17 audit plus R18 decision/budget record | Before pilot progression | All checks hold, not merely forty recorded attempts | Stop; report failed criteria |

Run from the benchmark repository root. This command is available with Python 3 and validates the saved spec only:

```sh
python3 - <<'PY'
from pathlib import Path
import re
p = Path('docs/specs/benchmark-evaluation/benchmark-evaluation-spec.md')
s = p.read_text()
requirements = s.split('## Requirements: obligation ↔ proof\n', 1)[1].split('### Budget boundary', 1)[0]
rows = re.findall(r'^\| (R\d{2}) \| .*? \| `(proof-[^`]+)` \|', requirements, re.M)
assert len(rows) == 18, 'expected 18 obligation rows'
assert len({r for r, _ in rows}) == 18, 'duplicate obligation'
assert len({p for _, p in rows}) == 18, 'reused named proof'
a = s.split('## Acceptance\n', 1)[1].split('## Eval / gates\n', 1)[0]
boxes = re.findall(r'^- \[ \] (R\d{2}) — `(proof-[^`]+)` PASS\.$', a, re.M)
assert sorted(boxes) == sorted(rows), 'acceptance/proof mismatch or completed box'
d = s.split('## Nine-dimension landings\n', 1)[1].split('## Unresolved\n', 1)[0]
for dim in ('validation', 'failure modes', 'idempotency and retry', 'authorization', 'concurrency and ordering', 'data lifecycle', 'external-dependency failure', 'state transitions', 'observability'):
    assert f'| {dim} |' in d, dim
u = s.split('## Unresolved\n', 1)[1].split('## Waves\n', 1)[0]
assert '| Identifier | Type | Decision needed | Owner | Blocking effect | Disposition |' in u
ids = re.findall(r'^\| (U-[a-z-]+) \|', u, re.M)
assert len(ids) == len(set(ids)), 'duplicate unresolved identifier'
for row in u.splitlines():
    if row.startswith('| U-'):
        assert len(row.split('|')) == 8 and all(c.strip() for c in row.split('|')[1:-1]), row
assert all(ref in ids for ref in re.findall(r'`(U-[a-z-]+)`', d)), 'unresolved landing without row'
print('proof-benchmark-spec-obligations: PASS')
PY
```

## Out of scope

Hosted dashboards, leaderboards, automatic publication, model training, broad agent support, real customer repositories, full public benchmark runs, pilot execution, and ablations. The unchanged Wolven and external tool source trees are not edit targets.

## Pragmatic-guard refuses

Need 8/10 for a bounded independent smoke evaluation; complexity 6/10 because four workflows require genuine control of inputs and grading. Proceed with the narrow local scope. Do not add plugin discovery, a distributed scheduler, or a remote artifact backend: present need 1/10 versus complexity 7/10. Existing PRD excludes these surfaces; no new deferral changes are required by this spec.

Refuse fabricated acceptance commands, documentation score as success, rerun-until-pass selection, invented team task obligations, silently weakened spending caps, or paid runs authorized merely by writing this document.

## Cross-domain leak table

| Leak | Correct lane |
| --- | --- |
| Ordered units, ownership assignments, batch packing | `code-plan` after spec approval |
| Building the runner or installing the harness here | `code-execute` under approved requirements |
| Authoring tasks and reviewing final rubrics | User/team, as confirmed |
| Model/provider selection and spending authorization | Human configuration decisions and launch authorization |
| Sixty-task pilot or expanded external benchmarks | Later approved initiative after smoke readiness |
| Publishing reports or changing repository visibility | Separate explicit request |

## ADR

No new durable architecture decision is settled here. Local Docker, Codex, OpenSpec, and separate repository scope are already confirmed inputs. Harbor compatibility, language/packaging, storage, and state persistence remain unresolved; this spec does not assign ADR numbers or invent decision records. If their resolution settles a durable architecture choice, invoke the `adr` skill before recording it as established architecture.
