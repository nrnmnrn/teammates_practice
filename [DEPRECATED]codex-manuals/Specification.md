# Adaptive Scheduler Arena — Technical Specification

> Implementation specification derived from `docs/hackathon-prd.md`.
> Audience: the four-person development team and coding agents.
> Language: Traditional Chinese with English technical terms, schemas, and code examples.

## 1. Purpose and Engineering Principles

Adaptive Scheduler Arena is a deterministic MiniGrid-based environment that demonstrates policy adaptation under a sudden workload shift. The implementation must make the complete causal chain observable:

```text
Observe → Plan → Execute → Evaluate → Reflect → Accept/Rollback → Store
```

The environment and evaluator are authoritative. The OpenAI model proposes policy-level changes; it does not dispatch individual jobs, modify hard-gate thresholds, or execute unrestricted code.

### 1.1 MVP boundaries

The hackathon MVP includes one custom MiniGrid environment, one `FLASH_SALE` event, initial deterministic scheduling skills, Planner + Reflection, a constrained Code-as-Policy sandbox, a hard-gate evaluator, JSON Skill Library artifacts, and a Gradio demo.

The MVP does not include PPO training, online RL weight updates, per-job LLM control, four-event coverage, production deployment, live trading, or unverified business-performance claims.

### 1.2 Repository layout

```text
app.py                         # Gradio entrypoint and demo orchestration only
src/arena/
├── environment/               # AdaptiveSchedulerEnv and workload engine
├── planner/                   # OpenAI Planner and Reflection adapters
├── policies/                  # Built-in skills and policy execution API
├── evaluator/                 # Metrics, hard gates, replay, rollback
├── sandbox/                   # AST validation and subprocess execution
├── ui/                        # Gradio state/update handlers
└── shared/                    # Pydantic contracts and shared enums
tests/                         # pytest unit and integration tests
artifacts/
├── runs/                      # Per-run state/KPI traces
├── skills/                    # Accepted SkillRecord JSON files
└── replays/                   # Fixed-seed replay results
```

`app.py` must not contain simulation, LLM, policy, or evaluator business logic. It creates the dependency graph, starts Gradio, and exposes the nominal demo command:

```text
python app.py
```

## 2. Configuration Defaults

All values are configuration fields with the following reproducible defaults:

| Key | Default | Meaning |
| --- | ---: | --- |
| `grid_width` | `16` | MiniGrid width |
| `grid_height` | `10` | MiniGrid height |
| `worker_count` | `4` | Available workers/servers |
| `normal_arrival_rate` | `2` | Jobs generated per simulated step before the event |
| `flash_sale_multiplier` | `3` | Arrival-rate multiplier at step 20 |
| `processing_time_min` | `1` | Minimum job processing time |
| `processing_time_max` | `6` | Maximum job processing time |
| `deadline_min` | `8` | Minimum relative deadline |
| `deadline_max` | `20` | Maximum relative deadline |
| `vip_ratio` | `0.2` | Fraction of VIP/high-value jobs |
| `evaluation_horizon` | `60` | Simulated seconds per evaluator run |
| `flash_sale_step` | `20` | Event injection step |
| `validation_seeds` | `[11, 23, 37]` | Fixed replay seeds |
| `planner_attempts` | `3` | Maximum Planner calls |
| `reflection_attempts` | `3` | Maximum Reflection calls |
| `llm_timeout_sec` | `15` | Timeout per OpenAI call |

The normal success path targets a 90-second live demonstration. The target is nominal rather than a global hard cutoff because LLM success has priority. If all allowed Planner/Reflection attempts fail, the deterministic fallback must still complete the demo.

The OpenAI model identifier is supplied through an environment variable such as `OPENAI_MODEL`. API keys are read only from environment variables and are never committed or written to artifacts.

## 3. Shared Contracts

`src/arena/shared/contracts.py` is the only source of truth for cross-module data. Use Pydantic models with strict validation and JSON serialization. Modules may add private implementation details, but must not change the public field meanings without a contract-change PR.

### 3.1 Core enums

