# Adaptive Scheduler Arena

> Hackathon MVP PRD · MiniGrid integration · 90-second vertical slice

## 1. Product Summary

**Adaptive Scheduler Arena** is a controlled MiniGrid environment for demonstrating how an AI agent adapts a scheduling policy when a workload changes abruptly.

The MVP models a platform/operations engineer's problem: a normally healthy order queue is hit by a Flash Sale, causing arrival-rate and queue pressure to rise. An OpenAI-powered Planner observes the workload regime, selects or composes a scheduling skill, proposes executable Code-as-Policy, and reflects on the result. The candidate policy is executed in a constrained sandbox and accepted only when a deterministic evaluator confirms that all hard KPI gates pass.

The goal is not to train the largest model or claim production readiness. The goal is a clear, reproducible, judge-readable proof that **model adaptation can produce policy adaptation**:

`Observe → Plan → Execute → Evaluate → Reflect → Accept/Rollback → Store`

## 2. Target User and Problem

### Primary user

Platform and operations engineers who need to keep service quality stable while workload distributions change.

### Problem

Static scheduling policies can work under normal load and fail under a sudden regime shift. Operators need to understand:

- which workload signal caused degradation;
- which scheduling strategy is appropriate;
- whether a proposed change actually improves service;
- when to safely deploy, rollback, or reuse a previously successful strategy.

### Product promise

The Arena makes each decision observable: the event, diagnosis, policy/code diff, KPI movement, evaluator decision, and stored skill are visible in one flow.

## 3. MVP Scope and Non-goals

### In scope

- One custom MiniGrid environment: `AdaptiveSchedulerEnv`.
- One primary event: `FLASH_SALE`, injected at simulated step 20.
- A 60-step deterministic evaluation window (`1 step = 1 simulated second`).
- Initial skills: FIFO/FCFS, SJF, Priority, Round Robin, and EDF.
- LLM Planner + Reflection at policy level.
- Constrained Python Code-as-Policy sandbox.
- Multi-metric hard-gate evaluator with fixed-seed replay.
- Session Skill Library persisted as JSON artifacts.
- Four-zone Gradio interface and fixed 90-second demo.
- Deterministic Hybrid Priority-SJF fallback.

### Non-goals

- PPO training or online RL weight updates during the hackathon.
- Per-job LLM dispatch decisions.
- Four-event coverage in the MVP.
- Production deployment, live trading, or unverified business/financial performance claims.
- A parallel simulator outside MiniGrid.

## 4. System Architecture

```text
MiniGrid AdaptiveSchedulerEnv
        │ reset / step / observation / render
        ▼
WorkloadState ──► OpenAI Planner ──► PolicyCandidate
      ▲                                  │
      │                                  ▼
      │                         Policy Sandbox
      │                                  │
      └──── Simulator ◄──── policy execution
                         │
                         ▼
                    Evaluator
              accept / rollback / reflect
                         │
                         ▼
                 Skill Library (JSON)
```

The LLM decides **what strategy to try**. The policy engine decides **how each job is dispatched**. The environment and evaluator remain deterministic and authoritative.

## 5. MiniGrid Environment Contract

### 5.1 Environment class

Implement a custom `MiniGridEnv` subclass named `AdaptiveSchedulerEnv` using the standard Gymnasium interface:

```python
env = AdaptiveSchedulerEnv(config=arena_config)
obs, info = env.reset(seed=seed)
obs, reward, terminated, truncated, info = env.step(action)
rgb = env.render()
```

Required behavior:

- `reset(seed=...)` fully resets queue, workers, policy, event schedule, metrics, and RNG state.
- `step(action)` advances exactly one simulated second.
- `render()` returns an RGB representation for Gradio; RGB is not the Planner's primary input.
- A fixed seed produces identical arrivals, processing times, events, dispatches, and KPI traces.
- `terminated` is true after the configured 60-step evaluation horizon; `truncated` is reserved for safety/time limits.

### 5.2 Grid semantics

The grid is a visual and stateful representation of the scheduling world:

| Grid/state element | Meaning |
| --- | --- |
| Pending-order cells | Jobs waiting in the ready queue |
| Worker/server cells | Available and busy processing capacity |
| VIP/high-value marker | Priority/value-sensitive job |
| Deadline/SLA marker | Urgent or at-risk job |
| Policy/status panel | Current skill and adaptation state |
| Event indicator | `NORMAL`, `FLASH_SALE`, or fallback/error state |

The queue and worker engine are the source of truth. Rendering must not change simulation state.

### 5.3 Observation space

Use a structured `gymnasium.spaces.Dict` observation. The exact tensor dtypes may be implemented freely, but the semantic fields are fixed:

```python
{
    "queue_stats": {
        "length": int,
        "arrival_rate": float,
        "avg_wait": float,
        "p95_latency": float,
        "deadline_miss_rate": float,
        "vip_ratio": float,
    },
    "workers": {
        "capacity": int,
        "busy": int,
        "failed": int,
    },
    "event": str,
    "current_policy": str,
    "recent_metrics": list[dict],
    "recent_trace": list[dict],
    "available_skills": list[str],
}
```

The Planner receives a serialized `WorkloadState` derived from this observation. Gradio may additionally display `render()` output.

### 5.4 Action space

Actions are policy-level macro-actions, not job-level choices:

```text
SELECT_SKILL(skill_id)
COMPOSE_POLICY(skill_ids, parameters)
REQUEST_REFLECTION(candidate_id)
DEPLOY_CANDIDATE(candidate_id)
```

The simulator invokes the selected policy for each dispatch. The LLM cannot choose an individual job or bypass the evaluator.

### 5.5 Events and workload generation

- Steps `0–19`: `NORMAL` workload; FIFO is the baseline policy.
- Step `20`: inject `FLASH_SALE`; arrival rate increases by the configured multiplier and queue pressure becomes visible.
- Steps `20–59`: evaluate adaptation and recovery.

The workload generator must support fixed seeds, deterministic replay, configurable worker capacity, processing time, priority, deadline, value, and category.

### 5.6 Reward

Return a normalized KPI-delta reward for RL compatibility, for example:

```text
reward_t =
  + α · Δthroughput
  − β · Δp95_latency
  − γ · Δdeadline_miss_rate
  + δ · Δvalue_served
  − ε · Δstarvation_penalty
```

Weights and normalization are configuration values. Reward is useful feedback only; evaluator hard gates decide acceptance.

## 6. Planner and Reflection Contract

### 6.1 Planner input

The OpenAI model receives:

- current `WorkloadState`;
- recent event and KPI trace;
- current policy;
- available `SkillRecord`s;
- allowed scheduler API specification;
- evaluation window and hard constraints.

The prompt must explicitly prohibit per-job LLM control, unsafe code, unsupported imports, and claims of production or live-trading results.

### 6.2 PolicyCandidate envelope

The Planner must return machine-validated JSON with this semantic shape:

```json
{
  "candidate_id": "hybrid-priority-sjf-v1",
  "base_skill_ids": ["priority", "sjf"],
  "rationale": "Flash Sale creates queue pressure and VIP urgency; combine priority with shorter-job preference.",
  "policy_code": "def select_job(queue, now, api): ...",
  "parameters": {
    "priority_weight": 0.7,
    "urgency_weight": 0.2,
    "short_job_weight": 0.1
  },
  "eval_window_sec": 60,
  "constraints": {
    "p95_improvement_min": 0.10,
    "throughput_delta_min": 0.0,
    "deadline_miss_delta_max": 0.0,
    "fairness_drop_max": 0.05
  }
}
```

The implementation may add fields, but these fields and meanings are required. Invalid JSON, missing fields, unsupported skills, or code that fails validation must be rejected before execution.

### 6.3 Reflection

After a rejected candidate, Reflection receives the `EvaluationResult` and rejection reason. It may revise the candidate or recommend retrieval of another skill, but it cannot alter hard gates, write directly to the Skill Library, or deploy without a new evaluator pass.

## 7. Policy Sandbox

The sandbox executes only the policy entry point against immutable job snapshots and an allow-listed scheduler API.

Required safeguards:

- reject arbitrary imports;
- reject file, network, subprocess, shell, and system access;
- enforce CPU/time and memory limits;
- enforce a bounded number of dispatch operations;
- expose no credentials or host filesystem;
- return a structured error for timeout, exception, invalid job selection, or non-deterministic behavior.

The sandbox must support code diff display and deterministic re-execution for replay.

## 8. Evaluator and Skill Library

### 8.1 Evaluation metrics

Every baseline and candidate run records:

- throughput;
- P95 latency;
- deadline miss rate;
- value served;
- fairness;
- starvation penalty;
- aggregate score;
- event and seed metadata;
- execution trace.

### 8.2 Hard-gate decision

For each candidate:

1. Run the fixed 60-second Flash Sale scenario.
2. Compare against the same-seed baseline.
3. Replay with three fixed validation seeds.
4. Require every gate to pass on the configured aggregate result.
5. Accept and deploy only on success; otherwise rollback and invoke Reflection or fallback.

Default gates:

```text
P95 latency improvement >= 10%
Throughput delta       >= 0
Deadline miss delta    <= 0
Fairness drop          <= 0.05
```

### 8.3 SkillRecord and persistence

