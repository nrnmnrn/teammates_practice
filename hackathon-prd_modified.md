# Background Report Scheduling Strategy Validation Bench

> Hackathon MVP PRD (Product Requirements Document) · **Unapproved Candidate** · For team decision-making only.
>
> This document is reorganized from [task-scheduler-validation-candidate.md](docs/product/task-scheduler-validation-candidate.md). It does not replace competition rules, is not an implementation handoff, and must not be added to `build-handoff/`. The organizer has confirmed that the topic may be changed before formal development without a separate application; the team must still approve the overall product direction before implementation.
>
> **Core specifications confirmed in this round**: Urgency is calculated from remaining time to the deadline; AI makes one proposal at the fixed checkpoint of each scenario; the evaluated object is the same proposer procedure. The overall product remains a candidate awaiting team approval.

## 1. Product Summary

The **Background Report Scheduling Strategy Validation Bench** is an unapproved candidate for an offline simulation tool. It replays a frozen background-report job scenario: when urgent jobs surge, it compares fixed scheduling rules, simple search, and an AI proposal, then uses pre-locked thresholds to decide whether AI candidate passes simulation eligibility checks.

It is not an Operating System (OS) scheduler, does not connect to a real queue, and never changes a live service automatically. AI reads an observed summary only at a fixed checkpoint, then proposes one strategy from limited weight choices with a rationale. Deterministic simulator and evaluator remain only authorities for execution and judgment.

Goal is not claim AI always performs best. Goal is let judges see how an AI recommendation is compared fairly, accepted, or rejected:

```text
Observed summary → constrained AI candidate → replay from same snapshot → evaluator gates → complete results
```

### Decision Status and Basis

- **Confirmed information**: Source document explicitly labels this as an unapproved candidate that cannot replace current handoff material; see [candidate document](docs/product/task-scheduler-validation-candidate.md).
- **Confirmed competition information**: The organizer has confirmed that the topic may be changed before formal development without a separate application; see [competition record](docs/competition/README.md).
- **Team-confirmed technical choices**: Deadline-based urgency, one constrained AI proposal per scenario at its checkpoint, and Gradio + Plotly for the demo interface.
- **Team tentative candidate**: Background-report jobs, two-node simulation, and an approximately 90-second internal demo story.
- **Assumption**: A fixed synthetic job table can validate causal relationships in this demo. It is not production operating data.
- **Open question**: Formal presentation format and remaining delivery constraints are unconfirmed. Do not treat this candidate as an approved competition entry until the team approves the overall direction.

## 2. Target Users and Problem

### Primary Users

Operations or platform staff who manage background-report queues. During urgent-job surges, they must consider both urgent overdue jobs and normal jobs waiting too long.

### Problem

With FIFO (First In, First Out), urgent jobs may wait too long. With permanent urgent-job priority, normal jobs may starve, meaning they go unprocessed for a long time. Users need to see:

- What queue pressure has been observed;
- Which data a candidate strategy may use and may not use;
- Whether it regresses against simple approaches from same initial state;
- Why system rejects it when it regresses.

### Product Promise and Limits

This candidate promises only replayable, comparable, rejectable simulation evidence. Evaluator acceptance gates use FIFO as operational protection baseline. Grid search is required informational comparison; it does not decide acceptance. If AI does not outperform best simple baseline, demo may say only that “AI candidate can be audited and safely rejected.” It must not claim AI has optimized scheduling or created real business value.

## 3. MVP Scope and Non-goals

### In Scope (Unapproved)

- One or a small number of fixed, versionable background-report job scenarios.
- One fixed development fixture and two fixed validation scenarios from section 5.4.
- One fixed checkpoint and identifiable snapshot (complete, replayable simulation state at a point in time).
- Deterministic simulator with two equivalent background-job nodes.
- Fair comparison among FIFO, Priority, limited grid search (trying every combination in a small fixed weight set), and one AI candidate for each scenario.
- Constrained AI weight proposal, rationale, model information/failure status, and evaluator gates locked in advance after approval.
- Gradio interface, Plotly charts, complete results, and an approximately 90-second internal rehearsal story.

### Non-goals

- MiniGrid, Gymnasium, reinforcement learning (RL), reward functions, or training loops.
- Real queue integration, online switching, automatic deployment, or business-value claims.
- AI dispatching individual jobs, reading future jobs, executing arbitrary code, or changing gates.
- Reflection loops (automatic retries after reflection), skill libraries, long-term storage, or strategy reuse.
- Databases, multi-user features, OS/CPU scheduling semantics, or full e-commerce order flows.

## 4. System Architecture and Responsibilities

```text
Frozen job table + fixed scenario
          │
          ▼
   Checkpoint snapshot ──► observed summary ──► AI proposer
          │                              │
          ├──► FIFO / Priority / grid search / AI candidate
          │                              │
          ▼                              ▼
   Deterministic two-node simulator ───► evaluator
                                             │
                                             ▼
                          Complete results, rationale, acceptance/rejection view
```

