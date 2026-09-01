# Daily arXiv scan — 2026-09-01

## Scope

- Interval: since the 2026-08-31 run's cutoff (~2026-08-31 10:15 UTC) through
  2026-09-01 ~04:10 UTC. No missed dates (the 08-31 run executed); 09-01 is
  today. Missed-date logic therefore covers the remainder of 08-31 plus 09-01
  and — because of arXiv announce lag — the batches newly exposed since the
  last run.
- The arXiv export API (`https://export.arxiv.org/api/query`, HTTPS) was
  healthy but heavily rate-limited: persistent `429 Rate exceeded` on first
  attempts (one `503`) throughout the run, resolved by 30 s+ backoff between
  retries. To minimize request count, the interval was queried once per
  category (`cat:cs.RO OR …` semantics via per-category requests over
  `submittedDate:[20260828000000 TO 20260901235959]`), plus a separate
  `[20260827]` pass for the growth/revision recheck. XML saved under the
  workspace scratch dir (`.scratch/arxiv-2026-09-01/`, removed after the run).
- Batch status vs the 08-31 run:
  - `[20260831]` (announced 2026-09-01 00:00 UTC) is **exposed for the first
    time** — 430 unique entries across cs.RO/cs.AI/cs.CL/cs.CV/cs.LG, all v1
    with `published == updated` (0 revisions). Fully screened here.
  - `[20260830]` **exposed for the first time** — 234 unique, all v1 (0
    revisions). Fully screened here.
  - `[20260829]` **exposed for the first time** — 235 unique, all v1 (0
    revisions). Fully screened here.
  - `[20260828]` **grew** from 285 unique (08-31 run) to **364 unique** (+79
    late additions); the not-previously-screened subset was screened. 11
    entries carry `updated != published`, all minor same-day v2 updates dated
    2026-08-31: five predate the 08-31 run's cutoff (Logos `2608.28553`
    05:59, GeoFF3D `2608.28288`, WeAgent-MMSearch `2608.28062`, Not to Break
    but to Attest `2608.27954`, Rubric-to-Code `2608.27906`); the post-cutoff
    ones (repurchase `2608.28393`, GraspHOI `2608.28386`, Prove2Me
    `2608.28433`, RealSWE `2608.27831`, Ladders in Chaos `2608.28496`) are
    domain-excluded or already watched — **no curation-status change**.
  - `[20260827]` **grew** from 430 unique (08-31 run) to **446 unique** (+16,
    mainly the late `2608.28692`–`2608.28709` block: CV/perception items,
    RoboGesture `2608.28693`, SNF-Bench `2608.28694`, pipe routing
    `2608.28697`, ORDDAR `2608.28704`, …). 34 entries now carry
    `updated != published` (vs 6 at the 08-31 run): 28 additional minor
    same-day v2 updates dated 2026-08-28–31 (RATIO `2608.27394`, LiveSim
    `2608.26849`, MVC-Bench `2608.27004`, Beyond Parallel Blindness
    `2608.27339`, LongGuard `2608.27580`, SOLO `2608.26583`, TwinKV
    `2608.27128`, …) — none curated (LongGuard stays on the watch list; SOLO
    remains domain-excluded), **no curation-status change**.
  - `[20260901]` still returns 0 entries (announce lag). Next run should
    re-check `[20260901]` and growth in `[20260831]`/`[20260830]`/
    `[20260829]`.
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action models,
  code-as-policy, skill discovery, memory, recovery, evaluation, runtime
  monitoring, and safety. Candidate claims were checked against the arXiv
  record, official project pages, and official code/model/data repositories
  (live HTTP, GitHub `ls-remote`/API, and Hugging Face API checks on
  2026-09-01); README, landscape, and source records were checked for
  duplicates (no name/ID collisions). All performance results below remain
  author-reported unless stated otherwise.

## Included (new curated entries)

### Hydra: A Navigation World Action Model with Discrete Latent Planning and Continuous Flow-Matching Execution

- Paper: https://arxiv.org/abs/2608.28995
- Project page: https://robotixx.github.io/hydra (live 2026-09-01)
- Weights: https://huggingface.co/mhnazeri/Hydra (live 2026-09-01, MIT,
  ships `hydra.zip` + `vertiformer.zip`; the project page's linked code
  repository `github.com/mhnazeri/Hydra` returns 404 — treated literally, code
  is not open as of 2026-09-01)
- Classification: Robot Foundation/World Model (navigation world-action model
  with in-model discrete planning).
