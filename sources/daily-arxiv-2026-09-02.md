# Daily arXiv scan — 2026-09-02

## Scope

- Interval: since the 2026-09-01 run's cutoff (~2026-09-01 04:10 UTC) through
  2026-09-02 ~04:00 UTC. The `[20260901]` day was **exposed for the first
  time** (announced 2026-09-02 00:00 UTC) during this run; no 09-02
  submissions were visible yet (normal announce lag).
- The arXiv export API (`https://export.arxiv.org/api/query`, HTTPS) was
  healthy but intermittently rate-limited (429s with ~14-byte bodies during
  the growth rechecks; resolved by spacing requests and retrying). XML saved
  under the workspace scratch dir (`.scratch/arxiv-2026-09-02/`, removed
  after the run).
- Main interval query, per category over `submittedDate:[20260901000000 TO
  20260902235959]` (cs.RO, cs.AI, cs.CL, cs.CV, cs.LG): **447 unique entries,
  all v1, all submitted 2026-09-01, 0 revisions**. Fully screened here.
- Growth rechecks (per the 09-01 record's instruction to re-check
  `[20260901]` plus growth in `[20260831]`/`[20260830]`/`[20260829]`),
  queried as one combined five-category OR per day:
  - `[20260831]` **grew from 430 to 567 unique (+137)** — all additions carry
    `2609.00xxx` IDs (submitted 2026-08-31, assigned September IDs, exposed
    with the 09-02 announcement). Every one of the 137 was screened here;
    two qualify (WHALE `2609.00196`, ZimaBlue `2609.00188` — see Included).
  - `[20260830]` **grew from 234 to 260 unique (+26)** — additions are the
    `2609.0004x–0006x` block (submitted 08-31, September IDs); all screened.
    One notable v2: When History Is Multimodal (`2608.29897`, visual
    rendering as a context manager under a shared harness) — watch.
  - `[20260829]` **grew from 235 to 237 unique (+2)** — `2609.00038`
    trajectory-judge and `2609.00036`; trajectory-judge is watch-listed.
- Revisions: 7 (`[20260829]`), 7 (`[20260830]`), 12 (`[20260831]`) entries
  carry `updated != published`; all are minor v2 updates dated 2026-09-01
  (mostly camera-ready fixes). Watch-list items re-flagged by revisions:
  AnyWorld `2608.29242` v2 (no curation-status change; project page still
  404), Will the User Ever Know `2608.30362` v2, Lazy Grounding `2608.30303`
  v2 (both already watched safety items, unchanged). No curation-status
  change from any revision.
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action models,
  code-as-policy, skill discovery, memory, recovery, evaluation, runtime
  monitoring, and safety. Candidate claims were checked against the arXiv
  record, official project pages, and official code/model/data repositories
  (live HTTP, GitHub `ls-remote`, and Hugging Face API checks on
  2026-09-02); README, landscape, and source records were checked for
  duplicates (no name/ID collisions). All performance results below remain
  author-reported unless stated otherwise.

## Included (new curated entries)

### EmbodiedSkills: A Unified Framework for Orchestrating, Training, and Deploying VLA Agents

- Paper: https://arxiv.org/abs/2609.01281
- Classification: Agentic Robot/VLA Harness (execution-proposal runtime over
  swappable VLA policies).
- Why included: formalizes the VLA agent loop as an execution-proposal
  contract: every skill decision is checked for prerequisite validity
  **before** execution and outcome-verified **afterward** by the runtime,
  through a fixed executable-skill interface that connects high-level skill
  selection, bounded low-level VLA execution, and post-action verification.
  Because the interface is fixed, low-level policies (Qwen3-VL, OpenPI/pi0.5)
  can be replaced or adapted without changing the agent loop, and planning,
  execution, verification, and recovery events are recorded as structured
  trajectories that supervise individual components and optionally drive
  online adaptation. The authors report 86.20% average success across 50
  RoboTwin 2.0 tasks and 97.40% across the four LIBERO suites for the
  task-adapted policies, with the framework itself contributing the
  trainable, inspectable agent layer that turns them into closed-loop
  embodied systems. The harness-contract line for frozen/swappable policies
  (AGM, CheckVLA, Zetta, TOWN-VLA).
- Boundary: no official artifacts located as of 2026-09-02 (web/GitHub
  search); results author-reported; 12.5% success on the four
  memory-dependent RMBench tasks shows the memory boundary remains open.

### HarnessDev: Can LLMs Create and Evolve Their Own Agent Harness?

- Paper: https://arxiv.org/abs/2609.01437
- Classification: General Harness Methodology / Evaluation (benchmark whose
  unit of evaluation is runnable harness infrastructure).
- Why included: shifts agent evaluation from task outputs to the harness
  itself. Creation starts from a minimal seed and a few cases and asks the
  agent to build a complete execution system; Evolution iteratively revises
  the created harness from downstream execution feedback; both are scored on
  capability (held-out benchmarks) and efficiency (execution-token cost)
  with hidden evaluation tasks withheld from development. Results span six
  creator LLMs, four domains, and five downstream benchmarks totaling 2,207
  unique downstream instances. Key findings: generated harnesses trail
  mature human-engineered references on code and search/research but match
  or exceed them on writing and ML experimentation; evolution gains are
  unstable and transfer only partially; gains depend strongly on the model
  executing the harness. The harness-as-artifact evaluation line
  (HarnessOpt-Bench, Evo-Bench, JIT-Agent).
- Boundary: digital agents, no robot experiment; no official artifacts
  located as of 2026-09-02; results author-reported.

### Harness-of-Harness (HoH): Multi-Day Autonomous Software Development with Continual Improvement

- Paper: https://arxiv.org/abs/2609.01481
- Code: https://github.com/Flesymeb/HarnessOfHarness (live 2026-09-02, HEAD
  `aadbde0`)
