# Daily arXiv scan — 2026-09-04

## Scope

- Interval: since the 2026-09-03 run's cutoff (~2026-09-03 06:45 UTC, commit
  `22875bd`, "Curate September 3 robot harness research") through 2026-09-04
  ~04:30 UTC. No missed dates.
- The arXiv export API (`https://export.arxiv.org/api/query`, HTTPS) was
  healthy but rate-limited early (429s with 60 s retry backoff; later queries
  succeeded on first attempt with ≥25 s spacing). XML saved under the
  workspace scratch dir (`.scratch/arxiv-2026-09-04/`, removed after the run).
- **Announcement-block analysis (important for screening accuracy):** the
  2026-09-04 00:00 UTC announcement block (IDs `2609.02940`–`2609.04203`)
  contains submissions from several days: 08-31 (`2609.02940`–`2609.02942`),
  09-01 (`2609.02944`–`2609.02948`), 09-02 (`2609.02954`–`2609.03222`) and
  09-03 (`2609.03225`–`2609.04203`). Day-scoped queries
  (`(cat:cs.RO OR cat:cs.AI OR cat:cs.CL OR cat:cs.CV OR cat:cs.LG) AND
  submittedDate:[…]`, parenthesized per the 09-03 record) return each day's
  unique entries across the five categories; totals below come from that
  query form, which the 09-03 record verified returns exact day counts.
- Day counts (five-category parenthesized query, 2026-09-04):
  - `[20260903]` = **352 unique** (new day, first exposure; announced
    2026-09-04 00:00 UTC). Fully screened here; all v1, 0 revisions.
  - `[20260902]` = **401 unique** (was 323 at the 09-03 run's first exposure
    → **+78 late arrivals** in the 09-04 block, IDs `2609.02954`–`2609.03222`
    published 09-02). Whole batch re-screened once here (see below); 7 new v2
    revisions dated 2026-09-03 (all minor, none curation-relevant).
  - `[20260901]` = **546 unique** (was 543 → +3: `2609.02944` Reflect-SQL,
    `2609.02947`, `2609.02948` — all screened; none qualify). 27 v2 (19 dated
    09-02 known; 8 dated 09-03 — all minor, none curation-relevant).
  - `[20260831]` = **577 unique** (was 574 → +3: `2609.02940`, `2609.02941`,
    `2609.02942` — all screened; none qualify). 20 v2 (the only new one,
    `2608.30960` v2 dated 09-03, is a math result; not relevant).
  - `[20260830]` = **261 unique** (unchanged; no growth).
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action models,
  code-as-policy, skill discovery, memory, recovery, evaluation, runtime
  monitoring, and safety. Candidate claims were checked against the arXiv
  record (abs pages and HTML full text), official project pages, and official
  code/model/data repositories (live HTTP and GitHub API checks on
  2026-09-04); README, landscape, and prior source records were checked for
  duplicates (no name/ID collisions; see the FailBench disambiguation below).
  All performance results remain author-reported unless stated otherwise.

## Included (new curated entries)

### FailBench: How Reliable are VLMs at Judging Robot Task Success?

- Paper: https://arxiv.org/abs/2609.03611
- Classification: Evaluation/Safety (VLM success-judging reliability
  benchmark for robot manipulation).
- Why included: a cross-domain evaluation contract for the increasingly
  common practice of letting VLMs judge whether a robot attempt succeeded.
  2,197 manipulation attempts are assembled from 14 public sources (12
  real-world, 2 simulated), with 75% of failures occurring naturally and six
  real-world sources drawn from datasets that were never built for failure
  detection. Evaluating 13 VLM-based detectors, the authors report the best
  model at only 0.77 mean balanced accuracy; models fine-tuned for failure
  detection consistently underperform general-purpose VLMs *and their own
  pretrained baselines*; performance is strongly evidence-dependent
  (near-saturation when outcomes hinge on observable object motion,
  near-chance <0.60 balanced accuracy on contact-intensive assembly); and
  judges show a systematic bias toward predicting success under ambiguous
  evidence that survives increased reasoning effort. Spatially localizing and
  cropping outcome-relevant regions improves the top detector by 2.4 points
  without extra training. Sits in the success-judging/evaluation line
  (SAFECAST hidden-state failure probes, Failing Gracefully, the "What to
  Measure" guidance): treat VLM success verdicts as a measured, gated signal.
- Boundary: evaluation benchmark over public sources; no new artifacts on the
  arXiv record as of 2026-09-04 (no code/data link); results author-reported.
- **Name disambiguation:** the "Failing Gracefully" entry (2608.05313, ICRA
  2026) also uses the name FailBench for its internal MuJoCo failure-mode
  simulation framework — an unrelated artifact. The two are distinguished in
  the README text.

### A Blind Trust, the Bloody Thrust: When Attacker-Controlled Hook Updates Steer AI Agent Harnesses towards Malicious Behaviors (HookPry)

- Paper: https://arxiv.org/abs/2609.03884
- Code: https://github.com/whalefal1/HookPry (live 2026-09-04, HEAD
  `a706d86`, Apache-2.0)