- Why included: attacks the representation misalignment that keeps world-model
  planning off real-time control — the planner and the generative model live on
  decoupled manifolds, so candidate evaluation requires decoding back to pixel
  space. Hydra closes the gap by moving both the sampler and the evaluator
  inside the model: a unified latent manifold over visual states, physical
  poses, and control actions is compressed through modality-specific
  vector-quantized bottlenecks into discrete vocabularies of kinodynamic
  intents and visual states; candidates are drawn from the shared manifold and
  ranked by a Kinematic-Perceptual Cost without ever decoding to pixels
  (Discrete Latent Planning), then a conditional flow-matching head maps the
  selected intent to a continuous executable trajectory. The authors report
  that on two physical robotic platforms Hydra outperforms state-of-the-art
  world models in goal-directed planning while matching or exceeding the
  closed-loop execution of leading reactive foundation policies. A concrete
  real-hardware statement of the in-model-planning WAM thesis (the latent
  planning line of LaWAM/PAWBench/Ω-0).
- Boundary: navigation domain; results author-reported; weights live on HF but
  the linked code repository is a 404, and the HF model has not been
  downloaded as of 2026-09-01.

### AGM: Achievement-Grounded Memory for Closed-Loop Agents with Frozen VLA Policies

- Paper: https://arxiv.org/abs/2608.29537
- Classification: Agentic Robot/VLA Harness (verification-gated progress
  memory for frozen policies).
- Why included: names the failure mode of external memory for open-loop VLA
  execution — treating attempted actions as completed progress turns local
  execution errors into persistent task-state errors — and repairs it by
  construction: a task is represented as a subgoal sequence with a progress
  pointer that advances **only after physical evidence verifies the current
  subgoal**. Proprioceptive interaction cues decide *when* to verify; coherent
  point tracking and language-conditioned cross-view comparison, sourced from
  frozen foundation models through a single 2.43M-parameter verification head,
  decide *what* was achieved — no test-time large-model inference, policy
  frozen. The authors report top results on the RoboMME Counting benchmark
  (PickXTimes and BinFill, surpassing the strongest memory-augmented baseline)
  and equally decisive gains on a physical robot; the conclusion — reliable
  embodied memory depends more on disciplined state updates than on memory
  capacity — is a design contract for the frozen-policy harness line
  (CheckVLA, Zetta, TOWN-VLA, PonderPounce).
- Boundary: no official artifacts located as of 2026-09-01 (GitHub/web
  search); results author-reported; RoboMME Counting numbers are absent from
  the abstract (blank in the arXiv record), so only qualitative results are
  stated here.

### Scaffolding Foundation Models into Physical-World Agents Pushes the Frontier of Long-Horizon Navigation (NavMCP)

- Paper: https://arxiv.org/abs/2608.30396
- Classification: Agentic Robot/VLA Harness (VLM-agent × navigation-foundation-
  model scaffolding).
- Why included: a clean split of the long-horizon navigation problem across
  two frozen model classes — a VLM reasoning agent decides what evidence to
  seek, where to search, and when to stop, while a navigation foundation model
  grounds each semantic sub-goal into closed-loop navigation. Three named
  channels structure the collaboration (intent: evidence needs → navigation
  calls; observation: rollouts → source-grounded trajectory evidence; memory:
  findings, negative evidence, and unresolved goals across calls), turning
  isolated navigation rollouts into persistent embodied interaction without
  retraining either model. The authors report state-of-the-art results on
  HM-EQA, MT-HM3D, and EXPRESS-Bench, outperform an episodic interface by 14.9
  points on HM-EQA under matched backbones, and reach 78.3% success on a
  physical Unitree Go2 with the margin over the strongest baseline growing
  from 10 to 45 points as the task horizon increases. The agentic-scaffold
  line for embodied agents (Embodied Agents Take Control, Harness VLA, GaP).
- Boundary: navigation domain; results author-reported; no official artifacts
  located as of 2026-09-01.

### Motus2: A Self-Evolving General World Model for Dexterous Manipulation

- Paper: https://arxiv.org/abs/2608.30237
- Classification: Robot Foundation/World Model (closed decision-and-learning
  loop world-action model).