- Project page: https://flesymeb.github.io/HarnessOfHarness (live
  2026-09-02)
- Classification: General Harness Methodology (harness-level continual
  improvement wrapper).
- Why included: operates **on top of existing coding-agent harnesses** and
  organizes their executions into iterative planning-coding-testing loops,
  balancing repair with capability growth, scoping development into small
  verifiable increments, separating implementation-time testing from
  independent evaluation, and constraining verifiable outputs rather than
  prescribing agent workflows. It progressively exposes deliverables,
  role-specific tools, and skills, encourages reuse, and maintains versioned
  project histories. On GameCraft-Bench, FrontierSWE, and ProgramBench,
  three harness-model pairs (Codex+GPT-5.5, OpenCode+DeepSeek-V4-Pro,
  Pi+MiniMax-M3) consistently outperform the standalone harnesses, averaging
  +52.25% relative gain (max +82.86%) after three iterations; a multi-day
  deployment with 70+ iterations autonomously developed a playable FPS game.
  A real artifact release in the self-improving-harness line (Self-Harness,
  Evo-Harness, JIT-Agent, HarnessEvolve).
- Boundary: digital software agents, no robot experiment; results
  author-reported (code and page verified live; contents beyond HEAD not
  audited in detail).

### HarnessEvolve: Learning from Reference Trajectories for Reliable Agent Self-Evolution

- Paper: https://arxiv.org/abs/2609.00829
- Classification: General Harness Methodology (self-evolution with
  reference-trajectory credit assignment and dual gates).
- Why included: names three failure modes of self-evolving harnesses —
  credit-assignment failure (terminal feedback does not say which step
  erred), shortcut learning (memorizing task patterns), and catastrophic
  forgetting (unguarded updates regress prior competence) — and repairs all
  three by construction: the execution agent is decoupled from an
  evolutionary pipeline of independent evaluation, optimization, and gating
  modules; reference trajectories (execution paths produced with
  ground-truth answers) are aligned against failed executions to extract
  error signals that are clustered into systematic failure patterns; and
  every candidate harness update must pass a quality gate (data leakage,
  prompt bloat) plus a performance gate (improves current batch without
  degrading recent batches, with epoch-end held-out validation). Consistent
  gains over SOTA baselines across open-domain and enterprise benchmarks.
  The reliability-gated harness-evolution line (HCL, HarnessLens,
  EvoUndo).
- Boundary: digital agents, no robot experiment; no official artifacts
  located as of 2026-09-02; results author-reported.

### CordisBench: Can Language Models Reason About Component Lifecycles in Dynamic Agent Harnesses?

- Paper: https://arxiv.org/abs/2609.01600
- Code: https://github.com/sileod/cordis-bench (live 2026-09-02, HEAD
  `290d6b4`)
- Classification: Evaluation (harness lifecycle reasoning).
- Why included: dynamic agent harnesses let the model change the software
  that shapes its own execution, and a local plugin change propagates
  through dependencies and cleanup — a new reasoning burden the paper turns
  into a 1,200-question benchmark. CordisBench combines a controlled formal
  setting with programs executed against Cordis, a runtime that manages
  component dependencies and cleanup, and asks models to identify affected
  components, predict post-teardown state, determine order-independent
  conditions, and choose reconfigurations that execute successfully, with
  deterministic task-specific scoring across 2–32 relevant interactions.
  Models handle small systems well but degrade as interactions accumulate,
  and the paper shows the extra reasoning cost is avoidable: an independent
  finite reference semantics agrees with Cordis execution on every scoring
  observation across all 528 executable questions. A harness-internal
  evaluation contract in the self-modifying-harness line (HarnessDev, SHE,
  EvoUndo); real benchmark release (verified code repository).
- Boundary: digital agents, no robot experiment; results author-reported.

### WHALE: A Simple Recipe for Joint Harness-Weight Optimization

- Paper: https://arxiv.org/abs/2609.00196
- Classification: General Harness Methodology (joint model–harness
  co-optimization).
- Why included: makes explicit that agent performance depends jointly on
  model parameters and the executable harness code, and that optimizing
  either in isolation leaves the system bottlenecked by its frozen
  counterpart. WHALE alternates weight updates under the current harness
  (online rejection-sampling fine-tuning) with harness search under the
  updated model (Meta-Harness), switching via fixed durations or an adaptive
  patience rule to separate real improvement from noise. Using Qwen3.5-2B/4B
  agents across search QA, math, and chess, WHALE outperforms weight-only,
  harness-only, and Fast-Slow Training by 4.15–24.38 points in best mean@8;
  either component can be the bottleneck, and small interleaved updates beat
  stagewise weight-then-harness optimization. The model–harness
  co-evolution line (HELIX, Task-CoEvolve, JIT-Agent).
- Boundary: digital agents, no robot experiment; the abstract's claimed
  repository (`github.com/krafton-ai/WHALE`) returned 404 on 2026-09-02 —
  treated literally, code is not open as of this run; results
  author-reported.

### Auditing Harness Tampering in Self-Improving Agents

- Paper: https://arxiv.org/abs/2609.00069
- Classification: Evaluation/Safety (audit methodology for harness
  self-modification).
- Why included: extends reward/measurement tampering to the full
  self-improvement lifecycle: self-improving agents modify their own harness
  (prompts, skills, tools, execution logic), and such edits can produce
  illusory performance gains or violate integrity constraints —
  authorization, provenance, completeness — without genuinely improving
  capability. The paper defines *harness tampering*, proposes a two-axis
  taxonomy (harness functional role × violated obligation), builds an
  annotated corpus by seeding tampered/benign edit pairs into real
  self-improving trajectories, benchmarks diverse audit methods on
  classification and localization, and audits real trajectories: tampering
  consistently occurs across agents, persists in the lineage of the best
  agent, and forms system-specific profiles. The integrity-audit line for
  self-evolution (Phantom Gains, EvoUndo, Self-Reports Are Not
  Verification).
