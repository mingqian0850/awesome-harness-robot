# Daily arXiv scan — 2026-08-27

## Scope

- Interval: since the 2026-08-26 run's cutoff (2026-08-26 09:10 UTC) through
  2026-08-27 ~06:00 UTC. The 08-26 run's `[20260826…]` slice (the batch
  announced 2026-08-27 00:00 UTC) returned nothing at its query time; that
  batch is now exposed and is the main target of this run. This is the
  **first screening pass** over the newly exposed batch.
- The arXiv export API (`https://export.arxiv.org/api/query`) was healthy.
  `submittedDate:[20260826000000 TO 20260826235959]` returned **317 unique
  entries** (cs.RO 41, cs.AI 127, cs.CL 75, cs.CV 88, cs.LG 118 raw hits, many
  cross-listed), all `v1` with `published == updated` — the entire batch is new
  submissions announced 2026-08-27. `submittedDate:[20260827000000 TO
  20260827235959]` returned nothing (tomorrow's announcement day). XML saved
  under the workspace scratch dir (`.scratch/arxiv-2026-08-27/`, removed after
  the run; `/tmp` is not persistent in this sandbox).
- Recheck of the previous batch: `submittedDate:[20260825000000 TO
  20260825235959]` now returns **436 unique entries** vs 430 at the 08-26 run's
  query time, so the previous batch grew slightly (late additions possible).
  Per procedure, the relevant subset of that batch was re-screened once; it
  surfaced candidates the 08-26 record did not name (SimVerity, Edge
  Skillguard, GaussVLA, RDR, Game2World, CRESSim-Neo, Longitudinal LfD,
  BFN-RL) — all are documented below so later agents can distinguish deliberate
  rejection from an overlooked candidate.
- Revisions: 8 entries in the [20260825] set carry `published != updated`
  (Simthesizer v2 `2608.24650`, AgentWorld v2 `2608.24076`, IAPO v2
  `2608.24588`, JEPA-x v2 `2608.24044`, The Invisible Editorial Layer v2
  `2608.24662`, Diverse by Reasoning v2 `2608.24001`, Matched Excess-Outranker
  v2 `2608.24273`, and **CAT v2** `2608.24111` — the last is a watch item,
  probed via `id_list`/abs page: no artifact links added, unchanged). Today's
  official listing pages for all five categories (`/list/{cat}/new`, HTTPS)
  were parsed for Replacement submissions and cross-matched against every
  watch-list ID from the 08-26 record; **DELE-w0.5 v3** (`2608.22067v3`,
  updated 2026-08-26T12:45) was probed via its abs page — no artifact links
  added, unchanged (watch).
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action models,
  code-as-policy, skill discovery, memory, recovery, evaluation, runtime
  monitoring, and safety. Candidate claims were checked against the arXiv
  record, official project pages, and official code/model/data repositories
  (live HTTP and GitHub API checks on 2026-08-27); existing README, landscape,
  and source records were checked for duplicates (no name/ID collisions,
  including the watch-list PRISM `2608.17962` vs today's PRISM `2608.25666`).
  All performance results below remain author-reported unless stated
  otherwise.

## Included (new curated entries)

### JIT-Agent: Scaling Harness Intelligence via Just-in-Time Harness Evolution

- Paper: https://arxiv.org/abs/2608.25593
- Project: https://bingreeky.github.io/JIT-site (live 2026-08-27; no code
  repository located as of 2026-08-27)
- Classification: General Harness Methodology (trainable harness intelligence).
- Why included: treats harness intelligence itself as a trainable capability:
  the agent harness (memory management, planning strategy, action protocol,
  tool/skill orchestration) is formalized as a composable, machine-generatable
  artifact under a fixed four-module protocol, and a purpose-built
  harness-intelligence model synthesizes task-adaptive harnesses on the fly for
  arbitrary off-the-shelf agentic LLMs, repairs them for stable execution, and
  self-evolves by distilling performance signals from an expanding archive of
  prior harness configurations. With JIT-Agent as a harness helper, the authors
  report DeepSeek-V4-Flash surpassing GPT-5.6 on DeepSearchQA (+9.1) and
  OdysseyBench (+4.3), and GLM-5.2 gaining up to +20.2 points, with
  JIT-Agent-generated harnesses competitive with mature runtimes (OpenCode,
  Claude Code). The strongest statement yet of the "harness intelligence is
  trainable" thesis in the Self-Harness/Task-CoEvolve/StarHarness line.
- Boundary: digital agents; no robot experiment; project page only (no code).

### OpsHarness: A Self-Evolving Harness for Root Cause Analysis

- Paper: https://arxiv.org/abs/2608.25661
- Classification: General Harness Methodology (self-evolving, verification-gated
  harness).
- Why included: contributes a quantitative argument and a design for *why* LLM
  RCA should be harness-led: a comparative study finds general-purpose agents
  now often surpass purpose-built RCA agents, and the remaining accuracy gap
  stems mainly from the external adaptation layer — the harness. OpsHarness
  then makes that harness self-evolve: a data plane combines layered
  operational knowledge with an idea-card tool library, a control plane
  coordinates setup, diagnosis, evolution, and verification, and evolution
  contrasts successful and failed trajectories, converts evidence into atomic
  proposals, and admits updates only through a dual-gate verification process
  designed to prevent overfitting and regression. A clean harness-ownership
  decomposition (data plane vs control plane, atomic proposals, dual-gate
  admission) in the Evo-Harness/Harness-R1 line.
- Boundary: digital SRE agents; no robot experiment; no official artifacts
  located as of 2026-08-27.

### Zero-WAM: In-Context World-Action Modeling from Human Videos for Open-Ended Task Generalization

- Paper: https://arxiv.org/abs/2608.26103
- Project: https://robbyant-research.github.io/Zero-WAM/ (live 2026-08-27)
- Code: https://github.com/robbyant-research/Zero-WAM (Apache-2.0, ~69 MB,
  pushed 2026-08-27, 33 stars; verified live)
