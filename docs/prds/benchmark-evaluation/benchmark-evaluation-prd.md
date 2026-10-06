---
type: prd
title: Wolven benchmark evaluation
description: Independent evaluation of coding-agent workflows through controlled smoke experiments and a later frozen pilot.
status: draft
---

# PRD: Wolven benchmark evaluation

Status: Draft for review  
Date: 2026-10-02  
Updated: 2026-10-06  
Repository: `rafiti052/wolven-harness-bench` (public)  
Product: An independent benchmark runner and task suite for evaluating coding-agent workflows.

## Problem

Wolven harness adds instructions, decision retrieval, planning, validation, and resume records to coding agents. Its repository structure score does not establish whether these additions improve delivered work. Existing coding benchmarks measure useful capabilities, but do not fully measure decision compliance, scope control, supervision, or interrupted-work recovery.

We need reproducible evidence of when Wolven helps, when it introduces overhead, and how it compares with lightweight instructions and alternative workflow frameworks. The evaluation must distinguish workflow effects from model, information, and environment differences.

## Users and decisions

- Wolven maintainers need evidence to prioritize features and detect regressions.
- Teams considering adoption need to compare quality, cost, review effort, and onboarding effort.
- Researchers and contributors need inspectable tasks, configurations, grading rules, and raw run artifacts.

The initial decision is whether adding Wolven to a fixed coding-agent configuration produces a worthwhile improvement on representative repository work. Later decisions include which Wolven components contribute and how complete agent products compare.

## Goals

1. Compare the same agent and model with and without Wolven.
2. Compare Wolven with a lightweight checklist and one alternative workflow framework.
3. Measure independently verified outcomes, total execution cost, elapsed time, and human involvement.
4. Test functional correctness alongside architectural constraints, scope control, verification claims, and resume reliability.
5. Produce an auditable report with paired results and uncertainty estimates.
6. Keep benchmark code, task fixtures, grading, and experiment configuration in a repository separate from `wolven-harness`.

## Non-goals for the first release

- A hosted dashboard, public leaderboard, or automatic publication.
- Model training, workflow optimization, or automatic harness tuning.
- Declaring a universal best agent from a single benchmark.
- Using `harness:score` or Wolven validation as the primary success metric.
- Broad support for every coding agent or workflow framework.
- Running the entire public benchmark collection before the custom pilot is reliable.

## Evaluation tracks

| Track | Controlled comparison | Interpretation |
| --- | --- | --- |
| Workflow effect | Fixed agent/model; change workflow | Contribution of the workflow configuration |
| Workflow alternatives | Fixed agent/model; Wolven versus another framework | Relative effectiveness under the selected protocol |
| Complete products, later | Different agent/model product configurations | Practical configuration performance; no causal attribution to the harness alone |

The first release implements the workflow-effect and workflow-alternatives tracks with one supported coding-agent runtime and one fixed model configuration.

### Initial conditions

| ID | Condition | Behavior |
| --- | --- | --- |
| A | Default agent | Existing repository instructions and normal agent behavior |
| B | Lightweight checklist | A plus concise instructions to plan proportionately, respect decisions, test changes, and report evidence |
| C | Wolven | Same agent/model with a pinned Wolven release and documented initialized configuration |
| D | Alternative workflow | Same agent/model with a pinned Spec Kit or OpenSpec configuration |

Use Codex CLI with OpenSpec as condition D. Pin the exact model, reasoning settings, runtime, and framework versions through the compatibility smoke test before freezing the pilot configuration. Record these choices in the experiment manifest. No claim of cross-model generality is made by this pilot.

## Scope and milestones

### Milestone 1: Validated smoke experiment

Deliver 10 tasks drawn from synthetic fixtures and pinned public benchmark tasks, all four condition adapters, independent grading, artifact capture, and a cost estimate. Cover the six task families through synthetic fixtures where public tasks do not exercise them. Run one trial per task and condition, for 40 agent runs, after validating the task environments. Execute locally with Docker. The total smoke experiment cap is US$50 for model and execution costs; stop before exceeding it, even if this leaves the 40-run experiment incomplete. The user's team authors tasks and reviews grading. Do not dispatch paid runs solely because the budget is recorded; experiment preparation and launch remain separate steps.