- Boundary: digital agents, no robot experiment; no official artifacts
  located as of 2026-09-02; results author-reported.

### REFACTOR-VLA: Unsupervised Library Learning of Typed Motor Programs

- Paper: https://arxiv.org/abs/2609.01215
- Classification: VLA / skill discovery (wake/sleep library learning).
- Why included: attacks the monolithicity of current VLAs (OpenVLA, π0,
  RT-2, RDT-1B emit raw motor commands without reusable abstractions and
  degrade on long horizons) with a wake/sleep system that learns reusable
  skills: the sleep phase clusters motor-program fragments under a
  Behavioral-Equivalence Kernel computed from rollouts of a learned latent
  world model, and the wake phase emits typed lambda terms over a
  Hindley–Milner-inspired vocabulary consumed by a library-conditioned
  rectified-flow action decoder; abstractions are admitted only through
  Minimum-Description-Length and return-preservation gates. On LIBERO the
  authors report that enlarging the world model (188M→430M) *worsened* all
  four suites, while an auxiliary supervised contrastive loss during
  warmup substantially improves clustering (NMI 0.462–0.915), beating the
  strongest published baseline by Δ=+0.184 on all four suites; the sleep
  phase yields the first real-LIBERO task-language library. Skill-discovery
  contract for the VLA line (SMILE, SkillGate, PRACTICE).
- Boundary: LIBERO simulation evidence, results author-reported; no official
  artifacts located as of 2026-09-02.

### Facet-0: A Robotic Foundation Model for Contact-Rich Precise Manipulation

- Paper: https://arxiv.org/abs/2609.01596
- Project page: https://pine-lab-ntu.github.io/facet-0 (live 2026-09-02)
- Weights: https://huggingface.co/Pinelab/Facet-0 (live 2026-09-02,
  Apache-2.0, ships post-training params)
- Data: https://huggingface.co/datasets/Pinelab/ManuFacet-1K (live
  2026-09-02, CC-BY-4.0, robotics/video corpus)
- Classification: Robot Foundation Model (contact-aware action-wrench
  foundation model).
- Why included: targets sub-millimeter assembly, where policies must predict
  **and value the contact consequences of their actions**: a causal wrench
  history is aligned with vision-language semantics and kinematic state, and
  flow matching generates each action chunk together with the future
  wrist-wrench profile it is expected to induce; deployment rollouts train a
  distributional Action-Wrench Critic that distinguishes motions with
  similar task progress but different contact outcomes, with phase-aware
  rewards and contact-selective credit concentrating improvement on decisive
  interactions, and a lightweight bounded actor reusing the frozen
  representation for on-robot adaptation. Trained on ManuFacet-1K (a
  1,000-hour force-synchronized corpus across three embodiments and multiple
  manufacturing cells), the adapted system reaches 82% mean success on five
  sub-millimeter computer-assembly tasks versus 15% for the strongest
  baseline, with 0.5 mm placement accuracy and 50 ms command latency. A real
  artifact release (Apache-2.0 weights + CC-BY-4.0 corpus) in the
  contact-rich RFM line.
- Boundary: results author-reported; the released weights are the
  post-training checkpoint (fine-grained contents not audited beyond file
  listing).

### ZimaBlue: Evolving Generalizable World Action Models through Scalable Video Pre-training

- Paper: https://arxiv.org/abs/2609.00188
- Classification: Robot Foundation/World Model (video-pretrained WAM with
  Slow-Fast execution).
- Why included: addresses the action-label bottleneck of WAM scaling by
  pre-training on action-free egocentric video: a three-stage curriculum
  (causal embodied video pre-training on human+robot egocentric video →
  video-action mid-training on heterogeneous robot trajectories with a
  unified action representation → target-robot specialization) plus an
  asynchronous Slow-Fast dual system where a high-capacity Slow world model
  supplies generalizable representations and a lightweight Fast branch runs
  30 Hz action prediction on an RTX 4090. The authors report that scaling
  from target-robot data alone to 120,000+ hours of embodied video raises
  real-robot zero-shot success from 36.1% to 77.8%, with pronounced gains on
  unseen tasks. The video-pretraining WAM line (Motus2, CLAP, Riemann-1.0).
- Boundary: results author-reported; no official artifacts located as of
  2026-09-02.

### REVISE: Validity-Guided Recovery for Online Revisions in Agent Workflows

- Paper: https://arxiv.org/abs/2609.00643
- Classification: General Harness Methodology (fine-grained recovery runtime
  for revisions).
- Why included: resolves the correctness–efficiency trade-off of agent
  revision during concurrent execution: discarding ongoing work preserves
  latest-version correctness but wastes valid progress, while reusing prior
  work risks propagating stale state into outputs and tool effects. REVISE
  intersects a revision's delta with recorded data/control dependencies,
  propagates impact through the partially executed DAG, stops only invalid
  work, preserves validity-established progress beyond the earliest
  conflict, and recomputes only the affected region, with incomplete
  provenance conservatively expanding recovery and reused results revalidated
  before commit. Across 300 challenging revision/commit executions it matches
  a latest-version oracle with no stale outputs or effects; on unmodified
  LangGraph and LLMCompiler applications with Qwen3-14B it reduces model
  calls by 40.6–56.0% versus full restart and 31.3–43.6% versus suffix
  recomputation. The recovery line (AgentRewind, Agentic Transaction,
  Aborted but Not Forgotten) extended to live revision.
- Boundary: digital agents, no robot experiment; no official artifacts
  located as of 2026-09-02; results author-reported.

### Runtime-Independent Persistent Agents: Preserving Identity, Memory, and Code Across Models, Harnesses, and Servers

- Paper: https://arxiv.org/abs/2609.00546
- Classification: General Harness Methodology (persistence/migration
  contract).