- Classification: Agentic Robot/VLA Harness (in-context task specification);
  VLA / World-Action Model.
- Why included: brings in-context learning to manipulation by making a *human
  video* the task specification: a causal video-action model executes unseen
  tasks by following in-context human-video guidance, with an automatic
  pipeline that converts task-sampled robot trajectories into semantically
  matched human videos (HumanGen: 74.2K human-robot ICL pairs across 8.6K
  tasks) and an in-context future-chunk prediction objective that suppresses
  seen-task shortcuts. The authors report 47.0% average success on seven unseen
  RoboTwin 2.0 tasks (+29.5 points over the strongest video-action baseline)
  and real-world generalization to unseen tasks from human video guidance. A
  real Apache-2.0 artifact in the in-context VLA/WAM line (StellaVLA,
  In-Context VLA).
- Boundary: simulation evidence (RoboTwin 2.0) plus author-reported real-world
  results; results author-reported.

### StreamPI: Streaming Multimodal Temporal Modeling for Vision-Language-Action Models

- Paper: https://arxiv.org/abs/2608.26067
- Project: https://happinesslz.github.io/projects/StreamPI (live 2026-08-27)
- Code: https://github.com/hku-sail/StreamPI (Apache-2.0, pushed 2026-08-27,
  13 stars; verified live — full openpi-based training/eval implementation
  with real-robot examples: UR5, ALOHA, CALVIN, LIBERO, DROID)
- Classification: VLA (temporal modeling/architecture).
- Why included: equips single-frame VLAs (pi0.5-class) with temporal reasoning
  without adding parameters: instruction-anchored temporal modeling treats each
  (observation, instruction) pair as an atomic temporal unit — bidirectional
  attention within the pair, causal attention across pairs — so the language
  instruction stays a persistent semantic anchor, and a random-interval
  streaming training strategy bridges synchronous training to asynchronous
  real-robot deployment (randomized inter-frame intervals improve robustness
  to frame-timing perturbation). Real artifact release (code, examples,
  norm-stats assets) in the streaming/temporal VLA line.
- Boundary: author-reported results on real-robot tasks; trained-checkpoint
  availability not individually enumerated as of 2026-08-27 (training builds
  on openpi/pi0.5 checkpoints).

### MA-VLA: Multi-Arm Vision-Language-Action Model for Collaboration and Compositional Generalization

