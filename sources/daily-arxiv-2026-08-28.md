# Daily arXiv scan — 2026-08-28

## Scope

- Interval: since the 2026-08-27 run's cutoff (~2026-08-27 06:00 UTC) through
  2026-08-28 ~05:30 UTC. Main target: the `[20260827]` submittedDate slice
  (the batch announced 2026-08-28 00:00 UTC), which returned nothing at the
  08-27 run's query time and is now fully exposed.
- The arXiv export API (`https://export.arxiv.org/api/query`, HTTPS) was
  healthy. `submittedDate:[20260827000000 TO 20260827235959]` returned **341
  unique entries** (cs.RO 41, cs.AI 109, cs.CL 86, cs.CV 102, cs.LG 105 raw
  hits, many cross-listed), all `v1` with `published == updated` — the entire
  slice is new submissions; no revisions inside it. XML saved under the
  workspace scratch dir (`.scratch/arxiv-2026-08-28/`, removed after the run).
- Recheck of the previous batch: `submittedDate:[20260826000000 TO
  20260826235959]` now returns **334 unique entries** vs 317 at the 08-27 run's
  query time — the previous batch grew by late additions. Per procedure, the
  scope-relevant subset of that batch that the 08-27 record did not name was
  re-screened once (Same Model Different Harness, SKILL.state, Agent Mesh,
  WALL-SS, LocalLSTC, Verdict Staleness/FBS, KnownLieBench, CTF-ABACUS,
  OpEmbed, the neuro-symbolic reproducibility audit, Cross-Platform 3D
  Reconstruction for lab robots, ATC runtime monitoring, LLM judging in MAS,
  AutoVerifier, parametric KG memory, and the 12 entries now carrying
  `updated != published`).
- Revisions in the `[20260826]` slice (12 entries with `published != updated`):
  watch items probed via abs pages — **LM-X v2** (`2608.25757v2`, updated
  2026-08-27T12:24, no artifact links added, unchanged/watch), **Zero-WAM v2**
  (`2608.26103v2`, same-day update of an already-included paper, no artifact
  change), **When Stale Constraints Go Unchecked v2** (`2608.25553v2`, now
  archives manuscript, LaTeX source, all 5,400 episode files, frozen
  specifications and scripts at Zenodo doi:10.5281/zenodo.22117197 — a real
  data-artifact release for a digital-agent memory-verification measurement;
  domain unchanged, stays watch), **VISA v2** (`2608.26013v2`, excluded
  domain), plus 4DStreamCtrl v2, ClueWeaver v2 (code now at
  github.com/Ameame1/ClueWeaver), QLoRA facts v2, DESCENT v2, and other
  excluded-domain revisions — no qualifying change.
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action models,
  code-as-policy, skill discovery, memory, recovery, evaluation, runtime
  monitoring, and safety. Candidate claims were checked against the arXiv
  record, official project pages, and official code/model/data repositories
  (live HTTP and GitHub API checks on 2026-08-28); existing README, landscape,
  and source records were checked for duplicates (no name/ID collisions). All
  performance results below remain author-reported unless stated otherwise.

## Included (new curated entries)

### Riemann-1.0: An Embodied World Action Model for Physical AI

- Paper: https://arxiv.org/abs/2608.27033
- Classification: Robot Foundation/World Model (unified policy + simulator WAM).
- Why included: a fully causal autoregressive World Action Model that unifies
  online robot policy execution and action-conditioned world simulation in a
  single checkpoint — multi-view observations, robot state, and
  embodiment-specific actions form one causal sequence, so actions and world
  evolution are the same modeled state transitions (contrasted explicitly
  against joint-generation, video-first, and decoupled WAM paradigms). A
  progressive embodied pretraining framework unifies egocentric human videos,
  handheld-gripper demonstrations, and heterogeneous robot trajectories under
  one World Action Modeling objective; built on 200K+ hours of interaction
  data, the authors report 94.3% RoboTwin2.0, 99.0% LIBERO, 62.6% on
  long-horizon RoboCasa-365 (+8.4 over the previous best) and long-horizon
  real-world manipulation results. The strongest statement of the
  unified-policy-plus-simulator WAM thesis to date.