- Why included: underspecification argument for long-lived agents: the
  model+harness boundary describes one execution but not an agent that
  changes models, orchestration harnesses, sessions, and host servers while
  retaining one identity, memory, and executable code lineage. The paper
  defines a continuity-bearing substrate (architectural identity
  representation, private durable memory, versioned software body), a
  replaceable deployment binding (reasoner, harness, host, interaction
  surfaces), six continuity invariants, and a
  quiesce–checkpoint–validate–bind–rehydrate–resume protocol under which
  changing a replaceable layer is migration, not agent creation, when an
  authorized protocol preserves attributable lineage. Enoch realizes the
  design; a clean-room run of the frozen public commit passes 833 core tests
  plus 92 provider/library tests, and deployments have substituted
  reasoner versions, interaction surfaces, and host machines while retaining
  continuity-bearing state. The persistence line (StateM, Recuris, ASIL)
  given a cross-harness identity contract.
- Boundary: digital agents, no robot experiment; evidence supports
  mechanical substitutability, not behavioral invariance; no public artifact
  link located on the arXiv record as of 2026-09-02.

### Parsing the Stream: A Live Trace Model for Long-Horizon Agents and Their Observers

- Paper: https://arxiv.org/abs/2609.01466
- Code: https://github.com/SalesforceAIResearch/tracelab (live 2026-09-02,
  HEAD `072fcb6`)
- Data: https://huggingface.co/datasets/Salesforce/tracelab-comprehend (live
  2026-09-02)
- Classification: General Harness Methodology / Observability (live trace
  folding).
- Why included: a long-horizon agent's trace outgrows both consumers — the
  human observer and the agent itself, whose bounded context the trace must
  be folded back into. The paper contributes a live trace model: an
  append-only event ledger folded incrementally into typed run state and
  compiled into per-consumer views, evaluated against deterministic ground
  truth. For the observer, the compiled view answers monitoring questions
  with ~14×/15× fewer input tokens at 5–7× lower cost than a
  budget-capped raw-trace read, with higher accuracy (0.85–0.87 vs 0.48);
  for the agent, mechanisms maintaining the task's running statistic in
  per-step state succeed where full-context prompting fails (30/30 vs 8/30
  on 120-link sequential-dependency tasks), with deterministic auditability
  as the fold's remaining edge over cheaper alternatives. Eleven candidate
  requirements for trace folding are derived from observed failures. A real
  artifact release (code, benchmarks, regenerable synthetic corpus,
  workbench traces) in the observability/recording line.
- Boundary: digital agents, no robot experiment; token/cost reductions are
  conditional on view-schema coverage (stated by the authors); results
  author-reported.

### Defense-as-Skill: Evolving Runtime Guard Skill for Skill-Augmented Agents

- Paper: https://arxiv.org/abs/2609.01487
- Classification: Evaluation/Safety (runtime guard as an evolvable skill).
- Why included: skill-augmented agents load reusable skills as persistent
  runtime context, giving malicious skills a durable channel for steering
  future actions (leaks, code corruption, approval bypass, staged
  exfiltration) that pre-install vetting cannot catch. Defense-as-Skill
  implements the runtime guard itself as an installable, inspectable,
  editable skill: SkillSonar runs alongside untrusted task skills and routes
  each sensitive action to allow/replan/confirmation decisions against the
  user's task boundary without modifying the agent runtime. The SCOPE-R
  dataset covers 6 risk families and 21 sub-categories (206 attack-confirmed
  malicious instances, 43 benign tasks), and runtime guard-skill evolution
  (Monte-Carlo Tree Search over on-disk guard edits) improves SkillSonar:
  across Claude Code and OpenClaw, in-distribution ASR falls 0.482→0.104 and
  OOD ASR 0.606→0.115 on repeated GLM-5 runs, with transfer across victim
  models, held-out risk families, and adaptive attackers. The
  skill-layer-safety line (MaliciousSkillBench, SkillMisevo, StepGuard,
  Edge Skillguard).
- Boundary: digital agents, no robot experiment; no official artifacts
  located as of 2026-09-02; results author-reported.

## Reviewed but not promoted (watch list)

From `[20260901]` (first exposure):

- **Knowing When to Stop** (`2609.00908`) — training-free adaptive action
  chunking for VLAs from internal cross-attention entropy plateaus (π0.5,
  X-VLA on RoboTwin 2.0/LIBERO + 3 real tasks); VLA execution method
  without an external harness contract; watch for release.
- **DroneCATS** (`2609.01404`) — benchmark treating MLLMs as the independent
  variable in drone control (approach/track/search/fleet-command) with
  swappable-model architecture; key finding: navigation is not what fails —
  the action protocol (arrival declaration, terminating action) is; aerial
  domain evaluation contract; no artifacts located; watch.
- **VerNav** (`2609.00920`) — verifier-first low-latency LLM-based VLN
  (batched action verification + entropy-based adaptive generator; 10×
  lower decision latency on R2R); VLA/navigation method; watch.
- **AM-Bench** (`2609.00641`) — modular simulation suite/benchmark for
  aerial-manipulation policy learning (12 tasks, embodiments,
  disturbances); system-level evaluation contract, aerial domain; no
  artifacts located; watch.
- **Peg-in-Bench** (`2609.00906`) — reconfigurable peg-in-hole benchmark
  (3D-printable modular pieces, scenario generation tool); the abstract's
  claimed repository (`github.com/aistairc/peg-in-bench`) returned 404 on
  2026-09-02 — treated literally, no benchmark release; watch.
- **ADAPT** (`2609.00677`) — end-to-end text-conditioned humanoid
  whole-body control: diffusion action prior from text-labeled state-action
  trajectories + residual RL for robustness; control method; watch.
