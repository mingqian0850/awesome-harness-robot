# Daily arXiv scan — 2026-08-25

## Scope

- Interval: since the 2026-08-24 run's cutoff (2026-08-24 07:45 UTC) through
  2026-08-25 10:00 UTC. The 08-24 run recorded that the `submittedDate`
  [20260824…] range returned 0 records ("the 08-24 batch not yet exposed at
  scan time"); that batch is now visible, so **the entire newly exposed
  batch plus today's listings were screened in full** (this is also the
  first screening pass over this batch — it is not a re-screen of an
  unchanged batch).
- The arXiv export API search endpoint (`export.arxiv.org/api/query`) was
  heavily throttled during this run (persistent HTTP 429/500 for
  `submittedDate` search queries across many minutes and multiple retry
  windows; only the `id_list` path and the listing pages were reliable).
  In a brief healthy window it returned the late batch's records
  (e.g. `2608.23224`, published 2026-08-24T13:18Z, cs.RO total = 30),
  confirming that the API's [20260824…] range corresponds to the batch now
  shown on the arXiv listing pages for **Tue, 25 Aug 2026**. Enumeration
  therefore used the official arXiv listing pages (HTTPS) for all five
  categories: `/list/{cat}/new` for cs.RO, cs.AI, cs.CL, cs.CV, cs.LG →
  **575 new + 307 cross + 506 replacement submissions** (1,388 unique
  entries), each with title and category. The `submittedDate`
  [20260825…] range (tomorrow's announcement day) could not be confirmed
  as non-empty via the API; the listing pages expose nothing beyond the
  Tue 08-25 day, so no additional visible batch exists.
- Revisions: all six README/watch-list papers appearing in today's
  replacement listing were probed via `id_list` and abs pages — Task-CoEvolve
  (`2608.20169v2`, 2026-08-24, typo fix + GitHub URL in comments; repo still
  README+figures), ForeTime-VLA (`2608.20735v2`, 2026-08-24, text revision,
  no artifacts), SPADE (`2608.19197v2`, 2026-08-24, minor; code/weights
  already public), ScienceFlow (`2608.14354v2`, 2026-08-23, no artifact
  links), GhostTac (`2608.20817v2`, 2026-08-24, ACM CCS 2026 acceptance
  note; repo already verified), GigaBrain-WBC-0.5 (`2608.18234v2`, project
  page still "Code coming soon"). No revision added qualifying artifacts.
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action
  models, code-as-policy, skill discovery, memory, recovery, evaluation,
  runtime monitoring, and safety. Candidate claims were checked against
  the arXiv record, official project pages, and official code/model/data
  repositories (live HTTP and GitHub API checks on 2026-08-25); existing
  README, landscape, and source records were checked for duplicates. All
  performance results below remain author-reported unless stated
  otherwise.

## Included (new curated entries)

### Prime Agent: A Self-Improving RLM Harness

- Paper: https://arxiv.org/abs/2608.23552
- Code: https://github.com/PrimeIntellect-ai/prime-agent (MIT, verified
  live 2026-08-25; actively maintained, ~18k stars; a persistent IPython
  REPL implementing the Recursive Language Model abstraction, Continual
  Harness state, recursive subagents, and an Agents View for daemon-backed
  sessions)
- Classification: General Harness Methodology.
- Why included: an open-source harness that standardizes execution,
  recovery, verification, and resource accounting for long-horizon
  evaluation and coding-agent workflows while leaving strategy
  construction to the model — the harness failure boundary made explicit.
  The persistent REPL gives programmatic context processing and test-time
  compute outside the prompt; Continual Harness preserves histories,
  memories, skills, prompts, and subagent specs across trajectories;
  recursive subagents coordinate through direct agent-to-agent
  communication. The authors report ARC-AGI-3 RHAE Best@1 rising from 30%
  to 95.5% and matching or exceeding native and popular harnesses across
  long-context tasks. A real, MIT-licensed artifact release in the
  harness-runtime line (StateM, Evo-Harness).
- Boundary: digital agents; no robot experiment.

### AutoSaddler: Automatic Harness Optimization with Durable Updates from Agent Execution Traces

- Paper: https://arxiv.org/abs/2608.23041
- Project: https://autosaddler-projectpage.github.io/ (live 2026-08-25;
  code marked "Coming Soon" — described as substantial but not open)
