# Daily arXiv scan — 2026-11-07 (the owed 2026-11-05 batch arrives, 2026-11-04 grows by 88 records, no Friday batch exists yet, and fourteen inclusions plus one important revision)

## Scope

- **Interval owed.** The previous successful run started **2026-11-05 09:02 UTC** and committed at
  **`2026-11-05T10:15:03+01:00`** (09:15 UTC) on `main`, producing `1417a23` and record
  `sources/daily-arxiv-2026-11-05.md`. That record screened the owed 2026-11-04 batch and 120 late arrivals
  from 2026-10-31 → 2026-11-03, and closed with **"Still owed: 2026-11-05. The next run must query it by
  `submittedDate` again, and must recount 2026-11-04 and 2026-11-03 before trusting either count."** This run
  started **2026-11-07 15:37 UTC** on `main` at `1417a23`, so the interval it owes is **submission dates
  2026-11-05 → 2026-11-07 inclusive**, with 2026-11-05 the explicit debt and 2026-11-06/07 unknown.
- **2026-11-05 arrived, and it is a complete batch.** It returned **0** at the previous run's 09:02 UTC probe
  because the batch had not closed; at 15:37 UTC today it returns **433**, `<published>` spanning
  **`2026-11-05T00:06:00Z`–`2026-11-05T18:59:42Z`**. Every one of those is new to this repository.
- **2026-11-06 and 2026-11-07 return 0, and the reason is visible in the index rather than assumed.** The newest
  record in the whole harvest is **`2611.08487`** at **`2026-11-05T18:59:42Z`**; a descending `cs.AI` frontier
  probe returns **`2611.08483`** at **`2026-11-05T18:59:29Z`** and the descending `cs.RO` probe returns
  **`2611.08462`** at **`2026-11-05T18:53:16Z`**, so the newest record in the API is still a Thursday submission. 2026-11-05 is a **Thursday**; arXiv's announcement schedule carries Friday's
  submissions into the following Sunday announcement, so on a Saturday no Friday or Saturday batch exists. These
  two days are **unharvestable, not screened**, and the next run must query both again. (The previous record's
  note that "a day is only harvestable after its batch closes, roughly a day later" is still the operative rule;
  here the lag runs over a weekend.)
- **The previous run's warning was correct again, for the fourth day running, and the late block was large.**
  Recounting every day in the previous record's window found **91 records that appeared after that record was
  written**:

  | query (one parenthesised OR-query over cs.RO/cs.AI/cs.CL/cs.CV/cs.LG per day) | 2026-11-01 | 2026-11-02 | 2026-11-03 | 2026-11-04 | 2026-11-05 | 2026-11-06 | 2026-11-07 |
  | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
  | count at 2026-11-07 15:37 UTC | 364 | 642 | **585** | **487** | **433** | 0 | 0 |
  | recorded by the previous run | 364 | 642 | 582 | 399 | 0 | — | — |
  | **late arrivals screened by this run** | **0** | **0** | **3** | **88** | **433** | — | — |

  **524 records** are new to this repository. Zero records from the previous harvest disappeared.
- **The late arrivals are above the declared frontier again.** The three records added to 2026-11-03 occupy
  **`2611.06950`–`2611.06956`** and the 88 added to 2026-11-04 occupy **`2611.06962`–`2611.07247`**, i.e. the
  contiguous block immediately **above** `2611.05377`, the ID the previous record declared the 2026-11-03
  frontier. 2026-11-01 and 2026-11-02 held still at **364** and **642** and should not be re-harvested.
- **Frontier movement.** The visible frontier moved from **`2611.05377`** (`2026-11-03T18:59:56Z`) to
  **`2611.08487`**, the newest of the 524 new records — **3,110 IDs and, in effect, two days of submissions**
  (November 4 and November 5), with November 6 absent as above.
- **Revisions: 24 of the 773 records the two harvests share carry a new version; none of the 524 new records is
  a revision** — every new record is `v1` with `<updated>` equal to `<published>`. All 24 bumps fall inside this
  run's window (`<updated>` ≥ `2026-11-04T23:12Z`). **Two of them are already curated in this repository**
  (`2611.05163`, `2611.06817`) and are handled under **revision handling**; the other 22 are model, application
  or benchmark papers whose revision added no artifact and no harness-relevant result.
