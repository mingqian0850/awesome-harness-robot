# Daily arXiv scan — 2026-11-09 (the owed Monday catch-up: the Friday 2026-11-06 batch finally announces with 391 records, 2026-11-05 grows by 93 late arrivals, 2026-11-04 by 1, and thirteen entries are added)

## Scope

- **Interval owed.** The previous successful run committed at **2026-11-08T08:55** local
  (`2026-11-08T07:55Z`) on `main`, producing `e8162d2` and record `sources/daily-arxiv-2026-11-08.md`.
  That record screened the re-read 2026-11-05 batch, found 2026-11-04 at **487** and 2026-11-05 at
  **433** with **zero** late arrivals, and closed with **"Still owed: 2026-11-06, 2026-11-07 and
  2026-11-08. The next run must query all three by `submittedDate` again and must recount 2026-11-05
  and 2026-11-04 before trusting either count."** This run started **2026-11-09 09:05 UTC** on `main`
  at `e8162d2`, so the interval it owes is **submission dates 2026-11-06 → 2026-11-09 inclusive**,
  plus the mandatory recount of 2026-11-04/05.

- **The debt is now paid, and the batch that was missing is the reason.** Monday's 20:00 ET Sunday
  announcement (≈ Monday 01:00 UTC) is the slot that carries Friday's submissions, so the run that
  starts Monday morning is the first that can see 2026-11-06 at all. It returns **391** records,
  `<published>` spanning **2026-11-06T00:01:47Z → 2026-11-06T18:59:57Z**. 2026-11-07 and 2026-11-08
  return **0** because arXiv takes no weekend submissions, and 2026-11-09 returns **0** because
  Monday's own batch is not announced until Tuesday — so those three days are **empty**, not
  unharvestable, and the debt is closed rather than rolled forward.

  | query (one parenthesised OR-query over cs.RO/cs.AI/cs.CL/cs.CV/cs.LG per day) | 2026-11-04 | 2026-11-05 | 2026-11-06 | 2026-11-07 | 2026-11-08 | 2026-11-09 |
  | --- | ---: | ---: | ---: | ---: | ---: | ---: |
  | count at 2026-11-09 09:07 UTC | **488** | **526** | **391** | **0** | **0** | **0** |
  | recorded by the previous run | 487 | 433 | 0 | 0 | 0 | — |
  | **late arrivals screened by this run** | **1** | **93** | **391 (new day)** | — | — | — |

- **The index has advanced and the frontier is now inside the batch.** A descending `cs.RO` probe
  returns **`2611.09892`** at **`2026-11-06T18:59:34Z`** and a descending `cs.AI` probe the same
  record, i.e. the two probes agree and the harvest's newest record is **`2611.09894`** at
  **`2026-11-06T18:59:57Z`** — Friday's block, not Thursday's. The previous run's frontier
  (`2611.08462` / `2611.08483`, both Thursday evening) therefore no longer describes the index, which
  is the check that distinguishes "a batch landed" from "the API is returning a cached page".

- **Late arrivals resumed after the single clean run, in the direction this time unexpected.** The
  previous run recorded **zero** late arrivals for the first time in five runs; this run adds **94**
  across two days. The 2026-11-05 additions are **93** records numbered **`2611.08541`…`2611.08802`**
  and spread over the whole day (**02:12:41Z → 23:55:09Z**), not clustered above a declared frontier;
  2026-11-04 adds exactly one (`2611.08538`, *Subliminal Learning in Vision-Language Models*). The
  mechanism is visible in the numbering: the "missing" block for 2026-11-05 sits **between** two
  already-announced ranges, so a count alone would have been read as a leaderboard change. Both days
  are diffed as parsed records rather than trusted as counts.

