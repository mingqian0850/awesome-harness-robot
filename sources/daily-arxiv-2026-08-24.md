# Daily arXiv scan — 2026-08-24

## Scope

- Interval: since the 2026-08-23 run's cutoff (2026-08-23 09:40 UTC) through
  2026-08-24 07:45 UTC, with re-checks of the previously covered announcement
  days. **The arXiv search index expanded since the previous run**: the 08-20
  announcement day now returns 320 records (was 254) and the 08-21
  announcement day is now exposed with 268 records (was 0 in both the 08-21
  and 08-23 runs). The previously unscreened portion is the 08-20 tail above
  the previously reviewed ID span (`2608.20319`–`2608.20627`, 76 records) and
  the entire 08-21 day; both were screened in full here.
- Queries (HTTPS `export.arxiv.org/api/query`, `submittedDate` range syntax,
  categories `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, `cs.LG`; 14-digit
  `YYYYMMDDHHMMSS` timestamps used because the 12-digit form was observed
  leaking the following day's records):
  - `[20260818000000 TO 20260818235959]` → 364 records (documented 363 in
    the 08-23 run; one new arrival, screened below as part of the tail
    analysis — no qualifying item).
  - `[20260819000000 TO 20260819235959]` → 331 records (documented 325; all
    previously reviewed IDs present, +6 index-late records screened, none
    qualifying).
  - `[20260820000000 TO 20260820235959]` → 320 records, all `published` =
    2026-08-20 (documented 254). 76 records above the previously reviewed ID
    span (`2608.20319`–`2608.20627`) screened here; the 244 records at or
    below `2608.20318` match the set re-screened by the 08-23 run.
  - `[20260821000000 TO 20260821235959]` → **268 records** (the 08-21
    announcement day, exposed late; never screened before) — all screened.
  - `[20260822000000 TO 20260822235959]`, `[20260823000000 TO
    20260823235959]`, `[20260824000000 TO 20260824235959]` → 0 records each
    (weekend days; the 08-24 batch not yet exposed at scan time).
  - Revisions: `lastUpdatedDate:[202608230940 TO 202608242359]` → 0 entries,
    but this index proved unreliable (it also missed the 08-21 v2 updates in
    the 08-23 run), so all 31 watch-list papers were instead probed directly
    via `id_list`. Only previously documented revisions exist: HODAgent
    (`2608.17584v2`, withdrawn), DECOWAM (`2608.20114v2`, updated
    2026-08-21T10:04 — no new artifact links), StageWAM (`2608.10780v3`, old),
    and nine 08-20-batch v2s (CoToGrasp, Hear2Act, Block3D, CJSD, Green BOA,
    Identity Essentialism, Learning When to Think, compact image models) — all
    minor updates without new artifacts; DECOWAM and CoToGrasp re-checked in
    detail.
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action models,
  code-as-policy, skill discovery, memory, recovery, evaluation, runtime
  monitoring, and safety. Candidate claims were checked against the arXiv
  record, official project pages, and official code/model/data repositories
  (GitHub API and live HTTP checks on 2026-08-24); existing README, landscape,
  and source records were checked for duplicates. All performance results
  below remain author-reported unless stated otherwise.

## Included (new curated entries)

### GhostTac: Manipulating Tactile Sensors without Physical Contact

- Paper: https://arxiv.org/abs/2608.20817 (ACM CCS 2026)
- Code/demos: https://github.com/GhostTac/GhostTac_CCS (verified live
  2026-08-24; closed-loop grasping, slip detection, and material
  classification code for Franka Panda + Inspire dexterous hand, plus demo
  videos; the repository declares no license)
- Classification: Evaluation/Safety (physical-layer security of embodied
  sensing).
- Why included: the first contactless attack on tactile sensing, a physical
  attack vector against the sensor layer itself. GhostTac exploits nonlinear
  rectification and limited-bandwidth amplification in tactile-sensor
  front-ends to convert crafted EMI signals into persistent DC offsets that
  bypass onboard filtering, giving fine-grained, spatially targeted
  manipulation of measured contact. Evaluated on 10 sensor modules and two
  dexterous hands covering 15 tactile sensors of different types, with three
  closed-loop case studies (grasping, slip detection, material
  classification) in which the attack induces excessive force (object damage
  or injury risk), false or suppressed slip, or misclassification. Extends
  the curated embodied-security line (physical prompt injection, bit-flip
  weight attacks) to sensing hardware, with a real official artifact release.
- Boundary: results are author-reported; the repository is a demonstration
  and attack-implementation artifact rather than a defensive toolkit.

### ClawSentry: A Progressive Multi-Tier Security Monitor for Safeguarding Autonomous LLM Agents

- Paper: https://arxiv.org/abs/2608.21101
- Code: https://github.com/Elroyper/ClawSentry (MIT, verified live
  2026-08-24; reference implementation of the Agent Harness Protocol — AHP —
  with src, examples, Docker/systemd packaging, docs, changelog)
- Classification: General Harness Methodology; Evaluation/Safety.
- Why included: a framework-agnostic security supervision gateway for agent
  runtimes that treats agentic risk as *progressive* across the four loci of
  the agent control loop — skill admission, invocation-time intent,
  execution-time effect, post-action consequence — with a denied dangerous
  objective able to reappear across surface forms, tools, or turns.
  First-use Skill Package Review audits skill packages under a deterministic
  evidence floor before execution (escalating to bounded read-only agentic
  review), a three-tier decision engine (deterministic L1, rule-anchored L2
  semantic reviewer, read-only L3 evidence-seeking agent) spends contextual
  review only on residual ambiguity, and a session-level anti-bypass
  mechanism recognizes tool-switching and rephrased retries. One AHP policy
  applies across Codex, Claude Code, Kimi CLI, and Gemini CLI without
  modifying agent internals. On SkillInject with Codex/GPT-5.4 contextual
  attack success falls from 39.55% to 2.61% (contextual true-skill rate
  83.78%→83.05%); across five Work Agents on SkillsSafety ASR drops to
  9.09–15.03% from 33.5–49.7% unprotected, with aggregate TSR on clean skills
  at 98.7%. A reusable skill-admission/permission contract for any harness
  (the same boundary as Bounded Agents and MaliciousSkillBench), with real
  MIT-licensed code.
- Boundary: digital agents; no robot experiment.

### ACES: Evaluating Skills, Not Just Agents — Agentic Continuous Evaluation of Skills

- Paper: https://arxiv.org/abs/2608.20614 (extended preprint of Agent Skills
  '26 / ACM CAIS 2026 and KDD 2026 workshop versions)
- Code: https://github.com/NVIDIA/SkillEvaluator (Apache-2.0, verified live
  2026-08-24; multi-tier evaluation framework with quality gates, semantic
  overlap detection, synthetic evaluation, CI, docs)
- Classification: General Harness Methodology; Evaluation/Safety.
- Why included: moves the evaluated unit from the agent to the *skill*
  (reusable capability package), answering the deployment question — does the
  capability package help a live agent complete tasks under the same model,
  sandbox, and grading policy? ACES runs paired live trials with and without
  a target skill, normalizes trajectories into the Agent Trajectory
  Interchange Format (ATIF), grades six default runtime metrics, and reports
  **Skill Lift**: the skill's added value for a fixed task, harness,
  workspace, and scorer, supporting baseline/skill/bundle/team-skill/plugin
  targets. The authors report that scan-only gates measure facets
  complementary to LLM-judge scoring (structural versus LLM-judge Spearman
  ρ = 0.14 on 145 real skills) and, across 947 scored paired cases from 58 of
  64 production skills and four harnesses, mean composite Skill Lift 0.2134
  (95% paired-case CI [0.1967, 0.2301]), with the largest process-metric
  gains in skill execution, behavior check, and skill efficiency. A directly
  reusable evaluation contract for skill admission in harnesses, with an
  official Apache-2.0 NVIDIA implementation.
- Boundary: enterprise digital agents; no robot experiment.

### Beyond Imitation: Self-Improving Robot Policies via Off-Policy Q-Planning

- Paper: https://arxiv.org/abs/2608.21204
- Project: https://varungiridhar.github.io/qplanning/ (live 2026-08-24;
  videos) — code marked "coming soon" as of 2026-08-24, so the paper is
  described as substantial but not open.
- Classification: Agentic Robot/VLA Harness (self-improvement); VLA.
- Why included: gives large visuomotor BC policies a self-improvement loop
  without RL fine-tuning of the policy. A small off-policy Q-function is
  trained on the same demonstrations as the BC policy and then absorbs both
  successful and failed deployment rollouts — an asymmetry BC does not have —
  enabling single-step value-guided action selection at inference and online
  self-improvement that fine-tunes only the Q-function while the BC weights
  stay frozen. Ten iterations lift LIBERO-10 from 93% to 99% and bimanual
  RoboTwin from 83.8% to 91.4%; on two contact-rich bimanual real-robot tasks
  the identical loop (BC frozen, no human intervention) improves stack-cups
  40%→90% and insert-wallet 25%→80% in five iterations, where SFT on
  successful rollouts alone stalls at 55%/30%; under an identical online
  budget it is the only method among Best-of-N, filtered SFT, IBRL, DSRL, and
  DAWR that improves stably from failures without an auxiliary actor. A
  recovery-and-improvement contract that leaves the deployment policy
  untouched, extending the self-improving-policy line (Zetta, HERO) with
  real-robot evidence.
- Boundary: results are author-reported; no official code or weights yet.

### DreamBench-SWE: A Multi-Session Memory-Hygiene Benchmark for Software Agents

- Paper: https://arxiv.org/abs/2608.20664
- Release: https://github.com/iroiro147/dreambench-swe/releases/tag/v2.1.0
  (Apache-2.0 repo and v2.1.0 release assets verified live 2026-08-24)
- Classification: Evaluation/Safety (agent memory as harness state).
- Why included: makes agent *memory hygiene* — a harness responsibility —
  the object under test: later software tasks depend on non-inferable
  evidence from earlier sessions and are scored by executable hidden oracles,
  so memory mechanisms are evaluated as executable profile benchmarks rather
  than by self-reports. The benchmark ships with a separately preregistered
  v2.1 successor audit (360/360 work units, 720/720 S3 cells, four
  conditions) whose findings are unusually careful: no external memory
  system improved on the no-memory control (21/180 passes) except one pinned
  hosted-memory configuration (97/180), both mechanism contrasts were
  unavailable after conformance rejection, and the audit explicitly does not
  establish equivalence, superiority among memory-bearing conditions, or
  broad product generality. A discriminating, reproducible evaluation
  contract for the memory layer (the HCL/StateM line) with a real artifact
  release.
- Boundary: digital software agents; no robot experiment.

## Reviewed but not promoted (watch list)

- **ForeTime-VLA** (`2608.20735`) — dense pi0.5 policy distilling a
  future-aware, action-equivalent representation from a frozen Fast-WAM
  teacher, causal at inference; real-robot conveyor-belt grasping (81.1%
  stationary, 58.9% slow-moving, 44/90 vs 23/90 for pi0.5 across three belt
  speeds). Substantial VLA+WAM method with real-robot evidence, but the
  domain is conveyor-belt industrial manipulation, no code/weights on the
  arXiv record as of 2026-08-24; watch for artifacts.
- **Rethinking Demonstration Unlearning in Imitation Learning** (`2608.20784`)
  — retrain-calibrated two-axis (behavior/evidence) audit for robot
  demonstration unlearning with conformal joint tests and preregistered
  conditions on ACT and other real-robot policy classes. Strong evaluation
  methodology for robot data hygiene; no artifacts located; watch.
- **CertVLA** (`2608.20791`) — certified defense for closed-loop VLA control
  under bounded patch/texture attacks (calibrated action-consistency region,
  deterministic covering masks, finite-sample clean coverage, consistency
  certificate ⇒ task-success certificate under dual-mask correctness).
  Embodied-security line; no official repo located as of 2026-08-24; watch.
- **Q-Planning** (`2608.21204`) — included above; code "coming soon" — watch
  for the release to upgrade the entry to open.
- **PhysCaP** (`2608.21031`) — code-as-policy agent with physics-informed
  exploration (mass/stiffness extraction from proprioception, Planner/
  Prioritizer dual-agent exploration control; real tabletop + LIBERO). Project
  page live (template-based) but no verified code; watch.
- **Graph-Operator World Models** (`2608.20936`) — graph-structured world
  model factorizing transition into morphology-independent local dynamics +
  morphology-conditioned structured operator; MuJoCo splits across Hopper/
  Walker2d/HalfCheetah. No artifacts; watch.
- **Logic-VLA** (`2608.20556`) — STL-conditioned VLA via syntax-graph STL
  encoder + SFT + trajectory-level preference optimization; +24.8–40.7 pp STL
  satisfaction in closed-loop quadcopter navigation simulation. Simulation-
  only, quadcopter domain, no artifacts; watch.
- **CIVA** (`2608.21114`) — critic-induced value-subspace attacks on visual
  world-model agents (DreamerV3-class); digital control domains; no artifacts;
  watch.
- **Don't Solve, Just Compare / COTA** (`2608.21027`) — comparison-only tiny
  advisors for constructive runtime intervention (WebShop, ALFWorld,
  tau^3-Retail). Digital runtime-intervention line (Outcome Monitors); no
  artifacts; watch.
- **AID-Guard** (`2608.21159`) — stateful authorization-to-effect closure
  protocol (revalidate at commit, single reservation, delivery fence;
  Python/SQLite prototype; Stripe/Resend trials; 44/44 attacks blocked under
  complete proposer compromise). No public repo verified as of 2026-08-24;
  watch.
- **TraceGrant** (`2608.21126`) — contract-governed security framework for
  the task-effect lifecycle of networked LLM agents. No artifacts; watch.
- **Artic** (`2608.21341`) — artifact-driven compilation of natural-language
  workflows (declared read/write artifacts per step). Digital; no artifacts;
  watch.
- **When Failures Propagate / AgenticRAG-FP** (`2608.20627`) — interventional
  benchmark for causal failure attribution in agentic RAG. Digital; no
  artifacts; watch.
- **Calibrating Criterion Revision** (`2608.20729`) — five-condition
  attribution protocol for criterion revision in agents (CMB-0.4 prospective).
  Digital evaluation methodology; no artifacts; watch.
- **Weighted Memory Tree** (`2608.20631`) — hierarchical agent memory with
  per-memory weights (tasks/subtasks/actions). Digital; no artifacts; watch.
- **Utility Under Attack** (`2608.21230`) — agent memory poisoning; screening
  pipelines reject 0 of 360 poisoned memories; boundary argument for
  write-time screening limits. Digital; no artifacts; watch.
- **The Claws in Plain Sight** (`2608.20658`) — authority-pressure tool-call
  context disclosure. Digital; no artifacts; watch.
- **Automated Trajectory Evaluation / CRATE** (`2608.20797`) — two-stage
  VLM-as-judge trajectory evaluation for mobile agents with step-level
  consequence reasoning. Digital; no artifacts; watch.
- **Trustworthy RAG** (`2608.21095`) — evaluation agent for misinformation/
  knowledge poisoning (Trust Index, five-signal detector); GitHub
  `GPT-Laboratory/TrustworthyRAG` claimed in abstract, not verified live as of
  2026-08-24 (API rate-limited); digital; watch.
- **No Judgment Without a Reason** (`2608.20938`) — counterfactual judgment
  receipts and ReasonBench (19,520 cases) for versioned AI evaluators.
  Digital evaluation accountability; no artifacts; watch.
- **AsmEvo** (`2608.20711`) — agentic assembly-level AMDGPU kernel
  optimization with functional-equivalence verification. Digital systems
  optimization; no artifacts; watch.
- **BC-Bench** (`2608.20851`), **AgentMercury** (`2608.20634`), **SAGE**
  (`2608.20630`) — digital agentic-engineering benchmarks/frameworks without
  harness contracts or verified artifacts; excluded.
- **Koala Gripper** (`2608.20546`) — co-designed handheld data-capture +
  robotic gripper for scaling dexterous manipulation data; website and video
  live, but the linked `rai-inst/Koala-Gripper` repository returns 404 as of
  2026-08-24; watch for the hardware/CAD release.
- **VisTa3D** (`2608.20740`) — thin-object reconstruction dataset/benchmark
  (RGB + depth + tactile + laser-scanned ground truth). Perception-oriented;
  no verified dataset release; watch.
- **RISE** (`2608.20430`) — adaptive Roll/Stop imagination budgets for WAMs
  with CounterDrive counterfactual dataset; evaluated on NAVSIM/nuScenes —
  driving domain, excluded per policy (noted for its plug-in-generality
  claim).
- **WA-JEPA** (`2608.20974`), **VLA-based E2E driving** (`2608.20890`),
  **Roadside-Cooperative Driving** (`2608.21032`), **Distributed Sim-Based
  Driving Testing** (`2608.20904`), **Collision-Avoidance Verification**
  (`2608.20864`) — driving-domain; excluded per policy.
- **Sit-to-Stand humanoid RL** (`2608.20823`), **Demonstration-guided
  humanoid stand-up** (`2608.20852`), **Fast Coordinated Bimanual Motion
  Planning** (`2608.20946`), **Hybrid Roller-Jamming Gripper** (`2608.20962`),
  **SRL-MPC** (`2608.21175`), **Neural-Primitive flight planner**
  (`2608.20948`), **FF-MPCC formation flight** (`2608.21056`), **NeSAM
  off-road kinodynamics** (`2608.21330`), **IMU-free quadcopter state
  estimation** (`2608.20891`), **Coastline ASV localization** (`2608.21276`),
  **Drone swarm safety architecture** (`2608.20906`) — conventional
  control/hardware/perception without a reusable agent/VLA harness, recovery,
  safety, or evaluation contract; excluded.
- **VT-MUSE** (`2608.21290`), **ViTacPhys** (`2608.21355`), **AUSO**
  (`2608.21292`), **TaPeR** (`2608.21035`), **TOSS** (`2608.21083`),
  **Action-JND token compression** (`2608.21247`) — representation/policy/
  human-factors contributions without verified artifacts or harness
  contracts; watch.
- **CARD** (`2608.20763`), **Belief Without Behavior / MOSAIC** (`2608.20975`)
  — VLM diagnostics/benchmarks (grid-world, social action) without robot or
  harness contracts; excluded.
- **Terminal Agents survey** (`2608.20485`), **Graph Engineering**
  (`2608.21156`), **Space-mining robotics survey** (`2608.21358`) — surveys/
  position papers; excluded (noted for reference).
- **Applying Anthropic Primitives at Large Enterprises** (`2608.20622`) —
  harness-paradigm experience report for enterprise knowledge work; no
  artifacts or robot content; excluded, noted as an enterprise-harness data
  point.
- **AI with Authority** (`2608.21356`), **ProofJudge** (`2608.20432`),
  **Asymmetric Capacity Allocation** (`2608.21345`), **MTCR** (`2608.20820`),
  **ReFrame** (`2608.21100`), **AEGIS MCP resource abuse** (`2608.20481`),
  **Consilience** (`2608.20564`), **AgentDecarbonizer** (`2608.20566`),
  **Beyond End-to-End Success** (`2608.20563`), **Specification Portability**
  (`2608.21208`), **6G intent control** (`2608.21049`), **Military C2 agent
  testing** (`2608.20597`), **Medical/legal/finance domains** (`2608.21057`,
  `2608.20661`, `2608.21089`, `2608.21036`) — domain-specific or digital
  without a reusable harness/recovery/safety/evaluation contract; excluded.
- Generic perception, planning, navigation, medical, UAV, video, finance,
  quantum, physics, and conventional RL papers in the screened ranges without
  a reusable agent/VLA harness, recovery, safety, or evaluation contract were
  excluded per policy.

## Watch-list status (rechecks)

- Direct `id_list` probe of 31 watch items (2026-08-24): no new revisions
  beyond the documented HODAgent v2 (withdrawn), StageWAM v3, DECOWAM v2
  (re-checked; still no artifact links, ARMDOG dataset not located).
- **Task-CoEvolve** (`2608.20169`) — repo still README + figures only, no
  license, pushed 2026-08-23; code still marked "will be available soon";
  unchanged.
- **Koala Gripper** (`2608.20546`) — NEW watch item; linked repo 404.
- **Q-Planning** (`2608.21204`) — NEW watch item (included; code pending).
- **EXIMO** (`2608.19891`), **HiTac-WAM** (`2608.19574`), **OrthoSkillVLA**
  (`2608.19589`), **SafeBranch** (`2608.19729`), **SCAPE** (`2608.19425`),
  **EAFG** (`2608.20084`), **Panda ROS 2 stack** (`2608.19740`), **Outcome
  Monitors** (`2608.19303`), **StateMemBench** (`2608.19652`), **Adaptive
  Probabilistic Shielding** (`2608.19836`), **Orthogonal JEPA**
  (`2608.20065`), **DBOSC** (`2608.19492`), **Temporal-Logic TAMP**
  (`2608.19453`), **Credit Without Ground Truth** (`2608.19760`),
  **AI4AI-Bench** (`2608.20318`), **Optimal Skill Selection** (`2608.19993`),
  **Self-Demonstrated VLA fine-tuning** (`2608.19490`), **ExPhy**
  (`2608.20009`), **The Missing Touch** (`2608.19372`), **Neural Reduced
  Dynamics** (`2608.19375`), **Brain Researcher** (`2608.19902`),
  **Thinkingbox** (`2608.19741`), **StageWAM** (`2608.10780`), **ReflexVLA /
  ReflexBench** (`2608.14379`), **DreamX-Phi** (`2608.13489`), **ForceU-VLA**
  (`2608.15009`), **UniTexture** (`2608.13453`), **PRISM** (`2608.17962`),
  **GigaBrain-0.7** (`2608.15875`), **GigaBrain-WBC-0.5** (`2608.18234`),
  **BATON** (`2608.16889`), **Hydra-0** (`2608.18077`), **ADEPT**
  (`2608.19182`), **LIBERO-VIFO** (`2608.17600`), **Agent Lightning v1.0**
  (`2608.17528`), **VLCP** (`2608.16978`), **MANIGUARD** (`2608.17386`),
  **LEGO-RL** (`2608.17393`) — no new revisions or artifact releases;
  unchanged.
- Newly added: ForeTime-VLA, Demonstration Unlearning audit, CertVLA, Q-
  Planning (code), PhysCaP, GraphOp-WM, Logic-VLA, CIVA, COTA, AID-Guard,
  TraceGrant, Artic, AgenticRAG-FP, Criterion Revision, Weighted Memory Tree,
  Utility Under Attack, Claws in Plain Sight, CRATE, Trustworthy RAG,
  Judgment Receipts, AsmEvo, Koala Gripper, VisTa3D, VT-MUSE, ViTacPhys,
  AUSO, TaPeR, TOSS, Action-JND — see "Reviewed but not promoted".
