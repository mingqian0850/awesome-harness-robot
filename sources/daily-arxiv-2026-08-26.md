# Daily arXiv scan — 2026-08-26

## Scope

- Interval: since the 2026-08-25 run's cutoff (2026-08-25 10:00 UTC) through
  2026-08-26 09:10 UTC. The 08-25 run could not confirm the `submittedDate`
  [20260825…] range (the Wed, 26 Aug 2026 announcement day) as non-empty via
  the API and saw nothing beyond the Tue 08-25 listing; that range is now
  exposed and was screened in full. This is the **first screening pass** over
  the newly exposed batch (not a re-screen of an unchanged batch).
- The arXiv export API search endpoint (`https://export.arxiv.org/api/query`)
  was healthy this run. `submittedDate:[20260825000000 TO 20260826235959]`
  returned **430 unique entries** (cs.RO 33, cs.AI 155, cs.CL 61, cs.CV 82,
  cs.LG 99; many cross-listed), all `v1` with `published == updated`, i.e. the
  entire batch is new submissions announced 2026-08-26; the [20260826…] slice
  (tomorrow's announcement day) returned nothing, so no additional visible
  batch exists and no previous-day re-check was needed (the [20260824] batch
  was fully screened in the 08-25 run). XML saved under the workspace scratch
  dir (`.scratch/arxiv-2026-08-26/`, removed after the run; `/tmp` is not
  persistent in this sandbox).
- Revisions: today's official listing pages for all five categories
  (`/list/{cat}/new`, HTTPS) were parsed for the **Replacement submissions**
  sections and cross-matched against all watch-list IDs from the 08-25 record
  (104 IDs) and all README IDs. Four watch/excluded papers have new versions
  (see Watch-list status below); **DELE-w0.5 v2** (`2608.22067v2`, updated
  2026-08-25) was probed via `id_list` and its abs page — no artifact links
  added, unchanged. **XPolicyLab v3** (`2608.09892v3`, updated 2026-08-25) is
  already in the README with a live repository; the revision changes nothing
  material for the entry.
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action models,
  code-as-policy, skill discovery, memory, recovery, evaluation, runtime
  monitoring, and safety. Candidate claims were checked against the arXiv
  record, official project pages, and official code/model/data repositories
  (live HTTP and GitHub API checks on 2026-08-26); existing README, landscape,
  and source records were checked for duplicates. All performance results
  below remain author-reported unless stated otherwise.

## Included (new curated entries)

### Recuris: Recursive Experiential–Working Memory Evolution for Long-Horizon Agent Harnesses

- Paper: https://arxiv.org/abs/2608.24876
- Code: https://github.com/Gen-Verse/Recuris (Apache-2.0, verified live
  2026-08-26; pushed same day, 15 stars)
- Classification: General Harness Methodology (memory + recursive
  self-improvement).
- Why included: couples Working Memory (task progress) with Experiential
  Memory (skills) so skill invocation is grounded in current need rather than
  the full growing history, and the coupling turns execution itself into
  structured evidence that localizes failures to specific memory components; a
  fixed Meta-Agent converts that evidence into validation-gated, localized
  Skill Memory updates — a bounded recursive memory-evolution loop. The
  authors report success gains in 35 of 37 completed model–benchmark pairs
  across four long-horizon benchmarks (tau-bench +17.8 for GPT-5.6 Sol and
  +15.6 for Claude Opus 5, taking Opus 5 to 87.9%; SkillFlow +16.6/+13.5 on
  Qwen3.6-27B/35B), with the advantage widening to +32.2 points on the longest
  tasks. A real Apache-2.0 artifact in the harness-memory/RSI line (Prime
  Agent, HCL, StateM).
- Boundary: digital agents; no robot experiment.

### PonderPounce: A Pretrained MLLM as an Episode Context Engine for Robot Control

- Paper: https://arxiv.org/abs/2608.24115
- Project: https://worv-ai.github.io/ponderpounce/ (live 2026-08-26; linked
  repo `worv-ai/PonderPounce` exists but is a placeholder — size 0, no
  license, no code; treated literally as not open)
