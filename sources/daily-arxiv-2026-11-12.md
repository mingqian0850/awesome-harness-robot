# Daily arXiv scan — 2026-11-12 (the index stops moving: 2026-11-11 and 2026-11-12 both return 0, the frontier holds at 2026-11-10T18:59:54Z, four re-harvested days reproduce unchanged — and the required re-screen of the frozen batch recovers five entries the previous pass missed)

## Scope

- **Interval owed.** The previous successful run committed at **2026-11-11** (`86ad13e`) and produced record
  `sources/daily-arxiv-2026-11-11.md`. That record closed with an explicit debt:
  **"Query 2026-11-11 and 2026-11-12 and re-harvest 2026-11-10, 2026-11-09 and 2026-11-08 fully."**
- **This run** started **2026-11-12 11:14 UTC** (**12:14 CET**) on `main` at `86ad13e`, with `origin/main`
  also at `86ad13e`. The interval owed is therefore submission dates **2026-11-11 → 2026-11-12
  inclusive**, plus full re-harvests of **2026-11-08, 2026-11-09 and 2026-11-10**; **2026-11-07** was
  re-harvested as well, because the standing instruction after 2026-11-11's own finding is to recount
  every recent day on **every** run, not only the day after it was first seen.
- **Headline: nothing new was published, and the run still produced five entries.** Both owed days return
  **0** at 11:19 UTC on a Thursday, and the newest record visible anywhere in the index is still
  `2611.14519` at **2026-11-10T18:59:54Z** — the same frontier the previous run recorded 26 hours earlier.
  The four re-harvested days (2026-11-07 **334**, 11-08 **287**, 11-09 **564**, 11-10 **379**) reproduce
  their previous record sets exactly: **0 added, 0 removed, 0 changed** on every one. The procedure's own
  rule for this case — *"if the newest visible batch is unchanged from the previous run, re-screen that
  whole batch once before concluding nothing qualifies"* — is what carried the run: the second pass over
  the frozen 2026-11-10 batch, plus the recount of the earlier days, recovers **five** papers that are
  squarely inside this list's scope and were not recorded anywhere in the repository.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`, one parenthesised OR-query per
  day: `(cat:cs.RO OR cat:cs.AI OR cat:cs.CL OR cat:cs.CV OR cat:cs.LG) AND
  submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` with brackets percent-encoded. **20 export-API
  requests**: **eleven** count requests (one connectivity probe, seven day counts 2026-11-06 → 11-12, and
  three re-checks of 2026-11-11/11-12 spread across the run so a late batch could not be missed), **six**
  full-day harvests at `max_results=2000&sortBy=submittedDate&sortOrder=ascending` (11-07 → 11-12), and
  **three** descending frontier probes (`cs.RO`, `cs.AI`, and a `cs.RO` re-check).
- **No request returned the API's internal-error entry.** `grep -c 'api/errors'` on every response: all
  clean. The check stays in the method because a count that trusts `totalResults` alone can record a false
  zero.
- **Full-day harvests return exactly their counted totals**, so truncation is excluded: 2026-11-07 **334**
  (0.82 MB), 11-08 **287** (0.70 MB), 11-09 **564** (1.39 MB), 11-10 **379** (0.93 MB), 11-11 **0**,
  11-12 **0**.
- **Differential diff against the previous harvest, as a parsed set difference.** The previous run's
  `day-20261107.xml` … `day-20261110.xml` were re-parsed and compared over bare numeric ids and over
  `title`, `published`, `updated`, `summary`, `comment` and `journal_ref`. Result: **0 added, 0 removed,
  0 changed fields** on all four days. The raw byte sizes match the previous run's files exactly (815063,
  700149, 1388831, 933266 bytes); the md5s differ only because the Atom feed carries a request-time
  `<updated>` stamp, which is why the diff is parsed rather than byte-wise.