| Module | Sole responsibility | Required output | Must not do |
| --- | --- | --- | --- |
| `scenario_loader` | Validate and load an immutable scenario | `scenario` or `scenario_invalid` | Rewrite the job table or correct data based on results |
| `simulator` | Advance two nodes in the fixed event order | `snapshot`, complete `event_record`, and per-job results | Call AI or judge strategy quality |
| `policy` | Choose one visible waiting job | `job_id` and a stable ordering rationale | Read unarrived data, actual durations, validation results, or arbitrary code |
| `ai_proposer` | Turn an allowed summary into a constrained candidate | A valid candidate or `not_evaluable` | Dispatch individual jobs, change gates, retry, or read the future |
| `evaluator` | Calculate metrics and every gate from completed records | Immutable `evaluation` and per-gate reasons | Choose results, call AI again, change strategies, or change gates |
| `demo_ui` | Read one already-computed `comparison_record` | Four complete rows, statuses, and event comparison | Change a scenario or snapshot, call AI again, or retry automatically |

### 4.1 Data Flow and Access Contract

`scenario_loader` is the only module allowed to load a source file. It passes the complete validated job table to `simulator`, which may read it to replay the scenario. `policy` receives only the current `dispatch_view`: `now`; the `id`, `arrival_time`, `priority`, and `deadline` of each arrived, unstarted job; and the `id`, `arrival_time`, `priority`, `deadline`, and `completed_at` of completed history. Completed history must not include `started_at`, so `actual_duration` cannot be reconstructed from start and completion times. The weighted policy calculates urgency from `deadline`; only the Priority baseline uses `priority` directly.

`simulator` may retain `remaining_duration` for a running job, but it is hidden state used only for replay. It must not enter `dispatch_view`, the AI summary, UI strategy inputs, or evaluation-candidate data. Each strategy branch creates an immutable `strategy_run`; one `comparison_record` contains exactly the FIFO, Priority, grid-search, and AI slots for that scenario. Charts, tables, and gates must refer to the same `comparison_id`, then use `strategy_run_id` to locate their events.

### 4.2 Fixed Types and `null` Rules

All times are non-negative integer simulation ticks, never floating-point values. `null` means only that a field is inapplicable or has no value yet; it must not stand in for `0`, an empty string, or an error. Every invalid input becomes `scenario_invalid`; it must not be silently corrected.

| Record | Fields and types | Constraints and `null` meaning |
| --- | --- | --- |
| `scenario` | `scenario_id: string`, `job_table_version: string`, `checkpoint_time: integer`, `end_time: integer`, `workers: ["worker-1", "worker-2"]`, `jobs: job[]` | Identifier is non-empty; `0 <= checkpoint_time < end_time`; nodes are always exactly two in this fixed order. |
| `job` | `id: string`, `arrival_time: integer`, `actual_duration: positive integer`, `priority: urgent|normal`, `deadline: integer` | `id` is unique within a scenario; `arrival_time >= 0`; `deadline >= arrival_time`; `deadline > end_time` is a valid future deadline. |
| `worker_state` | `worker_id: string`, `running_job_id: string|null`, `started_at: integer|null`, `remaining_duration: positive integer|null` | All three running fields are populated together or are all `null`; remaining time is never exposed to policy or AI. |
| `job_result` | `job_id: string`, `started_at: integer|null`, `completed_at: integer|null`, `worker_id: string|null`, `wait_time: integer`, `status: completed|unfinished` | A completed job has all three fields populated; a started unfinished job has non-null `started_at` and `worker_id` but `completed_at=null`; an unstarted unfinished job has all three fields `null`; wait is defined in section 7.2. |
| `event` | `event_time: integer`, `phase: completion|arrival|dispatch`, `sequence: non-negative integer`, `job_id: string`, `worker_id: string|null` | At the same tick: completion, arrival, dispatch; within a phase: worker name then ASCII `job_id`. An arrival has `worker_id=null`. |
| `proposal_record` | `proposal_request_id: string`, `candidate_id: string`, `proposer_version: string`, `status: in_flight|evaluable|not_evaluable`, two weights, `rationale`, model metadata, `error_code` | Weights and rationale are non-null only when evaluable; model metadata comes from caller measurement or API output, never the model's self-report. |
| `strategy_run` | `strategy_run_id: string`, `strategy_id: fifo|priority|grid|ai`, `strategy_parameters: object|null`, `status: completed|failed|not_evaluable`, `events: event[]`, `jobs: job_result[]`, `metrics: object|null`, `error_code: string|null` | FIFO and Priority parameters are `null`; `completed` requires events, jobs, and metrics; `failed` and `not_evaluable` require `metrics=null`. |
| `comparison_record` | `comparison_id: string`, scenario/snapshot identity, `proposal: proposal_record`, `runs: strategy_run[4]`, `evaluation: object` | `runs` has four fixed slots and never drops a row after AI failure; a comparison cannot be overwritten. |