Done when every task's reference solution passes; known incorrect solutions fail the intended checks; each condition completes an end-to-end run; an interrupted runner can resume without duplicating finished runs; and a report can trace each result to its artifacts.

### Milestone 2: Frozen custom pilot

Deliver 60 validated tasks, a disjoint development set, frozen configuration, three independent trials per task and condition, and the paired report. The pilot contains 720 agent runs. Estimate the expense and configure an explicit spending limit before starting.

### Milestone 3: External validation and ablations

Expand public benchmark coverage beyond the pinned tasks included in the smoke experiment, using official evaluators for SWE-bench Verified and selected Terminal-Bench tasks where applicable. Run focused Wolven ablations only after the main comparison is stable. Broader external validation and ablations are follow-up releases, not prerequisites for Milestone 2.

## Task suite

| Family | Pilot count | What is evaluated |
| --- | --- | --- |
| Small fixes | 10 | Correctness and overhead on straightforward changes |
| Features across files | 10 | Complete implementation and compatibility |
| Architecture decisions | 10 | Compliance with explicit settled constraints |
| Stale or conflicting documentation | 10 | Distinguishing stable decisions from obsolete proposals |
| Interrupted work and handoffs | 10 | Recovery from persisted state in a fresh agent session |
| Scope and verification traps | 10 | Staying within requested scope and making supported completion claims |

Use synthetic repository fixtures plus pinned public benchmark tasks; user-supplied real repositories are outside the initial scope. The user's team owns task authoring and grading review. Prefer several representative fixtures and languages supported by Codex CLI. Publish the task distribution and explain any limits on representativeness. Do not select final tasks based on which configuration wins.

Each task package must include a starting snapshot, user request, factual documentation, allowed paths and actions, reference solution, acceptance checks, regression checks, constraint checks, resource requirements, budget, and grading rubric where necessary. Reference solutions and hidden checks must be inaccessible to the agent.

Task authors must verify that the request is solvable without hidden knowledge. Existing tests and nonnegotiable constraints must be available equally across conditions. Hidden tests assess stated behavior rather than unstated requirements.

Handoff tasks must specify a reproducible interruption boundary, the state retained, and the state discarded. Start a fresh session without the prior conversation, preserving the workspace and any records legitimately written by that condition. Record the cost of both phases.

## Functional requirements

### Experiment configuration

The runner accepts a versioned manifest that pins task IDs and revisions, repository commits, condition versions and initialization, runtime/model settings, trial count, resource limits, network policy, human-response policy, cost limits, and randomization seed. Archive the resolved manifest with each experiment. Record model identifiers as reported by the provider and disclose when immutable model snapshots are unavailable.

### Isolation and fairness

Every task-condition-trial starts in a fresh workspace and agent session. Use equal resource and permission policies within a comparison. Isolate credentials, caches that contain task answers, reference solutions, grading files, and previous run artifacts.

All conditions receive equivalent factual knowledge. If Wolven initialization produces architecture documentation, normalize the factual content for the other conditions before freezing snapshots. Condition-specific workflow instructions and legitimate task-generated artifacts remain part of the treatment.

Prepare initialized snapshots outside the primary task runs. Record onboarding model cost, execution time, and human effort separately. A later adoption track may measure setup and execution together.

Use one declared human-response policy for each experiment. The unattended pilot uses predefined factual answers and approvals at documented workflow boundaries, without supplying implementation advice. Log each interaction. Unanswerable clarification requests become `needs_input` results under the declared stopping rule; they must not disappear from reporting. A human-assisted study measures actual intervention and review time separately.

### Execution and recovery

Schedule runs in randomized order within task/trial blocks to reduce time and provider effects. Support bounded concurrency, timeout enforcement, cancellation, and resuming a partially completed experiment.