- Classification: Evaluation/Safety (harness lifecycle-hook attack surface;
  General Harness Methodology adjacency).
- Why included: names a blind trust in the harness lifecycle: modern agent
  harnesses bind shell commands to runtime events (session start, tool calls,
  file edits) through lifecycle-hook configuration that runs with host
  privileges and may fire at times the model never observes, and the update
  path that delivers hooks is trusted without verification. Under a
  supply-chain threat model where the attacker controls only plugin metadata
  and lifecycle-hook configuration, a benign versioned plugin can be
  trojanized by an update that silently binds attacker-chosen commands to
  benign events (privilege escalation etc.). HookPry automates ten attack
  objectives; across 25 harness–backend combinations in 1,000 end-to-end
  runs it compromises all seven evaluated harnesses (OpenHarness, OpenClaw,
  Claude Code, Codex CLI, OpenCode, Hermes Agent, WorkBuddy), per-harness
  success reaching 92.5%; representative defenses are insufficient (Microsoft
  Defender 0% recall; the union of three static defenses misses 47.5% of
  malicious artifacts). The harness-configuration/lifecycle security line
  (HarnessRisk's most vulnerable phase was Harness Configuration; Harness-R1
  already trains a harness engineer to *write* lifecycle-hook patches — the
  update path that delivers them is exactly what HookPry attacks;
  CordisBench lifecycle reasoning).
- Boundary: digital agents, no robot experiment; results author-reported.
  The Apache-2.0 repository (46 JSON benchmark cases = 40 core + 6 MSG-*
  extensions, cross-harness adapters/drivers, runners, scorer, dual-use
  warning) matches the paper's harness list and objective taxonomy; it is
  identified by content match because the arXiv record does not link it
  (checked abs page and HTML full text on 2026-09-04).

### Toward Unified Robot Learning: Bridging Representation, Vision-Language-Action, and World Models (added to Surveys)

- Paper: https://arxiv.org/abs/2609.03927 (TMLR 2026)
- Classification: Robot Foundation/World Model + VLA (survey).
- Why included: a current, vetted (TMLR) survey organized along exactly the
  axes this repository curates — understanding (representation learning),
  acting (VLA models), and reasoning (world models) — with a design-choice
  taxonomy (environment representation, policy learning, predictive
  modeling) and, unusually, an analysis of how the three component families
  interact rather than a parallel catalog. It argues that open robot-learning
  challenges (uncertainty quantification, OOD generalization,
  cross-embodiment transfer, long-context understanding, long-horizon
  planning) stem from missing integration across perception/action/reasoning
  rather than from individual components. Robot/embodied-focused (the
  Surveys section's bar); complements The Embodiment Gap (portability) and
  Weights or Skills (weights-vs-skills).
- Boundary: survey; no artifacts.

## Included (artifact-status updates to existing entries)

### ReflexBench (artifact-status update)

- Paper: https://arxiv.org/abs/2608.14379 (curated 2026-08-17)
- Project page: https://reflexvla.github.io (live)
- Code: https://github.com/LxRoboticsLab/ReflexBench — **now live** (HEAD
  `8bb9314`, "init ReflexBench", 2026-08-24; official Isaac Lab
  implementation, 105 Python files: six reaction-critical tasks with
  joint-position/IK/play/evaluation variants, latency-aware evaluation
  harness with decoupled simulator stepping, data-collection scripts; BSD-3-Clause)
- Data: https://huggingface.co/datasets/cyx337/ReflexBench_dataset (live)
- The README entry previously recorded "code only after acceptance as of
  2026-08-17"; per policy the literal status is now reversed by a verified
  live release (code + dataset). README entry updated accordingly (no
  re-classification of the paper itself).