```python
from enum import Enum

class EventType(str, Enum):
    NORMAL = "NORMAL"
    FLASH_SALE = "FLASH_SALE"

class ArenaPhase(str, Enum):
    NORMAL = "NORMAL"
    DEGRADED = "DEGRADED"
    PLANNING = "PLANNING"
    EXECUTING = "EXECUTING"
    EVALUATING = "EVALUATING"
    REFLECTING = "REFLECTING"
    RECOVERED = "RECOVERED"
    ROLLED_BACK = "ROLLED_BACK"
    STORED = "STORED"
    FALLBACK = "FALLBACK"

class MacroActionType(str, Enum):
    SELECT_SKILL = "SELECT_SKILL"
    COMPOSE_POLICY = "COMPOSE_POLICY"
    REQUEST_REFLECTION = "REQUEST_REFLECTION"
    DEPLOY_CANDIDATE = "DEPLOY_CANDIDATE"
```

### 3.2 Job

```python
class Job(BaseModel):
    job_id: str
    arrival_time: int = Field(ge=0)
    processing_time: int = Field(ge=1)
    priority: int = Field(ge=0)
    deadline: int = Field(ge=0)
    value: float = Field(ge=0)
    category: str
    vip: bool
    started_at: int | None = None
    completed_at: int | None = None
    worker_id: str | None = None
```

`deadline` is represented as an absolute simulated-time deadline after workload generation. Mutable execution fields are changed only by the environment engine, never by Planner code.

### 3.3 WorkloadState and observations

```python
class WorkloadState(BaseModel):
    step: int = Field(ge=0)
    event: EventType
    queue_length: int = Field(ge=0)
    arrival_rate: float = Field(ge=0)
    avg_wait: float = Field(ge=0)
    p95_latency: float = Field(ge=0)
    deadline_miss_rate: float = Field(ge=0, le=1)
    vip_ratio: float = Field(ge=0, le=1)
    worker_capacity: int = Field(ge=1)
    busy_workers: int = Field(ge=0)
    failed_workers: int = Field(ge=0)
    current_policy_id: str
    available_skill_ids: list[str]
    recent_metrics: list[dict[str, float]]
    recent_trace: list[dict[str, object]]
```

The Gymnasium observation is a `spaces.Dict` carrying the same semantic fields. String fields may be encoded as bounded IDs internally, but the Planner-facing serialization must preserve readable names.

### 3.4 Actions and decisions

```python
class MacroAction(BaseModel):
    type: MacroActionType
    skill_ids: list[str] = []
    candidate_id: str | None = None
    parameters: dict[str, float | int | str] = {}

class SchedulingDecision(BaseModel):
    selected_job_id: str
    worker_id: str
    policy_id: str
    reason: str
```

`SchedulingDecision` is produced by the policy engine. A Planner or Reflection call may produce only `MacroAction`/`PolicyCandidate`, never a direct job dispatch.

### 3.5 PolicyCandidate

```python
class PolicyCandidate(BaseModel):
    candidate_id: str = Field(pattern=r"^[a-z0-9][a-z0-9-]{2,63}$")
    base_skill_ids: list[str] = Field(min_length=1)
    rationale: str = Field(min_length=1, max_length=2000)
    policy_code: str = Field(min_length=1, max_length=20000)
    parameters: dict[str, float | int | str] = {}
    eval_window_sec: int = Field(default=60, ge=1, le=60)
    constraints: dict[str, float] = {
        "p95_improvement_min": 0.10,
        "throughput_delta_min": 0.0,
        "deadline_miss_delta_max": 0.0,
        "fairness_drop_max": 0.05,
    }
```

The required Planner JSON keys are `candidate_id`, `base_skill_ids`, `rationale`, `policy_code`, `parameters`, `eval_window_sec`, and `constraints`. Extra keys are ignored only after validation; malformed or missing required keys are rejected.

### 3.6 Execution and evaluation results