- **Frontier probe.** Descending `cs.RO` returns **`2611.14488`** at **2026-11-10T18:47:57Z** and
  descending `cs.AI` returns **`2611.14519`** at **2026-11-10T18:59:54Z** — identical to the values the
  2026-11-11 run recorded. A Thursday-morning probe therefore still sees a **Tuesday-night** frontier and
  **no Wednesday batch at all**: the announcement that should have landed at Thursday 01:00 UTC had not
  appeared by 11:19 UTC, which is the signature of a held or late batch rather than of an empty day.
- **Screening.** (a) all **379** titles of the unchanged 2026-11-10 batch in ascending ID order, read in
  full; (b) **20** abstracts read for records carrying a harness, agent-runtime, memory, monitoring,
  evaluation or robot signal; (c) **7** full-text HTML renderings (`arxiv.org/html/<id>v1`, all seven
  returned article bodies) read for artifact claims, evidence scope and the sentences the abstracts omit;
  (d) a concept-regex sweep over all **1,185** records of 2026-11-07, 11-08 and 11-09 (harness, VLA,
  agentic, embodied, manipulation, world model, skill, runtime, permission, verifier, self-improvement,
  recovery, teleoperation, robot, monitor) — **458** hits — used to check that earlier decisions were
  deliberate and to look for the same class of miss, with the 2026-11-10 batch screened by reading all 379
  titles instead; (e) resolution of **every** claimed artifact at the source, including repository metadata
  through the GitHub API rather than only the HTTP status code.

## What the index did

| query (one parenthesised OR-query per day) | 11-07 | 11-08 | 11-09 | 11-10 | 11-11 | 11-12 |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| count at 2026-11-12 11:19 UTC | **334** | **287** | **564** | **379** | **0** | **0** |
| recorded by the previous run | 334 | 287 | 564 | 379 | 0 | — |
| **new to this run** | **0** | **0** | **0** | **0** | — | — |

- **Zero growth on every re-harvested day, which is itself the finding.** Every previous record in this
  project reported that a day grew after it was first seen; 2026-11-05 and 2026-11-06 grew **again after
  a full re-harvest**. Today all four days are flat, and the two days after them are empty. The index is
  not moving at all: announcement has stalled after the 2026-11-10 batch, and no new day has entered it.
- **2026-11-11 and 2026-11-12 return 0 for different reasons.** 2026-11-12 is today and is not owed
  until its own batch announces. 2026-11-11 is a full day in the past and *is* owed — it returned 0 at
  11:19 UTC on Thursday, which is late for a Wednesday batch, so it is carried forward as debt rather
  than written off.
- **2026-11-06 returns 511** on the count probe, unchanged from the previous run, so the stall is
  consistent back through the week rather than an artefact of the days chosen.
- **No revisions.** With no new records and no `<updated>` movement on any of the 1,564 already-seen
  records across the four re-harvested days, the "meaningful revisions" half of the requirement is empty
  this run — checked by field diff, not by count.

## Inclusions (5)

Classification per the handoff: General Harness Methodology; Agentic Robot/VLA Harness; VLA; Robot
Foundation/World Model; Evaluation/Safety. Every number below is **author-reported**; no paper in this
window has an independent reproduction. Artifact status is stated literally, with the date it was checked.

- All five are **recoveries from the re-screen**, not new submissions: two are from the frozen
  2026-11-10 batch (`2611.14505`, `2611.14111`), and three from the re-harvested earlier days —
  `2611.12654` from 2026-11-09, `2611.11688` from 2026-11-08 and `2611.10884` from 2026-11-07. None of the
  five appears in `README.md`, `docs/*.md` or any earlier `sources/daily-arxiv-*.md` — grepped before
  insertion — and each was absent from the previous records' inclusion **and** exclusion lists, so these
  are first-pass misses rather than decisions being reversed.

### General harness methodology (2)