Canonical JSON means UTF-8, object keys sorted ascending, array order preserved, and no insignificant whitespace. `snapshot_id` is the lowercase SHA-256 hex digest of canonical snapshot JSON. `comparison_id` is the SHA-256 digest of canonical JSON containing `scenario_id`, `job_table_version`, `snapshot_id`, `end_time`, grid-search parameters, and `proposer_version`. Set `proposal_request_id="<comparison_id>:proposal"`, `candidate_id="<comparison_id>:ai"`, and each `strategy_run_id="<comparison_id>:<strategy_id>"`. These rules make identity reproducible without a database.

### 4.3 Implementation Layout and Runtime

The future competition repository uses Python with `uv`. Keep one direction of dependency: UI calls orchestration; orchestration calls proposer, simulator, and evaluator; domain modules never import UI.

```text
app.py                       Gradio entry point and orchestration
src/records.py               record types and validation
src/scenarios.py             frozen tables and snapshot creation
src/policies.py              FIFO, Priority, weighted policy, grid search
src/simulator.py             deterministic event engine
src/proposer.py              one constrained OpenAI call per scenario
src/evaluator.py             metrics and fixed gates
src/presentation.py          table and Plotly timeline conversion
tests/test_records.py        invalid data and null rules
tests/test_simulator.py      event order, snapshot, endpoint, determinism
tests/test_policies.py       formulas, ties, all nine pairs
tests/test_evaluator.py      metrics and every gate branch
tests/test_proposer.py       schema, timeout, no retry, duplicate request
tests/test_fixtures.py       section 5.4 and section 11 expected outcomes
```

Runtime dependencies are `openai`, `gradio`, and `plotly`; tests use `pytest`, and lint uses `ruff`. Domain records use Python standard-library dataclasses or typed dictionaries; do not add a database or validation framework. Start the demo with `uv run python app.py`. Required configuration is `OPENAI_MODEL`; credentials come from each participant's process environment and are never displayed, logged, copied into records, or stored in repository files. A missing model or credential produces `unavailable`, while the three non-AI rows remain usable.

“Accepted” in interface means candidate passed eligibility checks for this simulation only. It does not mean deployment to a real system or an automatic switch.

## 5. Background Report Jobs and Scenario Contract

### 5.1 A Job

This candidate handles only background-report jobs. It does not call them orders or borrow CPU-process semantics.

| Field | Definition, validity, and visibility |
| --- | --- |
| `id` | Non-empty, unique string; used only for tracing and tie-breaking. Same-tick arrivals sort by ascending ASCII `id`. |
| `arrival_time` | Non-negative integer; visible only after that tick's arrival phase. The newly arrived cohort is `[checkpoint_time, end_time)`; arrivals at `end_time` are excluded. |
| `actual_duration` | Positive integer; only simulator reads it to determine completion time. Policy, AI, and gates cannot read it. |
| `priority` | Only `urgent` or `normal`; visible after arrival. The Priority baseline sorts with it; the weighted policy excludes it from the score. |
| `deadline` | Integer with `deadline >= arrival_time`; `deadline > end_time` is a valid future deadline; `completed_at <= deadline` is on time. |
| `worker_id` | Fixed as `worker-1` and `worker-2`; nodes are equivalent and each handles at most one job per tick. |
| preemption | Prohibited. A started job cannot be cancelled, moved, reordered, or have its `actual_duration` changed. |

At every tick, process completion first, then arrival, then snapshot or dispatch. At checkpoint `C`, completion occurs before arrivals at `C` join; the first branch dispatch occurs only after the snapshot is formed. Therefore, jobs completed at `C` are complete before the snapshot and excluded from the cohort, while jobs arriving at `C` belong to it. Every dispatch before `C` is fixed FIFO and is unaffected by AI or weighted strategy. At `E`, process only completion of running jobs and then stop; do not process arrival or dispatch at `E`. A job is still on time only if `completed_at <= deadline`, not merely because it completed at `E`.

FIFO chooses the earliest `arrival_time`, then ASCII `id`. Priority always chooses `urgent` first, then the earliest `arrival_time` and ASCII `id` within the same priority. Neither uses weights. The weighted policy uses the deadline-urgency and waiting-compensation formula in section 6.

When the waiting queue is empty, policy must return `null` and simulator does not dispatch. When it is non-empty, policy must return the one valid `job_id` from `dispatch_view`; `null`, an unknown `job_id`, a duplicate started job, or an unstable ordering is an explicit `simulator_error`, which immediately terminates that run. Never silently skip it or choose another job.

### 5.2 Fixed Scenario and Snapshot

Scenarios use the frozen, finite, versionable job-event tables in sections 5.4 and 11. State at checkpoint after completion and arrival, but before dispatch, forms snapshot. FIFO, Priority, grid-search selected strategy, and the scenario's AI candidate all replay from that same snapshot branch to the same endpoint.

`snapshot` must contain `scenario_id`, `job_table_version`, `checkpoint_time`, `end_time`, the position of the next unarrived event, waiting `job_id` values in fixed order, two `worker_state` values, completed history before `C`, and event sequence. Running jobs are preserved on their original workers with hidden `remaining_duration` and cannot be dispatched again; jobs completed before `C` remain only in history and are excluded from cohort and metrics.