- Boundary: closed model — no official code, weights, or project page located
  as of 2026-08-28 (the model was publicly announced in July 2026 by Riemann
  Dynamics; the arXiv paper is new today). Results author-reported.

### CLAP: Cross-Embodiment Video World Models are Zero-Shot Physical Simulators

- Paper: https://arxiv.org/abs/2608.27406
- Project: https://omni-clap.github.io (live 2026-08-28)
- Code: https://github.com/omni-CLAP/clap (MIT, ~230 MB, pushed 2026-08-25;
  verified live via GitHub API)
- Weights: https://huggingface.co/omni-CLAP/CLAP (OXE curriculum and
  end-effector checkpoints, bimanual YAM and G1 humanoid adaptations,
  DROID/Bridge post-training variants; verified live)
- Classification: Robot Foundation/World Model (cross-embodiment video WAM).
- Why included: makes universal physics the training signal by learning
  action-conditioned video generation across human and robotic agents on
  internet-scale video. Disparate action spaces (end-effector poses, language
  instructions, latent actions) are reconciled in one model, and a
  curriculum-based cross-embodiment recipe first learns foundational physical
  priors from unlabeled video via latent actions, then grounds them into
  end-effector action spaces for zero-shot deployment. The authors report CLAP
  approaching or surpassing single-embodiment SOTA in DROID with gains
  compounding under few-shot adaptation. A real MIT-licensed artifact release
  (code + checkpoints) in the cross-embodiment video-world-model line
  (XEWorld, Hydra-0's action flow).
- Boundary: results author-reported; training-scale numbers (internet-scale
  video) not independently reproducible by users.

### HarnessLens: Verify Smarter, Evolve Further (Efficient Harness Evolution through Behavior-Aware Verification)

- Paper: https://arxiv.org/abs/2608.27311
- Code: https://github.com/jhxu5214/HarnessLens (live 2026-08-28, pushed
  2026-08-27; no license asserted, small codebase)
- Classification: General Harness Methodology (budget-aware harness evolution).
- Why included: fixes the verification bottleneck of propose-and-verify
  harness optimization — scoring every candidate on a fixed task set wastes
  rollouts on unrelated behaviors and lets aggregate scores obscure specific
  regressions. HarnessLens jointly explores the task space and user-configurable
  harness components, derives candidate modifications from execution
  trajectories, and selectively verifies each candidate on behavior-relevant
  tasks through an attributable-evidence gate. Across three agent harnesses and
  four benchmarks the authors report +7.6–13.6% average held-out performance at
  substantially lower evaluation budget than competing baselines. Direct
  continuation of the JIT-Agent/Task-CoEvolve/OpsHarness harness-evolution
  line, with released (unlicensed) code.
- Boundary: digital agents; no robot experiment; no license on the repository
  as of 2026-08-28.

### LoopHarness: Safety Does Not Compose (Non-Decaying Loop State for Autonomous LLM Agents)

- Paper: https://arxiv.org/abs/2608.27141
- Classification: General Harness Methodology / Evaluation–Safety (loop-level
  safety state).
- Why included: a formal composition failure for trajectory-scoped agent
  safeguards. Against attacks whose evidence is fragmented across iterations,
  every trajectory-scoped monitor has true-positive rate equal to its
  false-positive rate, because the evidence never appears in the window it
  sees; a monitor retaining cross-iteration state separates the two perfectly.
  The obvious repair — a geometrically decaying risk score — is insufficient,
  because the cooling-off period a patient adversary must wait is a constant
  that does not grow with the horizon. LoopHarness restores persistent,
  non-decaying safety state at the loop level and bounds expected unauthorized
  irreversible actions by B+m−1+m/δ_M — a constant in the horizon N, of which
  the model-free-rule term survives a fully colluding verifier. A
  harness-ownership result (safety state as a harness responsibility) in the
  Bounded Agents / One Gate Is Not Enough line.
