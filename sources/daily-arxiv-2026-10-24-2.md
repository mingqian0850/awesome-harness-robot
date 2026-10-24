# Daily arXiv scan — 2026-10-24 (second pass: unchanged-batch re-screen + 2026-10-20/21 tail)

## Scope

- **Interval since the last successful run.** The 2026-10-24 00:05 CEST catch-up run finished at
  **00:24 CEST** (state file epoch `1792794269`) and covered submission dates **2026-10-02 →
  2026-10-22**; its own record states that submissions dated 2026-10-23 onwards were not yet indexed
  and must be re-scanned. This run started **2026-10-24 12:19 CEST** on `main` at `6cf165d`, i.e. the
  interval it owes is **submission dates 2026-10-23 → 2026-10-24**.
- **There is no new arXiv content in that interval yet.** The export API index still ends at
  **2026-10-22**, for both date axes:

  | query (all five categories) | cs.RO | cs.AI | cs.CL | cs.CV | cs.LG |
  | --- | ---: | ---: | ---: | ---: | ---: |
  | `submittedDate:[20261023]` | 0 | 0 | 0 | 0 | 0 |
  | `submittedDate:[20261024]` | 0 | 0 | 0 | 0 | 0 |
  | `lastUpdatedDate:[20261023 TO 20261024]` | 0 | 0 | 0 | 0 | 0 |
  | `submittedDate:[20261022]` (control) | 57 | 169 | 89 | 101 | 151 |
  | `lastUpdatedDate:[20261021]` / `[20261022]` (control, cs.RO) | 70 / 57 | — | — | — | — |

  So no new submission **and no new revision** is visible since the previous run; the frontier has
  not moved in the ~12 hours between the two runs.
- **Consequence, per procedure.** The newest visible batch (2026-10-22) is unchanged from the
  previous run, so that whole batch was **re-screened once from scratch** rather than concluded as
  exhausted: all **443** unique records with `submittedDate = 2026-10-22` were re-harvested
  (identical count to the previous run) and every title was read, not only the keyword hits.
- **Supplementary tail re-screen.** Because the previous run was a 21-day catch-up that harvested
  three-day batches under an HTTP 429 incident, the two days immediately preceding the frontier —
  **2026-10-20 and 2026-10-21** — were re-harvested as a recall check (**1,160** unique records).
  This found twelve on-topic records that appear **nowhere** in `sources/daily-arxiv-2026-10-24.md`
  (neither as inclusions nor as recorded deferrals), i.e. first-pass misses rather than deliberate
  rejections. Five of them are included below with reasons; the rest are recorded by ID so a later
  run does not have to re-litigate them from title alone.
- **Categories.** `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, `cs.LG`, as in every run.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`, one request per category
  per day with `max_results=600`, 3–4 s between requests, XML saved under
  `.scratch/arxiv-2026-10-24b/` (`.scratch` is untracked and never committed; `/tmp` does not
  survive between tool calls on this host). Requests: 10 for the 2026-10-22 batch (5 categories ×
  one date), 10 for the 2026-10-20/21 tail, 4 count probes, 1 `id_list` version check, 3 HTML fetches
  and 3 PDF-fallback checks for artifact triage. **No HTTP 429 this run** — the rate limiting that
  dominated the previous run did not reproduce at 3–4 s spacing.
- **Screening.** (a) Full title review of the 443-record frontier batch. (b) Keyword-profile screen
  (harness / agent loop / scaffold / tool use / self-improvement / self-evolution / memory / recovery /
  runtime / observability / monitoring / safety / guardrail / permission / sandbox / evaluation ×
  robot / embodied / manipulation / VLA / world model / humanoid / navigation / agentic) over the
  1,160-record tail. (c) Abstract review of **16** shortlisted papers. (d) Artifact verification of
  the **10** included and **6** rejected/watch papers against live pages and repository metadata.
