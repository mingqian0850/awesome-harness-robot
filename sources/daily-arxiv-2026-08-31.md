# Daily arXiv scan — 2026-08-31

## Scope

- Interval: since the 2026-08-30 run's cutoff (~2026-08-30 08:00 UTC) through
  2026-08-31 ~10:15 UTC. No missed dates (the 08-30 run executed); 08-31 is
  today. Missed-date logic therefore covers only the remainder of 08-30 plus
  08-31, and — because of arXiv announce lag — the batches newly exposed since
  the last run.
- The arXiv export API (`https://export.arxiv.org/api/query`, HTTPS) was
  healthy (one 429 burst on per-category probes; resolved by pacing). Batch
  status vs the 08-30 run:
  - `[20260828]` (`submittedDate:[20260828000000 TO 20260828235959]`) is now
    **exposed for the first time** — 285 unique entries across cs.RO/cs.AI/
    cs.CL/cs.CV/cs.LG, all v1 with `published == updated` (0 revisions). This
    slice was invisible to the 08-28/08-30 runs and is fully screened here.
  - `[20260827]` **grew** from 350 unique (08-30 run) to **430 unique**;
    per-category raw hits moved 41/109/86/102/105 → 49/144/104/120/139 (+113).
    Per procedure the whole changed batch was re-screened once, with the
    growth subset (the 27xxx-range IDs not named by the 08-28/08-30 records)
    as the focus. Six `[20260827]` entries now carry v2: AgentFold
    (`2608.26747`), TempJail (`2608.26971`), GRAFT (`2608.27079`), LoopHarness
    (`2608.27141`), PAWBench (`2608.27345`), Understanding Evolution Strategies
    (`2608.27351`) — all same-day minor revisions dated 2026-08-28 (after the
    08-28 run's cutoff; file sizes ~unchanged; no new artifact links on the
    abs pages), so no curation-status change, including for the included
    LoopHarness.
  - `[20260826]` grew 390 → 391 unique (late additions: `2608.26449`,
    `2608.26451`, `2608.26453`, `2608.26462`, `2608.26465`, `2608.26469`,
    `2608.26471` — all domain-excluded). 20 entries carry `updated !=
    published`, all revisions ≤ 2026-08-28T17:03 (MathAdv, Stale Constraints
    v2, Redwood, SHROOM-Visions 2026, SKILL.state, TraceML, QLoRA facts,
    Zero-WAM v2, MACGen, LM-X v2, ClueWeaver, …) — **no new revisions** since
    the last run.
  - `[20260829]`, `[20260830]`, `[20260831]` still return 0 entries (announce
    lag; the environment's newest exposed data is `[20260828]`). Next run
    should re-check `[20260829]`/`[20260830]` and growth in
    `[20260828]`/`[20260827]`.
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action models,
  code-as-policy, skill discovery, memory, recovery, evaluation, runtime
  monitoring, and safety. Candidate claims were checked against the arXiv
  record, official project pages, and official code/model/data repositories
  (live HTTP and GitHub checks on 2026-08-31); README, landscape, and source
  records were checked for duplicates (no name/ID collisions). All performance
  results below remain author-reported unless stated otherwise.

## Included (new curated entries)

### openJiuwen: Beyond Static Harnesses for Long-Horizon Coding Agents

- Paper: https://arxiv.org/abs/2608.27969
- Code: https://github.com/openJiuwen-ai/jiuwenswarm (Apache-2.0; the paper's
  "Source Code" link; org `openJiuwen-ai` — 19 public repos including
  agent-core, agent-runtime, agent-tools, agent-memory, agent-protocol,
  agent-studio, skillhub — all live 2026-08-31, pushed today; note the linked
  repo's README currently fronts the JiuwenClaw assistant, while the harness
  platform components live across the org repositories)
- Classification: General Harness Methodology (open-source harness platform;
  structural composability + runtime adaptivity).
- Why included: an open-source harness from the openJiuwen team (Huawei
  Technologies) that attacks the two harness challenges of long-horizon coding
  agents directly: *Structural Composability* (compose capabilities, delegate
  sub-agents, and scale multi-agent orchestration without rebuilding) via a
  shared execution substrate and Rail-based capability composition across
  single agents, delegated sub-agents, and Swarm Flow under common execution
  semantics, and *Runtime Adaptivity* (semantic diagnostics, execution
  outcomes, task progress, and changing context relevance dynamically steer
  framework-controlled runtime decisions around a fixed model policy). On
  SWE-bench Verified and Terminal-Bench 2.1 the authors report 82.6% and
  87.19%, exceeding the strongest selected official-leaderboard results by
  3.4 and 3.39 percentage points. A real Apache-2.0 artifact release in the
  production-harness line (StateM, Prime Agent, openJiuwen).