- Boundary: digital agents; no robot experiment; no official artifacts located
  as of 2026-08-28; formal results and an evaluation protocol are the
  contribution.

### SARA: When Tool Outputs Become Commands (Separating Action Induction from Runtime Authorization)

- Paper: https://arxiv.org/abs/2608.27146
- Classification: Evaluation/Safety (runtime authorization separation);
  General Harness Methodology.
- Why included: names a concrete control-plane conflation — tool outputs that
  begin to specify concrete actions become "commands" that can drive real-world
  side effects beyond user intent, and the risk arises from conflating action
  induction with execution authorization. SARA treats them as distinct runtime
  roles: a context-isolated Action Probe exposes action-inducing semantics and
  persistently records action-origin provenance as a review signal, while tool
  calls are authorized only against the user objective and audited evidence
  from authorized successful executions, with No-History-Promotion preventing
  historical recurrence from laundering action origins into execution
  authority. Across AgentDojo and AgentDyn the authors report ASR at most
  0.63% across four primary settings with competitive task utility. An
  authorization-contract contribution in the One Gate Is Not Enough /
  ClawSentry / StepGuard line.
- Boundary: digital agents; no robot experiment; no official artifacts located
  as of 2026-08-28.

### TrapVLA: Trapping Vision-Language-Action Models in Configured Failure Modes

- Paper: https://arxiv.org/abs/2608.26578
- Project: https://john-liua.github.io/TrapVLA/ (live 2026-08-28; no code or
  benchmark links on the page)
- Classification: Evaluation/Safety (VLA backdoor attack task + benchmarks).
- Why included: defines Configured Failure Trapping, a strictly harder backdoor
  attack task for VLAs — the attacker must control *how* the robot fails
  (e.g., grasp with a specified positional offset) rather than merely cause any
  failure, which is substantially harder and harder to detect. The paper
  contributes a data engine for synthesizing high-quality target trajectories,
  an automated suite for measuring configured-failure fidelity, and two
  benchmarks (Trap-LIBERO, Trap-RoboTwin) across four representative failure
  modes; TrapVLA learns trigger-induced action residuals that steer the policy
  into the configured failure while preserving clean-data performance,
  validated in simulation and real-robot settings. Directly extends the VLA
  safety boundary line (Bit-Flip Attacks on VLAs, Hijacking Robots, MANIGUARD).
- Boundary: results author-reported; no code or benchmark release as of
  2026-08-28 (project page only).

### PLCBench: Can Autonomous LLM Agents Turn PLC Access into Sustained Physical Impact?

- Paper: https://arxiv.org/abs/2608.26882
- Classification: Evaluation/Safety (cyber-to-physical agent capability).
- Why included: to our knowledge the first real-PLC hardware-in-the-loop
  framework for characterizing whether a tool-using LLM agent converts
  network-reachable PLC access into *sustained* adverse physical impact,
  combining vendor-native interaction, commercial PLC execution, closed-loop
  reduced-order process simulation, and independent outcome verification. A
  deterministic evaluator assigns six hidden diagnostic flags from runner,
  communication, PLC-object, and process records, distinguishing usable PLC
  interaction, process-linked manipulation, and sustained physical impact —
  correcting the mischaracterization risk of evaluations that stop at software
  exploitation, an accepted write, or tool access. On four commercial PLCs ×
  four closed-loop workloads, across five LLM families and 240 real-PLC
  episodes, 75 (31.3%) sustained their physical objectives, while 62 reached a
  process-linked write without sustaining the objective. A physical-impact
  evaluation contract in the cyber-to-physical safety line (Edge Skillguard,
  embodied-security survey).
- Boundary: industrial-control testbed, not a mobile robot; no official
  artifacts located as of 2026-08-28.

