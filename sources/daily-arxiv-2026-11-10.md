# Daily arXiv scan — 2026-11-10 (the "empty" weekend days were unindexed, not empty: 321 + 264 records appear on 2026-11-07/08, the Monday batch adds 423, 145 late arrivals and 57 revisions follow, one listed paper is withdrawn, and sixteen entries are added)

## Scope

- **Interval owed.** The previous successful run committed at **2026-11-09** (`a6a54f8`) and produced
  record `sources/daily-arxiv-2026-11-09.md`. That record closed by naming the debt explicitly:
  **"The next run must query 2026-11-09 and 2026-11-10 and must re-harvest 2026-11-06 and 2026-11-05
  fully rather than trust their counts, since both have already moved once after being recorded."**
  It also recorded **2026-11-07 = 0** and **2026-11-08 = 0**, attributing those zeros to arXiv taking no
  weekend submissions.
- **This run** started **2026-11-10 14:23 UTC** (**15:23 CET**) on `main` at `a6a54f8`, with
  `origin/main` also at `a6a54f8`. The interval it owes is **submission dates 2026-11-07 → 2026-11-10
  inclusive**, plus the mandated full re-harvest of 2026-11-05/06 and a recount of 2026-11-03/04.
- **The headline finding is a correction, not an addition.** Re-querying the two ranges the previous run
  recorded as zero returns **321** records for **2026-11-07** (`<published>` spanning
  `00:02:17Z → 23:51:27Z`) and **264** for **2026-11-08** (`00:02:50Z → 23:40:37Z`). Those submissions
  existed; the API had not indexed them at the previous run's query time. The spread also refutes the
  mechanism rather than only the count: these are not a handful of delayed stragglers but a full day of
  submissions running past 23:00 UTC on both weekend dates. The claim "arXiv takes no weekend
  submissions" is therefore **withdrawn** in this record, and the README's Contents blurb, which carried
  it, is rewritten.
- **The day that was actually waiting was Monday.** 2026-11-09 returns **423** records
  (`<published>` `00:00:16Z → 18:59:54Z`, the batch cutting off in the evening as usual) and 2026-11-10
  returns **0**, because Tuesday's own batch is not announced until Wednesday 01:00 UTC. So the debt is
  **not** carried forward: 2026-11-10 is **empty-but-owed-for-tomorrow**, and the next run must query
  2026-11-10 and 2026-11-11.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`, one parenthesised OR-query per
  day: `(cat:cs.RO OR cat:cs.AI OR cat:cs.CL OR cat:cs.CV OR cat:cs.LG) AND submittedDate:[YYYYMMDD0000
  TO YYYYMMDD2359]` with brackets percent-encoded. **22 export-API requests**, spaced 3–6 s: **ten**
  day-count requests (eight days, with 2026-11-09 re-queried twice after internal errors), **one** extra
  count probe on 2026-11-06, **two** single-category diagnostic probes, **two** descending frontier
  probes (`cs.RO`, `cs.AI`), and **seven** full-day harvests at
  `max_results=2000&sortBy=submittedDate&sortOrder=ascending`.
- **Three of the 22 requests returned the API's own error entry** — `<title>Error</title>`,
  `<summary>The server encountered an internal error…</summary>` wrapped in a normal 200 feed with
  `totalResults` of 1. They were **not** read as "zero results"; each was retried after a pause and
  succeeded. This matters: a checker that trusts `totalResults` would have recorded a spurious zero
  with a green HTTP status. The error signature (a single `<entry>` whose id is
  `https://arxiv.org/api/errors`) is now documented here for later runs.
- **Full-day harvests return exactly their counted totals**, so truncation is excluded:
  2026-11-03 **593**, 11-04 **497**, 11-05 **546**, 11-06 **499**, 11-07 **321**, 11-08 **264**,
  11-09 **423** (1.45 / 1.24 / 1.34 / 1.20 / 0.78 / 0.64 / 1.04 MB).
- **Differential diff against the previous harvest, as a set difference over parsed XML**, not counts.
  The previous run's `recs_20261103..06.json` were loaded and compared field-by-field over
  `title`, `published`, `updated` and `summary`. IDs are keyed **without** the `vN` suffix (the two
  harvests store versions differently; a naive comparison reports "585 removed, 593 added" for a day
  that changed by 8 — the first parse of this run did exactly that and was corrected).
