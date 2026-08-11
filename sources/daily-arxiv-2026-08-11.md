# Daily arXiv scan — 2026-08-11

## Scope

- Gap interval: after the second 2026-08-10 scan through 2026-08-11 08:43 UTC.
  The `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, and `cs.LG` new-listing pages had
  advanced to the **Tuesday, 11 August 2026** announcement batch.
- The arXiv API returned 1,253 records submitted from 2026-08-07 through
  2026-08-11 in the target categories. The first 1,000, sorted newest first,
  were screened with title/abstract terms covering harnesses, agentic robots,
  VLAs, world/action models, memory, recovery, skills, and evaluation. Existing
  README, landscape, and source records were checked before promotion.
- Facts below were verified from official arXiv records and, where available,
  official project or code repositories. Performance numbers remain
  author-reported unless explicitly stated otherwise.

## Included

### XPolicyLab: A Unified Standard and Open Ecosystem for Robot Policy Evaluation and Deployment

- Paper: https://arxiv.org/abs/2608.09892
- Project: https://xpolicylab.github.io/
- Code: https://github.com/XPolicyLab/XPolicyLab
- Submitted: 2026-08-10.
- Classification: Agentic Robot/VLA Harness; Evaluation.
- Why included: defines common observation, action, trajectory, lifecycle, and
  reset contracts, then isolates policy and environment dependencies behind a
  client/server boundary. The public repository integrates 42 policies and
  supports RoboTwin, RoboDojo, and standardized real-robot evaluation.
- Evidence boundary: integration-effort and evaluation results are
  author-reported; the repository and its adapter/runtime implementation were
  independently verified as public.

### SHE: Trajectory-driven Safety Harness Evolution for LLM Agents

- Paper: https://arxiv.org/abs/2608.09885
- Submitted: 2026-08-10.
- Classification: General Harness Methodology; Evaluation/Safety.
- Why included: decomposes a safety harness into System Prompt, Rule Bank,
  Safety Memory, and Tool Policy, attributes rollout failures to an artifact,
  and promotes localized changes through safety/utility validation.
- Evidence boundary: the reported 3.1x attack-success-rate reduction is on
  Agent-SafetyBench, with held-out AgentHarm transfer. There is no robot
  experiment and no official code link was located as of 2026-08-11.

### Evo-Bench: Can Language Models Improve Agent Harness?

- Paper: https://arxiv.org/abs/2608.09096
- Submitted: 2026-08-10.
- Classification: General Harness Methodology; Evaluation.
- Why included: explicitly benchmarks intrinsic harness-evolution ability while
  separating framework improvement from base-model strength and using
  sensitivity-aware splits to test transfer rather than same-task overfitting.
- Evidence boundary: Search, Office, and General are digital-agent domains;
  robotics transfer is unvalidated and no official code or benchmark release
  was linked as of 2026-08-11.

### Agentic Harnesses: LLM-Driven Verification Layers for Robot Autonomy

- Paper: https://arxiv.org/abs/2608.09857
- Submitted: 2026-08-10.
- Classification: Agentic Robot/VLA Harness; Evaluation/Safety.
- Why included: places an LLM-judge ensemble between a planning server and the
  robot-facing MCP server, with approve, reject-for-reformulation, and
  human-escalation outcomes. It makes semantic plan permissibility an explicit
  middleware contract rather than relying on the planner itself.
- Evidence boundary: the authors report about 85% three-way precision and 97%
  adversarial containment. No public code or independent hardware validation
  was located as of 2026-08-11.

### HarnessWAM: Bridging Prediction and Deliberation in World Action Models

- Paper: https://arxiv.org/abs/2608.09516
- Submitted: 2026-08-10.
- Classification: Agentic Robot/VLA Harness; Robot Foundation/World Model.
- Why included: adds a task manager, evidence-grounded scene belief, structured
  task graph, capability-constrained skill projection, and event-driven
  dual-timescale feedback around a WAM, including local recovery without
  discarding acquired scene state.
- Evidence boundary: RoboMemArena and RoboCerebra results are author-reported;
  no official code or project link was located as of 2026-08-11.

### Skills in Weights, Memory in Code: Hybrid Learning for Memory-Dependent Robot Manipulation

- Paper: https://arxiv.org/abs/2608.09410
- Submitted: 2026-08-10.
- Classification: Agentic Robot/VLA Harness.
- Why included: HyMeS keeps reusable motor skills in a Markovian VLA while a
  coding agent iteratively evolves executable memory-management heuristics from
  rollout feedback; multimodal stage-completion verification closes the loop.
- Evidence boundary: RoboMemArena gains are author-reported, and no official
  code or project link was located as of 2026-08-11.

### Rethink Before You Execute: Adaptive Execution for World Action Models

- Paper: https://arxiv.org/abs/2608.09492
- Submitted: 2026-08-10.
- Classification: Agentic Robot/VLA Harness; Runtime.
- Why included: TempoWAM replaces a fixed executed prefix with a recurrent
  progress monitor and adaptive replanning protocol, turning chunk commitment
  into an evidence-driven runtime decision.
- Evidence boundary: the authors report 26.9% fewer WAM calls without reduced
  success on easier real-robot tasks and a 13.3-point success gain on difficult
  tasks. No official code link was located as of 2026-08-11.

### WorldSimProbe: Diagnosing Simulator Faithfulness in Action-Conditioned World Models

- Paper: https://arxiv.org/abs/2608.09298
- Submitted: 2026-08-10.
- Classification: Robot Foundation/World Model; Evaluation.
- Why included: formalizes an Observable Simulator Contract—actions must cause
  corresponding robot motion and environment responses must be grounded in that
  motion—and tests six open ACWMs on more than 18,000 instances across RoboTwin,
  ManiSkill, and LIBERO.
- Evidence boundary: findings are author-reported; no official benchmark code or
  dataset release was linked as of 2026-08-11.

### Compiling and Benchmarking Task-State Horizons for Embodied Agents

- Paper: https://arxiv.org/abs/2608.08036
- Submitted: 2026-08-08.
- Classification: Agentic Robot/VLA Harness; Evaluation.
- Why included: defines task-state horizon separately from action length and
  releases the reported RoboGraph compiler/benchmark design for constructing
  causal state-transition graphs with failures and interventions. Fifteen
  agentic models are evaluated over 588 episodes and 84 scenes.
- Evidence boundary: results are author-reported; no official public repository
  was located as of 2026-08-11, so “release” here refers to the paper's described
  benchmark rather than verified downloadable artifacts.

## Reviewed but not promoted

- **VANE** (`2608.09448`) offers reversible, future-evidence-gated test-time VLA
  adaptation, but the reported average gain is modest and embodiment-dependent,
  with no released implementation.
- **JEPA-WAM** (`2608.09381`) and **4D-WAM** (`2608.08023`) are relevant model
  advances, but are primarily representation/training contributions and expose
  no verified artifacts; the curated list prioritizes the stronger interface and
  evaluation contributions in this batch.
- **ALVA** (`2608.08273`) is an interpretable trajectory feedback mechanism, but
  the evaluation is limited to simulated household environments and no reusable
  robot-harness artifact was verified.
- **Ouroboros** (`2608.08311`) is a notable self-developing coding harness, but
  its benchmark claims and live-agent experiment are not robot results; SHE and
  Evo-Bench more directly fill the safety/evaluation gaps relevant here.
- **Auditing Instruction-Trajectory Mismatches** (`2608.07895`) is valuable data
  quality work with real-robot experiments, but it is a dataset-cleaning method
  rather than a runtime, foundation model, or reusable evaluation harness.
- Narrow policy-learning, perception, navigation, teleoperation, conventional
  control, driving, and generic video-world-model papers were excluded unless
  they introduced a reusable interface for state, recovery, serving, safety, or
  evaluation.

## Artifact-status checks

- XPolicyLab: official project page and active public implementation repository
  verified on 2026-08-11.
- SHE, Evo-Bench, Agentic Harnesses, HarnessWAM, HyMeS, TempoWAM,
  WorldSimProbe, and RoboGraph: no official code, dataset, or weights repository
  was linked from the arXiv record or located through official repository search
  as of 2026-08-11.