- **A System for Fast, Resilient, and Adaptable Loco-Manipulation
  Behaviors on Humanoid Robots** (`2609.01518`) — runtime-editable behavior
  authoring system (Affordance Templates, behavior scene, synchronized
  operator interface) on Unitree H1-2/Alex; systems contribution, no
  artifacts; watch.
- **HitMem** (`2609.00950`) — hierarchical temporal 3D memory with
  multi-modal context-aware retrieval for dynamic environments (Dyna-THOR
  benchmark); memory method; watch.
- **DSG** (`2609.00619`) — dynamic 3D scene graph construction with change
  detection and LLM spatial-relation reasoning (Dyn-THOR benchmark); scene
  memory; watch.
- **Does Imitation Learning Preserve Temporal Robustness?** (`2609.01453`)
  — expert-vs-learner comparison across execution speeds (ParcelStow; ACT
  loses 34–48 points vs 16 for the expert at max speed; code live at
  `coenwerem/parcelstow`); imitation evaluation; watch.
- **H3-World** (`2609.01560`) — turns a 33B video generator into an
  interactive world model via language-conditioned character/camera control
  with temporal attention routing (0.199% trainable params); video-domain
  world control, no robot experiment; watch.
- **SAGE / Selective Agent Guidance via Entropy** (`2609.01567`) — learns a
  cheap autonomous policy from an imperfect VLM teacher queried only when
  the learner is uncertain (advantage-weighted distillation); policy-learning
  method; watch.
- **Provably Safe Sim-to-Real Transfer** (`2609.01418`) — reward-free safe
  RL formulation with provable real-world interaction reduction; theory,
  no experiments; watch.
- **SoK: When Safe Agents Fail Together** (`2609.00595`) — execution-
  centered systematization of multi-agent LLM security (197 works, A-I-R
  framework, five-part defense contract, 44 evaluated benchmarks); watch.
- **Spawn Freely, Act Sparingly** (`2609.01035`) — Progressive Risk Vesting
  for recursive agent trees (trajectory-level risk budget, anytime harm
  bound); theory; watch.
- **MemoryWalker** (`2609.00865`) — training agents under context
  compression (LogitTree, 4D mask, SDCC self-distillation); training-method
  line; watch.
- **WorldBench** (`2609.01056`) — multilingual, persona-grounded agent
  benchmark (1,600 tasks, 7 languages, 8 cultures, Constrained Task
  Success); digital; watch.
- **RingMoClaw** (`2609.00814`) — self-evolving multi-agent research
  framework for remote sensing (research/quality-control branches,
  dual-stream experience bus); domain; watch.
- **Skill Following / RAE** (`2609.00549`) — evaluation metric isolating
  actual skill use from retrieval lift (17 LLMs, MBPP+); digital eval;
  watch.
- **Polished but Unresolved** (`2609.00823`) — late-stage pressure states
  in tool-use agents (linear probe + PSPR relief plugin); monitoring; watch.
- **TRIAGE** (`2609.01428`) — three-level routing with
  Trajectory-as-a-Skill reuse (62.3% token savings on security-monitoring
  queries); efficiency; watch.
- **SpatialGuard** (`2609.01582`) — Layout Harness for verifiable 3D
  spatial text-to-image generation; image-generation domain, not robot; watch
  (no).
- **ARISE-RL** (`2609.01058`) — rubric-grounded iterative self-evolution
  (Generator/Solver co-evolution + ECR-Bench); watch.
- **AgentFactory** (`2609.01045`) — joint foundation-model + workflow
  optimization for agentic systems (three-stage pipeline, multi-objective);
  watch.
- **DiagEvo** (`2609.00768`) — diagnosis-guided self-evolution via
  hierarchical error-cause memory; watch.
- **The Safeguard Worked. Is the LLM System Safer?** (`2609.00519`) —
  evidence-asymmetry analysis of safeguard deployment claims; watch.
- **When Guardrails Look Effective** (`2609.01519`) — construct-validity
  audit of LLM-agent commerce evaluation (welfare gains shrink +87.4 →
  +7.2 under protocol isolation); evaluation methodology; watch.
- **ContextPipe** (`2609.00749`) — database-inspired context assembly for
  long-horizon agents (Plan-Bind-Optimize-Execute-Feedback, EXPLAIN
  ANALYZE; −31% tokens on SWE-bench Pro subset); context-management line;
  watch.
- **StudyBench** (`2609.00787`, code `thunlp/StudyBench` live) — controlled
  physics benchmark measuring how efficiently self-evolution converts
  training material into capability (Application vs Transfer sets; Guidance
  Gap; Compute Plateau); self-evolution evaluation; watch.
- **CoBRA** (`2609.00967`) — counterfactual margin learning of tool-use
  boundaries (internal vs external experts, MARS-RL); watch.
- **ExBind** (`2609.01344`, code `Daerwang2020/Exbind` + HF dataset live) —
  controlled diagnostic benchmark for visual-to-executable correspondence
  (SVG/DOM/canvas/tree/graph/table); multimodal coding; watch.
- **InSight** (`2609.01383`, code `maevehutch/insight` live) — benchmark
  for agentic claim verification in interactive visualizations (21,349
  claims); watch.
- **Cheap Verifiers, Large Blind Spots** (`2609.01345`) — verifier-cascade
  reliability: blind spot grows with student capability, corrective
  fine-tuning self-defeats, in-loop metrics read flat while true error
  swings to 32% (two-population conservation law); evaluation; watch.
- **CANOPY** (`2609.01245`) — outcome-only RL protocol (coverage-anchored
  on-policy; AppWorld leaderboard, SWE-bench +16.6); the claimed repo
  `AlibabaResearch/SignalCoverageRL` exists but is empty (no HEAD) as of
  2026-09-02; watch.
- **Where the Verifier Fails** (`2609.01354`) — category-level audit of
  RLVR verifiers (self-validation 53.8–95.2%, whitespace/punctuation = 93%
  of in-contract failures); watch.