- Classification: Agentic Robot/VLA Harness (episode-memory interface); VLA.
- Why included: reuses a pretrained MLLM's native causal context as robot
  memory instead of a purpose-built history module. Ponder (System 2) keeps
  episode observations, demonstrations, and prior cognition in-context;
  Pounce (System 1) VLA receives the newest continuous cognition token and its
  age asynchronously through the Ponder–Pounce interface (p50 78 ms refresh,
  25 ms action invocation, 20 Hz playback), jointly trained end to end with no
  bridge pretraining. The authors report RoboMME 60.83% (9B) and 50.04%
  (0.8B) versus 44.51% FrameSamp+Modul and 17.93% current-observation π0.5,
  75.54% at 9× data, and RoboCasa-DC learning from action supervision alone.
  A crisp System-2/System-1 harness contract for VLA episode memory (same
  boundary line as Harness VLA, AtlasVLA); simulation evidence, author-reported.
- Boundary: no usable artifacts as of 2026-08-26 (placeholder repo).

### GlanceWAM: Sparse Test-Time Imagination for World-Action Models

- Paper: https://arxiv.org/abs/2608.23927
- Code: https://github.com/linhanwang/GlanceWAM (MIT, verified live
  2026-08-26; pushed same day)
- Classification: VLA / Robot Foundation–World Model (WAM architecture).
- Why included: breaks the speed–success dilemma of WAM test-time imagination
  by generating it asynchronously off the critical path and consuming it in
  latent space: an asynchronous proposer imagines a single lookahead frame on
  a slow clock while the action head decodes chunks at control rate (48 ms),
  via a non-interfering attention mask and staleness-robust horizon training.
  The authors report 72.2% on the 24-task RoboCasa suite (vs 67.1% synchronous
  Cosmos Policy, 64.4% imagination-free co-training) and 99.0% on LIBERO,
  24× faster than synchronous baselines. Same line as RIFT/DynamicWAM/LAWA
  (future conditioning vs latency); real MIT-licensed code.
- Boundary: simulation benchmarks (RoboCasa/LIBERO); results author-reported.

### TrAct: Bridging Robot Control and Visual Prediction with Visual Tracks

- Paper: https://arxiv.org/abs/2608.24101
- Classification: Robot Foundation–World Model (interface contract); VLA.
- Why included: uses *visual tracks* (embodiment-agnostic image-space point
  motion) as the intermediate interface between control and prediction, since
  raw actions are embodiment-specific and weakly aligned with visual change:
  a VLAT proposes candidate action–track pairs, a track-conditioned world
  model rolls out their visual consequences, and a vision-language reward
  model selects the outcome that best satisfies the instruction. The authors
  report success rising 27%→55% on the proposed LIBERO-INTEGRAL benchmark and
  49%→76% on real Franka tasks versus the strong π0.5 VLA baseline. A reusable
  interface contract with real-robot evidence (same WAM-interface line as
  Hydra-0's pixel motion).
- Boundary: author-reported; no official artifacts located as of 2026-08-26.

### NVIDIA Cosmos-H-Dreams: Real-Time Generative Physics Simulation for Surgical Robotics

- Paper: https://arxiv.org/abs/2608.24199
- Runtime: https://github.com/isaac-for-healthcare/Cosmos-H-Dreams (live,
  26 MB, 39 stars; license NOASSERTION)
- Model: https://huggingface.co/nvidia/Cosmos-H-Dreams (live; NVIDIA Open
  Model License, commercial and non-commercial use, global deployment; based
  on nvidia/Cosmos-H-Surgical-Simulator, trained from the Open-H-Embodiment
  surgical corpus)
- Classification: Robot Foundation–World Model / Simulation (interactive
  surgical world model).
- Why included: a causal, few-step self-forcing distilled student of the
  bidirectional Cosmos-H-Surgical-Simulator that streams action-conditioned
  surgical video at ~160 FPS on a single workstation GPU and is
  controller-agnostic — driven live by browser keyboard (WebRTC), Meta Quest
  (WebXR), a commercial surgical console (CMR Versius), or learned policies in
  closed loop. A genuine NVIDIA artifact release (runtime repo + released
  checkpoint) in the generative-simulation line; the released checkpoint
  specializes to dVRK tabletop suturing.
- Boundary: surgical-robotics domain; results author-reported.

### StepGuard: Step-Level Guardrails with Scalable Supervision and Safety-Utility Balancing

- Paper: https://arxiv.org/abs/2608.24777
- Project: https://zheng977.github.io/StepGuard/ (live)
- Code: https://github.com/zheng977/StepGuard (live 2026-08-26, ~6 MB, pushed
  same day; EMNLP 2026; no license declared as of 2026-08-26)
