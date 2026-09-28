# Daily arXiv scan — 2026-09-28

## Scope

- **Interval:** everything announced after the 2026-09-27 run's cutoff. That run
  (started ~07:56 UTC on **Sunday 2026-09-27**, committed 10:07 local) screened
  the announcement block whose OAI datestamp is **2026-09-25** — 902 unique base
  IDs, newest base ID `2609.30266` — and recorded that the 2026-09-27 window
  returned `noRecordsMatch`. This run started ~08:20 UTC on **Monday 2026-09-28**.
  **There are no missed dates:** no cron slot between the two runs was skipped,
  the 2026-09-27 window was re-checked and is still empty, and the new block is
  the one dated **2026-09-28**.
- **A new announcement block landed.** The OAI-PMH harvest for
  `from=2026-09-28&until=2026-09-28` returned records for all five categories:
  **1,150 memberships = 886 unique base IDs**, every one carrying datestamp
  **2026-09-28** (memberships: cs.AI 342, cs.LG 357, cs.CV 190, cs.CL 134,
  **cs.RO 127**). This is the Sunday-night US-Eastern batch that the 2026-09-27
  record predicted, arriving one day after the Saturday/Sunday weekend gap.
- **The 2026-09-27 window is still empty and the 2026-09-26 window is still
  empty.** The `prev` harvest (`from=2026-09-27&until=2026-09-27`) returned
  `noRecordsMatch` for all five categories (646 bytes per response, zero
  records), confirming the weekend gap was real rather than an indexing delay
  that has since resolved.
- **Window and delta.** The wide window `from=2026-09-25&until=2026-09-28`
  returns **2,354 memberships = 1,762 unique base IDs** across two datestamps:
  **876** at 2026-09-25 and **886** at 2026-09-28. Against the previous run's
  parsed full window (902 unique IDs): **860 IDs appeared, 0 disappeared, and 26
  records changed**. The arithmetic closes exactly — the 2026-09-25 datestamp set
  shrank from 902 to 876 because **26 of its records were re-announced with
  datestamp 2026-09-28** after being revised, which is the same 26 the field-wise
  delta reports. **Newest base ID moved from `2609.30266` to `2609.31620`**, so
  the ID frontier advanced by 1,354 and the block is not being held back.
- **An entity-normalisation control was applied** (`&#34;`/`&quot;` versus `"`)
  before comparing abstracts, titles, and comments, so no parser-induced false
  diff is reported. Record ordering is not used as a change signal.
- **Block composition.** 842 of the 886 new records carry a September 2026
  created date; the tail runs back through August (13), July (3), June (2),
  May (3), April (3), March (1), February (2), January (1), and then through 2025
  to 2018 (re-announced replacement versions). **263 records carry pre-`2609.`
  IDs** and **545 carry `2609.3xxxx` IDs**.
- **Withdrawals in this block: five.** `2505.09738` and `2506.22497` carry the
  arXiv admin note "withdrawn because it does not meet arXiv's research content
  quality standards"; `2605.05236` is withdrawn by the authors "for substantial
  revisions"; `2608.04408` is withdrawn by the authors "due to issues in
  experimental validation"; and **`2608.06994` is withdrawn at the authors'
  institution's request** pending institutional clearance — that last one
  matters because the repository references it, and the reference has been
  annotated (see "Revision edits" below). The three admin withdrawals recorded on
  2026-09-25 (`2608.06825`, `2608.24098`, `2608.25326`) remain in the window and
  remain out of scope.
- **Export-API cross-check returns nothing usable this window.** With the range
  syntax `submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` URL-encoded and
  called over **HTTPS**, all five categories return **0 entries for 2026-09-26,
  2026-09-27, and 2026-09-28**. The endpoint itself is healthy — the same query
  without a date filter returns `opensearch:totalResults` 59,068 for cs.RO — so
  this is the announcement-versus-indexing lag seen in previous runs, and the
  export API is used strictly as a boundary check. The OAI harvest is the
  authoritative source.
- **21 of the 886 new-block IDs already appear somewhere in the repository**
  (README, `docs/`, `sources/`). Because OAI datestamp 2026-09-28 means
  "announced in this block", those 21 are re-announcements of records curated
  earlier, and each was checked for a revision that changes its recorded status
  (see "Revision edits").
