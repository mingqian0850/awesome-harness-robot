# Daily arXiv scan — 2026-11-11 (the day that had been owed returns 379 records, 2026-11-09 more than doubles its first count to 564, 22 records advance to v2, and eighteen entries are added — including a verifier harness, a falsification gate, an RSI price, an empty repository and a 404)

## Scope

- **Interval owed.** The previous successful run committed at **2026-11-10** (`c18d067`) and produced record
  `sources/daily-arxiv-2026-11-10.md`. That record closed with an explicit debt:
  **"The next run must query 2026-11-10 and 2026-11-11 and must re-harvest 2026-11-09, 2026-11-08 and
  2026-11-07 fully, since all three were seen for the first time here and every first-seen day in this
  project's history has grown afterwards."**
- **This run** started **2026-11-11 09:27 UTC** (**10:27 CET**) on `main` at `c18d067`, with `origin/main`
  also at `c18d067`. The interval owed is therefore submission dates **2026-11-10 → 2026-11-11
  inclusive**, plus full re-harvests of **2026-11-07, 2026-11-08 and 2026-11-09**.
- **Headline: the debt was paid in full and the re-harvest rule earned its place three times over.**
  2026-11-10 — recorded as **0** by the previous run because Tuesday's batch had not yet been announced
  (announcement is Wednesday 01:00 UTC) — now returns **379** records. The three days the previous run
  saw for the very first time grew again exactly as predicted: **321 → 334** (+13), **264 → 287** (+23),
  and **423 → 564** (**+141**, the second-largest single-day late block this project has recorded, behind
  only 2026-11-04's +120 block within a day). Because the previous record's own rule said to keep
  re-harvesting, **2026-11-05 and 2026-11-06 were also recounted and had grown again** (546 → **547**,
  499 → **511**, +13 late arrivals) — a day that had *already been fully re-harvested once* was still
  moving.
- **2026-11-11 returns 0, and that zero is correct**: Wednesday's own batch announces Thursday 01:00 UTC.
  It is owed for tomorrow exactly as 2026-11-10 was owed for today.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`, one parenthesised OR-query per
  day: `(cat:cs.RO OR cat:cs.AI OR cat:cs.CL OR cat:cs.CV OR cat:cs.LG) AND submittedDate:[YYYYMMDD0000
  TO YYYYMMDD2359]` with brackets percent-encoded. **15 export-API requests**: **seven** day-count
  requests (2026-11-05, 11-06, 11-07, 11-08, 11-09, 11-10, 11-11), **six** full-day harvests at
  `max_results=2000&sortBy=submittedDate&sortOrder=ascending` (11-05, 11-06, 11-07, 11-08, 11-09, 11-10),
  and **two** descending frontier probes (`cs.RO`, `cs.AI`).
- **No request returned the API's internal-error entry this run.** The signature documented on 2026-11-10
  (`<entry>` id `https://arxiv.org/api/errors`, `totalResults` 1, HTTP 200) was checked for on every
  response via `grep -c 'api/errors'`; all fifteen were clean. The check is kept because a count that
  trusts `totalResults` alone would silently record a false zero.
- **Full-day harvests return exactly their counted totals**, so truncation is excluded: 2026-11-05
  **547**, 11-06 **511**, 11-07 **334** (0.82 MB), 11-08 **287** (0.70 MB), 11-09 **564** (1.39 MB),
  11-10 **379** (0.93 MB).
- **Differential diff against the previous harvest, as a parsed set difference over bare numeric ids.**
  The previous run's `day_20261105.xml`, `day_20261106.xml`, `day_20261107.xml`, `day_20261108.xml` and
  `day_20261109.xml` were re-parsed and compared over `title`, `published`, `updated` and `summary`.
  Result: **0 removed** on every day, which is the check that the id-key normalisation is right.
- **Frontier probe.** Descending `cs.RO` returns **`2611.14488`** at **2026-11-10T18:47:57Z** and
  descending `cs.AI` returns **`2611.14519`** at **2026-11-10T18:59:54Z**; the two agree that the index
  now reaches Tuesday evening, i.e. **past** the 2026-11-06 frontier (`2611.12885` / 2026-11-09T18:59:45Z)
  the previous run recorded.
- **Screening.** (a) all **556** new-record titles in ascending ID order; (b) **87** additional
  keyword-matched records pulled out of the 556 by a concept regex (harness, VLA, robot, agentic,
  embodied, manipulation, world model, skill discovery, runtime, permission, verifier, self-improvement,
  recovery, teleoperation, policy learning) and inspected; (c) **64** full abstracts read for records
  carrying a harness, agent-runtime, evaluation, permission, memory or robot-agent signal; (d) **17**
  full-text HTML renderings (`arxiv.org/html/<id>v1`, **all 17** returned article bodies) read for
  artifact claims, scope statements and the sentences the abstracts omit; (e) resolution of **every**
  claimed artifact at the source, including repository contents through the GitHub API rather than only
  the HTTP status code.

## What the index did

| query (one parenthesised OR-query per day) | 11-05 | 11-06 | 11-07 | 11-08 | 11-09 | 11-10 | 11-11 |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| count at 2026-11-11 09:30 UTC | **547** | **511** | **334** | **287** | **564** | **379** | **0** |
| recorded by the previous run | 546 | 499 | 321 | 264 | 423 | 0 | — |
| **new to this run** | **1** | **12** | **13** | **23** | **141** | **379** | — |

- **556 new records** are screened — **13** on 2026-11-07, **23** on 2026-11-08, **141** on 2026-11-09 and
  **379** on 2026-11-10 — with **22** of the already-seen records carrying a revised `<updated>` (below). The +141 on 2026-11-09 is the
  largest first-growth step this project has recorded for a day inside its own week.
- **Category coverage of the six harvested days.** 2026-11-07: `cs.AI` **153**, `cs.LG` **140**, `cs.CV`
  **90**, `cs.CL` **52**, `cs.RO` **35** (35 with `cs.RO` anywhere in the category list). 2026-11-08:
  `cs.LG` **138**, `cs.AI` **110**, `cs.CV` **69**, `cs.CL` **50**, `cs.RO` **39**. 2026-11-09: `cs.LG`
  **229**, `cs.AI` **224**, `cs.CV` **134**, `cs.CL` **91**, `cs.RO` **90**. 2026-11-10: `cs.LG` **153**,
  `cs.AI` **140**, `cs.CV` **102**, `cs.CL` **75**, `cs.RO` **48**. Across the four entirely-or-partly new
  days **212** records touch `cs.RO`.
- **`<published>` spread is ordinary on every day**, including the newly visible Tuesday: 2026-11-07
  `00:02:17Z → 23:51:27Z`, 11-08 `00:02:50Z → 23:40:37Z`, 11-09 `00:00:16Z → 23:56:03Z`, 11-10
  `00:00:44Z → 18:59:54Z`. The 11-10 cutoff in the evening is the normal batch boundary, not a partial
  index — the frontier probe reaches the same 18:59:54Z.
- **No new posting is a revision, again.** All 556 new records have `<updated>` equal to `<published>`,
  so the "meaningful revisions" half of the requirement is answered entirely by the already-screened
  days below.

## Revisions on the already-screened days (22)

- **22** records advanced `<updated>` to **2026-11-10**: **6** on the 2026-11-07 harvest, **8** on
  11-08, **8** on 11-09. **Every one is a version bump** (`v1→v2`) with the new `<updated>` landing on a
  later date, not a same-day whitespace artefact.
- **None is retitled**, so the retitle sweep that found four substantive changes on 2026-11-10's run
  finds nothing this time.
- **None is an entry in this list**, checked by grepping all 22 ids against `README.md`, `docs/*.md` and
  `sources/*.md`: no existing entry needs an edit, and there is no repeat of the BOXBOX withdrawal case.
  The nearest the batch comes to an entry is `2611.11089` (asynchronous control / action inference
  chunking) and `2611.11648` (multi-robot safe search), neither of which is curated here.
- **2026-11-05 and 2026-11-06 grew by 13 late arrivals between them** against a day the previous run had
  *already* fully re-harvested. All 13 are listed under exclusions; the only two with any robot or agent
  signal are `2611.12933` (included, below) and `2611.12949` (a conventional ROS 2 navigation stack).

## Inclusions (18)

Classification per the handoff: General Harness Methodology; Agentic Robot/VLA Harness; VLA; Robot
Foundation/World Model; Evaluation/Safety. Every number below is **author-reported**; no paper in this
window has an independent reproduction. Artifact status is stated literally.

### General harness methodology (8)

- **`2611.13144` — Teaching the Harness** (`cs.AI`, 2026-11-09). Turns skill transfer into an **admission
  test**. The student is an agent inside a harness that fixes tools, context and action budget; a
  **learner model** (empirical profile plus hypotheses about the student's limits) is probed
  diagnostically, and skills are admitted to the library only if they raise success on **matched
  development tasks**. Claude-Sonnet-5 teaching DeepSeek-V4-Flash: **86.46%** versus **81.04%**
  (profile-free teacher) and **75.00%** (untaught), in every run; under deliberately misleading profiles
  **none of 48** candidates is admitted; libraries keep up to **11.1** points after the teacher leaves.
- **`2611.13336` — Veritas** (`cs.AI`, 2026-11-09). A harness that **generates verifiers**, with three
  mechanisms general agent harnesses lack (evidence header, domain-rule library, authoring principles
  partly refined by a self-improvement loop). Beats **seven** other harnesses on all four
  execution-scored benchmarks and lifts Claude Code **0.24 → 0.59** on Terminal-Bench; **9 of 12**
  harness-benchmark pairs improve when its mechanisms are transferred.
- **`2611.13524` — The RSI Tax** (`cs.LG`, 2026-11-10). Adds **cost** to the self-improvement scoreboard:
  the RSI Tax is the token cost of improving, counted alongside execution on continual task streams
  scored before feedback is used. A **bounded-note** baseline matches or beats **Meta-Harness, EvoSkill
  and RRSI** at **65.6–71.0%** lower mean total tokens; verification is **74.8–93.6%** of the tax and
  **23.7–63.6%** of it buys artifacts later tasks never use.
- **`2611.13232` / `2611.13236` — W-ISA and NCP** (`cs.AI`, 2026-11-09). A transactional **runtimes**
  pair: W-ISA executes request-scoped weight-space intervention programs and restores parameters after
  completion *or interruption*; NCP adds telemetry, candidate synthesis and an **admission gate** over a
  versioned policy table. Headline result is negative — one projection cut token repetition **0.4133 →
  0.2191** and yet **all three** candidates were archived for producing zero newly correct exact answers.
  Fidelity check: identical greedy sequences at 32/128/512 tokens, max prompt-logit difference
  **0.000184**.
- **`2611.13254` — Dependable Long-Horizon LLM Agents** (`cs.AI`, 2026-11-09). An **episode record** for
  fault origin → propagation → detection → response → consequence, applied by hand to **95** failed runs
  of six multi-agent systems: faults at **median 0.18**, **37 of 88** before the human-labelled decisive
  step, **72%** with contrary evidence already present and **88%** of those with no agent acting on it.
  The harness-owned finding: **45%** of the hand-crafted Who&When failures end in a **provider
  content-filter crash** followed by a harness-emitted final answer no agent produced.
- **`2611.14326` — FGSE** (`cs.AI`, 2026-11-10). Admission for **memory**: each lesson is a hypothesis
  (precondition, behavior change, expected effect, verifier), applied only temporarily, then tested on
  target-transfer, falsification and archived-regression probes. **39.8** WebArena, **78.8** AppWorld,
  **0.724/0.487** Pass^1 on τ-Retail/τ-Airline; falsification failures **5.0%**, regression damage
  **1.3%**, harmful committed updates **3.6%**.
- **`2611.13615` — Synapse-Control** (`cs.CL`, 2026-11-10). A lightweight **runtime control plane** that
  shortlists advice with rules, scores it with a **1.7B** classifier instead of generating a diagnosis,
  encodes interface checks as micro-contracts, and throttles repeats through a Prompt Value Table.
  **270** successes across **1,308** attempts per method (**29.8%** over base), **4,987** diagnosis calls
  replaced, and **50.2% / 51.0%** less control time than the Sentry-style baseline.
- **`2611.13367` — No finite rulebook** (`cs.AI`, 2026-11-09, CC-BY-4.0). The limit case for the
  gatekeeping line: two complete rulebooks backed by finitely checkable evidence **cannot both exist**;
  prohibitions fail one-sidedly, confidentiality and responsiveness fail on both sides; what survives is
  a **checkable list containing everything that must be allowed and nothing that must be refused**.
  Simulation support on **2.2 million** random programs. **MIT repository with proofs and simulations.**

### Agentic robot / VLA harnesses (5)

- **`2611.13034` — RoboACE** (`cs.RO`, 2026-11-09). Co-evolves a **curriculum harness** and an
  **execution harness** around frozen models, with admitted challenges frozen as snapshots so revisions
  stay comparable. **74.0%** on LIBERO-PRO (**+15.0** over its own initial deployment, **+13.5** over
  direct VLA execution), best on all four LIBERO-family panels. **Simulation only**; the authors name
  physical robots as future work. **Advertised repository is empty.**
- **`2611.13039` — Robo-MLT** (`cs.RO`, 2026-11-09). Puts a **multiframe workflow monitor** and an
  **asynchronous runtime** in the harness so a completed skill is not mistaken for the right step.
  **82%** and **70%** on real blood-gas analysis and sample-tube sorting (**+30** and **+22** over π0.5)
  on an Agilex dual-arm Static-Aloha rig, with replanning and continuation after an operator removes the
  sample being handled.
- **`2611.13482` — ExRL** (`cs.RO`, 2026-11-10). Moves adaptation from **what to do** to **how long to
  do it**: a frozen base policy plus an adapter that predicts each chunk's execution length, trained with
  off-policy RL. Adapts π0.5, π0-FAST and Cosmos Policy in simulation and on real precise-manipulation,
  long-horizon and clothing-folding tasks with **tens of minutes of physical interaction**.
- **`2611.14488` — Is Your Robot's Memory Just a Clock?** (`cs.RO`, 2026-11-10). Names **phantom
  progress**: a null event (a pause, a wrist fidget) is counted as work because memory is indexed by
  time, cutting success **47 → 16 of 50** and **44 → 10 of 50**; rescaling timestamps alone reproduces
  the failure, removing the time encoding does not cure it, and **event-indexed memory recovers half or
  more of the loss without retraining**.
- **`2611.12933` — Auditable Human-Agent Teams for Autonomous X-ray Experimentation** (`cs.AI`,
  2026-11-06, a **late arrival**). A coding-agent team running **real synchrotron** X-ray experiments,
  with complete transcripts audited for **decision provenance**: **61–67%** of messages involve
  interpretation or judgment, methodological/scientific share rises **46% → 56–61%**, and **59–68%** of
  error corrections involve more than one participant.

### Evaluation (2)

- **`2611.13160` — How Much of Your VLA Is kNN** (`cs.RO`, 2026-11-09, CoRL 2026 workshop). Retrieval
  plus reused action chunks reaches **91.7%** on LIBERO with **no policy training**, shows the smallest
  drop under LIBERO-Plus, and fine-tuning mostly makes the backbone a better **retrieval key** across
  nine VLAs; introduces **Local Action Consistency** as an offline diagnostic. **Claimed repo is 404.**
- **`2611.13541` — Agent Benchmarks Stop Ranking** (`cs.AI`, 2026-11-10, CC-BY-4.0). Locates saturation
  in the **pass/fail bit**: too coarse (top-ten SWE-bench Verified agents differ by less than test
  errors) and capped at the reference (ALE-Bench top ten tie at the median human). Proposes a clipped
  log-ratio score with dated anchors; pass/fail-tied Terminal-Bench models differ **>10×** on what the
  tests discard.

### VLA (1)

- **`2611.14432` — One Policy for 100 Household Tasks** (`cs.RO`, 2026-11-10). The deliberately
  non-agentic control: one π0.5-based checkpoint for all 100 BEHAVIOR-1K tasks, **9th** overall and the
  only independent team in the top 10, Q **0.260 → 0.371** on the shared 50 and **0.392** overall, with
  the authors attributing the gain to **longer training**. Its sibling 2nd-place report says it uses
  **"neither recursive self-improvement (RSI) nor agentic harness methods"**.

### Safety (2)

- **`2611.14115` — LEASH / RoboRisk** (`cs.RO`, 2026-11-10). A five-category safety benchmark
  (**92** simulated + **13** physical tasks) that executes every task closed-loop, measuring harm rates
  **9.06–76.92%** across six frontier planners, plus a three-gate harness (task, action, commit) sharing
  one safety policy; the authors report reduced harm for all six models including **0% harm on the real
  robot for four planners**.
- **`2611.13960` — ActiveSecHarness** (`cs.CR`, 2026-11-10, CC-BY-4.0). Treats a coding harness as a
  **Living-off-the-Land** platform and answers with separation of intelligence and authority: HMAC
  capability tokens over four rings, a 2D trust × sensitivity taint lattice with a provenance ledger,
  Ring-0 guards, bytecode certificates. **ABR 1.000 / FPR 0.000** at **12.7 μs** on 252 fixtures against
  **98.3%** benign refusal at **142.3 ms** for an LLM judge; an oracle stateless policy misses **4/12**
  kill chains.

## Excluded and watch-listed

**Watch list (artifact claimed, not yet a release):**

- **`2611.13355` — DEPUTYBENCH** (`cs.SE`). Enterprise contextual-safety benchmark, 79 base items / 375
  instances, six models, and a real result (restricted execution more frequent with missing evidence than
  explicit non-permission, 10.3% vs 3.5%). **Code and data "will be publicly released at a later date"** —
  literal statement in the paper; nothing is downloadable, so it is not open source and not listed.
- **`2611.14320` — WikiTrace** (`cs.AI`). Evidence-linked memory that lets an agent check a lesson against
  its trajectory; Apache-2.0 `yukyeongleee/wikitrace` is live (4 stars, pushed 2026-11-11). Watch rather
  than include because the admission-gate claim is already carried by FGSE in this batch and the paper is
  under review (ACL Rolling Review, October 2026 cycle).
- **`2611.14257` — Elastic-WAM** (`cs.RO`) and **`2611.14365` — MetaWAM** (`cs.CV`). World-action-model
  architecture papers with a project page and a `showlab/MetaWAM` repository respectively; model
  contributions with no harness interface, so excluded by the standing model-versus-harness rule and
  watch-listed in case either ships an evaluation contract.

**Rejected with reasons:**

- **Model and architecture papers without a harness interface** (standing rule): `2611.14048` LaST0.5
  (world action model), `2611.14420` OnlyAct (action representation extraction), `2611.13592` few-shot
  residual adaptation of latent world models (real robot, 3 episodes, no artifact), `2611.13030` APS-VLA
  (token pruning, no artifact), `2611.13732` CIALLO (action codec), `2611.13685` Ego Guidance,
  `2611.14092` DexSeed, `2611.13819` game-enhanced embodied pretraining, `2611.13610` Ego2Robot survey.
- **`2611.12988` — Third Eye** (`cs.LG`). A careful persistence measurement for recursive self-improvement
  (immediate-selected utility near zero; continuation lowers it by 1.78 pp, 95% interval [0.68, 2.88]),
  but the updates it studies are **model weight updates**, not harness updates, so it does not meet this
  list's harness-versus-model boundary. No artifact.
- **`2611.12983` — CAFE** (AI2-THOR scene task formulation and execution). Simulation-only household
  cleaning with no artifact; overlaps the existing agentic-robot entries without adding a reusable
  interface.
- **`2611.13114` — ASPIRE** (agentic system identification). The agent selects identification methods for
  measured systems including a mobile robot, but the contribution is system identification rather than a
  robot execution harness, the error is evaluated on withholding test data (a good design) and no
  artifact is released.
- **`2611.13251` — Curie's Jarvis** (robotic chemistry from printed protocols; 5/5 and 4/5 real runs on a
  Franka, 160/160 label locates). Strong physical evidence, but the contribution is protocol reading and
  container identification — a perception/execution system, not a harness contract — and no code, data or
  model is released.
- **`2611.13227` — Latent Barrier Steering** (hierarchical CBF safety for flow-matching planners).
  Control-theoretic safety filtering with no harness, permission or evaluation contract and no artifact.
- **`2611.14019` — PASE** (program-aided patchable skill evolution) and **`2611.14493` — Comparative
  Analysis of Agent Evaluation Frameworks** (KDD 2026, three evaluation paradigms, F1 0.90 for trace-based
  judging). Both are close to the list's themes — skill maintenance and evaluation-harness stability — but
  each duplicates a mechanism already represented in this batch (skill admission by SAT/FGSE; benchmark
  measurement by `2611.13541`) and neither releases an artifact.
- **`2611.13818` — WAM is capable in BEHAVIOR-1K** (2nd place). Recorded inside the `2611.14432` entry as
  the explicit counterpoint ("neither RSI nor agentic harness methods"); not given its own entry because
  no code, weights or data are released.
- **`2611.13810` — Camera equalisation of an 11.5× illuminance drop** (SO-101, four policies, 20/40 vs
  19/40, 95% CI −23 to +19). A clean real-robot null result with a public Hugging Face dataset, but a rig
  and lighting study rather than a harness, safety or evaluation contract.
- **`2611.13003` — Branch-and-Commit**, **`2611.12953` — Agent Architecture Cost-Success Trade-offs**,
  **`2611.13055` — Portable Agent Format**, **`2611.13994` — BioXArena-Pro** (biomedical ML agents with
  code and data), **`2611.12958` — Embodied Science Agents** (position paper), **`2611.13289` — MRFoundry**
  (MRI agent loops), **`2611.13333` — Do AI Agents need Preventive Care?** (cohort-design proposal),
  **`2611.13015` — Ask2Decide**, **`2611.12950` — SetPrint**, **`2611.14010` — NumPrint**,
  **`2611.13613` — Megrez**, **`2611.13776` — DisasterAgent**, **`2611.13540` — AgenticConflictLens**,
  **`2611.13410` — SteerDistill**, **`2611.13412` — prefix-cached agent serving**: off-domain, digital-agent
  adjacent, model-level, or application papers that do not introduce a reusable agent/VLA harness,
  recovery, safety or evaluation contract.
- **Late arrivals on 2026-11-05/06 (13)**: twelve rejected here, with `2611.12933` included above. The rejected ones are `2611.12930` CAD decomposition RL, `2611.12932`
  LiDAR simulation, `2611.12934` EarthRecord satellite agents, `2611.12935` sparse-autoencoder topic
  monitoring, `2611.12936` LandslideFM, `2611.12937` psychometric tracing, `2611.12938` KG-QA prompt
  visualisation, `2611.12940` chemotherapy RL control, `2611.12941` AI governance position,
  `2611.12942` tabular churn modelling, `2611.12943` "what is a paper for", and `2611.12949` **Maze
  Runner** — a conventional ROS 2 navigation stack (A*, NMPC, CBF) whose open `alvgaona/maze-runner`
  repository does not change the fact that it introduces no agent harness, recovery or evaluation
  contract. `2611.12933` is the one inclusion from that block.
- **The remaining ~390 records** of the 556 are outside the topic: time-series and tabular foundation
  models, medical/remote-sensing imaging, speech/TTS, tokenisation, geometry processing, quantum and
  materials ML, benchmark-adjacent NLP, and agent-adjacent work with no runtime, permission, memory or
  evaluation interface.

## Artifact findings that changed an entry

- **An empty repository is a placeholder, not an open release.** `2611.13034` (RoboACE) links
  "[GitHub Repo] `DeLin1001/RoboACE`" in its title block; the repository reports size **0** and the API
  returns *"This repository is empty."* The entry says so.
- **A 404 claimed as a release is a 404.** `2611.13160` (kNN) states "our evaluation and source code are
  available at `https://github.com/sereact/lerobot_knn`"; the URL returns **404** and the API returns
  *Not Found*. The LAC implementation is therefore not obtainable, and the entry says so.
- **A licence statement without a location is not an artifact.** `2611.13232` (W-ISA) states "Code is
  distributed under the MIT License, copyright 2026 Ekrem Aksoy", but neither W-ISA nor its companion
  `2611.13236` links a repository, package or archive, and the only external page they cite
  (`rikiyat.github.io/blog/weight-space-programming/`) is an MIT CSAIL blog post about a **related**
  result by a different author list — not this code.
- **"Protocol, records and scripts are included" is not a link.** `2611.13254` (Dependable Long-Horizon
  Agents) says the materials are included, but the record and full text carry no supplementary, repository
  or dataset URL, so nothing was located.
- **A release that exists is recorded with its licence.** Verified open this run: **MIT**
  `Tomasao/Impossibility-Theorem` (proofs and simulations), **MIT** `bennidict23/memory-clock`,
  **Apache-2.0** `Robot-Learning-Collective/behavior-1k-solution-2026` plus a public Hugging Face
  checkpoint; **no licence declared** on `Frankluox/ExRL`, `zhuqinfeng1999/LEASH` and
  `CFN-softbio/VISION-CC`.

## Files modified

- `README.md` — badge to **2026-11-11**; Contents blurb rewritten for this batch; `## Current Landscape`
  verification date and **five** new bullets; **eighteen** new entries across six subsections.
- `sources/daily-arxiv-2026-11-11.md` — this record (new).
- Untouched by design: `docs/reference-architecture.md` (user modification), `docs/ring-harness.png`
  (untracked user file), `handoff.md` (untracked by design), `.scratch/` (working data, never staged).

## Validation performed

- **Markdown structure.** Heading inventory before and after is identical: **17** `##` sections and
  **22** `###` subsections; every new entry is a single top-level list item under the correct subsection;
  a script check confirms no heading is preceded by a non-blank line, no list item sits directly under a
  heading, and no line carries trailing whitespace.
- **`git diff --check`** reports no whitespace errors.
- **Link resolution.** All **30** URLs added by this run returned **HTTP 200**: 20 `arxiv.org/abs/` links
  and 10 non-arXiv links (`exrl-project.github.io`, `zhuqinfeng1999.github.io/LEASH`,
  `robot-learning-collective.github.io`, the four GitHub repositories, the Hugging Face checkpoint, and
  the Impossibility-Theorem repository). The two dead links named in entries
  (`sereact/lerobot_knn`, and `DeLin1001/RoboACE` as an *empty* repository) are written as inline code,
  not as Markdown links, because they must never be clicked as if they were releases.
- **Duplicate check.** All 19 candidate ids were grepped against `README.md`, `docs/*.md`,
  `sources/*.md` and `CONTRIBUTING.md` **before** insertion; none was present. After insertion each id
  appears once in its entry, plus once in a landscape bullet where cited, plus once in the Contents blurb
  where cited.
- **Dates and categories.** Every included entry names its arXiv ID and its `<published>` date falls on
  2026-11-06 (late arrival), 2026-11-09 or 2026-11-10 — never 2026-11-11, for which no record exists.
  Category placement follows the contribution rather than the arXiv primary: `2611.13144`, `2611.13336`,
  `2611.13524`, `2611.13232`, `2611.13236`, `2611.13254`, `2611.14326`, `2611.13615`, `2611.13367` under
  *General Harness Design and Self-Improvement*; `2611.13034`, `2611.13039`, `2611.13482`, `2611.14488`,
  `2611.12933` under *Agentic Robot and VLA Harnesses*; `2611.13541` under *Unified Evaluation*;
  `2611.13160` under *Benchmarks and Evaluation → Manipulation and VLA*; `2611.14432` under
  *Vision-Language-Action Models → Open and Reproducible*; `2611.14115`, `2611.13960` under *Safety*.
- **Cross-file consistency.** The README badge, the `## Current Landscape` `Last verified:` line, and the
  Contents blurb all read **2026-11-11**, matching this record's filename. `docs/landscape.md` keeps its
  earlier verification date as a historical note rather than a maintenance surface.
- **Artifact-status discipline.** Each entry states exactly one literal status: **verified open with a
  licence** (MIT `Impossibility-Theorem`, MIT `memory-clock`, Apache-2.0 `behavior-1k-solution-2026`),
  **verified open without a licence** (`Frankluox/ExRL`, `zhuqinfeng1999/LEASH`, `CFN-softbio/VISION-CC`),
  **advertised but empty** (RoboACE), **claimed but 404** (kNN), **claimed but unlocatable** (W-ISA/NCP),
  **stated as included but unlinked** (Dependable Long-Horizon Agents), and **nothing located**
  (`2611.13144`, `2611.13336`, `2611.13524`, `2611.14326`, `2611.13615`, `2611.13541`, `2611.13039`,
  `2611.13960`). No entry describes a promise, a placeholder or a private link as open.
- **Author-versus-independent separation.** All numbers are marked author-reported. Evidence scope is
  stated per entry: digital-agent only for eight of the nine general-harness entries (all except the
  theory paper, which has simulations); simulation only for `2611.13034` and `2611.14432`; simulation
  plus real robots for `2611.13482` and `2611.14115`; a physical dual-arm rig for `2611.13039`; released
  policies evaluated in simulation for `2611.14488`; a physical synchrotron beamline for `2611.12933`.

## Operational notes and blockers

- **A day can be "owed" for exactly one announcement cycle, and paying it late still finds the records.**
  2026-11-10's zero was recorded on 2026-11-10 itself and returned **379** the next morning with an
  ordinary `<published>` spread. Combined with the 2026-11-10 correction (weekend days were unindexed, not
  empty), the rule now has two independent confirmations: **a zero is a statement about the index at query
  time**, and a zero-bearing day must be re-queried until its batch appears.
- **Re-harvesting once is not enough, and this run is the first evidence for that.** 2026-11-05 and
  2026-11-06 had already been fully re-harvested by the previous run, which recorded 546 and 499. Today
  they return **547** and **511**. The standing instruction to re-harvest the previous days therefore has
  to mean *every run*, not *the run after a day is first seen* — a day's count is still moving two days
  later, and the +12 block on 2026-11-06 sits above the frontier the earlier run had recorded.
- **The failure mode that was not present.** Fifteen API requests and zero internal-error entries this
  run, against three on 2026-11-10. The check is cheap (one `grep -c 'api/errors'` per response) and is
  now part of the standing method rather than a workaround for a bad day.
- **A repository can be advertised and be entirely empty.** `DeLin1001/RoboACE` reports size 0 and
  *"This repository is empty"*; the GitHub API `size` field is the fastest discriminator, and it is the
  check that separates "linked" from "released" without downloading anything.
- **`/tmp` remains non-durable across tool calls** even within a single run (a link list written to `/tmp`
  disappeared before the verification loop read it). `.scratch/arxiv-2026-11-11/` holds all 15 API
  responses, the four full-day harvests, the parse and diff scripts, the 17 full-text renderings and the
  GitHub/project-page fetches. It is **not staged and not committed**.
- **OpenSSH workaround still required.** `git ls-remote`, `git fetch` and `git push` fail with *"Bad owner
  or permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf"*; `GIT_SSH_COMMAND='ssh -F /dev/null'`
  restores access with the user's key and `known_hosts` unaffected. Before this run, `origin/main` and
  local `HEAD` were both `c18d067`.
- **Debt for the next run.** Query **2026-11-11 and 2026-11-12** and re-harvest **2026-11-10, 2026-11-09
  and 2026-11-08** fully. 2026-11-11 returns 0 because Wednesday's batch announces Thursday 01:00 UTC; the
  other three are named because every day in this project's history has grown after being recorded,
  including two days that had already been re-harvested once.