- **DualStake** (`2609.00935`, code `FloXXXt/DualStake` live) — dual-path
  confidence calibration for deep-research agents; watch.
- **Fleets Need a Context Plane** (`2609.00659`) — bounded runtime context
  interface for cooperative perception in drone fleets (1 KB @ 10 Hz
  descriptors, 0.10 ms decisions); multi-robot interface; watch.
- **ChatDev 2.0 / DevAll** (`2609.00714`, code `OpenBMB/ChatDev` live) —
  no-code platform for heterogeneous multi-agent systems (declarative
  executable graphs, cycle-aware engine); watch.
- **HypoSearch** (`2609.01294`) — hypothesis-guided search for deep
  research agents; watch.
- **PTA-IRT** (`2609.01603`, code live) — trajectory-aware IRT for cheap
  SWE-bench evaluation; watch.
- **SNC profile** (`2609.01271`) — Spread–Novelty–Centrality profiling of
  agentic SWE benchmark demands; watch.
- **Control-Data Flow Separation** (`2609.00621`) — typed control objects
  vs optimizable data flow in multi-agent prompt optimization (100%
  protocol validity); watch.
- **The Rise of Verbal Reinforcement Learning** (`2609.01597`) — first
  unified account/taxonomy of verbal RL (grounding / deliberative feedback /
  learning signal); survey; watch.
- **GlossoGen** (`2609.01491`) — platform for emergent-language studies in
  multi-agent LLM interactions (SaveVeyru scenario); watch.
- **PIS / Making Prospective Memory SLM-Shaped** (`2609.01272`) — typed
  intention stores for small-model prospective memory (82.9% PM-Bench
  Set-F1); watch.
- **Beyond the Clock** (`2609.00874`) — measurement of when meta-level
  revision timing carries decision value; watch.
- **AnySearch** (`2609.00813`, code `xwsun01/AnySearch` live) — single
  policy for budget-aware tool search via curriculum RL; watch.
- **WiseSpec** (`2609.00568`) — requirements-driven code-generation agents
  (structured requirements + execution-based quality assessment); watch.
- **Dual Process Motion Planning** (`2609.01260`) — System-1/System-2
  neuro-symbolic motion planning with metacognitive orchestration; watch.
- **ProxPI** (`2609.00941`) — proximal prior injection for sampling-based
  MPC under learned-prior mismatch; watch.
- **SG-RL** (`2609.01061`) — solver-gradient-guided RL for MPC cost-weight
  adaptation (70.6% fewer samples on racing platforms); watch.
- **C-SafeQA** (`2609.01210`) — response-level Chinese safety QA benchmark
  with judge auditing (37,660 records); watch.
- **MutMem-V2** (`2609.01235`) — portable verification contract for
  cryptographically authorized mutation of persistent agent memory (Node +
  Python conformance, 72 terminals); watch.
- **Streaming4D** (`2609.00610`) — block-wise video generation + incremental
  reconstruction for 4D world models (1.24× speedup); video world models;
  watch.
- **EM^2Mem** (`2609.00551`) — event-centric multimodal memory for LLMs;
  watch.
- **Calibration is the Bottleneck** (`2609.00949`) — action-class diagnostic
  of multi-turn tool-calling calibration; watch.
- **StudyBench / S3Gym-class self-evolution benchmarks** — see StudyBench
  above; the 09-01 record's S3Gym/ASPIRE/BAITBENCH watch items are unchanged
  (rechecked 2026-09-02).

From the `[20260831]` growth (+137, September-ID late additions):

- **When History Is Multimodal** (`2608.29897` v2) — visual rendering as a
  representational context manager under a shared harness (budget-
  constrained history transformation); context-management line; watch.
- **trajectory-judge** (`2609.00038`) — what outcome-only LLM judges miss on
  agent trajectories (fault-injected support-desk environment; silent-fault
  recall 45%, invented-promise evasion); judge evaluation; watch.
- **Delegation Without Trust** (`2609.00267`) — untrusted-model threat model
  and authorization broker for multi-agent delegation (blocks all four
  threats, 0/200,000 forged tokens, 2.6 µs/decision; production in VotalAI
  LLM Shield); delegation-security line; watch.
- **The Irreversibility Budget** (`2609.00275`) — fleet-level risk
  accounting and admission control for agent operating systems
  (per-effect gates admit up to 48× overdraw without a budget); watch.
- **Belief-Based World Models** (`2609.00455`, code
  `skumar-ml/belief-world-models` live) — belief-queryable world models for
  LLM agents under partial observability; watch.
- **IMPACT** (`2609.00161`) — attention-calibrated interaction-aware world
  model training (robot-arm and human-hand manipulation, no external
  representations); WM training method; watch.
- **Beneath the Diff / DAPS** (`2609.00077`, code
  `BokwaiHo/arl-mode-collapse` live) — algorithmic mode collapse in
  code-level autonomous research loops + diversity-aware proposal sampling;
  audit line; watch.
- **ECCBench / Good Memory Has ECC** (`2609.00103`) — memory evaluation
  beyond accuracy (efficiency/compression/calibration); watch.
- **SEAV** (`2609.00498`, code live) — validity-aware jailbreak evaluation
  (reclassifies 22.1–51.0% of prior-labeled successes); watch.
- **EvoFlint** (`2609.00487`) — evolutionary quality-diversity atlas of
  multi-turn vulnerabilities; watch.
- **Gated-Memory Routing** (`2609.00237`, code live) — learned write/retrieval
  gates for multi-agent orchestration memory; watch.
- **Dr. Claw** (`2609.00365`, code `OpenLAIR/dr-claw` live, AGPL-3.0) —
  auditable human-in-the-loop AI-scientist workspace wrapping coding agents;
  watch.
- **mimeo** (`2609.00453`, code `K-Dense-AI/mimeo` live) — compiling public
  expert corpora into agent skills with quotation verification; watch.