- Boundary: digital coding agents; no robot experiment; results
  author-reported; the linked repository's top-level README presents the
  consumer assistant surface rather than the harness internals.

### EvoUndo: Recoverability-Constrained Self-Evolution for LLM Agent Harnesses

- Paper: https://arxiv.org/abs/2608.28363
- Classification: General Harness Methodology (recoverability verification
  for runtime self-modification).
- Why included: makes *recoverability* a first-class verification target for
  self-evolving harnesses — the missing half of the self-evolution line
  (Self-Harness, Evo-Harness, HarnessLens, JIT-Agent): a successful mutation
  may leave persistent effects that cannot be safely reversed in states
  different from the one in which it was created. EvoUndo represents,
  synthesizes, diagnoses, and independently verifies recoverability of
  model-generated self-modifications across counterfactual states. Across 600
  unseen one-shot self-evolution tasks it identifies 197 capability-improving
  mutations that fail recoverability verification; conventional repair
  strategies recover 0/197, deterministic oracle analysis 48/197 under the
  original recovery language L0, and the extended recovery calculus 191/197.
  A protocol-locked 2×2 grounding-by-expressivity intervention separates the
  bottlenecks: exact state-address grounding lifts recovery 0/48 → 38/48 when
  the original language suffices, while extending the recovery language
  recovers 142/143 in the oracle-defined stratum. On gpt-oss-120b the richer
  language plus exact-address diagnostics *reduces* recovery to 133/143; a
  Qwen3.8-27B replication preserves the grounding and expressivity effects but
  not this negative interaction (model-dependent). The conclusion — reliable
  self-evolution requires co-designing verification, state grounding, witness
  semantics, and recovery-language expressivity — is a direct design contract
  for the harness-engineering line.
- Boundary: digital agents; no robot experiment; no official artifacts
  located as of 2026-08-31 (GitHub/web search); results author-reported.

### Logos: An Agent Harness on a Cross-Process Bus

- Paper: https://arxiv.org/abs/2608.28553
- Classification: General Harness Methodology (fault-isolated cross-process
  harness; formal composability).
- Why included: closes a fault-domain gap in the spatiotemporal-composability
  calculus line (capabilities as components with tracked inverses, agents as
  plugins): the plugin carrier is normally a single process sharing one
  context, so one fault suspends every component and process death interrupts
  every co-resident session. Logos shows the calculus does not bind an agent
  to one process (stateless LLM inference keeps cross-step state outside the
  model; the soundness invariant lives on the state space), and builds a
  ROS-like cross-process harness in which a plugin is a process and the only
  shared state is an append-only transcript. Eighty sessions resume with no
  repeated effect after kills placed at the four boundaries of the tool-call
  cycle, and a same-fault comparison with a single-process reference shows one
  fault interrupting every co-resident session versus ending at one node under
  the peer-process construction. A concrete fault-isolation contract for
  agent runtimes, complementary to the durability line (Agentic Transaction,
  StateM).
- Boundary: digital agents; no robot experiment; no official artifacts
  located as of 2026-08-31; results author-reported.

### CEDAR: Automata as Verifiable Interfaces for Language-Guided Embodied Action

- Paper: https://arxiv.org/abs/2608.27797 (OpenReview record exists)
- Classification: Agentic Robot/VLA Harness (verifiable constraint interfaces
  for embodied agents).
- Why included: attacks the verification gap of code-generating embodied
  agents — free-form programs provide "no stable object to verify, compose
  with new constraints, or repair from a failing trace" — by grounding
  natural-language instructions as regular languages over environment event
  traces: a counterexample-guided framework uses an LLM for semantic judgments
  and execution traces for correction, representing both skills and
  specifications as deterministic finite automata, so a learned skill
  intersected with a learned *sleep at night* / *stay in this biome*
  specification yields a controller that enforces the constraint *by
  construction* rather than by repeated prompting. In Minecraft, with the same
  simulator/API observations as a program-generating baseline, CEDAR maintains
  temporal and spatial constraints the baseline fails to preserve and
  amortizes reuse of learned skills, reducing cumulative LLM queries. A
  practical verification layer between natural-language instructions and
  embodied-agent policies — the automata/verifiable-interface line for the
  robot-agent cluster.