- **Coverage.** Every record carries at least one of the five target categories. Primary categories over the 524
  new records: `cs.LG` **145**, `cs.CV` **107**, `cs.AI` **75**, `cs.RO` **68**, `cs.CL` **62**, `stat.ML` 10,
  `cs.SE` 9, `cs.CR` 6, `math.OC` 5, `cs.CY` 5, `cs.DC` 4, `cs.GT` 3, `eess.SY` 2, `physics.chem-ph` 2,
  `cs.GR` 2, `cs.NI` 2, `stat.ME` 2, `cs.IR`/`cs.LO`/`cs.DB` 1 each. `cs.RO` appears as primary or cross-list on
  **80**. Per day: 2026-11-03 late block (3) `cs.LG` 2, `cs.RO` 1; 2026-11-04 late block (88) and 2026-11-05
  (433) are dominated by `cs.LG`/`cs.CV` with `cs.RO` **68** primaries across both.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`. **12 export-API requests**: **seven**
  per-day count queries (2026-11-01 → 2026-11-07), **two** descending frontier probes (`cs.RO`, `cs.AI`), and
  **three** full-day harvests at `max_results=2000&sortBy=submittedDate&sortOrder=ascending` returning
  **585 / 487 / 433** entries (1.43 MB / 1.21 MB / 1.06 MB). Spaced 3–5 s; **zero HTTP 429**.
- **No re-download of the previous run's harvest was needed.** The untracked `.scratch/arxiv-2026-11-05/`
  directory is still on disk, so `allrecs.json` and `per_day.json` from that run were diffed directly against
  today's parse: the 524-record delta is a **set difference over parsed XML**, not a comparison of prose. This
  is the only reliable way to produce the late-arrival table and it again took seconds.
- **`/tmp` is still not durable between tool calls**, reproducing every previous run's finding. All XML, HTML,
  JSON and image captures are in the untracked `.scratch/arxiv-2026-11-07/` directory, which is **not staged and
  not committed**.
- **Query syntax still matters.** The per-day counts use
  `(cat:cs.RO OR cat:cs.AI OR cat:cs.CL OR cat:cs.CV OR cat:cs.LG) AND submittedDate:[...]`; the unparenthesised
  form drops the date filter and returns a constant site-wide count.
- **Screening.** (a) All **524** titles were read in ascending ID order. (b) Candidates were split by anchor:
  the **80** cs.RO-touching records read in full, plus harness-anchored titles in the non-`cs.RO` remainder
  (harness|agent|runtime|recover|memory|skill|tool|evaluat|benchmark|safety|verif|monitor|permission|sandbox|
  replay|audit|guardrail|self-improv|workflow|serving|latency|deploy|contract|world model|embodi|manipulat);
  the remainder was read to confirm it is off-scope. (c) **32 abstracts** were read in full. (d) **11 full-text
  HTML renderings** (`arxiv.org/html/<id>v1`) were fetched for artifact claims, ablation detail and
  citation-versus-release disambiguation, plus **4 arXiv abstract pages** for the two curated records' version
  history. (e) Every claimed artifact was resolved at the source: **11 GitHub repository metadata reads**,
  **5 repository trees**, **2 branch lists**, **3 GitHub searches**, **13 Hugging Face API and raw-card reads** (two datasets,
  two models, two model trees, one dataset tree, two collections, two dataset cards), **3
  `anonymous.4open.science` status checks**, **6 project pages** (plus one JavaScript bundle and one site asset
  inspected for outbound links) and **19 direct HTTP status checks**.
- **Model papers remain excluded by rule**, even when they are the batch's headline results, unless the
  contribution is an external runtime, interface, or evaluation/optimization target. The rejections below name
  the ones that came closest.

## Included (14) and one folded-in revision

### General Harness Methodology and Verification (3)

- **`2611.07372` — EnvOpt: Automated Optimization for Agent Environments** (cs.AI). Adds the **third axis** to
  the model/harness pair: an agent's cost and success depend on its model, its harness **and its environment**,
  and a *stationary environment* shared by many tasks (a Slack workspace, a document or ticket collection) is
  itself optimizable. EnvOpt optimizes that environment end-to-end for a given model and harness from **real or
  synthetic tasks with no graders and no reference answers**. On AutomationBench it cuts Claude Haiku 4.5's
  cost by **72.7%** and raises pass rate **14.34%**; the environment transfers to Opus 5 (**68.1%** lower cost,
  **1.34%** higher pass rate); stacked on Meta-Harness's optimized harness for Haiku 4.5 it removes a further
  **83.4%** of cost while adding **11.17%** accuracy over Meta-Harness alone; across five benchmarks cost falls
  while pass rate is improved or held. The paper also reports which environment-rewriting strategies survived
  the search and which were rejected. Digital-agent evidence; author-reported; **no artifact claimed** — the
  full text carries no code, data or model link. Placed in *General Harness Design and Self-Improvement*.
- **`2611.08318` — Learning the Task or Learning the Verifier? Rethinking Criteria Exposure in Embodied
  Self-Improvement** (cs.AI, cs.RO). Names and measures **Criteria Exposure (CE)** — showing the benchmark's
  success criteria to a self-improving embodied agent — and separates the task from the verifier with a
  controlled design: one actor, one learner, **25** tasks split **in advance** into **16 criteria-limited** and
  **9 faithful**, five rounds on five development layouts, held-out testing on 20 layouts, and **both** criteria
  scoring the same episodes. Success alone lifts **24.6% → 47.8%**; reading the criteria adds **14.2 points**
  (**62.0%**), and the whole extra gain sits in the criteria-limited group (**8.4% → 27.8% → 51.6%**) while
  faithful tasks are level (**53.3% / 83.3% / 80.6%**). Changing the criteria slightly without changing the
  instruction removes the advantage; CE-trained libraries transfer poorly across tasks and benchmarks. The
  constructive half is **instruction-faithful criteria**, validated by re-judging identical episodes under both
  criteria. Simulation-only (RoboDojo, LIBERO-PRO, RoboCasa365). **Artifact status literal (verified this run):
  project page live; its Code button resolves to `DAGroup-PKU/PyRUA-Lean` (Apache-2.0), but the repository tree
  (244 files, four branches) contains no RSIJudge evaluator or criteria code.**
- **`2611.07483` — Signed but Not Safe: Requirement Integrity in Multi-Agent LLM Handoffs** (cs.CR, cs.AI,
  cs.MA). Treats a handoff as a requirement-transmission channel: the orchestrator extracts each requirement as
  a verbatim quote plus a checkable predicate, signs it with HMAC-SHA256, and deterministic code verifies the
  merger. Preregistered; Qwen3-8B; **1,440** trajectories; five threats. Signed manifests cut unsafe commits
  under attack **69.4% → 9.0%** (**60.4 pp**, 95% CI [51.4, 69.4]) with no net loss of conformant commits; an
  unsigned manifest committed violations in **79%** of mutated runs, the signed one in **none**; the
  deterministic verifier caught **78.7%** of violating decisions against **59.0%** for an LLM checker, with
  **7%** versus **50%** false alarms. **The residual is extraction**: one injected line raised signed-manifest
  violations **14.6% → 37.5%** (Qwen3-8B), **11.1% → 63.9%** (gpt-oss-20b), **8.3% → 75.0%** (GPT-5.6-luna) and
  the signature then authenticated the corrupted manifest. Digital-agent evidence; author-reported; **code,
  task banks, freeze manifests and run logs are "available from the authors on request"**, which is not open
  source.

### Evaluation, Benchmark Maintenance and Measurement (4)

- **`2611.08417` — Benchmarks as Software: Terminal-Bench and the Case for Continuous Benchmarks** (cs.AI).
  Names **benchmark maintenance debt** and ships the maintenance process: community reports, task artifacts,
  agent traces, automated oracle checks and human-gated repairs, applied to an already heavily reviewed
  Terminal-Bench 2.0. The audit finds issues in **roughly 30%** of the benchmark and releases the repairs as
  **Terminal-Bench 2.1**; accuracy rises by **more than 10 percentage points**, leaderboard positions move by up
  to **three ranks**, and the ranking stays highly correlated (**Spearman ρ = 0.958**). The harness-relevant
  result is that **resource budgets are part of the measurement specification**: extending the time limit closes
  a **20 pp harness gap on GPT 5.4** and shrinks a **30 pp gap on GPT 5.4 mini to 5 pp**. **Artifact verified
  open (2026-11-07): `harbor-framework/terminal-bench-2-1`, Apache-2.0**, 47 MB, 136 stars, pushed 2026-11-05;
  its README states **26 tasks were modified**, against the paper's "roughly 30%" — the released count is the
  literal one.
- **`2611.07042` — Follow the Policy, Fail the Benchmark: Auditing and Revising Policy-Based Decision
  Benchmarks** (cs.CL, cs.SE). Audits **nine** policy-based decision benchmarks by source tracing, program
  execution and controlled checks. Rescoring unchanged responses isolates the scoring effect: on the **96
  LegalBench SARA numeric cases** a corrected scorer raises acceptance by **>80 percentage points** for each of
  three models; in **RuleArena Tax** restoring a required zero floor changes **27 of 300** references. **BAR**
  ships the audit evidence and revised data as versioned components. **Artifact verified open (2026-11-07): MIT
  `SugarRuy/BAR-Policy-Decision`** (456 KB, created 2026-11-01, pushed 2026-11-06; 116 files with
  per-benchmark `component.json`, evaluators, patches and upstream licences). Digital-agent/non-robot evidence.
- **`2611.07441` — Closed-Loop Physics-Based Manipulation Evaluation at a Commodity Price: What 4.4 L4 GPU Hours
  at $3.50 Buys You** (cs.RO; IROS 2026 workshop). A reproducible 200-rollout closed-loop physics protocol on
  serverless commodity GPUs at about **$3.50 and 38 minutes per replication** (**4.4 L4-GPU-hours** over eight
  shards), used to report a **partially controlled negative result**: three chunked checkpoints produce **zero
  genuine successes across 780 rollouts**, three single-step checkpoints reach **2.6%–11.8%**, and a
  per-step-replanning control localises the failure to chunk prediction in the **training formulation** rather
  than to open-loop execution. **Artifact status literal: `rohungry/evaluation_harness_wrapper` is public**
  (132 KB, created 2026-08-10, pushed 2026-09-27) and documents the environment and reproduction steps, but
  declares **no licence**. Author-reported; the authors state the benchmark software and demonstration data are
  the original providers' (TRI) with no artifact withheld for licensing.
- **`2611.07964` — Time-Indexed Skill Success Models for Latency-Aware Skill Selection and Timing in Dynamic
  Manipulation** (cs.RO; RA-L submission). Makes **launch time** a predicted property: success is modelled as a
  function of the delay at which a skill starts, so a receding-horizon planner picks skill **and** launch time
  under fixed or random latency. On a MuJoCo UR5e puck task the planner beats a static competence model by
  **2.8–6.2 points** at **100–300 ms**, with all **80** paired 95% intervals against four baselines excluding
  zero. The measurement result is the transferable one: the time-indexed model is calibrated (**ECE 0.009**
  versus **0.239**) while **pausing the world during latency overstates success by up to 23 points**. On a real
  UR5e capturing a rolling ball after **0.2 s** of injected latency, a static simulator-matched model predicted
  **99%** on a pre-specified 35-launch subset it rated ≥0.8; **63%** succeeded (95% interval 46–77%) and the
  time-indexed model predicted **56%**. **No artifact on the arXiv record.**

### Agentic Robot and VLA Harnesses (2)

- **`2611.08039` — HITCH: Teaching a Vision-Language Model to Operate a Humanoid through a Whole-Body Harness**
  (cs.RO). The harness is the paper: perception returns the **3D position of any point the VLM selects in its
  image**, and execution turns commands for pelvis, torso, hands, walking and gaze into whole-body motion via a
  learned tracker. **Both** interface accuracy components raise task success, which separates perception-interface
  quality from tracker quality. Without task-specific trajectories the fine-tuned model completes **51%** of
  SIMPLE and **42.5%** of HumanoidArena episodes; five trajectories per SIMPLE task lift SIMPLE to **95%**; on a
  real **Unitree G1** it carries a box and walks to a goal with no real training data. Simulation plus one real
  robot; author-reported. **Artifact status literal: project page live (a JavaScript application whose only
  outbound links are the paper and author pages); no code, weights or data.** The single repository cited in the
  paper (`EGalahad/sim2real`) is a third-party sim-to-real tracking adapter used for a baseline.
- **`2611.08462` — OopsieRecover: Learning Transferable VLA Corrections from Semantic Interventions** (cs.RO).
  Attacks the cost of teaching recovery: abduction over a handful of corrective demonstrations infers the intent
  behind them and encodes it as **dense spatial representations and conditional costs** that mark undesirable
  behaviour and steer the policy, instead of binding a corrective action to one observation. Six tasks plus
  object-pose, scene and related-task variations; the authors report that a handful of corrections generalises
  across novel situations **without new corrective data per variation**. Simulation; author-reported.
  **Artifact status literal: project page with videos and a PDF only; no code, weights or data.**

### Benchmarks (1)

- **`2611.08209` — RoboARCade: Benchmarking Agentic Robot Control** (cs.RO). Defines **agentic robot control
  (ARC)** and benchmarks it: **22** long-horizon tasks over **three embodiments** on one MuJoCo backend, each
  task a self-contained data pack, with **interchangeable interfaces** so tasks, agents and interfaces are added
  independently. GPT-6 Astra **49.8%** versus Claude Fable 5.1 **43.6%**; a richer visual interface buys
  **1.5–6.8 points at 1.7–2.3× cost**; agents **wrongly claim success on 24%–39%** of attempted subtasks; one
  in-context demonstration lifts Astra **53.5% → 73.0%**; a self-written, self-revised prompt reaches **72.0%**.
  Failures are mostly poor strategy or imprecise execution, rarely scene misunderstanding. **Artifact status
  literal:** leaderboard site live and prices every run (**$13.89–$60.00 per trial**); a public repository
  **`Satvik1701/RoboARCade`** (16 MB, 17 stars, pushed 2026-11-03, **no licence**) ships setup, `run_eval`, the
  VIA interface, a dashboard and a Hugging Face asset dataset — but the paper still says "we **will** release
  the benchmark code", the site does not link that repository, and the absent licence makes it source-available
  rather than open.

### Runtime and Safety (2)

- **`2611.08390` — SCALE: Real-Time Latent Communication for Cyber-Physical Agents** (cs.RO, cs.MA). Moves latent
  communication to physical agents where the constraint is **bandwidth and heterogeneous deadlines**: tokens are
  matched across consecutive latent messages so reusable information is not re-sent, semantic gain and
  communication cost are profiled, and a **real-time scheduler** decides which messages go and how many latent
  tokens each carries. Evaluated on three cyber-physical benchmarks (autonomous driving, cooperative perception
  via OPV2V, LIBERO manipulation) plus a **physical spider-robot case study** under constrained, time-varying
  links with background traffic; the authors report better performance and on-time delivery than
  full-latent-transmission and text baselines. **Artifact status literal: not released** — the only code link,
  `anonymous.4open.science/r/SCALE-4E80/`, returns **HTTP 401**.
- **`2611.07712` — AffectiveVLA: Safe and Intent-Aware VLA Control for Human-Humanoid Social Interaction**
  (cs.RO). An **intent gate** in front of a VLA for physical social touch: an RGB model reads intent from two
  onboard cameras and writes it as a language prompt, **low-confidence predictions suppress motion execution**,
  interaction-specific compliance profiles adjust arm impedance during contact, and an independent
  voice-triggered mechanism starts a **latched** safe-stop. About **1,300** VR-teleoperated episodes fine-tune
  four VLAs (**GR00T, RLDX-1, Being-H0.5, UnifoLM-VLA-0**) on a Unitree G1 with dexterous hands; GR00T is best
  standalone, intent integration raises its macro-F1 **0.75 → 0.90**, and the intent model's p95 latency is
  **6.27 ms**. Real-robot evidence on one platform; author-reported; **no artifact on the arXiv record.**

### Observability and Replay (2)

- **`2611.08425` — Failure Prediction and Classification in Vision-Language-Action Models Via Calibrated Latent
  Probing** (cs.RO, cs.AI). VLA-Tug is a probe over a frozen VLA backbone's latent layers, supervised on the
  **pre-failure segment** so it alarms before the failure, with a threshold calibrated to hold
  missed-detection and false-alarm targets. Early detection at matched false-positive rates rises by up to
  **30%** over the strongest learned baseline; under shift the bounds stay valid in up to **86%** of runs versus
  at most **21%** for baselines; naming the failure mode raises top-3 accuracy on unseen tasks by up to **44%**
  over a majority baseline. The authors state they release **3,180 annotated episodes and over 288,000
  embeddings**. **Artifact status literal (verified this run) — not released:** the project page is live but is
  an anonymous review page listing **"arXiv soon"** and **"Data soon"**, and its code link
  (`anonymous.4open.science/r/VLA-Tug/`) returns **HTTP 401**. Author-reported; simulation plus real-robot
  steering demonstrations.
- **`2611.08144` — Detecting Silent Faults at the Environment-Trainer Boundary in Reinforcement Learning**
  (cs.RO, cs.LG, cs.SE; CoRL 2026 workshop). Makes the boundary a **wire protocol** and injects **23** boundary
  faults plus **2** trainer-log faults across Pendulum, HalfCheetah, Hopper and a seven-joint arm. Outcome checks
  are nearly blind: a learning-curve threshold caught **at most one run in ten** and a ten-seed comparison at
  most **21%**; a clean evaluation of **1,060** final policies agreed with the curves (**723 of 728** curve-clean
  runs within three standard deviations of fault-free seeds) while nearly all ended on different weights. **A
  step ledger kept on both sides caught and located every crossing fault**, with no fault-free false positive and
  a median **3%** extra training time; an environment-side episode record caught the trainer-log faults. The
  paper lists real faults in **Gymnasium 1.3** and five multi-agent libraries. **Artifact verified open
  (2026-11-07): `PlugRL/plugrl-protocol` (Apache-2.0, 36 files; normative `SPEC.md` whose checkable clauses are
  tests) and `PlugRL/step-ledger` (Apache-2.0, 33 files, created 2026-10-27)** with the
  Gymnasium/Stable-Baselines3 integration.

### Important revision folded in rather than added (1)

- **`2611.06817` — Open-HumanScale → Open-HumanWAM** (cs.CV primary; already curated on 2026-11-05). The **v2**
  revision (2026-11-05T16:35:16Z) renames the paper and the corpus (**Open-HumanScale-50K →
  Open-HumanWAM-50K**) and the artifacts moved with the name: the former Hugging Face dataset and collection
  identifiers now answer with **307 redirects** to `DAGroup-PKU/Open-HumanWAM-50K` and
  `DAGroup-PKU/open-humanwam`; the previously **empty** weights collection now holds **five ungated model
  repositories** (`HumanWAM-Goal-RoboTwin`, `HumanWAM-Vanilla-Base`, `HumanWAM-Vanilla-RoboTwin`,
  `HumanWAM-Vanilla-RoboDojo`, `HumanWAM-Vanilla-Exploration`), e.g. `HumanWAM-Vanilla-Base` at **24.1 GB** and
  `HumanWAM-Goal-RoboTwin` at **32.2 GB** with `transformer/`, `vae/` and a 2.47 GB `goal_cache.tar`; the corpus
  card moved from **4.71 TB / 26.52 TB (17.76%)** to a snapshot of **8.58 TB / 26.67 TB (32.17%)** while the Hub
  reports **14.19 TB** of used storage; the card still reads **"DATA UPLOAD IN PROGRESS"**, the dataset declares
  **no licence**, and the `open-humanscale` branch still has **no `LICENSE` file**. The entry was updated in
  place (title, links, artifact verdict); the reported numbers are unchanged from v1 and remain author-reported.

## Revision handling

24 records shared by the two harvests changed version inside this window. All were checked against the
repository and against their abstracts:

- **`2611.06817` v1 → v2** — substantive (rename + artifact move). The entry was **updated in place**; see above.
- **`2611.05163` v2 → v3** (already curated as *RoboTryOn*) — the v3 abstract is **byte-identical** to v2 over the
  portion compared and the page/table counts are unchanged; no artifact appeared and no claim changed, so the
  entry was left as written on 2026-11-05.
- The remaining **22** bumps (`2611.04216`, `2611.05003`, `2611.05004`, `2611.05445`, `2611.05578`, `2611.05592`,
  `2611.05612`, `2611.05637`, `2611.05729`, `2611.05730`, `2611.05793`, `2611.05942`, `2611.06046`, `2611.06149`,
  `2611.06157`, `2611.06194`, `2611.06433`, `2611.06476`, `2611.06543`, `2611.06559`, `2611.06748`, `2611.06849`)
  are model, application or benchmark papers outside the harness scope; none of them was curated before, none
  adds an artifact this list would carry, and none changes a claim already recorded here.

## Rejected and watch list

- **Robot/embodied candidates rejected after abstract or full-text reading** (each is a model, perception or
  application result without an external runtime/interface contract this list would own):
  **`2611.07651` GraspBench** (execution-aware cross-robot grasp evaluation, ICARCV 2026 — a useful protocol but a
  single-pipeline scoring study with no released harness; no artifact on the record); **`2611.07403` Dynamic
  watermarking / replay-attack detection** (ACC 2027 submission; a control-theoretic detector for a UR5 sim
  task, no artifact); **`2611.07411` Kinematic hypothesis context** (video codec for bandwidth-limited VLA
  uplinks — a real harness-adjacent result with a success cliff at 6 kbps, but its comment says code is "to be
  released upon formal publication", so nothing exists yet); **`2611.07517` Embodied RAG++** (adds the E2W-Bench
  pairing task, SII 2027; no artifact link on the record); **`2611.07493` open-vocabulary search with absence
  detection** (an honest specificity measurement — existing systems abstain on at most 9% of absent objects —
  but a perception/search result, no artifact); **`2611.07229` Spatial-UMI-Dream**, **`2611.08156` La-JEPA**,
  **`2611.08373` SLA**, **`2611.08413` T3R**, **`2611.08338` GPWM**, **`2611.07850` RoboImagine**, **`2611.08009`
  fractal impedance control**, **`2611.08368` VisionSIM**, **`2611.07912` GuidedWAM** and the rest of the 80
  cs.RO-touching records — model, world-model, control, sim, perception or driving results. La-JEPA is worth one
  line: it reports that the **officially released LIBERO-Plus evaluator corrupts instructions for most variants**
  and re-evaluates strong baselines, which is the same evaluation-instrument thread this list tracks, but the
  paper is a compact-policy result and its fix lives inside a model paper.
- **Digital-agent harness and benchmark candidates read and set aside** (plausible, but each is either a
  single-case study, a claim without an artifact, or a duplicate of a thread already carried):
  **`2611.07051` Closed-Loop Agent Evaluation** (layered measurement with versioned rules, ICC 0.86 vs 0.81 on one
  business-analysis case; no artifact); **`2611.07595` Evolver** (persistent "Gene" capability assets across
  Claude Code/Codex, +4.38 pp and −24.24% tokens on 320 paired executions; no artifact); **`2611.07652` Skillware**
  (51 pages; the MIT `ARPAHLS/skillware` repository is real with 178 stars, but the regime tested is
  typed-deterministic tool execution, not agentic robotics); **`2611.06996` TRAP** (runtime authorization policy
  characterized over 100 Monte Carlo traces, no artifact); **`2611.06989` Two-Ledger Model** (transaction-grain
  cost attribution; the public repository is BSD-3-Clause-Clear, but FinOps accounting rather than a harness
  contract); **`2611.07140` More Agents,
  More Reliable?** (reliability envelopes; a methodology note, no artifact); **`2611.07222` SPILLWAY** (a good
  deterministic admission/sanitization split with an Apache-2.0 repository, but agent-web security rather than a
  robot/VLA runtime and no embodiment); **`2611.07689` MCPHallu** (repository verified — its `LICENSE` file is Apache-2.0 while its README badge says MIT —
  and the README matches the paper's 358 tasks/36 servers/5,012 trajectories, but tool-use hallucination without
  a physical runtime);
  **`2611.07554` The Memory Trap**, **`2611.08281` Multimodal-SEA**, **`2611.07586` CoT-monitoring stress test**,
  **`2611.07624` Beneath the Surface**, **`2611.08134` Who Can Evaluate AI Independently?**, **`2611.07238` How Do
  AI Agents Waste Your Money?** — kept on the watch list rather than curated today.
- **Out-of-scope remainder** (driving perception, medical imaging and clinical models, video generation, protein
  and molecular work, weather/time-series forecasting, quantum, federated learning, recommendation, database,
  and ML-theory records among the 524) — read at title level and rejected as conventional model/application work
  under the standing scope rule.

## Validation performed

- **Markdown structure.** `README.md` renders as valid Markdown: the fourteen new entries and the four new
  landscape bullets sit inside their existing sections, each entry is a single top-level bullet with no nested
  list breakage, the section headings are unchanged in count and order (22 `###` headings), and the table of
  contents anchors are unchanged. `sources/daily-arxiv-2026-11-07.md` has one `#` heading and uses only `##`/
  `###` below it.