- **Coverage.** The **391**-record 2026-11-06 batch has primaries `cs.LG` **91**, `cs.AI` **77**,
  `cs.CV` **76**, `cs.RO` **48**, `cs.CL` **33**, with **52** records carrying `cs.RO` as primary or
  cross-list. The **93** late 2026-11-05 arrivals are dominated by `cs.LG` **21**, `cs.AI` **17**,
  `cs.CV` **16**, `cs.CL` **8**, `cs.RO` **6**, with **7** touching `cs.RO`. The single 2026-11-04
  arrival is primary `cs.CV` with four cross-lists. **No record in any of the three additions is a
  revision** — every `<updated>` equals its `<published>` — so the "new submissions and meaningful
  revisions" requirement is answered on the revision side by the 2026-11-05 diff below rather than by
  any new posting.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`. **16 export-API requests**:
  **six** per-day count queries (2026-11-04 → 2026-11-09), **four** recount queries (2026-10-30,
  10-31, 11-01, 11-02, to test whether earlier days also grew), **two** descending frontier probes
  (`cs.RO`, `cs.AI`) and **four** full-day harvests (2026-11-03, 11-04, 11-05, 11-06) at
  `max_results=2000&sortBy=submittedDate&sortOrder=ascending`, returning **585 / 488 / 526 / 391**
  entries (1.43 / 1.21 / 1.29 / 0.93 MB). Spaced 3–6 s; **zero HTTP 429**. The per-day counts use the
  parenthesised form `(cat:cs.RO OR cat:cs.AI OR cat:cs.CL OR cat:cs.CV OR cat:cs.LG) AND
  submittedDate:[...]`; the brackets must be percent-encoded or `curl` rejects the range as a glob.
- **Differential diff against the previous harvest, as a set difference over parsed XML.**
  `per_day.json`/`recs_1105.json` from `.scratch/arxiv-2026-11-08/` were compared against today's parse
  of `day_20261105.xml`: **433 → 526** IDs, **93 added, 0 removed**, and a field-by-field comparison
  over `title`, `published`, `updated` and `summary` reports **26** records with a difference. Eleven of
  those are **real v2 revisions** (`updated` advanced into 2026-11-06, one of them —
  **`2611.08335`** — with a changed **title**): `2611.07295`, `2611.07393`, `2611.07551`, `2611.07648`,
  `2611.07722`, `2611.08080`, `2611.08162`, `2611.08177`, `2611.08255`, `2611.08335`, `2611.08409`.
  The other fifteen differ only in `summary` while `<updated>` is unchanged, which is a
  whitespace/normalisation artefact of the two parsers, not an edit; the record states this rather than
  reporting 26 revisions. 2026-11-04 was diffed the same way: **487 → 488**, one addition, zero
  removals. 2026-11-03 was re-harvested at **585**, the number the previous run recorded.
- **Screening.** (a) all **391** 2026-11-06 titles in ascending ID order; (b) all **93** late-arrival
  titles; (c) **full abstracts** for **30** records carrying a harness, evaluation, monitoring,
  permission, memory or robot-agent signal (four of them read twice, end to end, once the record was a
  candidate); (d) **16 full-text HTML renderings** (`arxiv.org/html/<id>v1`) read for scope statements
  and artifact claims, because two artifact claims in this batch live in the **comment field** rather
  than the abstract and one lives only in a **footnote**; (e) every claimed artifact resolved at the
  source.
- **`/tmp` remained non-durable between tool calls**, reproducing every previous run's finding, so all
  XML, HTML, JSON and page captures plus the parse and diff scripts are in the untracked
  `.scratch/arxiv-2026-11-09/` directory, which is **not staged and not committed**.

## Included (13)

Every one is new to the repository: before this run's edits a grep of `README.md`, `docs/*.md`,
`sources/*.md` and `editorial/` returned **no match for any of the thirteen IDs**.

### General Harness Methodology and Verification (4)

- **`2611.09218` — BOXBOX: Recursive Harness Self-Improvement for Long-Running Performance
  Optimization** (cs.SE, cs.AI; 47 pages, 22 figures, 12 tables including appendices). The batch's
  harness paper and the cleanest statement of the regime the offline line brackets away: an open-ended
  task keeps producing difficulties a harness tuned before the run never saw. BOXBOX adds
  **agent-triggered harness revision** under a **kernel-managed hot reloading** step that **preserves
  execution context**, and introduces **PerfRace**, a benchmark of long-running software-performance
  optimization built to study it. Under **matched test-time budgets** against Frozen Harness, offline
  Harness Evolution and Harness Scaling the authors report **Speedup@4h 1.70×** against **1.54×** for
  the strongest baseline, at lower task time and model cost. Coding-agent evidence; no robot
  experiment; author-reported. **Artifact status literal (verified 2026-11-09): nothing released, and
  the project link is not a project page.** `http://aka.ms/boxbox` is a **301 into a private Overleaf
  project** (`overleaf.com/project/6ae04…`) returning **403** anonymously; no repository, dataset,
  weights or benchmark appears on the record or in the full text, so **PerfRace cannot be obtained**.
- **`2611.09088` — When Routine Operations Break Architectural Pattern Relationships in LLM Agent
  Harnesses** (cs.SE, cs.AI). Names a class of failure that per-component testing cannot see: harnesses
  spread tool mediation, state management and guardrails across native services, independently written
  plugins and configuration, so a **routine operation** can change the runtime order or retained state
  a pattern relationship depends on **without any code change**. Five relationships — Guardrail
  Enforcement Point, Autonomy Controller, Blackboard and two more — over **five released harnesses**,
  main sample **fourteen independently written plugins**; **PatternOps** traces each requirement from
  the pattern description, provider documentation and deployment profile to the provider that realises
  it and the record that decides it, then compares alternative allocations under supported operations.
  The observed failure is the entry's justification: after a plugin was reapplied or an allow rule
  saved, **policy stopped evaluating edits while every plugin stayed active, and a prohibited edit
  executed**. The general claim — a pattern relationship is a property of the **running system and its
  history** — is what a harness audit has to adopt. Digital-agent harnesses; author-reported.
  **Artifact status literal (verified 2026-11-09): the evidence repository returns 401.** The full text
  points to `anonymous.4open.science/r/pattern-relationships-supplement-C35A/`; an anonymous fetch
  redirects to the API file endpoint and **401s**, so the procedure and its evidence cannot be
  inspected.
- **`2611.09802` — When Instruction Files Go Stale: Documentation Debt and Hidden Cost in Coding
  Agents** (cs.SE, cs.AI; 11 pages). Measures the artifact every coding-agent harness reads in every
  session and no harness evaluates: the repository instruction file (`AGENTS.md`, `CLAUDE.md`, context
  files). A stale instruction is **documentation debt**, and its interest is charged to every later
  session — invisible to a task-success metric. Three real stale instructions from active open-source
  repositories (a **test command** in `pyflow`, a **code location** in `cog`, a **procedure** in
  `aghub`), each under **three versions** (stale, corrected, removed) with code and task held fixed,
  **22 maintenance tasks**, **732 trajectories** of two models analysed **tool call by tool call**. The
  authors report agents following stale guidance when it is task-relevant **even with correct
  information available elsewhere**; on the stale test command Claude Sonnet 5.5 needed about **two
  extra tool calls per run, a third more** than with the corrected command, while the other two cases
  showed no detectable difference. Two reusable findings: agents recovered from immediate failures but
  **none repaired the underlying instruction file**, and correcting the instruction eliminated
  operations on obsolete paths while removing it prevented most. **Artifact verified open
  (2026-11-09): CC-BY-4.0 replication package**, Zenodo **`10.5281/zenodo.23992004`** (published
  2026-11-06, North Carolina State University). Digital coding agents; no robot experiment.
- **`2611.08946` — Flex-RSI: Benchmarking Recursive Self-Improvement of Frontier Agents** (cs.AI,
  cs.CV, cs.RO). Makes the **harness the scored unit**: each round an **executor** improves the
  solution (kept only if it scores higher on **withheld validation** data) and an **improver** revises
  the harness **including itself**, every run starts from the **same general-purpose harness**, and the
  final solution is scored once on hidden test data under a budget covering wall-clock time, GPU
  hours, model usage, validation evaluations and round count. Tasks span perception, reasoning,
  **robot action** (RoboDojo, RoboLab, RoboCasa), 3D reconstruction, visual creation and
  infrastructure. Findings: self-improvement helps **most when the agent builds its own solution**;
  recording feedback as bare success/failure **holds back** open-weight models; open-weight models fall
  furthest behind where outputs must pass strict checks of **physical stability or exact
  correctness**; and **paired runs show the harness edit itself contributes**. **Artifact status
  literal (verified 2026-11-09): real code, no licence declared.** `flex-rsi/rsi-bench` (649 KB,
  created and pushed 2026-11-04) holds `harness/executor/`, `harness/improver/`, `harness/core/`,
  `protocol/`, `tasks/`, `tests/` and English/Chinese READMEs stating the executor–improver split and
  the three-split budget protocol; the repository **declares no licence**, and the project page's
  controls still read **"Report soon"** and **"Leaderboard soon"**.

### Agentic Robot/VLA Harness (4)

- **`2611.09892` — On Agentic Manipulation Policies** (cs.RO, cs.AI, cs.CV, cs.LG; Apple). Sends a
  generalist model at the arm instead of a specialist policy at the task. **AMP1** is a **reference
  harness that runs unchanged across single-arm, bimanual and mobile manipulation**: the model writes
  and runs Python that calls the harness for **sensing** (RGB-D, calibration, fused point cloud),
  **perception** (a scene model it can correct) and **motion** (timed end-effector waypoints and
  gripper commands), choosing what to observe and checking each outcome before acting again. Reported:
  **85.6%** zero-shot on **RoboLab-120** against **49.2%** (VLA) and **42.9%** (WAM); on the project
  page **95.2%** LIBERO-PRO, **100%** robosuite, **96.0%** BEHAVIOR-1K, **84.1%** RoboDojo; and
  **104/108** real-world trials across **twelve tasks and three robot instances**, a **20×** and
  **15×** failure-rate reduction — **with the basic harness supplying no grasp, push or place skill**,
  the agent instead searching for hidden objects, using objects as tools and calibrating its cameras;
  a **persistent memory store** carried across episodes lowers the failure rate further. Real robots
  plus simulation; author-reported. **Artifact verified open (2026-11-09), non-OSI licence:**
  `apple-aiml-research/ml-amp` (209 MB, **1,633** files, created 2026-11-07, pushed 2026-11-09) ships
  the harness in **`src/amp/agents/`** (control loop, `amp1.md` system prompt, recording, model
  adapters) plus `docs/` with per-benchmark reproduction commands and pinned third-party commits, under
  **Apple's own sample-code licence** (use/modify/redistribute granted; no patent grant; not OSI). The
  harness calls a hosted model by API key, so **no weights** are claimed or needed. Placed in *Agentic
  Robot and VLA Harnesses*.
- **`2611.09030` — A Controlled Study of Models, Skills, Feedback, and Curriculum in Agentic
  Robotics** (cs.RO, cs.AI, cs.LG). The batch's best-controlled answer to "what makes the agentic-robot
  loop work": **nine** manipulation tasks, **631** experiment configurations, **28,000** coding-agent
  rollouts, varying **model capability, skill priors, feedback structure and curriculum**. The authors
  report the bottleneck is often **not access to sufficient information but interpreting it into the
  right policy update**, with a working loop depending on **failure attribution**, **task-relevant
  spatial and geometric reasoning**, and **selective code restructuring**. Simulation study;
  author-reported. **Artifact status literal (verified 2026-11-09): none located** — no repository,
  dataset, weights or project page on the record or in the full text, so neither the 631
  configurations nor the rollout corpus can be re-run. Placed in *Agentic Robot and VLA Harnesses*.
- **`2611.09124` — Toward the Era of Agentic Embodied Intelligence** (cs.RO). Makes **code the scaling
  interface** and separates what is learned from what is looked up at runtime: **A²-MEAS** absorbs
  learned correction codes (corrective language, spatial evidence, execution history, action proposals)
  into policy weights through a correction encoder and adapter during training, while **fresh codes
  adapt behaviour with frozen weights** at deployment, and **MetaBrain** supplies both training
  supervision and runtime guidance; **A²-WLA** adds motion-aware history and a latent world model to
  the policy. Reported: **0.5755** Q-score on the **50-task BEHAVIOR-2026 public subset** before online
  correction, **0.6411** Q-score and **45.40%** task success over all 100 public tasks, **93.98% →**
  **95.05%** on RoboTwin 2.0, **57.92%** against **26.67%** for π0.5 across **twelve real-robot tasks
  in five household settings**, and a history-conditioned adapter raising **post-failure completion**
  at two filter-handling steps of a long-horizon pour-over coffee task. Simulation plus real robots;
  author-reported. **Artifacts verified open (2026-11-09): Apache-2.0 code, ungated weights, Docker
  image** — `AutoLab-SAI-SJTU/A2MEAS` (1.8 MB, **1,281** tree entries, created 2026-10-01, pushed
  2026-11-09) carries the correction stack as source (`gate.py`, `controller.py`, `grounding.py`,
  `review.py`, `adapter.py`), and `shibohuang/policy-weights` is public, ungated and Apache-2.0 with
  `behavior/`, `robotwin/`, `adapter/` and `weights/` groups. The full text's "we **will** release the
  system, models, training scripts" is stale rather than false.
- **`2611.09847` — Computer-Use Agents Are Embodied Generalists** (cs.LG, cs.RO; Carnegie Mellon).
  A position paper whose argument is carried by the **interface**, which is why it qualifies: embodied
  tasks are formulated as **interaction with digital software**. The authors build software that
  renders the agent's view of a physical environment on a screen with controls for acting in it, so a
  **computer-use agent** performs embodied tasks through the same screenshots and mouse/keyboard
  actions it uses on ordinary software. One CUA design, only the software changed: **first, second,
  third and sixth place** across four ECCV 2026 workshop competitions (VLNVerse, DialNav, OpenSUN3D,
  MeViSv2-Text) with **no competition-specific tuning**. No robot policy trained, no physical robot
  driven. **Artifact verified open but unlicensed (2026-11-09):** `Pranjal2041/cua-embodied` (1.14 MB,
  created 2026-11-05, pushed 2026-11-09) is the real implementation — `agent.py` (one system prompt, one
  Gemini model, exactly one tool per turn, images aged out of context), `computer/` (`mouse` and
  `keyboard` only), `apps/` (one package per competition), `gui/` (a local server the agent never calls
  — it only sees the page) and `run.py`; the repository **declares no licence**.

### VLA (1)

- **`2611.08577` — Meituan-Robotics-0: A Vision-Language-Action Foundation Model for Desktop
  Manipulation** (cs.RO, cs.CV; 25 pages). A complete open VLA whose **training stages and deployment
  harness ship together**: **147M** vision-language samples plus **15,060 hours** over **12
  embodiments** through FAST action-token pre-training, an **MMDiT flow-matching mid-training**
  transfer and per-suite post-training, behind a **unified end-effector interface** and **asynchronous
  action-chunk merging**. Two harness-owned additions: **Rollback** pairs replaced policy segments with
  human corrections so CFG-DAgger gets positive and negative supervision **without value learning or
  advantage labeling**, and an **event-driven agent** keeps persistent task events, reassesses plans
  when progress or feasibility changes, and supports cross-task retrieval and conditional resumption.
  Reported: **93.6%** RoboTwin 2.0 (the release documents **93.22%** clean / **93.88%** randomized over
  50 tasks × 100 episodes with unseen instructions), fifth in the RoboDojo VLA track, better average
  success and progress than π0.5 across three real-robot tasks under matched HG-DAgger post-training,
  and **70%** / **0.90** progress on Ethernet insertion for Rollback-trained CFG-DAgger.
  **Artifacts verified open (2026-11-09): MIT code, four ungated Apache-2.0 checkpoints, CC BY-NC-SA
  4.0 dataset** — the `opensource` branch of `Meituan-Robotics/Meituan-Robotics-0` (418 MB, created
  2026-11-05, pushed 2026-11-09) carries the model, dataloaders, training launchers, a RoboTwin
  evaluation interface, an **OpenPI-compatible policy server**, deployment docs and twelve tests;
  `MeituanRobotics/Meituan-Robotics-0` and `-RoboTwin` each hold an **11.37 GB** `checkpoints/model.pt`
  with `SHA256SUMS` and `release.json`, `-RoboDojo` and `-RoboCasa365` are also public and ungated, and
  `RoboDesk` is a public dataset. **Documentation drift recorded literally:** the repository's own
  `docs/model_release.md` still reads *"No weight upload has been performed by this README update.
  Download links remain placeholders"*, while the four Hugging Face repositories hold the named
  checkpoints with checksums — the weights are live and that file is stale. Placed in *VLA → Open and
  Reproducible*.

### Evaluation (2)

- **`2611.09130` — Auditing Grasp Feasibility Predictors with Planner-Verified Counterfactuals**
  (cs.RO; WACV 2027, 10+8 pages). Shows a benchmark can certify the wrong competence and supplies the
  intervention that separates them. Grasp-feasibility predictors report **near-perfect** TAMP accuracy,
  which does not establish that a predictor reads the **collision geometry the planner checks**. Hold
  the grasp pose fixed, insert a solid obstacle into the approach corridor, add its points to the input,
  and let a planner **re-derive the label** — it flips to infeasible while the model can see the
  obstacle. Across **eight** predictors (point clouds, voxels, depth, object tokens) **CF-pair AUROC
  falls from natural 0.920–0.996 to chance, 0.470–0.504**, across **four planners, two arms and a
  real-object obstacle**, with failures at resolutions from a bounding box to a **12.5 cm voxel**; the
  dissection finds the obstacle either moves the representation in an unrelated direction or is **not
  registered at all**. The constructive result is bounded and says so: a **clearance-aware read**
  trained on the **unmodified** benchmark with no counterfactual scenes reaches **0.983** CF-pair AUROC
  yet remains bound to its trained obstacle placements. **Artifact verified open (2026-11-09): MIT
  `codewithsajid/graspfc-cf`** (3.5 MB, created 2026-06-26, pushed 2026-09-05) with the counterfactual
  benchmark, three interventional metrics, the relabeling pipeline and an audit harness that scores
  obstacle-awareness **with no motion planner in the loop** (`audit/run_audit.py`, `docs/protocol.md`,
  `docs/datasheet.md`, `docs/leaderboard.md`, `graspfc_cf/`). Placed in *Unified Evaluation*.
- **`2611.09792` — Knowing When to Intervene: Causal Evaluation of AI Agents** (cs.AI, cs.LG,
  econ.EM, stat.ME). Moves the evaluation question from "did it succeed" to **"was that intervention
  worth making"**, which is what a deployment harness decides at runtime. Benchmarks are static and so
  say nothing about the value of verifying an output or steering behaviour **inside** a trajectory;
  that value is a causal effect on the final outcome that depends on context, environment and the
  agent's future behaviour. This is the first methodologically grounded framework for estimating
  **dynamic causal effects of randomized interventions**: from a relatively small set of rollouts
  collected under randomized interventions it identifies the effect of a rich class of contextual
  intervention policies, and the structural parameters build **safety leaderboards** ranking agentic
  actions by risk for audit prioritisation. Applied to **τ-bench Retail, ToolSandbox and
  ImpossibleBench**, the authors report intervention effects **varying substantially across actions and
  states**. Digital agents; author-reported. **Artifact verified open but unlicensed (2026-11-09):**
  `sawarniayush/agent-evaluation-sequential-randomization` (5.0 MB, created 2026-11-05, pushed
  2026-11-06) carries code, analysis data and figures with a reproducing README documenting the
  analysed counts — **2,280** per agent on τ-bench Retail, **3,096** on ToolSandbox, **1,218** on
  ImpossibleBench and **4,560** trajectories for the direct-policy comparison; the repository
  **declares no licence**. Placed in *Unified Evaluation*.

### Safety (2)

- **`2611.09113` — Authorization Drift in LLM Agents: Evaluating Boundary Compliance in Multi-Step Tool
  Use** (cs.CR, cs.LG). Separates over-reach from adversarial input: an agent can **exceed its
  authorization boundaries while pursuing the user's goal with no adversary present**. Formalised as
  **authorization drift** over **data, tool, scope and time**, with the **Authorization Drift Benchmark
  (ADB)**: **648** samples, **seven** domains, each pairing a **sufficient compliant path** with an
  **attractive unauthorized shortcut**, validated by *Staged Execution-Reflective Synthesis* on real
  agent rollouts rather than LLM self-critique. Over **13 LLMs**, **three frameworks** and **25,207**
  trajectories: **greater capability accompanies greater drift** (**0.43–0.61** for frontier-capable
  models) while **86.18%** of tasks admit an observed clean-success trajectory; drift is **persistent**,
  dominated by **early data over-collection**, and apparent scaffold improvements **coincide with
  collapsing clean-completion**. Drift points are mapped to legal provisions in four jurisdictions, with
  data minimisation most frequently implicated and **93.5%** of legally grounded drifting runs spanning
  several. **Artifact status literal (verified 2026-11-09): the repository is narrower than the claim.**
  The full text says "our dataset and code are available" at `github.com/ZJR-FZD/AgentDrift`; that
  repository (MIT, 103 KB, created 2026-07-23, pushed 2026-11-09) is the **data-synthesis / QC tooling**
  and its own README states it "contains only the synthesis / quality-control tooling", that "**Benchmark
  data is not included** and will not be published in this repo", and lists raw skill corpora, generated
  data and environments, the **formal evaluation harness** and rollout results under **"Not
  included"** — so **ADB is not released**. Placed in *Safety*.
- **`2611.08895` — Localizing and Editing the Goal Representation of a Vision-Language-Action Model**
  (cs.RO). Tests whether a prohibition stated in language constrains a VLA at all, and finds it does
  not. The setting is readable and that is why it is credible: the **10 tasks of LIBERO-Goal share one
  scene**, so the executed goal can be labelled. Closed-loop rollouts plus **activation patching, linear
  probes, activation steering and runtime monitors**: **negation had no detectable effect** on which
  task OpenVLA executed — it executed the **forbidden task in 79%** of negated rollouts and completed
  **78%** of plain tasks, and a goal probe reads out similarly under "not X" and "X". The localisation
  transfers: the first action depends mostly on **instruction tokens before layer 12** and on the **last
  prompt token from layer 16 onward**; a fixed steering vector at instruction tokens at layer 8
  redirected **12/18** rollouts, fixed vectors and prefill patches at layers 12–20 redirected **none of
  158**, and from layer 16 only patches at **every decoded action token** worked (**15/18** at layer
  24); with a mid-rollout instruction change the **layer-32 goal read-out detects non-compliance better
  than failure detectors**, and an **external probe on image and instruction features** is comparable —
  so the authors conclude prohibitions must be **enforced outside the instruction** and monitored
  through goal read-outs. **Artifact verified open (2026-11-09): MIT code plus a Zenodo archive** —
  `sudsho/vla-goal-representation` (2.1 MB, created and pushed 2026-11-01) carries code, experiment
  plans, episode summaries and analysis outputs, archived at Zenodo **`10.5281/zenodo.23871372`**.
  Placed in *Safety*.

## Rejected and watch list

- **Watch list, with the artifact state recorded literally.**
  **`2611.08914` HomeLoop** (cs.RO, cs.AI) — an Isaac Sim **simulation framework and benchmark for
  robot-IoT coordination** with a **runtime monitor scoring each episode by temporal-logic checks on the
  final outcome and on the actions along the way**, 20 tasks over five household-hazard families
  designed so the same alert can have different causes, and six VLMs evaluated with the best at
  **61.9%** family-balanced score and a documented **over-reaction** failure pattern. It is squarely on
  this list's evaluation-and-monitoring thread, but **no code, data, benchmark or project link appears
  on the record or in the full text** (verified 2026-11-09), so nothing can be re-run.
  **`2611.09721` MemoryDojo** (cs.CR, cs.AI, cs.CL) — a **155**-trajectory, four-domain, five-memory-system
  benchmark that traces a poisoning from storage to retrieval to action, with a memory-aware poison
  optimiser; a real result about the memory harness (all five systems are susceptible, with wide
  variation) and **no artifact link anywhere** (verified 2026-11-09).
  **`2611.08905` From Procedures to Permissions** (cs.CL) — learns **per-subagent tool and workspace
  authorization from Agent Skills**, reporting **+13.4 pp** tool-permission precision, **+24.1 pp**
  workspace-permission precision and **+11.6 pp** task success on ClawArena-Team for
  Qwen3.5-35B-A3B, competitive on Terminal-Bench 2.1 and SkillsBench. The clearest statement of
  permissions as a harness surface in the batch, but **no artifact claimed or located** (2026-11-09).
  **`2611.08768` Conflux** (cs.CR, cs.AI) — a **principal-context defence** that puts privilege-escalation
  enforcement **outside model judgement**, with a conditional authority-confinement theorem and two
  system-level evaluators (SLED black-box, SLED-V white-box). Strong; **no artifact claimed or
  located** (2026-11-09).
  **`2611.08987` VLA Handover** (cs.RO, cs.LG) — a **deployment harness** result: a small on-robot VLA
  with a **failure-prediction trigger** to a large server VLA, formulated as minimizing request cost
  under a required success probability; **+11–22 pp** over the small VLA alone and **49–77% fewer
  requests** than server-only control at **2–4 pp** lower success, with completion time growing less
  with latency. Held back for scope rather than quality: **six pages, LIBERO simulation only, no
  artifact**, submitted to ICC 2027.
- **Robot/embodied candidates re-read and rejected on scope** (models, controllers, perception or
  simulation results without an external runtime/interface contract this list would own):
  **`2611.08917` PRISM** (NeurIPS 2026; skill primitives as per-skill low-rank action-expert adapters
  with a per-chunk VLM-conditioned router — a model-side answer to skill retention, no artifact),
  **`2611.08698` VICTR** (value-informed in-context retrieval for π0.5; a model improvement, no
  artifact), **`2611.09684` SURE** (spatiotemporal uncertainty in VLA training), **`2611.09355` HiMEM**
  (hippocampus-inspired memory consolidation for VLAs in 3D scenes), **`2611.09672` ODEAction**,
  **`2611.08827` ActPrune**, **`2611.09145` Augmentron**, **`2611.09293` MDF++** (visuo-tactile world
  action model), **`2611.09340` Horizon-Coupled Obstacle Guidance**, **`2611.09670` Goodhart in the
  Planner** (sampling-based control exploiting learned rewards — strong safety content, model-side
  framing), **`2611.09376` Can LLMs Debug Robots by Editing Their Weights?** (a genuinely interesting
  **weight-editing harness** with a CPU workspace and budgeted simulator feedback, but the finding is
  precise lexical repair rather than general capability, and no artifact is claimed),
  **`2611.09605` DKWBC**, **`2611.09777` snake-robot loco-manipulation**, **`2611.09864` Grasp-TAGS**,
  **`2611.09266` SUGARv2**, **`2611.09794` Seeing What Matters** (a task-conditioned spatial-belief
  benchmark with 860 cases and **115,000+** evaluator episodes — promising, but the record states
  extended evaluation and **physical-robot validation in progress**), **`2611.08930` Wellness Robots
  and the Path to Full Autonomy**.
- **Generic driving, CPS and infrastructure candidates rejected on scope.** **`2611.09888` Scenario
  Orchestration** (closed-loop AD policy evaluation that separates **scenario realization from policy
  failure** — a good evaluation contract, but autonomous driving), **`2611.09364` SafeOrchestrate**
  (runtime-checked intent-driven CPS orchestration over **MCP** affordances with **SMT**-backed
  guardrails — harness-shaped, but smart-home/greenhouse/industrial rather than robotic),
  **`2611.08684` Cancellation Is Not Enough** (retry and cancellation behaviour of a vLLM engine under
  overload — a serving study), **`2611.09729` One Run Is Not Enough** (a Monte Carlo reliability
  protocol for data-science QA), **`2611.09348` When Feedback Becomes a Backdoor** (backdoor formation
  through poisoned reference answers in text-space skill optimisation; a NeurIPS workshop poster),
  and the two VLA goal-understanding follow-ups that did not need a second entry.
- **Re-confirmed exclusions are not re-litigated.** The six inclusions and the watch-list verdicts in
  `sources/daily-arxiv-2026-11-08.md` stand as written, as do the earlier 2026-11-07 and 2026-11-05
  records. This run re-read the owed interval to find **what had newly arrived**, not to reopen
  decisions taken on evidence.

## Validation performed

- **Markdown structure.** `README.md` parses as valid Markdown: **17** `##` and **22** `###` headings,
  identical to `HEAD`, so no section was added, removed or reordered; each of the thirteen new items is
  a single top-level bullet inside an existing section, the VLA entry is one additional table row plus
  one bullet in the established pattern, and no nested list was broken. The new record has one `#`
  heading and uses only `##`/`###` below it.
- **`git diff --check`** reports no whitespace errors, no trailing whitespace and no conflict markers.
- **Links.** Every added link was resolved from this host on 2026-11-09: `arxiv.org/abs/{2611.09218,
  2611.09088, 2611.09802, 2611.08946, 2611.09892, 2611.09030, 2611.09124, 2611.09847, 2611.08577,
  2611.09130, 2611.09792, 2611.09113, 2611.08895}` (HTML renderings fetched), `doi.org/10.5281/
  zenodo.23992004` (200 → CC-BY-4.0 record) and `doi.org/10.5281/zenodo.23871372` (200),
  `apple-aiml-research.github.io/ml-amp/` (200), `flex-rsi.github.io` (200), `a2-meas.github.io` (200),
  `meituan-robotics.github.io/Meituan-Robotics-0/` (200), and the repositories
  `apple-aiml-research/ml-amp` (Apple sample-code licence), `codewithsajid/graspfc-cf` (MIT),
  `Meituan-Robotics/Meituan-Robotics-0` (MIT), `AutoLab-SAI-SJTU/A2MEAS` (Apache-2.0),
  `sudsho/vla-goal-representation` (MIT), `ZJR-FZD/AgentDrift` (MIT), `sawarniayush/agent-evaluation-
  sequential-randomization` (none), `flex-rsi/rsi-bench` (none), `Pranjal2041/cua-embodied` (none), plus
  the ungated Hugging Face weight repositories `MeituanRobotics/{Meituan-Robotics-0, -RoboTwin,
  -RoboDojo, -RoboCasa365}` and `shibohuang/policy-weights`. The **two deliberate non-additions** are
  recorded because they resolve *badly*: `aka.ms/boxbox` (301 → private Overleaf, 403) and
  `anonymous.4open.science/r/pattern-relationships-supplement-C35A/` (401).
- **Duplicate IDs.** All thirteen IDs were grepped against `README.md`, `docs/*.md`, `sources/*.md`,
  `editorial/` and `CONTRIBUTING.md` **before** insertion and **none was present**; after the edit each
  new ID occurs in exactly one entry (and, where an entry also cites the arXiv URL in its `[paper]`
  link, exactly twice), with no cross-section duplication.
- **Dates and categories.** Every included entry states its arXiv ID, and each `<published>` date is
  **2026-11-05** (the 93 late arrivals screened) or **2026-11-06** (the new batch) — never 2026-11-07/08/
  09, because no such record exists. Category placement follows the contribution rather than the arXiv
  primary: `2611.09218` (cs.SE), `2611.09088` (cs.SE), `2611.09802` (cs.SE) and `2611.08946` (cs.AI
  primary) sit under *General Harness Design and Self-Improvement*; the four `cs.RO`-centred agent
  entries sit under *Agentic Robot and VLA Harnesses*; `2611.08577` (cs.RO primary) is placed in
  *VLA → Open and Reproducible* by contribution; `2611.09130` and `2611.09792` under *Unified
  Evaluation*; `2611.09113` and `2611.08895` under *Safety*.
- **Cross-file consistency.** The README badge, the `## Current Landscape` `Last verified:` line and
  the Contents blurb all now read **2026-11-09**, matching this record's filename. `docs/landscape.md`
  keeps its earlier verification date because it is a historical note rather than a maintenance
  surface.
- **Artifact-status discipline.** Each of the thirteen entries states exactly one literal status:
  **verified open with a licence** (`Zenodo` CC-BY-4.0; MIT `graspfc-cf`, `vla-goal-representation`,
  `Meituan-Robotics-0`, and MIT-plus-Zenodo for the goal-representation study; Apache-2.0
  `A2MEAS` plus ungated Apache-2.0 weights), **verified open without a licence** (`flex-rsi/rsi-bench`,
  `sawarniayush/…`, `Pranjal2041/cua-embodied`), **verified open under a non-OSI licence**
  (`apple-aiml-research/ml-amp`, Apple sample-code), and **not released** (`BOXBOX` — 403 private
  Overleaf; `PatternOps` — 401; `A Controlled Study` — nothing located; **ADB** — the repository
  explicitly excludes the benchmark data and the evaluation harness). No entry describes a promise, a
  placeholder or a private link as open.
- **Author-versus-independent separation.** Every number in all thirteen entries is marked as what the
  authors report; no paper in this batch has an independent reproduction. Evidence scope is stated per
  entry: digital-agent-only for `2611.09218`, `2611.09088`, `2611.09802`, `2611.09113` and
  `2611.09792`; harness-audit over released harnesses for `2611.09088`; simulation-only for
  `2611.09030` and `2611.09130`; vision/robotics benchmark tasks for `2611.08946`; vision-language
  competition entries for `2611.09847`; simulation plus real robots for `2611.09892`, `2611.09124` and
  `2611.08577`; and real LIBERO rollouts for `2611.08895`. One entry carries an author-measured
  **documentation drift** rather than a number: MR0's `docs/model_release.md` still says the weight
  upload has not happened while four ungated 11.37 GB checkpoints are live.

## Operational notes and blockers

- **The weekend-lag rule finally paid what it promised, and the shape of the payment was predicted by
  the batch boundary rather than by the clock.** Two runs in a row wrote "2026-11-06 is owed"; the
  Monday catch-up is the first slot that can see it, and it holds **391** records — larger than either
  of the two days screened before it. The general lesson for the next run is that the *first* slot after
  a weekend announcement should be budgeted for a full batch, not for a tail.
- **"Late arrivals" is not a one-directional phenomenon, and this run found the counter-example.** Four
  consecutive runs observed a screened day growing *above* the declared frontier, which suggested a
  frontier-tail mechanism; the **93** additions here sit **strictly inside** the 2026-11-05 ID range
  (`2611.08541`…`2611.08802`, between already-announced `2611.08538`-adjacent and `2611.08802`
  records), with `<published>` spread across the whole day. The only reliable detector is the full-day
  re-harvest plus a set difference over parsed records; a count comparison, or a frontier probe, would
  have reported "no change" or "only the usual tail".
- **A stale documentation file inside a live artifact can invert an artifact verdict, so both must be
  read.** MR0's repository ships `docs/model_release.md` saying **"No weight upload has been performed
  … Download links remain placeholders … pending"** while the four Hugging Face repositories hold
  11.37 GB checkpoints with `SHA256SUMS`, `release.json` and per-checkpoint evaluation metadata. Reading
  only the repository would have produced a "weights not released" verdict; reading only Hugging Face
  would have produced "released" with no note of the contradiction. The entry states the measured state
  and the drift.
- **A benchmark's own repository can be the evidence that the benchmark is not released.** `2611.09113`
  cites its repository as making "dataset and code available"; that repository's README says the data
  "is not included and will not be published in this repo" and lists the evaluation harness among what
  is excluded. This is a third kind of artifact verdict, distinct from 404 and from "coming soon": the
  artifact exists, resolves, and **explicitly declines** to carry the thing the paper is about.
- **A "project page" link can be a private authoring tool.** `aka.ms/boxbox` resolves through a 301 to
  `overleaf.com/project/<id>` and 403s — a paper under review pointing at its own source, not at a
  release. Recording it as "project page live" would have mis-stated the artifact state of the batch's
  most substantial harness paper.
- **`/tmp` is still non-durable across tool calls**, confirmed again; `.scratch/arxiv-2026-11-09/`
  holds every response, the four day harvests, the parse and diff scripts, the sixteen full-text
  renderings, and the GitHub/Hugging Face JSON, and is **not staged and not committed**.
- **OpenSSH workaround still required.** `git ls-remote`/`git fetch`/`git push` fail with *"Bad owner or
  permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf"*; `GIT_SSH_COMMAND='ssh -F /dev/null'`
  restores access with the user's key and `known_hosts` unaffected. Before this run, `origin/main` and
  local `HEAD` were both `e8162d2`.
- **Nothing is owed as a withheld day.** 2026-11-07 and 2026-11-08 return **0** because arXiv takes no
  weekend submissions and 2026-11-09 returns **0** because Monday's batch announces on Tuesday, so the
  debt carried since 2026-11-05 is now cleared. **The next run must query 2026-11-09 and 2026-11-10**
  and **must re-harvest 2026-11-06 and 2026-11-05 fully** rather than trust their counts, since both
  have already moved once after being recorded.