## Reviewed but not promoted (watch list)

From `[20260903]` (first exposure; all v1):

- **WISE** (`2609.03681`) — world-model-guided imagination scheduling for
  VLA post-training: selectively invokes bounded multi-view imagination at
  interaction-relevant states, evaluates candidate futures with
  progress/completion signals, and refines actions from real interaction
  contexts; consistent gains with π0/π0.5 plus real-world robustness
  reported, ~80% GPU-time reduction vs full imagination. Training-time
  framework without an external runtime contract; no official artifacts on
  the arXiv record; watch for release.
- **XR-2 / Scaling Bimanual Household Manipulation** (`2609.03591`) —
  releases (claimed) 1,500 hours of bimanual household demonstrations used
  to train XR-2, plus DAgger on-policy-correction post-training scaling
  studies. The abstract says the dataset "we open source", but no link
  exists on the arXiv record and no official repository/dataset could be
  located (GitHub/Hugging Face searches 2026-09-04) — treated literally as
  not yet open; watch for the release link.
- **MINERVA** (`2609.03715`) — LIBERO capacity floor: a 0.54M-parameter
  policy reaches 95.1% average success (2,000 rollouts, four suites; 2.4
  points below reported π0.5 with 7,700× fewer parameters), performance
  saturates ~1M and collapses <0.25M parameters; flow matching shows no
  advantage over L1 regression; a task-ID permutation probe drops LIBERO
  instruction-conditioned policies to near chance, arguing the benchmark's
  instruction conditioning mostly selects among memorized tasks; LIBERO-Plus
  robustness 46–56%. Policy-scale study with a benchmark critique; no
  artifacts; watch.
- **R2S-Eval** (`2609.03276`) — robot-policy evaluation pipeline combining
  real-to-sim calibration (simulator rollouts calibrated to the real
  evaluation setting, fewer hardware trials) with VLM pairwise-preference
  judging aggregated into policy rankings, plus a protocol for validating
  whether the pipeline yields correct policy conclusions. Project page live
  (r2s-eval.github.io); no code link on the page or record; watch.
- **What Do CAE Simulation Agents Really Need Beyond a Generic Harness?**
  (`2609.03718`) — controlled study: with information access and repair
  budget fixed, a single-agent generic harness matches or beats
  multi-agent specialized CAE systems (FoamBench 96.4% vs 88.2%);
  execution-feedback repair is the driver (71.8%→96.4%), scripted
  reflection adds nothing, and solver-tutorial domain knowledge is the
  largest remaining gain. Harness-sufficiency evidence in the
  simulation/engineering domain; no artifacts; watch.
- **GIFT** (`2609.04193`) — architecture-flexible training-time feature
  guidance (geometry/affordance/goal supervision) instantiated in a VLA and
  two WAM variants; zero-shot LIBERO-Plus/RoboCasa gains. Model-paper
  without an external contract; no artifacts; watch.
- **FWBC-VLA** (`2609.03889`) — force-aware whole-body compensation bridge
  for contact-rich loco-manipulation on wheeled-legged robots (sensorless
  residual-torque estimator + VLA token injection); policy method; watch.
- **LEAP: Latent Energy Action Planning** (`2609.03294`) — treats the whole
  action horizon as a differentiable variable optimized through a frozen
  LeWorldModel with terminal latent-goal + energy coupling (77.5%→94.8% mean
  success vs LeWM+CEM on four control domains, official checkpoints).
  Frozen-WM planning method; no robot experiment beyond control domains;
  watch.
- **Toward Physically Grounded JEPA World Models** (`2609.03565`) — IROS
  2026 workshop (PWMS) 5-page: JEPA WM + inverse dynamics + state alignment
  for goal-conditioned planning; watch (workshop-scale, no artifacts).
- **Predictive Zonotope Reduction (PZR)** (`2609.03699`) — reducer selection
  for uncertainty-aware runtime monitors framed as optimal control
  (beam-search MPC + distilled policy), implemented in the RLola runtime
  monitoring framework; robotic-arm MuJoCo evaluation, Raspberry Pi 5
  timings. Runtime-monitoring building block; no artifacts; watch.
- **Rethinking World Models for Safety-Critical Embodied Systems**
  (`2609.03774`) — 6-page perspective proposing the Risk-Informed World
  Model (decision-centric WMs organized around consequences, intervention,
  epistemic uncertainty, recoverability); watch (perspective).