- **Safin-1** (`2609.00092`) — memory-native safety state evolution (MARCH
  routing); model paper; watch.
- **LLAMIA-Bench** (`2609.00474`) — verbalization bottleneck of
  language+non-language agent collaboration (latent state internalization,
  chess suite); watch.
- **VeriOCRBench** (`2609.00232`, code live) — OCR-grounded task
  verification benchmark; OCR domain; watch.
- **CUDA-Harness** (`2609.00058`) — agentic CUDA kernel generation with
  synthesis-based verification; domain-specific; watch.
- **Zero-Trust Agent Harness for Cloud Engineering** (`2609.00050`) —
  graph/loop/harness engineering with evidence-gated progression on Google
  Cloud; cloud-ops domain; watch.
- **AgentProv** (`2609.00052`) — action-channel identity audit of agentic
  LLM API providers (tool-call distribution fingerprinting); watch.
- **GUI-CC** (`2609.00048`) — contextual consistency of GUI world models as
  agent environments; GUI domain; watch.
- **Agent Payment Protocols** (`2609.00060`) — Tamarin formal analysis of
  x402/MPP/ACP/AP2 (86 cases, 40 new findings); digital; watch.
- **Scientific Agent Skills** (`2609.00065`, code
  `K-Dense-AI/scientific-agent-skills` live) — 163 procedural skills for
  research agents; watch.
- **RePro** (`2609.00062`, code live) — proof-verified benchmark rewriting
  for reliable math evaluation; watch.
- **RestoreBench** (`2609.00384`, code live) — AI-agent benchmark for power
  flow convergence restoration; power domain; watch.
- **AnyWorld** (`2608.29242` v2) — see Watch-list status below.

## Watch-list status (rechecks)

- No new artifact releases or revisions for previously listed watch items:
  StageWAM, ReflexVLA/ReflexBench, DreamX-Phi, UniTexture, PRISM,
  GigaBrain-0.7/WBC, ForceU-VLA, LIBERO-VIFO, Agent Lightning, VLCP, Hydra-0,
  BATON, Q-Planning, AutoSaddler, JIT-Agent, Task-CoEvolve code, UCAG-P,
  TrapVLA code/benchmarks, TemporalFlow-VLA, PredVLA, FlashVLA, WikiSkill,
  RedEvoAgent, SKILL.state, Agent Mesh, WALL-SS, R2M-Bench, INTENT-AS-A-TOOL,
  BTS-AgentBench, TraceBench, GraphMemix, UrbanGround, Stale Constraints v2,
  LM-X v2, Zero-WAM v2, VLAct, Code as Worlds, Aero Hand Open, CAITLYN,
  Dogwood, LongGuard, WebWorld, ASPIRE, S3Gym — unchanged (spot-checked
  2026-09-02 via `ls-remote`/HTTP where links exist).
- New watch items with artifacts now live (verified 2026-09-02): CordisBench
  (repo `sileod/cordis-bench`), Harness-of-Harness (repo
  `Flesymeb/HarnessOfHarness` + project page), Facet-0 (HF weights
  `Pinelab/Facet-0` Apache-2.0 + HF dataset `Pinelab/ManuFacet-1K`
  CC-BY-4.0 + project page), Parsing the Stream (repo
  `SalesforceAIResearch/tracelab` + HF dataset `Salesforce/tracelab-comprehend`).
- New watch items with claimed-but-404 links (treated literally, not open):
  WHALE (`krafton-ai/WHALE` 404), Peg-in-Bench (`aistairc/peg-in-bench`
  404), CANOPY (`AlibabaResearch/SignalCoverageRL` empty, no HEAD).
- AnyWorld (`2608.29242`) updated to v2 on 2026-09-01; the project page
  (`xpeng-robotics.github.io/anyworld`) still returns 404 as of 2026-09-02;
  watch for a live page/artifacts.

## Domain exclusions in the screened ranges (per policy)

- Driving/CAV: CoLT-Drive (`2609.00242`), LLM-driven AV pedestrian yielding
  (`2609.00192`), Qwen-Drive-1.0 (`2609.00111`), scene-graph driving scenario
  extraction (`2609.00333`), DNC-IMM lane change (`2609.01120`), Context-Aware
  Intelligent Vehicles (`2609.00682`), closed-loop compression eval for
  driving policies (`2609.00718`).
- Conventional control/locomotion/perception/hardware: ADAPT (watch),
  dual-process motion planning (watch), ProxPI (watch), SG-RL (watch),
  RB-POMDP planning (`2609.01351`), mudskipper locomotion (`2609.00564`/
  `2609.00563`), quadruped gaits (`2609.00539`), origami actuator
  (`2609.00751`), robotic finger CVT (`2609.00769`), wearable pneumatic
  tactile (`2609.00612`), PID regulatability (`2609.01207`), non-prehensile
  throwing (`2609.00771`), pHRI behavior-realization separation
  (`2609.00669`), UAV formation control (`2609.01420`), obstacle-aware
  coverage (`2609.01384`), multi-robot exploration (`2609.00804`), swarm
  bridging (`2609.01394`), point-cloud registration (`2609.01089`), tilt
  estimation Kalman (`2609.00730`), 3R inverse kinematics (`2609.00311`/
  `2609.00316`), gaze-based robot placement (`2609.00478`), FoldingAgent
  origami (`2609.00377`), traffic noise VSL control (`2609.01339`), MeRoPE
  video generation (`2609.01252`).