Rule prevents swapping jobs or initial states during comparison. AI may see summary of surge effects, but cannot read arrivals, actual processing times, `remaining_duration`, or validation-scenario results after snapshot. AI waiting time stops simulation clock, so API latency does not count as job wait time.

### 5.3 Deterministic Two-node Simulator

Same job table, snapshot, strategy, and settings must produce same event record and metrics. Two processing nodes are equivalent and non-preemptive. Any random source must be fixed or replaced by frozen job table. Rendering must not change simulation state.

When nodes become free in the same tick, they choose jobs one at a time in `worker-1`, `worker-2` order; a selected job cannot be selected again. Completion events sort by worker name and arrival events by ASCII `job_id`; every event records `phase` and `sequence`. This gives simultaneous two-node dispatch a unique trace.

### 5.4 Frozen Scenario Pack

The MVP uses exactly one development scenario and two validation scenarios. These synthetic tables are product fixtures, not competition facts or production data. Their rows, `C`, and `E` are frozen before model evaluation. The development scenario is `fixture_two_node_fixed` in section 11; grid search may read its complete outcomes. AI evaluation uses only the two validation scenarios below and receives only each checkpoint summary.

Both validation scenarios use `job_table_version="v1"`, `C=30`, and `E=90`. At t=0, `warm-a` and `warm-b` start under the common FIFO prefix; both complete during the t=30 completion phase and are excluded from the evaluation cohort.

#### Validation A: urgent surge

`scenario_id="scenario-a-v1"` (the UI display name is `Urgent surge`; AI receives only the non-semantic identifier).

| `id` | `arrival_time` | `actual_duration` | `priority` | `deadline` |
| --- | ---: | ---: | --- | ---: |
| `warm-a` | 0 | 30 | `normal` | 30 |
| `warm-b` | 0 | 30 | `normal` | 30 |
| `normal-1` | 20 | 20 | `normal` | 80 |
| `normal-2` | 20 | 20 | `normal` | 85 |
| `urgent-1` | 20 | 5 | `urgent` | 38 |
| `urgent-2` | 20 | 5 | `urgent` | 45 |
| `urgent-3` | 20 | 5 | `urgent` | 50 |

FIFO must produce `overdue_count=3`, all urgent, with `max_wait=35`. Priority must produce `overdue_count=0` and `max_wait=20`. These are baseline regression checks, not an acceptance promise for AI.

#### Validation B: protect long-waiting normal work

`scenario_id="scenario-b-v1"` (the UI display name is `Protect long-waiting normal work`; AI receives only the non-semantic identifier).

| `id` | `arrival_time` | `actual_duration` | `priority` | `deadline` |
| --- | ---: | ---: | --- | ---: |
| `warm-a` | 0 | 30 | `normal` | 30 |
| `warm-b` | 0 | 30 | `urgent` | 30 |
| `normal-1` | 10 | 10 | `normal` | 45 |
| `normal-2` | 10 | 10 | `normal` | 50 |
| `urgent-1` | 20 | 20 | `urgent` | 75 |
| `urgent-2` | 20 | 20 | `urgent` | 75 |
| `urgent-3` | 30 | 3 | `urgent` | 34 |

FIFO must produce `overdue_count=1` (`urgent-3`) and `max_wait=30`. Priority must produce `overdue_count=3` (`urgent-3`, `normal-1`, `normal-2`) and `max_wait=43`. The weighted policy `(2,1)` must produce `overdue_count=0` and `max_wait=23`; this proves the fixture can reward both deadline pressure and waiting protection. It does not predetermine which weights AI must choose.

## 6. Strategies, AI Candidate, and Baselines

### 6.1 Fixed Baselines

Every comparison contains at least following four rows. Do not hide them when AI is rejected:

1. FIFO.
2. Priority.
3. Limited-weight strategy selected by grid search.
4. AI candidate.

Grid search tries all nine pairs in `{0,1,2} × {0,1,2}` on development scenarios only. It ranks pairs lexicographically by: lowest `overdue_count`, lowest `unfinished_count`, lowest `max_wait`, then lowest `(urgency_weight, waiting_compensation_weight)`. A `null` `max_wait` ranks before any numeric value because it means the cohort is empty. The selected pair is frozen before validation and applied unchanged to every validation scenario.

Grid search and AI use different information. Grid search sees complete development-scenario results. AI sees only the allowed summary at the current scenario's checkpoint. The interface must state this difference and must not describe the comparison as identical training conditions.

### 6.2 Weighted Scheduling Policy

For each arrived, unstarted job at simulation time `now`, calculate:

```text
time_to_deadline = deadline - now
urgency_pressure = clamp(10 - time_to_deadline, 0, 10)
waiting_pressure = clamp(now - arrival_time, 0, 10)

score =
  urgency_weight × urgency_pressure
  + waiting_compensation_weight × waiting_pressure
```

`clamp(value, 0, 10)` limits a value to the inclusive range `0..10`. A job due now or already overdue has `urgency_pressure=10`; a job with at least ten ticks remaining has `urgency_pressure=0`. Both pressures use the same fixed range. The horizon `10` and allowed integer weights `{0,1,2}` are fixed product constants and must not be tuned after validation results are seen.