- **`2611.14505` — Continual-ARC** (`cs.LG`, 2026-11-10). Puts **memory-based and weight-based learning on
  one scale**, which is the measurement gap the whole harness-as-continual-learning line has been carrying:
  a lifelong stream built from the three ARC-AGI editions, where a task returns after a **dormant gap** with
  a **fresh generated instance** (25 of the 400 ARC-AGI-1 training tasks, **200** verified instances from
  the Re-ARC generators, so no cached answer fits), new tasks keep arriving, and every instance is scored
  by the share of a **16-unit feedback budget** it consumed — submit, and pay for each wrong attempt; buy a
  demonstration, and pay for it. After the stream the learner is frozen and tested twice: fresh instances
  of tasks it met (**memorization**) and **100 unseen ARC-AGI-1 evaluation tasks** (**generalization**).
  The result is a **negative result about harnesses**, stated with intervals: persistence in the harness
  "generally does not improve memorization unless provided with explicit expert guidance" — told to save
  liberally, prime-agent on GPT-6 Luna saved **0.42 [0.29, 0.55]** of the budget on seen tasks while
  hermes-agent saved **0.18 [−0.01, 0.39]** and openclaw **0.07 [−0.02, 0.17]**, both indistinguishable
  from nothing, and on Qwen3.5-9B **0.02 [−0.02, 0.07]**; with **expert guidance** the same harnesses reach
  **0.62–0.74**, which shows they *could* do the work and locates the failure in model judgment plus the
  harness's own prompts. On the held-out test **no harness method generalized measurably** — including the
  run in which GLM-5.3 Flash, given the guidance, wrote every program and note and solved **98%** of
  instances, and Qwen3.5-9B failed to use them — while **training weights** generalized, DAgger saving
  **0.29 [0.23, 0.35]** held-out against **0.57 [0.48, 0.67]** seen. The authors screened roughly **200**
  benchmarks and found none combining their five properties (fresh instance per exposure, recurrence after
  variable gaps, a task set that grows, charged binary feedback, and a protocol open to prompting, memory
  *and* weight updates). Digital-agent evidence only — no robot experiment, and the tasks are ARC grids
  precisely so that no domain knowledge is needed. **Artifact verified open (2026-11-12): Apache-2.0
  `unifyai/continual-arc`** (13 stars, 2.6 GB, created 2026-10-17, pushed 2026-11-11) — the benchmark,
  harness adapters, DAgger training and the analyses, with scripts that recompute the paper's numbers and
  rebuild every table and figure from the run directories.

- **`2611.10884` — Keeping LLM Agents on Procedure** (`cs.AI`, 2026-11-07). Answers a failure this list has
  collected piecemeal with a mechanism and a measurement: agents **drift** from a procedure stated once at
  the start of a long run, and the drift cannot be talked away — **restating the instructions changes
  nothing, and resuming a stopped agent removes one shortcut, after which it takes another**. The authors
  call these **escape routes** and make the finding structural: while one stays open the drift moves there.
  The fix is to move the procedure out of the context: an **executable finite-state machine** whose state
  lives outside the model's context, where compaction cannot touch it, enforced by the runtime at **three
  points of the agent loop** — a tool-call gate, a gate before a state transition, and a **stop gate** that
  refuses to let the agent end its turn before the workflow is complete. The gates read only which tools
  were called, for which account, and in what order, so the FSM enforces *how* the agent works, not *what*
  it decides. In a **pre-registered** experiment on 200-account SOP-Bench batches against six non-FSM
  baselines and two ablations, Claude Haiku 4.5 with the FSM decided **139 of 200** accounts correctly and
  by the procedure (8-seed means **139** against **9**, **15** and **48**; vs the strongest baseline 48,
  95% CI of the difference **[46, 130]**) and held that rate to the last account, while the gain tracked
  how far the plain agent drifted — large for Haiku 4.5 and for Sonnet 4.6 sessions that batched accounts,
  small for Qwen3.8-flash, which mostly held the procedure. The consequence the harness argument needs:
  **Haiku 4.5 with the FSM makes more correct procedural decisions than plain Sonnet 4.6 at 38% of the
  cost**, so long-horizon reliability can be bought in the runtime instead of the model. Extensions to
  SWE-bench Verified, τ²-bench and staged/cross-context workflows. Digital-agent evidence only.
  **No artifact located (verified 2026-11-12):** neither the record nor the full text carries a repository,
  dataset or package link, so the FSM, gates and SOP-Bench harness are not obtainable.

### Agentic robot / runtime monitors (3)