- Classification: General Harness Methodology.
- Why included: formulates harness improvement as an offline learning
  problem — failure-trace diagnosis, structured patch generation that
  treats the harness itself as code, and validation-based update selection
  with durable, per-patch updates from mini-batches of execution traces
  (a Capability vs Steering patch taxonomy). The authors report
  +9.0/+9.6/+10.0 percentage points over the base harnesses on
  GAIA2, SWE-Bench Pro, and Terminal-Bench 2.0, with ablations isolating
  deep debugging, structured interfaces, and validation-based selection.
  The same "the harness is the optimization target" line as
  Task-CoEvolve/HCL/EvoHarness-RL, with a live project page; artifact
  status: no verified code yet.
- Boundary: digital agents; no robot experiment.

### Think Only When Needed (TOWN-VLA): Prompt-Authority Control for Selective Slow-Path Intervention in VLA Manipulation

- Paper: https://arxiv.org/abs/2608.23224
- Classification: Agentic Robot/VLA Harness (intervention authority);
  VLA.
- Why included: makes *prompt authority* an explicit harness contract for
  frozen VLA policies. Retrieved text becomes a control intervention once
  it enters the executed prompt; a matched audit shows raw appended text
  dropping mean success from 92.47% to 3.00%, with meaningful and
  length-matched meaningless appends both failing on all 500 states
  ("prompt-form collapse": changing the instruction form, not adding
  semantics, dominates execution). TOWN-VLA separates candidate generation
  from permission to alter the policy input: a fixed compatibility rule
  authorizes a canonical compact instruction, otherwise the interface
  restores the original Base prompt exactly. Across 900 audited routes
  every route follows the contract (525 recover Base with matching
  hashes; all 375 authorized prompts preserve the task signature). On a
  matched 4×7 LIBERO-Plus evaluation with 10,030 episodes per method,
  success rises 69.5%→73.1% (+362 episodes; 95% CI 1.89–5.45 points) across
  six perturbation axes and all four suites; on a physical PiPER arm with a
  frozen π0.5 checkpoint, success rises 52.7%→78.7% over 150 trials per
  method (p = 3.16×10⁻⁶). A crisp statement of the model-proposes /
  harness-decides boundary at the prompt layer, with real-robot evidence.
- Boundary: author-reported; no official artifacts on the arXiv record as
  of 2026-08-25.

### Pointing-VLA: Typed Spatial Grounding Interfaces for Vision-Language-Action Manipulation

- Paper: https://arxiv.org/abs/2608.23138
- Classification: Agentic Robot/VLA Harness (typed interface contract);
  VLA.
- Why included: replaces the two brittle spatial channels of current VLAs —
  autoregressive text coordinates and opaque action tokens — with a typed
  hidden-state spatial readout: geometry-specific heads predict normalized
  points, object-functional-grounding (OFG) heatmaps, and visual
  trajectories without serializing geometry as text, and an explicit
  execution contract assigns PICK to source-conditioned OFG and PLACE to
  Pointing (stage-aligned spatial targets). On the evaluated Bridge/WidowX
  four-task set the authors report 72.9% average success without
  Bridge-specific fine-tuning under collision-enabled CuRobo execution;
  the OFG/contact readout transfers to NORA-1.5 preserving or improving
  success while cutting recorded controller time by more than 20×. A
  reusable interface contract for spatial grounding (the same boundary as
  OpenETA's typed tool calls).
- Boundary: author-reported; no official artifacts located on the arXiv
  record as of 2026-08-25.

### The Imitator Game: Benchmarking Robot Imitative Ability Beyond Action Prediction

- Paper: https://arxiv.org/abs/2608.22301
- Code: https://github.com/imitator-game/The-Imitator-Game (MIT, verified
  live 2026-08-25; official repo with baseline implementations — ACT,
  DETR-style — and eval/train templates)
- Data: https://huggingface.co/datasets/imitator-game/IG-10K-Dataset
  (verified; plus assets set and an Imitator Arena blind A/B platform)
- Classification: Evaluation/Safety (benchmark); VLA.
- Why included: a four-level benchmark (L0–L3) that progressively widens
  the gap between a human demonstration and the robot's own scene,
  isolating where trajectory replay ceases to suffice and task
  understanding becomes necessary — imitating *intent* rather than only
  action. Ships IG-10K, the largest environment-aligned paired
  human–robot dataset to date and the only one instantiated across all
  four levels in both real and simulated settings (20,000+ paired
  episodes, 50+ tasks, 6 domains), plus a blind A/B evaluation arena.
  Real benchmark, data, and evaluation-contract artifacts.