- Medical/clinical: pathology VLM benchmark (`2609.00866`), GazeRefine
  (`2609.01310`), BS PET/CT (`2609.01554`), BrainDiff (`2609.00593`), CTA
  report benchmark (`2609.00909`), chest-CT anchoring (`2609.00447`),
  Clear-cell RCC grading (`2609.01426`), clinical trial matching
  (`2609.01202`), medical QA robustness (`2609.01361`), Smart-Agriculture
  soybean agents (`2609.00106`), TRUST breast screening (`2609.00300`),
  biomedical NER MiNER (`2609.00073`), suicide-risk calls (`2609.00191`),
  health misinformation (`2609.00403`), clinical AI failure review
  (`2609.00076`), healthcare NLP workflow benchmark (`2609.00296`),
  whole-slide reasoning SlideBank (`2609.00342`), physiological sensing
  (`2609.00435`), NSIDDx differential diagnosis (`2609.00256`).
- Finance/e-commerce: agentic asset pricing (`2609.00731`), FinLifeBench
  (`2609.01198`), VIBE-Bench (`2609.00921`), recommendation/ranking items
  (`2609.01240`, `2609.00618`, `2609.00996`, `2609.00886`-class), CATeye
  (`2609.01425`), A/B-test personas (`2609.01038`), marketplace autoresearch
  (`2609.00274`), electricity price forecasting (`2609.00089`).
- LLM inference efficiency/KV: CacheBridge (`2609.00891`), LatentPress
  (`2609.01507`), SinkPruner (`2609.01004`), S^2Prune (`2609.01224`), Faster
  Than Flash (`2609.00097`), OCGQuant (`2609.00066`), QTEA (`2609.00224`),
  HBQ (`2609.00450`), REAL-Q (`2609.00049`), DynaNDE (`2609.00407`), MoE
  routing items, mzCache (`2609.01338`), TopoCompress (`2608.30811` v2).
- RLVR/post-training/RL methods: Group Adaptive Clipping (`2609.00444`),
  AMRP reward-hacking mitigation (`2609.00213`), ReNFT (`2609.00061`),
  Where the Verifier Fails (watch), Explore More Drift Less/CANOPY (watch),
  HypReflect personalization (`2609.00251`), Flawed in Nature evolution
  (`2609.00129`), verbal RL survey (watch).
- Scientific-domain agents without reusable contracts: Dr. Claw (watch),
  Beneath the Diff (watch), EvoSCM (`2609.01526`), agentic programs in
  materials science (`2609.00795`), AutoXRD (`2609.00070`), RAPIDMap
  (`2609.00046`), Super Library-style items, RingMoClaw (watch), SpecMind
  spectrum agents (`2609.00427`), Scientific Agent Skills (watch),
  RestoreBench (watch).
- General LLM-agent papers in creative/security/speech/GUI domains without
  harness scope: ChatDev 2.0 (watch), GlossoGen (watch), DramaChain
  (`2609.00646`), RingMoClaw (watch), Agentic multimodal hyperspectral
  (`2609.01289`), InSight (watch), ExBind (watch), GUI-CC (watch),
  EvoFlint (watch), SEAV (watch), Distributed Implicit Harm (`2609.00206`),
  TempJail-class attacks, EvoFlint-class red-teaming, C-SafeQA (watch),
  HiveTraceGuard-Pro (`2609.01046`), When Safety Routing Breaks
  (`2609.01455`), Recursive Criticality (`2609.00137`), Capability-Gated
  LMs (`2609.00445`), TRIS RAG poisoning defense (`2609.00470`),
  Authority Bias (`2609.00248`), MemeBridge (`2609.00491`), Instella-MoE
  (`2609.00791`), Arkios (`2608.30092` v2).
- Image/video generation and perception: SpatialGuard (watch), H3-World
  (watch), Streaming4D (watch), TempCloze (`2609.01515`), Beyond the Image
  Plane (`2609.00924`), VOIM (`2609.00775`), most cs.CV entries in range.

## Operational notes

- The arXiv API was intermittently rate-limited (429 with ~14-byte bodies)
  during the growth rechecks; spacing requests and retrying with backoff
  completed 8 queries (5 interval categories + 3 combined-day rechecks).
  The combined five-category OR queries return each entry once, so unique
  counts are direct.
- The `[20260901]` day is fully exposed (447 unique); no 09-02 submissions
  were visible — the next run should re-check `[20260902]` and any further
  growth in `[20260831]`/`[20260830]` (both grew between the 09-01 run and
  this one, so late additions are still arriving).
- Artifact checks on 2026-09-02 (live HTTP/GitHub/HF): flesymeb.github.io/
  HarnessOfHarness (live; repo Flesymeb/HarnessOfHarness live @ `aadbde0`),
  github.com/sileod/cordis-bench (live @ `290d6b4`), pine-lab-ntu.github.io/
  facet-0 (live; HF Pinelab/Facet-0 Apache-2.0 with post-training params;
  HF datasets/Pinelab/ManuFacet-1K CC-BY-4.0), SalesforceAIResearch/tracelab
  (live @ `072fcb6`; HF Salesforce/tracelab-comprehend live),
  github.com/krafton-ai/WHALE (404, all case variants), github.com/aistairc/
  peg-in-bench (404), github.com/AlibabaResearch/SignalCoverageRL (exists,
  empty — no HEAD), xpeng-robotics.github.io/anyworld (still 404). Watch-list
  repos thunlp/StudyBench, Daerwang2020/Exbind, maevehutch/insight,
  FloXXXt/DualStake, OpenBMB/ChatDev, xwsun01/AnySearch, coenwerem/parcelstow,
  BokwaiHo/arl-mode-collapse, K-Dense-AI/mimeo, OpenLAIR/dr-claw,
  skumar-ml/belief-world-models all live.
- Scratch XML kept under `.scratch/arxiv-2026-09-02/` during the run and
  removed afterward (not committed).
- Working tree before the run: `docs/reference-architecture.md` modified,
  `docs/ring-harness.png` and `handoff.md` untracked — preserved untouched.
- Consistency: README header badge and Current Landscape "Last verified"
  both updated to 2026-09-02 (both were 2026-09-01 after the previous run).