An accepted skill records:

```json
{
  "skill_id": "hybrid-priority-sjf-v1",
  "policy_code": "...",
  "parameters": {},
  "event_signature": "flash_sale/high_queue_pressure",
  "metrics": {},
  "seeds": [11, 23, 37],
  "trace_artifact": "artifacts/traces/hybrid-priority-sjf-v1.json",
  "created_at": "..."
}
```

The MVP uses in-session memory plus JSON artifacts. Retrieval must match event signature and workload features, show the reused skill in the UI, and never skip evaluator validation for a newly deployed run.

## 9. Gradio Interface

The interface must communicate the complete causal story at a glance:

1. **Environment**: MiniGrid RGB render, queue state, worker state, Flash Sale control.
2. **Policy**: current policy, candidate policy, code diff, Planner rationale.
3. **Metrics**: live P95 latency, throughput, deadline miss, fairness, score, and baseline comparison.
4. **Adaptation**: Skill Library, reflection result, accept/rollback decision, fallback status, and artifact identifier.

Color language:

- green: healthy/recovered;
- yellow: pressure or candidate evaluation;
- red: critical degradation, rejection, or failure.

Human-triggered and deterministic/random modes must use the same state and color language.

## 10. 90-Second Demo Acceptance Script

| Time | Visible behavior | Judge takeaway |
| --- | --- | --- |
| 0–20s | Normal workload; FIFO performs steadily | Baseline is understandable |
| 20s | Trigger Flash Sale | Workload regime changes |
| 30s | KPI degradation appears | Static policy is insufficient |
| 35s | Planner identifies queue pressure/HOL blocking | LLM explains the failure |
| 45s | Hybrid Priority-SJF candidate and code diff appear | Policy adaptation is concrete |
| 60s | KPI recovers | Improvement is measurable |
| 70s | Candidate passes gates and is stored | System is auditable and reusable |
| 80–90s | Similar event retrieves stored skill | Self-evolution means repertoire growth |

## 11. Team Workflow and Integration

- **Neo**: Planner, MiniGrid/shared contracts, sandbox, integration, pitch.
- **于喬**: queue/worker simulation, workload generator, backend.
- **昊祁**: Gradio render, controls, charts, demo flow.
- **家紳**: scheduling algorithms, evaluator, rollback, Skill Library.

Branch ownership remains:

```text
feat/planner-policy
feat/environment-simulator
feat/gradio-ui
feat/evaluator-library
```

`src/shared/contracts.py` is protected. Contract changes require a separate PR, merge before dependent work is rebased, and review by a teammate plus the coding agent. `main` must remain smoke-testable.

Recommended implementation order:

1. Freeze contracts and MiniGrid reset/step/observation behavior.
2. Implement deterministic workload, queue, workers, and baseline skills.
3. Implement evaluator and hard gates before enabling LLM deployment.
4. Add sandbox and deterministic fallback.
5. Add Planner/Reflection adapter and Skill Library artifacts.
6. Connect Gradio and rehearse the fixed script.
7. Run fresh-clone smoke test and prepare fallback demo data.

## 12. Test Plan and Definition of Done

### Automated tests

- MiniGrid `reset`, `step`, `seed`, termination, and observation schema.
- Flash Sale injection exactly at step 20.
- Same seed produces identical workload, trace, and KPI values.
- Macro-actions cannot dispatch a specific job through the LLM path.
- Sandbox rejects unsafe imports, filesystem/network access, timeout, and invalid dispatch.
- Evaluator independently tests each hard gate, rejection reason, and rollback.
- Planner malformed JSON, API timeout, and Reflection failure invoke fallback.
- Skill JSON serialization, loading, matching, and reuse.

### Manual/demo tests

- Four Gradio zones are readable in one screen.
- Code diff, rationale, KPI recovery, gate decision, and skill reuse are visible.
- Fallback is clearly labeled and completes the same 90-second flow.
- Fresh clone starts with one documented command and reproduces the fixed-seed demo.

### Definition of done

The team can run the fixed Flash Sale scenario from a fresh checkout, show baseline degradation, show an LLM-generated or deterministic-fallback policy change, pass the hard gates, persist the accepted skill, and reuse it within 90 seconds without claiming production deployment or live-trading performance.

## 13. Configuration and Assumptions

- OpenAI model ID is supplied through an environment variable.
- API keys are never committed to the repository or included in artifacts.
- Seeds, event step, evaluation horizon, gate thresholds, and fallback policy are configuration values with the documented defaults above.
- MiniGrid is the sole MVP Arena environment; future environments may implement the same contract.
- The RL-ready interface is preserved for future PPO/baseline work, but no training run is required for hackathon acceptance.