- Why included: the strongest statement yet of the *self-evolving* WAM thesis
  — instead of appending an action head to a world simulator, one checkpoint
  with shared weights exposes three control interfaces (a policy / world-action
  model, a simulator / action-conditioned world model, and an evaluator / value
  model) whose coupling forms a closed decision-and-learning loop: the policy
  proposes candidate action chunks, the simulator predicts their visual
  consequences, and the evaluator assesses the predicted outcomes, while
  curated expert demonstrations drive action learning and failed/suboptimal
  interactions provide evidence for dynamics and value learning. Data scaling
  progresses from large-scale monocular egocentric data to synchronized stereo
  egocentric data and robot-domain adaptation; the model is instantiated on a
  fully biomimetic platform (stereo vision, dual arms, dual dexterous hands,
  tactile sensing) with sliding-window, global-autoregressive, and
  hybrid-memory context variants. The follow-up to Motus (2512.13030) from the
  same team (Zhu Jun et al.).
- Boundary: no Motus2-specific artifacts located as of 2026-09-01 (the Motus
  v1 line's official repository `thu-ml/Motus` and the `motus-robotics`
  Hugging Face org are live, but no Motus2 release was found); results
  author-reported.

### PRACTICE: From Experience to Expertise in Self-Evolving Embodied Agents

- Paper: https://arxiv.org/abs/2608.30760
- Project page: https://baai-agents.github.io/PRACTICE (live 2026-09-01; the
  page's linked repository `github.com/BAAI-Agents/PRACTICE` returns 404 —
  treated literally, code is not open as of 2026-09-01)
- Classification: Agentic Robot/VLA Harness (trained skill learner over frozen
  embodied executors).
- Why included: moves skill evolution for embodied agents from hand-written
  prompting workflows to a *trained* skill learner. Given accumulated skills
  and incoming trajectories, the learner produces structured batch-edits
  (add/refine/merge/remove) that are hierarchically consolidated into a
  consistent persistent skill library while the task executor stays frozen; a
  two-stage curriculum first teaches basic skill generation and library
  maintenance from oracle trajectories, then contrasts successful and failed
  trajectories from heterogeneous executors to learn invalid-action patterns
  and recovery strategies, with online skill-edit distillation aligning the
  learner with a stronger teacher. The authors report consistent performance
  improvements across successive library-update rounds for multiple frozen
  executors and gains over the strongest experience-based baselines on
  EB-ALFRED and EB-Habitat. The self-evolving-skill line for embodied agents
  (HERO, ASPIRE, Zetta, Evo-Harness).
- Boundary: simulation benchmarks (EB-ALFRED/EB-Habitat); results
  author-reported; linked repository is a 404 as of 2026-09-01.

### Safe to Resume? Breaking Execution Continuity of Agent Execution via Rollback

- Paper: https://arxiv.org/abs/2608.29381
- Classification: Evaluation/Safety (checkpoint/rollback security).
- Why included: the first systematic security study of checkpoint-and-rollback
  (C/R) in agent systems, extending the durability/rollback line (Agentic
  Transaction, Aborted but Not Forgotten) to the recovery path: correct
  rollback does not imply secure recovery, because a faithfully restored
  checkpoint can resume an execution whose states, assumptions, and external
  effects never coexisted in any valid history. The paper characterizes the
  C/R design space across representative agent frameworks, develops a general
  execution model capturing recovery boundaries and state dependencies, and
  identifies five fundamental failure modes (incomplete/inconsistent internal
  state, stale external dependencies, nondeterministic replay, unrecorded
  external effects), demonstrated through three end-to-end attacks on Hermes,
  Cline, and LangGraph (malware-verification bypass, unauthorized mail
  forwarding, double payment) and reproduced across five frameworks via a
  multi-agent analysis pipeline that reconstructs execution semantics,
  identifies violations, and validates them through actual rollback.
- Boundary: digital agents; no robot experiment; no official artifacts located
  as of 2026-09-01; results author-reported.

### CorrectVLA: Training-Free Action Correction for VLA Model Failures via Language Feedback

- Paper: https://arxiv.org/abs/2608.29967
- Project page: https://correctvla.github.io (live 2026-09-01)
- Code: https://github.com/owenk3/correct_vla (live 2026-09-01, HEAD
  `34e29a3`)
- Classification: Agentic Robot/VLA Harness (training-free corrective
  interface for frozen VLA policies).
- Why included: a minimal, deployable correction contract for frozen VLA
  policies — a human gives a *single task-level* language correction, which is
  translated into additive action-magnitude adjustments applied uniformly
  across rollouts without per-episode intervention or weight updates. The
  paper contributes a failure-mode taxonomy on LIBERO-90 separating execution
  misalignment (correct target, miscalibrated action magnitude — the
  correctable subset) from semantic-comprehension failures, and shows on a
  UFactory xArm7 under environment shift that CorrectVLA restores near-perfect
  success where the base policy almost entirely breaks down, generalizing
  across object locations and identities. A real artifact release (live code
  repository) in the frozen-policy recovery line (ContactGuard, TOWN-VLA,
  Q-Planning, FLARE).