- Paper: https://arxiv.org/abs/2608.25864
- Code: https://github.com/zhangzaibin/future-robots (live 2026-08-27;
  Apache-2.0/MIT declared license; ECCV 2026; the paper states code, models,
  and data are available at this repository)
- Classification: Agentic Robot/VLA Harness (multi-arm task assignment); VLA.
- Why included: decomposes cooperative multi-arm behavior into mid-level atomic
  prompts allocated to individual arms (explicit subgoal specification and
  compositional reuse), and introduces Arm Shuffle — a training-time
  permutation of per-arm observation, state, and assigned atomic prompts that
  enforces role-agnostic instruction following — yielding multi-arm
  compositional generalization to unseen coordination patterns, benchmarked
  with test-time collaboration patterns held out of training. Prior SOTA VLAs
  largely fail under unseen collaborations while MA-VLA succeeds in simulation
  and on a real dual-arm platform. Real artifact release with a benchmark in
  the multi-arm VLA line.
- Boundary: results author-reported; model/data artifacts not individually
  enumerated as of 2026-08-27.

### UCAG-P: One Policy, Many Embodiments (Unified Camera-Centric Action Geometry Pre-training)

- Paper: https://arxiv.org/abs/2608.26058
- Project: https://public-bots.github.io/UCAG-P (live 2026-08-27)
- Code: https://github.com/Public-BOTs/UCAG-P (live but a placeholder — "paper
  figures and project page; code release coming soon", no license; treated
  literally as not open)
- Classification: VLA / Robot Foundation Model (cross-embodiment action space).
- Why included: a structural answer to the cross-embodiment bottleneck: instead
  of robot-specific commands as the shared policy target, UCAG-P represents
  manipulation as camera-observable anchor motion in image and camera-frame
  coordinates, treating robot arms, humanoids, and human hands as embodiments
  of a common action schema, with a geometry-conditioned action translator
  converting predicted motion into executable controls per target embodiment.
  Trained on 4.03K hours of robot/simulation data and 2.34K hours of human
  demonstrations, one checkpoint reaches 98.3% on LIBERO, 88.7%/89.2% on
  RoboTwin Easy/Hard, and 82.0% zero-shot on LIBERO… (author-reported). A
  distinctive camera-centric action-geometry contract (same boundary line as
  Hydra-0's pixel motion, but action-side).
- Boundary: no usable artifacts as of 2026-08-27 (placeholder repo); results
  author-reported.

### GaussianDream++: Efficient 3D Gaussian World Modeling for Robotic Manipulation

- Paper: https://arxiv.org/abs/2608.25659
- Code: https://github.com/TuojingAI/GaussianDream (Apache-2.0, live since
  2026-07-10, 79 stars; the official repository of the GaussianDream series
  the paper extends)
- Project: https://tuojingai.github.io/GaussianDream-Series-project-page/
  (live 2026-08-27)
- Classification: VLA / Robot Foundation–World Model (training-time world
  supervision).
- Why included: makes 3D-Gaussian world supervision policy-native and
  inference-free: World State/Prediction Tokens are inserted directly into the
  VLA backbone, a training-only World Representation Head decodes them into a
  Current World plus coupled Future Prediction over shared Gaussian primitives
  with static–dynamic factorization, and at inference the head, renderer, and
  auxiliary pathways are removed, leaving only 20 world tokens — no online
  Gaussian decoding or rollout. The authors report 98.6% on LIBERO and 87.8%
  on LIBERO-Plus with clear gains under camera and layout perturbation. Real
  Apache-2.0 artifact in the 3D-structured world-model-for-VLA line (GAF,
  GeoPredict).
- Boundary: simulation benchmarks; results author-reported.

### SimVerity: When Does Simulated Agent Success Survive Physical Deployment?