- Boundary: simulation-only (Minecraft); no official code located as of
  2026-08-31; results author-reported.

### CURA: Certified Runtime Alarms for Computer-Use Agents

- Paper: https://arxiv.org/abs/2608.27808
- Classification: Evaluation/Safety (certified false-alarm runtime monitoring
  for agents).
- Why included: quantifies and repairs the failure of self-report as an
  oversight channel — on 361 OSWorld tasks the authors' own pipeline reaches a
  mean task score of 82.9 (above the 72.4 human reference) yet 64 of its 71
  failures (90%) end with a success claim, 61 acknowledge no blocker, and the
  explicit failure affordance is never used in roughly 9,100 calls. CURA is an
  external monitor that reads only harness-visible telemetry (no model
  internals, no extra LLM calls, no prompt changes) and turns the running
  trajectory into a sequential test with *certified* false-alarm control: at
  α = 0.10 its CUSUM alarm detects 42.3% of failures a median of 31 steps
  before termination at a realized false-alarm rate of 0.066; alarm-gated
  mid-execution oversight recovers 23 of 70 failures while spending a frontier
  overseer on 38, giving a deployable cascade at mean score 86.8 and 84.5%
  full-solve (305/361). The certificate bounds false alarms only, and the
  paper reports where behavioral monitoring is uninformative — an honest
  monitoring-contract contribution in the runtime-oversight line (ContactGuard,
  VLA task-progress probe, StepGuard).
- Boundary: computer-use (digital) agents; no robot experiment; no official
  artifacts located as of 2026-08-31; results author-reported.

### OBPE: Out-of-Band Policy Enforcement at a Trusted Tool Boundary

- Paper: https://arxiv.org/abs/2608.27646
- Prototype: https://github.com/dogwood-policy/dogwood (live 2026-08-31; the
  paper's linked artifact — a reference interpreter for the Dogwood governance
  language over Cedar with temporal conditions, plus conformance tests; the
  full simplified HTTP-proxy prototype described in the paper is not separately
  public)
- Classification: Evaluation/Safety (out-of-band enforcement at the tool
  boundary).
- Why included: moves policy enforcement outside agent reasoning — a trusted
  boundary that authorizes the typed operation and resource, narrows the query
  before the backend call, and filters records/fields or masks values in the
  response, with semantic gating able to deny or hold authorized calls; a data
  policy owner sets the maximum grant and agent policy can only narrow it. The
  paper proves order-independence of the policy plan and that agent policy
  cannot widen the ceiling, and evaluates a prompted-agent baseline with and
  without OBPE against Jira and ServiceNow mocks on four models including 20
  adaptive red-team tasks: across 3,621 trials, trace failure (protected data
  entering agent context / exact value in the answer / forbidden effect
  completed) falls from 57.6% to 0.2% (cluster-weighted −41.2 points, 95% CI
  [27.7, 54.9]); fulfillment falls 79.1% → 60.9% while paired safe-useful
  completion rises 21.8 points [9.5, 35.2]. It also reports honestly where
  enforcement falls short: some answers reconstructed values that never
  entered context or used filtered row counts as an oracle — "shaping one
  execution is not noninterference." A typed-authorization contract in the
  runtime-authorization line (Bounded Agents, One Gate Is Not Enough, SARA).
- Boundary: digital agents; no robot experiment; prototype is a simplified
  proxy (Cedar policy core) rather than the production system; results
  author-reported.

## Reviewed but not promoted (watch list)

From the `[20260828]` batch:

- **Credo** (`2608.27790`) — recovers structured declarative descriptions of
  searched harnesses (primitives + metadata + provenance) so a compiler can
  reuse them for new tasks instead of restarting harness search; squarely the
  harness-engineering line, but the paper is preliminary results plus a
  research agenda (cs.DB); watch for a fuller evaluation.
- **GOD: Govern, Observe, Direct** (`2608.27992`, EMNLP 2026 System
  Demonstrations) — local-first control room for agent societies (live
  replay, Ask/Intervene, portable experiment packs); observability tooling;
  no code link in the record; watch.
- **String** (`2608.28027`) — agentic OS where every app is a Markdown
  document (SFMD: views, typed actions, credentials; two verbs /open and
  /act); claims an open-source runtime but no link located; watch.