Choose the job with highest `score`. Ties use earliest `arrival_time`, then ASCII `id`. If both weights are `0`, every score is zero and this tie rule makes the policy equivalent to FIFO. `priority` does not enter this score: it remains the input for the Priority baseline and the grouping field for fairness metrics.

The policy may read only `now`, `id`, `arrival_time`, `deadline`, and `priority` from `dispatch_view`. It must not read `actual_duration`, hidden remaining time, future arrivals, baseline results, or evaluator results. It returns `null` only when no job is waiting. For a non-empty queue, an unknown id, `null`, or more than one id is `simulator_error`.

Hand check at `now=30`: job A has `deadline=32`, `arrival_time=29`; job B has `deadline=40`, `arrival_time=23`. With weights `(1,2)`, A scores `1×8 + 2×1 = 10`; B scores `1×0 + 2×7 = 14`, so B runs first. This deliberately shows that sufficient waiting compensation may outweigh deadline pressure.

### 6.3 Constrained AI Proposal

The evaluated object is one frozen proposer procedure, identified by `proposer_version`. Its prompt template, model settings, input schema, output schema, allowed weights, timeout, and no-retry rule are frozen before validation. Each scenario creates exactly one proposal slot and calls this same procedure exactly once at that scenario's checkpoint. Different scenarios may produce different weight pairs; each output has its own `candidate_id` and must not be called the same candidate.

The AI input contains only this schema:

```json
{
  "scenario_id": "scenario identifier without development/validation label",
  "job_table_version": "version",
  "checkpoint_time": 30,
  "waiting": {
    "total_count": 0,
    "urgent_count": 0,
    "normal_count": 0,
    "max_observed_wait": null,
    "nearest_deadline_in": null
  },
  "workers": {
    "busy_count": 0,
    "idle_count": 2
  },
  "completed_by_checkpoint": {
    "total_count": 0,
    "urgent_count": 0,
    "normal_count": 0,
    "completed_late_count": 0
  },
  "allowed_weights": [0, 1, 2],
  "score_horizon": 10
}
```

All counts are non-negative integers. `nearest_deadline_in` is the minimum `deadline-checkpoint_time` among waiting jobs and is `null` when none wait; it may be negative for an already overdue job. `max_observed_wait` is `null` when none wait. The summary must not contain actual or remaining durations, individual future jobs, post-checkpoint events, baseline/search results, evaluator results, or whether a scenario is used for development or validation.

AI does not dispatch individual jobs or generate code. It may output only:

```json
{
  "urgency_weight": 0,
  "waiting_compensation_weight": 0,
  "rationale": "Rationale based only on the observed summary"
}
```

Both weights must be JSON integers in `{0,1,2}`. `rationale` must be a non-empty string of at most 500 characters. Extra fields, wrong types, values outside the set, empty rationale, or non-JSON output are `malformed_output`. The caller, not the model, generates `candidate_id` and records `proposal_request_id`, `proposer_version`, provider, model/version when available, measured latency, and terminal status.

Use `proposer_version="deadline-wait-v1"`. The system instruction is fixed verbatim before validation:

```text
You select two integer weights for a deterministic background-report scheduler.
Use only the supplied observed summary. Never infer future jobs or processing times.
urgency_weight controls pressure from an approaching deadline.
waiting_compensation_weight protects jobs that have waited longer.
Each weight must be 0, 1, or 2.
Return one JSON object only, with exactly urgency_weight,
waiting_compensation_weight, and rationale. Do not return Markdown or extra fields.
The rationale must use only supplied facts and must be at most 500 characters.
```

The user message is the canonical JSON serialization of the input schema. The caller derives `proposal_request_id` and `candidate_id` exactly as section 4.2 specifies; neither id is sent to the model.

Set temperature to `0` when supported. Timeout is 15 seconds. Do not retry, switch model, repair malformed output with another model call, or substitute a human answer. Timeout, unavailable model, malformed output, or internal failure records “AI candidate not evaluable”; FIFO, Priority, and grid-search results still run and remain visible. The exact response is stored with the run record for audit, but the prompt must contain no credentials or personal data.

## 7. Fair Evaluation and Evaluator Gates

### 7.1 Comparison Conditions

Each strategy uses same frozen job list, two equivalent nodes, same snapshot, and same comparison period. Results include scenario name, job-table version, snapshot identifier, settings, and event record. Same random seed alone is insufficient; actual job table and initial state must be reused.

Validation contains exactly the two frozen scenarios in section 5.4. Their job tables, checkpoint, endpoint, proposer procedure, and gates must be frozen before any validation run. Each scenario receives one AI call using only its checkpoint summary. Validation scenarios must not tune grid-search parameters, the proposer prompt, constants, or gates. Small synthetic replays support conclusions inside those replays only; they cannot prove generalization or real operating value.

### 7.2 Metrics and Gates

The evaluation cohort consists of jobs that arrived before `C` but were unfinished at the snapshot, plus jobs arriving in `[C,E)`. Jobs completed before `C`, including in the completion phase at `C`, are excluded. Each cohort job may be overdue in exactly one way: `completed_late` (`completed_at > deadline`) or `unfinished_due` (`completed_at = null` and `deadline <= E`). The categories are mutually exclusive; `overdue_count` counts their union and must never double-count.

