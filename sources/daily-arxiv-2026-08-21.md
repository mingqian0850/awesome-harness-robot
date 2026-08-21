# Daily arXiv scan — 2026-08-21

## Scope

- Interval: after the 2026-08-20 scan cutoff (06:30 UTC) through 2026-08-21
  08:30 UTC. The previous run reported the 2026-08-20 batch empty at scan time;
  this scan covers the complete 08-20 announcement day, re-checks 08-19 for late
  arrivals, and checks 08-21 (empty at scan time).
- Queries (HTTPS `export.arxiv.org/api/query`, `submittedDate` range syntax,
  categories `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, `cs.LG`):
  - `[202608200000 TO 202608202359]` → 254 unique records, all `published` =
    2026-08-20 (the full 08-20 announcement day, absent from the previous run;
    all v1, `updated` = `published`).
  - `[202608190000 TO 202608192359]` → 325 records (was 274 in the 08-20 run):
    50 late arrivals with IDs above the previously covered range
    (`2608.19280`, `2608.19281/2/5`, `2608.19297`–`2608.19504`, `2608.20149`),
    all `published` = 2026-08-19, screened in full here; one additional record
    may lie within the already-covered ID range (keyword re-screen of the
    previously reviewed span surfaced no new candidate).
  - `[202608210000 TO 202608212359]` → empty (the 08-21 batch was not yet
    exposed at scan time).
  - Revisions: `lastUpdatedDate:[202608200630 TO 202608210900]` → 197 entries;
    all are v1 `published` = 2026-08-20 (the new batch itself) except two
    previously reviewed items: **HODAgent** (`2608.17584v2`, updated
    2026-08-20T04:11) — an author **withdrawal** (company-mandated internal
    review; the arXiv record now carries the "(withdrawn)" marker), and
    **DA-WAM** (`2608.19085v2`, updated 2026-08-20T06:46) — driving-domain,
    excluded per policy, no change to its status. No other older paper was
    revised in the window.
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action models,
  code-as-policy, skill discovery, memory, recovery, evaluation, runtime
  monitoring, and safety. Candidate claims were checked against the arXiv
  record, official project pages, and official code/model/data repositories
  (GitHub/Hugging Face API and live HTTP checks on 2026-08-21); existing
  README, landscape, and source records were checked for duplicates. All
  performance results below remain author-reported unless stated otherwise.

## Included (new curated entries)

### EnvHarness: Awakening Static Worlds for Agent Learning

- Paper: https://arxiv.org/abs/2608.19880
- Project: https://envharness.com (live)
- Code: https://github.com/google-research/envharness (Apache-2.0, verified
  live 2026-08-21; `envharness` package, experiments, RL utilities, tests)
- Classification: General Harness Methodology; Self-improvement.
- Why included: applies the *agent harness* idea to the other side of the
  interaction — wrapping a **frozen environment** instead of a frozen model.
  EnvHarness is a programmable plug-in layer (Setup reshapes the initial
  state, Rule reshapes which actions are allowed, what they do, and what the
  agent observes, Link composes in other environments' tasks) that operates
  strictly at the standard `reset`/`step` interface and never touches the
  environment's internal code or its trusted goal predicate. This gives
  static benchmarks an adaptation surface for targeting an agent's weaknesses
  without rebuilding environments, while preserving the original verifiers —
  a reusable environment-side harness contract with a real official
  implementation from Google Research.
- Boundary: digital-agent environments; no robot experiment. Results are
  author-reported.

### The Verification Gap in Networked Physical AI: A Post-Semantic Communication Framework

- Paper: https://arxiv.org/abs/2608.19593
- Companion: https://github.com/sarulab-ou/post-semcom (verified live
  2026-08-21; frozen paper snapshot, exact evaluator over 4,096 weighted
  states, Dockerfile, examples)
- Classification: General Harness Methodology; Evaluation/Safety.
- Why included: names the *verification gap* — a task-effective proposal is
  not yet a justified physical action when valid, timely, proposal-bound
  evidence or the authority to finalize an action is unavailable — and
  formalizes the post-semantic communication interface between proposal
  formation and physical execution: application-declared evidence
  requirements, evidence records, validation of supporting and conflicting
  records, and authorized finalization at a permitted finalizer. This is a
  systems-interface contract for Physical AI execution (evidence,
  authorization, and timing live outside the model), the same ownership
  boundary as the previously curated One Gate Is Not Enough and Harness
  Engineering for Physical AI lines, and it ships a frozen, executable
  companion artifact with an exact evaluator.
- Boundary: a conceptual/interface paper; the companion evaluator is a
  controlled synthetic study whose parameters are declared design probes, not
  measurements. No robot experiment.

### What Matters for Latent Actions in Robot Learning

- Paper: https://arxiv.org/abs/2608.19613
- Project: https://carldegio.github.io/latent_action.github.io (live)
- Code: https://github.com/XizoB/What-Matters-for-Latent-Actions-in-Robot-Learning
  (redirects to https://github.com/XizoB/LAM, verified live 2026-08-21)
- Weights: https://huggingface.co/XizoB/LAM (public, verified)
- Data: https://huggingface.co/datasets/XizoB/franka_20260617_lerobotv2.1
  (public, verified)
- Classification: Robot Foundation/World Model (latent-action representation
  study); Evaluation.
- Why included: the first comprehensive empirical study of latent action
  learning for robot manipulation. Existing LAM work evaluates design choices
  in isolation under inconsistent settings; this study fixes one protocol and
  isolates which factors (latent-action prediction targets, VLM backbone
  fine-tuning, policy learning on top) actually determine downstream
  manipulation performance, with real-world experiments. An evaluation
  contract for a representation family the field is adopting at scale, with
  real code, weights, and a released Franka dataset.
- Boundary: results are author-reported; the study is an analysis artifact
  rather than a new policy.

### Task-CoEvolve: Efficient Harness Optimization via Adaptive Validation Task Selection

- Paper: https://arxiv.org/abs/2608.20169
- Repo: https://github.com/Agent4Science-UTokyo/Task-CoEvolve (live, but
  README + figures only; code marked "will be available soon" as of
  2026-08-21 — **not** described as open)
- Classification: General Harness Methodology; Self-improvement.
- Why included: harness optimization iteratively rewrites harness code from
  validation performance, but evaluating the fixed validation set in full
  every iteration is wasteful on tasks that stop discriminating as the
  harness evolves. Task-CoEvolve co-evolves the validation tasks with the
  harness: it samples tasks where candidate harnesses disagree
  (variance-weighted sampling at the capability frontier) and estimates
  full-set scores from partial evaluations with sampling-probability
  correction. The authors report matching the final performance of full-set
  search while cutting the optimization evaluation budget by 80% on online
  text classification and Terminal-Bench 2.1. A named efficiency contract for
  the harness-optimization loop (the line of Self-Harness, HCL, HarnessOpt).
- Boundary: digital-agent results; no robot experiment; no code released yet.

### Loreley: Repository-Scale Program Evolution with Quality-Diversity Search

- Paper: https://arxiv.org/abs/2608.19703
- Code: https://github.com/NeapolitanIcecream/loreley (Apache-2.0, verified
  live 2026-08-21; package, benchmarks, docs, CI, public experiment evidence)
- Classification: General Harness Methodology; Self-improvement.
- Why included: a self-improving program-search harness that keeps complete
  repository states in a Quality-Diversity archive and samples them as
  parents or context for later edits, with candidates produced as Git commits
  in isolated worktrees and judged by a project-supplied evaluator — a
  reusable interface for agentic program evolution (the RHO line of
  repositories-as-policies search). Notable for reporting a **matched
  negative result**: in a 1,008-job Zstandard experiment (seven paired
  blocks, 48 candidate jobs per policy and block), neither comparison
  established a QD advantage over sequential champion editing, with the full
  evidence published alongside the code.
- Boundary: digital software repositories, not robotics; no robot experiment.

### Phantom Gains: Auditing Self-Improvement Against a Measured Null

- Paper: https://arxiv.org/abs/2608.20290
- Code: https://github.com/chengxuphd/phantom-gains (Apache-2.0, verified live
  2026-08-21; full analysis pipeline recomputing every number in the paper)
- Classification: Evaluation/Safety (self-improvement audit methodology).
- Why included: auditing whether a model improved itself means differencing
  two noisy per-problem estimates, and the events of interest sit exactly
  where that error bites. Auditing three rounds of rank-32 LoRA self-training
  on Qwen3-8B against a frozen control pushed through the identical pipeline,
  the authors identify **seven measurement failures**, each of which inverts
  a reported finding when the control is absent — including a single-greedy-
  decode ledger that manufactures capability change and an expansion
  statistic that assigns an untrained model a rate of 0.280. They replace
  them with a per-problem exact test against a pooled baseline under FDR
  control. A directly reusable evaluation contract for any harness or
  self-improvement claim, with a fully reproducible artifact.
- Boundary: digital LLM self-training; no robot experiment.

### MaliciousSkillBench: A Comprehensive Benchmark for Malicious Agent Skill Detection

- Paper: https://arxiv.org/abs/2608.19901
- Project: https://protectskills.github.io/MaliciousSkillBench/ (live)
- Code: https://github.com/protectskills/MaliciousSkillBench (verified live
  2026-08-21)
- Data: https://huggingface.co/datasets/ProtectSkills/MaliciousSkillBench
  (public, verified)
- Classification: Evaluation/Safety (skill-layer security).
- Why included: Agent Skills are a direct distribution channel for malicious
  behavior (scripts, resources, and service configuration shipped inside
  reusable instruction packages), yet prior malicious-skill resources were
  fragmented. MaliciousSkillBench consolidates 13 public sources into 9,740
  benchmark identities (7,505 malicious, 2,235 benign; 4,588 structural
  families, 11 harmonized attack categories) with canonicalization,
  deduplication, cross-label conflict handling, and random/structural-
  disjoint/source-disjoint evaluation protocols. Skill loading is a core
  harness responsibility, so a detection benchmark for the skill layer is a
  harness-safety contract; code and the full public dataset are released
  (the exact frozen text of five sanitized records is withheld).
- Boundary: digital agents; no robot experiment.

## Updated entries (important revision)

### HODAgent (`2608.17584`) — withdrawn by the authors

- v2 (2026-08-20) carries an author notice: the paper is being withdrawn
  immediately to comply with a mandatory company internal-review policy; the
  arXiv record is marked "(withdrawn)". The README entry from 2026-08-19 is
  annotated accordingly and the reported results (84.8%/91.5% joint success
  over 164 interactive simulation cases; 92%/72%/63.3% physical pass rates)
  are now to be treated as unverified pending re-release.

## Reviewed but not promoted

- **EXIMO** (`2608.19891`) — VLM-guided exploration for fine-tuning VLA
  policies on the fly (DeepMind authors; exploration proposals from a VLM
  guide a policy optimizer without teleoperation data). Relevant agentic
  VLA-harness line; no official project page, code, or weights were located
  as of 2026-08-21; watch.
- **DECOWAM** (`2608.20114`) — decoupled whole-body world-action model for
  legged mobile manipulation (separates camera ego-motion from base and arm
  actions; introduces the ARMDOG real-robot dataset). No official page, code,
  or dataset link located; watch.
- **HiTac-WAM** (`2608.19574`) — hierarchical tactile world-action model
  forecasting contact state, 3D deformation, and slip risk before execution.
  Related tactile-WAM coverage exists (Tactile-WAM, TacWAM); no artifacts;
  watch.
- **OrthoSkillVLA** (`2608.19589`, PRCV 2026) — continual skill learning via
  gradient-informed skill-subspace adaptation for pretrained VLAs. No code;
  watch.
- **SafeBranch** (`2608.19729`) — branch-pair safety alignment for embodied
  agents (trains safety by contrasting branch pairs at safety-critical
  steps). No code located; watch.
- **SCAPE** (`2608.19425`) — scenario-conditioned simulation-augmented policy
  evaluation (combines limited real rollouts with simulation proxies and
  reports scenario-specific, not population-average, estimates). No artifacts
  located; watch.
- **Evidence-Gated TAMP / EAFG** (`2608.20084`) — evidence acquisition and
  feasibility gating for VLM+TAMP under partial observability. No code; watch.
- **Panda ROS 2 stack** (`2608.19740`) — open-source ROS 2 stack restoring a
  reliable position interface for the Franka Panda (root-cause analysis of
  control-loop timing/jitter; async hardware interface, rate matching). The
  paper says open-source and the project site is live, but the linked code
  (anonymous.4open.science `libfranka-636F`) is submission-gated (401) and no
  public repository was verified as of 2026-08-21; watch for the release.
- **Outcome Monitors** (`2608.19303`) — runtime monitors that detect
  violations of mined outcome contracts and issue recovery receipts for
  silent tool failures (ToolMaze 10.9%→28.1%). Digital; no code; watch.
- **StateMemBench** (`2608.19652`) — 234 multi-session scenarios testing
  whether agent memory systems track evolving world state. Digital; no
  artifacts located; watch.
- **Adaptive Probabilistic Shielding** (`2608.19836`, RV 2026) — learns the
  MDP to build probabilistic shields for safe RL when transition models are
  unavailable. General RL safety contract; no code; watch.
- **Orthogonal JEPA** (`2608.20065`) — factorized predictive states for JEPA
  world models (decomposes the monolithic target embedding). Extends the JEPA
  line (PSG-JEPA, StageWAM, DA-LeWM); no code; watch.
- **DBOSC** (`2608.19492`) — Disjoint-Bridge Operator-Substitution
  Certificate: operational capability hierarchy asking whether independently
  trained modality compilers enter a frozen response chart interchangeably.
  World-model interface certification; no artifacts; watch.
- **Temporal Logic Compilation for Stream-Based TAMP** (`2608.19453`) —
  enforces safety-critical ordering/invariance/liveness constraints in
  stream-based TAMP solvers. No code; watch.
- **Credit Without Ground Truth** (`2608.19760`) — pre-registered audit of
  step-level credit-assignment signals against executed replay (ALFWorld):
  LLM-judge scores, logprob ratios, and policy confidence all fail to beat
  chance. Digital; no artifacts; watch.
- **AI4AI-Bench** (`2608.20318`) — benchmark isolating whether agents can
  design training algorithms (recursive self-improvement). Digital; no
  artifacts; watch.
- **Optimal Skill Selection** (`2608.19993`) — first provable bicriteria
  guarantees for skill-set selection under bounded context. Digital; no code;
  watch.
- **Self-Demonstrated VLA fine-tuning** (`2608.19490`) — self-supervised VLA
  adaptation using online interaction rollouts from the zero-shot policy
  (UIUC). Project page live but code/data "coming soon"; watch.
- **ExPhy** (`2608.20009`) — 24,000-scene benchmark with explicit physical
  property labels (mass/friction/restitution) for trajectory forecasting.
  Simulated; no artifacts verified; watch.
- **The Missing Touch** (`2608.19372`) — spatially distributed tactile
  feedback study for teleoperation dexterity. Human-factors/hardware study
  rather than a harness contract; watch.
- **Neural Reduced Dynamics** (`2608.19375`) — argument for learning the
  "right abstraction" (reduced neural dynamics) for complex-robot control
  simulation. Simulation-engineering position; no code; watch.
- **Brain Researcher** (`2608.19902`) — agentic research harness for
  neuroimaging with rules for admissible analyses, required checks, and claim
  scope. Digital science domain; no artifacts verified; watch.
- **Thinkingbox** (`2608.19741`) — sandbox + benchmark for agents in stateful
  business workflows (isolated MCP-compatible tool sessions). Digital; no
  verified release; watch.
- **DeltaML-Bench** (`2608.19653`) — 48-task benchmark where agents must
  improve published baselines inside imperfect open-source research
  repositories; real code and benchmark on GitHub. Digital ML-agent domain;
  excluded per policy (like ComponentBench, noted so the verified release is
  not lost).
- **SWE-bench Science** (`2608.19799`) — 119 repository-level tasks repairing
  scientific software across 20 domains. Digital coding-agent benchmark;
  excluded per policy.
- **G-MARK** (`2608.19964`), **Planning-Oriented E2E Driving** (`2608.20111`),
  **CAViAR** (`2608.19380`) — driving-domain; excluded per policy.
- **SAPO** (`2608.19842`), **MileGPO** (`2608.19803`), **Beyond Imitation
  (OPD)** (`2608.19408`), **Learning When to Think** (`2608.20256`),
  **Stopping and Routing LLM Judge Panels** (`2608.19802`) — digital
  agentic-RL / evaluation-training methods without robot or harness contracts;
  excluded.
- **MidTool** (`2608.20314`) — mid-training data synthesis for agentic tool
  use (open corpus + models on HF). Training-data contribution, digital;
  excluded per policy.
- **ReguSim/ReguBench** (`2608.19974`), **ContractScrub** (`2608.20204`),
  **InsufficiencyBench** (`2608.20220`) — legal/financial domains; excluded.
- **VGI-Bench** (`2608.19583`), **BeyondMasks** (`2608.20107`),
  **StreamSoccer** (`2608.19723`) — video-generation/understanding evaluation;
  excluded.
- **RuleMaze** (`2608.20237`) — MLLM spatial-planning benchmark; digital;
  excluded.
- **Inter-X++** (`2608.20312`), **PersonalBench** (`2608.19746`),
  **OenoBench** (`2608.20106`) — digital/HCI benchmarks; excluded.
- **ReCache** (`2608.19662`), **FlashPrefill V2** (`2608.19758`),
  **FleetSieve** (`2608.19659`), **CacheRoute** (`2608.19677`),
  **Pandora's AI Model Routing** (`2608.20316`) — serving/optimization
  infrastructure; excluded.
- **PETA** (`2608.19906`), **FAR-DPO** (`2608.19808`), **Manifold Drift in
  Flow Preference Optimization** (`2608.20011`) — domain-specific ML
  methods; excluded.
- **VSPG** (`2608.18404`) — vector-symbolic policy gradient for discrete
  actions; general RL method with code but no robot/harness contract;
  excluded.
- **ATC with LLMs** (`2608.19299`), **Hybrid Feedback Sampling MPC**
  (`2608.19443`), **Time-Varying Data-Driven CBF** (`2608.19366`),
  **Wave-Based Bilateral Teleoperation** (`2608.20043`),
  **Teleoperation tactile feedback** (`2608.19372`) — conventional
  control/teleoperation without a reusable agent/VLA harness, recovery,
  safety, or evaluation contract; excluded.
- Generic perception, planning, navigation, medical, UAV, video, finance, and
  conventional RL papers in the screened ranges without a reusable
  agent/VLA harness, recovery, safety, or evaluation contract were excluded
  per policy.

## Watch-list status (rechecks)

- **HODAgent** (`2608.17584`) — RESOLVED as withdrawn (v2, 2026-08-20);
  README entry annotated; treat results as unverified.
- **HELIX** (`2608.13951`) — repository still live (MIT); no further change.
- **StageWAM** (`2608.10780`) — still v3; no new code, weights, or revision.
- **ReflexVLA / ReflexBench** (`2608.14379`) — project page still says code
  "after acceptance"; unchanged.
- **DreamX-Phi 1.0** (`2608.13489`) — official repo `AMAP-ML/DreamX-Phi` still
  README-only; unchanged.
- **ForceU-VLA** (`2608.15009`) — official repo `VMVLab/ForceU-VLA` still
  README+assets only; unchanged.
- **UniTexture** (`2608.13453`) — still no official attack code or benchmark
  artifacts; unchanged.
- **PRISM** (`2608.17962`) — project page still says "Dataset soon";
  unchanged.
- **GigaBrain-0.7** (`2608.15875`) — no verified release announcement;
  unchanged.
- **GigaBrain-WBC-0.5** (`2608.18234`) — project page live, code still
  "coming soon"; unchanged.
- **BATON** (`2608.16889`) — still no code; unchanged.
- **Hydra-0** (`2608.18077`) — project page still marks code "coming soon";
  unchanged.
- **ADEPT** (`2608.19182`) — project page live, no code link; unchanged.
- **LIBERO-VIFO** (`2608.17600`), **Agent Lightning v1.0** (`2608.17528`),
  **VLCP** (`2608.16978`) — no new artifacts; unchanged.
- **MANIGUARD** (`2608.17386`), **LEGO-RL** (`2608.17393`) — code repos live
  since 08-20; no further change.
- **Newly added**: EXIMO, DECOWAM, HiTac-WAM, OrthoSkillVLA, SafeBranch,
  SCAPE, EAFG, Panda ROS 2 stack, Outcome Monitors, StateMemBench, Adaptive
  Probabilistic Shielding, Orthogonal JEPA, DBOSC, Temporal-Logic TAMP,
  Credit Without Ground Truth, AI4AI-Bench, Optimal Skill Selection,
  Self-Demonstrated VLA fine-tuning, ExPhy, The Missing Touch, Neural Reduced
  Dynamics, Brain Researcher, Thinkingbox, DeltaML-Bench (excluded, noted),
  SWE-bench Science (excluded, noted) — see "Reviewed but not promoted".