- **Frontier probe.** Descending `cs.RO` returns **`2611.12885`** at **`2026-11-09T18:59:45Z`** and
  descending `cs.AI` returns **`2611.12872`** at **`2026-11-09T18:57:00Z`**; the two agree that the
  index now reaches Monday evening, i.e. **past** the 2026-11-06 frontier (`2611.09894`) the previous
  run recorded. The index advanced, so the weekend zeros were not a cached-page artefact of today's
  query either.
- **Screening.** (a) all **1,008** titles of the three newly visible days in ascending ID order;
  (b) all **202** late-arrival and revision records, filtered and then fully listed; (c) **30 full
  abstracts** for records carrying a harness, agent-runtime, evaluation, permission, memory or
  robot-agent signal; (d) **8 full-text HTML renderings** (`arxiv.org/html/<id>v1`, **7** of which
  returned article bodies — `2611.12885` has no HTML at the time of the scan) read for artifact claims,
  scope statements, and the sentences that the abstracts omit; (e) resolution of **every** claimed
  artifact at the source.

## What the index did

| query (one parenthesised OR-query per day) | 11-03 | 11-04 | 11-05 | 11-06 | 11-07 | 11-08 | 11-09 | 11-10 |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| count at 2026-11-10 14:23 UTC | **593** | **497** | **546** | **499** | **321** | **264** | **423** | **0** |
| recorded by the previous run | 585 | 488 | 526 | 391 | 0 | 0 | 0 | — |
| **new to this run** | **8** | **9** | **20** | **108** | **321** | **264** | **423** | — |

- **1,153 new record-days** by count, of which **1,210** records are diffed (1,008 new postings plus
  **202** late arrivals and revisions). 2026-11-06's **+108** is one of the largest single-day
  late-arrival blocks this project has recorded — second only to the **+120** block on 2026-11-04 — and
  the block sits **above** the previously recorded frontier (`2611.09894`), continuing the pattern the
  2026-11-09 record found sitting *inside* a recorded range. The lesson is the same in both directions:
  only a full re-harvest plus a parsed set difference detects either shape.
- **Coverage of the three newly visible days.** 2026-11-07: `cs.LG` **79**, `cs.CV` **73**, `cs.AI`
  **54**, `cs.CL` **36**, `cs.RO` **31**, with **35** records carrying `cs.RO` as primary or cross-list.
  2026-11-08: `cs.LG` **75**, `cs.CV` **48**, `cs.RO` **30**, `cs.CL` **27**, `cs.AI` **25**, `cs.RO`-any
  **37**. 2026-11-09: `cs.LG` **108**, `cs.CV` **80**, `cs.AI` **61**, `cs.RO` **54**, `cs.CL` **44**,
  `cs.RO`-any **73**. Across all three days **145** records touch `cs.RO`.
- **No new posting is a revision.** All **1,008** new records have `<updated>` equal to `<published>`,
  so the "meaningful revisions" half of the requirement is answered entirely by the four already-
  screened days below.

## Revisions on the already-screened days (57)

- **57** records advanced `<updated>` — **10** on the 2026-11-03 harvest, **13** on 2026-11-04, **22** on
  2026-11-05, **12** on 2026-11-06. **Every one is a version bump** (`v1→v2`, one `v2→v3`) with the new
  `<updated>` landing on a later date, not a same-day whitespace artefact of the kind the 2026-11-09
  record had to discount.
- **Six are retitled, and four of those are substantive**: `2611.04714` (drone detection retitled to
  "Leakage-aware acoustic drone detection…"), `2611.05227` (retitled to "Regret Equals Integrated
  Covariance: Exact Identities for the Value of Information…"), `2611.08140` (anomaly detection renamed
  to "FATE: Fault-Aware Transfer Error…"), `2611.08671` (BPE bound retitled from "at least 0.361" to
  "0.391 of the Optimum…"). The other two are cosmetic: `2611.06918` (dash spacing) and `2611.07130`
  (subtitle appended). None of the six is a harness or robot paper requiring an entry edit.
- **One revision does require an entry edit, and it is a withdrawal.** `2611.09218` (**BOXBOX**) became
  **v2 on 2026-11-09T08:31:43Z** with the comment: *"We are withdrawing this submission temporarily to
  add a clarification regarding the use of Claude Code, in accordance with our company's compliance
  requirements."* The `abs` page now renders the **"(withdrawn)"** marker. BOXBOX is one of the thirteen
  entries the previous run added, and its **PerfRace** result is the batch's headline harness number, so
  the README entry now carries the withdrawal and the numbers are marked unverified pending re-release.
  This is the first time in this project that an entry was withdrawn within one day of being added.

## Inclusions (16)