- **WorldReward** (`2609.03952`) — VLM pairwise-preference reward unifying
  action-consistency and visual-quality evaluation for camera-conditioned
  world models (chunked evidence + voting; reasoning-augmented preference
  dataset). Video-domain WMs; project page live; watch.
- **Statebench / Stateagent** (`2609.03673`) — benchmark + method for
  world-state reasoning in video continuation (past-visible, occluded,
  complex-transition states); video-domain; watch.
- **Principia** (`2609.04200`) — relational (calibration-independent)
  Newtonian-physics consistency tests for video models; all six evaluated
  generators ≤0.42 while scoring ~0.8 on VBench; VLMs near chance at
  detecting violations. Video-domain physics evaluation; watch.
- **Puffin-World** (`2609.04196`) — unified multimodal model over native 3D
  world states (physics/geometry/appearance + Omni-Camera), Puffin-16M
  dataset; code/models/data claimed released; 3D-video domain, no robot
  experiment; watch.
- **Unreal Engine action-conditioned video pipeline** (`2609.03557`) —
  two-stage physics-in-PIE + offline-MRQ production pipeline (25 servers, 8
  × RTX 5090 each) for action-conditioned world-model pretraining data;
  data-infrastructure contribution for video WMs; watch.
- **SMC: Speculative Macro Commit** (`2609.03236`, MLSP 2026) — runtime
  mechanism: fast drafter pre-executes future action chains on an isolated
  environment snapshot; macro-library mining commits drafted steps when the
  authoritative actor's call matches (latency −18.6% vs sequential on
  τ²-Bench Telecom). Tool-agent harness latency; digital; watch.
- **HookPry** (`2609.03884`) — see Included.
- **Environment Evolution for Terminal Agents** (`2609.04128`) — off-policy
  difficulty-scheduled environment evolution via a loop-engineered
  multi-agent harness; +14.4/+18.0 points on Terminal-Bench 2.1
  (Qwen3.6-27B/35B-A3B). Digital co-evolution line; no artifacts; watch.
- **Terminal-Universe** (`2609.04148`) — reconstructs reusable, executable
  terminal environments from recorded agent trajectories (file-op replay +
  completion agent), synthesizing new tasks; digital; watch.
- **DRACO** (`2609.04094`, code `IBM/draco`) — closed-form redistribution of
  once-per-trajectory rubric judgments into per-step GRPO advantages
  (outcome-blind setting); +15.9 over base on AppWorld; digital; watch.
- **Fresh Memory, Stale Plans** (`2609.03340`) — dependency-scoped
  validation for distributed LLM-agent memory; digital memory line; watch.
- **Plan Pointers / Budgeted Verification of Inherited Agent Memory**
  (`2609.03450`) — verification of inherited agent memory; digital; watch.
- **CONFLICTGUI / CONFLICTGUARD** (`2609.03438`) — benchmark + inference-time
  feasibility-verification protocol and conditional action modulation for
  conflict-aware termination in GUI agents (over-compliance under infeasible
  instructions); digital analogue of when-to-stop; watch.
- **NTEP-R** (`2609.03493`) — necessary tool-evidence path rewards for
  agentic VLMs (pre-call intent and post-call summary alignment); digital;
  watch.
- **Interface-Induced Trajectory Censoring** (`2609.03966`, code
  `nebula-1999/Interface-Induced-Trajectory-Censoring` live) — tool-call
  rates are an interface artifact: changing only the serving adapter moves
  the same model from 0.00 to 0.96/0.19 on BFCL v4 and 0→636 parsed calls on
  tau-bench; reaches the training loop (verl AgentLoop). Eval-interface
  audit line; digital; watch.
- **Clean Engineering, Unstable Measurement** (`2609.04198`) —
  preregistered reliability failure of black-box LLM observers on shared
  endpoints; digital eval reliability; watch.
- **CAE harness study** — see above (`2609.03718`).
- **Value-Preserving Architectures for Agentic AI Systems** (`2609.03920`) —
  architecture-level framing; watch.
- **A Case Study on Emergent Cheating and Whistleblowing in Autonomous
  Research Swarms** (`2609.04170`) — case study; digital safety; watch.
- **The Natural Language Interaction Protocol and Standard for AI Agents**
  (`2609.04135`) — protocol proposal; watch.
- **A computable representation of the physical laboratory enables
  verifiable workflows** (`2609.03621`) — physical-lab (not robot)
  verifiable-workflow representation; watch.