- Boundary: results author-reported; evidence spans LIBERO-90 simulation and
  real-robot xArm7 trials; the repository's contents beyond the HEAD commit
  were not audited in detail.

## Reviewed but not promoted (watch list)

From `[20260829]`:

- **AnyWorld** (`2608.29242`) — cross-embodiment world modeling that
  factorizes a single human interaction into action/camera/embodiment factors
  and recomposes them into robot-native rollouts without paired human–robot
  demonstrations (large-scale human-video pretraining + mixed-embodiment
  fine-tuning); the project page (`xpeng-robotics.github.io/anyworld`)
  returned 404 on 2026-09-01; watch for a live page/artifacts.
- **Safe-to-Resume's sibling study set** — see Included.
- **BLA: Brain-Language-Action** (`2608.28967`) — language-conditioned EEG
  decoding for robotic action generation (drone proof-of-concept on BCI
  Competition IV 2a); BCI/assistive domain without a reusable robot harness
  contract; watch.
- **Teaching Robot Policies to Humans Using Erroneous Examples** (`2608.29023`)
  — HRI study using robot failures to teach human operators; watch.
- **DREAM: Deployment-Time Demonstration Generation via Real-to-Sim**
  (`2608.29078`) — generates fine-tuning data for a pretrained VLA from a
  captured workspace + language instruction via TAMP and real-to-sim
  augmentation, verified by generated success criteria; real-robot
  experiments; watch for artifact release.
- **Selective Forgetting** (`2608.28978`) — graph-based memory framework for
  long-term LLM agents (forgetting as a first-class memory operation);
  digital; watch.
- **CGFM-Nav** (`2608.29114`) — cognitive graph-field memory for lifelong
  multimodal navigation (scene graph + semantic-frontier field; GOAT-Bench
  preliminaries); watch.
- **GCJR / Localizing Emergent Failures** (`2608.29228`, conference paper) —
  Minimal Repair Family Recovery: recovers all inclusion-minimal event sets
  whose counterfactual replay restores task success (1.000 Family Exact Match
  on 90 in-scope DAG cases, 55.1% fewer replays); failure-attribution line;
  watch.
- **AGM's benchmark, SMILE** (`2608.29432`, RA-L submission) — B-spline
  coefficient action representation for longer VLA horizons (SMILE-Evo1 98.0%
  LIBERO, xArm real-world); VLA method without external harness contract;
  watch.
- **Does Latent Planning Survive Point Clouds?** (`2608.29434`) — lifts three
  JEPA world-model designs to point clouds; latent planning survives heavy
  dropout; world-model robustness measurement; watch.
- **APIFlow-Bench** (`2608.29128`) — measures whether agents survive long,
  dependent API workflows; watch.
- **Agri-Sim** (`2608.29100`) — agricultural simulation platform for embodied
  intelligence evaluation in greenhouse robotics; domain-specific but an
  evaluation contract; watch.
- **CanonNav** (`2608.30242`, 08-31) — disentangles navigation behavior from
  camera geometry in cross-platform visual navigation; watch.

From `[20260830]`:

- **Towards a Systems Foundation for Agentic Skills** (`2608.29596`) —
  reference architecture for the agentic-skills ecosystem: skills formalized
  as externalized procedural knowledge across a nine-stage lifecycle
  (discovery, authoring, storage, retrieval/routing, composition, execution
  and repair, lifelong adaptation, evaluation, security governance) with
  marketplace dynamics and runtime verification/defense; a position/systems
  paper without experiments; watch (the field's skill-systems counterpart to
  the Physical-AI harness position paper).
- **DUOTRACE / Detect Before You Attribute** (`2608.29646`) — plug-and-play
  detection filter for LLM-based failure attribution (VAE anomaly detection
  over dual-view semantic-structural trajectories, then focused evidence for
  LLM attributors); watch.
- **Last Step Matters** (`2608.29685`, EMNLP 2026) — early uncertainty cannot
  predict failure in long-horizon agents; measurement; watch.
- **EMERGE-Policy** (`2608.29896`) — graph-structured agentic framework with
  role-specific sub-agents (perception, execution monitoring, verification,
  memory), criterion-grounded verification, and Branch Stack recovery;
  no artifacts; watch.