```python
class ExecutionError(BaseModel):
    code: str
    message: str
    retryable: bool

class ExecutionResult(BaseModel):
    policy_id: str
    decisions: list[SchedulingDecision]
    error: ExecutionError | None = None
    elapsed_ms: int = Field(ge=0)

class EvaluationResult(BaseModel):
    candidate_id: str
    seeds: list[int]
    throughput: float = Field(ge=0)
    p95_latency: float = Field(ge=0)
    deadline_miss_rate: float = Field(ge=0, le=1)
    value_served: float = Field(ge=0)
    fairness: float = Field(ge=0, le=1)
    starvation_penalty: float = Field(ge=0)
    aggregate_score: float
    gate_results: dict[str, bool]
    accepted: bool
    rejection_reason: str | None
    trace_artifacts: list[str]
```

### 3.7 SkillRecord and DemoState

```python
class SkillRecord(BaseModel):
    skill_id: str
    policy_code: str
    parameters: dict[str, float | int | str]
    event_signature: str
    metrics: dict[str, float]
    seeds: list[int]
    trace_artifact: str
    created_at: str

class DemoState(BaseModel):
    phase: ArenaPhase
    current_policy_id: str
    candidate_id: str | None
    latest_state: WorkloadState
    latest_evaluation: EvaluationResult | None
    fallback_active: bool
    status_message: str
```

## 4. MiniGrid Environment

### 4.1 Class and lifecycle

```python
class AdaptiveSchedulerEnv(MiniGridEnv):
    def __init__(self, config: ArenaConfig): ...
    def reset(self, *, seed: int | None = None, options: dict | None = None): ...
    def step(self, action: MacroAction | dict): ...
    def render(self) -> RGBArray: ...
```

`reset()` resets queue, workers, policy, event schedule, metrics, trace, and RNG. `step()` advances exactly one simulated second. `terminated` becomes true after step 60; `truncated` is reserved for safety limits. `render()` is read-only.

### 4.2 Grid mapping

The fixed 16×10 grid contains queue lanes, four worker/server cells, event/status indicators, VIP/high-value markers, and SLA/deadline markers. The queue/worker engine is the source of truth; the renderer is a projection and must never mutate jobs or metrics.

### 4.3 Workload generation

At each step, generate jobs from the configured seeded RNG. Each job receives processing time in `[1, 6]`, an absolute deadline derived from `[8, 20]`, priority/value/category, and VIP status with probability `0.2`.

- Steps `0–19`: `NORMAL`, arrival rate `2`.
- Step `20`: set event to `FLASH_SALE` and arrival rate to `normal_arrival_rate × 3`.
- Steps `20–59`: continue arrivals and dispatch using the active policy.

The same seed must reproduce job IDs, job attributes, dispatch decisions, completions, metrics, and trace ordering.

### 4.4 Observation and action semantics

The observation is a structured `Dict` derived from `WorkloadState`. RGB is for Gradio only. Macro-actions are restricted to selecting a skill, composing a candidate, requesting reflection, or deploying a validated candidate. The environment must reject any action containing a direct job-selection command from the LLM path.

### 4.5 Reward

Return a normalized KPI-delta reward for RL compatibility:

```text
reward_t =
  + α · Δthroughput
  − β · Δp95_latency
  − γ · Δdeadline_miss_rate
  + δ · Δvalue_served
  − ε · Δstarvation_penalty
```

Weights and normalization are configuration values. Reward does not override evaluator hard gates.

## 5. Built-in Policies and Policy API

Implement deterministic, side-effect-free policy callables behind one allow-listed API:

```python
class SchedulerAPI(Protocol):
    def jobs(self) -> Sequence[ReadOnlyJob]: ...
    def now(self) -> int: ...
    def worker_capacity(self) -> int: ...

PolicyFunction = Callable[[Sequence[ReadOnlyJob], int, SchedulerAPI], str]
```

The return value is a valid `job_id` from the supplied snapshot. Initial skills:

- `fifo`: earliest arrival first;
- `sjf`: shortest processing time first;
- `priority`: highest priority first;
- `round-robin`: bounded fair rotation;
- `edf`: earliest deadline first;
- `hybrid-priority-sjf`: deterministic fallback using priority, urgency, and shorter processing time.