- Paper: https://arxiv.org/abs/2608.25067
- Classification: Evaluation/Safety (verdict transfer, sim-to-real assurance).
- Why included: systematically quantifies how much evidence a simulated pass
  provides about physical deployment — the evaluation question the field keeps
  assuming. SimVerity replays matched scenarios on target smart-home
  deployments and cross-validates agent execution against independently
  qualified physical witnesses, and shows deployment success is a process, not
  a static property: completion, reported state, observable effect, and settled
  outcome diverged within the same execution (an advanced simulator cleared all
  240 light trials while a camera caught 42 sub-second failures invisible to
  settled-state checks). A risk profile learned from measured trials and locked
  before evaluation predicted failures on a path it never physically measured,
  and a second qualified simulator added no independent cross-check — only
  physical measurement exposed shared blind spots. The framework turns verdict
  transfer into an explicit decision: clear, abstain, or escalate. An
  evaluation contract in the sim-to-real line (DeepInsight II, WorldSimProbe).
- Boundary: smart-home agents (physical deployment, not robot manipulation); no
  official artifacts located as of 2026-08-27. Surfaced by the [20260825]
  re-screen; not named in the 08-26 record.

### Edge Skillguard: Auto-Policy, not Auto-Skill — Compiled Agent Skills for the Physical World

- Paper: https://arxiv.org/abs/2608.25091
- Classification: Evaluation/Safety (skill-layer authority for physical-world
  agents); General Harness Methodology.
- Why included: names the missing safety layer in the self-evolving-skill line:
  a Skill describes how an agent should behave; a Policy decides which behavior
  is allowed to become an action, and today's skill formats leave the second to
  the model. The paper defines **Borrowed Authority** — a skill format gives
  the receiving agent no typed way to reject an inter-agent permission claim,
  so a malicious or misused skill can drive actuation by attaching one — the
  intersection of the two adjacent documented attacks (malicious skills
  compromising software; jailbroken LLM-controlled robots causing physical
  harm). Edge Skillguard is a typed authority layer living inside the skill
  artifact, with guards over world state and sensor evidence; on a live edge
  control-plane testbed the guards reject 60/60 borrowed-authority requests
  across five attack variants without blocking benign requests, holding at 5×
  scale and across hosts on a Tailscale mesh. Directly relevant to physical-
  world agent safety (same line as Bounded Agents, One Gate Is Not Enough,
  ClawSentry), with the physical-actuation angle most skill-safety work skips.
- Boundary: edge control-plane testbed evidence (not a robot); no official
  artifacts located as of 2026-08-27. Surfaced by the [20260825] re-screen;
  not named in the 08-26 record.

## Reviewed but not promoted (watch list)

- **V-Link** (`2608.25308`) — recovers visual representations during
  VL→A feature transfer in Action DiT VLAs (Spatial + Semantic Query tokens
  via asymmetric pathways); +1.9% LIBERO, +31.2% LIBERO-Plus, +18.8% RoboTwin
  2.0 over GR00T N1.6, +20/+24% on two real AGIBOT A3 Ultra humanoid tasks.
  Solid architecture paper; no artifacts located; watch.
- **R³: Training Robots to Reason in Natural Language via RL** (`2608.26053`)
  — mid-trains a VLM on expert reasoning traces then improves it with
  single-step rubric-based RL to produce test-time language guidance for
  low-level policies; gains on Language Table and simulated bimanual grocery
  packing. Simulation-only evidence; no artifacts; watch.
- **TacForcing** (`2608.25798`) — streaming action expert consuming
  execution-time tactile feedback (EATA attention), no separate reactive
  controller; 65% simulated UniVTAC, 69% real contact-rich tasks. Real-robot
  evidence but no artifacts; watch.
- **LM-X** (`2608.25757`) — explainable action modeling emitting return-to-go,
  event-to-go, and heteroscedastic action flow; 74.1% on 50 randomized-hard
  RoboTwin 2.0 tasks after 20,000+ hours of real-robot training. No artifacts;
  watch.
- **RA-VLA** (`2608.25585`) — retrieval-augmented VLA for training-free
  test-time adaptation (behavior-aligned retrieval + grounded execution);
  LIBERO and real UR5e gains. No artifacts; watch.
- **GaussVLA** (`2608.24959`) — Mamba-based VLA with Gaussian Spatial
  Tokenizer + Depth-Aware CoT; 93.5% LIBERO at 200M params. No artifacts;
  watch (surfaced by [20260825] re-screen).