- **AcrossWAM1.0** (`2608.29937`) — modularization/scaling study of the latent
  world-action stack (policy adapter, retained latent world decoder, flow
  matching expert; 97.45% LIBERO with a Qwen3.5-0.8B backbone, 42.4% fewer
  deployment parameters); note: distinct from the 08-31 watch item
  AcrossVAM1.0 (`2608.28491`); VLA/WAM method without external harness
  contract; watch.
- **CorrectVLA** — see Included (promoted).
- **Zeva** (`2608.30880`, 08-31) — in-context causal learning from the robot's
  own physical interactions (causal interaction extractor, dual-timescale
  causal memory, frozen policy; success improves with accumulated experience
  in simulation and real world); self-evolution line; watch.
- **SkillGuard / Reachability-Based Capability Confinement** (`2608.30041`) —
  harness-level enforcement layer treating untrusted data entering agent state
  as contamination; Skill Impact Graph + steerability signatures + inline
  reference monitor; AgentDojo results; no artifacts; watch.
- **How do World Models and Policies Compose in LLM Agents?** (`2608.30067`,
  EMNLP 2026) — spectral/behavioral account of world-model + policy additive
  updates; watch.
- **The Intervention Gap in Latent World Models** (`2608.29998`) — planning-
  time intervention fidelity as a distinct measurable property (TD-MPC2
  checkpoints); watch.
- **SearchWiki** (`2608.29953`) — harness framework synthesizing a corpus into
  a hierarchical typed wiki with a trained retrieval agent (WikiResearcher-9B);
  digital RAG harness; watch.
- **GATE** (`2608.29395`) — training-free test-time adaptation of VLMs;
  generic TTA, excluded from the agent line; watch (no).
- **Agents in the Large / Pera** (`2608.30478`, 08-31) — 41-page position
  paper proposing a Perception-Centered Architecture for persistent agents;
  watch.

From `[20260831]`:

- **Behavior-Skill** (`2608.30536`) — fine-grained VLA benchmark over 235,492
  executable skill instances from 10,000 demonstrations across 50 household
  tasks and 34 skill categories, with restorable intermediate states, skill
  success conditions, and trajectory-/skill-level metrics (evaluated on
  pi0.5, GR00T); the abstract's claimed repository
  (`github.com/nubot-nudt/Behavior-Skill`) returned 404 on 2026-09-01 —
  treated literally, no benchmark release yet; watch.
- **RoboPhys-3D** (`2608.28718`, 08-28 growth) — 3D-grounded embodied
  world-model evaluation benchmark on RoboTwin 2.0 (50 tasks, 5,000 episodes,
  25,000 multi-view videos, 50 metrics/18 sub-dimensions, same-pipeline
  reconstruction to separate generation vs reconstruction error; Cosmos 3 best
  RoboPhyscore 0.6330 with r=0.976 agreement with human evaluation); no
  artifacts located; watch.
- **S3Gym** (`2608.31100`) — interactive benchmark for LLM self-improvement
  via self-testing/self-judging/self-improvement over seven text-based games
  with executable verifiers; self-evolution evaluation line; watch.
- **ASPIRE** (`2608.31111`) — vague-goal-driven self-evolution benchmark that
  supports both model-weight and agent-harness evolution with hidden
  evaluation (project page live at self-developing-agents.github.io); watch.
- **BAITBENCH** (`2608.30724`) — measures agent reward hacking with optional
  shortcuts planted in three synthetic tabular ML tasks (57.1% of runs across
  seven frontier agents); reward-hacking evaluation; watch.
- **EvoSkill Injection / SARGE** (`2608.30429`, EMNLP 2026) — red-teaming of
  autonomous skill generation/evolution pipelines in self-evolving agents;
  skill-security line; watch.
- **SIR: Self-improving Red-teaming for Compute Use Agents** (`2608.30207`) —
  black-box adaptive IPI attack composing reusable injection principles with a
  failure-diagnosis feedback loop, scored by a deterministic oracle; watch.
- **SkillZip Pro** (`2608.30785`) — evaluation-free compression of
  progressively loaded skill bundles for self-evolving agents; watch.
- **WebWorld** (`2608.30530`, EMNLP 2026) — browser-as-world-model interface
  for self-improving web code: typed interaction contracts and acceptance
  certificates as a quality ratchet for SFT export (WebWorld-27B +5.3
  HTMLBench-400, +14.9 MiniAppBench-Val); verification-loop line, web-code
  domain; watch.
- **MNIST-PRO** (`2608.31022`) — benchmark isolating agentic perception under
  partial observability (glimpse-based search with lookback); identifies
  perceptual-state construction, premature stopping, and belief-revision
  bottlenecks; watch.