- **Watch-list re-check:** **none** of the thirteen carried watch IDs
  (`2609.24274`, `2609.24350`, `2609.24563`, `2609.22587`, `2609.21223`,
  `2609.21502`, `2609.22218`, `2609.22247`, `2609.26121`, `2609.26520`,
  `2609.25558`, `2609.29091`, `2609.29065`) is present in the 2026-09-28 block.
  The last two are present in the wide window (they belong to the 2026-09-25
  block and were already re-checked on 2026-09-27); neither paper links a
  first-party repository in its full text and no artifact was located again, so
  both stay on the watch list unchanged.

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`) over three windows: `2026-09-28…2026-09-28` (new
   block), `2026-09-27…2026-09-27` (late-batch/empty-window re-check), and
   `2026-09-25…2026-09-28` (previous block plus new block). Pages saved to
   `.scratch/arxiv-2026-09-28/oai-{new,prev,full}-*.xml`; every category returned
   a single complete page with no resumption token, and the `prev` window
   returned `noRecordsMatch` for all five.
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`oai-records-{new,full}.json`, `parse_oai.py`), then **delta against the
   previous run's parsed harvest** with field-wise before/after values
   (`delta.json`), plus the entity-normalisation control.
3. **Export-API boundary cross-check** over the 2026-09-26, 2026-09-27, and
   2026-09-28 `submittedDate` windows per category, over HTTPS, with a
   no-date-filter liveness control on the same endpoint.
4. **Screening passes with three distinct emphases** over the 886 new records:
   (a) a **harness-layer** sweep (harness, scaffold, agent loop, runtime,
   middleware, tool call, skill discovery, code-as-policy, execution trace,
   replay, rollback; 49 uncurated records scored ≥2 on harness or
   self-improvement terms); (b) a **robot/embodied** sweep (88 uncurated
   records; 127 new-block records carry cs.RO); and (c) a **VLA / robot
   foundation / world-action** sweep (42 uncurated records), with an
   **eval/safety** ranking (182 uncurated records) and an **artifact-signal**
   listing of every uncurated record naming a concrete release object, repository
   URL, "we release" sentence, or project page (148 records). `screen.py`;
   outputs in `sweep-{harness,robot,vla,eval,artifacts}.txt`.
5. **Dossiers and primary-source verification** for every shortlisted record:
   arXiv export-API version history and dates, the arXiv abstract page and
   full-text HTML, official project pages, the GitHub API (license, size,
   creation and push dates, default branch, top-level tree) and the Hugging Face
   Hub API (gating, license, last modified) — all on **2026-09-28**. Nothing was
   taken from a secondary source.
6. **Duplicate screening** of every candidate against `README.md`, `docs/*.md`,
   and `sources/*.md` by arXiv ID, project name, and repository URL before
   writing. No included ID was already present in `README.md`.
7. **Revision screening of the 21 already-referenced IDs** by fetching each
   arXiv abstract page's submission history and each full text, then checking
   whether a new version added an artifact, changed the evidence class, or
   altered a status the repository records.
8. **Watch-list re-check** by ID against the new block and the wide window (see
   Scope).

## Included (README updates)

Eleven new entries in five README sections, plus four revision edits. Artifact
status is reported literally; every repository, license, dataset, date, project
page, and tree claim below was verified live on **2026-09-28**.

### Harnesses and Development Platforms — General Harness Design and Self-Improvement

- **MoMHa** (`2609.30967`, cs.AI; v1 2026-09-25, NeurIPS 2026): makes the harness
  a **first-class multi-objective design surface** rather than a prompt to be
  tuned. **Meta-Harness** searches over three per-domain objectives — accuracy,
  behavioural safety, token cost — with an agentic proposer (Claude Code) given
  full filesystem access to prior harness source, execution traces, and scoring
  artifacts, so proposals are conditioned on what earlier candidates did. The
  load-bearing result is the optimizer's shape: a **single-phase joint-reward
  proposer** beats a two-phase "accuracy then tokens" ablation, scalar-only
  feedback, and an accuracy-only baseline. Reported over seventeen domains (seven
  synthetic capability suites, seven public benchmarks — HumanEval, MBPP, Spider,
  FEVER, MMLU-Pro, LawBench, NuminaMath — and three U-SafeBench-derived
  user-specific safety domains) with a 12-model fleet across four families: joint
  mean **0.482 vs 0.198–0.422** for ten baselines (7/10 columns) on the synthetic
  track and **0.461 vs 0.377** for the strongest baseline (DSPy) on the
  real-world track, transferring to unseen benchmarks on 8 of 12 target models,
  with the highest measured behavioural-safety composite (U-SafeBench 0.781) and
  95 fewer tokens per example than the two-phase alternative. **Artifact status
  literal:** the paper states "We will release all harness code, evaluation
  infrastructure, and cross-model logs" and answers "Yes" on the same promise in
  its reproducibility checklist, but **no repository link appears in the full
  text** — promised, not released. Digital agents, no robot experiment; results
  author-reported.

### Robot Agent Systems — Agentic Robot and VLA Harnesses

- **HuGo** (`2609.30594`, cs.RO; v1 2026-09-24): puts **code generation on top of
  a frozen whole-body controller** instead of training a new loco-manipulation
  policy per task. Given a task description plus observation and command
  specifications, an LLM writes executable closed-loop high-level policy code,
  and a refinement loop converts rollouts — numerical trajectories and selected
  video frames — into feedback and targeted code updates. Reported across five
  simulation tasks with two different low-level policies: substantial improvement
  over a high-level RL baseline and performance approaching a demonstration-based
  baseline without task-specific reward design or demonstration collection, plus
  zero-shot transfer of simulation-generated policies to hardware and further
  gains from applying the refinement loop to real-world rollouts. **Artifact
  status literal:** the project site is live, but the official repository
  `labicon/HuGo` contains a **108-byte README and an empty `docs/` directory** —
  a placeholder, not a release. Simulation plus hardware evidence; results
  author-reported.
- **Kintsugi-VLA** (`2609.31048`, cs.RO; v1 2026-09-25, applied to ICRA 2027):
  makes the simulator's ability to **restore exact state and branch** the harness
  primitive for recovery learning. It defines **interventional recoverability**
  as the probability that a fixed privileged expert completes the task after the
  simulator is restored to a given state, estimates it with adaptive Monte Carlo
  continuations and pointwise Wilson intervals, and shows the estimate's
  evolution along a failed trajectory is **non-monotonic** — which is why uniform
  sampling allocates budget wrongly. The estimates locate a **terminal
  low-recoverability frontier** that selects informative recovery starting states.
  Reported in a simulated Franka task: **34.6% and 38.4%** aggregate SmolVLA
  recovery success under difficulty- and frame-budget matching, 5.8 and 6.7
  points above uniform sampling in the same window, same ordering under disturbed
  execution and shifted clutter/physics, clean-task cost 76.8% → 74.7%. **No
  artifact located:** no repository link in the full text; simulation-only;
  results author-reported.

### Benchmarks and Evaluation — Agent and Embodied Reasoning

- **Agentick** (`2605.06869`, cs.AI; **v3** announced in this block, v1
  2026-05-07, NeurIPS 2026 Evaluations & Datasets): places RL agents that learn
  from scratch, foundation-model agents, and hybrid and human agents **on one
  evaluation surface**: 37 procedurally generated tasks across six capability
  categories, four difficulty levels, and five observation modalities behind a
  single **Gymnasium-compatible interface**, shipped with a Coding API, oracle
  reference policies, pre-built SFT datasets, a **composable agent harness**, and
  a live leaderboard. The reported evaluation spans 27 configurations and over
  90,000 episodes: no single paradigm dominates (GPT-5 mini leads overall at
  0.309 oracle-normalized score while PPO dominates planning and multi-agent
  tasks), the reasoning harness multiplies LLM performance by **3–10×**, and
  ASCII observations consistently outperform natural language. **Verified open:**
  the MIT-licensed `roger-creus/agentick` repository (7.7 MB, created 2026-02-12,
  pushed 2026-09-16) carries the benchmark package, docs, examples, leaderboard
  data, and tests; the project page and leaderboard are live.
- **SciHorizon-eLab** (`2609.30971`, cs.AI; v1 2026-09-25): makes benchmark
  construction itself the agentic pipeline — given a natural-language protocol,
  the compiler performs **semantic grounding, executable task synthesis, and
  multi-stage simulation-based certification** to emit a grounded environment, an
  executable manipulation program, and a step-level success specification, so
  tasks are generated with their verification conditions rather than annotated
  afterwards. The same pipeline regenerates expert demonstrations and execution
  traces, which is what makes the resulting **300-task certified benchmark**
  reproducible. Reported: the strongest policy reaches only **49.7% average
  success**, with further weaknesses in human–embodied-agent coordination.
  **Verified open:** the MIT-licensed `SciHorizon-elab/SciHorizon-elab`
  repository is live (created 2026-07-27, pushed 2026-09-25) with the
  `scihorizon_elab` package, `scripts`, `docs`, environment locks, and a
  third-party tree. Simulation evidence; results author-reported.

### Runtime, Safety, and Observability — Runtime Building Blocks

- **RoboMonitor** (`2609.30715`, cs.RO; v1 2026-09-25): attacks the annotation
  problem that keeps execution monitors out of deployment. It **pre-trains on 25
  hours of multi-camera trajectories across 12 manipulation tasks and two
  embodiments** with action-conditioned future-feature prediction, inverse
  dynamics, and masked-present prediction — self-supervised objectives needing no
  monitoring supervision — then transfers the encoders to a causal monitor and
  applies **temporal supervised fine-tuning** over every position in a window
  with within- and cross-window consistency objectives. At deployment it consumes
  only the task instruction and camera observations. Reported on a four-task
  benchmark: **93.1% mean phase accuracy and 85.9% macro recall from 52 labeled
  episodes** across two seeds, exceeding Qwen3-VL and Robometer under identical
  supervision and still exceeding both when they get 100 episodes; a Qwen3-VL
  ablation attributes the temporal objective to a fall in mean spurious phase
  switching from **15.23% to 4.95%**; closed-loop deployment completes **39/40
  simulated Toolbox Sorting and 35/40 real-world Reel Packing** trials with no
  false recovery triggers. **No artifact located.** Simulation plus real-robot
  evidence; results author-reported.
- **Hide-and-Seek** (`2605.30834`, cs.AI/cs.RO; **v2** announced in this block,
  v1 2026-05-29, NeurIPS 2026): takes the supervision a deployment actually has —
  one success/failure label per trajectory — and treats VLA failure detection as
  a **coarsely supervised** problem instead of propagating that label uniformly
  across every timestep. **Inter-trajectory and intra-trajectory contrastive
  objectives** localize failure-indicative actions and induce temporally
  structured failure signals with **no step-level annotation**, and conformal
  prediction gives the accuracy–timeliness trade-off an explicit calibration
  handle. Reported on LIBERO, VLABench, and a real-world platform across
  OpenVLA, π0, and π0.5. **Artifact status literal:** the project page is live
  with videos and states "Code and videos are available", but the official
  repository it links, `deeplearning-wisc/hide_and_seek`, contains a **single
  1.3 KB README reading "🚧 Code coming soon!"** — a placeholder, not open
  source. Simulation plus real-robot evidence; results author-reported.
- **Realizability Is Not Enough** (`2609.30460`, cs.RO/cs.LO; v1 2026-09-24, 88
  pages): draws the line between a specification that can be synthesized and a
  supervisor that can be deployed. The released pipeline generates
  capability-based GR(1) specifications for **ROS 2 FlexBE**, analyzes
  assumptions before synthesis, audits strategies, reduces states with a
  behavior-preservation proof, and emits executable state machines. Across four
  case studies and six comparisons, including hardware on two quadcopter
  platforms, **enumerated encoding usually synthesized faster, yet fewer
  propositions did not reliably predict smaller controllers or lower symbolic
  cost**, and only System-Goal without pending memory yielded executable
  controllers under both encodings across the grid, while Fair-Outcome could
  permit realizable cycles with no designer-intended completion. The auditor is
  sound and complete for four structural defect classes (protocol violations,
  deadlocks, bounded-failure violations, goal-unreachable traps) and is
  explicitly **not** a general liveness verifier. **Verified open:** the
  Apache-2.0 `CNURobotics/flexbe_synthesis` repository (2.3 MB, created
  2025-07-24, pushed 2026-09-16) and the Apache-2.0
  `CNURobotics/flexbe_synthesis_demo` repository (9.9 MB, pushed 2026-09-16) are
  live with the synthesis packages and the drone/pyrobosim/river-crossing demo
  and experiment trees; the paper states implementation, demonstration artifacts,
  run directions, and raw per-seed data are released with pinned tags and commit
  identifiers. Simulation plus hardware evidence; results author-reported.
- **EffectMatch / Beyond Approved Actions** (`2609.31301`, cs.AI/cs.SE; v1
  2026-09-25, submitted to IEEE TSE): locates the admission-control boundary one
  step later than existing safeguards — approving an action and recording its
  aftermath both stop short of checking the **persistent result** before
  execution continues, so an approved database update that also leaves an
  unapproved notification is accepted as success and propagated. EffectMatch
  collects persistent changes inside a controlled execution boundary and compares
  them against what the application approved, and that comparison governs commit
  and dependent execution. Reported on 206 public business tasks: all clean
  executions preserved and all tested incorrect commits prevented; six 20-run
  ablations expose the failure caused by each removed mechanism, and 80
  task-topology cases preserve truthful handoffs while blocking invalid
  continuation. **No artifact located:** the paper's links point to the
  third-party task suites it evaluates on (MCPMark, STATE-Bench, ToolSandbox).
  Digital agents, no robot experiment; results author-reported.

### Runtime, Safety, and Observability — Safety

- **AuthGuard-R** (`2609.31110`, cs.RO/cs.CR; v1 2026-09-25): separates two
  questions most robot defenses conflate — whether an action is **physically
  safe**, and whether it is **authorized by the approved mission**. An action can
  pass a safety gate and still redirect a delivery robot, substitute an approved
  object, extend an operating region, power an unnecessary sensor, or delay a
  mission; the paper names this **safety-compliant mission hijacking**, builds the
  adaptive attack that searches for such plans (MissionPAIR), and then the
  defense: **AuthGuard-R**, a deterministic authorization layer binding every
  executable action to a signed mission, robot identity, object and region scope,
  current state, time, and input provenance, running beside an independent safety
  gate as a **dual-gate architecture**. The formal development proves
  authorization soundness, mission non-escalation, replay resistance, robot
  binding, provenance separation, threshold-approval security, audit-log tamper
  evidence, and trace-level composition. Reported with Claude Haiku 4.5 and
  open-source Qwen2.5 7B: across **240 live attack trials** the planners followed
  an injected mission deviation in 109, and AuthGuard-R rejected all 109
  resulting unauthorized actions, with a separate eleven-attack protocol- and
  policy-level suite blocked completely. **No artifact located.** Planner-level
  rather than physical-hardware evidence, and the authors label the evaluation
  preliminary; results author-reported.
- **Containing Behavioral Cascades** (`2609.30523`, cs.RO; v1 2026-09-24): treats
  semantic manipulation of an LLM-driven fleet as a **containment** problem rather
  than a trust decision — once one robot has accepted a false world-state claim
  the corruption at that node is done, and what remains controllable is whether it
  becomes a fleet-wide behavioral cascade (replanning, path cost, congestion,
  apparent mission infeasibility). The verification module emits a structured
  **Verify–Adapt–Hold** plan in which selected robots inspect consequential
  regions, a limited subset provisionally adapts, and the rest retain their
  trusted plans, so verification is allocated as team-level planning instead of
  flipping every robot into distrust. Evaluated in a multi-robot transportation
  environment under injected false obstacle claims across impact levels and team
  sizes, measuring cascade containment, sum-of-costs, makespan, and coverage
  ratio. **Artifact status literal:** the additional-materials site is live but
  no code, model, or data repository is linked from the paper or the page.
  Simulation evidence; results author-reported.

## Revision edits to existing entries

Four line-level edits covering three records, each driven by a revision or
withdrawal detected in this block. No earlier verdict was reversed except where
the artifact status genuinely changed.

1. **RIFT** (`2608.11521`) — **v3 (2026-09-25) changed both the artifact and the
   evidence class.** The entry recorded "no real-robot experiment or official
   code link was located as of 2026-08-13"; v3 adds a **real-world evaluation**
   section and real-world execution sequences, and the MIT-licensed
   `ChushanZhang/RIFT` repository is now live (129 KB, created 2026-08-14, pushed
   2026-09-25) with the flow-matching training code, configs, experiments, and
   tests, joined by released checkpoints (`PoopBear/RIFT`, ungated, last modified
   2026-09-27) and LIBERO (CC-BY-4.0) and RoboTwin 2.0 (MIT) datasets on Hugging
   Face. GitHub reports the repository license as `NOASSERTION` while the LICENSE
   file is the MIT text, and that discrepancy is stated in the entry. The
   matching Current Landscape bullet, which said "current evidence is
   simulation-only", was corrected.
2. **SimpleMemVLA** (`2609.05533`) — **v2 (2026-09-25) links a different
   repository than the one curated.** The full text says "Code available at
   https://github.com/OpenBMB/SimpleMemVLA"; the table row pointed at
   `wadeKeith/SimpleMemVLA`, which is an **empty repository** (0 KB, no license,
   created 2026-09-21). The link was corrected to the MIT-licensed OpenBMB
   repository (53 MB, 77 stars, created 2026-08-29, pushed 2026-09-24).
3. **PILOT** (`2608.06994`) — the disambiguation note in the PILOT-in-the-Loop
   entry now records that `2608.06994` was **withdrawn at the authors'
   institution's request** in this block, so a reader following the
   cross-reference is not sent to a withdrawn preprint.
4. **Neither `2608.25395`** (withdrawn at v2, restored at v3 on 2026-09-24) nor
   any other already-referenced ID required a change: the remaining 17
   re-announced records carry revisions that do not alter an artifact status,
   evidence class, or claim the repository records (page counts, author lists,
   journal-ref formatting, LaTeX-to-plain-text abstract normalisation, and
   metadata-only category additions).

## Current Landscape additions

Four bullets added to `README.md`:

1. Harness design is being formalized as a multi-objective search problem, and
   the optimizer's shape is the finding (MoMHa's single-phase joint proposer
   over accuracy, safety, and tokens — with its harness code promised rather than
   published).
2. Monitoring and recovery are being grounded in measurement instead of
   heuristics (Kintsugi-VLA's measured per-state recoverability; RoboMonitor's
   self-supervised pretraining and Hide-and-Seek's trajectory-level labels as
   answers to the supervision problem rather than detector architecture).
3. The runtime contract is widening outward to mission authority and inward to
   persistent effects (AuthGuard-R's signed dual gate; EffectMatch's
   commit-vs-approval comparison; Containing Behavioral Cascades' fleet-level
   verification allocation).
4. Supervisor synthesis is being held to deployability, and benchmarks are being
   compiled rather than hand-built (Realizability Is Not Enough on proposition
   count not predicting deployability; SciHorizon-eLab's protocol-to-task
   compiler; Agentick's single Gymnasium interface for RL, LLM, and human
   agents).

## Rejected / watch list

### Screened this pass and rejected

- **Audit Before You Commit** (`2609.30608`, cs.RO) — the closest call of the
  pass. Separates two conditions a probe-then-commit robot must satisfy before an
  irreversible action — the belief must still cover the truth in the coordinate
  that decides the action, and the failure model must track realized failure —
  and audits them separately with ground truth on a deployed probe-then-commit
  pipeline, reporting that more taps sharpen the belief while the truth leaves its
  support on **16.9%** of simulated insertion episodes, that the failure score
  turns optimistic by 0.31, and that conformal calibration restores coverage but
  **not** the decision because confidently wrong instances still pass a
  confidence gate. Excluded for this pass because it is a **diagnostic audit of
  an existing pipeline rather than a reusable contract or released artifact** —
  the additional-materials page is live but no code, data, or model is linked —
  and the list already carries the admission-control line it belongs to
  (EffectMatch above, plus the calibration entries under "What to Measure").
  **Watch:** a released audit harness would make it a direct companion to
  EffectMatch.
- **Evolutionary Safety of Recursive Self-Improving AI** (`2609.31186`, cs.AI) —
  a 25-page taxonomy plus risk discovery and proposed evaluation for safety under
  persistent and recursive self-improvement, arguing safety should be studied as
  a property of the improving process (preserved, weakened, inherited, restored
  across updates) rather than of a single checkpoint. The project page is live and
  lists the five-domain taxonomy. **Artifacts are an awesome list and a promise:**
  `ChaunceyKung/awesome-evolutionary-safety` is a README-only resource list
  (33 KB, no license) and the YUVANE evaluation system is "coming soon" with its
  repository (`ChaunceyKung/yuvane`) returning **404** (verified 2026-09-28).
  Excluded as a perspective contribution whose evaluation system is not
  released; **watch** for the YUVANE release, which is what would make it an
  entry rather than a framing.
- **Skill Cascading Attacks** (`2609.30383`, cs.AI, NeurIPS 2026) — threat
  paradigm in which a malicious objective is distributed across multiple skills
  so each modification looks benign alone, plus **SkillCascade-Bench** built from
  real-world ClawHub skills covering seven objectives and three cascade patterns.
  The paper's benchmark claim is present tense ("We release SkillCascade-Bench")
  but **no first-party repository or dataset URL appears anywhere in the full
  text** — the only repository links are to third-party skill scanners it
  evaluates against (verified 2026-09-28). **Watch:** the skill-ecosystem attack
  surface is directly relevant and the list carries SkillGate and
  MaliciousSkillBench; the benchmark is the transferable object.
- **AgentXploit** (`2609.31318`, cs.CR/cs.AI) — two-role white-box pre-deployment
  auditing that separates repository-level attack-path discovery from runtime
  exploitation, with attacks required to act through the task-defined interface
  and be confirmed by an external verifier. The repository
  `lwd17/AgentXploit` is real (36.5 MB, `benchmarks/`, `src/`,
  `codex_baseline/`) but **asserts no license**, has one star, and has not been
  pushed since 2026-08-07 — source-available rather than open source. Excluded
  as a digital-agent red-teaming framework that overlaps the existing
  REDAgentBench / trace-integrity line without contributing a robot-facing
  contract. **Watch** for a license and a push.
- **Fast Plans, Faithful Actions** (`2609.30833`, cs.RO) — targets the
  planning-execution gap in hierarchical VLAs: the high-level planner's subgoals
  are not what the low-level executor actually has to realize. A crisp systems
  observation, but the full text links no first-party artifact (only the upstream
  openpi LIBERO README it builds on), so it is a method paper this pass.
  **Watch.**
- **Causeway** (`2609.30913`, cs.RO) — restores task accessibility for
  **instruction switching** in VLA policies, i.e. what happens when the goal
  changes mid-execution rather than at episode start. Relevant to the harness's
  goal-state layer; excluded this pass because no artifact is linked and the
  contribution is a policy-side training method. **Watch.**
- **Representation-Guided Generation and Integration of Executable Programs for
  Robot Manipulation** (`2609.31337`, cs.RO) — code-as-policy for manipulation
  with representation-guided program synthesis. Excluded: no artifact located,
  and the list's code-as-policy line already carries stronger entries with
  released interfaces (RACaP, RAPID, Code as Policies).
- **VLaRL** (`2609.30868`), **DualManip** (`2609.31112`, project page only),
  **Grounded Action Model** (`2609.23863`, v2; repository is README-only with the
  description "code coming soon"), **MA-WAM** (`2609.31281`; repository is a 232
  byte README reading "code release in preparation"), **Praxis** (`2609.30735`),
  **CognitiveReality** (`2609.31418`), **InternW0-Δ** (`2609.31394`, every
  release sentence is future tense — "We will open source training code and the
  model weights…"), **Towards VLA-Dreamer** (`2609.31313`), **The Linear
  Representation Hypothesis for VLA Models** (`2609.30996`) — VLA, world-action,
  and whole-body policy contributions screened and excluded this pass: each is a
  model or representation result rather than a runtime/interface contribution,
  and where a repository exists it is a placeholder or the release is promised
  rather than published. InternW0-Δ in particular **claims 20K+ hours of open
  data but releases none of its own** — the links are to third-party corpora.
- **Statistical Priors for Implicit Preferences** (`2606.05828`, v2, Findings of
  EMNLP 2026) — a "local preference harness" that decouples statistical
  preference learning from semantic intent parsing for personal-agent skill
  selection, with the newly curated ToolBench-60 benchmark "open-sourced and
  released under the MIT License". The code repository
  (`ZyGan1999/Personalized-Skill-Selection`) is live but **asserts no license**
  and has not been pushed since 2026-05-22. Excluded as a digital
  personal-agent method on an already-covered harness layer (skill selection)
  with no robot-facing contract.
- **Auditing Latent-Space Monitors for Autonomous Driving** (`2609.30557`) and
  the remainder of the driving/perception sweeps — screened and excluded under
  the standing generic-driving and perception-only scope rule, notwithstanding
  the monitoring-audit framing.
- **CALM/robot-method papers screened and rejected this pass** —
  `2609.31323` (blind grasp reflex), `2609.30543` (GraspTwin digital twin;
  method), `2609.23863` covered above, `2606.15550` (multi-robot trajectory
  diffusion), `2609.30959` (tactile co-training), `2609.30770` (NavGen data
  engine), `2609.30521` (aerial manipulation), `2609.31507` (SatNav UAV
  benchmark), `2609.30833` covered above, `2609.30735` covered above,
  `2609.30428` (underspecified-task clarification), `2609.30404` (one-shot
  imitation), `2609.30462` (DAgger noise injection), `2609.31606` (STL-guided
  policy gradient), `2609.30690` (UAV deployment RL), `2609.30461` (DGT-Map),
  `2609.31232` (Rust autopilot), `2609.30358` (TinyCVIO), and the remainder of
  the 88-record robot sweep — method, hardware, or representation papers without
  a reusable runtime, recovery, safety, or evaluation contract.
- **Artifact-promise exclusions, carried or repeated** — `2609.30715`
  RoboMonitor (no link), `2609.31048` Kintsugi-VLA (no link), `2609.31110`
  AuthGuard-R (no link), `2609.30967` MoMHa ("we will release"), `2609.31394`
  InternW0-Δ ("we will open source"), `2609.31281` MA-WAM ("in preparation"),
  `2609.23863` GAM ("code coming soon"), `2609.30594` HuGo (README-only),
  `2605.30834` Hide-and-Seek ("🚧 Code coming soon!"), and `2609.30608` Audit
  Before You Commit (materials page only). Where an entry was still included, the
  placeholder is stated in the entry itself rather than in this list. The
  artifact placeholders recorded on 2026-09-25 (Maithili/GAP's 1 KB repository,
  HEXIS's HTTP 401 anonymous link, the RoboRecover dataset "being prepared for
  Hugging Face", and HarnessPAI's "not yet publicly released" code) are
  unchanged.

### Carried watch list — unchanged this window

Carried forward without status change (none of the eleven original IDs appears in
this block; checked by ID): vla.simd (`2609.24274`), LIBERO-VPro (`2609.24350`),
ARSTAG (`2609.24563`), React When You Need To (`2609.22587`), SafeStage
(`2609.21223`), AWM-3DFM (`2609.21502`), Toollery (`2609.22218`), CHART
(`2609.22247`), DTOC (`2609.26121`), MATE (`2609.26520`), HABILIS Brain 0
(`2609.25558`). The two IDs watch-listed on 2026-09-26 (`2609.29091`
Passive→Active Exploration, `2609.29065` DA-GRD) are present in the wide window
only; re-checked again, records unchanged and still no first-party artifact, so
both are carried forward. The artifact placeholders recorded on 2026-09-25
(Maithili/GAP's 1 KB repository, HEXIS's HTTP 401 anonymous link, the RoboRecover
dataset "being prepared for Hugging Face", and HarnessPAI's "not yet publicly
released" code) are unchanged.

## Validation performed

- `git diff --check` clean (no whitespace errors, no conflict markers); zero
  conflict-marker lines in `README.md`.
- Markdown structure re-checked after every insertion: all **481** top-level
  `- [` bullets have balanced parentheses and brackets with zero imbalance,
  every bullet has an even number of `**` and backtick markers, and the section
  headings (17 `##`, 22 `###`) and the Contents list are unchanged.
- **Every link added or rewritten this run was fetched on 2026-09-28 and returns
  HTTP 200**: 14 arXiv abstract pages, the MIT `ChushanZhang/RIFT` repository,
  the `PoopBear/RIFT` checkpoint repository, the MIT `OpenBMB/SimpleMemVLA`
  repository, the MIT `SciHorizon-elab/SciHorizon-elab` repository, the two
  Apache-2.0 `CNURobotics/flexbe_*` repositories, the MIT
  `roger-creus/agentick` repository, the Agentick project page and leaderboard,
  the Hide-and-Seek project page, the HuGo project site, and the
  Containing-Behavioral-Cascades materials site. A wider sweep of all 265 URLs
  in the touched regions also returned 200/200.
- Repository licenses, sizes, creation and push dates, default branches, and
  top-level trees were read from the GitHub API; Hugging Face gating, license,
  downloads, and last-modified from the Hub API; project pages and full texts
  were fetched directly.
- Dates checked for cross-file consistency: the README badge, the
  "Last verified" line, and this record all carry **2026-09-28**.
- Categories checked against the arXiv record for every included entry.
- Cross-file consistency: each of the four Current Landscape bullets refers only
  to entries now present in the main list, and the provenance caveats stated in
  the README (MoMHa's promised release, HuGo's and Hide-and-Seek's placeholder
  repositories, Kintsugi-VLA's and RoboMonitor's missing artifact, EffectMatch's
  third-party-only links, RIFT's `NOASSERTION` license versus MIT text, and
  Agentick's v3 provenance) match this record.

## Operational notes and blockers

- `/tmp` is per-command in this sandbox, so harvest XML, parsed JSON, dossiers,
  full texts, project pages, and logs were kept under
  `.scratch/arxiv-2026-09-28/` (untracked, never staged).
- The arXiv export API returned **HTTP 429 (rate limited)** on the first batched
  metadata pass; the pass was retried one ID at a time with 5-second spacing and
  backoff and succeeded for **25 of 26** shortlisted IDs (`2609.30715` exhausted
  its retries). Metadata for that ID was taken from the OAI harvest and abstract
  page instead. No result in this record depends on the failed call.
- The export API's `submittedDate` windows return zero records for all three
  dates tested while the same endpoint returns 59,068 results for cs.RO without a
  date filter; it is used only as a boundary check and the OAI-PMH harvest is
  authoritative.
- `git fetch origin` initially failed with the known OpenSSH error
  (`Bad owner or permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf`);
  it succeeded with `GIT_SSH_COMMAND='ssh -F /dev/null'`, which leaves the user's
  key and known_hosts unaffected.
- No GitHub API rate limit, conflict, or other network blocker; the OAI-PMH
  endpoint, export API, GitHub API, Hugging Face Hub API, and all project pages
  were reachable.

## Commit

Task-owned files committed on `main`: `README.md`,
`sources/daily-arxiv-2026-09-28.md`. Unrelated user changes
(`docs/reference-architecture.md`, `docs/ring-harness.png`, `handoff.md`,
`.scratch/`) were left untouched and unstaged. No branches, no pull requests, no
force-push.