- **4DGS-WAM** (`2608.25956`) — object-centric world action model on explicit
  4D Gaussian splatting (dynamic objects vs reusable static background);
  KITTI-MOT short-horizon prediction. Driving-domain evaluation, no artifacts;
  watch.
- **ConfAL-WM** (`2608.25572`) — confidence-guided active learning for
  post-training action-conditioned world models (EVAC-based confidence probe,
  task/frame/patch selection); RoboTwin2.0 efficiency gains. Project page live;
  no code; watch.
- **A Statistical Audit of Physical AI Benchmark Redundancy** (`2608.25940`) —
  51 models × 12 physical-AI benchmarks; two substitute pairs move 22 of 51
  models by ≥3 places when collapsed; a four-benchmark subset retains 78.5% of
  utility. Evaluation methodology; no artifacts; watch.
- **EASEL / Paint What You See** (`2608.25417`) — benchmark for dexterous
  visual tool use (reference-guided visual reconstruction) with EASEL-Data
  (440K samples) and EASEL-9B; 25 models struggle. Digital-agent benchmark;
  watch.
- **RLHEV / Agentic Game Development as a Verifiable Trajectory Data Engine**
  (`2608.25518`) — game-engine checks (collision, physics, navigability) as
  grounded rewards for scaling spatial world models, with implicit human
  acceptance. Data-engine position; no artifacts; watch.
- **Code World Model** (`2608.25927`) — coding agent as "world brain" keeping
  persistent executable state, proxy representation compiled to video for
  visual realization. Concept paper; no artifacts; watch.
- **VBVR-Pro** (`2608.26105`) — closed-loop testbed for native visual
  reasoning: 300 procedural tasks, verifiable reward scorers (vs VLM-judge
  failure modes), RL post-training. Visual-reasoning domain; no artifacts;
  watch.
- **KOPE / Beyond Scaling: Self-Evolving LLM Agents for Hardware Kernel
  Optimization** (`2608.25570`) — experience graph memory + active context
  injection for self-evolving optimization agents; 1.54× CANNBot under GLM-5.2.
  Digital; no artifacts; watch.
- **SymTrace / Repair or Resample?** (`2608.25920`) — controlled evaluation
  framework (intervention anchors, recorded replay) showing unguided rerun
  "repairs" are mostly stochastic resampling (repair rate 6.90%); SymFail
  dataset of 536 annotated failure trajectories. Digital MAS evaluation; watch.
- **EVOMAL: Self-Poisoning in Self-Evolving Coding Agents** (`2608.25776`) —
  malicious skills planted in shared libraries become templates for
  self-authored malicious skills (ASPR 20.3–41.8% across six models on
  SWE-bench Verified; self-propagating worm). Safety audit of the
  skill-evolution loop; no artifacts; watch.
- **Trace Integrity for LLM Data Agents** (`2608.26036`) — deployment
  reliability criterion (explicit, executable, schema-valid, replayable,
  auditable traces) with execution contracts and CAIT Rate. Digital; vision
  paper; watch.
- **When Stale Constraints Go Unchecked** (`2608.25553`) — budgeted
  verification of inherited agent memory under supersession; re-assigning one
  of two verification slots to the provenance path raises
  current-record-consistent decisions by +61–74 points. Memory-verification
  measurement; no artifacts; watch.
- **CaSKG** (`2608.25500`) — counterfactual-causal skill graphs calibrating
  procedural relations before retrieval; SOTA in all twelve model×benchmark
  combos (ALFWorld/ScienceWorld). Digital skill retrieval; no artifacts; watch.
- **HiPS / Hierarchical Strategy Co-Evolution for Agent Memory** (`2608.25329`)
  — shared universal memory strategy + persona-specific adaptive tier with
  cross-level rule flow. Digital memory management; no artifacts; watch.
- **Routed Graph Handoff** (`2608.25277`) — lightweight LLM router selects
  typed dependency graph vs natural language per delegation (+12.7 pp τ-retail
  at 3.2× compression). Digital delegation interface; no artifacts; watch.
- **AWM: Answerable Working Memory** (`2608.25618`) — memory-only
  answerability diagnostic + GRPO reward shaping for long-document VQA agents.
  Digital; no artifacts; watch.