- **`git diff --check`** reports no whitespace or conflict-marker problems.
- **Links.** Every non-arXiv link added to `README.md` was resolved at the source this run:
  `github.com/harbor-framework/terminal-bench-2-1` (200, Apache-2.0, 47 MB, 136 stars, pushed 2026-11-05),
  `github.com/SugarRuy/BAR-Policy-Decision` (200, MIT, 456 KB, 116 files, pushed 2026-11-06),
  `hitch.axell.top` (200, JavaScript application; no code link), `robin-lab.cs.utexas.edu/oopsierecover` (200,
  videos and PDF only), `roboarcade.org` (200, leaderboard with per-run costs),
  `github.com/Satvik1701/RoboARCade` (200, public, **no licence**, pushed 2026-11-03),
  `github.com/rohungry/evaluation_harness_wrapper` (200, public, **no licence**, 132 KB),
  `vla-tug.github.io` (200, anonymous review page: "arXiv soon", "Data soon"),
  `github.com/PlugRL/plugrl-protocol` and `github.com/PlugRL/step-ledger` (200, Apache-2.0),
  `dagroup-pku.github.io/PyRUA-Lean/RSIJudge` (200), `github.com/DAGroup-PKU/PyRUA-Lean` (200, Apache-2.0; no
  RSIJudge code in its 244-file tree), `huggingface.co/datasets/DAGroup-PKU/Open-HumanWAM-50K` (200, ungated),
  `huggingface.co/collections/DAGroup-PKU/open-humanwam` (200, five ungated models), and the redirecting former
  identifiers (307). The two `anonymous.4open.science` code links (`SCALE-4E80`, `VLA-Tug`) return **HTTP 401**
  and are cited only in rejection statements; neither is described as open source.