- Weights: https://huggingface.co/ninty-seven/StepGuard (live, verified
  2026-08-26)
- Classification: Evaluation/Safety (pre-execution monitoring); General
  Harness Methodology.
- Why included: a learned step-level guard model that audits completed
  trajectories *and* checks tool actions before execution — the pre-execution
  monitoring regime most guardrails skip — trained with StepGen (automatic
  generation of context-identical safe/unsafe trajectories differing only in
  the risky action) and Balance-GRPO for safety–utility balancing. The authors
  report the highest average accuracy among open-weight guard models
  (comparable to GPT-5.4); guarding agents on AgentDojo and AgentDyn reduces
  mean attack success by 77.3% with a mean utility drop of only 2.8
  percentage points. Real artifact release (code + weights) in the agent
  guardrail line (ClawSentry, SHE, RePolicy).
- Boundary: digital agents; no robot experiment.

## Reviewed but not promoted (watch list)

- **Meta^n: Recursive Self-Improvement through Emergent Depth** (`2608.24735`)
  — keeps a fixed meta-operation Ω and recurses on its own products (each
  layer reads the solver-stack traces and the code that produced them, then
  writes the next layer as a strategic pre-process plus callable helpers),
  with depth set by convergence and an evolutionary archive over layer chains;
  reports gains on all eight benchmark families including ARC-AGI-2. Strong
  RSI position in the harness line; the linked repository
  (github.com/minnesotanlp/meta-n) returns 404 as of 2026-08-26 — not open;
  watch for release.
- **VideoHarness-RSI** (`2608.24302`) — controlled baseline for recursive
  search over executable context-construction programs around a frozen VLM,
  making long-video understanding an instance of automated harness design;
  selected harnesses transfer to new benchmarks without further search.
  Harness-RSI line (Self-Harness, Task-CoEvolve, AutoSaddler); no artifacts;
  watch.
- **StarHarness** (`2608.24804`) — harness evolution with stratified search
  (tasks stratified by baseline failure behavior, proposer-visible search vs
  proposer-hidden selection tasks, held-out generalization); 20–35 pp
  full-benchmark gains across ITBench SRE, EnterpriseOps-Gym, AutomationBench
  after 4–12 accepted changes, transferring across GPT/Qwen families. No
  artifacts; watch.
- **The Empire, Long Divided, Must Unite: Architectural Convergence in Three
  LLM Agent Harnesses** (`2608.23953`) — source-level multi-case study of
  LangChain deepagents, Earendil's pi, and DeepSeek's dsh finding convergence
  on five recurring elements (commoditized loop, append-only replayable
  session record, model quirks as data, progressive disclosure, extension
  seams) and the absence of external verifiability. Directly relevant to
  harness engineering as a discipline; analysis only, no artifact; watch.
- **The Handoff Tax** (`2608.24358`) — measures the cost–quality penalty of
  continuing non-native trajectories across LC→HC model handoffs (full
  escalation recovers less than half the quality gap; the preferred interface
  reverses with direction). Harness-interface measurement; no artifacts; watch.
- **Adaptive Influence Graphs** (`2608.24361`) — structured trace
  representation + agent-directed traversal for failure attribution in
  multi-agent systems; SOTA on Who&When. Observability line; no artifacts;
  watch.
- **RePolicy** (`2608.24275`) — RL-trained agent safeguard that invokes
  context-dependent safety policies from a policy library (PolicyTraj-20K +
  GRPO with verifiable rewards); robust invocation under policy-context
  perturbation across six benchmarks. No artifacts; watch.
- **StepGuard companions** — see included entry above.
- **When "Must" Becomes "Maybe": Constraint Weakening in LLM Agent Workflows**
  (`2608.24569`) — shows handoff transformations (compression, plan
  assimilation, convergence, ownership deferral, precedent substitution)
  degrade action-binding safety state: normal handoff compression yields
  100.0% blocker deactivation and 54.2% forbidden action; restoring all four
  state fields recovers 100.0% preservation. Important harness-state
  measurement (same line as The Compaction Cliff); no artifacts; watch.
- **More Rejective, Not More Discriminative** (`2608.23941`) — twin-prefix
  framework isolating the unit of verification in pre-execution LLM
  oversight: longer review windows make fallible monitors more rejective, not
  more discriminative (informedness peaks at 1–2 actions). Pre-registered
  AI-control evaluation; no artifacts; watch.