- **VoiceMem** (`2608.26005`) — streaming dual-brain (informational +
  emotional) memory for speech agents; 134 ms retrieval. Speech domain;
  peripheral; watch.
- **PolyMemDB** (`2608.25577`) — polyglot storage + probabilistic provenance
  engine for agent memory. System demonstration; watch.
- **ProgRouter** (`2608.25992`) — online progress-guided LLM routing under
  quality–cost budgets. Digital orchestration; no artifacts; watch.
- **Trust-Aware Sequential Decision Making for Multi-Robot Systems**
  (`2608.25690`) — localization-spoofing-aware trust monitor + tiered matching
  for resilient multi-robot routing; real GPS spoofing data. Multi-robot
  security/planning; no artifacts; watch.
- **VISTA-Policy** (`2608.25872`) — visual deformation field of a compliant
  gripper as visuo-physical feedback for imitation learning; beats 3D Diffusion
  Policy and tactile baselines on cross-scale grasping/cap-unscrewing/
  calligraphy. No artifacts; watch.
- **Beyond Pairwise Feedback: Listwise VL Preference Reward Learning**
  (`2608.25350`) — Plackett-Luce reward models from VLM-generated rankings for
  robotic policies (Meta-World); first PL formulation in this line. Simulation
  only; no artifacts; watch.
- **Advantage-Driven Explicit Memory for Social Navigation** (`2608.25610`) —
  non-parametric memory indexing prior steps to critical (high-advantage)
  events in social navigation; sim-trained with real-data memory at test time.
  Navigation domain; no artifacts; watch.
- **TARCAT** (`2608.25395`) — occupation-grounded taxonomy of construction task
  activities (41 action primitives, 12 groups) with composition into reusable
  skills; demonstrated on a DOBOT CR3. Annotations repo
  (github.com/AICPS/TARCAT-Taxonomy) live, no license; watch.
- **VirTooS** (`2608.26066`) — ROS 2–Unity virtualization toolkit for AMR
  fleet management (mixed reality, ChoiRbot). Source "will be made publicly
  available"; watch.
- **RDR / Rollout-Decoded Reconstruction** (`2608.25017`) — free-running
  rollout decoding during latent-world-model training closes the
  train/deploy decoder gap (1.80× valid prediction time on Kuramoto-
  Sivashinsky). World-model training fix; no artifacts; watch (surfaced by
  [20260825] re-screen).
- **Game2World Engine** (`2608.24680`) — GameUI-Taxonomy + G2WEngine for
  gameplay-UI removal to unlock in-the-wild gameplay video for world-model
  training (96K synthetic pairs, 1,079 clips, 303 games). Data-engineering for
  world models; no artifacts located; watch (surfaced by [20260825] re-screen).
- **CRESSim-Neo** (`2608.25192`) — batched GPU surgical simulation engine
  (position-based rigid/deformable/fluid/strands, DLPack zero-copy); up to
  2.03M env steps/s on RTX 4090. Surgical sim; no artifacts located; watch
  (surfaced by [20260825] re-screen).
- **Longitudinal Robot LfD with Care Providers** (`2608.25196`) — multi-visit
  home-environment LfD usability study with care providers; dataset to be open
  sourced. Dataset release pending; watch (surfaced by [20260825] re-screen).
- **BFN-RL** (`2608.25163`) — Bayesian Flow Networks for offline trajectory
  planning (categorical + continuous). Offline-RL method; no artifacts; watch
  (surfaced by [20260825] re-screen).