Classification per the handoff: General Harness Methodology; Agentic Robot/VLA Harness; VLA; Robot
Foundation/World Model; Evaluation/Safety. Every number below is **author-reported**; no paper in this
window has an independent reproduction. Artifact status is stated literally.

### General harness methodology (5)

- **`2611.12300` — Reconfigurable Action Theories** (`cs.AI`). Formalises the thing BOXBOX and PatternOps
  approach empirically: a harness whose tools, providers, and policies change **during** execution.
  A theory *module* carries a vocabulary, causal laws, and an initializer; module load/unload are
  **actions governed by ordinary causal laws**, so reconfiguration stays inside the semantics instead of
  becoming an unmodelled side effect. Contributions: sufficient conditions for at most one next
  configuration, behaviour preservation on module addition, order-independence of loading, complexity of
  the reasoning tasks, and a translation of finite RAT systems into nonmonotonic causal theories and
  ASP with bounded executions preserved. Design choices are checked against executions of official
  **DeepSeek Harness, Pi, and OpenHands** implementations. **Artifact:** companion page at
  `staff.ustc.edu.cn/~jianmin/rat/` serving `rat-code.zip` (262,807 bytes, HTTP 200), a DeepSeek table-1
  archive, `rat-formal-plot-values.csv`, three Mac-mini addenda, an OpenHands report, a reasoning smoke
  test, `main.pdf` and `supplementary.pdf` — static downloads, **no versioned repository and no asserted
  licence**. Evidence: digital agents only.

- **`2611.12122` — System Optimizations for Cost-Efficient Agent Harnesses** (`cs.AI`). Takes the claim
  this list keeps citing — harness variance moves **cost**, not accuracy — and builds the mechanism:
  trace analysis of cost accumulation yields token and call reductions, and **Agency** is an interposition
  platform implementing them portably on **Claude Code** and **OpenAI Codex**. Positioned against prior
  spreads of up to **2.6×** cost at fixed model and task; the authors report up to **35%** cost reduction
  across real-world benchmarks at unchanged accuracy. **Artifact:** full system prompts and tool schemas
  for each harness are reproduced in the appendix; **no platform release** was located on the record or
  in the full text. Evidence: digital coding agents.

- **`2611.11499` — JoMoHa: Joint Model–Harness Optimization with RL** (`cs.LG`). Makes model–harness
  co-evolution a joint optimisation: a harness-generation policy and a task-execution policy are trained
  **together** rather than alternately, after the authors measure that naive alternation drifts into
  progressive model–harness incompatibility that degrades both execution and training. Reported:
  **70.3%** average and **97.0%** on ALFWorld, highest among evaluated methods on ALFWorld, WebShop,
  DBBench; the loop is described as recursive self-improvement from its own trajectories.
  **Artifact:** none located. Evidence: digital tool-use.

- **`2611.11342` — Agent Self-Improvement with Frozen Weights** (`cs.LG`). Prices failure traces against a
  matched search that never sees them: +**4.0** points from mistake-based notes over mistake-withheld
  notes on generated customer-service decisions, but the try-and-keep process alone reached about
  **60%** of the total gain and **full conversations showed no demonstrated improvement**. A strict
  screen admitted two lessons awaiting tests and one tool-use lesson with no demonstrated gain; a
  neutral note outscored the learned lessons (exploratory); a blind audit judged **146 of 240** tool-use
  test items defective. **Artifact verified open (2026-11-10):** `hopit-ai/harness-mistakes-artifacts`
  (589 KB; `data/`, `figures/`, `paper1/`, `reproduce.py`, `tests/`, `tools/`) under **PolyForm
  Noncommercial 1.0.0** — public but **not OSI-open**. Registered analyses; digital agents.