- **Duplicate IDs.** All 14 new IDs and the revised record were checked against `README.md`, `docs/*.md` and
  `sources/*.md` before insertion (**0 of 524** batch records were previously named in the repository). After
  insertion each new ID occurs once as its entry link and once more in a landscape bullet where it is discussed;
  the updated `2611.06817` occurs three times, which is this repository's established pattern for an entry that
  is also cited in a landscape synthesis.
- **Dates and categories.** Every included entry states its arXiv ID, and its `<published>` date falls inside the
  harvested window (2026-11-03 late → 2026-11-05); no entry cites a 2026-11-06 or 2026-11-07 record, because
  none exists. Category placement follows each paper's contribution rather than its arXiv primary:
  `2611.08318` and `2611.07483` are cs.AI/cs.CR but sit with general harness methodology, `2611.08417` and
  `2611.07042` under *Unified Evaluation*, `2611.07441` and `2611.07964` under *What to Measure*, `2611.08390`
  under *Runtime Building Blocks*, `2611.07712` under *Safety*, and `2611.08425`/`2611.08144` under
  *Observability and Replay*.
- **Cross-file consistency.** The README badge, the `## Current Landscape` `Last verified:` line and the Contents
  blurb now read **2026-11-07**, matching this record's filename. The superseded sentence in the 2026-11-05
  landscape bullet (empty weights collection, 17.76% corpus) was annotated as superseded and points forward to
  the new note, so no file states a fact the newest verification contradicts. `docs/landscape.md` keeps its own
  earlier verification date because it is a historical note, not a maintenance surface.