Assign an immutable run ID to each attempt. Persist status transitions and artifacts. Do not silently rerun failures until they pass. Infrastructure retries follow a predefined policy and retain links to all attempts. Agent crashes and tool failures attributable to the condition count as condition outcomes.

Enforce configured cost limits where runtime metering permits. When precise real-time metering is unavailable, use conservative token/cost estimates and stop dispatching new runs at the configured boundary; disclose possible overshoot from active runs.

### Independent grading

A task is accepted when acceptance tests and regression checks pass, mandatory constraints are met, and execution finishes within budget. Agent-written tests cannot replace evaluator-owned checks.

Grade final code in a clean evaluation environment using protected checks. Any modifications to agent-visible tests or validation configuration must be recorded and assessed against the task's allowed scope.

Report functional success separately from overall acceptance. Check architecture and scope obligations with executable assertions where possible. Use a frozen rubric and condition-blinded human review for properties requiring judgment. Retain original artifacts; mask condition-identifying metadata in reviewer copies where practical and disclose residual blinding limits.

Evaluate completion claims against recorded evidence. An unsupported claim means the agent asserted a check passed or work was complete without evidence sufficient under the task rubric. Honest reporting of a failed check is not an unsupported claim.

Run reference solutions repeatedly to identify flaky environments, and validate known-failing outputs. Quarantine invalid tasks through a documented rule applied to every condition. Report original and eligible task counts plus exclusion reasons.

### Artifacts

Capture the task revision, resolved configuration, environment/image identity, condition setup, transcript, tool commands and results, final diff, timing, usage and cost, human interactions, grading output, stop reason, and retry history. Redact credentials before storing or exporting artifacts.

Missing usage or cost data is explicitly marked unknown, never zero. Record whether amounts are measured or estimated and the price schedule used.

### Reporting

Generate machine-readable results and a standalone Markdown report. Include aggregate outcomes, paired wins/losses, task-family breakdowns, budget exhaustion, input requests, condition failures, infrastructure errors, exclusions, and links to artifacts.

Do not publish results automatically. Reports remain local until publication is explicitly requested.

## Metrics and statistical analysis

Primary metrics:

- Accepted-task rate, averaging trial outcomes within each task and then weighting eligible tasks equally.
- Total model, tool, and execution cost divided by the number of accepted runs, including costs of unsuccessful attempts. If no runs are accepted, report the metric as undefined.

Secondary metrics: functional success; architecture/scope violations; unsupported completion claims; elapsed time at median and p95; intervention count; human review minutes when measured; handoff success and rework; onboarding effort. Report elapsed time across all attempts and successful attempts separately to avoid hiding failure-related delays.

Predeclare C versus A as the primary comparison. Treat C versus B and C versus D as secondary comparisons. Define any multiple-comparison adjustment before analysis. Report absolute percentage-point differences and paired outcomes, not only relative lift.

Generate 95% confidence intervals by resampling tasks while retaining their paired conditions and repeated trials. For generalization across repositories, disclose repository clustering and use a repository-aware analysis when the number of fixtures supports it. Do not count repeated trials as independent tasks.

The 60-task pilot is a practical starting point, not a power guarantee. Use development/smoke results to estimate variance. The user will choose the minimum worthwhile effect and acceptable cost/review tradeoff after reviewing smoke results and before freezing the pilot. Evaluate whether more distinct tasks are needed; label inconclusive results explicitly.

## Product acceptance criteria

- A contributor can reproduce a smoke experiment from a documented manifest and supported environment.
- Four conditions run on identical task inputs with isolated sessions.
- Reference and known-failing solutions demonstrate grader sensitivity before agent evaluation.
- Every reported outcome links to its resolved configuration, final diff, and grading evidence.
- Resume skips completed runs and preserves failed attempts and retry history.
- Reports distinguish invalid infrastructure from agent failure, budget exhaustion, and required input.
- Cost reporting includes unsuccessful work and flags missing data.
- Paired analysis preserves task-level correlation.
- Benchmark functionality requires no source changes to `wolven-harness`.

