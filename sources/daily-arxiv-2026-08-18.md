# Daily arXiv scan — 2026-08-18

## Scope

- Interval: after the 2026-08-17 second-pass cutoff (~08:20 UTC) through
  2026-08-18 08:33 UTC.
- The arXiv API now exposes the 2026-08-15, 2026-08-16, and 2026-08-17
  announcement batches that were not visible during the weekend scans. Query
  `submittedDate:[202608150000 TO 202608182359]` returned 769 unique records
  across `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, and `cs.LG`. Every entry's
  `updated` field equals its `published` date in this window, so no meaningful
  revision was indexed (the API normalizes `lastUpdatedDate` to
  `submittedDate`).
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action models,
  code-as-policy, skill discovery, memory, recovery, evaluation, runtime
  monitoring, and safety. Candidate claims were checked against the arXiv
  record, official project pages, and official code/model/data repositories;
  existing README, landscape, and source records were checked for duplicates.
  All performance results below remain author-reported unless stated otherwise.

## Included (new curated entries)

### Zetta ζ: An Efficient Closed-Loop Embodied Harness for Self-Evolving Physical Intelligence

- Paper: https://arxiv.org/abs/2608.16590
- Project: https://air-embodied-brain.github.io/zetta
- Code: https://github.com/air-embodied-brain/Zetta-Embodiment (public as of
  2026-08-18; no license asserted at verification time)
- Classification: Agentic Robot/VLA Harness; Self-improvement.
- Why included: a closed-loop embodied harness that evolves code-based runtime
  critics and recovery skills online while keeping the base policy frozen.
  Three timescale-separated loops separate action-frequency governance,
  rollout-level critic–recovery proposals, and validation-gated skill updates;
  Z-Infra decouples agent logic from heterogeneous execution resources. The
  authors report 90.8% on LIBERO-Pro and 93.6% on RoboCasa with an 11.1×
  inference speedup, zero-shot transfer of learned skills, and emergent "aha
  moments" under self-exploration. This is the first clearly harness-centric
  physical-agent entry in this batch.
- Boundary: results are author-reported (simulation benchmarks plus reported
  robot experience); the public repository is a fresh release (2026-08-18)
  with no license asserted.

### ClawGym II: Exploring Black-Box RL on Agent Harness

- Paper: https://arxiv.org/abs/2608.16798
- Classification: General Harness Methodology; Self-improvement.
- Why included: treats the harness as the optimization target by running
  reinforcement learning *through* opaque harnesses. A sandboxed rollout
  infrastructure isolates environments and harnesses, a serving proxy at the
  model boundary captures calls, multi-turn trajectories are reconstructed as
  prefix trees, and both PPO and GRPO optimize over the tree while maintaining
  training–inference consistency; mix-harness training lets one model be
  jointly optimized by heterogeneous harnesses. With Qwen3-30A3B the authors
  report Pass@1 gains of 9.98 and 14.81 points on ClawGym-Bench through
  OpenClaw and Claude Code, stable over 200–400 steps, with further gains on
  JobBench and OfficeQA.
- Boundary: digital long-horizon agents; no robot experiment and no official
  code link was located as of 2026-08-18.

### τ0-VLA: a Hierarchical Robot Foundation Model with World-Model-Guided Test-Time Computation

- Paper: https://arxiv.org/abs/2608.16885
- Project: https://tau0-vla.github.io/
- Code: https://github.com/sii-research/tau-0-vla (Apache-2.0, verified)
- Weights: https://huggingface.co/sii-research/tau-0-vla (verified)
- Classification: VLA; Robot Foundation Model.
- Why included: a hierarchical robot foundation model that treats high-level
  subtask generation as compute-scalable inference, using a world model to
  guide test-time search over candidate subtasks before commitment, with a
  low-level policy executing across embodiments. Trained on 40,115 hours of
  heterogeneous real-world data with multimodal co-training; the authors
  report that extra test-time compute improves next-subtask accuracy and
  closed-loop long-horizon success. Code and weights are open.
- Boundary: reported gains are author-reported; robot evidence is described at
  system level in the paper.

### StateM: Reaching 95.3% Raw Accuracy, or a $15 Frontier Run, on Terminal-Bench 2.1 via Harness Scaling

- Paper: https://arxiv.org/abs/2608.15089
- Code: https://github.com/henryqin1997/statem (Apache-2.0, verified; Python
  runtime package with tests, docs, and examples)
- Classification: General Harness Methodology.
- Why included: a concrete harness-scaling result with a released runtime.
  StateM organizes execution around durable states, phase-local context,
  checked transitions, recoverable runbooks, and versioned procedural
  practices. The authors report raising GPT-5.6 Sol xhigh to 95.3% raw
  accuracy on Terminal-Bench 2.1 (445 trials) at ~$15 final-score API usage
  versus $574.68 for the reference, with runbooks transferring across models
  (GPT-5.6 Luna 76.7 → 85.4%) and a frozen profile lifting DeepSeek-V4 Flash
  from 82.7 to 88.1%.
- Boundary: digital terminal agents; no robot experiment.

### Evo-Harness: Context-to-Harness Skill Compilation for Self-Evolving Agents

- Paper: https://arxiv.org/abs/2608.15071
- Code: https://github.com/A-EVO-Lab/a-evolve (branch `release/evo-harness`,
  MIT, verified; five benchmark pipelines)
- Classification: General Harness Methodology; Self-improvement.
- Why included: formalizes *online harness learning* — a frozen solver
  improves by continually updating a structured skill harness across a task
  stream. Context-to-harness compilation distills noisy single-shot
  executions into reusable cross-task and task-type guidance
  (`Select → Inject → Execute → Reflect → Compile → Update`), with all
  learning stored as inspectable Markdown skill files. Evaluated on
  TerminalBench2, SWE-bench, CL-Bench, and WebArena-Infinity.
- Boundary: digital agents; no robot experiment.

### Bounded Agents: Delegation Security for Multi-Agent AI Systems

- Paper: https://arxiv.org/abs/2608.15888
- Code: https://github.com/xmuruaga/bounded-agents (Apache-2.0, verified;
  reference implementation and evaluation artifact with 215 tests, zero
  runtime dependencies)
- Classification: General Harness Methodology; Evaluation/Safety.
- Why included: the Agentic Principal Chain (APC) makes delegated authority an
  enforced runtime object — six conjunctive checks (identity, scope and
  composition, context, approval, evidence, intent) evaluated over accumulated
  session state, with scope narrowing monotonically across delegation hops and
  composition checks over session history. The authors report AgentDojo
  exfiltration falling from 75–100% to 0%, all 544 InjecAgent data-stealing
  cases blocked, at a utility cost of 8.6–13.9 percentage points, with 0.24 ms
  p99 authorization latency. Public code and evaluation data are verified.
- Boundary: digital multi-agent systems; no robot experiment.

### SCOPE: Score-Isolated Agentic Optimization for Video World Models

- Paper: https://arxiv.org/abs/2608.15043
- Code: https://github.com/YuhuaJiang2002/SCOPE (public as of 2026-08-18; the
  repository's LICENSE_STATUS.md asserts no license, so it is not treated as
  openly licensed)
- Classification: General Harness Methodology; World Model.
- Why included: an auditable harness for inference-time adaptation of frozen
  video world models: external controls are a typed state, updated only
  through bounded, evidence-supported changes, with the resulting policy
  frozen before held-out evaluation (selection-aware conformal risk control,
  score-blind filtering, exact base fallback). The authors report +14.24
  (95% CI [+8.10, +21.23]) over the frozen base on Physics-IQ and find that
  gains do not transfer uniformly across backbones.
- Boundary: video-world-model domain, not robotics; artifact status is
  code-without-license.

### Aborted but Not Forgotten: KV-Cache Retention Breaks Rollback Consistency in Language Agents

- Paper: https://arxiv.org/abs/2608.15939
- Classification: General Harness Methodology; Recovery.
- Why included: a cross-layer correctness result for agent rollback: clearing
  a rejected branch from the application transcript is not a complete abort
  when the serving session retains key/value state, because the model can keep
  attending to content the application believes it discarded. The paper
  formalizes rollback consistency, introduces a same-token/different-cache
  audit, and shows retained KV alone flips a typed protected effect in 25 of
  63 audited cells across seven open-weight families (3.8B–36B), including
  under LangGraph time-travel; transaction-local cache restoration closes the
  channel without a global flush. Directly extends this repository's recovery
  theme (AgentRewind, Agentic Transaction) with an attended-state integrity
  requirement.
- Boundary: digital language agents; the paper states results are
  deterministic and reproducible from released artifacts, but no public
  artifact link was located on the arXiv record as of 2026-08-18.

### DeepInsight II: One Trace from Benchmark to Robot

- Paper: https://arxiv.org/abs/2608.16556
- Classification: Evaluation/Safety.
- Why included: evaluation continuity across the Physical AI stack. It
  reproduces released-checkpoint references on two navigation and four
  manipulation benchmarks, places four released whole-body controllers under
  one workload/metric contract (MotionBench), and carries a qualified cohort
  from parallel simulation to matched real-robot trials where simulated and
  physical rollouts share a parent trace identity, making the sim-to-real gap
  a native reduction. A composed System 2–1–0 study maps trace localization to
  five evidence-grounded handoff labels with concrete repair actions and a
  measured repairability criterion, tested under hardware-observable state.
- Boundary: an evaluation methodology and report; no new public artifacts were
  linked as of 2026-08-18.

### CaliBench: Are the Stochastic Dynamics of Video World Models Physically Calibrated?

- Paper: https://arxiv.org/abs/2608.16829
- Classification: Evaluation/Safety; World Model.
- Why included: benchmarks the *aleatoric calibration* of stochastic video
  world models by scoring generations in physically interpretable discrete
  outcome spaces (Galton boards, Bernoulli forks, dice/cards/lottery, roulette)
  whose reference distributions are known in closed form, decomposing
  performance into scorability and calibration (total variation distance), with
  a chi-squared significance test and a released metric (mean normalized total
  variation, mnTV). Across nine scenes and six image-to-video models the
  authors find pervasive miscalibration and outcome collapse, with no model
  dominating all scenes. Complements the existing GAUGE entry by testing
  distribution-level physical calibration rather than per-scene dynamics.
- Boundary: image-to-video models, not robot policies; the protocol and metric
  are described in the paper with no separate repository located as of
  2026-08-18.

### HarnessEval-W: Agentifying the Evaluation of Visual Worlds

- Paper: https://arxiv.org/abs/2608.16859
- Project: https://mirros-lab.github.io/HarnessEval-W
- Code: https://github.com/MirroS-Lab/HarnessEval-W (public as of 2026-08-18;
  no license asserted at verification time)
- Classification: Evaluation/Safety; World Model.
- Why included: brings the harness paradigm to world-model benchmarking: an
  agentified pipeline interprets each evaluation case, decomposes it into
  measurable subproblems, spawns specialized sub-agents with diagnostic tools,
  and has a parent agent validate evidence into a final verdict, producing a
  transparent evidence tree per rollout. Applied to 18 world models over 330
  cases; judgments reportedly align with human preferences while exposing
  fine-grained diagnoses. The pipeline is released as a live benchmark.
- Boundary: video world-model evaluation; no robot interface.

### RigidBench: Evaluating Rigid-Body Physics in Video Generation Models

- Paper: https://arxiv.org/abs/2608.15555
- Code: https://github.com/swarnim-j/RigidBench (MIT, verified)
- Dataset: https://doi.org/10.5281/zenodo.21649156 (verified)
- Classification: Evaluation/Safety; World Model.
- Why included: simulator-grounded benchmark that separates motion, geometry,
  identity, background stability, and visual similarity when scoring generated
  continuations against reference rollouts, with per-frame masks, depth, 6-DoF
  trajectories, and contacts. Rankings depend strongly on the measurement
  (higher SSIM correlates with larger 3D trajectory error, r = 0.89); the
  released 5,000-video training set with exact simulator state enables
  fine-tuning and intervention analysis (e.g., ~20% 3D trajectory error
  reduction for Wan 2.2 TI2V-5B). Code and dataset are verified public.
- Boundary: video generation models; evaluation evidence is benchmark-based.

### Security of Foundation-Model-Powered Embodied Agents: Attack Surfaces, Attacks, Defenses, and Evaluation

- Paper: https://arxiv.org/abs/2608.16843
- Classification: Evaluation/Safety (survey).
- Why included: a trust-boundary-centric survey of embodied-agent security that
  separates attack surface from attack mechanism, organizing the system into
  five layers and twelve attack surfaces (model supply chain, user
  instructions, context/memory, physical environments, perception, world
  state, reasoning, planning, action interfaces, middleware, multi-agent
  communication, execution control) and quantitatively analyzing 58 attack and
  61 defense records collected through 2026-08-15. It identifies underexplored
  boundaries (context and long-term memory, middleware/network, world-state
  integrity, multi-agent trust) and open evaluation challenges.
- Boundary: survey/analysis; no artifacts.

### Bit-Flip Attacks on Vision-Language-Action Models: Action-Decoding Architecture Shapes the Vulnerability

- Paper: https://arxiv.org/abs/2608.15475
- Code: arXiv ancillary material (`/src/2608.15475v1/anc/code`, verified)
- Classification: Evaluation/Safety (weight-integrity attacks).
- Why included: the first bit-flip (Rowhammer-style weight-fault) attack on
  VLAs: a few gradient-selected flips drive closed-loop success to 0%, damage
  concentrates in a few action-generating layers, and the flip budget depends
  sharply on the action head (1–5 flips for direct regression/token policies,
  ~100–300 for evaluated flow-matching policies). Task-calibrated emulated
  flips give 0/20 real-robot successes versus 14/20 clean, establishing weight
  integrity as a security boundary for embodied foundation models.
- Boundary: attack study on quantized VLAs; code is released as arXiv ancillary
  material rather than a maintained repository.

## Important artifact release (existing entry updated)

- **HELIX** (`2608.13951`) — the previously 404 `HKUDS/HELIX` repository is
  now live (MIT, TypeScript monorepo, created 2026-08-17): a harness
  composition, evaluation, and data-loop workspace that decomposes four
  product harnesses (OpenCode, Pi Mono, Nanobot, Hermes Agent) into 1,332
  atoms across eight dimensions with 96 standard swap ports, and reports
  outperforming upstream harnesses on LiveCodeBench and SWE-bench subsets. The
  README and landscape entries were updated to mark the implementation
  available; the paper's robot evidence remains unchanged (none).

## Reviewed but not promoted

- **GigaBrain-0.7** (`2608.15875`) — 37,000+ hour embodied foundation model
  with a three-system architecture; substantial system report, but the
  official blog and arXiv record state training code and weights *will be*
  released, so no artifact exists as of 2026-08-18. Watch for the release.
- **ViTaR** (`2608.15816`) — visuo-tactile residual adaptation for frozen VLAs
  with real-robot results; no official code or project page located.
- **HAF** (`2608.16837`) — humanoid whole-body loco-manipulation adaptation of
  generalist VLAs (hierarchical action flow + spectral latent RL; seven
  real-world tasks); project page exists but no code or weights.
- **SparkVLA** (`2608.16172`) — stop-aware hierarchical VLA with unified
  stop/chunk ranking and real-robot results; no code.
- **NebulaVLA** (`2608.16503`) — asynchronous dual-frequency VLA with a
  language-grounded action representation; overlaps existing async
  chunk-serving coverage and has no verified code.
- **PhaseLoRA** (`2608.15285`) and **StructRL** (`2608.15139`) — VLA PEFT /
  structured exploration for flow VLAs; incremental, no verified code (StructRL
  project page only).
- **ForceU-VLA** (`2608.15009`) — force-aware ultrasound VLA; the official
  repository contains only a README and assets (no code) and the dataset is a
  partial release on Hugging Face (`usvla/USForce`). Watch for full release.
- **US-VLA** (`2608.16074`) — ultrasound VLA with Apache-2.0 code verified;
  niche clinical-domain VLA without a general harness contribution; recorded
  for completeness.
- **FabriMAE** (`2608.16697`) — self-evaluating VLA via Markov attention
  entropy with a LIBERO-Reflect benchmark; no code or benchmark artifact
  located.
- **Neurosymbolic Embodied Agents** (`2608.16794`) — VLM exploration harness +
  PDDL-constrained decoding + MCTS for executable-by-construction household
  plans (>90% on VirtualHome/ALFWorld); conceptually relevant, but no code and
  no robot experiment.
- **When State Becomes an Attack Surface** (`2608.16806`) — state-semantic
  injection in LLM-driven embodied agents; USENIX Security 2027 submission with
  no public code yet.
- **Robo-Dopamine 2.0** (`2608.15680`) — history-/OOD-aware process reward
  model for VLA RL; no code.
- **PACE** (`2608.15026`) — phase-progress-aware credit assignment for VLA
  post-training; no code.
- **Dual-head coordination / runtime collapse certificate** (`2608.15748`) —
  interpretable runtime signal for flow-matching policies with MIT code
  verified, but a narrow mechanism study; recorded with artifact note.
- **GaussMemory** (`2608.14986`) — task-driven 3D Gaussian scene memory for
  long-horizon manipulation (IROS 2026); relevant but no code and overlaps
  existing persistent-memory coverage.
- **Remember Smarter** (`2608.15269`) — plug-and-play robotic memory (visual
  history compressor + hyperbolic experience space) for pi0; no code.
- **HyperSkill** (`2608.16114`), **SkillCommit** (`2608.15165`),
  **VCE-Skill** (`2608.16544`), **QUMem** (`2608.16168`) — digital-agent skill
  or memory evolution methods; no verified artifacts; incremental over covered
  themes.
- **Agent Gym** (`2608.15591`) — continuous evaluation/evolution loop for LLM
  agents; a reference implementation is described but no repository URL was
  verified.
- **TaoLive** (`2608.15763`) — harness-aware training (HAT) that makes harness
  states part of the training distribution; live-commerce avatar domain, no
  code.
- **A Policy Algebra for Trust-Preserving Agentic AI Execution**
  (`2608.16402`) — enterprise reliability-envelope runtime with audit
  evidence; no code.
- **Towards Risk-free AI Agent Deployment** (`2608.16411`) — position article
  on trajectory-grounded agent testing/debugging.
- **CompoSkill** (`2608.16246`) — compositional skill-chain attacks with
  CompoSkill-Bench; security-relevant but no verified artifacts.
- **Trusted-monitor ensembles** (`2608.16190`) — AI-control trusted-monitoring
  analysis; digital, no robot or artifact link.
- **When Agents Coordinate** (`2608.16801`) — coordination instrument for
  multi-agent coding; no artifact link.
- **The Working Set of a Coding Agent** (`2608.16630`) — coherence-debt
  analysis of five harnesses; digital-agent harness study, no artifact.
- **LongRCA Bench** (`2608.15242`) — long-horizon agent failure diagnosis
  benchmark (role + root-step); relevant to evaluation, but no artifact link.
- **AstronOS** (`2608.16381`) — unified execution model/runtime with versioned
  authoritative state; substantial but digital-agent and no code link.
- **Agent-Native Telemetry** (`2608.16178`) — verifiable state-delta telemetry
  for autonomous operations; relevant to observability, no code link.
- **Beyond Direct Access / ResourceHijackBench** (`2608.15108`) — agent
  resource-hijacking benchmark; no verified artifacts.
- **PDDLCoder** (`2608.16637`), **HaReCAP** (`2608.16447`), **TRCA**
  (`2608.16156`), **PIHF** (`2608.16831`) — planning/credit/grounding methods
  for digital or symbolic agents; incremental, no artifacts.
- **ScenarioCharacterization** (`2608.16041`) — Apache-2.0 safety
  characterization toolkit for trajectory datasets, but driving-domain;
  recorded with artifact note.
- **DriveCache** (`2608.16354`), **GaussianDWM++** (`2608.16234`) — driving
  world-model serving/generation; driving-domain.
- **Orbit-Planner** (`2608.16651`) — latent world model for satellite
  obstacle avoidance with code verified; niche domain.
- **MAGE / AppliancePlan** (`2608.15863`) — manual-grounded appliance
  manipulation with real-robot results; no code; watch for release.
- **SPD** (`2608.15917`) — simulation pre-training for dexterity (VR
  teleoperation); project page only, no code verified.
- **Tactile Sim2Real via SBLR** (`2608.15897`) — sensor-agnostic tactile
  sim2real; no code.
- **SurgVIL** (`2608.16058`), **ViHaTeleop** (`2608.16572`), **GUIDER**
  (`2608.15446`) — surgical/teleop data or evaluation contributions; no code.
- **BaT** (`2608.16211`), **HiPHI** (`2608.16222`) — medical agent
  self-improvement / human-motion dataset; no verified access paths.
- **FloodReasonBench** (`2608.15410`), **MistyPilot** (`2608.15549`),
  **GAINS** (`2608.15707`), **TwinGridShield** (`2608.15391`), **CUBICS**
  (`2608.16564`), **AeroCopilotBench** (`2608.16349`), **ReRef-3D**
  (`2608.16011`) — domain benchmarks or safety studies without reusable
  robot-harness contracts or verified artifacts.
- Generic perception, control, planning, UAV, humanoid-motion, soft-robot,
  driving, medical, and video-generation papers without a reusable agent/VLA
  harness, recovery, safety, or evaluation contract were excluded per policy
  (e.g., `2608.16433`, `2608.16476`, `2608.16640`, `2608.16642`, `2608.16712`,
  `2608.16728`, `2608.16741`, `2608.16523`, `2608.16555`, `2608.16499`,
  `2608.16058`, `2608.16686`, `2608.15995`, `2608.15490`).

## Watch-list status (rechecks)

- **HELIX** (`2608.13951`) — repository is now live (MIT, TypeScript); promoted
  to "available" in the README. See above.
- **StageWAM** (`2608.10780`) — still v3 (last updated 2026-08-14); no new
  version, code, or weights. Unchanged.
- **ReflexVLA / ReflexBench** (`2608.14379`) — still v1, still pre-release
  ("after acceptance"). Unchanged.
- **DreamX-Phi 1.0** (`2608.13489`) — still v1; no weights/inference release.
  Unchanged.
- **UniTexture** (`2608.13453`) — still no official attack code or benchmark
  artifacts. Unchanged.
- **GigaBrain-0.7** (`2608.15875`) — newly added to watch list pending the
  promised training-code/weights release.
