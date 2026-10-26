# Daily arXiv scan — 2026-10-26 (the 2026-10-23 announcement batch finally becomes visible; 2026-10-24 → 2026-10-26 are empty)

## Scope

- **Interval owed.** The previous successful run finished on `main` at `f6fe4cb` on **2026-10-25 09:17 CEST** and
  recorded that the export index had been **pinned at 2026-10-22 for a third consecutive run** — 2026-10-23,
  2026-10-24 and 2026-10-25 all returned zero records in all five categories, while an unrestricted descending
  `cs.RO` query returned `2610.31670` (2026-10-22T17:59:51Z) as the newest record. That run explicitly re-screened
  the whole 2026-10-22 batch instead. This run started **2026-10-26 13:24 UTC** on `main` at `f6fe4cb`, so the
  interval it owes is **submission dates 2026-10-23 → 2026-10-26**.
- **The frontier moved.** An unrestricted descending `cs.RO` query now returns **2026-10-23T17:59:21Z**
  (`2610.32978`, SceneBloom) as the newest record, i.e. the 2026-10-23 batch has been indexed. That batch is
  therefore **new material this run owes and no earlier run has screened**, and it is screened in full below.

  | query (one OR-query over cs.RO/cs.AI/cs.CL/cs.CV/cs.LG per day) | 2026-10-23 | 2026-10-24 | 2026-10-25 | 2026-10-26 |
  | --- | ---: | ---: | ---: | ---: |
  | `submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` | **369** | 0 | 0 | 0 |
  | `lastUpdatedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` | **369** | 0 | 0 | 0 |

- **The batch.** 369 unique records, `published` spanning **2026-10-23T00:05:21Z → 2026-10-23T17:59:56Z**. Every
  record is still **v1**: no revision reached the window. The `submittedDate` and `lastUpdatedDate` sets for
  2026-10-23 are **identical** (set difference 0), so no record submitted on an earlier date was revised on any
  day in this window — **no existing README entry needed a status, version, or number update**.
- **Coverage.** 2026-10-24 and 2026-10-25 are Saturday and Sunday and carry no submissions in these categories;
  2026-10-26 is still inside the harvesting day and returned zero, consistent with a batch that announces in the
  early-morning window and had not yet closed. All 369 records carry at least one of the five target categories:
  primary `cs.CV` 90, `cs.LG` 70, `cs.AI` 67, `cs.CL` 48, `cs.RO` 37, and 57 cross-listed from another primary
  (`stat.ML` 10, `cs.SD` 6, `cs.SE` 5, `eess.AS` 3, `cs.CR` 3, `cs.DC` 3, `eess.SY` 3, `cs.HC` 2, `math.OC` 2,
  `cs.CY` 2, others 1).