- **Can Video World Models Track Unobserved World States?** (`2608.30692`,
  project page `joonghyuk.com/stateful-vwm-web` returned 404 on 2026-09-01) —
  action-conditioned video "Shell Game" showing pixel-faithful models fail to
  carry hidden state beyond training horizons; world-model evaluation
  measurement; watch.
- **SwarmBench** (`2608.30661`, EMNLP 2026 Findings) — benchmark for LLMs as
  agent-swarm orchestrators (accuracy, efficiency, cost, process quality) plus
  a SwarmExp experience-replay booster; watch.
- **Measure Before You Manage** (`2608.31057`) — semantic heterogeneity of
  agent working memory across 55 coding-agent trajectories; calibration gains
  may not transfer; memory evaluation; watch.
- **PaperGym** (`2608.31119`) — turns each research paper into a complete RL
  training environment via rubric induction (question from goal/background,
  criteria from methods/results to prevent paraphrase rewards); watch.
- **AutoSciRub** (`2608.31076`, work in progress) — evaluation-first rubric
  induction for autonomous research agents; watch.
- **Science sandboxes** (`2608.30165`, q-bio) — repeated experimentation/
  feedback/hypothesis-revision protocol for measuring agent scientific
  capability in regulatory-genomics and protein-fitness settings; biological
  domain; watch.
- **Temporal Forcing** (`2608.30643`) — 4D representation alignment for VLAs
  (history pathway + pretrained 4D foundation model); VLA method; watch.
- **SMILE/AdaVLA/DriftingVLA** (`2608.29432`/`2608.29208` IROS 2026/
  `2608.29749`) — VLA inference-efficiency methods (smooth action
  representation; training-free flow-matching acceleration; one-step
  distribution drifting); watch.
- **CometVLA** (`2608.30289`) — co-training on an embodied data pyramid for
  physical understanding; VLA method; watch.
- **LightNav-0** (`2608.30935`, technical report) — compact generalist
  navigation model eliciting VLM spatial intelligence via dual-channel
  pointing + residual VQ action tokens; watch.
- **Autonomously Acquiring Robot Manipulation Skills with Language-Driven
  Quality-Diversity** (`2608.30983`) — LLM-based functional-space exploration
  to output diverse motion-primitive archives from a free-form task
  description; skill discovery; watch.
- **SUN / Kuafu** (`2608.31167`) — typed executables where geometric/contact
  relations compile into MPC costs, RL rewards, satisfaction predicates, and
  transition guards, with automatic synthesis from language/scene semantics
  (82.03% macro-success across nine tasks); control↔learning bridge; watch.
- **PAVE** (`2608.30378`) — direct world-action policy combining JEPA
  predictive learning with trajectory-relative multi-horizon transition
  alignment and value-guided evolution; watch.
- **SmoothRL** (`2608.29768`) — online RL fine-tuning of pretrained policies
  during asynchronous chunked execution; watch.
- **Stateful video WMs / Intervention Gap / WM-Policy composition** — see
  above.
- **GENIA-style guardrail items** — **SafeAtlas-VL** (`2608.29098`),
  **When Safety Speaks a Language** (`2608.29936`), **Influence Is Not
  Authority** (`2608.29942`), **Lazy Grounding** (`2608.30303`), **Will the
  User Ever Know** (`2608.30362`), **Beyond the Payload** (`2608.30686`),
  **Selective Disclosure** (`2608.29070`), **Fragility of Jailbreak
  Robustness** (`2608.30748`), **CPR for LLMs** (`2608.30158`), **Hidden
  Directives** (`2608.29070`) — safety/security measurement or attack items
  without a reusable harness contract; watch for artifact releases or a
  clearer harness framing.
- **ORDDAR** (`2608.28704`, 08-27 growth) — observation-driven reasoning
  that detects localized distortions, retrieves related prior reasoning, and
  repairs only affected cognitive states (local-recovery line); watch.
- **SNF-Bench** (`2608.28694`, 08-27 growth) — separates static drift from
  natural flow in long-horizon fixed-camera video generation evaluation;
  video-WM fidelity evaluation; watch.
- **RoboGesture** (`2608.28693`, 08-27 growth) — real-time semantic-aligned
  co-speech gesture generation for humanoid interaction; HRI domain; watch.

## Watch-list status (rechecks)