| Metric | Exact calculation | Empty-set and boundary behavior |
| --- | --- | --- |
| `completed_late_count` | Cohort jobs completed with `completed_at > deadline` | `completed_at = deadline` is on time. |
| `unfinished_count` | Cohort jobs with `completed_at = null` | Includes jobs whose deadlines have not yet passed. |
| `unfinished_due_count` | Cohort jobs unfinished with `deadline <= E` | Combines with completed late to form overdue, without duplication. |
| `unfinished_not_due_count` | Cohort jobs unfinished with `deadline > E` | Valid future-deadline jobs; not overdue. |
| `overdue_count` | `completed_late_count + unfinished_due_count` | Count for an empty set is `0`. |
| `wait_time` | `started_at-arrival_time` when started; `E-arrival_time` when unstarted (censored wait) | Every cohort job has exactly one non-negative integer. |
| `max_wait` | Maximum `wait_time` across the cohort | `null` for an empty cohort; never write `0`. |
| `p95_wait` | Sort waits ascending and take item `ceil(0.95*n)` (nearest-rank) | `null` when `n=0`; include `wait_sample_count=n`. |

Every count, `max_wait`, and `p95_wait` is presented for total, `urgent`, and `normal` groups. For an empty priority subgroup, counts are `0` and wait values are `null`. Results must also include `scenario_id`, `job_table_version`, `snapshot_id`, `C`, `E`, strategy parameters, the complete `event_record`, and per-job results, so they can be recomputed.

For every validation scenario, compare that scenario's AI candidate with FIFO from the same snapshot. The scenario passes only when all gates pass:

1. `AI.overdue_count <= FIFO.overdue_count - 1`.
2. `AI.unfinished_count <= FIFO.unfinished_count`.
3. For both `urgent` and `normal`, AI must not increase `overdue_count` or `unfinished_count`.
4. `AI.max_wait <= FIFO.max_wait`; two `null` values pass, while a numeric AI value against FIFO `null` fails.

If FIFO has `overdue_count=0`, gate 1 has no improvement space and the scenario does not establish AI benefit. Do not replace the scenario or relax the gate after seeing this result. The proposer procedure establishes AI benefit only if every frozen validation scenario passes. A `not_evaluable` proposal cannot pass. With no queue-capacity limit, rejected-job count is fixed at zero.

`evaluation.status` is `accepted`, `rejected`, or `not_evaluable`. It contains four ordered gate records with `gate_id`, FIFO value, AI value, `passed`, and a plain-language reason. `not_evaluable` has no numeric gate result and records the proposal error. Overall proposer status contains both scenario evaluation statuses and is `accepted` only when both are accepted; otherwise it is `rejected` or `not_evaluable` when any required proposal cannot be evaluated.

Evaluator outputs pass/fail and reason for every gate. Rejecting a regressing AI candidate proves process works; it does not prove AI benefit.

### 7.3 Complete Results

Every demo presents all four rows: FIFO, Priority, grid search, and AI candidate, including when AI is not evaluable or rejected. Do not show only averages or one attractive case. Retain complete results and replayable records for each validation scenario.

## 8. Gradio and Plotly Demo Interface

Gradio is a simple Python interactive interface. Plotly creates numeric charts. The team has selected Gradio + Plotly for this candidate. The interface reads one identical computed record only.

The interface states are fixed as `no_scenario`, `loaded`, `proposing`, `evaluable`, `not_evaluable`, `completed`, and `failed`. The Run button must be disabled while `proposing`; repeated clicks must not create a second run. Terminal states show only existing results and must not automatically retry, switch models, switch candidates, or create a new proposal. This MVP provides no re-proposal action.

Each scenario has exactly one proposal slot with a stable `proposal_request_id`. Before the call, a single-process lock must atomically reserve that id as `in_flight` and write it to `comparison_record`; concurrent or repeated requests for the same id only read the existing in-flight or terminal result and must never call the API again. Both success and failure are terminal without retry; reloading an existing `comparison_record` also reads only its result and makes no API call. Do not add a database: a single-process lock and in-memory record are sufficient.

Minimal interface has four areas:

1. Observed queue summary: scenario, job-table version, snapshot, and data visibility scope.
2. AI proposal: two constrained weights, rationale, model status, and not-evaluable reason.
3. Complete comparison: always display the FIFO, Priority, grid-search, and AI-candidate rows together. Each row includes total, `urgent`, and `normal` overdue and unfinished values; `max_wait`; `p95_wait`; sample count; strategy parameters; `strategy_run_id`; and gate reasons. The area shows the shared `comparison_id`. Do not replace this with charts, omitted rows, or averages.
4. Two-node timeline: start/completion order of same jobs under each strategy, to explain metric differences.

The user selects one frozen scenario and presses `Run comparison` once. The UI loads or creates its snapshot, reserves the proposal request, obtains the single AI proposal, runs all evaluable branches, evaluates gates, then renders one `comparison_record`. Selecting another scenario creates that scenario's independent comparison. Returning to a completed scenario reloads its record and never calls the model again.