- **`2611.11688` — Streaming Failure Forecasting (SFF)** (`cs.RO`, 2026-11-08). Treats the **monitor** as
  the part of the harness that must survive deployment drift, and shows the cheapest adaptation that works:
  keep the offline monitor's network **fixed** and **refit only its last linear layer** after every episode,
  in closed form, from the one label every episode supplies — whether it succeeded — with an alarm
  threshold that moves only after **successes**. Ranking, not thresholding, is the property recovered,
  because a stale monitor's score *order* collapses after a task or policy switch and no recalibration can
  repair an order. Cost: **2.18 ms per policy call on a Jetson CPU core** and a **1.0 MB** state that never
  grows. In simulation, with every setting locked before test and false alarms held near target except
  under compound drift, it ranks failures better than the frozen monitor on **4 of 5** suites including the
  pre-specified primary test, and better than resetting on detected drift on **every** suite. The settings
  then carry unchanged to **434 recorded runs of four VLAs on real SO-101 arms** (ArmnetBench, replayed
  across **40** unannounced switches of task or policy version): pooled within-task AUROC rises **0.536 →
  0.656** (**+0.121 [+0.077, +0.162]**, one-sided Holm p < 0.05 pre-specified), and every VLA improves —
  SmolVLA **+0.163 [+0.046, +0.272]**, π0 **+0.106 [+0.061, +0.145]**, MolmoAct 2 **+0.150 [+0.040,
  +0.259]**, π0.5 **+0.064 [+0.016, +0.116]** — with the frozen monitor near chance after a switch. Two
  honest limits are recorded: the real-robot evidence is **replay of recorded runs**, not live closed-loop
  on the authors' own arms (which the paper lists as next), and the reset/retrain comparisons are marked
  exploratory. **Artifact status literal (verified 2026-11-12): promised, not released.** The paper says
  the protocols, the record of when each result was first opened, and the code "will be released at
  `sff-research.org`"; that site is live (HTTP 200) but is a research blog — its links are its own posts
  and there is no repository, archive, weights or download of any kind, so nothing is obtainable today.

- **`2611.14111` — Acquisition-State Telemetry (AST)** (`cs.RO`, 2026-11-10). Adds the missing question to
  runtime monitoring: a system that monitors kinematics, environment state and outcomes still cannot tell
  whether its **information gathering is working**, so it can stay active while never resolving its own
  mission requirements. AST is **model-free and task-referenced**: a domain interface maps raw observations
  into task-resolution coordinates and normalized comparative representations, and a **fixed causal
  monitor** — no parameters trained, update rules identical across domains — emits two continuous signals,
  **prospective progress** across unresolved requirements and **informational friction** (recurrent evidence
  with negligible informational gain). One unadjusted configuration with fixed thresholds (friction above
  **0.20** = stalled, resolution at **0.80** = complete) is applied to **eight** substrates spanning
  dialogue (SAR witness interview, MultiWOZ, HCRC Map Task, AnnoMI), discrete exploration (**MiniGrid**,
  **Crafter**), **continuous robotic manipulation (MetaWorld, simulation)** and embedded intent inference
  (BOIR): friction separates unproductive stagnation from purposeful exploration before completion and
  stays muted afterwards, and in **closed-loop reinforcement learning under early-termination constraints**
  penalizing friction let agents sustain mission success where unpenalized policies and baselines with
  matched operational cost failed to finish the requirements. This is a reusable runtime interface rather
  than a detector for one task — which is why it belongs beside the VLA probes in this list — but the robot
  evidence is **simulation only** (MetaWorld), with no physical-robot run. **Artifact verified open
  (2026-11-12): `dimipan/ast-telemetry`** (100 MB, created and pushed 2026-11-02; monitor, substrate
  bindings and experiment scripts with a single command that reproduces every result, table and figure) —
  the repository **declares no licence** and carries no description.