- **Artifact-status discipline.** Every claimed artifact in the 14 entries is stated in one of four literal
  forms: **verified open with a licence** (Terminal-Bench 2.1 Apache-2.0; BAR MIT; PlugRL protocol and step
  ledger Apache-2.0), **public but unlicensed** (`Satvik1701/RoboARCade`, `rohungry/evaluation_harness_wrapper`),
  **inaccessible** (both `anonymous.4open.science` links, HTTP 401), or **promised / not released** (VLA-Tug's
  "Data soon", HITCH's project page, OopsieRecover's videos). Six entries claim no artifact at all and say so
  (`2611.07372`, `2611.08318`, `2611.08462`, `2611.07964`, `2611.07712`, `2611.07483`'s on-request data). No entry
  describes an unverified artifact as open source.
- **Author-versus-independent separation.** Every number in all 14 entries is marked as what the authors report;
  no paper in this batch has an independent reproduction. Evidence scope is stated per entry: simulation-only
  for `2611.08318`, `2611.08462`, `2611.08209`, `2611.07441`; simulation plus a real robot for `2611.08039`,
  `2611.07964`, `2611.08390`, `2611.07712` and `2611.08425`; digital-agent-only for `2611.07372`, `2611.07483`,
  `2611.08417` and `2611.07042`; and RL-infrastructure for `2611.08144`.
- **One cross-source discrepancy is recorded rather than smoothed:** Terminal-Bench 2.1's released repository
  says **26** tasks were modified while the paper says issues were found in "roughly **30%**" of the benchmark;
  and the Open-HumanWAM corpus card (**8.58 TB / 26.67 TB**) and the Hub's used-storage figure (**14.19 TB**)
  are different measurements of the same repository.

## Operational notes and blockers

- **Four consecutive runs have caught a "screened" day growing after the fact** (117 → 115 → 120 → 91 late
  records). The operational consequence is unchanged and should be treated as the pipeline's normal behaviour:
  **recount the previous two or three `submittedDate` ranges and diff against the on-disk harvest before writing
  "closed".** 2026-11-01 and 2026-11-02 held still this run (364 and 642), so a three-day lookback is enough.
- **The 2026-11-05 debt was real and worth 433 records, i.e. 83% of this run's new material.** At 09:02 UTC on
  its own day it returned 0; at 15:37 UTC two days later it is complete, with a last `<published>` stamp of
  `2026-11-05T18:59:42Z`.
- **A weekend gap is not a late batch.** 2026-11-06 and 2026-11-07 return 0 because the newest record in the
  index is a Thursday-evening submission and arXiv's schedule carries Friday's submissions to the Sunday
  announcement. Recording these as "not harvestable yet" rather than "screened" is what keeps the next run's
  interval honest; the record therefore leaves them explicitly owed.
- **`anonymous.4open.science` links are not verifiable artifacts from this host.** Two papers this batch
  (`2611.08390`, `2611.08425`) link a `/r/<name>/` review repository; both return **HTTP 401**, while
  `/status/<name>` returns a generic page. They are recorded as inaccessible, not as open.
- **A GitHub link can name an organisation rather than a repository.** `github.com/PlugRL` (the comment link in
  `2611.08144`) is an **organisation**; the REST API returns `Not Found` for it as a repository while its six
  member repositories carry the actual artifacts. `users/<org>/repos` is the right call.
- **Artifact claims can move to a new name under the same paper.** `2611.06817`'s v2 rename redirected both
  Hugging Face identifiers, populated a previously empty weights collection, and left the site URL and repository
  branch name unchanged — which is exactly the case where a stale entry silently becomes wrong. Re-checking the
  two previously curated revisions rather than only the 524 new records is what caught it.
- **A project page can promise a repository while a different public repository already exists.** RoboARCade's
  paper says "we will release the benchmark code", its site does not link a repository, and the first author's
  `Satvik1701/RoboARCade` (17 stars, no licence) already carries setup, `run_eval` and a dashboard. Both facts
  are recorded; neither is described as an open release.
- **`/tmp` is still non-durable across tool calls**, confirmed again; `.scratch/arxiv-2026-11-07/` holds every
  response, the three day harvests, eleven full-text renderings, the GitHub/Hugging Face JSON, the applied
  README edit script and its backup.
- **OpenSSH workaround still required.** `git fetch`/`git ls-remote`/`git push` fail with a "Bad owner or
  permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf" error; `GIT_SSH_COMMAND='ssh -F /dev/null'`
  restores access with the user's key and `known_hosts` unaffected.
- **Still owed: 2026-11-06 and 2026-11-07.** The next run must query both by `submittedDate` again — the Friday
  and Saturday batches should appear in the Sunday announcement — and **must recount 2026-11-05 and 2026-11-04**
  before trusting either count.