Plotly timeline and charts aid explanation. They cannot replace complete table, overdue count, or unfinished count. If time slider, step playback, or animation is added later, it may read computed records only. It must not trigger a new model call, swap jobs, or change evaluation.

If AI fails, retain the AI row and show `status=not_evaluable` with exactly one reason: `malformed_output`, `timeout`, `unavailable`, or `internal_error`. Metrics and gates in that row are always `null`; never fill them with `0` or copy another row. FIFO, Priority, and grid search remain fully visible. `failed` means only that the scenario or calculation could not establish a result; it must show error type and known identity data and must not mislabel a partial result as successful.

## 9. Approximately 90-second Demo Story

This section is an **internal team rehearsal candidate**, not confirmed organizer presentation time or format. Demo should explain following story in approximately 90 seconds:

| Section | Visible content | What judges should understand |
| --- | --- | --- |
| Problem | Queue summary before and after urgent-job surge | Fixed FIFO may not balance urgent and long-waiting jobs. |
| Limits | Snapshot, visible data, and two AI weights | AI has no per-job control and cannot see future. |
| Comparison | Four complete result rows from same snapshot | Comparison is fair and includes simple baselines. |
| Judgment | Result of each fixed gate | Evaluator can reject regressing recommendations. |
| Limits and conclusion | Offline replay, validation scenarios, and AI result | This is auditable simulation evidence, not production results. |

If confirmed organizer presentation time, judging format, or delivery rules differ, reorganize this script. Do not treat approximately 90 seconds as official requirement.

### Tentative Internal Rehearsal Cadence

Table totals 90 seconds and is for internal rehearsal only. Results for each section must be visible. It is not development schedule or organizer rule.

| Time | Visible content | Visible acceptance result |
| --- | --- | --- |
| 0–15 seconds | Queue summary before and after urgent-job surge | FIFO baseline pressure is visible. |
| 15–30 seconds | Snapshot, visible-data scope, and AI-weight limits | Audience can verify comparison starts from same snapshot and AI cannot see future. |
| 30–45 seconds | AI weights and rationale, or error/timeout state | Real AI output is traceable; malformed output or timeout explicitly says “AI candidate not evaluable.” |
| 45–70 seconds | Complete comparison of FIFO, Priority, grid search, and AI rows | Three non-AI comparisons remain visible; when AI is not evaluable, no fallback impersonates it. |
| 70–85 seconds | Result of each FIFO protection gate | Show acceptance or rejection and reason; grid search is informational comparison only. |
| 85–90 seconds | Offline replay and limits | Clearly say this is not live deployment or real benefit and does not prove AI always performs better. |

## 10. Pre-implementation Work Boundary

This repository is for planning, not competition implementation. This PRD does not authorize competition source code, AI agent loops, RL pipelines, reward functions, or reusable implementation. It also does not change `build-handoff/`.

The organizer has confirmed that the topic may change before formal development. Overall product approval and implementation handoff remain pending. Before formal implementation, the team must approve this product direction and freeze the evaluator gates and demo claims; the job semantics, scenario pack, weight set, AI procedure, failure behavior, and UI technology are specified in this candidate.

## 11. Test Plan and Definition of Done

### Test Items (Apply After Approval)

- Re-running same job table, snapshot, settings, and strategy produces same event record and metrics.
- Simulator always follows completion, arrival, dispatch event order and two-node limits.
- Strategies cannot read unarrived jobs, actual processing times, post-snapshot data, or validation results.
- FIFO, Priority, nine-candidate grid, and each scenario's AI candidate use same strategy interface and same snapshot.
- Deadline urgency, waiting pressure, clamp, tie-breaking, and all-zero weights match the hand calculations in section 6.2.
- Each validation scenario creates one proposal request; concurrent or repeated requests never cause a second model call.
- The same frozen `proposer_version` is used in every validation scenario, while each scenario keeps its own `candidate_id`.
- Evaluator independently tests every gate, every rejection reason, and deliberately regressing candidate.
- On malformed AI output, timeout, or unavailability, clearly show not evaluable without affecting fixed-baseline results.
- Interface shows all four result rows, data-visibility scope, snapshot identifier, and each decision.

### Fixed Two-node Hand-calculation Fixture

This fixture validates the contracts for time, snapshot, FIFO, Priority, cohort, and metrics. It does not use weights, grid search, or AI. Settings: `scenario_id="fixture_two_node_fixed"`, `job_table_version="v1"`, `C=3`, `E=10`, and nodes `worker-1` and `worker-2`.

| `id` | `arrival_time` | `actual_duration` | `priority` | `deadline` | Purpose |
| --- | ---: | ---: | --- | ---: | --- |
| `job-a` | 0 | 3 | `normal` | 3 | Completes in the completion phase at C; excluded from the cohort. |
| `job-b` | 0 | 3 | `urgent` | 3 | Completes in the completion phase at C; excluded from the cohort. |
| `job-c` | 3 | 4 | `normal` | 5 | Normal job arriving in the same tick. |
| `job-d` | 3 | 4 | `normal` | 7 | Normal job arriving in the same tick. |
| `job-e` | 3 | 1 | `urgent` | 4 | Urgent job arriving in the same tick. |
| `job-f` | 3 | 1 | `normal` | 6 | Normal job arriving in the same tick. |