- Boundary: results across nine evaluated methods are author-reported;
  dataset scale claims are from the official release materials.

### CatchBench: When Can an Agent Failure Be Caught?

- Paper: https://arxiv.org/abs/2608.22808
- Code/data: https://github.com/yzhao062/catchbench (MIT, verified live
  2026-08-25)
- Classification: Evaluation/Safety.
- Why included: an agent-failure auditing benchmark that treats the
  auditor's information state as the independent variable — the declared
  configuration before a run (PRE), a growing trace prefix (LIVE), and the
  finished trace (POST) — under one task–method interface, with seven task
  contracts (four evidential, three mechanism diagnostics) carrying their
  own labels and metrics. The release scores 72 entrants from rule
  scanners and structural models to eleven LLM judges across nine model
  families over 1,187 declared configurations and 1,162 recorded runs, and
  honestly reports that most of the arena does not order (47 of 118
  pre-declared contrasts separate; the rest are published unresolved).
  A discriminating evaluation contract for runtime monitoring and
  failure-attribution claims (the Outcome Monitors / runtime monitoring
  line), with real MIT-licensed code and data.
- Boundary: digital agents; no robot experiment.

### ROS2SmolVLA: Enabling Small Vision-Language-Action Models for Integration into Industrial-Grade Lightweight Robots

- Paper: https://arxiv.org/abs/2608.23320 (accepted, Industry of the
  Future and Smart Manufacturing 2026)
- Code: https://github.com/una-auxme/ros2smolvla_docker plus companion
  repos (interface_camera, ur10e_real, ur10e_sim) — Apache-2.0, verified
  live 2026-08-25 (Docker definitions, LeRobot framework integration,
  ROS 2 ↔ SmolVLA interface for Universal Robots UR10e, sim and real
  stacks)
- Classification: Runnable Agent-to-Robot Bridge; VLA harness integration.
- Why included: a real, released ROS 2 interface that adapts Hugging Face's
  SmolVLA to industrial-grade Universal Robots hardware, directly
  addressing the compliance/on-premise and lab-hardware gaps that keep VLAs
  out of industrial settings. The author-reported validation covers
  simulation and a real UR10e pick-and-place path. A concrete open
  middleware artifact in the robot-middleware-as-harness line.
- Boundary: author-reported validation; the released repos are integration
  scaffolding rather than new model weights.

## Reviewed but not promoted (watch list)

- **UniMem** (`2608.22869`) — unifies high-level multimodal memory and
  low-level control in one VLA backbone (event classifier, keyframe
  encoder, keyframe caching); five simulation and four hardware memory
  tasks. Substantial VLA-memory architecture, but no official artifacts
  located as of 2026-08-25; watch.
- **Modality Masking Mechanism (M3)** (`2608.22419`) — training-only
  modality-channel masking for query-based bimanual VLA robustness;
  +21.7% (Clean) / +11.4% (Clean2Rand) over an Adapter baseline on ten
  RoboTwin 2.0 tasks plus three real-world tasks. Simple, well-executed;
  no artifacts located; watch.
- **LD4WAM** (`2608.22403`) — motion-aligned latent dynamics for WAMs
  trained on human+robot video (5,000+ hours); RoboTwin and real-robot
  results. No artifacts located; watch.
- **DELE-w0.5 / Inferring Action from Future Latent State** (`2608.22067`)
  — argues video generation is an unnecessary intermediate objective for
  WAMs and infers actions from a compact future latent state. Notable
  conceptual position; no artifacts; watch.
- **Selective Cross-View Consistency (SCVC)** (`2608.21402`) — shows
  cross-view consistency on the covariant output block of WAMs is provably
  harmful (shrinkage law 1/(1+4λ)) and constrains only the invariant block;
  LIBERO-based carve-and-hold-out protocol. Strong analysis; simulation
  only, no artifacts; watch.
- **CounterAlign** (`2608.21740`) — counterfactual instruction relabeling
  turns expert demonstrations into dense corrective supervision for
  offline VLA RL without extra rollouts; project page live but code marked
  "Coming Soon"; watch for release.
- **Intention Distillation (INDI)** (`2608.23478`) — distills
  behavior-level intent from a frozen teacher VLM into the VLA action
  decoder; project page live, no code links; watch.
- **InstructMove** (`2608.22990`) — text-indispensable instruction-
  following manipulation benchmark (semantic distractors; category /
  attribute / spatial / compositional decomposition). Strong evaluation
  idea; no artifacts located as of 2026-08-25; watch.