- **Consequence, per procedure.** Because the owed interval contained a genuinely new batch, this run is a
  **first-pass screen of new material**, not a re-screen. No tail re-screen of 2026-10-20/21/22 was performed:
  those batches were screened twice by the two preceding runs, and the new batch is where the risk of a missed
  candidate lies in a fresh window.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`, saved under `.scratch/arxiv-2026-10-26/`
  (untracked, never committed). Shape change from earlier runs: **one OR-query per day across all five categories**
  instead of five per-category requests, with pagination at 200 records. The 2026-10-23 batch harvested in 2
  requests (200 + 169); the three empty days and the four `lastUpdatedDate` days each cost one request. **15
  completed requests total** (two frontier probes, three day-count queries, the two harvest pages, and eight day
  sweeps), 19–20 s apart, **zero HTTP 429** — well inside the spacing that punished earlier runs.
- **Screening.** (a) **Every one of the 369 titles was read**, not only keyword hits. (b) Abstract review of
  **30** shortlisted records. (c) Artifact verification of **13** against live sources: the GitHub REST API
  (licence, size, creation/push timestamps, stars, top-level tree), raw `README.md`/`LICENSE` fetches, the
  Hugging Face model and dataset APIs (gating, licence tags, file counts), the university GitLab instance, Zenodo
  DOI resolution, and the project pages. (d) Full-text HTML search of **12** records for release statements and
  repository URLs, because several inclusions carry no link in their API metadata.
- **Deduplication.** Every shortlisted ID and title was grepped across `README.md`, `docs/`, `sources/` and
  `editorial/` before inclusion — none of the 13 was present. Name collisions were checked explicitly: **SCOPE**
  (`2610.31518`, already deferred by the 2026-10-25 run) and **PRISM** (`2610.32101`, sampling diversity for LLMs)
  are unrelated to the listed SCOPE and PRISM entries, and neither was a candidate here.
- **Revisions.** The `lastUpdatedDate` sweep for 2026-10-23 → 2026-10-26 returned 369 records on 2026-10-23 —
  exactly the submitted set — and zero on the other three days.

## Included (13)

### General Harness Design and Self-Improvement

- **Trial by Trace / TraceTrial-Bench** (`2610.32285`, cs.AI; v1 2026-10-23) — why it qualifies: it makes the
  harness the **measured object** and gives that measurement a component structure instead of one aggregate. Seven
  components (context, execution, governance, lifecycle, observability, tools, verification) are scored apart from
  task success; the machinery is what keeps them separable — a Trajectory Tracing module, a **two-layer**
  performance/reliability evaluation, a Multi-source Evidence Acquisition module that shows the judge agent
  context, harness configuration and execution environment, and a Failure Attribution module that walks explicit
  mechanism chains at run level. Reported over **4 harnesses × 9 models × 95 tasks** (3,420 runs): macro-averaged
  harness scores **59.8%–72.0%**, and **31.1% of runs succeed on the task while failing at least one harness
  criterion** — the number that makes a success rate insufficient evidence about a harness. **No artifact
  (checked 2026-10-26):** neither the arXiv record nor the full text links a repository, dataset, or benchmark
  release, so the 95 tasks are described rather than shipped. Digital agents, no robot experiment; author-reported.

- **HarnessMM** (`2610.32813`, cs.AI; v1 2026-10-23) — why it qualifies: it identifies the cheapest repeated cost
  in a harness — reasoning invoked again after tool feedback has already fixed the next action — and compresses it
  with a **guarded, revertible** structure rather than a prompt change. A controller at the model–tool boundary
  stores resolved decisions as Executable Decision Graphs (exact action, literal arguments, causal dependencies,
  public guards), re-activates them from observed receipts, and substitutes a certified callback for the slow one
  while the actor stays frozen; abstention, unsupported evidence and genuinely new decisions fall back to the
  actor. Reported: Codex success preserved at **90.82%** with tokens/task **66.72k → 47.02k** (29.54%), DeepSeek
  Harness **71.43% → 85.71%** with tokens/task **96.28k → 11.62k** (87.93%), and an MBPP ablation at 26.91% lower
  cost than single-use graphs and 19.52% lower than retained callbacks. The correct reading is narrow: a reused
  *resolved decision* is the unit of harness-level compression, and the guard is what makes reuse admissible.
  **No artifact (checked 2026-10-26)**; digital agents, author-reported.

- **When Agents Fail: Model or Harness — Who's to Blame?** (`2610.32483`, cs.AI/cs.CL/cs.SE; v1 2026-10-23;
  "Preprint under review") — why it qualifies: it refuses the component-level summary and reports the **pairing**.
  Five measures — Valid Completion Rate (does the agent finish normally and deliver something evaluable,
  correctness aside), Model–Harness Fit, Cross-Model Fit, Interaction Residuals, Token Load (including failed
  attempts) — are applied to four general-purpose harnesses (OpenCode, Codex CLI, Pi CLI, DeepAgents) × four
  models plus two fixed native model–harness systems, over six benchmarks, giving **111 settings and 52,168
  trajectories**. Findings: harness rankings move across models and benchmarks; task performance and valid
  completion can favour different pairings; similar task scores accompany distinct execution profiles; OpenCode
  ranks first on cross-model performance and DeepSeek V4 Flash Preview holds the highest mean VCR on the shared
  panel. **No artifact (checked 2026-10-26)**; digital agents, author-reported.

- **RSIHub** (`2610.32253`, cs.AI; v1 2026-10-23; NeurIPS 2026 Meta-Agents Workshop) — why it qualifies: it is
  the shared substrate the agent-evolution line keeps rebuilding, and it fixes the interface that matters —
  recipes compose operators and declare mutable resources, the framework owns execution, evaluation, recovery and
  lineage, and **evaluation sits outside the mutable surface**, so a result stays bound to the candidate and
  evaluation condition that produced it. AHE, Hyperagents, A-Evolve and GEPA are instantiated **without modifying
  the core**; on Terminal Bench 2 and τ³ Banking with MiniSWE and Codex under fixed model/evaluator configuration
  the largest overall pass@1 gain is **13.5 points**, rankings vary by setting, and gains do not always transfer to
  held-out tasks. **Artifact verified open (2026-10-26):** Apache-2.0 `simple-agent-lab/RSIHub` (198 stars,
  5.7 MB, created 2026-07-03, pushed 2026-10-26; README pins the arXiv ID, CI workflow, and the documentation site
  at `simpleagentlab.com/RSIHub/`, which resolves). Digital agents, author-reported.

- **MCP-Injector** (`2610.32555`, cs.SE/cs.AI; v1 2026-10-23; NeurIPS 2026 poster) — why it qualifies: it makes
  tool reliability a **tested dimension of the harness** with a fault model derived from the ecosystem rather than
  invented. The middleware interposes on the MCP tool-call path with controlled persistence (permanent, transient,
  intermittent) and seedable schedules, and its fault distribution is calibrated on **57,240 issue and pull-request
  threads across 1,774 MCP repositories**. In a 1,350-cell experiment (20B and 120B backbones × custom
  plan-and-execute, OpenAI Agents SDK, LangChain × three injection conditions) the permanent-heavy profile cuts
  task-completion from **8.08 to 3.23 of 10** (−60%); scale does not buy tolerance (resilience-ratio model
  p = 0.96; completion under faults 91.1% vs 95.6%, difference 4.4 points, 95% CI [−0.2, 9.3]); stacks differ in
  absorption. **Artifacts verified live (2026-10-26):** `gitlab.com/uniluxembourg/snt/sedan/papers/mcp-injector`
  returns HTTP 200 and Zenodo `10.5281/zenodo.23573765` resolves to a live record. Digital agents, author-reported.

- **Beyond Gain / skill retirement** (`2610.32322`, cs.AI; v1 2026-10-23) — why it qualifies: it supplies the
  measurement a skill hub needs to retire skills per model, and the defect it finds is **identifiability**, which
  no threshold tuning can fix. The standard gain-based rule cannot distinguish an internalized skill from an
  *uncovered* capability, because both yield a near-zero gain; the replacement withholds the skill, scores the
  model on a task synthesized from it, and trusts a high unaided score only when **another model gains from that
  skill** (nearly half of high scores come from tasks no model benefits from). Reported under four models: a
  published gain rule with one task per skill removes over half of the uncovered skills; on SkillsBench **28 of 31
  retirements rest on probes no run passed in full**; retiring on the unaided score costs no detectable reward;
  unaided verdicts reproduce more often than the gain rule's (**κ 0.42 vs 0.23**) but differ across models, so the
  verdict is per model. **Artifact verified open (2026-10-26):** Apache-2.0 `EverMind-AI/SkillCorpus` (692 stars,
  11 MB, created 2026-08-11, pushed 2026-10-23) with a real `skillcorpus/lifecycle/` module (`retire.py`,
  `judge.py`, `audit.py`, `labels.py`, `capability_items.py`, `_layout.py`, `reproduce/`), plus the **ungated**
  Hugging Face dataset `EverMind-AI/skillcorpus-lifecycle` (declares `license: other`, 103 downloads, 1,251
  skills with tasks and item scores under four models). Digital agents, author-reported. Note the repository is a
  pre-existing skill-corpus hub: the paper's contribution is the lifecycle measurement, not the hub.

- **Async-RBench** (`2610.32429`, cs.AI; v1 2026-10-23) — why it qualifies: it scores the part of harness state
  that final success hides. When a main agent keeps working while subagents run, a result can arrive after the
  world moved, and the agent must decide what to update, preserve, repeat, and avoid — a decision a success rate
  cannot reveal and natural asynchronous arrivals cannot reproduce. The benchmark supplies **200 executable tasks**
  across eight scenarios, a scenario controller that releases results on predefined task-state conditions, and an
  **Event Response Contract** naming per response what must be changed, preserved, avoided and re-verified; it
  reports a Dynamic Replanning Score weighting process and outcome equally, *separately* from task success.
  Reported across nine agents: DRS **19.2–45.1**, lowest on duplicate reports and subagent failures; all nine do
  worse with asynchronous than batched delivery (**26.0% vs 29.7%**); rebuilding the plan after every result buys
  only **2.6–3.5** DRS points for three agents. **Artifact verified open (2026-10-26):** Apache-2.0
  `pope8zdt/Async-RBench` (112 MB, created 2026-09-01, pushed 2026-10-23, 1 star) and a live project page
  `pope8zdt.github.io/Async-RBench`. Digital agents, author-reported.

### Agentic Robot and VLA Harnesses

- **LogLook** (`2610.32348`, cs.RO; v1 2026-10-23; 19 pages) — why it qualifies: it makes a robot planner's memory
  a **runtime object with a token budget**. The system keeps the record (execution log plus the model's earlier
  notes) and lets the *model* decide where to look: frames are returned only where it expects a change, and
  demonstrations are skimmed before the model inspects what it chooses — the alternative, a note rewritten every
  step, neither records the episode well nor controls cost. Reported: matches note-only memory's success with
  **43% fewer tokens and less than half the images**, fails fewer episodes through what it remembered or saw, and
  solves **93.5% of the RoboMME test set** under direct control. **Artifact verified open (2026-10-26):** Apache-2.0
  `bingaochen/Astra-on-RoboMME` (29 stars, 28 MB, created 2026-09-21, pushed 2026-09-23; task prompts, three-tier
  control loop, `docs/evaluation.md`, `packages/`, `scripts/`) plus **ungated** Apache-2.0 monitor weights
  `bingaochen/Astra-on-RoboMME-Monitor`. **Configuration caveat recorded:** the repository's own headline is
  **79.13%** on RoboMME for the full three-tier system (frontier planner + π0.5/MME-VLA executor + Qwen3-VL-4B
  LoRA monitor), which is a different configuration from the paper's 93.5% direct-control number; both are
  author-reported and only the latter is claimed in the paper. Simulation only.

- **Coupling Execution Horizon and Sampling Depth in Flow-Matching VLA Policies** (`2610.32102`, cs.RO;
  v1 2026-10-23) — why it qualifies: it turns two settings that deployment teams choose independently into one
  **execution contract**, and shows the conventional choice is wrong. In flow-matching VLAs, numerical solver depth
  and execution horizon are coupled with a systematic **sign reversal**: coarse integration degrades precision
  under rapid replanning yet improves success under extended open-loop commitment, because truncation contracts
  lateral action dispersion around an invariant trajectory centroid and so regularizes against compounding
  open-loop drift. Reported across LIBERO, RoboTwin 2.0 and real-world Franka manipulation on π0, π0.5 and GR00T:
  higher success alongside sharply lower inference latency, training-free and without architectural change. The
  transferable statement is a harness statement — **sampling depth is an operational variance regulator**, i.e. a
  deployment knob owned by the wrapper, not a precision budget. **No artifact (checked 2026-10-26):** the project
  page `jiabingyang01.github.io/Horizon-Depth-VLA/` is live but links no code, weights, or data. Simulation plus
  real-robot evidence, author-reported.

### Robot Foundation and World Models

- **RynnUnify** (`2610.32965`, cs.RO; v1 2026-10-23) — why it qualifies: it is a **16B unified embodied model** with
  shipped weights, and the layering is the point — embodied understanding and long-horizon visual planning through
  interleaved video–language reasoning are the substrate, an RL-learned **agentic extension** decides when to
  invoke video prediction for unobserved states and interaction outcomes, and a **world-action extension** turns
  understanding and prediction into robot actions. It also ships the evaluations its claims need (Rynn-VP for
  long-horizon visual planning, Rynn-GEU for anticipatory reasoning). Reported: highest average embodied
  understanding among the compared unified multimodal models, **+9.75** Rynn-VP overall over Cosmos 3 Nano,
  **+6.0** Rynn-GEU from the agentic extension, **51.5%** on 20 selected composite RoboCasa365 tasks and **46.7%**
  average across three real-robot tasks. **Artifact verified open (2026-10-26):** Apache-2.0
  `alibaba-damo-academy/RynnUnify` (37.8 MB, created 2026-10-25, pushed 2026-10-26, 17 stars; `BCPO/`,
  `VisualPlan/`, third-party notices, paper PDF) plus three **ungated** Apache-2.0 Hugging Face checkpoints
  (`RynnUnify-Base`, 20 files; `RynnUnify-Agentic-Reasoner`; `RynnUnify-WAM-RoboCasa`; 39–48 downloads each).
  Simulation plus real-robot evidence, author-reported.

- **UniAction** (`2610.32977`, cs.CV; v1 2026-10-23) — why it qualifies: it removes the interface heterogeneity that
  fragments action-conditioned world-model data. Any action becomes a **sparse set of action elements**, each with
  a discrete semantic identity and a continuous spatiotemporal grounding coordinate; those coordinates are mapped
  directly into the pretrained video model's **native positional space**, so force, camera control, hand pose and
  robot pose all condition one shared world model inside the same spatiotemporal structure, and
  cardinality-calibrated attention corrects the bias from differing representation granularity. Reported: better
  action following than conditioning alternatives in all four action forms, and — the load-bearing result — a
  **single jointly trained model surpassing the action-specific experts**. **Artifact verified open (2026-10-26):**
  Apache-2.0 `KBRASK/UniAction` (created 2026-10-26, `src/`, `tests/`, `examples/`, third-party licence tree),
  ungated Apache-2.0 checkpoint `BowenXue/UniAction`, ungated dataset `BowenXue/UniAction-Train`
  (`license: other`), and a live project page. Video-world-model evidence with a robot-pose action form, not a
  closed-loop robot evaluation; author-reported.

### Runtime, Safety, and Observability

- **Backtrack** (`2610.32941`, cs.RO/cs.LG; v1 2026-10-23; 17 pages) — why it qualifies: it puts recovery in the
  runtime around a **frozen** policy, which is the configuration a deployed VLA usually has, and it separates
  detection from restoration so each can be audited. A failure-prediction head costing **0.48–0.88%** of the
  policy's parameters, trained only on outcome-labeled rollouts, reads the VLA's existing internal representations;
  on threshold the controller pauses the policy, **retraces the arm's recently achieved motion**, and hands control
  back to the unchanged policy so it retries from an earlier configuration — no recovery demonstrations, no
  retraining, no second model at inference. Reported on LIBERO with SmolVLA, π0.5 and π0-FAST: LIBERO-Long failure
  rate falls **19–48%**, success rises up to **7.4** points, shorter suites (already 85–99%) move less, and all
  **twelve** MetaWorld controller configurations tested with SmolVLA improve on the unassisted policy. **No
  artifact (checked 2026-10-26):** neither the arXiv record nor the full text links a repository, dataset, or
  project page. Simulation only; author-reported.

- **AWARE** (`2610.32742`, cs.RO/cs.AI; v1 2026-10-23) — why it qualifies: it monitors where the harness has **no
  privileged signal** — legacy hardware without encoders, or access forbidden by security policy — by estimating
  system dynamics and predicting state from **external camera observations only**, so the world model is an
  introspection device rather than a rollout generator. On **240 real-world robotic crane trajectories** including
  payload collisions and motor degradations it reports **26%** better anomaly classification than the strongest
  baseline under noisy camera-only state estimation, and decodes the inferred dynamics parameter into a fault
  source, localizing the degrading motor at **88%** accuracy — "this joint is degrading" rather than "something is
  wrong". **Artifact status literal (2026-10-26):** `adaptive-intelligent-robotics/aware` is live and substantive
  (133 MB, created 2026-10-02, pushed 2026-10-26; Dockerfile, `src/`, `configs/`, `scripts/`, `data/`, `media/`,
  two per-seed checkpoint directories, `uv.lock`) and a project page is live, but the repository asserts **no
  LICENSE**, so it is source-available rather than open source. Real crane hardware; author-reported, no
  independent reproduction.

## Current Landscape additions

Eight bullets were added to `README.md` → *Current Landscape*: harness reliability as a component taxonomy with
its 31.1% criterion-failure result; model-versus-harness attribution over 52,168 trajectories; protocol-level tool
faults with no scale advantage; HarnessMM's resolved-decision compression; RSIHub's shared substrate reporting a
13.5-point best gain that does not transfer; the skill-retirement identification result; robot recovery and planner
memory moving into the runtime (Backtrack, LogLook); execution knobs and camera-only monitoring being re-owned by
the harness (Horizon × Depth, AWARE); and world models gaining readable action interfaces with released weights
(UniAction, RynnUnify).

## Rejected / watch list

### Shortlisted, verified, and deferred

- **RobustRLlib** (`2610.32971`, cs.LG/cs.AI/cs.RO) — Library and benchmark for robust RL: 17 robust online/offline/
  safe algorithms behind one interface, six reference methods, a shift toolbox, and an evaluation platform spanning
  humanoid and VLA tasks over **273 perturbation configurations**. **Artifact is real, not a placeholder:** MIT
  `robustrl-lib/robustrllib` (17 MB, created 2026-10-17, pushed 2026-10-22; `robustrllib/`, `vla/`, `isaac/`,
  `baselines/`, `tests/`) with a live site and docs — the project page's "Code coming soon" text is stale. Deferred
  on scope, not on evidence: it compares **RL algorithms**, and its perturbation sweep is an evaluation platform
  rather than a harness contract. A future run can promote it into *Benchmarks and Evaluation → Unified Evaluation*
  without re-verifying the repository; the licence is MIT and the code, docs and tutorial are live as of 2026-10-26.
- **MAST-VLA** (`2610.32312`, cs.RO, CoRL 2026) — Skill-conditioned LoRA-MoE adapters plus a trajectory-affordance
  spatial prior; reported CALVIN ABC→D average length 4.65 and real-robot multi-skill sequences. A model
  contribution whose reusable artifact is not released: **no code, weights, project page, or dataset is linked** on
  the arXiv record or in the full text (checked 2026-10-26). Watch for artifact release.
- **Do VLAs Know They Will Fail?** (`2610.32208`, cs.RO/cs.AI/cs.LG) — SAE features of OpenVLA activations carry
  outcome-predictive information and can flag failure before an unrecoverable state, with a VLM interpretation
  pipeline over activation intervals. Directly on the runtime-monitoring line this list tracks, but it is a
  **4-main-page non-archival workshop paper** (Science of Physical AI Safety, CoRL 2026) with **no artifact**
  (checked 2026-10-26). Watch for the archival version.
- **When Human Corrections Travel** (`2610.32888`, cs.RO/cs.LG, SPAIS workshop at CoRL 2026) — Argues that sharing
  VLA adaptations derived from human corrections across robots needs three requirements (preserve correction context
  and uncertainty, assess updates in each receiver's closed loop, evaluate leakage alongside retention of
  safety-critical evidence). A **position paper with counterexamples and a falsifiable agenda, no experiments and
  no artifact** (checked 2026-10-26). Recorded here so a later run can pick up the eventual empirical paper.
- **4DMemory / LongVideo** (`2610.32332`, cs.CV/cs.AI/cs.CL/cs.MM) — ~900 hours of video, 5,000+ reviewed
  questions, and a **4DMemory Harness** of four skills and 22+ tool models that lifts all 13 evaluated models by
  over 50%. A genuine harness-augmented memory study, but the domain is long-video question answering with **no
  robot experiment**, and **no code, data, or benchmark release is linked** on the record or in the full text
  (checked 2026-10-26). Deferred as off-domain-with-untested-artifact rather than rejected on quality.
- **DealsFlow** (`2610.32380`, cs.AI, AI4F workshop at ICAIF 2026) — An **evidence-gated agentic harness** deployed
  at a UK early-stage fund: a stage-aware corroboration ceiling, a sceptic call that may lower but never raise a
  score, and evidence pre-fetch before a single generation pass, evaluated on 243 SEIS/EIS investments with
  recorded outcomes. It is a strong harness contract in a non-robot domain with **no public artifact**
  (checked 2026-10-26) and a workshop submission; deferred rather than included so the general-harness section
  keeps its evidence bar.
- **UAV-SwarmAgent** (`2610.32559`, cs.RO) — Hierarchical multi-agent system in which one natural-language mission
  becomes generated code that routes N UAVs, with worker agents verifying incidents; 95.7% incident location over
  255 simulated missions and an Amdahl-type scaling law for flight time. Code-as-policy for a robot fleet, but
  **simulation only and no artifact located** (checked 2026-10-26).
- **Can AI Agents Help Robotic Engineers?** (`2610.32636`, cs.SE/cs.AI) — An honest negative industry case study:
  an SME programming industrial robots in vendor-specific languages saw no productivity gain except on a
  highly repetitive code-generation task, with agent experience mattering more than robotics experience. Useful
  evidence, but a two-focus-day case study with **no artifact** and no measurement that generalises yet.
- **TTT in VLA Models** (`2610.32094`, cs.RO/cs.AI/cs.CV) — Test-time training with fast-weight memory inside
  InternVLA-A1 and π0.5, reporting that **adding TTT and paired demonstration training did not produce consistent
  demonstration-conditioned target selection** at the tested scale, including under single-frame cues on a real
  robot. Valuable negative result on the VLA-memory line, but a company tech report with **no artifact**
  (checked 2026-10-26) and no positive interface contribution.
- **From Tabletop to Walking Bot** (`2610.32271`, cs.RO) — Deployment report for loco-manipulation on a Unitree Go2
  with an AgileX Piper arm: model variant, action horizon and demonstration convention, plus a data-coverage versus
  model-capacity study. Exactly the deployment detail this list values, but with **no artifact** and no release
  beyond the write-up (checked 2026-10-26).
- **RoboCode** (`2610.32225`, cs.RO/cs.LG) — Soft-gated primitive action interface that represents an action chunk
  as a composition of learned primitives and reuses it across tasks (ManiSkill, 5 tasks). Skill-discovery-adjacent
  at the action level, but **simulation-only, five tasks, and no artifact** (checked 2026-10-26).
- **DexPort** (`2610.32969`, cs.RO/cs.AI/cs.LG) — Hand-held skill interface for dexterous-hand data collection that
  uses the same hardware for collection and deployment and records commanded targets plus measured states; the
  "small skill set covers many tasks" observation is relevant to skill composition. Website live; **no code,
  dataset, or hardware release verified** (checked 2026-10-26).
- **Predictive Long-Horizon Progress Modeling / NimCritic + LOOM** (`2610.32776`, cs.RO) — Action-aware progress
  quantification with VLM auto-labeling plus human calibration, an 800+ hour training set, and a 19,618-frame
  long-horizon progress benchmark. Relevant to harness-side progress estimation, but **no artifact linked**
  (checked 2026-10-26).
- **MemCryst** (`2610.32295`, cs.AI) — Bidirectional memory–skill learning for lifelong agents around a frozen
  language model, 42 pages. On the memory/skill line but **no artifact linked** and no robot evidence
  (checked 2026-10-26).
- **SceneBloom** (`2610.32978`, cs.CV/cs.AI/cs.RO) — Agentic indoor-environment generation for embodied learning,
  combining diffusion layout sampling with contract-guided 3D compilation. Infrastructure-adjacent, but **no
  artifact linked** on the record or in the full text (checked 2026-10-26).
- **Who Verifies the Tools?** (`2610.32110`, cs.AI/cs.SE, NeurIPS 2026 workshop) — Tool **output formatters** can
  misrepresent what a correct tool did (omitting search limits, result caps, identifier namespaces) and mislead both
  agent and verifier; the MIT repository `williamcodes/who-verifies-the-tools` is live (1.7 MB, created
  2026-09-16, pushed 2026-09-16). A real and under-appreciated interface failure, but the study is small
  (two formatter versions, workshop paper) and the robot analogue is not evaluated; watch rather than include.
- **ViT-H** (`2610.32966`, cs.RO/cs.CV), **DynaSlots** (`2610.32330`, cs.RO), **UniWorld** (`2610.32381`, cs.CV),
  **Ψ-0.6** (`2610.32177`, cs.AI/cs.CV/cs.LG), **HGWM** (`2610.32554`, cs.RO/cs.MA) — Read and rejected on scope:
  video-to-humanoid motion generation, slot-centric 3D dynamics pretraining, real-time visual simulation, an
  adaptive-streams vision-language world model, and graph-guided navigation world models respectively. Each is a
  model or navigation contribution without a runtime/interface contract for the harness, and none needed an
  artifact check.

### Read and excluded as off-scope (representative, out of 369 titles)

Generic driving (`2610.32069`, `2610.32230`, `2610.32612`, `2610.32666`), medical imaging and clinical agents
(`2610.32263`, `2610.32266`, `2610.32469`, `2610.32478`), video/text generation and editing (`2610.32058`,
`2610.32205`, `2610.32459`, `2610.32635`), perception-only or benchmark-only work (`2610.32073`, `2610.32268`,
`2610.32517`, `2610.32691`), conventional control and estimation (`2610.32260`, `2610.32487`, `2610.32645`,
`2610.32143`), and the large NLP/LLM-systems remainder (compression, quantization, training dynamics, ASR/TTS,
retrieval, evaluation of LLMs rather than harnesses). Excluded per the standing rule: a paper enters only if it
contributes a reusable agent/VLA harness, recovery, safety, or evaluation contract.

## Validation performed

- **Markdown structure.** Each of the 13 entries is a single-line list item under an existing `###` heading, in the
  established `[Title](primary link) ([paper](…), [code](…), [weights](…)) — …` shape; blank lines separate every
  entry from the following heading; no heading level was added or removed.