- **`2611.12654` — Correctness Boundaries of Typed Plans for Natural-Language Robot Fleet Control**
  (`cs.RO`, 2026-11-09). Separates what **plan validation and the execution report** establish from what the
  fleet actually did, in a three-level ROS 2 implementation with **twenty simulated robots**: commands are
  traced through typed plans, deterministic corrections, fleet coordination and outcome checks, with an
  offline benchmark of **44** cases over plan structure and selected command requirements. The pass rates
  are high — **70.0–87.3%** of model responses across five local configurations without a response schema,
  and **all runs** in a selected **18-run** series meeting their **pose-based** task conditions — and that
  is the point: saved plans and logs still document **duplicate pickup assignments** and goal adjustments
  inside the original task zones, a separate formation trace shows a **structurally accepted plan with the
  wrong formation shape** next to a correctly rejected route, and an **arrival report omitted a requested
  robot** after replanning. Completion records cover pickup plus subsequent drop-off zone presence for the
  same robot, so the cases distinguish a change to executable actions from a change to the requirements a
  completion report claims, and motivate checking effective assignments and reported outcomes against the
  original command. This is the **verification boundary** this list keeps meeting from the other side —
  `moot success` (TRACE) for a `Success` whose purpose expired, EffectMatch for uncompared persistent
  effects — stated here as what a validated plan does *not* certify. Simulation only, author-reported, and
  a case analysis over a selected series rather than a controlled benchmark comparison. **Artifact verified
  open without a licence (2026-11-12):** `Innopolis-Robotics-Society/llm_swarm` (9 stars, pushed
  2026-11-10) carries the benchmark outputs, session metadata, logs, derived metrics and analysis scripts
  under `paper/results`, alongside a **135 MB** public results archive (`llm_swarm_results_2026-11-04.tar.zst`,
  SHA-256 `3866f1fc…9f740d`) with a per-file manifest; the repository declares **no licence**.

## Excluded and watch-listed

**Re-screened and rejected (second pass, this run):**

- **`2611.14068` — What a CAGE 4 Score Measures** (`cs.CR`). The strongest paper the second pass found that
  is still not listable: it audits a learned cyber-defence benchmark and finds the **harness** decides the
  number — the simulator silently discards an action asserted while the agent's previous action is running
  and charges the same point for it, switching that check off at evaluation costs the released KEEP policy
  **99.58** points per episode and H-MARL **145.98**, and holding interventions until the agent is idle on a
  supervisory harness is worth **25.3** and **27.6** points — plus 88.9% of per-episode outcome variance
  being the policy's own sampling draw and five published tables that do not state the artifact, sampling,
  criterion and adversary that would make them comparable. The instrument is real and MIT-licensed
  (`jrice-ai/cage`, tag `arxiv-v1`, plus a Zenodo data deposit). Excluded on the standing domain rule
  rather than on quality: a **cyber-defence multi-agent benchmark audit** carries no robot, VLA or agent
  harness interface this list can place, and its transferable claim (an action-admission contract inside
  the evaluator dominates the score) is already carried by this list's evaluation-integrity entries. Watch
  if it is ever restated for embodied or tool-using agents.
- **`2611.14009` — Coding Agent Memory Post-Training II (CAMG-II/CAMP-II)** (`cs.LG`). A five-environment
  long-horizon agentic-RL suite whose memory interface is ordinary Bash plus an episode-persistent
  workspace, with **2.97×** the gain of the strongest on-policy-distillation baseline and transfer to
  Terminal-Bench 2.1 and SWE-Bench Pro V2. Relevant to this list's memory line, but the contribution is
  **training a policy** to use file memory, not a harness contract, and the duplicate rule applies: memory
  admission is already carried by FGSE and by WikiTrace in the previous record. **No artifact:** neither the
  record nor the full text links a repository, dataset or checkpoint.