- **`2611.11344` — Spatial Harness** (`cs.LG`, NeurIPS 2026). Executable world state instead of imagined
  geometry: a Perceiver places objects and cameras in one frame from question constraints plus visual
  estimates, a **physics engine** instantiates the state, and a Reasoner answers by executing viewpoint
  changes and reading measurements and renders. Reported: MindCube **52.70% → 71.14%** (Qwen3.5-27B,
  **+7.52** over pySpatial) and **98.25%** vs **71.81%** model-internal given identical annotated poses;
  also SAT, MMSI-Bench, SpatialRGPT-Bench. **Artifact is a placeholder:** the paper states code is public
  at `tmlr-group/Spatial-Harness`; that repository contains **only `README.md`** ("Our paper and code will
  be released within a month"; 3 KB; verified 2026-11-10). Not open source.

### Agentic robot and VLA harnesses (5)

- **`2611.10748` — REINS** (`cs.RO`, NeurIPS 2026 Main Track). The harness moves to the millisecond side
  of control: LLM output is grounded to a learned variable-level dynamics model, filtered by calibrated
  predicates plus forward rollout, and consolidated into an indexed rule memory serving familiar scenes
  in **O(log n)** without re-invoking the model — a self-evolution loop with frozen weights. Reported:
  **98%+** constraint compliance, nearly **50%** collision reduction, **1.9–4.6 ms** latency at the
  tactical decision layer, LLM invocation rate declining monotonically as memory matures, over four
  simulated `highway-env` scenarios and the **highD**/**rounD** real-world driving datasets.
  **Artifact verified partially (2026-11-10):** Apache-2.0, 8.7 MB repository with `src/`, `config/`,
  `data/`, `docs/`, `requirements.txt`, `LICENSE`; its README states plainly that it **excludes** the
  closed-loop replay evaluation on highD/rounD and the baseline planners, and that the datasets are
  distributed by their own providers. Evidence: simulation plus dataset replay, not a physical robot.

- **`2611.12885` — RoboScript / World In-Context Learning** (`cs.CV` + `cs.RO`). A frozen model learns in
  a world instead of in its weights: one demonstration, rehearsal in simulation, feedback written into
  context, and the result frozen as a semantic **Task Memory** (task steps and object roles, not visual
  appearance) that transfers across scenes, with a data-isolation rule ensuring every deployment input
  exists on the real robot. Reported: **83.4%** mean success over all **50** RoboTwin 2.0 tasks across
  clean and randomized settings from **one demonstration per task and no gradient updates**, more than
  **20 points** above the strongest trained action policy, plus transfer to a physical dual-arm robot via
  cross-embodiment memory adaptation. **Artifact verified open (2026-11-10):** Apache-2.0
  `ArcheOne-AI/RoboScript` (93 MB; `packages/`, `recipes/`, `configs/`, `docs/`, `examples/`, `tests/`,
  `LICENSES/`, `NOTICE`, `pyproject.toml`) with the paper's arXiv ID in its README and a live project
  page; pushed the same day. Results author-reported.

- **`2611.11509` — SimplEvo: Review the Episodes, Refine the Lessons** (`cs.RO`). The **control experiment
  for the whole harness-engineering premise**, and the reason it is included: it asks whether additions
  around an MLLM exceed the model's native capability under direct control, and answers with the thinnest
  harness it can construct — a coordinate-free task memory written once from one demonstration plus a
  bounded working memory refined per training episode, **both fixed at test time**, no weights, no
  post-trained policies, no privileged information. Reported: EBench **46.7% → 81.7%** for GPT-6-Astra
  under direct control; **69.63%** SR and **75.32** score on RoboDojo; text memory matches frame-storing
  memories at fewer tokens; working memory written by one model is reused by weaker MLLMs unchanged.
  **Artifact: none located.** The paper carries **no availability or reproducibility statement**; the
  neighbouring `mmlab.hk/research/PhysicalRSI` page is cited as related work (PhysicalRSI 1.0), not as
  this paper's release, and is **not** counted as one here. Evidence: simulated embodied evaluation.

- **`2611.10510` — ReSkill** (`cs.RO`). Selective self-improvement localised to the failing stage: a
  coding agent builds an object-centric task graph against a skill library, uses node-level execution
  records to name the failure, revises stage organization, hand-offs, and recovery routes at the task
  level and strengthens repeatedly failing skills at the skill level, and promotes candidates only through
  full-task evaluation and paired comparison. The agent leaves at development time. Reported: ten
  RoboDojo tasks **30.0% → 86.2%**; **83.6%** for retained programs; **62 of 80** real-robot trials
  (**77.5%**) on eight tasks; median revision touches **three functions (~0.5% of a program)**; **39%**
  of later task-specific functions reuse existing implementations. **Artifact: not released.** The paper
  says the resulting code and skills "remain available for reuse **after this development expenditure**",
  and the data-availability statement defers episode-level results, revision snapshots, and figure
  scripts to **publication**. No repository located.

- **`2611.11941` — Flip Anything** (`cs.RO`, CoRL 2026 Late-Breaking). One contact-rich primitive as the
  unit of agentic self-learning: a coding agent develops and refines bimanual flipping strategies by
  iterative learning in physics simulation across object geometries, progressing from handover to
  toppling and pushing plus robust grasping, sensing, thin-object manipulation, and recovery, then
  integrates strategy selection with reactive control in one graph-based program that runs on physical
  robots. Reported: **24.7% → 95.0%**. **Artifact:** project page live (`berkeleyautomation.github.io/
  flipanything/`, real-world setup, sim-to-real analysis, videos) but **no code, model, or data link** on
  the page or the arXiv record as of 2026-11-10.

### Evaluation (4)

- **`2611.10063` — Done Is Not Correct** (`cs.SE`, late arrival on the 2026-11-06 harvest). Separates a
  completion claim from a correctness claim and measures the gap: 13 specification tasks mapping every
  requirement to a machine-checkable B-rep property under published tolerance bands, scored by an exact
  self-audited oracle; reports **silent-failure rate** and **completion-claim calibration**. Four models
  × 156 attempts: single-shot claimed completion on **98%** and were silently wrong on **14.1%**; a
  measure-then-claim loop cut silent failures to **3.2%** and lifted claim precision **64% → 96%**; a
  four-model ablation attributes it to the **claim contract**, not the feedback; a planner-coder-checker
  pipeline matched precision at lower coverage with no success gain; single-shot ECE **0.32**. Hardening
  their own oracle flipped **18 of 519** regraded verdicts pass→fail and none in reverse; a blind expert
  audit overturned **3 of 18** silent failures. **Artifact verified open (2026-11-10):** MIT-licensed
  `rodriguescarson/cad-silent-failure-bench` (2.7 MB) with benchmark, harness, per-attempt records, and a
  3D viewer; Zenodo **10.5281/zenodo.22312792** resolves. Digital CAD agents.

- **`2611.11123` — HARP-Bench** (`cs.RO`, CoRL 2026). The human as a **second active agent** rather than
  scene variation: four conditions (**Cueing, Contribution, Interruption, Co-presence**) instantiated as
  paired human-present variants of everyday tasks, plus a demonstration-generation pipeline for dynamic
  human-present settings. Reported: human-present variants challenge VLA, imitation, and heuristic
  policies; **higher task success does not consistently improve safety**; manipulation and recovery
  dominate remaining failures; training on the demonstrations helps in simulation with only partial
  real-world transfer (improvement under contribution, not under interruption). **Artifact verified open
  (2026-11-10):** `UMass-Embodied-AGI/harp` (13.5 MB; `envs/`, `config/`, `eval_sets/`, `baseline/`,
  `docs/`, `paper/`, `patches/`, `tests/`), Genesis-based, **49** canonical tasks (**37** evaluated in
  the paper) plus 32 aliases and six embodiments; site `harp-bench.vercel.app` live; **no licence file**,
  so public but not formally open source.

- **`2611.11789` — HolidayFactory 1.0 / 24/7-BENCH** (`cs.RO`). Industrial work as **persistent jobs**
  instead of reset-terminated episodes: HOLIDAY-SIM extends SDF hydroelastic contact with extra
  constitutive laws and a semi-implicit primal-dual solver, couples stateful actuator dynamics to
  articulated motion, derives tactile and joint observations from state; agentic world construction
  generates assets and scene variants under geometric and interaction checks; **24/7-BENCH** scores seven
  industrial tasks without resetting the scene — completed, failed, and unfinished jobs, throughput,
  latency, motor energy. Reported: controlled studies of the contact, actuator and tactile models,
  verification-guided world construction, and the limits of model critics at guaranteeing that a
  generated evaluation captures the task. **Scope stated by the authors and quoted rather than smoothed:
  job-level runs use scripted trajectories (e.g. code-as-policy) and they "have not yet compared policies
  over persistent runs."** No artifact located; evidence is simulation.

- **`2611.12386` — Say Shibboleth** (`cs.CR`; placed under Safety in the README). Harness as an
  observable signature: **shibboleth traps** condition resource access on informative interaction, and
  indirect probes recover harness-specific execution behaviour — secret masking in tool output, command
  time limits, tool-call scheduling — only from effects visible in later requests. Reported: **90%**
  average top-1 over **15** candidate models; **92%** harness identification across **four** harnesses;
  concealment instructions leave behavioural evidence intact while declarations become uninformative.
  **Artifact:** none located. Telemetry-side security work; no robot experiment.

### Safety (2 more)

- **`2611.11971` — Harness Matters (WebTrap Farm)** (`cs.CR`). The harness as the **security boundary**:
  a four-stage decomposition (goal interpretation, webpage perception, reasoning and planning, action
  execution) over three attack categories (malicious user prompts, malicious prompt injection, deceptive
  web design), with paired **usability calibration** tasks so incapability is not scored as defense.
  Reported: existing harnesses remain vulnerable; **some unsafe behaviour needs no attack content at
  all** (harness-intrinsic failure); harness and backend model each significantly shape outcomes; the
  stage at which defensive behaviour emerges is localisable. **Artifact:** none located. Digital web
  agents.

- **`2611.11438` — ArmedAgent: When Coding Agents Get Arms** (`cs.RO`, `cs.CR`, `cs.MM`; technical report,
  work in progress). The digital–physical boundary made a benchmark: frontier coding agents control a
  real robot arm zero-shot, so untrusted digital content becomes an actuation path. **36 tasks in 5
  scenarios** covering robustness, malicious use, and indirect prompt injection, in simulation with
  validation on a physical arm, including harmful behaviours earlier VLA/WAM paradigms could not execute;
  the authors report substantial risks across frontier models including **GPT-6-Astra** under widely used
  agent frameworks, with execution-level examples (an injected email inducing the agent to cut a joint's
  configured rotation limit from 320° to 40°). **Artifact claimed, not verified:** the paper states that a
  simulator mirroring the real setup "is released", but no repository, archive, or download link appears
  on the arXiv record, in the full text, or on the project page
  (`ymsun2020.github.io/CodingAgentEmbodiedSafety/`, which carries real-robot demonstrations but no code
  link). Recorded as an unlocatable claim, not as an artifact.

## Excluded and watch-listed

Screened in full, deliberately **not** added. Reasons are recorded so that a later run can distinguish
rejection from oversight.

- `2611.11994` **EmoHarness** — a genuine meta-harness evolution loop (Builder edits prompts, settings and
  verification code offline behind static checks and validation gates; MIT code and project page live).
  Excluded on contribution, not quality: the evaluation is emotion recognition, and the paper's own
  numbers say the evolved edits add at most **+0.95** and **no increment survives Holm correction**,
  with the gain coming from supplying transcript and prosody evidence. **Watch** for a general or robotic
  instantiation of Meta-Harness Evolving.
- `2611.10463` **LongCapHarness** — capability-guided auto-harnessing without data synthesis or
  post-training; no artifact, no robot evaluation, and the "harness instead of post-training" line is
  already represented.
- `2611.11558` **InfraRSI** — a real agentic harness for distributed-training optimisation (locked editable
  scope, calibrated cluster noise, cross-hardware experience bank, failed-trial cost accounting) with a
  live repository. Deferred: the object is training infrastructure, not a robot or general agent-runtime
  contract; a later run may add it under General Harness Methodology after verifying the licence state
  (none asserted).
- `2611.12109` **SkillDuet**, `2611.12041` **SMO**, `2611.10022` **SkillCache** — skill co-evolution,
  compilation, and reuse. No artifacts located; the skill-library line is already represented and the
  three would not change the map.
- `2611.10629` **DFT-Agent Bench** — scaffold effects and execution provenance measured under one harness;
  materials-simulation domain, no artifact.
- `2611.11216` **Replay Fidelity in LLM Agent Evaluation** — strong audit (71 unsuccessful AppWorld
  executions, 284 replay attempts, 42 vs 68 admitted under different reconstruction settings, four
  materialised-state discrepancies) with no artifact and no runtime contract. Kept as evidence.
- `2611.11581` **Same Error, Different Recovery** — error-delivery study with an opt-in recovery layer in
  a deterministic inference engine; no public artifact located.
- `2611.11840` **Correct Answers from Insufficient Histories** — separates reference agreement from
  certified evidence sufficiency; no artifact.
- `2611.11650` **FlowEval** — training-free adaptation diagnostic across Pi-0.5, MolmoAct2, GR00T,
  DreamZero. A metric, not a harness; no artifact located.
- `2611.11842` **CREG Benchmark** — evidence-to-belief trajectory evaluation for connected robotics with a
  live project page, but the page's code link (`RanulfoBezerra/Evidence-Graph-Toolkit`) returns **404**,
  so its "all scenarios, references, prompts, metrics, and analysis code are released" claim is
  **unverified**. **Watch** for the repository to appear.
- `2611.12804` **Stochastic challenges** (CoRL 2026) — a paradigm for real-world cross-lab comparability;
  no artifact located, and the contribution is methodological rather than an interface.
- `2611.12870` **SWAM** — states it releases curated data, training code, and weights; a world-action
  model paper whose artifact was **not** verified this run. **Watch.**
- `2611.11780` **Dithered Low-Bit World Models** — shows planner drift, not rounding-error magnitude,
  decides whether quantised world-model control collapses (19 affected Meta-World cells, median 95%
  damage removed by shared dither). Action-facing world-model robustness, no harness or release.
- `2611.10769` **Look Ahead** — selective test-time search over ACT chunks; simulation-only, and the
  authors state that language-conditioned VLAs and physical robots remain future work.
- `2611.10400` **MIND** — failure detection and explanation from sparse internal policy features; no
  artifact located.
- `2611.10498` **EmbodiedIntentBench** — mission-intent contracts for mobile robotic adaptation; author
  version posted for review, no artifact.
- `2611.10261` **Is Recursive Self-Improvement Safe?** — position paper (IEEE Intelligent Systems); no
  system, no artifact.
- `2611.12377` **Detecting Shadow AI**, `2611.11487` **THEATRON**, `2611.10996` **Toward a theory of
  Evaluability** — enterprise-agent governance, persona persistence, and measurability theory
  respectively; out of the harness/robot scope this list maintains.
- **Late arrivals screened and excluded on topic**: `2611.10170` (proof-set loss in long-term
  conversational memory), `2611.10240` (PACE KV-cache for long-horizon agents), `2611.10305`
  (plug-and-play verifier for process reward), `2611.10099` (Enmo Safety external oversight),
  `2611.10148`/`2611.10061` (cross-embodiment transfer), `2611.10235` (Agibot swerve-drive hardware),
  `2611.10298` (multi-robot mobility information capacity), `2611.10096` (mixed-precision KV cache for
  multi-agent systems). Each is a component, a hardware platform, or a digital-agent optimisation without
  a reusable harness contract.
- **Revisions reviewed and not acted on**: the six retitled records listed above, and the 51 records whose
  version advanced without a title change — checked for artifact or scope changes relevant to existing
  entries; BOXBOX was the only one requiring an edit.

## Files modified

- `README.md` — badge to **2026-11-10**; Contents blurb rewritten (including the withdrawn
  "no weekend submissions" claim); `## Current Landscape` verification date and **five** new bullets;
  **16** new entries across five subsections; **one** existing entry amended (BOXBOX withdrawal).
- `sources/daily-arxiv-2026-11-10.md` — this record (new).
- Untouched by design: `docs/reference-architecture.md` (user modification), `docs/ring-harness.png`
  (untracked user file), `handoff.md` (untracked by design), `.scratch/` (working data, never staged).

## Validation performed

- **Markdown structure.** Heading inventory before and after is identical: **22** `###` subsections and
  the same `##` sections; every new entry is a single top-level list item under the correct subsection;
  blank-line separation before and after each inserted block checked by line inspection (one edit
  initially consumed the `### End-to-End Robot Learning` heading and was repaired before commit).
- **`git diff --check`** reports no whitespace errors.
- **Link resolution.** All **18** non-arXiv URLs introduced by this run returned **HTTP 200**
  (`doi.org/10.5281/zenodo.22312792` included), as did all **16** new `arxiv.org/abs/` links. Artifact
  state was then checked *past* the status code, because 200 is not release: repository contents were
  listed through the GitHub API and READMEs fetched, which is how the Spatial Harness placeholder, the
  CREG 404, the REINS partial scope, and the HARP licence gap were found.
- **Duplicate check.** All 16 IDs were grepped against `README.md`, `docs/*.md`, `sources/*.md`, and
  `CONTRIBUTING.md` **before** insertion; none was present. After the edits each appears once in its
  entry and, where also cited in a landscape bullet, in that bullet only — no cross-section duplication.
- **Dates and categories.** Every included entry names its arXiv ID and its `<published>` date falls on
  2026-11-05 (late arrival), 2026-11-06 (late arrival), 2026-11-07, 2026-11-08, or 2026-11-09 — never
  2026-11-10, for which no record exists. Category placement follows the contribution rather than the
  arXiv primary: `2611.12300`, `2611.12122`, `2611.11499`, `2611.11342`, `2611.11344` under *General
  Harness Design and Self-Improvement*; `2611.10748`, `2611.12885`, `2611.11509`, `2611.10510`,
  `2611.11941` under *Agentic Robot and VLA Harnesses*; `2611.10063` under *Unified Evaluation*;
  `2611.11123`, `2611.11789` under *Benchmarks and Evaluation → Manipulation and VLA*; `2611.11971`,
  `2611.12386`, `2611.11438` under *Safety*.
- **Cross-file consistency.** The README badge, the `## Current Landscape` `Last verified:` line, and the
  Contents blurb all read **2026-11-10**, matching this record's filename. `docs/landscape.md` keeps its
  earlier verification date as a historical note rather than a maintenance surface.
- **Artifact-status discipline.** Each entry states exactly one literal status: **verified open with a
  licence** (Apache-2.0 REINS — partial scope; Apache-2.0 RoboScript; MIT cad-silent-failure-bench plus a
  resolving Zenodo DOI), **verified open without licence** (HARP-Bench), **verified open under a
  non-OSI licence** (PolyForm Noncommercial for the frozen-weights artifact), **static downloads with no
  licence** (RAT), **placeholder** (Spatial Harness README-only), **claimed but unlocatable**
  (ArmedAgent's simulator), **deferred to publication** (ReSkill), and **nothing located** (Agency, JoMoHa,
  SimplEvo, Flip Anything, Harness Matters, Say Shibboleth, HolidayFactory). No entry describes a promise,
  a placeholder, or a private link as open.
- **Author-versus-independent separation.** All numbers are marked author-reported. Evidence scope is
  stated per entry: digital-agent only for `2611.12300`, `2611.12122`, `2611.11499`, `2611.11342`,
  `2611.10063`, `2611.11971`, `2611.12386`; VLM/benchmark only for `2611.11344`; simulation plus dataset
  replay for `2611.10748`; simulation with real dual-arm transfer for `2611.12885`; simulation for
  `2611.11509`, `2611.11789`, `2611.11941`; simulation plus real robots for `2611.10510`; simulation plus
  a physical arm for `2611.11438`; Genesis simulation with a limited real-world study for `2611.11123`.

## Operational notes and blockers

- **A day can be *absent from the index* and still not be empty, and the previous record generalised the
  wrong way.** Two consecutive runs saw zeros on weekend dates and inferred a property of arXiv's
  submission calendar. Today the same ranges return **321** and **264** records with ordinary spread
  across the day. The rule carried forward is narrower and better: **a zero is a statement about the
  index at query time, not about submissions**, so a zero-bearing day must be re-queried until its batch
  is seen, and must not be explained away in a record.
- **The API's failure mode is a valid feed, not an HTTP error.** Three requests returned a normal Atom
  feed whose only entry was `https://arxiv.org/api/errors` with `totalResults` 1 and the summary "The
  server encountered an internal error". A count check that reads `totalResults` and ignores the entry
  id will record a false zero with a 200 status. Retries after 3–6 s all succeeded.
- **Comparing two harvests requires normalising the id key.** The previous run's parsed records carry the
  versioned id in the key; this run's parser keys on the bare numeric id. The first diff reported "585
  removed / 593 added" for a day that grew by 8. The corrected diff reports **0 removed** on all four
  days, which is the check that the key normalisation is right.
- **A withdrawal is an entry-edit trigger, and it can arrive the day after an inclusion.** BOXBOX v2
  (2026-11-09) is the first case in this project of a listed paper withdrawn within 24 h. The revision
  sweep that finds it is cheap — field-by-field over parsed XML for four days — and is now the reason
  "re-harvest the previous days" must stay in the standing instruction rather than being treated as
  belt-and-braces.
- **Related work is not a release.** SimplEvo cites the live PhysicalRSI 1.0 project page and its
  Apache-2.0 repository, which pattern-matches to "this paper has code" and does not. The distinguishing
  evidence is a citation reference, not an availability statement, and the paper contains no availability
  section at all. The same discipline caught the CREG page's 404 code link and Spatial Harness's
  README-only repository.
- **`/tmp` remains non-durable across tool calls.** `.scratch/arxiv-2026-11-10/` holds all 22 API
  responses, the seven day harvests, the parse and diff scripts, the eight full-text renderings, and the
  GitHub/project-page fetches. It is **not staged and not committed**.
- **OpenSSH workaround still required.** `git ls-remote`, `git fetch`, and `git push` fail with *"Bad
  owner or permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf"*;
  `GIT_SSH_COMMAND='ssh -F /dev/null'` restores access with the user's key and `known_hosts` unaffected.
  Before this run, `origin/main` and local `HEAD` were both `a6a54f8`.
- **Nothing is owed as a withheld day apart from 2026-11-10**, which is owed by construction: it returns
  **0** because Tuesday's batch announces Wednesday 01:00 UTC. **The next run must query 2026-11-10 and
  2026-11-11 and must re-harvest 2026-11-09, 2026-11-08 and 2026-11-07 fully**, since all three were seen
  for the first time here and every first-seen day in this project's history has grown afterwards.