- **Semantic Bayesian World Models** (`2609.03834`) — WM/Bayesian framing;
  watch.
- **Rethinking On-Policy Distillation / Sequential Beats Joint** (`2609.04108`),
  **Headroom-Drift Replay in GRPO** (`2609.03941`), **Spurious Advantage
  Hidden in GRPO** (`2609.04063`), **DE-Venus RLVR** (`2609.03324`),
  **FlowBalance** (`2609.03241`) — reasoning/RLVR-methodology items without
  harness scope; watch or domain-excluded as above.
- **TIGPO** (`2609.03383`), **RuleMem** (`2609.03915`), **KC-Bench**
  (`2609.03588`), **When Users Don't Ask** (`2609.03467`),
  **Do GUI Agents Know When Not to Act** (see CONFLICTGUI),
  **DuplexSpeechBench-IFEval** (`2609.03423`) — digital agent items without
  reusable contracts; domain-excluded or watch as listed.
- **SimSkill** (`2609.03753`, code link in abstract: github.com/qiliuchn/…)
  — self-evolving SUMO traffic-simulation agent with episodic/procedural/
  semantic memory and independent artifact-based verification; verified
  completion +25 points on held-out benchmarks (backbone/budget-dependent);
  traffic-simulation domain; watch.
- **IRWOZ 2.0** (`2609.04030`) — industrial-robot dialogue dataset (LLM
  refinement of IRWOZ, ieee-dataport release); HRI dialogue data, not an
  agent contract; domain-excluded.
- **Autonomously/agentic robot-side items from the screened range that stay
  domain-excluded:** SV-WAM (`2609.03602`, driving WAM), Drive-HWM
  (`2609.03572`, driving), Long-Horizon driving WMs (`2609.03225`, driving),
  MulDP quadruped parkour (`2609.03984`), BRIDGE humanoid hardware
  (`2609.03497`), QLAUN quadruped hardware (`2609.03623`), ARTiS gripper
  (`2609.03362`), soft robots (`2609.03758`, `2609.03175`), WeldSeam/NDE/
  photogrammetry, TRaIL-Odom, omnicopter/greenhouse/navigation,
  pose-registration certifier (`2609.03222`), VLN-CE macro-action RL
  (`2609.03906`, benchmark-focused), Air-Ground VLN (`2609.03483`),
  HRI-engagement dataset protocol (`2609.03255`), cognitive-ergonomics HRI
  (`2609.03704`), HRI-crane skills (`2609.03392`), SkillGLoW-class digital
  items (`2609.02217` etc. — already watch-listed on 09-03).
- **Digital/enterprise/game/medical/domain items** in the screened ranges
  (speech, medical imaging, graphs, weather, e-commerce, GUI, TTS, OCR,
  legal, biology, security ops, etc.) are domain-excluded per policy unless
  they carry an agent-harness/evaluation/safety contract (those are
  watch-listed above).