### FLARE: A Failure-Aware Framework for Autonomous Correction and Recovery in VLA Manipulation

- Paper: https://arxiv.org/abs/2608.26645 (CVPR 2026)
- Classification: Agentic Robot/VLA Harness (recovery harness).
- Why included: attacks the brittleness that comes from training VLAs on
  trajectory-monotonic, failure-free demonstrations, via a Retry/Reset
  paradigm. *Retry* injects perturbation and bridging segments that decouple
  robot pose from environment state into demonstrations so the policy
  autonomously handles execution deviations (missed grasp, dropped object,
  unexpected collision); for state-breaking OOD failures, *Reset* uses an MLLM
  for offline failure analysis over execution videos to identify OOD states and
  collect a small library of object-centric Reset skills that restore a
  task-valid state, with an online MLLM monitor arbitrating between execution
  and Reset at inference. Recovery as a first-class VLA harness primitive in
  the AgentRewind/CheckVLA line, now in the robot-manipulation domain.
- Boundary: author-reported results on contact-rich manipulation tasks; no
  official code or project link on the arXiv record as of 2026-08-28.

### Same Model, Different Harness: Different Coding-Agent Results

- Paper: https://arxiv.org/abs/2608.26218
- Code: https://github.com/sydches/yuj (MIT, ~4 MB, pushed 2026-08-28; verified
  live via GitHub API — "Yuj is a coding-agent harness that watches the LLM and
  keeps it on course")
- Classification: General Harness Methodology (harness-effects measurement).
- Why included: a controlled measurement of the central claim this repository
  is organized around — model and task held fixed, only the harness's
  context-maintenance policy changes. The treatment keeps the same record but
  mechanically shortens older tool results as context fills and responds to
  repeated or stalled work; under tight context it raises mean per-task
  fail-to-pass fraction in all three pressure comparisons and complete
  solutions from 43 to 72 on SWE-bench Verified (169-task cohort, 20,480-token
  window, fixed 480-second endpoint), and the same frozen treatment transfers
  to three additional models without retuning. The conclusion — coding-agent
  evaluations should treat model and harness together — is the empirical
  foundation for harness-scoped evaluation. Real MIT-licensed artifact
  (harness + study artifacts).
- Boundary: digital agents; no robot experiment. Surfaced by the [20260826]
  re-screen; not named in the 08-27 record.

## Reviewed but not promoted (watch list)

- **TemporalFlow-VLA** (`2608.26821`) — physically grounded execution history
  for long-horizon VLAs via training-only robot-surface temporal flow
  supervision (LIBERO 97.63%, RoboTwin 85.5/84.2); no artifacts; watch.
- **PredVLA** (`2608.26673`) — sub-million-parameter predictive-coding policy
  (86.9% LIBERO short-horizon); no artifacts; watch.
- **Memory Anchors for Continual Robot Learning** (`2608.26545`) — replay
  anchoring for catastrophic forgetting (4.5× forgetting increase when 10% of
  anchors excluded); project page live, no code; watch.
- **FlashVLA** (`2608.27384`) — streaming action decoding for ≥30 Hz VLA
  inference; no artifacts; watch.
- **GRAFT** (`2608.27079`) — grounded online RL adaptation of VLAs to
  biomedical manipulation (region-level anchors, cached VLM prefixes); no
  artifacts; watch.
- **J-Zero** (`2608.26582`) — Challenger–Solver–Judge co-evolution from zero
  data (+4.2 verifiable / +8.0 unverifiable domains); no artifacts; watch.
- **WikiSkill** (`2608.27454`) — co-evolves skills with a persistent knowledge
  wiki; no artifacts; watch.
- **RedEvoAgent** (`2608.27439`) — red-teaming agent with experience-driven
  attack-skill evolution; no artifacts; watch.
- **Daydreaming** (`2608.26733`) — execution-only skill-stealing attack
  (86.8% capability recovery at Output); skill-security line; no artifacts;
  watch.