These criteria establish benchmark readiness. They do not require Wolven to outperform the baseline. A neutral or negative result is valid.

## Repository and integration boundaries

Use the separate public repository `rafiti052/wolven-harness-bench`. Keep this PRD, runner, adapters, task fixtures, evaluator, analysis, and documentation there. Consume Wolven through a pinned released package or explicit commit; never rely on an unrecorded local checkout.

Use Harbor as the preferred execution integration, subject to the smoke-stage compatibility check. Reuse its task isolation and supported agent adapters where possible. Wolven is a configured workflow around the underlying agent, not a replacement model. Preserve official grading for external benchmarks.

Keep generated run artifacts and credentials out of Git. Store lightweight manifests and task metadata in version control; support configurable local artifact storage. A remote artifact backend is deferred until local usage demonstrates a need.

## Risks and mitigations

| Risk | Mitigation |
| --- | --- |
| Benchmark favors Wolven-specific behavior | Include routine tasks and independent outcome grading |
| Wolven receives more factual context | Normalize knowledge across conditions |
| Workflow approvals cause artificial stalls | Freeze and log a consistent human-response protocol |
| Public-task contamination or overfitting | Keep a separate holdout; disclose public exposure |
| Validation is mistaken for correctness | Grade with protected tests and explicit constraints |
| Task/environment flakiness | Repeated reference checks and symmetric quarantine rules |
| Model drift or provider outages | Pin available identifiers, randomize runs, retain timestamps and incidents |
| Trials exceed budget | Smoke cost estimate, bounded dispatch, explicit spending limits |
| Small suite produces uncertain conclusions | Task-level intervals and a prospective sample-size decision |

## Confirmed decisions

Confirmed by the user on 2026-10-06:

- Runtime: Codex CLI; alternative workflow: OpenSpec.
- Smoke spending cap: US$50 total for model and execution costs.
- Task sources: synthetic fixtures plus pinned public benchmark tasks.
- Execution environment: local Docker.
- Task authoring and grading review: the user and their team.
- Adoption threshold: decide after smoke results, before the final pilot.

The 720-run pilot has no approved budget yet. The smoke cap does not authorize pilot spending.

## Decisions still to resolve

- Pin the Codex CLI version, exact model and reasoning configuration, and Docker environment requirements.
- Pin the OpenSpec and Wolven versions and validate compatibility.
- Set per-task budgets and infrastructure retry rules within the US$50 smoke cap; estimate and approve a separate pilot budget later.
- The user's team selects synthetic fixtures and public task versions and names the task authors and grading reviewers.
- After smoke results, define the minimum worthwhile acceptance improvement and acceptable cost/review tradeoff before freezing the pilot.
- Repository ownership, name, and visibility are confirmed: `rafiti052/wolven-harness-bench`, public.

These choices do not block review of the product scope. Record their resolved values in the first experiment manifest.

## Sources

- Wolven overview: https://github.com/WolvenTech/wolven-harness
- Wolven entry instructions and structure score: https://github.com/WolvenTech/wolven-harness/blob/main/templates/WOLVEN.md
- Wolven execution and resume workflow: https://github.com/WolvenTech/wolven-harness/blob/main/templates/.agents/skills/code-execute/SKILL.md
- Wolven architecture-reference validation: https://api.github.com/repos/WolvenTech/wolven-harness/git/blobs/4e70ec034a4262f564a2d207af17d93533609b51
- Harbor: https://github.com/harbor-framework/harbor
- SWE-bench: https://github.com/SWE-bench/SWE-bench
- Terminal-Bench: https://github.com/harbor-framework/terminal-bench
- Spec Kit: https://github.com/github/spec-kit
- OpenSpec: https://github.com/Fission-AI/OpenSpec

Source links informed the design; experiment manifests must pin the versions actually evaluated. The earlier explainer's example scores were illustrative and are not evidence of benchmark results.