- **Bayesian Self-Escalation in Hierarchical LLM Agents** (`2608.24087`) —
  intra-generation delegation as a Bayesian optimal-stopping problem over a
  learned competence posterior; myopic threshold closed form + 1/√n finite-
  sample guarantee; MIT code and Zenodo data live. Digital delegation
  contract; watch.
- **AHEAD** (`2608.24114`) — step-aware agentic RL matching supervision
  sources to step types (environment feedback everywhere, LLM corrective hints
  on error steps); +13.3 ALFWorld / +11.0 WebShop at 7B over GRPO. Digital;
  no artifacts; watch.
- **SPO++** (`2608.24870`) — fixes trajectory-vs-action-token centering
  mismatch in single-stream agentic RL and organizes prompt evidence by
  policy event; better online efficiency on ALFWorld/Math-TIR. Digital; no
  artifacts; watch.
- **SkillForge** (`2608.24747`) — continuous skill evolution with
  evidence-based verification and multi-pathway induction (vs append-only
  SkillRL); ALFWorld/WebShop/AppWorld gains. Digital skill line; no artifacts;
  watch.
- **CAFE** (`2608.24794`) — coupled agent–feedback evolution: a shared model
  alternates search-agent and critic roles with feedback-conditioned recovery;
  out-of-domain retention across seven agentic search benchmarks. Digital; no
  artifacts; watch.
- **Attnlocate / What Guides the Agent?** (`2608.24022`) — runtime framework
  that localizes behavior-guiding instruction spans inside the attention
  matrix (object-detection formulation over attention traces) and adjudicates
  injection attempts by provider authority; mean IoU 0.743 across ten agent
  configurations. Digital runtime safety; no artifacts; watch.
- **OODA-Tool** (`2608.24368`) — typed closed-loop policy separating state
  preservation from action realization (Observe–Orient–Decide–Act) against
  state–action competition in multi-turn tool use. Digital; no artifacts;
  watch.
- **Paritok-4B** (`2608.24188`) — intent-conditioned extractive context
  compressor for coding agents (25.7% of context, 86.5% solve quality kept);
  harness-token line. Digital; watch.
- **PeakBench** (`2608.24509`) — benchmark for resource-aware tool invocation
  with execution-grounded dependency annotations and two-part logical vs
  physical scheduling evaluation. Code URL in the paper
  (github.com/Czzzk/Staggering-the-Peaks) returns 404 as of 2026-08-26 — not
  open; watch.
- **Who is the Agent to Blame?** (`2608.24306`) — per-agent faithfulness
  localization for agentic deep research with a four-type error taxonomy;
  repo live but nearly empty (2 KB, no license). Digital evaluation; watch.
- **BrowserForge** (`2608.24848`) — parallel browser sandboxes over the open
  web generating 203,238 screenshot-only web-agent trajectories (one per
  distinct website). Digital data release claim; watch.
- **LAION-BVD** (`2608.24845`) — 10M-hour open video dataset (80M videos) for
  multimodal pretraining. Large open data release but general video, not
  robot-specific; excluded per policy (note for the data line if robot usage
  emerges).
- **HSR: Hierarchical Skill Retrieval for VLA Adaptation** (`2608.24042`) —
  decomposes target tasks into candidate skill sequences, hybrid subtask-
  language + behavior-feature retrieval, two-stage pretrain/finetune
  adaptation; +10.3% LIBERO / +21.3% real-world over baselines. Project page
  live without code; watch for release.
- **MiGA / GVLA (Gripper-aware VLA)** (`2608.24603`) — multi-gripper dataset
  (103K demonstrations, five gripper types) + multi-gripper tokenizer with
  adapter-based policy routing. Substantial dataset/model contribution; no
  artifacts located; watch for data release.
- **WorldEcho / WorldSync** (`2608.24885`) — diagnosis of off-expert action
  following in robot world models (visual integrity + SE(3) trajectory
  alignment) plus three-axis alignment; RoboTwin + real-robot policy gains.
  World-model evaluation line (WorldSimProbe, RigidBench); no artifacts;
  watch.