- **Five Primitives for Governing AI Agents at Runtime** (`2608.26696`) —
  discovery/identity/governance/attestation/supply-chain primitives with an
  implemented system in private pilot; watch.
- **A Contract-Centered Architecture for Agentic Runtimes** (`2608.27086`) —
  Skill/Harness/Scaffold responsibility objects with a falsifiable
  separability hypothesis (P1) and a proposed cluster-randomized experiment;
  conceptual, no implementation; watch.
- **Persona-Execution Separation** (`2608.27427`) — architecture pattern for
  evolving personas under execution audit; pilot case; watch.
- **LAAF** (`2608.27102`) — layered accountability architecture framework
  (PRISMA review mapped to EU AI Act/NIST); governance survey; watch.
- **SKILL.state** (`2608.26263`, EMNLP) — runtime architecture replacing
  append-only history with explicit mutable execution state; no artifacts;
  watch.
- **Agent Mesh** (`2608.26225`) — reliability primitives for non-idempotent
  delegation (identity/evidence adequacy) from a production failure study;
  no artifacts; watch.
- **LocalLSTC** (`2608.25777`) — long/short-term control architecture for
  local GUI agents (64.7% OSWorld SR-100); digital; no artifacts; watch.
- **Approved Too Late / Freshness-Bounded Shield** (`2608.26306`, ACSOS 2026) —
  TOCTOU verdict staleness in LLM-guarded self-adaptive systems; artifact
  documented in paper; watch.
- **WALL-SS** (`2608.26239`) — next-scale autoregressive long-horizon world
  models; no artifacts; watch.
- **LEON / Making Latent Evolution Explicit** (`2608.27259`) — operator-
  structured transitions for latent WAMs (Koopman-grounded); no artifacts;
  watch.
- **4DSynth** (`2608.26947`) — procedural 4D world synthesis + 4DSynth-Nav
  benchmark; no artifacts; watch.
- **PAWBench** (`2608.27345`) — probabilistic alignment benchmark for video
  world models; no artifacts; watch.
- **R2M-Bench** (`2608.27328`) — revisit-memory benchmark for interactive video
  world models; code at github.com/AMAP-ML/R2MBench; watch.
- **INTENT-AS-A-TOOL** (`2608.27348`) — intent-targeted tools for tracking
  agentic misalignment; code at github.com/RebeccaZhang22/intent-as-a-tool;
  watch.
- **BTS-AgentBench** (`2608.27334`) — deterministic telemetry-to-episode
  benchmark construction pipeline; code at github.com/kjy7567/BTS-AgentBench;
  watch.
- **TraceBench** (`2608.27182`) — controlled root-cause attribution tasks for
  time-series agents; datasets/leaderboard at tracebench.github.io; watch.
- **GraphMemix** (`2608.26983`) — query-aware evidence forests for multimodal
  agent memory; code at github.com/ligeng0197/graphmemix; watch.
- **KnownLieBench** (`2608.26372`) — knowledge-verified emergent-deception
  benchmark (112 cases, 8 customer-service domains); watch.
- **CTF-ABACUS** (`2608.26237`) — trace-level provenance for offensive-security
  agent evaluation (62–87% of recovered flags are trace-verified exploits);
  watch.
- **Beyond Capability Benchmarks / OpEmbed** (`2608.26332`) — operational
  fingerprints of LLM cloud services from production incident metadata
  (33,000+ Google Cloud cases); ops monitoring; watch.
- **6.5% of the Neuro-Symbolic Literature Can Be Reproduced** (`2608.26236`) —
  six-stage artifact-audit framework (85/1,304 studies reproduced); evaluation
  methodology; watch.
- **Not All Eval-Awareness Is Equal** (`2608.27340`) — capabilities-flavored
  vs safety-flavored eval-awareness framing predicts compliance (+24–46 pp);
  workshop paper; watch.