- **GuardianBench** (`2608.21928`) — same-scene instruction-contrastive
  safety benchmark for embodied AI (3,024 scene-instruction examples,
  standards-grounded); average pair accuracy of state-of-the-art VLMs is
  only 24.1%. Evaluation/safety line; no artifacts located; watch.
- **SkillBloat** (`2608.21929`) — token-amplification resource-abuse
  attacks via skill injection into coding agents (5.4–10.1× amplification);
  digital skill-security line; no artifacts; watch.
- **WAM-OPD** (`2608.22364`) — on-policy distillation repair for
  video-first WAM students (deployment-consistent history distribution);
  preliminary RoboTwin 2.0 results; watch.
- **MCP-grounded robot program generation** (`2608.21417`) — RAG-grounded
  ABB RAPID code generation with an MCP server into RobotStudio for
  simulation-based correction (CIE53). Industrial code-as-policy workflow;
  simulation evidence, no artifacts; watch.
- **Hack-Verifiable Terminal Bench (HVTB)** (`2608.22103`) — embeds
  detectable hacks into Terminal Bench tasks to measure reward hacking
  automatically; environments and traces released. Digital; watch.
- **ClawProBench** (`2608.22510`) — trace-aware, runtime-native agent
  evaluation on OpenClaw (102-scenario full profile + 68-scenario frozen
  holdout). Digital; watch.
- **AUDITA** (`2608.22160`) — tamper-evident record + certified graded
  causal attribution for multi-agent fleets; digital; no artifacts; watch.
- **HANSARD** (`2608.22512`) — forensic readiness / runtime witnessing /
  graded attribution reference architecture against attribution
  laundering; digital; no artifacts; watch.
- **AIREP** (`2608.21363`) — protocol for per-decision evidence records of
  AI runtime governance with reference implementation + two-verifier
  conformance kit; digital; watch.
- **MemGuard** (`2608.21867`) — persists verifier signals as lifecycle
  metadata for agent memory governance (admission + drift); Terminal-Bench
  2.0/SWE-bench/WebArena/Mind2Web; digital; no artifacts; watch.
- **The Compaction Cliff** (`2608.22752`) — measures safety-rule loss under
  context compaction (Claude Code /compact preserves 53% of safety rules
  after one round, 10% after five) with a Knowledge Triage fix. Important
  harness-state measurement; digital; no artifacts; watch.
- **Scroll / Context as an Environment** (`2608.21690`) — programmatic
  context management: append-only event log + sandboxed persistent Python
  kernel as a Session Environment; digital; no artifacts; watch.
- **There Is No Neutral Harness** (`2608.21382`) — fragility grid:
  12 models × 3,679 items × 26 equally defensible harness configurations
  shows model scores are bands, not points (e.g. gemma4-31b 31–8…). Direct
  evidence that the evaluation harness is an independent variable; digital;
  no artifacts; watch.
- **WebDev-Skills-Bench / Signal or Noise?** (`2608.23067`) — controlled
  study of 31 public web-dev skills: target-skill injection reduces mean
  Pass@2 by 1.3–4.2% and raises token cost 72–394%; length-matched controls
  expose prompt-length artifacts. Negative-result evidence for skill
  injection; digital; no artifacts; watch.
- **Repo2Skill-Evo** (`2608.21964`) — skill staleness after repository
  releases (105 release transitions, 57 repos); digital; watch.
- **Coalition-Aware Skill Reliability** (`2608.22610`) — skill-bank audits
  revealing coalition pollution and cross-domain utility reversal; digital;
  watch.
- **Where World Models Break** (`2608.22421`) — natural-input failure
  discovery for world models under a finite query budget; digital/control
  domains; watch.
- **ParallelWorld** (`2608.22971`) — multi-horizon test-time scaling for
  embodied reasoning (verifier-guided tree search); watch.
- **ASP / Budget-Constrained Embodied Perception** (`2608.22975`) —
  four resource walls + training-free wrapper; pre-registered evaluation on
  a synthetic benchmark only (natural-video benchmarks not run); watch.
- **CatchBench companion methods** — see included entry above.
- **MCP-Universe RL** (`2608.22167`) — open framework for RL of MCP
  tool-use agents (environment + rollout orchestration layers); digital
  harness-RL line; watch.
- **GOLEM** (`2608.21550`) — open-source modular humanoid architecture for
  EV battery disassembly (Unitree H1-2, Docker/ROS 2, MuJoCo/IsaacLab
  twins). Application-specific but reusable module interfaces; watch.