- No new artifact releases or revisions for previously listed watch items:
  StageWAM, ReflexVLA/ReflexBench, DreamX-Phi, UniTexture, PRISM,
  GigaBrain-0.7/WBC, ForceU-VLA, LIBERO-VIFO, Agent Lightning, VLCP, Hydra-0,
  BATON, Q-Planning, AutoSaddler, JIT-Agent, Task-CoEvolve code, UCAG-P,
  TrapVLA code/benchmarks, TemporalFlow-VLA, PredVLA, FlashVLA, WikiSkill,
  RedEvoAgent, SKILL.state, Agent Mesh, WALL-SS, R2M-Bench, INTENT-AS-A-TOOL,
  BTS-AgentBench, TraceBench, GraphMemix, UrbanGround, Stale Constraints v2,
  LM-X v2, Zero-WAM v2, VLAct, Code as Worlds, Aero Hand Open, CAITLYN,
  Dogwood — unchanged (spot-checked 2026-09-01: project pages/repos for
  Q-Planning, Task-CoEvolve, TrapVLA, JIT-Agent, UCAG-P, VLAct, Code as
  Worlds, Aero Hand Open, CAITLYN, Dogwood, Hydra-0, ReflexVLA, ManiGuard,
  HELIX, LEGO-RL all live; Task-CoEvolve still README+figs only; Q-Planning
  still shows no code link; TrapVLA still no code).
- New watch items with artifacts now live (verified 2026-09-01): CorrectVLA
  (repo `owenk3/correct_vla`), Hydra (HF weights `mhnazeri/Hydra`, MIT — but
  linked GitHub 404), PRACTICE (project page live — linked GitHub 404).
- The `[20260826]` slice was not re-queried this run (no growth indication;
  its 20 revisions all predated the 08-30 run's cutoff and are unchanged per
  the 08-31 record).

## Domain exclusions in the screened ranges (per policy)

- Driving/CAV: Rethinking Language's Role in Efficient VLA for Autonomous
  Vehicles (`2608.30144`), VLA Driving multi-trajectory alignment
  (`2608.30122`), camera-only end-to-end driving degradation benchmark
  (`2608.29005`), Self-Aware Active Learning in driving (`2608.29772`),
  semantic-aware memory interface for autonomous vehicles (`2608.29000`),
  Driving on Memory (`2608.31029`), adversarial calibration attack on AVs
  (`2608.28778`), Self-Play Driving (`2608.30819`).
- Conventional control/locomotion/perception: SOLO (`2608.26583`), SleepWalking
  SWAQ (`2608.30883`), Blind Dexterity (`2608.29487`), agile perceptive
  traversal (`2608.29769`), 4WIS trajectory planning (`2608.29108`), LARC
  reachability certification (`2608.29767`), Geometric Attractor Monitoring
  (`2608.30804`), GraspHOI (`2608.28386`), GeoFF3D (`2608.28288`), RoboPhys-3D
  benchmark (watch), DARP dataset (`2608.31002`), SpectraTac (`2608.30368`),
  tactile anomaly detection (`2608.30506`), AGRICAM (`2608.29237`), GAFT
  (`2608.30858`), HorizonNet (`2608.30471`), ultrasound sim-to-real
  (`2608.29516`), soft-robot whole-arm inference (`2608.30773`), endovascular
  microrobots (`2608.30220`), MiBOT massage robot (`2608.30055`), sliding
  palpation (`2608.29396`), humanoid ankles actuator (`2608.30832`), CIG-RL
  source-term estimation (`2608.30673`), Shared Autonomy AUV (`2608.29347`),
  strain-energy lightweight design (`2608.29146`), soft modular robots
  (`2608.29547`), serial/dual-arm LQ games and multi-robot placement
  (`2608.29388`), cooperative risk-aware exploration (`2608.28409`), pipe
  routing (`2608.28697`).
- Medical/clinical: FedEHR-Agents (`2608.27856`), MedCache (`2608.29528`),
  GPAgentBench-2K (`2608.30188`), SIC-Agents (`2608.29481`), DIASENTINEL
  (`2608.31128`), MedAgent-R1 (`2608.30676`), Source-Dependent Deference
  (`2608.29800`), EVAR (`2608.29835`), When Patients Cut In (`2608.29241`),
  oncology/tumor-board agents (`2608.28974`), SurvSkill-Bench (`2608.30872`),
  MVC-Bench (`2608.27004`), and the MRI/pathology/clinical items — no reusable
  agent/VLA harness contract.
- Finance/e-commerce: Oculi (`2608.28944`), repurchase prediction
  (`2608.28393`), FORESIGHT-9 trading (`2608.29372`), Factor mining FaVOR
  (`2608.30192`), E-Commerce Bench (`2608.30730`), Differential Reasoning
  Router (`2608.30224`), next-basket recommendation (`2608.30333`),
  SemPOI-RL (`2608.30399`), Agent2UCB (`2608.29063`), ASTRA tickets
  (`2608.28790`).