- **`git diff --check`.** Clean (no whitespace errors, no conflict markers).
- **Links.** Every added URL was fetched during verification: arXiv abs/HTML pages (12), GitHub repositories and raw
  `README.md`/`LICENSE` files, five Hugging Face model and dataset APIs, eight project/documentation sites (including
  the RynnUnify and RSIHub sites and the `skillcorpus/lifecycle` tree page), the GitLab repository, and the Zenodo
  DOI. No added link is unverified.
- **Dates and categories.** Every entry states the arXiv ID, the primary category, and the v1 date (all
  2026-10-23). Artifact checks are stamped **(2026-10-26)**. Category assignment follows the list's own sections:
  seven general-harness, two robot-harness, two foundation/world-model, two runtime/safety.
- **Cross-file consistency.** README badge and `Last verified:` now read **2026-10-26**, matching this record's
  filename date. The Contents blurb was rewritten for this run rather than left describing the 2026-10-24 pass.
  No duplicate: all 13 IDs and titles absent from `README.md`, `docs/`, `sources/`, `editorial/`.
- **Artifact-status discipline.** Three distinct statuses are recorded literally: verified open with a licence
  (RSIHub, MCP-Injector, SkillCorpus, Async-RBench, Astra/LogLook, RynnUnify, UniAction), source-available without a
  licence (AWARE), and no artifact located (TraceTrial-Bench, HarnessMM, When Agents Fail, Horizon × Depth,
  Backtrack). Author-reported results are labelled as such throughout; simulation-only scope is stated per entry.