- **Reward-Free Continual Adaptation** (`2608.23452`) — world-model
  reward predictor + frozen-encoder adaptation for space robots under
  hardware degradation (SPAICE 2026; source code linked in comments);
  watch.
- **Lifelong Robot Recomposition** (`2608.21676`) — symmetric-monoidal
  compositional framework with SMT-based design/runtime synthesis and
  lifecycle queries; watch.
- **CIDER** (`2608.21899`) — continual interactive distillation for
  real-world embodied RL (six household/industrial tasks); watch.
- **Noise Floor Audit** (`2608.22331`) — matched AST-grading audit of
  tool-calling endpoint variability (rerun vs perturbation floors); digital;
  watch.
- **Agentic Scaffolding Amplifies Sycophancy** (`2608.21377`) — interaction
  scaffolding (feedback loops, reconsideration checkpoints) systematically
  amplifies sycophancy (−6.3 pp accuracy across 4,800 judgments; UAI 2026
  Safe AI workshop). Harness-side behavioral effect measurement; digital;
  watch.
- **Runtime Action Interference (RAI)** (`2608.21398`) — deployment-time
  action pacing/filtering preserving policy parameters (AlphaStar
  replication; open-source repo claimed, not located on arXiv); digital
  game domain; watch.
- **TRACE skill bank** (`2608.22793`), **Safety Hacking in constrained
  BoN** (`2608.22915`), **Macro-Action Topological Navigation**
  (`2608.23055`), **BeTaL-GBI** (`2608.21503`) — digital or conventional
  RL contributions without harness contracts or verified artifacts; watch.
- **WorldToken** (`2608.22591`), **PhyFilter / Physics Filtering**
  (`2608.22701`, npj Robotics), **SR-WM** (`2608.22294`), **TONAV**
  (`2608.22296`), **Triplet2Track** (`2608.22800`), **WorldMind**
  (`2608.21439`), **Mamba SmolVLA expert** (`2608.21407`), **ReWorld**
  (`2608.23565`), **MOSH-WM** (`2608.22750`), **EchoWM** (`2608.23189`),
  **From Generation to Simulation** (`2608.23070`), **Capability Separation
  WAM** (`2608.22197`) — policy/world-model/representation contributions
  without verified artifacts or harness contracts; watch.
- **Betting for Sim-to-Real Performance Certificates** (`2608.21572`) —
  portfolio-betting certificates linking a bank of simulators to
  confidence bounds on real outcomes; novel evaluation methodology, no
  artifacts; watch.
- **OptiSight** (`2608.23354`), **EndoNav** (`2608.22093`), **RACO**
  (`2608.22678`), **Geo-VLA** (`2608.21440`), **GOLEM** (above) —
  navigation/application systems without reusable harness contracts;
  OptiSight and RACO noted for open code claims; watch.
- Driving-domain exclusions (per policy): **RiskWorld** (`2608.21414`),
  **GeoWAM** (`2608.23486`), **MomADv2** (`2608.23405`), **Scaling
  Curriculum Learning for AD** (`2608.22549`), **BehaviorWorldGen**
  (`2608.22187`), **Active Interaction-Aware MPPI** (`2608.21400`),
  **ODG-NoMaD** (`2608.21395`) — excluded; BehaviorWorldGen and RiskWorld
  noted for reusable world-model ideas if they generalize.
- Conventional control/hardware/perception/HRI without reusable
  agent/VLA/harness contracts, excluded: **RoboShape** (`2608.21380`),
  **Force/Torque kinematic adaptation** (`2608.21592`), **DELTA
  quadruped locomotion** (`2608.22033`), **Safety-Critical Aerial
  Teleop** (`2608.21735`), **Vision-Guided Excavation** (`2608.21778`),
  **Tactile material perception** (`2608.21894`), **Thruster fault
  adaptation** (`2608.22976`), **Free-Energy-Gated Plasticity**
  (`2608.23000`), **Morphology evolution** (`2608.23100`), **GuRO**
  (`2608.23204`), **OpenSCvx** (`2608.21631`), **Companion/service-robot
  HRI** (`2608.21387`, `2608.21388`), **Robot privacy position**
  (`2608.21410`), **ISS 3DGS reconstruction** (`2608.21685`),
  **Multi-robot warehouse tuning** (`2608.21533`), **Mobile manipulation
  RL** (`2608.21554`), **EMG-IMU HRI** (`2608.21620`), **Digital twin
  clinics** (`2608.21416`, medical-scene evaluation; noted for the
  task-based evaluation protocol), **Capsulorhexis skill transfer**
  (`2608.21441`), **Lifelong co-design** (`2608.21676`, above).