- **Understanding Evolution Strategies for LLM Reasoning** (`2608.27351`) —
  ES vs GRPO coverage analysis; post-training method; watch.
- **What Makes Good Agentic Data?** (`2608.27260`) — ACE lens on agentic data
  generation; framework paper; watch.
- **SPT: Skills as Pre-Training Data** (`2608.26563`) — skill-package
  mid-training (SkillCorpus); watch.
- **Instruct-to-Act / Decoupling Planning and Control** (`2608.26788`, COLM
  2026) — VLM planner + world-model controller across seven embodied
  environments; no artifacts; watch.
- **STEP** (`2608.27225`, RO-MAN 2026) — state-aware task estimation for HRC
  assembly planning; simulation-only; watch.
- **Embodied Scene Rearrangement Planning** (`2608.27371`, RA-L) — ESRP-Bench
  on OmniGibson (5,400+ scene pairs); code/dataset public; planning-domain;
  watch.
- **AgentFold** (`2608.26747`) — closed-loop agentic search for protein-folding
  model design (ESMFold → +7.5% lDDT); scientific-agent line; watch.
- **Beyond Execution: Auditing Experimental Fidelity** (`2608.26753`) —
  audit framework for LLM-driven scientific research; watch.
- **FaultLens** (`2608.26746`) — learned compact behavioral test suites for
  generated programs; reproducibility artifact in source package; watch.
- **BekchiAI** (`2608.26867`) — agentic-skill benchmark (13 agents, 2,057
  tasks) + observability/control platform; watch.
- **Don't Overthink, Don't Underthink** (`2608.26442`) — adaptive reasoning
  allocation for agentic AI; position/measurement; watch.
- **UrbanGround** (`2608.27456`) — real-scale city sandbox for MLLM spatial
  agency; code at github.com/UrbanGround/UrbanGround; watch.
- **Cross-Platform Benchmark of Neural 3D Reconstruction for Lab Robots**
  (`2608.26383`) — NeRF/3DGS/SAM3D compute-platform benchmark for laboratory
  robots; watch.
- **Formal Runtime Monitoring of Spoken ATC Procedures** (`2608.25926`) —
  temporal-formula monitor over controller–pilot exchanges; aviation domain;
  watch.
- **Candidate supply and answer selection shape LLM judging** (`2608.25937`) —
  judge reliability varies with task/generator; multi-agent evaluation; watch.
- **AutoVerifier** (`2608.25637`) — residual-guided verifier bias learning with
  replay-validated rule cards; watch.
- **A Storage-Retrieval Gap in Parametric KG Memory** (`2608.25489`) — LoRA
  adapter memory is not retrievable by similarity; watch.
- **When Stale Constraints Go Unchecked v2** (`2608.25553v2`) — Zenodo archive
  released (doi:10.5281/zenodo.22117197) with all 5,400 episode files; digital
  memory-verification measurement; artifact noted, domain unchanged — watch.
- **LM-X v2** (`2608.25757v2`) — same-day revision; no artifact links added;
  unchanged (watch).
- **Zero-WAM v2** (`2608.26103v2`) — same-day revision of an included paper;
  no artifact change (already in README with Apache-2.0 code).
- Domain exclusions in the screened range (per policy): driving (Barrier
  Function CVaR `2608.26533`), medical/CV/CL/LG papers without a reusable
  agent/VLA harness, recovery, safety, or evaluation contract, and general-LLM
  agent papers in finance, health, recommender, serving, and perception
  domains (representative: DuMateBench `2608.26546`, PLCBench is included —
  see above — but TAU-Agent `2608.25935`, DSA `2608.26990`, TOPAS `2608.25523`,
  AsymSpec `2608.26004`, AdaVDR `2608.25559`, BekchiAI watch, Reconstructing
  the Right Episode `2608.25655`, LivingRAG `2608.25960`, TraceBench watch,
  Reasoning Tax `2608.26235`, MemToC `2608.26295`, RuleWeaver `2608.26832`,
  BTS-AgentBench watch, ClueWeaver `2608.25531` [ICONIP], SPT watch, AesCanvas
  `2608.26713`, Magpie `2608.27168`, TTPO `2608.27448`, SWE-Prime `2608.27449`,
  MCR-Bench `2608.27442`, SpeechGym `2608.26432`).