- **CoCoBench** (`2608.28266`) — construct-level benchmark for embodied
  multi-agent coordination (897 oracle-validated instances; task allocation,
  sequential ordering, mutual exclusion, handoff); evaluation contract for
  embodied MAS; watch (no artifacts located).
- **LoopArena** (`2608.28281`) — benchmark for evaluating models as *runtime
  controllers* of loop-engineered coding-agent runs, separating loop guidance
  from agent ability; directly the harness-loop evaluation line; watch.
- **DoCtOR / Finding Where the Buck Stops** (`2608.28264`, EMNLP 2026 main) —
  automated failure attribution identifies the decisive error agent and
  reflects only that agent, avoiding contamination of well-behaved agents'
  memory; recovery/reflection harness line; watch.
- **VICT** (`2608.28128`, EMNLP 2026) — verifier-instrumented credit tracing:
  exposes the terminal verifier's internal checks as executable evidence
  atoms traced to actions through dependency-valid proof edges; verification
  as a training-time interface; watch.
- **ContextLeak** (`2608.27800`) — malicious-tool attack inducing agents to
  disclose runtime context as tool arguments (the under-studied disclosure
  condition); agent-security attack line; watch.
- **CAITLYN** (`2608.27990`) — agent-agnostic prompt-injection defense
  middleware (two-tier rule/LLM library + lifelong yielding); code live at
  github.com/liangzid/caitlyn; watch.