- Generic LLM/CV/CL/LG papers in the screened ranges without a reusable
  agent/VLA harness, recovery, safety, or evaluation contract were
  excluded per policy (representative: **LitReview Arena** `2608.21374`,
  **K-Bench** `2608.21601`, **StrategyBench** `2608.23475`, **EarthVerse**
  `2608.23525`, **FIDES** `2608.23308`, **NetConfArena** `2608.23179`,
  **SWE Refactor Bench** `2608.23564`, **GuardedBest-of-N** `2608.22915`,
  **Terminal agents surveys** — noted as digital-agent data points).

## Watch-list status (rechecks)

- **Task-CoEvolve** (`2608.20169v2`, updated 2026-08-24) — v2 is a typo fix
  adding the GitHub URL to the comments; the repository still contains only
  README + figures (verified 2026-08-25); unchanged.
- **ForeTime-VLA** (`2608.20735v2`, updated 2026-08-24) — text revision
  only; no code/weights on the arXiv record; unchanged.
- **SPADE** (`2608.19197v2`, updated 2026-08-24) — minor revision; code and
  weights were already public; unchanged.
- **ScienceFlow** (`2608.14354v2`, updated 2026-08-23) — no artifact links
  added; unchanged.
- **GhostTac** (`2608.20817v2`, updated 2026-08-24) — adds ACM CCS 2026
  acceptance note; repository previously verified; unchanged.
- **GigaBrain-WBC-0.5** (`2608.18234v2`) — project page
  (shepherd1226.github.io/gigabrain-wbc-0.5/) verified; still "Code coming
  soon"; unchanged.
- **Q-Planning** (`2608.21204`) — project page unchanged; code still
  "coming soon"; unchanged.
- **CounterAlign** (`2608.21740`) — NEW watch item; project page live, code
  "Coming Soon".
- **INDI / Act with Intent** (`2608.23478`) — NEW watch item; project page
  live, no code links.
- **Imitator Game** (`2608.22301`), **CatchBench** (`2608.22808`),
  **ROS2SmolVLA** (`2608.23320`), **Prime Agent** (`2608.23552`) — included
  above with verified artifacts.
- All other previously listed watch items (EXIMO, HiTac-WAM, OrthoSkillVLA,
  SafeBranch, SCAPE, EAFG, Panda ROS 2 stack, Outcome Monitors,
  StateMemBench, Adaptive Probabilistic Shielding, Orthogonal JEPA, DBOSC,
  Temporal-Logic TAMP, Credit Without Ground Truth, AI4AI-Bench, Optimal
  Skill Selection, Self-Demonstrated VLA fine-tuning, ExPhy, The Missing
  Touch, Neural Reduced Dynamics, Brain Researcher, Thinkingbox, StageWAM,
  ReflexVLA/ReflexBench, DreamX-Phi, ForceU-VLA, UniTexture, PRISM,
  GigaBrain-0.7, BATON, Hydra-0, ADEPT, LIBERO-VIFO, Agent Lightning v1.0,
  VLCP, MANIGUARD, LEGO-RL, Koala Gripper, VisTa3D, ForeTime-VLA, CertVLA,
  PhysCaP, GraphOp-WM, Logic-VLA, CIVA, COTA, AID-Guard, TraceGrant, Artic,
  AgenticRAG-FP, Criterion Revision, Weighted Memory Tree, Utility Under
  Attack, Claws in Plain Sight, CRATE, Trustworthy RAG, Judgment Receipts,
  AsmEvo, VT-MUSE, ViTacPhys, AUSO, TaPeR, TOSS, Action-JND) — no new
  revisions or artifact releases detected in today's replacement listing;
  unchanged.

## Operational notes

- The arXiv API search endpoint was rate-limited/erroring for most of this
  run (HTTP 429/500 on `submittedDate` queries; `id_list` unaffected).
  Screening was completed from the official listing pages plus abs-page
  verification; the 08-24 batch identity was confirmed during a brief API
  healthy window (cs.RO total = 30, IDs overlapping the Tue 08-25 listing).
- The known OpenSSH `/etc/ssh/ssh_config.d` ownership error was handled
  per procedure with `GIT_SSH_COMMAND='ssh -F /dev/null'`.