## Operational notes and blockers

- **`/tmp` is not durable in this harness.** The first probe wrote `frontier.xml` under `/tmp/arxiv-2026-10-26/`,
  which was gone by the next shell invocation; the harvest was moved to the repository's untracked `.scratch/`
  directory (as previous runs did). Future runs should create the scratch directory inside the checkout first.
- **`git ls-remote` needs the OpenSSH workaround.** `git ls-remote origin` failed with
  `Bad owner or permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf`; retrying with
  `GIT_SSH_COMMAND='ssh -F /dev/null'` succeeded and confirmed `refs/heads/main` at `f6fe4cb`. The same workaround
  is required for `git push`.
- **Rate limiting was not an issue at 19–20 s spacing.** 19 requests, no 429 — consistent with the 2026-10-25 run's
  conclusion that request *spacing*, not count, is what the export endpoint punishes. The single-OR-query-per-day
  shape reduced the request count roughly 5× and is worth keeping.
- **A `bash` call without an explicit timeout is cut off at ~60 s**, which truncated the first day-count sweep
  mid-query; long harvests were re-run with an explicit large timeout and, for the multi-day sweep, as a background
  job.
- **No blockers.** No network, authentication, conflict, or artifact-verification problem prevented a step; the
  GitHub REST API did not rate-limit this run.