- **Deduplication.** Every shortlisted ID and title was grepped across `README.md`, `docs/*.md`, and
  `sources/*.md` before inclusion; the four 2026-10-22 records already curated by the previous run
  (`2610.31401`, `2610.30917`, `2610.31162`, `2610.31427`) were excluded on that basis. Name
  collisions were checked separately: **CARE** already exists in the list as
  [CARE: Experience-Guided Atomic Corrective Execution](https://arxiv.org/abs/2609.24118), which is
  why the unrelated `2610.30587` ("Know When to Escalate", also self-named CARE) is *not* added.
- **Revisions.** The `lastUpdatedDate` sweep for 2026-10-23 → 2026-10-24 returned **0** records in
  all five categories, and the previous run's sweep had already found no revision of a curated ID in
  its window. **No existing entry needed a status or number update.**

## Included (10)

### Agentic Robot and VLA Harnesses

- **Agentic World Action Models (AW-0)** (`2610.30136`, cs.RO; v1 2026-10-21) — why it qualifies:
  it does not wrap a harness around a world-action model, it makes the program a first-class
  generative stream, so one artifact supplies code-as-policy, skill reuse, and prediction. Reported
  on RoboTwin 2.0 (50 tasks × 50 rollouts): **80.8%** clean success versus **64.8%** for the
  strongest baseline (WLA), but **20.6%** under visual randomization — *below* the π0.5 VLA baseline
  (27.7%) — while the hybrid code/action-chunk mode reaches **63.1%** clean and **29.2%**
  randomized. Real-world bimanual evidence is on a YAM station. **Artifact verified open
  (2026-10-24):** BSD-2-Clause `ZhuoyangLiu2005/agentic-wam` (67 stars, created 2026-10-04, pushed
  2026-10-22) and an ungated BSD-2-Clause checkpoint
  `miniFranka/agentic-wam-pretrain-yamego3k-soup-e1` (37 downloads). Author-reported.

- **Memory before Goal / WholeBodyMem** (`2610.31166`, cs.CV/cs.LG/cs.RO; v1 2026-10-22) — why it
  qualifies: it makes pre-history the runtime's explicit responsibility (object-event-selected key
  frames inside an adaptive budget, read by the action expert's cross-attention) and contributes a
  memory-dependent benchmark, **HuMeGo** (six simulated + six real Unitree G1 tasks). Reported:
  base-policy success **29.1% → 72.6%** in simulation and **36.7% → 88.3%** on the physical G1, with
  SIMPLE also improving **70.2% → 86.9%**. **Artifact status literal:** the project page is a
  co-author draft that still displays "values marked TBD" and is attributed to anonymous authors
  under review; no code, data, or benchmark release is linked. RA-L submission; author-reported.

### Runtime Building Blocks

- **Progress Probes P²** (`2610.31613`, cs.RO; v1 2026-10-22) — why it qualifies: it is a runtime
  monitoring contract that removes the external judge, which is what makes VLA self-monitoring
  affordable. Linear probes on π0.5 activations from demonstrations estimate progress and flag
  deviation, with **no failure labels** and **0.026 ms** added latency per step. Reported: six
  VLABench tasks at **0.909 episode / 0.788 state AUROC**, competitive with failure-supervised SAFE;
  unseen-task **0.900 / 0.794** over all 20 three-seen/three-unseen splits; TPR@5%FPR 0.54 at 75% of
  episode and 0.64 at the end, failures detected at 0.60 of the episode on average; hardware AUROC
  **0.78** on a Franka Research 3 with DROID cameras and a fine-tuned π0.5-DROID. **No official code
  or probe weights** were linked as of 2026-10-24 (arXiv HTML/PDF checked). Author check: this is the
  same group's successor to the list's existing `2608.13474` entry ("Decoding Task Progress from VLA
  Representations") — same first and senior authors (Atiksh Bhardwaj, Preston Culbertson) — so the
  earlier entry is left unchanged and the new README text says so explicitly.

### Safety

- **Certified Execution Harness for Agentic AI** (`2610.31376`, cs.AI/eess.SY; v1 2026-10-22) — why
  it qualifies: it is the missing formal layer under this list's permission-surface attacks. The
  execution harness becomes a Petri-net supervisory-control problem with marks for capabilities,
  approvals, and budgets; the novel result is that in the **replanning closed loop** (an agent may
  retry after refusal) the conventional maximally permissive supervisor can be *arbitrarily more
  intrusive* than a stricter safe one, and the maximally permissive least-intrusive supervisor is a
  value-threshold refinement. One worked instantiation (an autonomous release agent); **no benchmark
  evaluation and no artifact** (verified 2026-10-24). Digital tool-use agents, no robot experiment.

- **Alpha: Systematic Fault Injection and Robustness Evaluation for LLM-Driven Embodied
  Manipulation** (`2610.30197`, cs.RO; v1 2026-10-21) — why it qualifies: it localizes the fault
  instead of scoring the outcome. Paired 150-episode matrix, LLM agent versus Scripted Oracle under
  identical seeds: Oracle **64.0% (48/75)**, LLM **44.0% (33/75)**, McNemar p = 3.01×10⁻⁴ — but
  excluding planner-output corruption the two are **identical at 55.0% (33/60) with zero discordant
  pairs** (p = 1.00), so the whole gap is the **prompt→JSON serialization/validation interface**
  (100% Oracle versus 0% LLM on that arm), not a reasoning deficit. **Artifact status literal:** the
  linked repository redirects to `jonajoy142/alpha-embodied-robustness`, a 12-week build log whose
  own checklist leaves the metrics harness, full experiment matrix, and public cleanup open, with
  **no LICENSE file** — source-available scaffolding, not a reproduction artifact. Simulation-only.

### Observability and Replay

- **AgentTriage: Replayable Security Diagnosis Across Heterogeneous Agent Runtimes** (`2610.30738`,
  cs.AI/cs.CR; v1 2026-10-22) — why it qualifies: the diagnosis itself becomes an executable,
  re-checkable contract with mandatory abstention, instead of an LLM's account of what happened.
  One contract over five runtimes covers all **159** applicable executions; on **120** adjudicated
  cases **0.858** grounded joint diagnosis, **0.966** required-evidence coverage, **0.036**
  unsupported-claim rate, **+0.175** (95% CI [0.092, 0.258]) over the strongest same-schema
  baseline. **Verified open (2026-10-24):** Apache-2.0 `AsnGs/agenttriage-open-source` (24 MB;
  canonicalizer, typed provenance graph, chain extraction, defense-gap attribution, five runtime
  overlays, contracts, schemas, pinned upstream agent snapshots, tests, a 53-case behavior repetition
  and 40 label-free evidence pages). Digital agents, no robot experiment.

### Benchmarks and Evaluation

- **AstroBench** (`2610.31064`, cs.RO; v1 2026-10-22) — why it qualifies: a real released benchmark
  in a regime the usual suites skip (free-floating capture), with useful honest baselines: a frozen
  **π0.5 DROID** captures **2/200** static and **4/200** moving targets, while a cube-specific ACT
  policy captures **8/10** and **6/10**. Nine tasks, 29 task–target combinations, two workcells
  (MELFI V6, RRM TB1), **8,414** Objaverse objects, five cabin contexts, deterministic episode plans
  and scoring. **Verified open (2026-10-24):** Apache-2.0 repository with package, Blender/GLB
  assets, docs, tests, CI, and a live site. Simulation-only, author-reported.

- **RoboCasa-Shift** (`2610.27861`, cs.RO; v1 2026-10-20) — why it qualifies: it supplies the
  matched-pair and stage-depth reporting that this list's "What to Measure" block demands.
  **1,040** formal rollouts (4 gated RoboCasa365 tasks × 20 base episodes × 13 conditions) for
  GR00T N1.5 Composite-Seen, clean macro SR 0.738; identical aggregate ΔSR hides different
  behaviour (camera L2 and blur L1 both 0.188, but 19.0% versus 36.4% of their success→failure
  transitions occur before the first tracked milestone). **Verified open (2026-10-24):** MIT
  repository with frozen protocol and StageTracker, plus an ungated CC-BY-4.0 Hugging Face release of
  the formal evaluation records. Simulation-only, author-reported.

- **RoboJudge** (`2610.27782`, cs.CV; v1 2026-10-20) — why it qualifies: it evaluates the judge that
  world-model papers now use as their metric. **RoboJudge-Bench**: 800 human-annotated videos from
  8 corpora and 10 generators scoring Physical Adherence and Instruction Alignment; general MLLMs
  agree less on physical adherence, and general-domain video judges do not transfer. Fine-tuned
  **RoboJudge-9B** reaches **0.719** Pearson, **+0.089** over the best proprietary baseline.
  **Verified open (2026-10-24):** Apache-2.0 code, ungated CC-BY-NC-4.0 benchmark dataset (955
  downloads), ungated Apache-2.0 judge weights (62 downloads). Author-reported.

### Surveys and Reading Lists

- **Toward Recursive Self-Improvement in Physical AI** (`2610.29677`, cs.RO; v1 2026-10-21) — why it
  qualifies: a 93-page synthesis (15 figures, 21 tables) that supplies the missing evaluation
  protocol for the self-improvement line this list tracks, and states the negative result plainly —
  the reviewed physical studies have **not isolated a gain attributable to recursion**. It gives the
  four carry-forward conditions (feedback still addresses a remaining need; the receiver can use the
  change; value survives task or hardware change; practice stays safe and affordable).
  **Verified open (2026-10-24):** MIT `Physical2RSI/Awesome-Physical2RSI` (24 stars, pushed
  2026-10-23) plus a live project page. No experiments of its own.

## Revision edits to existing entries

None. No revision of an already-curated ID is visible in the interval (`lastUpdatedDate` sweep for
2026-10-23 → 2026-10-24 returned zero records in all five categories).

## Current Landscape additions

**Four** bullets were appended to the existing list:

1. **The execution harness now has a formal safety theory, and it is not the theory the gate
   implements** — [Certified Execution Harness](https://arxiv.org/abs/2610.31376), placed next to the
   permission-surface bullets it formalizes.
2. **VLA monitoring is being pushed inside the policy and into the failure timeline** —
   [Progress Probes P²](https://arxiv.org/abs/2610.31613), with [RoboMonitor](https://arxiv.org/abs/2609.30715)
   and [ProTracer](https://protracer-failure.github.io/) as the label-free / label-cost / onset contrast.
3. **Evaluation is turning to what the aggregate metric cannot see: pairing, execution depth, and the
   judge itself** — [RoboCasa-Shift](https://github.com/HaochengWang23/RoboCasa-Shift),
   [RoboJudge](https://spherelab.ai/RoboJudge), [AstroBench](https://github.com/AstroUpLab/AstroBench).
4. **The recursive-self-improvement agenda is arriving in physical AI with a measurement gap
   attached** — [Toward Recursive Self-Improvement in Physical AI](https://arxiv.org/abs/2610.29677),
   cross-referenced to [Phantom Gains](https://arxiv.org/abs/2608.20290).

AW-0, WholeBodyMem, AgentTriage, and Alpha are **not** given new landscape bullets: the list already
carries adjacent bullets for code-as-policy inside the episode, VLA memory as persistent state, and
harness observability, and adding four more would restate them. They are reachable from their
sections instead.

## Rejected / watch list

### Verified and rejected or held for artifacts

- **RGraph-Eval** (`2610.31121`, cs.RO) — an on-topic evaluation framework (task-grounded temporal
  scene graphs, ShareRobot-SG with 1,000 annotated manipulation scenes) whose paper says "Code will
  be publicly available at" `github.com/SashaDance/rgraph-eval`. That repository contains **one
  1.3 KB README and an `assets/` folder**, ending "Code, evaluation scripts, and dataset release
  details will be added here", and asserts no licence (verified 2026-10-24). **Not open. Watch.**
- **CARE / Know When to Escalate** (`2610.30587`, cs.RO/cs.DC) — critic-guided escalation from a
  local executor to a cloud model with a shared handoff memory; on-topic as a runtime authority
  contract, but there is **no artifact** and, more decisively here, its self-chosen name collides
  with the already-listed [CARE](https://arxiv.org/abs/2609.24118). **Deferred** rather than listed
  twice.
- **TEAR: Task-Evidence Authority Routing** (`2610.30819`, cs.RO) — allocates control authority
  between human teleoperation and local autonomy from network-state and task-semantic evidence
  (teleoperation limited to 19.8–27.4% of steps; +30–40% over autonomy-only in the authors' setting,
  simulated and real trials). A control-routing policy rather than a harness contract or released
  interface; no artifact. **Deferred.**
- **EventSync** (`2610.27917`, cs.RO) — event-driven memory updates with an Event Monitor gating
  subgoal completion; 64.5% on RoboMME and four real-robot tasks, but no artifact linked on the
  record or in the HTML. Memory is already covered this pass by WholeBodyMem with a stronger
  artifact story. **Deferred.**
- **LocoManiLab** (`2610.30876`, cs.RO) — a unified humanoid loco-manipulation testbed (234 tasks,
  51.5 h simulated teleoperation, 12 policies, three sim-to-real tasks). Substantive, but no
  repository or project link appears on the record, so the testbed itself cannot be checked.
  **Watch.**
- **AgenticRoboRL** (`2610.28589`, cs.RO) — post-training robot planners across harnesses via
  harness-specific adapters (LIBERO-PRO 18.1% → 71.0% for a 9B planner). Reads as a strong result
  but the abstract says "**We will release** the framework and trained planner checkpoints" — future
  tense, so nothing is recorded as open. **Watch.**
- **Echo** (`2610.31569`, cs.RO) — stop/failure signals denoised alongside action chunks; project
  page live, but the linked `lyfeng001/Echo-Sim` repository is **simulation code only** (230 MB, no
  licence) while the reported evidence is real-world. **Watch.**
- **Dual Exam** (`2610.31066`) and **An Introduction to World Models for Generalist Agents**
  (`2610.30789`) — a controlled agent/world-model split evaluation and a tutorial, respectively;
  both useful but neither releases an artifact or a reusable contract. **Deferred.**
- **Multi-Tool Selection for Robotic Systems** (`2610.31013`) — routing among robot capabilities
  compared against a 120B LLM baseline (0.7822 versus 0.8869 macro score at far lower latency).
  Competent, small, no artifact, and the tool-routing question is already represented.
  **Deferred.**

### Tail (2026-10-20 / 2026-10-21) records missed by the previous pass

These were found by the recall check and are *not* in `sources/daily-arxiv-2026-10-24.md`. They were
screened at title + abstract level and are recorded here so the next run does not start from title
alone. All are substantive robot/agent work with **no first-party artifact visible on the record**:

`2610.28109` FIT-MAC (multi-agent closed loop for robust VLA), `2610.28431` (steering world-action
models), `2610.28626` UniWALM, `2610.27618` JARVIS (visual manuals → assembly), `2610.28259` (GPT-6
for direct robot control), `2610.29329` (streaming executable memory for robot control),
`2610.30043` TARSphere, `2610.27698` VLA-Retain, `2610.27725` World2HOSI, `2610.28074` Slot-JEPA,
`2610.27946`, `2610.28594`, `2610.27794` (multi-view latent action pretraining), `2610.29161` SimPT,
`2610.29702` CausGround, `2610.30105`, `2610.29646`, `2610.29565`, `2610.28523`, `2610.28184`,
`2610.28925`, `2610.27905`, `2610.28397` MAS-Guard, `2610.30445`, `2610.29346`, `2610.27496`,
`2610.27616` EchoNav, `2610.28814` T-WLA, `2610.28121`.

Two of these deserve an explicit note because they look like inclusions and were not:

- **HATCH: Hardware-Attested Task Chains** (`2610.27485`, IROS 2026 workshop, **2 pages**) — an
  openly specified, sensor-scored, unrehearsable "robot escape room" benchmark with a linked
  `shizacharania/hatch-benchmark` repository. Attractive, but the paper is a two-page proposal with
  one table; there is no evaluation in it to curate. **Watch** for the full version.
- **RoboMME-adjacent EventSync / RoboCasa-Shift neighbours** are handled above; the remaining tail
  records are policy, perception, or control contributions without a harness contract, an evaluation
  contract, or an artifact.

### Carried watch list — no change this pass

StageWAM (still v3), ReflexVLA/ReflexBench, DreamX-Phi (README-only), UniTexture (no code), PRISM
("Dataset soon"), GigaBrain-0.7/WBC (release unconfirmed), ForceU-VLA (README-only), LIBERO-VIFO,
Agent Lightning, VLCP, Hydra-0, BATON, CarryOn (2610.21453, "code in three weeks"), EMHO
(2610.08432), Self-Evolving Agent Harness (2610.18813, CC-BY-4.0 on a code repo), Skill2Real,
RoboBridge, Local-Recovery-RSI, AgenticTactileVLA, DiVeR, R²-WAM, UniIntervene++ — none appeared as a
new version or a new artifact in this interval.

## Validation performed

- **Markdown structure:** `README.md` is now **1,199 lines / 40 section headings**; the 10 new
  entries sit inside existing sections (`### Agentic Robot and VLA Harnesses`, `### Runtime Building
  Blocks`, `### Safety`, `### Observability and Replay`, `### Manipulation and VLA`,
  `### Simulation and World-Model Fidelity`, `## Surveys and Reading Lists`), and the five new
  `## Current Landscape` bullets were appended to the existing list rather than creating a heading.
- **Added lines:** 22 added lines checked for balanced `[`/`]` and `(`/`)` — none unbalanced; every
  `[text](url)` resolves to an absolute URL; every content line begins with `- `.
- **`git diff --check`:** clean.
- **Links:** all **30** distinct URLs added to the README were fetched — **30/30 HTTP 200**
  (10 arXiv abs pages, 3 project pages, 5 GitHub repositories, 4 Hugging Face pages, 2 site/doc
  pages, plus 3 pre-existing URLs re-cited in new prose).
- **Artifact claims checked against live metadata, not paper text (2026-10-24):** Apache-2.0 for
  `AstroUpLab/AstroBench` (32.9 MB, created 2026-10-18, pushed 2026-10-23, CI + 7 translated READMEs
  + `astrobench/`, `assets/`, `tests/`), `AsnGs/agenttriage-open-source` (24.5 MB, Apache-2.0,
  `agenttriage/`, `targets/`, `contracts/`, `schemas/`, `tests/`) and `Sphere-AI-Lab/RoboJudge`;
  MIT for `HaochengWang23/RoboCasa-Shift` (13.2 MB, created 2026-10-20) and
  `Physical2RSI/Awesome-Physical2RSI`; BSD-2-Clause for `ZhuoyangLiu2005/agentic-wam` (67 stars) and
  the `miniFranka/...` checkpoint; **no licence** for `jonajoy142/alpha-embodied-robustness` and
  `SashaDance/rgraph-eval` (both recorded as source-available/therefore not open).
- **Gating checked via the Hugging Face API:** `RoboJudge-Bench` (CC-BY-NC-4.0, 955 downloads),
  `RoboJudge-9B` (Apache-2.0, 62), `robocasa-shift` (CC-BY-4.0, 127), and the AW-0 checkpoint
  (BSD-2-Clause, 37) are all `gated: false`, `private: false`.
- **Literal-status checks:** the WholeBodyMem project page was read directly (draft banner,
  "values marked TBD", anonymous authors, "Paper under review"); the RGraph-Eval README was read
  directly ("will be added here"); the Alpha repository checklist was read directly (weeks 5–12
  unchecked, no LICENSE, current name `alpha-embodied-robustness`); the AgentTriage and AstroBench
  READMEs were read to confirm contents match what is claimed.
- **Duplicates:** all 10 included IDs and their titles grepped across `README.md`, `docs/`, and
  `sources/` before insertion; none present. `CARE`, `TEAR`, `SCOPE`, and "progress probe" name
  collisions were checked explicitly (see Screening).
- **Dates and categories:** each included ID's v1 date, latest version, and primary category were
  re-read from one `id_list` query (all ten are still `v1`), and cross-checked against the README
  text.
- **Cross-file consistency:** the README badge, the `## Current Landscape` "Last verified" line, this
  record, and the previous record all carry **2026-10-24**; this record's scope statement matches the
  maintenance log's state (last success 2026-10-24T00:24:29+02:00).

## Operational notes and blockers

- **Index lag (blocker for the interval, not workable around).** The export API has not indexed
  2026-10-23 or 2026-10-24 on either date axis as of 2026-10-24 10:19 UTC, while 2026-10-22 is
  fully present. **The next run must re-scan 2026-10-23 at minimum**; this run explicitly owes that
  date forward rather than skipping it.
- **No 429.** The HTTP 429 wall that the previous run hit after ~15 requests did not appear at 3–4 s
  spacing across 28 direct API requests, so no mirror/proxy fallback was needed.
- **`/tmp` is not persistent between tool calls on this host** — all harvests, PDFs, HTML captures,
  and URL lists live under `.scratch/arxiv-2026-10-24b/`, which stays untracked.
- **Repository hygiene.** `docs/reference-architecture.md` (modified by the user),
  `docs/ring-harness.png`, `handoff.md`, and `.scratch/` are pre-existing user state and were left
  untouched. Only `README.md` and this file are task-owned changes.