- **SimVerity, Edge Skillguard, GaussVLA** — see included/watch above.
- Domain exclusions in the screened range (per policy): **Gating Before
  Commitment** (`2608.26074`, driving), **Choose Your Game Wisely**
  (`2608.25917`, driving), **SkyDrive** (`2608.25142`, driving), **Phantom
  Navigator** (`2608.26011`, UAV attack, not harness), **PRISM** (`2608.25666`,
  bimanual MPC — conventional control; distinct from watch PRISM `2608.17962`),
  **Fast Generative Grasping via MeanFlow** (`2608.26076`), **U-TAMP
  affordances** (`2608.25641`), **Low-Resolution Perception for Robotic
  Packing** (`2608.25874`), **BVR Sim** (`2608.25419`, air-combat RL, not
  robot), **VGI white paper** (`2608.25924`, vision-centered AGI position),
  **RAEM** (`2608.25366`), **EgoNav** (`2608.25642`), **SUPER ODOMETRY 2.0**
  (`2608.25427`), **AGRO-Nav** (`2608.25799`), **Anytime Global Tensor Motion
  Planning** (`2608.25830`), **Tendon-Driven Five-Fingered Hand** (`2608.25547`),
  **Generative Action-Chunk Sampling for pHRC stiffness** (`2608.25284`),
  **Pose-Anchored Optical Flow HRT** (`2608.25495`), **ROS2 Connect**
  (`2608.25102`), **CoDrift** (`2608.23939`, offline-RL method), plus the usual
  medical/CV/CL/LG and general-LLM papers without a reusable agent/VLA harness,
  recovery, safety, or evaluation contract (representative: **FinRiskAtlas**
  `2608.25325`, **MMJailBench** `2608.25490`, **RefVideo-6M** `2608.26101`,
  **Video-IFBench** `2608.25529`, **EgoArgus** `2608.25561`, **LongVU-TTT**
  `2608.25729`, **OmniPhys** `2608.25398`, **TraceML** `2608.26086`,
  **Planetary Prediction Engine** `2608.26088`, **VISA** `2608.26013`,
  **Plans You Can Check** `2608.25622`, **Localize-Then-Decide** `2608.25824`,
  **SwarmWorld** `2608.26081`, **Skill Issue** `2608.25832`, **A Self-Evolving
  Multi-Agent Jailbreak Defense** `2608.26008`, **Can your AI agent be
  cheaper?** `2608.25399`, **Simthesizer v2** `2608.24650`, **AgentWorld v2**
  `2608.24076`).

## Watch-list status (rechecks)

- **DELE-w0.5 / Inferring Action from Future Latent State** (`2608.22067v3`,
  updated 2026-08-26T12:45) — v3 probed via abs page; no artifact links added;
  unchanged (watch).
- **CAT: Trajectory-Level Continuous Action Representation** (`2608.24111v2`,
  updated 2026-08-26T04:22) — v2 probed via abs page; no artifact links added;
  unchanged (watch).
- **Simthesizer** (`2608.24650v2`), **AgentWorld** (`2608.24076v2`), **IAPO**
  (`2608.24588v2`), **JEPA-x** (`2608.24044v2`), **The Invisible Editorial
  Layer** (`2608.24662v2`), **Diverse by Reasoning** (`2608.24001v2`),
  **Matched Excess-Outranker** (`2608.24273v2`) — excluded-domain papers with
  new versions; re-checked, no qualifying change.
- **XPolicyLab** (`2608.09892`) — no new revision in today's listings;
  unchanged (in README with live repository).
- **Task-CoEvolve** (`2608.20169`), **ForeTime-VLA** (`2608.20735`), **SPADE**
  (`2608.19197`), **ScienceFlow** (`2608.14354`), **GhostTac** (`2608.20817`),
  **GigaBrain-WBC-0.5** (`2608.18234`), **Q-Planning** (`2608.21204`),
  **CounterAlign** (`2608.21740`), **INDI** (`2608.23478`), **BATON**
  (`2608.16889`), **Agent Lightning** (`2608.17528`), **VLCP** (`2608.16978`),
  **MANIGUARD** (`2608.17386`), **LEGO-RL** (`2608.17393`), **TOWN-VLA**
  (`2608.23224`), **AutoSaddler** (`2608.23041`) — none appear in today's
  replacement listings; unchanged.