Tie-breaking must be stable: `(policy_key, arrival_time, job_id)` so fixed-seed replays remain identical.

## 6. Planner and Reflection

### 6.1 Interfaces

```python
class Planner:
    def plan(self, state: WorkloadState, skills: list[SkillRecord]) -> PolicyCandidate: ...

class Reflection:
    def revise(
        self,
        candidate: PolicyCandidate,
        result: EvaluationResult,
    ) -> PolicyCandidate | None: ...
```

The OpenAI adapter serializes only the required state, traces, available skills, allowed API, and hard constraints. The prompt must prohibit unsafe code, unsupported imports, per-job decisions, threshold changes, and production/live-trading claims.

### 6.2 Retry and fallback behavior

- Planner: at most 3 calls, each with a 15-second timeout.
- Reflection: at most 3 calls, each with a 15-second timeout.
- Validate every response with Pydantic before sandbox execution.
- Invalid JSON or validation failure may be retried with a format-correction prompt.
- If all attempts fail, activate deterministic Hybrid Priority-SJF and set `fallback_active=True`.
- The live 90-second demo target is nominal; the UI must show a waiting/retry state rather than silently changing policy.

## 7. Policy Sandbox

The sandbox must expose only an immutable job snapshot and the allow-listed `SchedulerAPI`.

### 7.1 Validation pipeline

1. Parse the candidate `policy_code` with Python AST.
2. Reject imports, attribute access outside the allow-list, file/network/system/subprocess calls, dynamic execution, global mutation, and unbounded loops.
3. Compile only the validated function entry point.
4. Execute in an isolated subprocess with CPU/time, memory, and dispatch-count limits.
5. Return `ExecutionResult` or a typed `ExecutionError`.

The sandbox must never receive API keys, host filesystem paths, or unrestricted environment variables. Candidate code cannot write Skill Library artifacts directly.

## 8. Evaluator, FSM, and Persistence

### 8.1 Evaluator interface

```python
class Evaluator:
    def evaluate(
        self,
        baseline_policy: PolicyCandidate,
        candidate: PolicyCandidate,
        seeds: list[int],
    ) -> EvaluationResult: ...
```

Run the fixed 60-step Flash Sale scenario for baseline and candidate, then replay seeds `11`, `23`, and `37`. Aggregate metrics and apply all gates:

```text
P95 latency improvement >= 10%
Throughput delta       >= 0
Deadline miss delta    <= 0
Fairness drop          <= 0.05
```

Acceptance is atomic: only a candidate passing every gate may become current policy and be persisted. Otherwise retain the previous policy, record the rejection reason, and enter Reflection or Fallback.

### 8.2 Finite-state machine

```text
NORMAL
  └─ workload degradation detected → DEGRADED
DEGRADED → PLANNING
PLANNING → EXECUTING
EXECUTING → EVALUATING
EVALUATING ── all gates pass → RECOVERED → STORED
EVALUATING ── gate fails → REFLECTING → PLANNING
PLANNING/EXECUTING ── API, validation, or sandbox failure → FALLBACK
EVALUATING ── unsafe/invalid result → ROLLED_BACK → REFLECTING or FALLBACK
```

Every transition emits a timestamped trace event containing the prior phase, next phase, reason, and correlation ID.

### 8.3 SkillStore

```python
class SkillStore:
    def save(self, record: SkillRecord) -> Path: ...
    def retrieve(self, state: WorkloadState) -> list[SkillRecord]: ...
```

Write accepted skills to `artifacts/skills/<skill_id>.json`, run traces to `artifacts/runs/<run_id>.json`, and replay summaries to `artifacts/replays/<run_id>.json`. Retrieval ranks matching event signature and workload features. A retrieved skill still requires evaluator validation before deployment.

## 9. Gradio UI Contract

The UI consumes `DemoState` and exposes event controls plus read-only views. It must have four visible zones:

1. **Environment**: MiniGrid RGB render, queue, workers, and Flash Sale control.
2. **Policy**: current/candidate policy, code diff, Planner rationale.
3. **Metrics**: P95 latency, throughput, deadline miss, fairness, aggregate score, baseline comparison.
4. **Adaptation**: Skill Library, Reflection output, accept/rollback, fallback, and artifact paths.

