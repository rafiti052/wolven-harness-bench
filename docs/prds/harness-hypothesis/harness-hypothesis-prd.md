---
type: prd
title: Harness hypothesis fixtures
description: A local way for an agent testing a wolven-harness feature or fix to run a fixture in this repo and receive a score plus a detailed report.
status: draft
---

# PRD: Harness hypothesis fixtures

## Problem

- **Who:** Wolven maintainers testing a feature or fix in `wolven-harness`, including through an agent that clones this repo.
- **Pain:** That agent cannot run a fixture it created and receive a score plus a detailed report that keeps infrastructure separate from behavior, so maintainers cannot tell whether the change altered agent behavior.
- **Why now:** Four QMD diagnostic campaigns finished on 2026-10-14. Their lesson note shows a campaign record that can carry this kind of result. The [benchmark evaluation PRD](../benchmark-evaluation/benchmark-evaluation-prd.md) compares Wolven with other workflows and does not provide this local run.
- **Evidence:** [Lessons from the QMD diagnostic campaigns](../../notes/qmd-diagnostic-lessons/qmd-diagnostic-lessons-note.md), recorded in [pull request #2](https://github.com/rafiti052/wolven-harness-bench/pull/2).

A **hypothesis fixture** is the package an agent adds for one feature or fix. It declares the hypothesis, the cases, and the score columns. This is not a task fixture in the benchmark evaluation PRD. In the lesson note, healthy and stale are **cases** of the first hypothesis fixture, not two fixtures.

A **campaign** is the record of one local run of a hypothesis fixture: the traces, the infrastructure result, the score, and the detailed report. The same word in the lesson note names the four finished runs.

**Infrastructure** is exit, survivors, and timing. The **score** is the behavioral result on the columns that fixture declared. A **hypothesis** is the claim about the harness change that those cases can support or contradict.

## Goals

- A maintainer's agent can clone this repo, add a hypothesis fixture, and run it locally — success signal: the run leaves a campaign record for that fixture.
- The campaign record states a score and a detailed report — success signal: a reader can see the behavioral result on the fixture's columns without reading the traces first.
- Infrastructure stays visible beside the score — success signal: an exit code of 0 on a case that missed its behavioral expectation is still a recorded miss.
- The four QMD campaigns remain readable as the first hypothesis fixture — success signal: their recorded scores and the unset placement choice are unchanged after a later fixture is scored.
- This repo keeps the other-tools comparison as its own PRD — success signal: the benchmark evaluation PRD's problem, conditions, and milestones stay as they are.

**Non-goals**

- Choosing which of the three QMD instruction placements won.
- Comparing Wolven with another workflow framework, a lightweight checklist, or a bare agent. That comparison stays in the benchmark evaluation PRD.
- Changing the `wolven-harness` package, its templates, or its open canon-proof pull request.
- A web page for watching runs. The live view is a `@clack/prompts` log of the campaign files.
- A public leaderboard, or a 90-run matrix.
- Regrading the four finished campaigns.

## User stories

### US-1: Run a hypothesis fixture in a clone

**As a** Wolven maintainer,
**I want** an agent that cloned this repo to run a fixture it created for a harness feature or fix,
**so that** the change is tested in this repo rather than only in a disposable launcher.

Traces to goal: a local run leaves a campaign record.

#### AC-1.1: Completed local run

**Given** a clone of this repo and a hypothesis fixture that declares its hypothesis, its cases, and its score columns
**When** the agent runs that fixture in the clone
**Then** the campaign record contains the traces, the infrastructure result, a score on those columns, and a detailed report

#### AC-1.2: Incomplete fixture

**Given** a package that omits the hypothesis, the cases, or the score columns
**When** the agent runs it as a hypothesis fixture
**Then** no score is issued, and the report names the missing declaration

#### AC-1.3: The runner installs the harness from source

**Given** a fixture whose `wolven-harness` revision is a recorded commit, including a commit that has not been published
**When** the runner prepares the environment
**Then** that commit is checked out and built inside the runner environment, `setup` writes that revision's templates into the measured workspace, the campaign record names the commit, and the harness checkout stays outside the measured workspace

#### AC-1.4: The live view is a clack log

**Given** a campaign that is still running
**When** the maintainer asks for its status
**Then** a `@clack/prompts` log shows each cell from the campaign files, and the runner has not started a web server. An open cell is a magenta spinner. A finished cell is a green mark when the behavioral result matches, a yellow mark when retrieval ran and the stated result does not, and a red mark when a result was stated without the retrieval the case requires. The hint is a second line of at most 100 characters on a finished cell. A progress bar counts finished cells. Exit and elapsed time stay on the line. Exit 0 does not select the color.

### US-2: Read behavior apart from infrastructure

**As a** Wolven maintainer,
**I want** the score to follow the fixture's columns while infrastructure stays in its own part of the report,
**so that** a clean process exit does not hide a behavioral miss.

Traces to goal: infrastructure stays visible beside the score.

#### AC-2.1: Both results are present

**Given** a finished case
**When** the maintainer reads the detailed report
**Then** the score states the behavioral result on the fixture's columns, and exit, survivors, and timing appear separately

#### AC-2.2: Exit 0 is not the score

**Given** a case that exited 0 and missed the behavioral expectation declared for that column
**When** the maintainer reads the score
**Then** the case is a miss, and the exit code remains an infrastructure fact

### US-3: Keep the QMD campaigns as the first example

**As a** Wolven maintainer,
**I want** the four finished QMD campaigns to map onto this record,
**so that** a later fixture can use the same shape without rewriting those results.

Traces to goal: the four campaigns remain readable as the first hypothesis fixture.

#### AC-3.1: The first fixture maps

**Given** the lesson note's four campaigns
**When** a reader maps them onto a hypothesis fixture
**Then** the ten-cell batch and the three later runs appear as cases, their recorded scores stay as written, and no instruction placement is marked as the winner

#### AC-3.2: A later fixture does not rewrite them

**Given** a new hypothesis fixture whose columns differ from the QMD columns
**When** that fixture is scored
**Then** the four campaigns' scores and traces stay as they were

### US-4: Leave the other-tools comparison alone

**As a** Wolven maintainer,
**I want** a harness feature or fix tested here, and a comparison against other tools tested under the other PRD,
**so that** the two questions do not share one score.

Traces to goal: this repo keeps the other-tools comparison as its own PRD.

#### AC-4.1: The other PRD stays put

**Given** this PRD and the benchmark evaluation PRD
**When** a reader opens the benchmark evaluation PRD
**Then** its problem, its four conditions, and its milestones are unchanged

#### AC-4.2: A comparison fixture is refused here

**Given** a package whose hypothesis is that Wolven beats another workflow on a coding task
**When** it is offered as a hypothesis fixture
**Then** it is refused on this track, and the report points to the benchmark evaluation PRD

## Scope

**In**

- A hypothesis fixture declared by an agent: hypothesis, cases, and score columns.
- A local run of that fixture in a clone of this repo.
- A runner environment that installs `wolven-harness` from a recorded source commit and builds it there, including a commit that has not been published. `setup` from that build writes the templates into the measured workspace. The checkout stays outside that workspace.
- A campaign record with traces, infrastructure, a score, and a detailed report. The record names the commit that was built.
- A `@clack/prompts` log of that record while the campaign is running. An open cell is a magenta spinner. A finished cell carries a green, yellow, or red mark and a hint of at most 100 characters. A progress bar counts finished cells.
- The four QMD campaigns as the first example of that record. Their columns are an example, not the only legal columns.

**Out**

- The benchmark evaluation track: task fixtures, conditions A through D, the smoke, and the pilot.
- Edits to `wolven-harness`.
- A verdict on the three instruction placements.
- Copying historical campaign trees into this repo. Whether those trees are imported later is an open question.
- A web monitor. The disposable launcher's page stays there.

## Open questions

- Whether the four campaign trees are imported as a worked example, or the first version only has to represent them — owner: Human
- Which runtimes, spending limits, and account routes a local run may use — owner: Human. The first example used five native CLIs on existing allowances. This PRD does not authorize a paid run.

## Handoff

After approval → `code-spec`. This PRD does not authorize implementation.