- All other previously listed watch items (UniMem, M3, LD4WAM, SCVC,
  InstructMove, GuardianBench, SkillBloat, WAM-OPD, MCP-grounded robot
  programs, HVTB, ClawProBench, AUDITA, HANSARD, AIREP, MemGuard, The
  Compaction Cliff, Scroll, There Is No Neutral Harness, WebDev-Skills-Bench,
  Repo2Skill-Evo, Coalition-Aware Skill Reliability, Where World Models Break,
  ParallelWorld, ASP, MCP-Universe RL, GOLEM, Reward-Free Continual
  Adaptation, Lifelong Recomposition, CIDER, Noise Floor Audit, Sycophancy
  Amplification, RAI, TRACE skill bank, Safety Hacking BoN, Macro-Action
  Topological Navigation, BeTaL-GBI, WorldToken, PhyFilter, SR-WM, TONAV,
  Triplet2Track, WorldMind, Mamba SmolVLA expert, ReWorld, MOSH-WM, EchoWM,
  From Generation to Simulation, Capability Separation WAM, Betting
  Certificates, OptiSight, EndoNav, RACO, Geo-VLA, StageWAM, ReflexVLA,
  DreamX-Phi, UniTexture, PRISM, GigaBrain-0.7, ForceU-VLA, Hydra-0, ADEPT,
  LIBERO-VIFO, Koala Gripper, VisTa3D, CertVLA, PhysCaP, GraphOp-WM, Logic-VLA,
  CIVA, COTA, AID-Guard, TraceGrant, Artic, AgenticRAG-FP, Criterion Revision,
  Weighted Memory Tree, Utility Under Attack, Claws in Plain Sight, CRATE,
  Trustworthy RAG, Judgment Receipts, AsmEvo, VT-MUSE, ViTacPhys, AUSO, TaPeR,
  TOSS, Action-JND, EXIMO, HiTac-WAM, OrthoSkillVLA, SafeBranch, SCAPE, EAFG,
  Panda ROS 2 stack, Outcome Monitors, StateMemBench, Adaptive Probabilistic
  Shielding, Orthogonal JEPA, DBOSC, Temporal-Logic TAMP, Credit Without
  Ground Truth, AI4AI-Bench, Optimal Skill Selection, Self-Demonstrated VLA
  fine-tuning, ExPhy, The Missing Touch, Neural Reduced Dynamics, Brain
  Researcher, Thinkingbox, Meta^n, VideoHarness-RSI, StarHarness, The Empire
  Long Divided, The Handoff Tax, Adaptive Influence Graphs, RePolicy, When
  "Must" Becomes "Maybe", More Rejective Not More Discriminative, Bayesian
  Self-Escalation, AHEAD, SPO++, SkillForge, CAFE, Attnlocate, OODA-Tool,
  Paritok-4B, PeakBench, Who is the Agent to Blame, BrowserForge, LAION-BVD,
  HSR, MiGA/GVLA, WorldEcho/WorldSync, LAWA, GaussianWAM, CAT, LeFlow,
  NeoWorld-Pro, From Seeing to Acting, DELE-w0.5) — no new revisions or
  artifact releases detected in today's replacement listings; unchanged.

## Operational notes

- The arXiv API was healthy for the entire run (no 429/500); both the
  `submittedDate` search and `id_list` paths worked. Listing pages were used
  only to enumerate replacement submissions.
- Artifact checks on 2026-08-27: JIT-Agent project page (live), UCAG-P project
  page (live) + repo (placeholder, figures only, no license), Zero-WAM repo
  (Apache-2.0, ~69 MB, pushed same day) + project page, StreamPI repo
  (Apache-2.0, openpi-based, pushed same day) + project page, MA-VLA repo
  (zhangzaibin/future-robots, license declared, ECCV 2026), GaussianDream repo
  (Apache-2.0, live since 07-10) + series project page, ConfAL-WM page (live),
  TARCAT annotations repo (live, no license), DELE-w0.5 v3 / CAT v2 abs pages
  (no artifact links). SimVerity, Edge Skillguard, V-Link, R³, TacForcing,
  LM-X, RA-VLA, GaussVLA, 4DGS-WAM, OpsHarness: no official artifacts located.
- The known OpenSSH `/etc/ssh/ssh_config.d` ownership error was handled per
  procedure with `GIT_SSH_COMMAND='ssh -F /dev/null'` (fetch succeeded;
  `ls-remote` without the env var still fails with the same error).
- Working tree before the run: `docs/reference-architecture.md` modified,
  `docs/ring-harness.png` and `handoff.md` untracked — preserved untouched.