Use green for healthy/recovered, yellow for pressure/evaluation, and red for critical/rejected/fallback states. Human-triggered and deterministic modes must share the same state model and visual language.

## 10. Demo Runbook

```text
0–20s   Start NORMAL workload with FIFO baseline.
20s     Inject FLASH_SALE.
30s     Show KPI degradation and queue pressure.
35s     Show Planner diagnosis of the regime shift/HOL blocking.
45s     Show Hybrid Priority-SJF candidate and code diff.
60s     Show KPI recovery and hard-gate result.
70s     Persist accepted SkillRecord and trace artifacts.
80–90s Retrieve and reuse the matching skill on a similar event.
```

If the live LLM call is still retrying, display the current phase and continue waiting. If all Planner/Reflection attempts fail, display deterministic fallback and continue the same causal story without claiming that the fallback was generated by the model.

## 11. Team Integration Rules

- **Neo** owns Planner, shared contracts, MiniGrid integration, sandbox, integration, and pitch.
- **于喬** owns queue/worker simulation, workload generator, and backend engine.
- **昊祁** owns Gradio render, controls, charts, and demo flow.
- **家紳** owns built-in scheduling policies, evaluator, rollback, and Skill Library.

Use the branches:

```text
feat/planner-policy
feat/environment-simulator
feat/gradio-ui
feat/evaluator-library
```

Contract changes in `src/arena/shared/contracts.py` require a separate PR. Merge the contract before dependent branches rebase. Every PR receives teammate review plus coding-agent review. `main` must remain smoke-testable after each integration step.

Recommended dependency order:

1. Shared Pydantic contracts and configuration.
2. MiniGrid reset/step/observation plus deterministic workload.
3. Built-in policies and evaluator hard gates.
4. Sandbox and deterministic fallback.
5. Planner/Reflection adapter and SkillStore.
6. Gradio bindings and demo rehearsal.
7. Fresh-clone launch and artifact verification.

## 12. Test Specification

Use `pytest` and Pydantic validation. Required scenarios:

### Environment

- `reset(seed)` clears all mutable state.
- Same seed produces identical jobs, dispatches, metrics, and traces.
- Grid is exactly 16×10 and render is read-only.
- `step(20)` injects `FLASH_SALE` exactly once.
- `terminated` is true at step 60.
- Observation contains all required semantic fields.

### Contracts and actions

- Missing/invalid `PolicyCandidate` fields are rejected.
- Constraint values are validated and cannot be changed by Reflection.
- Direct per-job LLM actions are rejected.
- Pydantic JSON round-trips preserve values and enums.

### Sandbox

- Reject imports, `open`, network, subprocess, shell, system calls, dynamic execution, and unbounded loops.
- Enforce timeout, memory, and dispatch limits.
- Return typed errors for exceptions, invalid job IDs, and non-determinism.

### Planner and failure paths

- Malformed JSON triggers format retry.
- API timeout and exhausted retries activate deterministic fallback.
- Reflection receives rejection reason and cannot bypass evaluator.

### Evaluator and persistence

- Test each hard gate independently.
- Verify rollback preserves the previous policy.
- Verify acceptance is atomic and writes SkillRecord only after passing all seeds.
- Verify SkillStore save/load/retrieve and artifact paths.

### Integration and UI

- `python app.py` starts the application from a fresh checkout.
- Gradio smoke test runs the complete nominal demo flow.
- UI shows event, code diff, KPI recovery, gate decision, skill reuse, and fallback state.

## 13. Definition of Done

The implementation is complete when a fresh checkout can run `python app.py`, reproduce the fixed-seed Flash Sale scenario in MiniGrid, show FIFO degradation, produce an LLM candidate or deterministic fallback, execute it through the sandbox, evaluate all hard gates across seeds `11/23/37`, persist an accepted skill and trace, and retrieve that skill on a similar event. No secrets, production-deployment claims, live-trading claims, or unsupported performance claims may appear in the UI or artifacts.