- **LAWA / Latent Action as Intention** (`2608.24882`) — compact latent
  actions as future-intention representation enabling efficient test-time
  imagination without future-video generation; SOTA 65.6%/80.8% RoboCasa
  few-shot/full-data, 42.9% lower inference latency than Joint-WAM. No
  artifacts; watch (note: distinct from README entry "What Matters for Latent
  Actions", `2608.19613`).
- **GaussianWAM** (`2608.24714`) — training-time distillation of 3D-Gaussian-
  organized geometry/semantic supervision into WAM representations (FastWAM
  52.05%→71.29%, Cosmos Policy 71.52%→77.30% on LIBERO-Plus), all teachers
  removed after training. No artifacts; watch.
- **CAT: Trajectory-Level Continuous Action Representation** (`2608.24111`)
  — frequency-aware continuous latent action trajectories for manipulation.
  No artifacts; watch.
- **LeFlow** (`2608.24855`) — amortized latent trajectory planning for world
  models (rectified-flow prior in latent dynamics + frozen-world-model
  verification), order-of-magnitude planning-time reduction; MIT code live.
  Digital pixel-control benchmarks; watch.
- **NeoWorld-Pro** (`2608.24212`) — monocular image → executable scene
  programs (URDF) with physics-in-the-loop refinement for embodied simulation.
  No artifacts; watch.
- **From Seeing to Acting: Smart Glasses as First-Person Intelligence
  Platforms** (`2608.24877`) — survey with an L0–L5 first-person
  perception–state–interaction–action framework and a live MIT companion
  repo (awesome-smart-glasses). Wearable-embodiment line; peripheral to robot
  harness curation; watch.
- **GlanceWAM, Recuris, PonderPounce, TrAct, Cosmos-H-Dreams, StepGuard** —
  included above.
- Driving-domain exclusions (per policy): **RoG-DAgger** (`2608.24525`),
  **NeuralParker** (`2608.24485`), **SIREN-Bench** (`2608.24094`),
  **Variance-Guided Spatial Attention Fusion for E2E Driving**
  (`2608.24366`), **GeoWAM v2** (`2608.23486v2`, replacement checked).
- Conventional control/hardware/perception/HRI without reusable
  agent/VLA/harness contracts, excluded: **CARO quadruped locomotion**
  (`2608.24217`), **Trusted Polytopic Action Sets** (`2608.24019`),
  **Safety-aware MPPI with STL** (`2608.23972`), **Sensorless damage-safe
  grasping** (`2608.23983`), **NeurRAFT motion planning** (`2608.24026`),
  **Tooth-preparation coverage planning** (`2608.24155`), **Slip detection
  visuo-tactile** (`2608.24162`), **Durable tactile fingertip**
  (`2608.24242`), **Fiber-optic sensing glove** (`2608.24572`), **FBG
  whiskers** (`2608.24724`), **Event-based motion estimation**
  (`2608.24223`), **One-shot LfD of contact-rich manipulation**
  (`2608.24741`), **VIP navigation planning** (`2608.24618`), **Autonomous
  ships imitation** (`2608.23924`), **Coupling dynamics in HRI teaching**
  (`2608.23994`), **GuRO v2** (`2608.23204v2`), **Free-Energy-Gated Plasticity
  v2** (`2608.23000v2`).
- Generic LLM/CV/CL/LG papers in the screened range without a reusable
  agent/VLA harness, recovery, safety, or evaluation contract were excluded
  per policy (representative: **RefineRank** `2608.23928`, **AgentSpec**
  `2608.24004`, **Interactive Visual Grounding** `2608.23978`, **AgentWorld**
  `2608.24076`, **Parason** `2608.24658`, **TrustDABench** `2608.24145`,
  **Simthesizer** `2608.24650`, **A Literate Programming Environment**
  `2608.24644`, **EgoErrorVQA** `2608.24134`, **DeepRepoQA** `2608.24221`,
  **RecurSE** `2608.24231`, **SA-Bench** `2608.24252`, **Who is the Agent to
  Blame** — see watch, **SteerCheck** `2608.24335`, **FraudBench**
  `2608.24551`, **Math/DoublesEval** `2608.24439`, **OmniJudge or OmniBias?**
  `2608.24160`, **A Judge Should Know What Changed** `2608.24419`).

## Watch-list status (rechecks)

- **DELE-w0.5 / Inferring Action from Future Latent State** (`2608.22067v2`,
  updated 2026-08-25) — revision verified via `id_list` and abs page; no
  artifact links added; unchanged (watch).
- **XPolicyLab** (`2608.09892v3`, updated 2026-08-25) — already in README with
  live repository; revision does not change the entry.
- **GeoWAM** (`2608.23486v2`), **GuRO** (`2608.23204v2`),
  **Free-Energy-Gated Plasticity** (`2608.23000v2`) — excluded-domain papers
  with new versions; re-checked, no qualifying change.
- **Task-CoEvolve** (`2608.20169`), **ForeTime-VLA** (`2608.20735`),
  **SPADE** (`2608.19197`), **ScienceFlow** (`2608.14354`), **GhostTac**
  (`2608.20817`), **GigaBrain-WBC-0.5** (`2608.18234`), **Q-Planning**
  (`2608.21204`), **CounterAlign** (`2608.21740`), **INDI** (`2608.23478`),
  **BATON** (`2608.16889`), **Agent Lightning** (`2608.17528`), **VLCP**
  (`2608.16978`), **MANIGUARD** (`2608.17386`), **LEGO-RL** (`2608.17393`) —
  none appear in today's replacement listings; unchanged.
- All other previously listed watch items (UniMem, M3, LD4WAM, SCVC,
  InstructMove, GuardianBench, SkillBloat, WAM-OPD, MCP-grounded robot
  programs, HVTB, ClawProBench, AUDITA, HANSARD, AIREP, MemGuard, The
  Compaction Cliff, Scroll, There Is No Neutral Harness, WebDev-Skills-Bench,
  Repo2Skill-Evo, Coalition-Aware Skill Reliability, Where World Models
  Break, ParallelWorld, ASP, MCP-Universe RL, GOLEM, Reward-Free Continual
  Adaptation, Lifelong Recomposition, CIDER, Noise Floor Audit, Sycophancy
  Amplification, RAI, TRACE skill bank, Safety Hacking BoN, Macro-Action
  Topological Navigation, BeTaL-GBI, WorldToken, PhyFilter, SR-WM, TONAV,
  Triplet2Track, WorldMind, Mamba SmolVLA expert, ReWorld, MOSH-WM, EchoWM,
  From Generation to Simulation, Capability Separation WAM, Betting
  Certificates, OptiSight, EndoNav, RACO, Geo-VLA, StageWAM, ReflexVLA,
  DreamX-Phi, UniTexture, PRISM, GigaBrain-0.7, ForceU-VLA, Hydra-0, ADEPT,
  LIBERO-VIFO, Koala Gripper, VisTa3D, CertVLA, PhysCaP, GraphOp-WM,
  Logic-VLA, CIVA, COTA, AID-Guard, TraceGrant, Artic, AgenticRAG-FP,
  Criterion Revision, Weighted Memory Tree, Utility Under Attack, Claws in
  Plain Sight, CRATE, Trustworthy RAG, Judgment Receipts, AsmEvo, VT-MUSE,
  ViTacPhys, AUSO, TaPeR, TOSS, Action-JND, EXIMO, HiTac-WAM, OrthoSkillVLA,
  SafeBranch, SCAPE, EAFG, Panda ROS 2 stack, Outcome Monitors, StateMemBench,
  Adaptive Probabilistic Shielding, Orthogonal JEPA, DBOSC, Temporal-Logic
  TAMP, Credit Without Ground Truth, AI4AI-Bench, Optimal Skill Selection,
  Self-Demonstrated VLA fine-tuning, ExPhy, The Missing Touch, Neural Reduced
  Dynamics, Brain Researcher, Thinkingbox) — no new revisions or artifact
  releases detected in today's replacement listings; unchanged.

## Operational notes

- The arXiv API was healthy for the entire run (no 429/500); both the
  `submittedDate` search and `id_list` paths worked. Listing pages were used
  only to enumerate replacement submissions.
- Artifact checks on 2026-08-26: GlanceWAM repo (MIT, live), Recuris
  (Apache-2.0, live), StepGuard code + weights (live, no license declared),
  PonderPounce repo (placeholder, size 0), meta-n (404), PeakBench repo
  (404), Cosmos-H-Dreams GitHub (live, NOASSERTION) + HF model (NVIDIA Open
  Model License).
- The known OpenSSH `/etc/ssh/ssh_config.d` ownership error was handled per
  procedure with `GIT_SSH_COMMAND='ssh -F /dev/null'`.
- Working tree before the run: `docs/reference-architecture.md` modified,
  `docs/ring-harness.png` and `handoff.md` untracked — preserved untouched.