- **LongPIBench** (`2608.28411`, Findings EMNLP'26) — long-context prompt-
  injection benchmark across four realistic scenarios; watch.
- **Cross-Session Decomposition Attacks** (`2608.27945`) — compositional
  safety risk: benign-looking subqueries across sessions recomposed toward a
  forbidden objective; conditional risk-transfer bound; watch.
- **Speculative Probing** (`2608.28099`) — repurposes the speculative-decoding
  module for sequence classification, monitoring at speculative-decoding
  cost; runtime-monitoring line; watch.
- **Layered LLM Defenses as an Ensemble** (`2608.28327`) — measures failure
  correlation between defense layers (stacks compound only if members fail on
  different inputs); security-evaluation measurement; watch.
- **PersonaForge** (`2608.28378`) — multi-turn user simulation for agentic
  systems (four-dimensional persona space, SOUL calibration, 6.3K training
  records, 138-task benchmark); watch.
- **ContextPilot** (`2608.28476`, EMNLP 2026 main) — proactive context
  management via fine-grained RL with expanded toolset; context-management
  line; watch.
- **AcrossVAM1.0** (`2608.28491`) — lightweight (0.28M-param) particle world
  modeling for text-assisted robot video prediction (SAM3-DLP particles +
  causal dual-stream decoder); world-model line; watch.
- **Aero Hand Open** (`2608.28578`) — simulation-ready tendon-driven hand
  (sim model reproducing the cable transmission + identified actuation map);
  project page live (tetheria.github.io/aero-hand-open); hardware/sim
  artifact without a harness contract; watch.
- **ChainSplat** (`2608.28570`) — screw-theoretic DLO dynamics from multi-view
  RGB; watch.
- **Post-Edit Re-Verification** (`2608.28147`) — controlled comparison of
  verification-cadence guidance in simulator-backed engineering agents
  (DWSIM); verification-policy measurement; watch.
- **On the Maintenance and Co-evolution of Agent Plugins** (`2608.28497`) —
  empirical study of 1,926 Claude Code plugin marketplaces (8,351 plugins,
  77,773 commits); config-as-code ecosystem line; watch.
- **CultureConverse** (`2608.28405`, EMNLP 2026) — multilingual multi-turn
  simulation harness for culturally grounded assistance; digital; watch.
- **Fidelity Is Not Enough** (`2608.28439`) — dispatch-level instrumentation
  for agentic datasheet extraction; silent-failure detector (tool-call rules
  only); instrumentation line; watch.
- **SILICA** (`2608.28182`) — open instrument benchmarking LLM agent societies
  against human behavioral distributions (five environments with published
  human anchors); agent-society evaluation; watch.
- **Optimal Adversarial Testing** (`2608.28362`) — testing theory for
  recovering honest results from dishonest test takers; generic testing, not
  agent-harness; watch.
- **Agent memory for UAQ handling** (`2608.27924`) — systematic study of
  memory for unanswerable-question handling; gains selective and fragile;
  watch.
- **Post-Training VLMs for Video Mistake Detection** (`2608.28406`, BMVC
  2026), **Iron** (`2608.27866`, GUI agents), **AcCoRD** (`2608.27818`),
  **RealSWE** (`2608.27831`), **COVER** (`2608.28475`), **AGENT-O**
  (`2608.28345`, healthcare agent cards), **MAP** (`2608.28384`),
  **TACIT-Switch** (`2608.27911`), **SEPO** (`2608.28067`), **CrabOS**
  (`2608.28165`), **Plan Along the Way** (`2608.28075`), **LUCID**
  (`2608.28437`), **MaCoPlanner** (`2608.28300`) / **PanelShield**
  (`2608.28305`), **FaulT-Bench** (`2608.27021`) — evaluation, agent-OS,
  planning, or domain-specific items without a reusable harness contract;
  watch for artifact releases or a clearer harness framing.

From the `[20260827]` growth subset (not named by the 08-28/08-30 records):

- **VLAct** (`2608.27550`) — representation-centric continued pre-training for
  VLAs (VLM-prior preservation, multi-head continuous action co-supervision,
  partially unified cross-embodiment layout); all models/pipelines public
  (starvla.github.io/VLAct, live); VLA method without an external harness
  contract; watch.
- **PHR-VLA** (`2608.27609`) — planning-horizon reasoning in VLAs via
  privileged future-latent supervision (lightweight future head; LIBERO
  84.1% → 8x); VLA method; watch.
- **Code as Worlds** (`2608.27549`) — agentic discovery of executable world
  representations (physical composition, dynamics, appearance as code);
  project page live (mirros-lab.github.io/code-as-world); world-model line;
  watch.
- **LongGuard** (`2608.27580`) — long-context guardrail failure: Safety
  Needle-in-a-Haystack over a 0.25k–32k grid shows unsafe recall dropping
  >50% across 15 guardrails, with attention-dilution mechanism analysis and
  training-free mitigation; guardrail evaluation line; watch.
- **Probe-based tool-calling error detection** (`2608.27750`) — linear probes
  detect incorrect tool calls (including wrong-value/right-type arguments
  invisible to standard logging) across 18 tool-calling LLMs; runtime-
  monitoring line; watch.
- **Why Didn't It Check?** (`2608.27768`) — separates occurrence and
  conditional repair of unsupported final claims in tool-equipped LLMs with
  exact-state replays; measurement; watch.
- **SegBench-GC** (`2608.27678`) — controlled stress test of segmentation
  invariance in offline goal-conditioned RL (35,000 artificial cuts;
  continuation-valid targets as control); evaluation methodology; watch.
- **When Robots Mishear Us** (`2608.28518`) — maps ASR-error-induced safety
  risks of voice-controlled embodied AI against SafeAgentBench/POEX;
  simulation-based safety evaluation; watch.

## Watch-list status (rechecks)

- No new artifact releases or revisions for previously listed watch items:
  StageWAM, ReflexVLA/ReflexBench, DreamX-Phi, UniTexture, PRISM,
  GigaBrain-0.7/WBC, ForceU-VLA, LIBERO-VIFO, Agent Lightning, VLCP, Hydra-0,
  BATON, Q-Planning, AutoSaddler, JIT-Agent, Task-CoEvolve code, UCAG-P,
  TrapVLA code/benchmarks, TemporalFlow-VLA, PredVLA, FlashVLA, WikiSkill,
  RedEvoAgent, SKILL.state, Agent Mesh, WALL-SS, R2M-Bench, INTENT-AS-A-TOOL,
  BTS-AgentBench, TraceBench, GraphMemix, UrbanGround, Stale Constraints v2,
  LM-X v2, Zero-WAM v2 — unchanged. All `[20260826]` revisions (20 entries)
  predate the 08-30 run's cutoff; all `[20260827]` revisions (6 entries) are
  same-day minor v2 updates from 2026-08-28.
- Watch items with code now live (verified 2026-08-31): VLAct (project page +
  pipelines), Code as Worlds (project page), Aero Hand Open (project page),
  CAITLYN (repo), Dogwood (OBPE prototype repo).

## Domain exclusions in the screened ranges (per policy)

- Driving/CAV: conditional diffusion for energy-efficient driving
  (`2608.28142`), Barrier Function Conformal CVaR (`2608.26533`), DPA-I2P
  (`2608.26589`), assisted lane-change LiDAR road testing (`2608.26669`).
- Conventional robot control/locomotion: Poppy (`2608.26505`), SOLO
  (`2608.26583`), Stay Seated humanoid-chair (`2608.28090`), PAMoR
  (`2608.28213`), gas-source localization (`2608.28214`), coordinated multi-arm
  LQ games (`2608.27726`), tensegrity (`2608.27221`), crowd-navigation
  diffusion policies (`2608.27158`), contact-guided locomanipulation
  (`2608.28140`), bin-picking (`2608.28175`), Distributed Model-Based
  Diffusion (`2608.27685`), CLIPPER coverage planning (`2608.26819`),
  one-year-in-a-forest navigation field study (`2608.27628`), UAV search
  (`2608.28270`), STEGNav (`2608.28279`), MAR fleet scheduling (`2608.27271`).
- Perception-only: CAVE-NAV (`2608.27793`), uScenes (`2608.27795`), MAGP
  (`2608.27497`), VidParse (`2608.27562`), GeoFF3D (`2608.28288`), WALDO
  (`2608.28216`), suction-grasp detection (`2608.28246`), GAAT (`2608.27971`),
  A-PAIR (`2608.27997`), SpatialCrafter (`2608.27073`).
- Medical/CV/CL/LG without a reusable agent/VLA harness, recovery, safety, or
  evaluation contract (CheXtriev `2608.28137`, MedFG-VQA `2608.26848`,
  CARDINAL `2608.27690`, sepsis `2608.27421`, pothole `2608.27633`, fetal
  ultrasound `2608.27051`, …).
- Finance/recsys: RetailAgent (`2608.28399`), VERA-8B (`2608.28402`), POI
  (`2608.27840`), portfolio (`2608.28252`), DSA stock (`2608.26990`).
- LLM inference efficiency (KV cache `2608.28293`, HyQuant `2608.27875`,
  block drafting `2608.27339`, PACE `2608.27206`, SABER `2608.27963`, AERA
  `2608.27964`, Parser States `2608.28276`).
- RLVR/post-training methods (Consolidating `2608.27409`, Boosting
  `2608.27420`, TTPO `2608.27448`, GRACE `2608.28361`, VISTA `2608.28306`,
  Rubric-to-Code `2608.27906`, RCCA).
- Scientific-domain agents without reusable contracts (MAIL `2608.28315`,
  LitCurate `2608.27629`, SETU `2608.27524`, Prove2Me `2608.28433`,
  NL2AGBench `2608.28481`, Gemini real-world science `2608.26701`).
- General LLM agent papers in creative/security/speech domains without
  harness scope (Magpie `2608.27168`, TempJail `2608.26971`, CamoDocs
  `2608.28389`, SURE-Challenge `2608.27783`, prosody `2608.27848`, AI
  Alignment survey `2608.27910`, LLM agents for security survey
  `2608.28490`), vendor/industry (Thomson/SovereignAI `2608.27147`, Nemotron
  3.5 moderator `2608.27548`), and RL negative-result method papers
  (Coverage-Not-Credit `2608.28011`).

## Operational notes

- The arXiv API was healthy for the run; one 429 burst on rapid per-category
  probes was resolved by pacing (3 s+ between requests). `submittedDate`
  ranges for 2026-08-29/30/31 return 0 entries — the environment's newest
  exposed data remains `[20260828]`; the next run should re-check
  `[20260829]`/`[20260830]` and growth in `[20260828]`/`[20260827]`.
- Artifact checks on 2026-08-31 (live HTTP/GitHub): openJiuwen-ai/jiuwenswarm
  (Apache-2.0, pushed 2026-08-31, org of 19 repos), dogwood-policy/dogwood
  (OBPE-linked governance-language interpreter, live), starvla.github.io/VLAct
  (live), mirros-lab.github.io/code-as-world (live),
  tetheria.github.io/aero-hand-open (live), github.com/liangzid/caitlyn
  (live). No repositories located for CEDAR, EvoUndo, CURA, Logos (GitHub
  repository search + web search; GitHub API rate limit hit late in the run —
  `git ls-remote` and HTML checks used as fallback).
- Scratch XML kept under `.scratch/arxiv-2026-08-31/` during the run and
  removed afterward (not committed).
- Working tree before the run: `docs/reference-architecture.md` modified,
  `docs/ring-harness.png` and `handoff.md` untracked — preserved untouched.
- Consistency: README header badge and Current Landscape "Last verified" both
  updated to 2026-08-31 (both were 2026-08-30 after the previous run).