Common prefix: at t=0, FIFO dispatches `job-a` to `worker-1` and `job-b` to `worker-2`; at t=3, `job-a` and `job-b` complete first, then `job-c`, `job-d`, `job-e`, and `job-f` arrive, the snapshot is created, and only then does dispatch occur. The cohort is exactly `job-c` through `job-f`.

| Strategy | Expected branch trace (`worker: job[start,completion]`) | Expected wait values |
| --- | --- | --- |
| FIFO | `worker-1: job-c[3,7], job-e[7,8]`; `worker-2: job-d[3,7], job-f[7,8]` | `job-c=0`, `job-d=0`, `job-e=4`, `job-f=4` |
| Priority | `worker-1: job-e[3,4], job-d[4,8]`; `worker-2: job-c[3,7], job-f[7,8]` | `job-e=0`, `job-c=0`, `job-d=1`, `job-f=4` |

| Strategy | overdue total / urgent / normal | unfinished total / urgent / normal | `max_wait` | `p95_wait` (`n=4`) |
| --- | --- | --- | ---: | ---: |
| FIFO | 3 / 1 / 2 (`job-c`,`job-e`,`job-f`) | 0 / 0 / 0 | 4 | 4 |
| Priority | 3 / 0 / 3 (`job-c`,`job-d`,`job-f`) | 0 / 0 / 0 | 4 | 4 |

Verify each item: `job-a` and `job-b` are not in the cohort; neither strategy has `unfinished_due`; both have `completed_late_count=3`; FIFO `p95_wait` takes the fourth item of sorted `[0,0,4,4]`, and Priority takes the fourth item of `[0,0,1,4]`. Any different trace or metric is an implementation error, not an acceptable strategy difference.

This fixture is also the only grid-search development scenario. Every pair with `urgency_weight>0` must dispatch in order `job-e, job-c, job-f, job-d`, with `overdue_count=2`, `unfinished_count=0`, and `max_wait=2`; the three pairs with `urgency_weight=0` produce `overdue_count=3`. Under the tie rule in section 6.1, grid search must choose `(urgency_weight=1, waiting_compensation_weight=0)`. Validation may apply only this fixed result and must not search again.

### Demo Checks

- Four Gradio areas are understandable on one demo screen.
- Plotly two-node timeline and table link to same event record.
- Accepted, rejected, and AI-not-evaluable states all have clear labels.
- Offline computed record can reproduce demo with no new model call.

### Definition of Done

After product approval, the team can replay FIFO, Priority, frozen grid-search strategy, and one real AI candidate per scenario—or explicit AI-not-evaluable state—from the same frozen job table and snapshot. The same frozen proposer procedure runs once per scenario. Interface shows complete results, fixed gates, per-gate reasons, and limits. A deliberately regressing candidate must be rejected. All section 11 automated and hand checks pass. Demo must not claim live deployment, real business value, or that AI necessarily outperforms simple search.

## 12. Settings and Assumptions

- Job-table version, scenario name, snapshot time, end time, and event order are visible settings. Sections 5.4 and 11 fix their MVP values.
- Two equivalent nodes, non-preemption, no queue-capacity limit, and synthetic demo data are current assumptions.
- Each weight uses `0`, `1`, or `2`; score horizon is `10`; grid search has nine parameter pairs.
- AI makes exactly one proposal per scenario. The frozen `proposer_version` is the cross-scenario object being evaluated; individual outputs are separate candidates.
- Evaluator gates must lock before comparison and be recorded with results; AI must not change them.
- Record model name/version and latency only when available. Never write API keys, tokens, or personal credentials into this repository or demo record.
- If any assumption becomes real-service integration, preemption, more nodes, or dynamic capacity, re-estimate scope and validation method.

## 13. Risks, Open Questions, and Next Step

### Main Risks and Mitigations

| Risk | Mitigation |
| --- | --- |
| Unfair comparison | Freeze job table, display snapshot identifier, fix event order, and test identical replay. |
| AI adds no value | Show all baselines and rejection results; narrow claim to safe validation. |
| AI unavailable | Show not-evaluable state; retain fixed baselines and search; do not impersonate AI result. |
| Demo misleads as live deployment | Label every acceptance result as offline simulation eligibility check. |
| Scope expansion | Exclude RL, arbitrary code, strategy libraries, persistence, and real integration. |

### Open Questions

1. Does the team approve this overall product direction for the competition?
2. Does the team approve the listed evaluator gates and “auditable and rejectable” fallback story?
3. Which `OPENAI_MODEL` available during the event will be recorded as the runtime model?
4. What are formal presentation time, judging format, submission portal, and post-deadline material rules?

### Next Step

The team next approves or rejects the evaluator gates and overall candidate. Before overall approval, maintain planning documents only. Do not treat this content as an implementation specification already handed off.