- **`2611.13993` — Temporal Behavior Trees for RL (TBTCL)** (`cs.LG`). One specification used
  simultaneously as task, dense reward, curriculum and diagnostic, with a curriculum DAG and trace
  segmentation into competence profiles — a genuinely harness-shaped idea (the specification is the
  runtime's object). Excluded for scope and artifact: the evaluation is six specifications in discrete and
  continuous RL domains with **no robot experiment**, and **no artifact** is linked.
- **`2611.13946` — TreeAgent**, **`2611.14017` — Personalized Memory Rubrics** (`cs.AI`). Structure-aware
  and per-user memory policies; TreeAgent's Apache-2.0 code is live (`ZJU-LLM-Safety/TreeAgent`). Both are
  model-side memory methods with digital-agent benchmarks, and memory admission is already represented;
  excluded by the same duplicate rule, with the artifacts noted for a future re-read.
- **`2611.13669` — GenEA**, **`2611.13850` — URDF2Ontology** (`cs.AI`), **`2611.14364` — ARE-bench**
  (`cs.CL`), **`2611.13713`** and **`2611.13705`** — hardware-verification agents (`cs.AI`).
  Respectively: a cognitive model of embodied plan inference, a URDF→OWL grounding pipeline, a digital
  research-agent benchmark and two invited ASP-DAC papers on agentic EDA. Each has a real claim but none introduces a
  robot harness, recovery, permission or evaluation contract this list does not already carry, and none
  releases a located artifact.
- **Still excluded from earlier records:** the standing model-versus-harness rule was re-applied to the
  frozen batch with no reversals — `2611.14048` LaST0.5, `2611.14420` OnlyAct, `2611.14257` Elastic-WAM,
  `2611.14365` MetaWAM, `2611.13819` game-enhanced embodied pretraining, `2611.13732` CIALLO,
  `2611.13610` Ego2Robot survey and `2611.13592` few-shot residual adaptation remain model or architecture
  papers without a harness interface.

**Watch list (unchanged, re-checked where cheap):**

- `2611.14320` WikiTrace (Apache-2.0 `yukyeongleee/wikitrace` live), `2611.13355` DEPUTYBENCH (code and data
  "at a later date"), `2611.14257` Elastic-WAM and `2611.14365` MetaWAM — no change since the 2026-11-11
  record, which remains authoritative for their status.
- **RoboACE and the kNN paper keep their literal status:** `DeLin1001/RoboACE` is an **empty repository**
  and `sereact/lerobot_knn` returns **404**. Neither was re-verified today because nothing in the index
  changed; the previous record's verification stands.

## Artifact findings that changed an entry

- **A live project page is not a release.** `2611.11688` points at `sff-research.org` for its "protocols,
  the record of when each result was first opened, and code". The domain resolves (HTTP 200) and is a
  research blog with posts, figures and dated results, but **no repository, archive, weights or download**,
  so the promise is recorded as such. It is a distinct artifact state this list now has to name
  alongside empty, 404 and unlocatable: **promised at a live address**.
- **A large results archive behind an unlicensed repository is a real release.** `2611.12654` publishes
  benchmark outputs, session logs and analysis scripts, plus a 135 MB bag archive with a published SHA-256
  and per-file manifest. The archive exists and is checksummed; the repository nonetheless declares no
  licence, which is recorded rather than glossed.
- **The five inclusions resolve to four distinct artifact states.** One is verifiably open **with a
  licence** (Apache-2.0 `unifyai/continual-arc`), two are open with **no licence declared**
  (`dimipan/ast-telemetry`, `Innopolis-Robotics-Society/llm_swarm`), and two have **nothing obtainable**
  (`2611.10884`, which links nothing at all, and `2611.11688`, which promises its code at a live project
  page).

## Files modified

- `README.md` — badge to **2026-11-12**; Contents blurb rewritten for this run; `## Current Landscape`
  verification date and **two** new bullets; **five** new entries across three subsections
  (*General Harness Design and Self-Improvement*: `2611.14505`, `2611.10884`;
  *Observability and Replay*: `2611.11688`, `2611.14111`; *Benchmarks and Evaluation → What to Measure*:
  `2611.12654`).
- `sources/daily-arxiv-2026-11-12.md` — this record (new).
- Untouched by design: `docs/reference-architecture.md` (user modification), `docs/ring-harness.png`
  (untracked user file), `handoff.md` (untracked by design), `.scratch/` (working data, never staged).

## Validation performed

- **Markdown structure.** Heading inventory unchanged: **17** `##` sections and **22** `###` subsections
  before and after; every new entry is a single list item under the correct subsection; no heading is
  preceded by a non-blank line, no list item sits directly under a heading, and no line carries trailing
  whitespace.
- **`git diff --check`** — no whitespace errors.
- **Link resolution.** Every URL added by this run returned **HTTP 200**: five `arxiv.org/abs/` links,
  `github.com/unifyai/continual-arc`, `github.com/dimipan/ast-telemetry`,
  `github.com/Innopolis-Robotics-Society/llm_swarm`, `sff-research.org` (checked while establishing that it
  hosts no code), and the three in-list cross-references the new entries make to existing entries
  (`2610.37461` Concordat, `2609.30123` HEXIS, `2610.38400` moot success).
- **Duplicate check.** All five ids were grepped against `README.md`, `docs/*.md`, `sources/*.md` and
  `CONTRIBUTING.md` **before** insertion; none was present, in an inclusion or in an exclusion. After
  insertion each id appears exactly **three** times — once in its entry, once in the Landscape bullet that
  cites it, and once in the Contents blurb — which is the pattern every recent record uses.
- **Dates and categories.** Every included entry names its arXiv ID and its `<published>` date — 2026-11-07,
  11-08, 11-09, 11-10 — and **none is dated 2026-11-11 or 2026-11-12**, for which no record exists.
  Category placement follows the contribution: two general-harness entries, two runtime monitors for robot
  and VLA execution, one evaluation-contract entry.
- **Cross-file consistency.** The README badge, the `## Current Landscape` `Last verified:` line and the
  Contents blurb all read **2026-11-12**, matching this record's filename. `docs/landscape.md` keeps its
  earlier verification date as a historical note rather than a maintenance surface.
- **Evidence-scope discipline.** Stated per entry: digital agents only (`2611.14505`, `2611.10884`);
  simulation plus **replayed** recorded real-robot runs, not live hardware (`2611.11688`); simulation only
  for the robot domain (`2611.14111`, MetaWorld; `2611.12654`, 20 simulated robots). No entry implies
  independent reproduction, and no entry describes a promise, a placeholder or a live-but-empty page as
  open.

## Operational notes and blockers

- **A frozen index is a distinct state from an empty day, and only the frontier probe tells them apart.**
  A zero on a past day can mean "not yet announced" (2026-11-10 on its own day), "unindexed weekend"
  (2026-11-07/08 as previously misread), or, as here, "announcement itself has stalled": the newest id in
  the index has not moved for 26 hours. Recording the frontier **with its timestamp** is what makes the
  three cases separable in a later record, so it is now part of the method rather than an optional check.
- **The re-screen rule paid for itself again.** Five entries — two general harness papers, two
  runtime monitors and one evaluation contract — were sitting in batches this project had already
  harvested, re-harvested and read once. The first pass over a 379-record batch is evidently not a filter;
  the second pass with abstracts and full texts is where the harness-relevant population actually
  surfaces. The standing instruction is therefore restated as: **re-screen the newest unchanged batch
  every run, and prefer abstracts over titles for records touching runtime, memory or evaluation.**
- **Frozen harvests diff cleanly, which is the check that matters when nothing is new.** Because the four
  days reproduce at 0 added / 0 removed / 0 changed over six fields, the run can state positively that no
  revision was missed rather than merely that no count moved.
- **No API internal errors, no rate limits, no OpenSSH failure on the first attempt.** Twenty
  `export.arxiv.org` requests and four `api.github.com` calls completed without a 403; the OpenSSH
  `/etc/ssh/ssh_config.d` workaround (`GIT_SSH_COMMAND='ssh -F /dev/null'`) was needed only for
  `git ls-remote`, as in every recent run, and was not needed again for `push`.
- **Debt for the next run.** Query **2026-11-11, 2026-11-12 and 2026-11-13** — 2026-11-11 and 11-12 are
  both owed and both returned 0, so they must be re-queried until their batches appear — and re-harvest
  **2026-11-06 through 2026-11-10** fully. If the index is still frozen at the 2026-11-10 frontier, treat
  it as an announcement stall, record the frontier timestamp again, and **re-screen the 2026-11-09 and
  2026-11-08 batches with abstracts** rather than titles, because the recoveries this run came from
  exactly those two passes.