## Watch-list status (rechecks)

- **XPolicyLab** (`2608.09892`) — no new revision in today's listings;
  unchanged (in README with live repository).
- **Task-CoEvolve** (`2608.20169`), **ForeTime-VLA** (`2608.20735`), **SPADE**
  (`2608.19197`), **ScienceFlow** (`2608.14354`), **GhostTac** (`2608.20817`),
  **GigaBrain-WBC-0.5** (`2608.18234`), **Q-Planning** (`2608.21204`),
  **CounterAlign** (`2608.21740`), **INDI** (`2608.23478`), **BATON**
  (`2608.16889`), **Agent Lightning** (`2608.17528`), **VLCP** (`2608.16978`),
  **MANIGUARD** (`2608.17386`), **LEGO-RL** (`2608.17393`), **TOWN-VLA**
  (`2608.23224`), **AutoSaddler** (`2608.23041`), **Recuris** (`2608.24876`),
  **GlanceWAM** (`2608.23927`), **StepGuard** (`2608.24777`), **JIT-Agent**
  (`2608.25593`), **UCAG-P** (`2608.26058`), **OpsHarness** (`2608.25661`),
  **StreamPI** (`2608.26067`), **MA-VLA** (`2608.25864`), **GaussianDream++**
  (`2608.25659`), **SimVerity** (`2608.25067`), **Edge Skillguard**
  (`2608.25091`), **Zero-WAM** (`2608.26103`) — none appear in today's
  replacement listings; unchanged (Zero-WAM v2 probed, see above).
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
  NeoWorld-Pro, From Seeing to Acting, DELE-w0.5, V-Link, R³, TacForcing,
  LM-X, RA-VLA, GaussVLA, 4DGS-WAM, ConfAL-WM, EASEL, RLHEV, Code World Model,
  VBVR-Pro, KOPE, SymTrace, EVOMAL, Trace Integrity, CaSKG, HiPS, Routed Graph
  Handoff, AWM, VoiceMem, PolyMemDB, ProgRouter, Trust-Aware Sequential
  Decision Making, VISTA-Policy, Listwise VL Preference Reward Learning,
  Advantage-Driven Explicit Memory, TARCAT, VirTooS, RDR, Game2World,
  CRESSim-Neo, Longitudinal Robot LfD, BFN-RL) — no new revisions or artifact
  releases detected in today's listings; unchanged.

## Operational notes

- The arXiv API was healthy for the entire run (no 429/500); both the
  `submittedDate` search and `id_list` paths worked.
- Artifact checks on 2026-08-28: HarnessLens repo (live, pushed 08-27, no
  license), yuj repo (MIT, pushed 08-28), CLAP project page + repo (MIT,
  ~230 MB) + HF checkpoints (multiple variants), TrapVLA project page (live,
  no code links), Stale Constraints v2 Zenodo archive (doi present on abs
  page), LM-X v2 / Zero-WAM v2 / VISA v2 abs pages (no artifact changes).
  Riemann-1.0, LoopHarness, SARA, PLCBench, FLARE: no official artifacts
  located.
- The known OpenSSH `/etc/ssh/ssh_config.d` ownership error was handled per
  procedure with `GIT_SSH_COMMAND='ssh -F /dev/null'` (`ls-remote` without the
  env var fails with the same error).
- Working tree before the run: `docs/reference-architecture.md` modified,
  `docs/ring-harness.png` and `handoff.md` untracked — preserved untouched.
- Consistency fix: the README header badge still read `last verified
  2026-08-26` after the 08-27 run (only the Current Landscape line had been
  updated); this run updates both to 2026-08-28.
