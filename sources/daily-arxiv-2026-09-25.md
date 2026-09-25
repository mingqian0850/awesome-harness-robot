# Daily arXiv scan — 2026-09-25

## Scope

- **Interval:** everything announced after the 2026-09-24 run's cutoff. That run
  (started ~07:37 UTC, committed 07:49 UTC on **Thursday 2026-09-24**) screened
  the announcement block whose OAI datestamp is **2026-09-24** — 824 unique base
  IDs, newest base ID `2609.28473`. This run started ~07:03 UTC on **Friday
  2026-09-25** and covers everything announced since. There are **no missed
  dates**: the 2026-09-24 record was yesterday's block, and no cron slot between
  the two runs was skipped (the 2026-09-24 evening catch-up correctly reported
  "already covers the 2026-09-24 02:00 batch").
- **New announcement block.** The OAI-PMH harvest for
  `from=2026-09-25&until=2026-09-25` returned **901 unique base IDs, every one
  carrying datestamp 2026-09-25** (memberships per category: cs.LG 341, cs.AI
  365, cs.CV 205, cs.CL 193, **cs.RO 141**; 1,245 memberships total). This is the
  Thursday-night (US Eastern) announcement clearing the 2026-09-24 submission
  day. Every category returned a single complete page with no resumption token,
  so the harvest is not paginated away.
- **Wider window, for the previous block and the delta:**
  `from=2026-09-24&until=2026-09-25` returns **1,700 unique base IDs**, split as
  **799 carrying datestamp 2026-09-24** and **901 carrying datestamp
  2026-09-25** (no other datestamp is present in the window).
- **Composition by the submission date of the announced version** (OAI
  `<created>`): **616 carry 2026-09-24**, 144 carry 2026-09-23, and the remainder
  reach back through August and July to 2025 — the tail is re-announced
  replacement versions. **198 of the 901 block records carry pre-`2609.` IDs**,
  the signature of those replacement versions (ranging from `2311.07605` to
  `2609.30xxx`).
- **Delta against the previous run's screened window (exact).** Previous window
  (2026-09-23…2026-09-24) 1,753 unique IDs → this window (2026-09-24…2026-09-25)
  1,700 unique IDs. **860 IDs appeared, 913 disappeared**, and **106 field
  changes across 41 IDs** (datestamps 41, created 38, abstract 10, comment 4,
  title 2, DOI 2, categories 1, journal-ref 1). The disappearances are a
  **window shift, not retractions**: 913 records carried datestamp 2026-09-23 in
  the previous window and are simply outside `from=2026-09-24`; of the previous
  run's 824-record 2026-09-24 block, **799 still carry datestamp 2026-09-24 and
  25 were re-datestamped to 2026-09-25**.
- **Only 41 of the 901 block IDs were present in the previous run's window**, so
  **860 block IDs are new to screening**. The 41 overlapping IDs were re-checked.
