# Daily arXiv scan — 2026-08-19

## Scope

- Interval: after the 2026-08-18 scan cutoff (08:33 UTC) through 2026-08-19 09:20
  UTC.
- Queries (HTTPS `export.arxiv.org/api/query`, `submittedDate` range syntax,
  categories `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, `cs.LG`):
  - `[202608180000 TO 202608192359]` → 289 unique records (the full 2026-08-18
    announcement day).
  - Recheck of the previous days for late arrivals: `20260817` → 428 records
    (84 of them, `2608.16887`–`2608.17213`, were not covered by the previous
    run), `20260816` → 212, `20260815` → 215. Only 5 entries across the 08-15
    and 08-16 ranges lay beyond previous coverage and none qualified (MoE
    serving, UAV radio world model, reasoning-effort API contract, two medical
    imaging papers).
  - The 2026-08-19 announcement batch was **not yet exposed** by the API at
    scan time; every returned entry has `published` = 2026-08-18.
- Revision check: in the 08-18 batch all entries are v1 with `updated` equal to
  `published` — no meaningful revision indexed. The 08-17 recheck surfaced
  eight v2 updates (`2608.16038`, `2608.16177`, `2608.16185`, `2608.16289`,
  `2608.16373`, `2608.16485`, `2608.16620`, `2608.16873`), all in
  out-of-scope domains (GNN explanations, LLM obedience studies, document
  search, e-commerce text generation, ocean data, CAD representation, an
  agentic-model technical report, acoustics ML); none relate to robot/VLA
  harness, world-model, or evaluation themes.
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action models,
  code-as-policy, skill discovery, memory, recovery, evaluation, runtime
  monitoring, and safety. Candidate claims were checked against the arXiv
  record, official project pages, and official code/model/data repositories;
  existing README, landscape, and source records were checked for duplicates
  (BATON `2608.16889` already appeared in the README from the 08-18 run and
  was not re-added). All performance results below remain author-reported
  unless stated otherwise.

## Included (new curated entries)

### MANIGUARD: A Benchmark and Data Suite for Specification-Grounded Safety Evaluation and Improvement of Robotic Manipulation

- Paper: https://arxiv.org/abs/2608.17386
- Project: https://nu-ideas-lab.github.io/ManiGuard (live)
- Data: https://huggingface.co/collections/IDEAS-Lab-Northwestern/maniguard-benchmark-and-datasets-6a83d488178bcba81688cd4e
  (public, CC-BY-4.0, verified)
- Code: https://github.com/NU-IDEAS-Lab/ManiGuard — returned 404 at
  verification time (2026-08-19); not described as open.
- Classification: Evaluation/Safety (manipulation).
- Why included: specification-grounded safety evaluation for foundation-model
  manipulation with safety scored independently of task success. 200 locked
  base tasks across six contact-rich household families under a skill ×
  constraint taxonomy, each under one in-distribution and four single-axis
  OOD perturbations (1,000 locked scenarios); every rollout is runtime-checked
  by LTL_f-grounded automaton monitors over physics-grounded predicates in
  simulation and on a physical Franka platform. A paired trajectory-generation
  pipeline (automated motion planning + human teleoperation, annotated by the
  same monitor) releases 8,000 safety-annotated demonstrations supporting
  safety-aware fine-tuning. Across 23,000+ rollouts the authors find 6–21% of
  successful rollouts violate the safety specification, fine-tuning raises
  safe completion from near zero to 7.5–29.8% and engaged-and-safe behavior
  from 16–40% to 51–72%, and a gap remains that scaling demonstrations alone
  does not close.
- Boundary: benchmark data are a real public release; code is not accessible
  at verification time. Results are author-reported (simulation plus a
  physical Franka platform).

### HarnessRisk: A Lifecycle-Oriented Benchmark for Agent Harness Safety

- Paper: https://arxiv.org/abs/2608.17597
- Project: https://baiyajing.github.io/harness-risk/ (live)
- Code: https://github.com/Baiyajing/HarnessRisk (MIT, verified; official
  implementation, active 2026-08-19)
- Data: https://huggingface.co/datasets/YajingB/HarnessRisk (public, verified)
- Classification: Evaluation/Safety (harness lifecycle).
- Why included: the first lifecycle-organized safety benchmark for agent
  harnesses, covering six operational phases — Harness Configuration,
  Capability Extension, Runtime Operation, State Persistence, Action Control,
  Incident Recovery. 128 sandboxed cases pair a benign user objective with an
  adversarial instruction embedded in an untrusted workflow artifact, and each
  trajectory is scored on Utility, Attack Success Rate, Persistence, and
  Detection. Across three harnesses, six language models, and 14 model/harness
  configurations, attack success ranges 12.6–80.9% while utility stays
  75.0–97.6%; Harness Configuration is the most vulnerable phase, and explicit
  risk recognition does not reliably lead to safe action. Real code and data
  make the protocol reproducible.
- Boundary: digital agents; no robot experiment.

### LIBERO-VIFO: Benchmarking the Capability and Safety of Visual Cue Following in Vision-Language-Action Models

- Paper: https://arxiv.org/abs/2608.17600
- Classification: Evaluation/Safety (VLA).
- Why included: evaluates both capability and safety of visual cue following
  in VLAs — eight visual cue families; Part I tests cue understanding and
  authorized following, Part II tests unauthorized following under
  language-cue conflict and empty-language conditions, with scene-instantiated
  cues and safety-critical settings. Across seven VLA models the authors find
  that cue understanding does not reliably translate into execution, yet
  current VLAs execute cue-indicated tasks without any language instruction —
  an emerging unauthorized-visual-cue-following risk corroborated by
  real-robot deployment. A benchmark with a clear evaluation protocol and a
  safety-relevant result (visual cues as an attack surface).
- Boundary: author-reported; no official benchmark release or code was located
  as of 2026-08-19.

### Agent Lightning v1.0: Towards Harnessed Agentic RL

- Paper: https://arxiv.org/abs/2608.17528
- Classification: General Harness Methodology; Self-improvement.
- Why included: names and formalizes *harnessed agentic RL* — the
  disaggregated architecture in which the deploy-time harness owns the
  environment interaction loop and the trainer observes only LLM
  request–response pairs through an endpoint proxy (the paper traces this
  design to its original Agent Lightning and to verl Uni-Agent, AReaL 2.0,
  slime, and Polar). v1.0 is a lightweight (~3,500-line) framework for
  arbitrary agent harnesses that treats retokenization, sample merging,
  advantage calculation, loss normalization, and backend scheduling as
  first-class training-stability problems, ships a complete reproducible
  pipeline for coding-agent RL, and reports improving Qwen3.5-9B on
  SWE-bench Verified with 6K examples and modest compute.
- Boundary: digital coding/search agents; no robot experiment. No official
  code link on the arXiv record as of 2026-08-19; an MIT-licensed reference
  implementation of the original design is public
  (https://github.com/fkatada/ms-agent-lightning), but v1.0-specific code was
  not verified.

### LEGO-RL: Harness-Native Reinforcement Learning for Coding Agents

- Paper: https://arxiv.org/abs/2608.17393
- Docs: https://lego-rl.pages.dev (live, comprehensive)
- Code: https://github.com/LegoX/Lego-RL — returned 404 at verification time
  (2026-08-19); not described as open.
- Classification: General Harness Methodology; Self-improvement.
- Why included: bridges native coding-agent harnesses with scalable
  policy-gradient optimization without modifying harness control flow.
  Three pillars: faithful optimization (in-process LLM proxying captures raw
  generation streams for token-level alignment and robust trainer-side
  log-probability recomputation even under harness-side compaction or
  re-serialization), reliable execution (sandbox orchestration with image
  caching and stage-wise reward-hacking defenses), and observable training
  (integrated validation/monitoring plugin with a live trajectory UI). With
  GSPO on Qwen3.5-35B-A3B the authors report SWE-bench Verified gains across
  OpenHands SDK (64.0→70.4%), Claude Code (62.4→68.2%), and OpenCode
  (57.2→66.6%) while maintaining a rollout–training probability correlation
  above 0.99.
- Boundary: digital coding agents; no robot experiment; the linked repository
  is a 404 at verification time, so the implementation is not currently
  accessible despite the live documentation site.

### HODAgent: Towards On-Demand, Responsive Humanoids for Physical World Human Interaction

- Paper: https://arxiv.org/abs/2608.17584
- Classification: Agentic Robot/VLA Harness.
- Why included: a System-2 embodied agent for humanoid service settings whose
  semi-duplex architecture (Env-Interactor, Planner, Executor, hierarchical
  Memory) keeps situated intent, responsive execution, task revision, and
  outcome verification coherent during service episodes — new requests during
  motion, retained progress, revised actions, and grounding closure in
  execution outcomes. A shared interface connects simulation and physical
  robots (Unitree G1) while isolating platform-specific control. The authors
  report 84.8%/91.5% Joint Success over 164 interactive simulation cases
  (outperforming baselines by 9.8 and 18.9 points) and physical pass rates of
  92% (atomic), 72% (composite), and 63.3% (complete tasks), plus 0.7–9.0
  points on embodied benchmarks.
- Boundary: results are author-reported (simulation plus physical robot);
  no official code or project link was located as of 2026-08-19.

### VLCP: Vision Language Control Policy Closed-Loop Code Replanning for Robot Manipulation

- Paper: https://arxiv.org/abs/2608.16978
- Classification: Agentic Robot/VLA Harness; Code-as-Policy.
- Why included: closes the loop where code-as-policy failures actually live —
  on the control code, inside a single episode. A frontier VLM stays frozen
  and writes the policy as a short Python control function (no demonstrations,
  no fine-tuning); every K steps it re-observes the scene from multi-view RGB,
  proprioceptive state, and a state delta, then rewrites the control function,
  so a failure is caught before it compounds. On a 57-task MuJoCo/RoboVerse
  sweep the authors report 35.1% pooled success versus 3.5% for the identical
  system queried once per episode (tenfold gap with non-overlapping confidence
  intervals in every scene family), traced to a 27.3% within-episode recovery
  rate on failed grasps, with ~84% of input tokens cache-hit and ~10 compact
  queries per episode.
- Boundary: evidence is simulation-only and author-reported; no official code
  link was located as of 2026-08-19.

### Hydra-0: Action Flow for Generalist World Modeling and Control

- Paper: https://arxiv.org/abs/2608.18077
- Project: https://nvidia-isaac.github.io/video_to_data/hydra-0/ (live; code
  marked "coming soon")
- Classification: Robot Foundation/World Model.
- Why included: an important conceptual contribution — robot actions
  represented as *pixel motion* (action flow) become a shared visual interface
  for generalist world modeling and control across embodiments, tasks,
  environments, and video-generation backbones. The best configuration
  reports 90.4% lower robot-motion error and 60.2% lower object-motion error
  than an action-conditioned baseline, zero-shot composition and
  data-efficient adaptation, and r=0.96 between replayed and reference success
  rates on the RoboLab benchmark; an emergent inverse mode acts as a world
  action model that predicts compatible robot motion from desired object flow
  transferred from a human demonstration, with a trained action head mapping
  the latent features to executable actions without task-specific expert
  demonstrations.
- Boundary: everything reported is open-loop; results are author-reported; the
  project page marks code "coming soon" as of 2026-08-19, so no implementation
  is currently available.

## Reviewed but not promoted

- **Prism-GRPO** (`2608.17423`) — splits same-outcome GRPO groups with a
  weighted trajectory-level quality score for VLA RL; reports up to 56% fewer
  rollouts to target success and real-robot deployment of the cleaner
  behavior, but it is a training-time algorithm rather than a harness
  contribution and no code was located.
- **EATR-Stereo** (`2608.17453`) — embodiment-aware stereo token routing for
  humanoid VLA with real-robot results (60.0% full-task success on a 33-DoF
  humanoid); no code; perception-routing mechanism rather than a harness.
- **UniReflex** (`2608.17432`) — plug-and-play variable impedance control for
  frozen generative policies via a fast reflex network; real bimanual
  experiments, 25–66× lower per-step latency; no code; a control-layer
  wrapper rather than a harness.
- **Reuse Before You Retrieve** (`2608.17484`) — diagnoses recoverable
  headroom vs retrieval complementarity for test-time augmentation of frozen
  VLAs (up to +21.0 success-rate points on LIBERO, transfers to another robot
  and simulator); no code; evaluation methodology with author-reported
  evidence.
- **Calibrated Predictive Safety for Heterogeneous Robots** (`2608.17496`) —
  action-conditioned JEPA risk/progress prediction plus deterministic
  per-embodiment safety shields and a fallback ladder; pre-registered
  LIBERO-Long simulation only; no code; extends the JEPA line (PSG-JEPA,
  StageWAM) toward runtime safety — watch for robot evidence.
- **Q-Learning With World Models (QWM)** (`2608.17163`) — test-time search
  over imagined trajectories on top of standard Q-learning, trained only on
  real transitions; Robomimic/LIBERO gains reported; no code.
- **Teach and Grow (TGL)** (`2608.17209`) — agent-centered architecture with
  Skill Blocks, Skill Library, and Experience Memory claiming LIBERO SOTA
  without task-specific retraining; overlaps existing skill-library/memory
  coverage (HERO, Harness VLA, HyMeS) and no artifacts were located.
- **Task-Aware Harness Provisioning** (`2608.17433`) — harness configuration
  as a resource-matching problem with map-guided escalation for
  mission-critical infrastructure (liquid cooling, power grids); interesting
  accuracy–cost Pareto framing but digital domain and no code.
- **On the Fragility of Self-Improving Agents** (`2608.18066`) — re-evaluation
  showing memory-based self-improvement is noisy and task-order-dependent,
  with underspecification a key driver; an important cautionary audit but no
  artifact and digital domain.
- **TRUSS** (`2608.17588`) — evidence-gated agent-skill generation with nine
  safety properties, a controllable shadow execution environment, and
  provenance-preserving traces; strong reported numbers on SkillInject /
  SkillSafetyBench / SkillGenBench but digital and no verified code.
- **D²ACCI** (`2608.17756`) — dual-loop diagnostic protocol for
  evidence-preserving agent memory with a graded observability metric; digital
  memory diagnostics; artifact described but not verified.
- **Ontological Trust / RGE** (`2608.17718`) — deterministic online monitor
  decomposing trajectory-prefix trust into Role, Goal, and Evidence; digital
  (OSWorld, FinanceBench, EICU-AC); no code.
- **Authorization Before Context** (`2608.17148`) — model-neutral audience
  boundary against cross-audience memory leakage in agentic systems; relevant
  memory-safety contract but synthetic preliminary evidence and no artifacts.
- **SkillEffect** (`2608.17007`) — checked-lowering runtime for
  memory-bounded agent tools (audited bounded implementation + postconditions);
  digital; no code.
- **Agentic ESOpt** (`2608.17310`) — evolution-strategies fine-tuning of
  long-horizon agents with minimal GPU memory; digital; no code.
- **Write-Execute-Refine (WER)** (`2608.17587`) — trains a Skill Optimizer
  from execution feedback outside a frozen executor; digital; no code.
- **ASI-Bench** (`2608.17271`) — 60 project-level autonomous scientific
  execution tasks with progressively withdrawn methodological guidance;
  relevant to the autoresearch line but digital and no verified artifact.
- **PRISM** (`2608.17962`) — 5,000-trajectory multimodal industrial skill
  dataset (45 hours, force/tactile/vision); the project page says "Dataset
  soon" and the official repository contains only a README, assets, and an
  HTML page as of 2026-08-19 — treat as not yet released; watch for the
  dataset.
- **FetchMan** (`2608.17027`) — sim-to-real humanoid loco-manipulation
  pipeline (150,000+ scenes, Flow-GRPO refinement, zero-shot Unitree G1
  reach-and-pick at 73.3%) with a released-in-name FetchMan-Bench; no verified
  artifact links; strong result but watch for the benchmark release.
- **PDDL-ART** (`2608.17146`) — autonomous VLM-based PDDL abstraction from a
  single demonstration with execution-guided correction; neurosymbolic
  planning for manipulation; no code.
- **PROBE** (`2608.17129`) — manipulation-grounded visual question answering
  (MG-VQA) benchmark with a VLM-agent fine-tuning recipe; simulation-only; no
  code.
- **No Gaussian Required (AC-MTM)** (`2608.17542`) — replaces the Gaussian
  anti-collapse regularizer in JEPA world models with a contrastive
  inverse-dynamics head; pixel-control tasks rather than robotics; method
  paper with no robot interface.
- **Graphectory Viewer** (`2608.17195`) — open-source process-centric
  trajectory analysis tool for software agents; useful observability artifact
  but SE/digital domain without a robot interface.
- **StagedWorkspace** (`2608.18050`) — versioned workspace contract for
  knowledge-work agents binding parsed views to content hashes; digital; no
  code.
- **AdaLens** (`2608.17834`) — interactive storyline for monitoring and
  steering long-running agentic data analysis; HCI study; digital; no code.
- **Auditing Self-Evolution in Financial Agents** (`2608.17684`) — audit of
  SkillOpt/AWM/ReasoningBank showing capability gains accompany security
  drift; digital finance domain; no artifacts.
- **Wuying-Browser-Agent** (`2608.17319`) — long-horizon browser agents with a
  structured browser harness and BrowserBench; browser domain without a
  robot/harness contract; excluded.
- **If, Then, Otherwise / CondVLN** (`2608.17318`) — scene-graph-grounded
  benchmark for conditional branching in vision-language navigation; VLN
  navigation rather than robot harness/evaluation contracts; no artifacts.
- **LLM-Only PDDL Domain Repair** (`2608.17341`) — planning-domain repair
  study; conventional planning; excluded.
- **Attention Steering for VLA Driving** (`2608.17095`), **ControlledShifts**
  (`2608.17882`) — driving-domain; excluded per policy.
- **WONDER** (`2608.16955`) — radio world-model negotiation for UAV coverage;
  UAV domain; excluded.
- **MobileWorldSafety** (`2608.17659`) — GUI-agent safety benchmark on Android
  apps; GUI domain; excluded.
- **PACE** (`2608.17220`), **When Agents Act on Web3** (`2608.17275`) — DeFi
  contract execution and Web3 MCP attack-surface survey; digital finance
  domains; excluded.
- **DeAR** (`2608.17282`), **PlanPO** (`2608.17289`), **SLAaaT**
  (`2608.17034`), **Benchmarking the Benchmarks** (`2608.17183`), **Safer RAG**
  (`2608.17153`), **Acknowledgment Point** (`2608.17176`), **Token
  Optimization** (`2608.17188`), **Fool's Gold** (`2608.17202`) — digital
  agent/reasoning/security studies without robot or harness-system contracts;
  excluded.
- **SemComp-Bench** (`2608.17426`), **Deep Academic Survey** (`2608.18034`),
  **Procedural Content Metageneration** (`2608.17947`), **Judge, Retrieve, or
  Abstain** (`2608.17994`), **Sampling-Verification Danger Law**
  (`2608.17956`) — video-generation, survey-automation, content-generation, or
  digital-evaluation domains; excluded.
- Generic perception, control, planning, navigation, UAV, humanoid-motion,
  surgical, medical, and conventional RL papers without a reusable agent/VLA
  harness, recovery, safety, or evaluation contract were excluded per policy
  (e.g., `2608.17320`, `2608.17323`, `2608.17347`, `2608.17416`, `2608.17512`,
  `2608.17553`, `2608.17592`, `2608.17596`, `2608.17601`, `2608.17628`,
  `2608.17633`, `2608.17690`, `2608.17691`, `2608.17703`, `2608.17779`,
  `2608.17819`, `2608.17874`, `2608.17928`, `2608.17744`).

## Watch-list status (rechecks)

- **BATON** (`2608.16889`) — already curated in the 08-18 run (README entry);
  re-screened in the 08-17 late batch and not duplicated. Still no code.
- **StageWAM** (`2608.10780`) — no new version, code, or weights; unchanged.
- **ReflexVLA / ReflexBench** (`2608.14379`) — still pre-release
  ("after acceptance"); unchanged.
- **DreamX-Phi 1.0** (`2608.13489`) — still no weights/inference release;
  unchanged.
- **UniTexture** (`2608.13453`) — still no official attack code or benchmark
  artifacts; unchanged.
- **HELIX** (`2608.13951`) — repository live (MIT) since 08-17/18; no further
  change this scan.
- **GigaBrain-0.7** (`2608.15875`) — still awaiting the promised training-code
  and weights release; unchanged.
- **ForceU-VLA** (`2608.15009`) — repository still README-only; partial HF
  dataset; unchanged.
- **MAGE / AppliancePlan** (`2608.15863`), **SPD** (`2608.15917`) — still no
  code; unchanged.
- **Newly added**: MANIGUARD code repo (404), LIBERO-VIFO artifacts,
  LEGO-RL repository (404), Agent Lightning v1.0 code, Hydra-0 code
  ("coming soon"), PRISM dataset ("soon"), VLCP code, HODAgent project/code,
  EATR-Stereo, UniReflex, Prism-GRPO, Calibrated Predictive Safety JEPA,
  QWM, Teach and Grow, Task-Aware Harness Provisioning, Fragility audit,
  TRUSS, D²ACCI, Ontological Trust/RGE, Authorization Before Context,
  SkillEffect, Agentic ESOpt, WER, ASI-Bench, FetchMan-Bench, PDDL-ART,
  PROBE, AC-MTM, Graphectory Viewer, StagedWorkspace, AdaLens.