From the `[20260902]` growth (+78, IDs `2609.02954`–`2609.03222`, first
screened this run; the 09-03 run's 323-entry view is fully re-confirmed):

- **EGR: Sensing Which Modality Matters** (`2609.03142`) — evidence-gated
  regularization against modality entanglement in VLA training (per-frame,
  per-sensor task-relevance gating of invariance/sufficiency consistency
  objectives; BEHAVIOR-1K-derived diagnostic + 47 rollout skills; two
  real-robot embodiments: Kinova bi-manual + MELFA/GelSight). Training-time
  method with a benchmark; real-robot gains author-reported; no artifacts;
  watch.
- **RoboTok** (`2609.03199`) — internet-scale data engine retrieving
  manipulation-relevant human demonstrations for dexterous policy training
  (actor-centered 3D hand-trajectory latent space, efficient search over
  web-video collections); data-side; no artifacts; watch.
- **Seeing Less Is Not Seeing Safely (TFPD)** (`2609.03055`) — domestic-robot
  perception-export privacy: task-functional perception distillation
  profiles downstream exports for utility vs direct/indirect exposure;
  AI2-THOR/ProcTHOR sim evidence: three navigation exports with identical
  success differ in linkability 0.532–0.970; target-region substitution
  preserves 0.995 success at 0.077 target macro-F1. Embodied privacy line;
  simulation-only; no artifacts; watch.
- **VeriPhy** (`2609.03153`) — auditable agentic physical verification for
  video generation: a text-only planner compiles the prompt into typed
  physical obligations and a statically validated execution plan before any
  frame is observed; execution gates and scopes only declared calls to frozen
  low-level experts, and every verdict is traceable to provenance-carrying
  evidence records (supported/contradicted/unknown with abstention). On a
  149-clip core carrying 304 human-annotated flaw records it accounts for 228
  (vs 164 for a published question-decomposition evaluator); recall alone does
  not separate it from monolithic prompting (222). Video-domain world-model
  evaluation; watch.
- **Out-of-this-World-Model** (`2609.03067`) — transformer world model for
  spacecraft rendezvous/docking + open-source JAX ISS-docking environment;
  space domain with an open env; watch.
- **You Can't Escape Your Own Activations** (`2609.03035`) — activation-based
  multi-agent collusion monitoring under monitor-awareness and feedback;
  probes stay accurate, agents keep colluding; digital; watch.
- **Reducing Catastrophic Risk from AI (Rogue AI progression indicators)**
  (`2609.03189`) — behavioral-indicator monitoring framework (cyber/national
  security methods); policy-level, no artifacts; watch-lite.
- **MasterControl** (`2609.03209`) — governed enterprise analytics: LLM
  interprets, deterministic policy selects pre-approved programs with
  evidence; 110/110 policy-executed vs 0/330 runtime-planning episodes meet
  the answer-and-evidence contract; digital; watch (configuration-specific
  result, stated by the authors).
- **Judging LLM-as-a-Judge: Rubric Artifacts** (`2609.02942`, EMNLP 2026;
  submitted 08-31, announced today) — rubric-only classifiers predict judge
  outputs without the candidate; judges fail to update on reversed
  responses/rubrics; extends the LLM-judge-reliability line; digital; watch.
- **CORAL: An LLM-Native Harness for Production Recommender Systems**
  (`2609.02730`, screened in the original 323) — harness-contract paper for
  recommender production; flagged here for the record; watch.
- **Remainder of the +78 tail** (`2609.02954`–`2609.03222`) is
  domain-excluded: legal (`2609.02954`), sensor data generation
  (`2609.02958`), GEO-defense (`2609.02964`), federated/PPML clusters
  (`2609.02967`/`2609.03064`/`2609.02971`), MRI/echo/cardiac, weather,
  watermarking, backdoor detection, OCR (`2609.03181`), soft-robot Koopman
  control (`2609.03175`), LLM serving/KV/quantization (`2609.03079`,
  `2609.03149`, `2609.03150`, `2609.03151`), offline RL, graphs,
  tabular/audio/vision models, SLIDEFORGE slides agent, lexical/clinical
  items, batch optimizers (`2609.03177`), mechanistic interpretability
  (`2609.03026`), memory/retrieval items (`2609.03201`, `2609.03148`),
  counterfactual audits (`2609.03073`-class), position papers, and the
  `2609.01956`–`2609.02886` remainder re-confirmed per the 09-03 record
  (which remains authoritative for those IDs).

## Watch-list status (rechecks)

- **Promoted this run (verified 2026-09-04):** ReflexBench — official Isaac
  Lab implementation live at `LxRoboticsLab/ReflexBench` (HEAD `8bb9314`,
  BSD-3-Clause; 6 tasks; latency-aware eval; data-collection scripts) with
  the dataset live at HF `cyx337/ReflexBench_dataset` (page linked the repo
  only after the 09-03 run's check). README entry updated.
- **New content-matched release located:** HookPry `whalefal1/HookPry`
  (Apache-2.0, HEAD `a706d86`; not linked from the arXiv record — flagged as
  such in the README entry and this record).
- **Still absent (treated literally, not open):** Task-CoEvolve
  (`Agent4Science-UTokyo/Task-CoEvolve` still README + `figs/` only — code
  still "available soon"), TrapVLA (project page live, no code link),
  HINT (page live, no code), WISE (no artifacts), XR-2 dataset (claimed in
  abstract; no link or search hit), StageWAM, ReflexVLA weights (benchmark
  code live, model weights not located), DreamX-Phi, UniTexture, PRISM,
  GigaBrain-0.7/WBC, ForceU-VLA, LIBERO-VIFO, Agent Lightning, VLCP,
  Hydra-0, BATON, Q-Planning, AutoSaddler, JIT-Agent, UCAG-P, TrapVLA
  code/benchmarks, TemporalFlow-VLA, PredVLA, FlashVLA, WikiSkill,
  RedEvoAgent, SKILL.state, Agent Mesh, WALL-SS, R2M-Bench,
  INTENT-AS-A-TOOL, BTS-AgentBench, TraceBench, GraphMemix, UrbanGround,
  LM-X, Zero-WAM, VLAct, Code as Worlds, Aero Hand Open, CAITLYN, Dogwood,
  LongGuard, WebWorld, ASPIRE, S3Gym, StudyBench/S3Gym-class — unchanged
  (no arXiv revisions signal change; spot-checks on 2026-09-04 where links
  exist: Task-CoEvolve, TrapVLA, HINT, ReflexBench as above).
- **Watch items re-flagged by v2 revisions (all minor; status unchanged):**
  DemoMimic `2609.01938` v2 (updated 09-03), SAGE `2609.01567` v2,
  Will-the-User-Ever-Know `2608.30362` v2, Lazy Grounding `2608.30303` v2,
  Arkios `2608.30092` v2 (all known at the 09-03 run except DemoMimic's
  09-03-dated v2).

## Domain exclusions in the screened ranges (per policy)

- Driving/CAV: SV-WAM (`2609.03602`), Drive-HWM (`2609.03572`),
  Long-Horizon Multi-Style driving WMs (`2609.03225`), End-to-end miniature
  Ackermann platform (`2609.04147`), VLA driving items (`2608.30144`,
  `2608.30122` in earlier rechecks).
- Locomotion/control/perception/hardware: quadruped parkour (`2609.03984`),
  BRIDGE humanoid (`2609.03497`), QLAUN (`2609.03623`), FWBC-VLA (watch),
  TRaIL-Odom (`2609.03561`), soft robots, grippers, surgical items, space
  LiDAR/lunar segmentation, weld/NDE, VLN-CE/air-ground VLN, odometry,
  greenhouse navigation, pose certifiers.
- Medical/clinical/finance/energy/audio: the usual cs.CV/cs.CL/cs.LG
  out-of-scope clusters (MRI/CT/echo, pathology, EEG/BCI, speech/TTS/ASR,
  weather/climate, power grids, retail/e-commerce, satellites).
- Image/video generation and perception: WorldReward (watch), Principia
  (watch), Statebench (watch), Puffin-World (watch), and the bulk cs.CV
  entries in range (restoration, super-resolution, 3DGS, tracking,
  segmentation, forensics).
- General LLM/agent papers without harness/evaluation/safety scope: CivBench
  class, UTP-Bench-class, tool-agent RLVR items, retrieval/memory/
  quantization/efficiency clusters.

## Operational notes

- SSH: `git fetch`/`ls-remote` initially failed with the known OpenSSH
  `/etc/ssh/ssh_config.d` ownership error; `GIT_SSH_COMMAND='ssh -F
  /dev/null'` worked (user key and known_hosts unaffected) — remote `main`
  confirmed at `22875bd` before the run.
- The arXiv export API rate-limited early (429s on first queries); 60 s
  backoff and ≥25 s spacing completed 13 queries cleanly (5 day counts, 3
  full-day fetches, plus validation queries).
- `/tmp` is ephemeral per shell invocation in this environment; all scratch
  XML/HTML was kept under `.scratch/arxiv-2026-09-04/` in the workspace and
  removed after the run (not committed).
- Artifact verification on 2026-09-04 (live HTTP/GitHub API):
  whalefal1/HookPry (live @ `a706d86`, Apache-2.0, content-matched),
  LxRoboticsLab/ReflexBench (live @ `8bb9314`, BSD-3-Clause) +
  cyx337/ReflexBench_dataset (live), Agent4Science-UTokyo/Task-CoEvolve
  (README-only, unchanged), john-liua.github.io/TrapVLA (no code),
  robot-hint.github.io (no code), r2s-eval.github.io (200, no code link).
- Working tree before the run: `docs/reference-architecture.md` modified,
  `docs/ring-harness.png` and `handoff.md` untracked — preserved untouched
  (not staged, not committed).
- Consistency: README header badge and Current Landscape "Last verified"
  both updated to 2026-09-04 (both were 2026-09-03 after the previous run);
  the Surveys, Benchmarks/Manipulation-and-VLA, and Runtime-Safety sections
  gained the entries above; ReflexBench's entry now carries code/data links.