- LLM inference efficiency: TwinKV (`2608.27128`), SemKV (`2608.28911`),
  block drafting (`2608.27339`), speculative decoding VAT (`2608.30135`),
  CateKV (`2608.30295`), EpaCache (`2608.29264`), TopoCompress (`2608.30811`),
  Universal Context-Reuse Layer (`2608.30963`), Faithfulness Is Not Free KV
  audit (`2608.30996`).
- RLVR/post-training/RL methods: GMTS (`2608.30632`), PAC (`2608.30528`),
  Last Step Matters (watch), Where RLVR Narrows (`2608.29188`), RL-FAT
  (`2608.29247`), When Do Larger Batches Help (`2608.29296`), TTPO-class items,
  Self-OPD (`2608.26872`), On-Policy Distillation (`2608.27857`/`2608.31046`),
  Influence-Directed Distillation (`2608.29846`), RubricRM (`2608.26956`),
  Rubric-guided RL survey (`2608.27505`).
- Scientific-domain agents without reusable contracts: SPARK (`2608.30214`),
  Prove2Me (`2608.28433`), COGTRL (`2608.30109`), Super Library Agent
  (`2608.29310`), ScienceArena (`2608.30517`), PaperGym (watch), AutoSciRub
  (watch), S3C-LLM (`2608.30910`), retrobiosynthesis (`2608.30702`),
  materials synthesis agents (`2608.29309`), Ag/agriculture agents
  (`2608.30088`, `2608.30392`).
- General LLM-agent papers in creative/security/speech/GUI domains without
  harness scope: LandingAgent (`2608.27902`), FRAMEWORKERS (`2608.29814`),
  CineForge (`2608.29621`), HiRS-Agent (`2608.30672`), SemPOI, culture agents
  (`2608.30498`), GPS/geo agents (`2608.29483`, `2608.29880`), document/OCR
  agents (`2608.29037`, `2608.30119`-class), TempJail-class attacks,
  Recognition-Refusal Misalignment (`2608.29109`), Emergent Misalignment
  (`2608.29118`), vendor/industry technical reports (`2608.30181` A.X K2,
  `2608.30320` Qwen3.8-Next), On the Design of Qwen3.8-Next, Manacá-1B
  (`2608.30114`), Arkios (`2608.30092`).
- Tree-search/TTS surveys: When LLM Meets Tree Search (`2608.30395`),
  ERR+ (`2608.28771`), Halt Vector (`2608.28859`) — reasoning methods without
  harness contracts.

## Operational notes

- The arXiv API was rate-limited for the whole run (429 on nearly every first
  attempt, one 503); combined-interval per-category queries plus 30 s+ retry
  backoff completed 10 queries (5 interval + 5 for the `[20260827]` recheck)
  without exhausting retries. `[20260901]` returns 0 entries — the next run
  should re-check it plus growth in `[20260831]`/`[20260830]`/`[20260829]`.
- Artifact checks on 2026-09-01 (live HTTP/GitHub/HF): robotixx.github.io/hydra
  (live; HF mhnazeri/Hydra live with hydra.zip + vertiformer.zip, MIT; GitHub
  mhnazeri/Hydra 404), correctvla.github.io + ovenk3/correct_vla (live),
  baai-agents.github.io/PRACTICE (live; BAAI-Agents/PRACTICE 404),
  xpeng-robotics.github.io/anyworld (404), joonghyuk.com/stateful-vwm-web
  (404), github.com/nubot-nudt/Behavior-Skill (404), thu-ml/Motus (live, Motus
  v1 line; no Motus2 release), motus-robotics HF org (Motus v1 models only).
  Watch-list pages for Q-Planning, Task-CoEvolve, TrapVLA, JIT-Agent, UCAG-P,
  VLAct, Code as Worlds, Aero Hand Open, CAITLYN, Dogwood, Hydra-0, ReflexVLA,
  ManiGuard, HELIX, LEGO-RL all live; Task-CoEvolve still README+figs.
- Scratch XML kept under `.scratch/arxiv-2026-09-01/` during the run and
  removed afterward (not committed).
- Working tree before the run: `docs/reference-architecture.md` modified,
  `docs/ring-harness.png` and `handoff.md` untracked — preserved untouched.
- Consistency: README header badge and Current Landscape "Last verified" both
  updated to 2026-09-01 (both were 2026-08-31 after the previous run).
