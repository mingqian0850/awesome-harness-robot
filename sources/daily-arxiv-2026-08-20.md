# Daily arXiv scan — 2026-08-20

## Scope

- Interval: after the 2026-08-19 scan cutoff (09:20 UTC) through 2026-08-20
  06:30 UTC. The previous run reported that the 2026-08-19 announcement batch
  was not yet exposed; this scan covers it fully, re-checks 2026-08-18 for late
  arrivals, and checks 2026-08-20 (empty at scan time).
- Queries (HTTPS `export.arxiv.org/api/query`, `submittedDate` range syntax,
  categories `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, `cs.LG`):
  - `[202608190000 TO 202608192359]` → 274 unique records, all `published` =
    2026-08-19 (the complete 08-19 announcement day, absent from the previous
    run).
  - `[202608180000 TO 202608182359]` → 360 records; 71 of them (IDs
    `2608.18177`–`2608.18389`, all `published` = 2026-08-18) were not covered
    by the 08-19 run and were screened here as late arrivals.
  - `[202608200000 TO 202608202359]` → empty (the 08-20 batch was not yet
    exposed at scan time).
  - Revisions: `lastUpdatedDate:[202608190000 TO 202608202359]` returned only
    the 08-19 batch itself; the 08-18 query surfaced twelve v2 updates made on
    08-19, of which only two touch previously reviewed work — **EATR-Stereo**
    (`2608.17453v2`) and **D²ACCI** (`2608.17756v2`) — and both are trivial
    (file-size-only edits, no new comment, no artifacts). No older paper was
    revised in the window.
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action models,
  code-as-policy, skill discovery, memory, recovery, evaluation, runtime
  monitoring, and safety. Candidate claims were checked against the arXiv
  record, official project pages, and official code/model/data repositories
  (GitHub/Hugging Face API and live HTTP checks on 2026-08-20); existing
  README, landscape, and source records were checked for duplicates. All
  performance results below remain author-reported unless stated otherwise.

## Included (new curated entries)

### The Embodiment Gap in Robot Foundation Models

- Paper: https://arxiv.org/abs/2608.18433 (published in TMLR, August 2026 —
  verified from the arXiv HTML reference list)
- Classification: Robot Foundation/World Model (survey/evaluation contract).
- Why included: defines the *embodiment gap* — the work required between a
  reusable model, representation, or dataset and execution on a robot with a
  particular body — which differs across methods and target robots and is
  hidden by success-rate comparisons. The survey places existing RFM/VLA
  methods on a two-axis map (type of shared structure × stage where adaptation
  is needed) across three research directions (shared semantics and
  perception; shared robot data and interfaces; learning correspondence across
  embodiments) and proposes a reporting framework for adaptation work. This is
  a peer-reviewed conceptual contribution with a reusable evaluation contract
  for judging RFM portability claims.
- Boundary: survey/position contribution; no artifacts.

### Harness Continual Learning (HCL): Continual Adaptation Beyond Model Parameters

- Paper: https://arxiv.org/abs/2608.19013
- Classification: General Harness Methodology; Self-improvement.
- Why included: formulates *harness-level forgetting* — a harness update
  (prompts, memories, tools, skills, routing rules) can disrupt previously
  reliable behavior even when the model is frozen — and defines Harness
  Continual Learning as evolution of the harness around a frozen foundation
  model. Four execution-facing components (Task Interface, Experience Memory,
  Capability Map, Adaptive Router) plus *guarded harness evolution*: a
  Continual Optimizer proposes candidate harnesses from post-execution
  feedback and a Continual Evaluator commits only after checking current
  improvement, historical retention, and validity. A named paradigm with
  concrete components, directly relevant to robot-harness maintenance.
- Boundary: digital-agent results; no official code was located as of
  2026-08-20 (a same-named "Continual Harness" repository belongs to a
  different May 2026 paper and is not this work).

### One Gate Is Not Enough: Composing Stateful Pre-Action Controls for Agentic AI

- Paper: https://arxiv.org/abs/2608.18360
- Code: https://github.com/besanson/sarc-suite-one-pass (Apache-2.0, verified
  live 2026-08-20)
- Data: Zenodo DOI 10.5281/zenodo.22003399 (resolves)
- Classification: General Harness Methodology; Evaluation/Safety.
- Why included: formalizes the composition of pre-action controls (authority,
  resource, evidence gates) and identifies *remediation-induced control
  coupling* — a remediation applied by one gate can change the action,
  evidence, or context another gate evaluates, invalidating its earlier
  judgment. Provides a remediate-and-regate protocol that restores per-action
  soundness, and a finite-model checker finds the two implemented remediation
  operators (evidence substitution, resource-budget downroute) do not
  commute, making remediation order part of control-plane semantics. This is
  a harness-ownership result (permissions and gating live outside the model)
  with a real reference implementation.
- Boundary: digital agents; no robot experiment.

### SkillGate: Training In-Policy Skill Selection in Long-Horizon Agents

- Paper: https://arxiv.org/abs/2608.18852
- Code: https://github.com/DeepExperience/SkillGate (Apache-2.0, verified live
  2026-08-20)
- Weights: https://huggingface.co/simonlqy/SkillGate-9B (public, verified)
- Classification: General Harness Methodology; Self-improvement.
- Why included: makes mid-episode skill selection — which skill document an
  agent reads — a trained policy decision, and identifies why outcome-rewarded
  RL cannot teach it: *selector credit starvation*, where under broadcast
  sequence-level advantage the few skill-naming tokens carry a vanishing and
  increasingly wrong-signed share of the loss. SkillGate partitions token
  support into disjoint credit channels (outcome credit reaches only execution
  tokens; a separate action-local advantage trains exactly the skill-naming
  tokens), fixing the failure by construction. Skill registry selection is a
  core harness responsibility; real code and released weights make the recipe
  reproducible.
- Boundary: digital-agent results (no robot experiment).

### SPADE: Self-Play in Adaptive Synthetic Executable Environments

- Paper: https://arxiv.org/abs/2608.19197
- Project: https://spade-rl.github.io
- Code: https://github.com/spade-rl/spade (MIT, verified live 2026-08-20)
- Weights: https://huggingface.co/spade-rl (SPADE-Qwen3-30B-A3B ToolUse and
  Games, Apache-2.0, verified)
- Classification: General Harness Methodology; Self-improvement.
- Why included: self-play across the environment-design boundary: one LLM
  plays Environment Designer (writes complete long-horizon training
  environments as executable code with Gym-style reset/step interfaces,
  spanning reasoning problems and multi-step agentic tool use) and Reasoning
  Agent (learns in them). The designer optimizes the agent's regret signal
  (reward with versus without privileged hints) so environments target the
  edge of capability while staying feasible — an explicit harness for
  self-improvement that owns goal generation. Real code and weights.
- Boundary: digital agents; the paper labels itself work in progress.

## Updated entries (artifact releases resolved)

### MANIGUARD — code repository went live

- The official repository https://github.com/NU-IDEAS-Lab/ManiGuard (Apache-2.0,
  pushed 2026-08-20, containing the `maniguard` evaluation package, monitors,
  configs, tests, and a teleop bridge) replaced the 404 observed on
  2026-08-19. README entries updated to reflect that the code is now open;
  benchmark data remain CC-BY-4.0. No paper revision.

### LEGO-RL — code repository went live

- The official repository https://github.com/LegoX/Lego-RL (Apache-2.0, pushed
  2026-08-20, core library, harness patches, web UI, docs) replaced the 404
  observed on 2026-08-19. README entries updated; the docs site
  (https://lego-rl.pages.dev) remains live. No paper revision.

## Reviewed but not promoted

- **GigaBrain-WBC-0.5** (`2608.18234`) — first Behavior World Model for
  humanoid whole-body control (the acting network also predicts next state and
  next latent command, with an automatic terrain-annotation pipeline). The
  project page (https://shepherd1226.github.io/gigabrain-wbc-0.5/) is live but
  marks code "coming soon" and real-robot footage forthcoming; watch for the
  release (GigaBrain-0.7 remains on the watch list for its promised
  training-code/weights).
- **Revisiting Push-T with Agentic Robotics** (`2608.18227`) — short study of
  an LLM coding agent (Claude Code with Fable 5) solving Push-T without
  demonstrations (100% success, 46% fewer steps than a diffusion policy,
  Push-A–Push-Z curriculum, 3D cross-embodiment sims). Interesting agentic
  code-as-policy evidence, but the abstract says videos/policies "will be
  posted online" — nothing was verified as released as of 2026-08-20; watch.
- **LabDex** (`2608.18618`) — hierarchical benchmark for dexterous lab
  manipulation (atomic skills → compositional tasks → long-horizon
  experiments, cross-platform sim+real). No official project page, dataset,
  or code was located as of 2026-08-20; watch for artifacts.
- **SoftVTBench** (`2608.18701`) — deformation-aware visuo-tactile dataset
  (4,000 demos, FEM ground truth, DSR metric). Its arXiv record carries no
  comment or links; note the name collides with an older, different paper
  (2607.04234, "Safety-Aware Visuo-Tactile Benchmark") whose Hugging Face
  dataset `Arthur12137/SoftVTBench` exists — the 2608.18701 artifacts were not
  verified; watch without conflating the two.
- **RoboEdit** (`2608.18948`) — human-to-robot video editing suite with
  RoboEdit-14M (174K aligned pairs, seven embodiments). No official page or
  dataset release located; watch for the data.
- **ADEPT** (`2608.19182`) — large-scale RL pre/post-training for dexterity
  (sim-to-real on 23-DoF Kuka-Allegro and 29-DoF Flexiv-Sharpa). Project page
  live but no code link; watch.
- **GS-VLA** (`2608.19066`) — plug-and-play viewpoint canonicalization for
  frozen VLA policies via 3D Gaussian novel-view synthesis (LIBERO success
  ~90%→~10% under camera shift, recovered without retraining). Relevant
  observation-space wrapper for VLA deployment; no code located; watch.
- **Dream2Reward** (`2608.18787`) — dense reward models from positive
  demonstrations via a learned successful-transition latent field (no failure
  annotations), reducing reward hacking in robot learning. No artifacts; watch.
- **DA-LeWM / Decision-Metric Alignment** (`2608.18746`) — JEPA-style latent
  world models: Plan-Real/CEM-stage Spearman diagnostics show latent distance
  can mis-rank action sequences, and action-conditioned heads fix alignment.
  Extends the JEPA line (PSG-JEPA, StageWAM) toward decision-grounded
  evaluation; no code; watch.
- **RoleSub** (`2608.18410`) — role-conditioned sub-token routing compresses
  VLA KV values (9.2–11.3% of original on OpenVLA-OFT-7B/LIBERO). Efficiency
  method rather than harness; no code; watch.
- **RoomWright** (`2608.18840`) — agentic usage-driven 3D code scenes for
  embodied interaction (task-centered object reasoning, trigger/condition/
  effect rules). Interesting environment-interface contract; no code located;
  watch.
- **LT-Mem** (`2608.19059`, IROS 2026) — volatility-aware spatio-temporal
  memory (Live/Delta/Meta) with LT-VQA for lifelong scene understanding.
  Memory architecture with an evaluation suite; no artifacts verified; watch.
- **RTPO** (`2608.18682`) — reverse-turn policy optimization stabilizing
  multi-turn agentic RL (sparse reverse trees, turn-level credit assignment).
  Training-time algorithm; no code; watch.
- **GuideFetch** (`2608.18292`) — coordination framework for concurrent
  navigation and object retrieval by heterogeneous guider/fetcher robot dogs
  (LLM-schedule-conditioned action schema with capability validation, 360
  simulated executions). Simulation study; no code or project page located;
  watch.
- **Affordance Sheet** (`2608.18317`, ECCV 2026 HCMIW workshop) — proposes a
  documentation standard (task formulation, modalities, architectures,
  datasets, protocols) for reproducible affordance benchmarking. The project
  page (https://apicis.github.io/aff-sheet) is live but marks arXiv,
  repository, and tool all "coming soon"; watch.
- **ComponentBench** (`2608.18307`, COLM 2026) — component-level failure
  diagnostics for computer-use agents with a 97-component ontology, 2,910
  verified tasks, code (MIT), data, and website all live. Real artifacts, but
  GUI/computer-use domain — excluded per policy (GUI/browser agents are out of
  scope); noted here so the verified release is not lost.
- **EATR-Stereo v2** (`2608.17453v2`) — v2 is a trivial revision (file-size
  change only); no code; watch status unchanged from 08-19.
- **D²ACCI v2** (`2608.17756v2`) — v2 is a trivial revision; artifact still not
  verified; watch status unchanged.
- **Trust as a Field** (`2608.18178`), **GAPL** (`2608.18254`), **DA-WAM**
  (`2608.19085`) — driving-domain world/planning models; excluded per policy
  unless a reusable harness/safety contract is offered (none verified).
- **Payload Swing Estimation** (`2608.18625`), **SLAM on UAV Footage**
  (`2608.18632`), **UAV Visual Odometry** (`2608.18624`), **Downwash Continuum
  Manipulator** (`2608.18507`) — UAV-specific control/perception; excluded.
- **Progressive Experience Fusion** (`2608.18647`), **Tool-Tissue Contact
  Detection** (`2608.18270`), **Tissue Tension Skill Assessment** (`2608.17935`)
  — surgical/medical; excluded.
- **AgriNav tractor** (`2608.19004`), **HarvestPoint-ACT** (`2608.18446`) —
  domain-specific agricultural robotics without a reusable harness contract;
  excluded.
- **Multimodal Rapport Estimation in HRI** (`2608.18401`, ICMI 2026) — HRI
  interaction-quality estimation; evaluation method without a robot/harness
  contract; excluded.
- **FACET** (`2608.18580`), **Governance Records as Supervision** (`2608.18324`),
  **Jagged Frontier** (`2608.18389`), **Least-Privilege Learning**
  (`2608.18351`), **Claim-aware Observability** (`2608.18312`), **Competence,
  Not Accuracy** (`2608.18719`), **AFANet / GNN Failure Attribution**
  (`2608.18575`), **FM-Bench** (`2608.18423`) — digital-agent evaluation/
  training papers without verified robot or harness-system contracts;
  excluded per policy (several were checked for artifacts; none located).
- **Selection, Recombination, or a Fresh Solve** (`2608.18379`), **Cacheable
  by Design** (`2608.18261`), **Think Shallow, Solve Deep** (`2608.18222`),
  **Redakto** (`2608.18260`), **HB-SJD** (`2608.18183`), **Continual Reasoning
  Gym** (`2608.18574`), **EvoResearcher** (`2608.18884`), **CurriPO**
  (`2608.18770`) — digital LLM/RL/reasoning studies; excluded.
- Generic perception, planning, navigation, video, medical, and conventional
  control papers in the two ranges without a reusable agent/VLA harness,
  recovery, safety, or evaluation contract were excluded per policy.

## Watch-list status (rechecks)

- **MANIGUARD** (`2608.17386`) — RESOLVED: code repo live (Apache-2.0,
  2026-08-20); README updated.
- **LEGO-RL** (`2608.17393`) — RESOLVED: code repo live (Apache-2.0,
  2026-08-20); README updated.
- **HELIX** (`2608.13951`) — repository still live (MIT); no further change.
- **StageWAM** (`2608.10780`) — still v3; no new code, weights, or revision.
- **ReflexVLA / ReflexBench** (`2608.14379`) — project page still says code
  "after acceptance"; unchanged.
- **DreamX-Phi 1.0** (`2608.13489`) — official repo `AMAP-ML/DreamX-Phi` still
  README-only (no weights/inference code); unchanged.
- **ForceU-VLA** (`2608.15009`) — official repo `VMVLab/ForceU-VLA` still
  README+assets only; unchanged.
- **UniTexture** (`2608.13453`) — still no official attack code or benchmark
  artifacts; unchanged.
- **PRISM** (`2608.17962`) — project page (tengbo-yu.github.io/PRISM) still
  says "Dataset soon"; GitHub link present but dataset not released;
  unchanged.
- **GigaBrain-0.7** (`2608.15875`) — blog page (gigaai.cc/blog/gigabrain07) is
  JavaScript-rendered and no release announcement could be verified; the
  promised training-code/weights release remains unconfirmed; unchanged.
- **BATON** (`2608.16889`) — still no code; unchanged.
- **Hydra-0** (`2608.18077`) — project page still marks code "coming soon";
  unchanged.
- **LIBERO-VIFO** (`2608.17600`), **Agent Lightning v1.0** (`2608.17528`),
  **VLCP** (`2608.16978`), **HODAgent** (`2608.17584`) — no new artifacts;
  unchanged.
- **Newly added**: GigaBrain-WBC-0.5, Push-T agentic, LabDex, SoftVTBench
  (2608.18701), RoboEdit-14M, ADEPT, GS-VLA, Dream2Reward, DA-LeWM, RoleSub,
  RoomWright, LT-Mem/LT-VQA, RTPO (see "Reviewed but not promoted").