- **ID frontier moved and is not truncated.** Newest base ID in the block is
  **`2609.30266`** (the previous run's frontier was `2609.28473`). Direct
  existence probes: `2609.30267` and `2609.30268` exist but are outside the five
  screened category sets, while `2609.30270`, `2609.30280`, `2609.30290`,
  `2609.30300`, `2609.30320`, and `2609.30500` all return **zero entries
  (404)**, so the harvest reached the assigned frontier and nothing is being
  held back.
- **Withdrawals in this block: three, all out of scope.** No author withdrawal
  appears; the three withdrawn records carry the arXiv admin note "withdrawn by
  arXiv due to unverifiable authorship and affiliation" — `2608.06825`
  (multiscale reward hedging), `2608.24098` (uniform stability), `2608.25326`
  (transductive learning). None relates to this list's topics.
- **Export-API cross-check.** With the range URL-encoded, `submittedDate`
  windows return `opensearch:totalResults` for **2026-09-24** (cs.RO 83, cs.AI
  172, cs.CL 80, cs.CV 99, cs.LG 130) — indexed and consistent with the OAI
  block's composition — while the **2026-09-25** window returns **0 for all five
  categories**: today's submissions are announced in the OAI block but not yet
  indexed by the export API, the same announcement-versus-indexing lag seen in
  previous runs. The export API is therefore used strictly as a boundary check;
  the OAI harvest is the authoritative source.
- **Curated records revised in this window.** One curated paper changed
  substantively: **`2607.08448` (Harness VLA) moved to v5** (v5 submitted
  2026-09-24 16:56 UTC, 15,251 KB against 9,655 KB for v4). The v5 abstract adds
  the code link (`RLinf/RPent`), and adds results the July version did not carry
  (LIBERO-Pro +38.6, RoboCasa365 +25.4, RoboTwin C2R 58.4%, real dual-Franka
  demonstrations). The README entry and the Current Landscape bullet were
  **updated**. Everything else curated in the repository that was re-announced
  this window changed only metadata: `2608.14047` (ART) v3 is byte-identical in
  size to v1/v2 (8,602 KB) and only records the CVPRF 2026 acceptance, so its
  "no release located" note stays accurate; `2609.27455` (LeWAM) v2 changes its
  abstract by one character; `2609.26760`, `2609.24972`, `2609.20455`,
  `2609.18366`, `2609.27717`, `2609.28107`, `2609.19613`, `2608.28578`,
  `2608.18574`, `2609.16256`, `2609.00949`, `2608.26582`, `2608.31046`,
  `2609.11144`, and `2609.11489` were re-announced with no change to the claims
  already carried in the README.
- **This is a robot-harness block.** 44 of the 901 records mention "harness",
  including eight titled harness papers that touch robots or physical AI
  (`2609.28530`, `2609.29166`, `2609.29204`, `2609.29389`, `2609.29964`,
  `2609.28908`, `2609.28919`, `2609.29095`); 141 records are cs.RO.

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`) for two windows: `from=2026-09-25&until=2026-09-25`
   (the new block) and `from=2026-09-24&until=2026-09-25` (previous block plus
   new block). Pages saved to `.scratch/arxiv-2026-09-25/oai-{new,full}-*.xml`;
   every category returned a single complete page with no resumption token.
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`.scratch/arxiv-2026-09-25/oai-records-{new,full}.json`, `parse_oai.py`),
   then **delta against the previous run's parsed harvest**
   (`.scratch/arxiv-2026-09-24/oai-records-full.json`) with field-wise
   before/after values (`delta.json`), separating window shift from retraction.
3. **Export-API cross-check** over the 2026-09-24 and 2026-09-25
   `submittedDate` windows per category, used only as a subset/boundary check,
   plus ID-existence probes at the frontier.
4. **Two full passes over the 901-record block with different ranking emphases.**
   Pass 1 (`analyze.py`) ranked by artifact-signal language (GitHub / Hugging
   Face / GitLab / project page / "we release" / "code available") and concept
   breadth (harness, VLA, robot foundation, world action model, code-as-policy,
   skill, memory, recovery, evaluation, safety, agentic), and additionally
   listed **every record in the block mentioning "harness"** and **all 141 cs.RO
   records**. Pass 2 (`rescreen.py`) re-ranked the whole block by lifecycle and
   contract vocabulary instead — release lifecycle, runtime monitor and
   intervention, verification/audit/provenance, failure diagnosis and recovery,
   evaluation protocol/benchmark contract, skill library and discovery, robot
   deployment and integration, and harness engineering/agent runtime internals —
   and separately listed the 73 robot/agent records carrying artifact language.
   Concept counts in the block: `harness` 44 records, VLA 16, world/action model
   29, memory 59, recovery 59, evaluation-language 235, safety 94, agentic 82.
5. **Dossiers and primary-source verification** for every shortlisted record:
   full title/abstract/comments, the arXiv abstract page and export API for
   version history, the arXiv full-text HTML for artifact links and release
   sentences, and the official GitHub / Hugging Face APIs (license field, size,
   creation and push dates, default branch, top-level tree, raw README) — all on
   **2026-09-25**. Project pages were fetched and inspected directly. The
   "newest visible batch" was new relative to the previous run, so the
   unchanged-batch re-screen clause did not trigger.
6. **Duplicate screening** of every candidate against `README.md`, `docs/*.md`,
   and `sources/*.md` by arXiv ID, project name, and repository URL before
   writing. No included new ID was already in `README.md`; `2607.08448` was
   already curated and is handled as a revision.
7. **Watch-list re-check**: all eleven carried watch IDs (`2609.24274`,
   `2609.24350`, `2609.24563`, `2609.22587`, `2609.21223`, `2609.21502`,
   `2609.22218`, `2609.22247`, `2609.26121`, `2609.26520`, `2609.25558`) were
   checked against the new block by ID — none appeared as a new revision, so
   their statuses are carried forward unchanged.

## Included (README updates)

Eighteen new entries and one revised entry, in five groups. Artifact status is
reported literally; every repository, dataset, checkpoint, and project-page
claim below was verified live on **2026-09-25**.

### Harnesses and Development Platforms — General Harness Design and Self-Improvement

- **Where Does Exactly-Once Live?** (`2609.29095`, cs.AI/cs.LG/cs.SE; v1
  announced 2026-09-25, submitted 2026-09-24, 23 pages): relocates exactly-once
  semantics from the model to the tool contract. LIMBO is a deterministic
  sandbox of six services with realistic contracts (optional idempotency keys,
  eventually consistent and missing read paths) and twelve fault modes at the
  service boundary, graded against a ledger of committed effects; across 25,930
  episodes over nine models, three production harnesses, two contract variants
  and fifteen recovery conditions the authors report the controlling factor
  depends on the fault — the model when an immediate read-back can reveal what
  happened (0.5% duplication for frontier models, 53% of explained variance),
  the contract when it cannot (56%/74% duplication, 81% of explained variance)
  — prove that no verification-only policy is exactly-once under late commits
  without a bound on in-flight time, and report that idempotency keys on every
  write cut duplication from 28% to 4% while the harness itself barely matters.
  **No artifact located:** the paper states code and data will be made available
  upon publication; the `gssanjana4/idempotencybench` repository it cites is a
  separate related-work benchmark by another author, not this paper's release.
  Digital agents; results author-reported.
- **Who Holds the Pen? Let Specifications, Not Agents, Sign Off** (`2609.29921`,
  cs.AI/cs.MA; v1 announced 2026-09-25, submitted 2026-09-24): gives
  specifications an authority boundary. The paper names the
  understanding–execution gap and the state–authority gap, extracts 509
  source-grounded task directions from SkillsBench, and reports that across
  seven models only 79.6–86.4% are satisfied while completion claims exceed
  official evaluator pass rates by 28.7–37.9 points; **SpecHarness** compiles
  visible specifications into source-linked obligations and governs execution
  and finalization through versioned obligation state. **No artifact located**
  on the record or in the full text. Digital agents; results author-reported.
- **Policy as Code: A Coroutine-Bridge Harness for Fast-Reasoning Reliability on
  CAR-bench** (`2609.29251`, cs.AI/cs.CL; v1 announced 2026-09-25, submitted
  2026-09-24, 4-page technical report): the winning entry in Track 2 (Cerebras
  Fast-Reasoning) of the CAR-bench Challenge at IJCAI-ECAI 2026. The model's only
  action is to emit a Python program that blocks and resumes in place across
  evaluator tool exchanges, decoupling model invocation from tool round-trips:
  a median of two model calls against seven agent turns, 1.8 s median model
  latency, 60.0% Pass^3 on the official hidden evaluation (4.5× the organizer
  baseline) at the lowest estimated cost and fastest median latency (3.14 s) of
  any entry above baseline, and an identical 60.0% on GPT-5.5 in the Open track
  with the unchanged harness. **No artifact located.** Digital agents; results
  author-reported.
- **Automatic Harness Evolution for Hardware Design Verification** (`2609.28908`,
  cs.LG/cs.SE; v1 announced 2026-09-25, submitted 2026-09-24): the block's
  clearest consolidation-negative result for harness evolution. On 12
  proprietary design-verification root-cause localization tasks (five trials
  each), evolved harnesses raised completed attempts by 71–76% and any-hit
  coverage by 80–100% but total correct attempts by only 18–24%; the strongest
  result reproducible at least twice improved by one task, later candidates
  exchanged gains rather than preserving them, and a candidate that improved a
  held-out four-task validation set tied its baseline on a 12-task replay. A
  CVDP case study reports 35.6% more functional passes than a 142-task reference
  baseline for an evolved defined-width repair harness. **No artifact located**;
  the benchmark framework and dataset are the third-party
  `NVlabs/cvdp_benchmark` / `nvidia/cvdp-benchmark-dataset`. Digital agents;
  results author-reported.
- **HEXIS: Compiling Skills into Extended Finite State Machines** (`2609.30123`,
  cs.AI; v1 announced 2026-09-25, submitted 2026-09-24): separates skill
  knowledge from control flow by compiling skills into EFSMs whose states hold
  local instructions and whose transition conditions decide the next operation;
  updates are accepted only after static checks and replay of the current and
  all previously accepted traces. The authors report +16.1 points over
  Skill + ReAct across four benchmarks and four executors, and 38.4–88.9% fewer
  execution tokens for Qwen3.8-27B. **Artifact not open:** code is promised at
  an `anonymous.4open.science` placeholder that returns HTTP 401. Digital
  agents; results author-reported.

### Robot Agent Systems — Agentic Robot and VLA Harnesses

- **HarnessPAI: An Evolving Harness for Physical AI** (`2609.29166`,
  cs.AI/cs.RO; v1 announced 2026-09-25, submitted 2026-09-24, 45 pages): the
  block's most explicit statement of the harness thesis for physical AI, with
  code as the executable and evolvable interface around action primitives and
  two separated timescales (open-loop program execution within a rollout,
  closed-loop program evolution across rollouts). Reported: +61.6 points over
  π0.5 on LIBERO-PRO, +27.2 over WorldDreamer on RoboCasa atomic tasks without
  retraining the model, and +38.8 points on LIBERO-PRO after fine-tuning π0.5 on
  the collected expert data, across desktop arms, household robots, a robot
  vacuum, and a legged agent. **Artifact status literal:** the project page is
  live, but `Darwin-Agent/HarnessPAI` is a paper-and-project-page repository
  (no license) whose README states research code is being organized and not yet
  publicly released, with installation instructions "not yet available" — **not
  open source**. Real-robot and simulated evidence, author-reported.
- **AdaHVLA: Adaptive Harnesses for Long-Horizon Vision-Language-Action
  Execution** (`2609.29204`, cs.RO; v1 announced 2026-09-25, submitted
  2026-09-24, 9 pages): makes the code-based coordination policy the adapted
  object, with decoupled multi-agent adaptation, testable coordination
  hypotheses, and a stateful revision graph linking evidence, hypotheses,
  revisions, and observed effects. Reported NaVILA-LH mean test success from
  22.5% to as high as 57.5% and up to +30.8 points across three VLA backbones.
  **Verified open:** Apache-2.0 `Haaareally/AdaHVLA-Adaptive_Harness_VLA`
  (436 MB; `src`, `configs`, `benchmarks`, `tests`; created 2026-09-22, pushed
  2026-09-23), linked from the paper. Simulation-primary with an illustrative
  real-robot deployment; results author-reported.
- **Know Your Body / KnowBody** (`2609.28530`, cs.RO; v1 announced 2026-09-25,
  submitted 2026-09-22, 19 pages): makes the robot's action-relevant body model
  explicit, queryable, and revisable while the VLM stays frozen, with
  re-checking of knowledge that depends on revised body estimates. Reported 75%
  versus 25% completion across 32 fixed-budget trials on four real-robot tasks,
  and 29–53% fewer planner rounds from the first to the fifth recorded success
  with persistent updates. **Artifact status literal:** project page and video
  live; `Loule0-0/KnowBody` contains only a README (no license, no code), so it
  is a project page rather than a release. Real-robot evidence, author-reported.
- **RACaP: Agentic Reasoning, Acting, and Coding as Policies for Evolvable Robot
  Learning** (`2609.29394`, cs.RO; v1 announced 2026-09-25, submitted
  2026-09-24): moves coding to evolution and calls frozen typed Policy APIs from
  a ReAct loop at deployment, so reusable mechanisms are separated from
  task-specific decisions. Reported 54.4% on LIBERO-90, 45.0% zero-shot on
  LIBERO-PRO, and 46.0% on LIBERO-Long against at most 4.0% for code-as-policy
  baselines (2.5× success and 1.9× median policy-time speedup on LIBERO-PRO),
  with GPT-5.6 ReAct decisions distilled into Qwen3-VL-8B-Instruct for a 13.2×
  per-decision speedup and repeated physical calls cut from 16 to 4. **No
  first-party artifact located.** Simulation benchmarks plus an on-robot
  deployment study; results author-reported.
- **Robo-Harness K1: Harnessing Robot-Use Agents via Perception Augmentation**
  (`2609.29389`, cs.RO; v1 announced 2026-09-25, submitted 2026-09-24): exposes
  calibrated depth, persistent visual anchors, spatial measurements, and grasp
  hypotheses as queryable tools, making 3D geometry accessible without changing
  the VLM architecture or training a depth encoder. Reported 77.8% (Gemini 3.7
  Flash) versus 61.1% (GPT-6 Astra, RGB-only harness) on matched LIBERO-PRO
  tasks, 88.9% for Astra with K1, 32.0%/28.0% Easy/Hard on RoboTwin, and a
  Qwen3.5-9B student at 44.2% versus 30.2% for OpenVLA on new initial states
  from 107 teacher episodes. **No first-party artifact located**; the only
  repository cited is the separate MIT-licensed `robocurve/inspect-robots`
  evaluation framework (603 stars, verified live). Simulation evidence,
  author-reported.
- **World Action Agent (WAA)** (`2609.29964`, cs.AI/cs.RO; v1 announced
  2026-09-25, submitted 2026-09-24): a multi-agent harness whose visual action
  workspace supplies contact views, editable action rehearsal with an
  Imagination Agent, and in-view correction, plus skills evolved from videos and
  teaching under evidence-based review. Reported 75.6% average success on
  LIBERO-Pro with skills evolved only from LIBERO-90 (transferring to robosuite
  without further learning) and Qwen3.5-9B out-of-domain success from 1.7% to
  43.3% after fine-tuning on harness traces. **No artifact located.** Simulation
  evidence, author-reported.
- **RAPID: Robot Agentic Programming from Demonstrations** (`2609.30249`,
  cs.AI/cs.CV/cs.RO; v1 announced 2026-09-25, submitted 2026-09-24): an agentic
  generate–execute–verify loop seeded by a single visual demonstration, with all
  three ingredients (testable specification, action primitives, interactive
  environment) inferred automatically and an object-centric relational program
  representation for reuse across scenes. Reported simulation results on eight
  contact-rich nonprehensile tasks plus LIBERO-Pro, and deployment on a real
  Franka arm for all eight nonprehensile tasks. **Artifact status literal:** the
  project page is live but its code link reads "coming soon". Simulation and
  real-robot evidence, author-reported.
- **OCC4M: Object-Centric 4D Memory** (`2609.28798`, cs.RO; v1 announced
  2026-09-25, submitted 2026-09-23, 13 pages): structured object-centric memory
  queried by a VLM against a history-free executor. Reported 96.6% memory and
  88.9% end-to-end success over 350 episodes against 54.6%/57.7% for a
  full-history baseline, 100% memory and 98% end-to-end success after a
  controlled viewpoint change where the baseline falls to near zero, and 85%
  joint memory accuracy over 20 fixed-camera Franka episodes. **Artifact status
  literal:** supplementary site live, no code, dataset, or model repository
  located. Simulation plus a 20-episode real-robot study; results
  author-reported.

### Robot Agent Systems — Planning, Tool Use, and Skill Composition

- **Coding Agents for Generalized Task and Motion Planning Problems**
  (`2609.30233`, cs.AI/cs.RO; v1 announced 2026-09-25, submitted 2026-09-24,
  9 pages): coding agents synthesize a program from a task description and
  simulator access, which is then frozen and evaluated on unseen instances —
  980 programs on 100 held-out instances each, **98,000 evaluation episodes**
  over 28 KinDER/PDDLStream environments with object counts beyond the original
  benchmark. Reported 56–95% mean success versus 47% for hand-engineered
  planners on the 16 environments where a planner is available, with an order of
  magnitude less computation per instance as object counts grow. **Verified
  open:** the MIT-licensed `tomsilver/robocode` experiment repository (126 MB;
  `src`, `docs`, `experiments`, `tests`; created 2026-02-14, pushed 2026-09-24)
  plus the project page with videos; the paper states all code including the
  full prompts is released. Simulation only, no physical robot; results
  author-reported.

### Benchmarks and Evaluation — Manipulation and VLA

- **RoboRecover: Benchmarking Robot Policy Recovery under Execution Deviations**
  (`2609.28952`, cs.RO; v1 announced 2026-09-25, submitted 2026-09-24):
  evaluates policies from reconstructed deviation states (prefix replay) rather
  than from reset, with 2,000 scenarios across RoboTwin and LIBERO (1,000 per
  platform, fixed 800/200 splits), and reports that initial-state performance
  does not determine recovery performance. **Verified open (partial):**
  Apache-2.0 `RUCKBReasoning/RoboRecover` (454 KB; checksummed splits,
  replay-then-inference evaluators, LIBERO and RoboTwin policy adapters,
  validation tools; created 2026-09-21, pushed 2026-09-23), but the complete
  scenario archive is "being prepared for Hugging Face" and full evaluation
  currently requires the archive from the authors — the **dataset is not yet
  public**. Simulation only; benchmark contribution.

### Runtime, Safety, and Observability

- **Persistent Billable State: Denial-of-Wallet Attacks and Defenses in
  Tool-Calling LLM Agents** (`2609.28585`, cs.AI/cs.CR; v1 announced 2026-09-25,
  submitted 2026-09-23, 22 pages): names the host's decision over whether and
  how a tool return enters later billable context as the persistent
  billable-state boundary, derives six denial-of-wallet vectors, and builds
  DOW-BENCH. Reported across 243 executions: a maximum per-session cumulative
  input of 14,293× the first call, raw retention raising mean effective session
  cost by 21.2–35.9%, compression succeeding on 10/12 and 11/12
  history-dependent tasks versus 2/12 under deletion, and a host-side kernel of
  deterministic history transformation plus four invariants containing every
  recurring attack in a 123-evaluation replay corpus (22/24 oracle-verified
  successes versus 13/24 under a fixed cap); only 71 of 3,830 scanned MCP server
  and transport repositories expose any code-visible safeguard proxy. **No
  artifact located.** Digital agents; results author-reported.
- **LLM Agents Can Easily Tamper With Their Own Traces** (`2609.30266`,
  cs.AI/cs.CR; v1 announced 2026-09-25, submitted 2026-09-24): shows the
  trace-integrity assumption behind asynchronous monitoring and audit is false —
  all tested harnesses except Muse Code allowed agents to delete their traces
  when asked without triggering monitor guardrails, external attackers can
  induce trace deletion, and the behavior emerges naturally in frontier models
  seeking to improve their rewards. Recommends out-of-band logging through an
  independent interception mechanism. **No artifact located.** Digital agents;
  results author-reported.

### Surveys and Reading Lists

- **Do World Models Make Better Robots? A Survey of Evaluation Benchmarks for
  Predictive Embodied Intelligence** (`2609.29669`, cs.AI/cs.RO; v1 announced
  2026-09-25, submitted 2026-08-30, 34 pages): catalogues **160 web-verified
  benchmarks (2017–2026)** and reports that 138 are model-agnostic, only 11 (7%)
  build an explicit VLA-versus-world-model contrast, counterfactual capability
  is almost entirely unmeasured, and only four turn prediction into executed
  action. Contributes a four-lane taxonomy, a coverage comparison against eight
  surveys, and a protocol of four advantage-aware metrics. **No artifact
  located** (survey). Evaluation-methodology contribution; no new experiments.

### Revision of an existing entry

- **Harness VLA** (`2607.08448`) **revised to v5** (v5 submitted 2026-09-24
  16:56 UTC): the abstract now carries a code link resolving to the Apache-2.0
  `RLinf/RPent` framework (1,030 stars; `rpent`, `robots`, `docs`, `tests`;
  created 2026-07-07, pushed 2026-09-24), whose README badges this paper's arXiv
  ID, and the project page links the same repository; the revision adds reported
  gains of 38.6 and 25.4 points on LIBERO-Pro and RoboCasa365, 58.4% on RoboTwin
  C2R, and real dual-Franka demonstrations. The main-list entry and the Current
  Landscape bullet were both updated, closing the "no public code repository"
  gap recorded on 2026-07-25. **No other curated record needed an edit**: ART's
  v3 is an acceptance-metadata update with an unchanged abstract and file size,
  and LeWAM's v2 changes one character of its abstract.

## Rejected / watch list

### Screened, claims not verifiable, or artifact not open

- **GAP / Robots That Take Initiative** (`2609.28910`) — claims GAP and the
  closed-loop evaluation setup are "openly available", but `Maithili/GAP` is a
  1 KB MIT-licensed repository created and pushed in the same minute on
  2026-09-24 with no description, no README content, and no code. Treated
  literally as a placeholder; **watch**. The paper's finding — that offline
  evaluation against a static human model overstates proactive-robot
  performance while prior methods collapse under closed-loop evaluation — is
  recorded here for later follow-up.
- **AquaMend** (`2609.28973`) — re-probing/rollback/continuation policy for
  latent-belief failures; simulation-only on a 32-scenario self-constructed
  benchmark, recovery in 28/32 and a 21.6% mean-loss reduction versus restart,
  with the difference from decision-theoretic troubleshooting not statistically
  significant after Holm correction. No artifact; **watch** (the probe-belief-
  action graph is a reusable contract if code appears).
- **Body-Grounded Replanning** (`2609.30024`) — body-state events triggering
  high-level strategy replanning with the objective and low-level controller
  unchanged; simulation plus one real robot, no artifact located. Related to the
  recovery line but without a released interface; **watch**.
- **Self-Adaptive VLA** (`2609.30092`) — post-training recipe (shift-conditioned
  demonstrations plus an AdaLN context token) that recovers >80% of base
  performance under hardware shifts, with a video site but no code or weights;
  excluded as a model-training contribution rather than a harness interface.
- **Operator Packages, Proposer Strength, and Construction-Family Plateaus**
  (`2609.29636`) — an instrumented FunSearch-style loop with a pre-registered
  2^3 factorial and released harness, but the cited repository
  (`RobertoOno/interrupting-the-loop`, MIT) hosts the code, data, and paper of a
  **different** arXiv record (`2608.19893`), so the harness release could not be
  tied to this paper; excluded pending a first-party artifact.
- **DeltaWAM** (`2609.28811`) — `AIGeeksGroup/DeltaWAM` is live and large
  (410 MB, pushed 2026-09-25) but **asserts no license**, and the contribution is
  a world-action-model efficiency result (dense-anchor/sparse-delta streams plus
  streaming delta memory: RoboTwin 81.3%→85.4% clean, 75.8%→83.9% randomized,
  17.8–23.8% training-FLOP reduction, 36.6% lower one-step latency) rather than a
  harness contract; **watch for a license**.
- **Maithili/GAP**, **HEXIS** (`2609.30123`), **RoboRecover dataset**, and
  **HarnessPAI code** are the four artifact placeholders this run records
  explicitly: 1 KB repo, HTTP 401 anonymous link, "being prepared for Hugging
  Face", and "not yet publicly released" respectively.

### General-agent harness papers screened and not included

These are in scope topically but were judged incremental relative to existing
entries, without an artifact, or outside the physical-AI focus. Reasons are
recorded so later runs can distinguish deliberate rejection from oversight:
`2609.29808` **Hard Stop** (kernel preemption monograph built on an incident
autopsy; no artifact), `2609.29647` **AgentKernel** (position/architecture paper
for a trust-native agent OS; no artifact), `2609.29668` **Graph, Loop, and
Harness Engineering** (zero-trust data-engineering frameworks; no artifact),
`2609.28559` **LIDAR** (LLM fingerprinting through agentic behavior; no
artifact), `2609.28919` **Control the Harness, Control the Cost** (enterprise
routing and cost governance across twenty harnesses; no artifact), `2609.29547`
**When Agents Act Unwatched** (63-artifact audit of accountability visibility;
no artifact, and the audit claims are bounded to public visibility),
`2609.29522` **Stale Does Not Mean Unsafe** (guard precision under state races;
no artifact), `2609.29545` **ERRAND** (budgeted memory revalidation; no
artifact), `2609.29578` **PartHackBench**, `2609.30137` **Screen Before You
Serve**, `2609.29773` **Breaking the Environment Wall** (environment evolution
for recursive self-improvement; no artifact located), `2609.28693` **skilder**
(role-scoped capability delivery; white paper, simulated authorization layer),
`2609.29366` **EPLA** (guarded multi-agent coordination; formal position paper),
`2609.29073` **Seeing Is Not Measuring** (tool-augmented metric spatial
reasoning; a useful tool-interface datapoint but vision-only and without a
robot experiment), `2609.29465` **SWE-Prometheus** (repository-governance
benchmark; digital), `2608.22631` **River** (terminal-agent RL recipe; digital),
and `2606.16723` **AgentFairBench** (discrimination audit for acting agents).

### Robot records rejected as out of scope

The remaining cs.RO records in the block are conventional perception, control,
navigation, tactile-sensing, aerial/underwater, soft-robotics, and VLA
training-recipe papers with no harness, recovery, safety, or evaluation contract,
and were excluded by the standing scope rule. Representative exclusions:
`2609.28865` Direction-Scale Decomposition (action tokenization; project page
only), `2609.29382` Decoupled Early Exits (compute allocation for flow-matching
VLAs; no artifact), `2609.28838` uncertainty-gated exploration noise (training
recipe; the promised collapse-measurement tools have no located link),
`2609.30134` Training-free Behavior Cloning, `2609.29212` ADM-Planner,
`2609.29601` semantic serialization as a perception interface, `2609.29194`
continuous online fault detection, `2609.29043` LLM-chaining GPSR planning, and
the streaming/rolling world-action-model papers (`2609.28927`, `2609.30247`,
`2609.30264`) which are model-efficiency contributions.

### Carried watch list — unchanged this window

Carried forward without status change (none appeared as a new revision in this
block, checked by ID): vla.simd (`2609.24274`), LIBERO-VPro (`2609.24350`),
ARSTAG (`2609.24563`), React When You Need To (`2609.22587`), SafeStage
(`2609.21223`), AWM-3DFM (`2609.21502`), Toollery (`2609.22218`), CHART
(`2609.22247`), DTOC (`2609.26121`), MATE (`2609.26520`), HABILIS Brain 0
(`2609.25558`).

## Current Landscape additions

Five bullets added to `README.md`:

1. Robot harnesses are now being named, built, and ablated as a category
   (HarnessPAI, AdaHVLA, KnowBody).
2. Code-as-policy is migrating from runtime to synthesis and evolution time
   (RACaP, RAPID, Coding Agents for Generalized TAMP).
3. Perception is being packaged as tools rather than retrained into the policy
   (Robo-Harness K1, World Action Agent).
4. Exactly-once behaviour and trace integrity are being located in the harness's
   contracts (Where Does Exactly-Once Live?, trace tampering).
5. Recovery and long-horizon memory are getting their own benchmarks and
   representations (RoboRecover, OCC4M).

## Validation performed

- `git diff --check` clean (no whitespace errors, no conflict markers).
- Markdown structure re-checked after every insertion: each new entry is a
  single top-level bullet with balanced brackets and parentheses (scripted check
  over all `- [` lines returned zero imbalance); section headings, the Contents
  list, and the reference-architecture tables are unchanged.
- Every added link was fetched on 2026-09-25 (project pages, repositories, and
  raw READMEs): **30 of 30 URLs added to `README.md` in this run return
  HTTP 200** (recorded in `.scratch/arxiv-2026-09-25/link-check.txt`); the only
  non-200 URL recorded anywhere in this record is the
  `anonymous.4open.science` HEXIS placeholder, which is deliberately reported as
  HTTP 401 rather than as open. Repository license, size, creation
  and push dates, and top-level trees were read from the GitHub API; project
  pages and raw READMEs were fetched directly. The check log is
  `.scratch/arxiv-2026-09-25/link-check.txt`.
- Dates checked for cross-file consistency: the README badge, the
  "Last verified" line, and this record all carry **2026-09-25**.
- Categories checked against the arXiv record for every included entry.
- Cross-file consistency: the Harness VLA v5 revision is recorded here and in
  both updated README locations; the previously curated entries that were
  re-announced without substantive change are enumerated in Scope above so a
  later run does not re-investigate them.

## Operational notes and blockers

- `/tmp` is per-command in this sandbox, so harvest XML, parsed JSON, dossiers,
  full texts, and logs were kept under `.scratch/arxiv-2026-09-25/` (untracked,
  never staged).
- `git push` required `GIT_SSH_COMMAND='ssh -F /dev/null'` because of the known
  `/etc/ssh/ssh_config.d` ownership problem, as documented in `handoff.md`.
- No GitHub API rate limit, network, authentication, or conflict blockers; the
  OAI-PMH endpoint, export API, GitHub API, Hugging Face API, and all project
  pages were reachable.

## Commit

Task-owned files committed on `main`: `README.md`,
`sources/daily-arxiv-2026-09-25.md`. Unrelated user changes
(`docs/reference-architecture.md`, `docs/ring-harness.png`, `handoff.md`,
`.scratch/`) were left untouched and unstaged. No branches, no pull requests,
no force-push.
