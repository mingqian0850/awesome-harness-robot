# Daily arXiv scan — 2026-09-23

## Scope

- **Interval:** everything announced after the 2026-09-22 run's cutoff. That run
  (started ~17:08 UTC on Tuesday 2026-09-22) screened the announcement block
  whose OAI datestamp is **2026-09-22** — 1,736 unique base IDs, newest base ID
  `2609.25001`. This run started ~06:47 UTC on **Wednesday 2026-09-23** and
  covers everything announced since.
- **New announcement block.** The OAI-PMH harvest for
  `from=2026-09-23&until=2026-09-23` returned **951 unique base IDs, every one
  carrying datestamp 2026-09-23** (memberships per category: cs.AI 381, cs.LG
  341, cs.CV 215, cs.CL 175, **cs.RO 152**, plus cross-listed stat.ML 47,
  cs.CR 37, cs.SE 27, cs.SY 23, eess.SY 23, cs.HC 19 and others). This is the
  Tuesday-night (US Eastern) announcement clearing the 2026-09-22 submission
  day; it is smaller than the previous block because the previous block was the
  post-weekend catch-up.
- **Wider window, for the previous block and the delta:**
  `from=2026-09-22&until=2026-09-23` returns **2,631 unique base IDs**, split as
  **1,680 carrying datestamp 2026-09-22** and **951 carrying datestamp
  2026-09-23** (no other datestamp is present in the window).
- **Composition by the submission date of the announced version** (OAI
  `<created>`): **612 carry 2026-09-22**, 159 carry 2026-09-21, and the
  remainder reach back through August and July into 2022 — the tail is
  re-announced replacement versions. **233 of the 951 records carry pre-`2609.`
  IDs**, the signature of those replacement versions.
- **Delta against the previous run's screened window (exact).** Previous window
  (2026-09-21…2026-09-22) 2,427 unique IDs → this window (2026-09-22…2026-09-23)
  2,631 unique IDs. **887 IDs appeared, 683 disappeared**, and 168 records
  changed in at least one parsed field. The disappearances are a **window shift,
  not retractions**: 842 of the previous window's records carried datestamp
  2026-09-22 and are simply outside `from=2026-09-22`… ; of the previous run's
  1,736-record 2026-09-22 block, **1,680 still carry datestamp 2026-09-22 and 56
  were re-datestamped to 2026-09-23**. **Only 64 of the 951 block IDs were
  present in the previous run's window**, so **887 block IDs are new to
  screening**.
- **ID frontier moved and is not truncated.** Newest base ID in the block is
  **`2609.26796`** (the previous run's frontier was `2609.25001`). Direct
  existence probes: `2609.26797`, `2609.26800`, `2609.26850`, and `2609.27000`
  all return **HTTP 404**, so the harvest reached the assigned frontier and
  nothing is being held back.
- **Export-API cross-check.** With the range URL-encoded,
  `submittedDate` windows return `opensearch:totalResults` for **2026-09-21**
  (cs.RO 90, cs.AI 191, cs.CL 70, cs.CV 133, cs.LG 155) and **2026-09-22**
  (cs.RO 81, cs.AI 125, cs.CL 54, cs.CV 92, cs.LG 123) — the 2026-09-22 window,
  which returned 0 at the previous run, is now indexed and confirms that
  Tuesday's submissions are the ones arriving in this block. The **2026-09-23**
  window returns **0 for all five categories**: today's submissions are
  announced in the OAI block but not yet indexed by the export API, the same
  announcement-versus-indexing lag seen in previous runs. The export API is
  therefore used here strictly as a boundary check; the OAI harvest is the
  authoritative source.
- **Withdrawals in this block: five** — `2606.22886`, `2609.24815`,
  `2605.29612`, `2609.13969`, `2608.20065`. **One touches this repository's
  history:** `2609.24815` (**Uranus**) was watch-listed by the 2026-09-22 run
  for a 404 SDK repository, and is now withdrawn by the authors (incomplete
  manuscript; the authors also did not reach unanimous agreement on releasing
  the artifact). It appears only in `sources/daily-arxiv-2026-09-22.md`, **not**
  in `README.md`, so **no curated entry is affected**; its watch status is
  closed. The other four appear nowhere in `README.md`, `docs/`, or `sources/`.
  `2608.20065` was withdrawn because it is superseded by `2609.20800`, which
  likewise appears nowhere in the curated files.
- **64 block IDs were already present in the previous run's window** and were
  re-checked. 168 records changed at least one parsed field. **No record listed
  in `README.md` changed**: the changed IDs that are mentioned anywhere in the
  repository (`2609.16639`, `2609.20812`, `2609.22332`, `2609.24815`,
  `2609.24971`, `2609.24411`, `2608.30880`, `2609.22829`, `2609.24526`,
  `2609.24660`, `2609.24682`, `2609.21712`, `2609.24187`, `2609.23184`) all
  live only in daily records. The changes are abstract additions, comment
  updates, category corrections, or datestamp moves.

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`) for two windows: `from=2026-09-23&until=2026-09-23`
   (the new block) and `from=2026-09-22&until=2026-09-23` (previous block plus new
   block). Pages saved to `.scratch/arxiv-2026-09-23/oai-{new,full}-*.xml`; every
   category returned a single complete page with no resumption token, so the
   harvest is not paginated away.
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`.scratch/arxiv-2026-09-23/oai-records-{new,full}.json`, `parse_oai.py`).
3. **Delta against the previous run's parsed harvest**
   (`.scratch/arxiv-2026-09-22/oai-records-full.json`): set difference for
   appeared/disappeared IDs plus field-wise comparison for content change
   (`delta.json`), and a separate datestamp-movement analysis to separate window
   shift from retraction.
4. **Export-API cross-check** over the 2026-09-21, 2026-09-22, and 2026-09-23
   `submittedDate` windows per category, used only as a subset/boundary check.
5. **Two full passes over the 951-record block with different ranking emphases.**
   Pass 1 (`analyze.py`) ranked by artifact-signal language (GitHub / Hugging
   Face / GitLab / project page / "we release" / "code available") and by concept
   breadth (harness, VLA, robot foundation, world action model, code-as-policy,
   skill, memory, recovery, evaluation, safety, agentic), and additionally listed
   **every record in the block mentioning "harness"** and **all 152 cs.RO
   records**. Pass 2 (`rescreen.py`) re-ranked the whole block by lifecycle and
   contract vocabulary instead — release lifecycle, runtime
   monitor/intervention, verification/audit/provenance, failure
   diagnosis/recovery, evaluation protocol/benchmark contract, skill
   library/discovery/evolution, robot deployment/integration, and harness
   engineering/agent runtime internals. Concept counts in the block: `harness`
   17 records, `VLA` 25, world/action model 27, memory 87, recovery 50,
   evaluation-language 280, safety 98, agentic 68.
6. **Dossiers and primary-source verification** for every shortlisted record
   (28 dossiers): full title/abstract/comments, the arXiv abstract page and the
   export API for version history, submission and update dates, acceptance
   notes, the arXiv full-text HTML for artifact links and release sentences, and
   the official GitHub / Hugging Face APIs (license field, size, creation and
   push dates, default branch, top-level tree) — all on **2026-09-23**.
   Project pages were fetched and inspected directly. Repositories, READMEs,
   licenses, and dataset file counts were inspected for the finalists.
7. **Duplicate screening** of every candidate against `README.md`, `docs/*.md`,
   and `sources/*.md` by arXiv ID, project name, and repository URL before
   writing: **no candidate ID appears in `README.md`**.
8. **Watch-list re-check** of the artifact placeholders recorded by the
   2026-09-22 run (vla.simd, LIBERO-VPro, ARSTAG, React When You Need To,
   Uranus, FoldQuantVLA, SafeStage, AWM-3DFM, Toollery, CHART).

## Included (README updates)

Sixteen entries added, plus five "Current Landscape" bullets. Artifact status is
reported literally; every repository, dataset, and project-page claim below was
verified live on **2026-09-23**.

### Harnesses and Development Platforms — General Harness Design and Self-Improvement

- **Grow the Harness, Not the Context** (`2609.26760`, cs.AI/cs.SE; v1 announced
  2026-09-22, submitted 2026-09-22, 16 pages): the strongest general-harness
  contribution in the block. It starts from a **strategy-free scaffold** that
  exposes fixed model and tool interfaces but encodes no task-solving controller,
  and learns the harness itself from task feedback: function-level execution
  traces localize each failure to a bounded code surface, an optimizer repairs a
  window of failures jointly, and a success-first held-out gate rolls back repair
  sequences that harm prior capability, with accepted edits accumulating in one
  shared harness. Across BrowseComp-Plus and WebArena-Verified with three
  deployment models from 4B to 120B the authors report the highest mean success
  in five of six settings and a 0.7 pp. shortfall in the sixth, with LLM calls
  reduced by 76.0–91.8% and deployed inference cost by 74.4–98.6% relative to a
  Tool-Calling agent; on WebArena-Verified success stays at 44.7–45.3% across
  model scales where Tool-Calling falls to 6.7% with the 4B model. Ablations
  isolate trace-local edits, joint repair, and gate-based rollback. **Artifact
  status:** no artifact was located — the arXiv record, the comment field, and
  the full text contain no repository, dataset, or project-page link. Digital
  agents only; no robot experiment; results author-reported.
- **FIRE: Failure-Informed Runtime Engineering for Reliable Language-Model
  Agents** (`2609.26048`, cs.AI/cs.CL/cs.SE; v1 announced 2026-09-22, submitted
  2026-09-22): makes **runtime policy** the harness's reliability lever —
  targeted natural-language instructions and action denials applied at states
  that preceded observed failures, with model weights and the user prompt
  unchanged. Across the complete 87-task Terminal-Bench 2.1 suite with two
  attempts per task, repeated success (pass^2) rises in all three GPT-5.6 tiers
  (50.6→54.0 Luna, 55.2→60.9 Terra, 64.4→73.6 Sol) while best-of-two moves only
  1.2 points for Sol (+9.2 repeated), which the authors read as policies
  converting reachable solutions into dependable delivery. A randomized five-arm
  experiment separates the mechanism: real policies reach 61% on eligible tasks
  against 39% without a policy, 36% for a timing-matched sham, and 39–43% for
  generic verification or reconsideration. **Verified open (data):** the
  authors release policy source, configuration, run-selection records,
  per-attempt outcomes, and analysis outputs at the public
  `failproofai/fire-runtime-policy-reliability` Hugging Face dataset (74 files,
  created 2026-09-22); the evaluation substrate is the third-party Apache-2.0
  `harbor-framework/harbor`. Digital terminal agents; no robot experiment;
  results author-reported.
- **CliffCompaction: Cost-Efficient Compaction for Long-Horizon Coding Agents**
  (`2609.26779`, cs.AI/cs.LG/cs.SE; v1 announced 2026-09-22, submitted
  2026-09-22): an autocompaction technique whose contribution is a **fidelity
  rule** for the harness's context manager — compacted information is only ever
  truncated or dropped, never rephrased or rewritten, and no compaction is ever
  compacted (each pass operates on original content, prior compacted output is
  discarded), which the authors argue prevents context drift from accumulating
  across sessions. Reported results: up to 50% lower cost under a bounded
  context with maintained or improved Terminal-Bench performance, over 10
  percentage points added on Terminal-Bench for less than the cost of two
  full-context runs, and on KernelBench CUDA-kernel speedups of 2.23× after 200
  steps and 3.58× after 400. **Verified open:** the MIT-licensed
  `nguyenvuthuentrang/cliffcompaction` repository is live (created 2026-08-13,
  pushed 2026-09-23) and the paper describes a scaffold-agnostic API-proxy
  implementation usable with Claude Code, Codex, and other harnesses. Digital
  coding agents; no robot experiment; results author-reported.
- **Recursive self-improvement of AI research agents** (`2609.26457`,
  cs.AI/cs.LG/cs.SE; v1 announced 2026-09-22, submitted 2026-09-22, 28 pages):
  studies the case where the agent's **own code** is the object of optimization,
  so each accepted rewrite becomes the agent that edits the next round. AIDE²
  proposes changes to its own code, benchmarks modified versions of itself on a
  suite of AI R&D tasks, and keeps changes that perform best on **hidden**
  evaluations; in an autonomous eight-day run it discovered seven successive
  improvements spanning a new search policy and context-compressing memory
  mechanisms. The authors report that the strongest discovered agent matches or
  exceeds a human-engineered production research agent on all four held-out
  benchmarks (ML engineering, heuristic algorithm engineering, and
  out-of-distribution physics-based weather forecasting), and that reward
  hacking on a separate held-out task family falls from 55% to 32% during the
  run, 7 points below the human-engineered agent, although the loop never
  optimized for it. **Artifact status:** no first-party artifact was located;
  the full text cites third-party projects (OpenEvolve, karpathy/autoresearch)
  but no release of AIDE² itself. Digital research agents; no robot experiment;
  results author-reported.
- **Making Agents More Consistent: Skills Should Form Habits for Repeat Tasks**
  (`2609.25299`, cs.AI/cs.CR; v1 announced 2026-09-23, submitted 2026-09-21):
  measures the consistency problem directly — across 42 tasks run three times
  each, 38–74% of answers disagree depending on the model, and 95.3–97.2% of
  generated tokens go to re-deriving a plan the system already knows — then
  proposes **skill habit formation**: the agent mines its own execution history
  for deterministic skill variants that compete against the incumbent rather
  than replacing it, each candidate declares the region of input space it
  claims, and four gates of ascending cost admit candidates, the central one
  testing an execution trace against a retained reference within a tolerance
  measured from that reference's own run-to-run variability. On text-to-SQL a
  habit-formed variant reproduced its output on all 456 repeated dispatches and
  was non-inferior to every arm it replaced (p<0.0001) at 14–56% fewer tokens.
  The paper is unusually explicit about the cost: the guard admitted work it
  should have deferred on 2.6% of natural paraphrases and 26% of near-boundary
  inputs, and 11 of 13 such failures were invisible to the trace-conformance
  gate at any threshold — deterministic errors repeat exactly. **Artifact
  status:** no first-party artifact was located in the record or full text.
  Digital agents; no robot experiment; results author-reported.
  The paper explicitly places prompts, context management, and feedback loops
  under "harness engineering", which is why it is curated here rather than under
  model training.

### Robot Agent Systems — Agentic Robot and VLA Harnesses

- **Generalizing Manipulation Skills with a Local Coding Agent** (`2609.26499`,
  cs.RO; v1 announced 2026-09-22, submitted 2026-09-22): asks whether a **local
  open-weight VLM** can control a robot and one-shot generalize to new task
  variations without new human programming or training. A locally served
  Qwen3.8-27B drives a UR3e through a **coding-agent harness**: it writes and
  executes its own code above a service that owns kinematics, safety limits, and
  classic computer-vision primitives, and the agent knows each game through two
  hand-written skills. Across nine tasks built from children's toys probing
  color, size, shape, and task variation, the authors report generalization in
  30 of 45 trials (3.4–67.5 minutes per trial), a 0.48× reduction in duration and
  tool calls when the agent redoes a solved task, and 15.8 h of robot time /
  1,034 motion commands overall; the paper also documents the failure modes that
  remain. **Verified open:** the project page
  `https://rtalwar2.github.io/agentic-coding-for-robot-manipulation/` is live
  (overview video, supplementary, per-task numbers) and links the MIT-licensed
  `airo-ugent/airo-mono` skills/kinematics package (created 2022, pushed
  2026-09-02) plus a public supplementary archive. Real-hardware evidence on one
  UR3e cell; author-reported, no independent reproduction.
- **SafeLoop: Risk-Aware Rollback for Vision-Language-Action Manipulation**
  (`2609.26313`, cs.RO; v1 announced 2026-09-22, submitted 2026-09-22, IROS
  2026): a **non-invasive external wrapper** that adds hazard prediction and
  rollback-based recovery to a frozen VLA without changing its parameters. A
  risk predictor trained on vision and proprioception emits four values — the
  probability and time-to-hazard for body collisions and for object failures —
  and a lightweight controller chooses among continue (noop), save a safety
  checkpoint (record), and retreat in joint space (rollback), with the base
  policy then queried again for an alternative continuation. Across 24 LIBERO
  tasks (16 seeds each) and three real-robot tasks (25 rollouts each) the authors
  report reducing hazard cases by roughly 70% while preserving task success and
  the base-policy control rate. **Verified open:** the Apache-2.0
  `Loule0-0/SafeLoop` repository is live on the `release/safeloop` branch
  (created 2026-07-09, pushed 2026-09-22, 12.7 MB), and the abstract's code link
  resolves. Real-hardware evidence, author-reported.
- **RouteRLT: Learning When and Which RL Specialist Should Control a VLA
  Policy** (`2609.26467`, cs.RO; v1 announced 2026-09-22, submitted 2026-09-22,
  IROS 2026 IARL workshop): treats controller **arbitration** as the runtime
  contract rather than refining the VLA further. A phase selector identifies
  which controller is active, a stabilizer suppresses transient switches, and an
  action-boundary manager handles transitions between chunked policy outputs, so
  a generalist VLA keeps its broad competence while RL specialists take over
  precision-critical phases such as connector insertion and cable management.
  The authors report gains on LIBERO multi-object pick-and-place and on
  real-world cable pickup and port-insertion tasks. **Artifact status:** no
  first-party repository or project page was located on the record or in the
  full text; the paper cites third-party infrastructure (RLinf, a Trossen
  stationary-AI cell) but releases no code of its own. Real-robot evidence,
  author-reported; no independent reproduction.
- **Beyond End-Task Success: How to Audit Visual Experience Retrieval in
  Robotics** (`2609.26567`, cs.CV/cs.RO; v1 announced 2026-09-22, submitted
  2026-09-22, IROS 2026 ReS AI workshop): an **audit methodology** for the reuse
  half of experience-based robot adaptation. Most systems select which stored
  experience to reuse by visual similarity and report only the success of the
  selected experience, which cannot show whether the selection rule was good;
  the audit instead executes every stored experience in every query scene over
  two manipulation tasks, three reuse mechanisms, and libraries of K=3, 10, and
  50, so a score can be traced to per-scene selection or to library quality.
  Findings include: one hindsight-chosen experience captures 30–58% of the gap
  between random selection and an oracle; at K≥10 visual rules concentrate on
  one experience 1.5–3× more than the oracle; and visual distance predicts
  whether a pair will succeed (AUROC up to 0.96) yet ranks candidates within one
  scene no better than chance for four of five embeddings at K=50. **Artifact
  status:** no artifact was located; because exhaustive execution is usually
  infeasible, the paper reduces the audit to two cheap reports — the
  distribution of selected experiences and the success of the best single
  experience in hindsight. Simulation evidence, author-reported, no code link.

### Benchmarks and Evaluation — Manipulation and VLA

- **RoboFollow: Unveiling the Instruction Following Mirage in Embodied Agents**
  (`2609.25636`, cs.RO; v1 announced 2026-09-22, submitted 2026-09-22, repo
  marked CoRL 2026): diagnoses why high success rates overstate language
  grounding. The structural cause is **low scene entropy** — when a scene admits
  only one valid task, language is redundant and a policy can score highly while
  barely using it — so RoboFollow builds high-entropy scenes where each training
  scene supports multiple kinematically distinct task branches, adds a four-level
  diagnostic protocol (L0–L3) that progressively perturbs visual layout and
  semantics across spatial relations, attributes, trajectory constraints, and
  logic, and reports stage-wise Intent and Execution scores to separate
  comprehension from motor execution. Across nine VLA and WAM policies the
  authors report that strong L0 performance does not reliably transfer to L1–L3,
  and that stronger VLM backbones, QA co-training, LangForce, and
  classifier-free guidance all fail to close the gap. **Verified open:** the
  MIT-licensed `AutoLab-SAI-SJTU/RoboFollow` repository is live (created
  2026-09-20, pushed 2026-09-23, 15 MB) and the 15,158-file
  `AutoLab-SJTU/robofollow-data` dataset is public on Hugging Face (updated
  2026-09-23). Simulation benchmark; results author-reported.
- **IndustrialVLA-Bench: A Traceable Multi-Axis Evaluation of Open Robot Policy
  Models** (`2609.25562`, cs.AI/cs.RO; v1 announced 2026-09-22, submitted
  2026-09-22): evaluates six released VLA and world-action-model systems under a
  **unified, evidence-tiered reporting schema** that separates clean capability
  (LIBERO), non-language robustness (LIBERO-Plus), instruction sensitivity
  (LIBERO-Para), and observed execution cost (latency, peak memory, runtime
  mode), with every system carrying an evidence status and only
  protocol-faithful entries supporting strict comparison. The authors report that
  clean LIBERO averages across the six systems differ by only 1.58 points while
  robustness and paraphrase summaries span 14.62 and 31.08 points, and that
  restricting every comparison to the three protocol-faithful systems preserves
  the effect (1.36 / 14.62 / 23.10). **Verified open:** the
  `xiaoqi-7/IndustrialVLA-Bench` repository is live and contains the harness,
  environments, model integrations, normalized results, and per-run records (42
  MB) — but it **asserts no license** and has not been pushed since 2026-07-27,
  so it is described as public-but-unlicensed rather than open source.
  Simulation evaluation records; results author-reported.
- **VLAQuantBench: Closed-Loop Evaluation of Post-Training Quantization for
  Vision-Language-Action Models** (`2609.25376`, cs.AI/cs.RO; v1 announced
  2026-09-23, submitted 2026-09-21, 28 pages): a controlled evaluation of the
  precision choices that determine whether a VLA fits on deployment hardware:
  409 runs and 94,574 simulation episodes over four models on LIBERO with X-VLA
  on three further benchmark families, plus real-kernel and physical-robot
  measurements. The headline interaction is that precision selection is
  recipe-dependent rather than governed by universal layer-sensitivity rules:
  under uncalibrated W4A4 round-to-nearest, expanding a π0.5 action-head subset
  from 126 to 167 layers raises success from 7.0% to 70.5%, two-episode
  calibration removes severe joint failures in the tested subsets while the same
  smoothing-and-clipping recipe lowers π0 success, and for OpenVLA-OFT
  protecting one 28,672-parameter output projection restores near-baseline
  success. **Verified open:** the MIT-licensed `jiuyixu25/VLAQuantBench`
  repository (created 2026-09-18, pushed 2026-09-21) releases code,
  configurations, and episode records. Simulation-led with some real-robot
  measurement; results author-reported.
- **HazardArena: Evaluating Semantic Safety in Vision-Language-Action Models**
  (`2604.12447`, cs.RO; **v2 announced 2026-09-23**, v1 2026-04-14, updated
  2026-09-22): an important revision of a semantic-safety benchmark. It is built
  from **motor-matched safe/unsafe twin scenarios** that share objects, layouts,
  and action requirements and differ only in the semantic context that decides
  whether an action is unsafe, so task capability and safety competence are
  separated rather than conflated; it contains over 2,000 assets and 40
  risk-sensitive tasks across seven risk categories grounded in established
  robotic safety standards, and instruments rollouts with ordered
  ATTEMPT/COMMIT/SUCCESS events. The authors report that VLAs trained
  exclusively on safe scenarios often fail to behave safely in the matched
  unsafe counterparts, and propose a **training-free Safety Option Layer** that
  constrains action execution through semantic attributes or a vision-language
  judge, substantially reducing unsafe behavior at minimal task-performance
  cost. **Verified open:** the `HazardArena-Team/HazardArena` repository is
  public under an MIT license (LICENSE verified 2026-09-23; the GitHub API
  reports `NOASSERTION` because the file lists eleven copyright holders) and
  contains the 51-scenario risk inventory, seed-defined evaluation, stage-wise
  evaluator, policy adapters, dataset export, and tests. The v2 revision carried
  the updated code-availability statement. Simulation benchmark; results
  author-reported.

### Benchmarks and Evaluation — Simulation and World-Model Fidelity

- **TriWorldBench: A Tri-View Consistency Perspective on Embodied World
  Models** (`2609.26314`, cs.AI/cs.RO; v1 announced 2026-09-22, submitted
  2026-09-22): evaluates embodied world models through **synchronized head,
  left-wrist, and right-wrist videos**, because head and wrist views are
  complementary and evaluating them independently cannot determine whether they
  describe the same action and object state. It contains 500 episodes across 50
  bimanual manipulation tasks and uses 19 metrics spanning tri-view consistency,
  task alignment, physical and 3D coherence, motion quality, temporal
  consistency, and visual quality, summarized by a TWB-Score with per-view
  results retained to localize failures. **Verified open:** the
  `TriWorldBench/TriWorldBench` repository is live (232 stars, 81 MB, created
  2026-07-27, pushed 2026-09-20) with the metric definitions, and the
  `TriWorldBench/Dataset` dataset is public on Hugging Face (verified
  2026-09-23) — the repository **asserts no license**, so it is described as
  public-but-unlicensed rather than open source. Simulation benchmark; results
  author-reported.

### Runtime, Safety, and Observability — Safety

- **Silent Sabotage** (`2609.26184`, cs.AI/cs.CR/cs.RO; v1 announced 2026-09-23,
  submitted 2026-09-02) and **StepTrigger** (`2609.26131`, cs.AI/cs.CR/cs.RO; v1
  announced 2026-09-23, submitted 2026-08-07): two complementary studies showing
  that the backdoor trigger surface for embodied agents is moving **inside the
  agent's own state** rather than into prompts, visible objects, or scene
  semantics. Silent Sabotage embeds a backdoor in an LLM-based robot controller
  that is triggered by a rare sequence of the robot's **own past actions**,
  stays dormant during normal operation, and can induce a stop or a collision;
  the authors report a near-perfect attack success rate in simulation across
  several robots and LLMs while remaining hard to detect. StepTrigger triggers on
  **pressure and foot-ground contact patterns** produced when a Unitree Go1
  crosses a dense terrain patch, learns a selective backdoor policy that uses
  incidental pressure events as benign examples, and reports 98.75% clean
  behavior preservation, 92.50% false-trigger rejection, 76.25% true-trigger
  activation, and 89.17% overall parsed behavior accuracy in a stratified
  offline evaluation. **Artifact status:** Silent Sabotage has a public
  MIT-licensed first-party repository (`doniobidov/silent_sabotage`) that
  predates the preprint — its description identifies it as the official
  repository for the EAI SmartSP 2025 accepted version (created 2025-07-10,
  pushed 2025-08-26) — while no artifact was located for StepTrigger. Both are
  simulation/offline-evaluation studies; results author-reported.

### Benchmarks and Evaluation — What to Measure

- **Beyond End-Task Success** — full description under *Robot Agent Systems —
  Agentic Robot and VLA Harnesses* above. Its README home is this section, since
  the reusable part is the audit contract (report the distribution of selected
  experiences and the success of the best single experience in hindsight) rather
  than the retrieval mechanisms it audits.

### Planning, Tool Use, and Skill Composition

- **X-Planner: Event-Structured Task Planning for Embodied Intelligence**
  (`2609.25187`, cs.AI; v1 announced 2026-09-23, submitted 2026-09-21): treats
  the intermediate structure between a high-level instruction and a VLA as the
  thing to supervise and represent, rather than leaving it implicit in
  chain-of-thought text. A shared VLM backbone exposes two event-structured plan
  forms — a discrete interface emitting interpretable event states, and a latent
  interface relaying continuous CoT states across staggered Transformer depths
  through Staircase Decoding with a frozen latent-to-text reconstruction anchor —
  and the planning data combine Ego, UMI, and teleoperation under a hierarchy
  granularity with takeover-time annotations and human-designed failures
  supervising ongoing error recognition. Offline two-step planning evaluation
  places X-Planner second among four evaluated models on both BERTScore-F1 and a
  judge-based Overall score; real-robot experiments are reported to outperform
  the evaluated baselines. **Verified open:** the MIT-licensed
  `X-Square-Robot/Xplanner` repository is live (45 stars, 133 MB, created
  2026-09-01, pushed 2026-09-21). Results author-reported.

## Rejected / watch list

Everything below was read against the arXiv record, the full-text HTML, and
(where a link existed) the live project page, repository API, or raw repository
content on 2026-09-23.

### Carried watch list — re-checked, status unchanged

- **vla.simd: Efficient CPU Inference for Language-Conditioned Manipulation**
  (`2609.24274`) — its project page still returns HTTP 200 and still states that
  code and checkpoints will be released on acceptance. **Not open**; watch.
- **LIBERO-VPro** (`2609.24350`) — no release; artifact links remain "Dataset
  (soon)". **Watch.**
- **ARSTAG** (`2609.24563`) — `boweili666/ARSTAG` still resolves only to the
  project website, not code. **Watch.**
- **React When You Need To** (`2609.22587`) — code still promised on acceptance.
  **Watch.**
- **Uranus** (`2609.24815`) — **withdrawn by the authors in this block**
  (incomplete manuscript; no unanimous agreement to release artifacts). The
  advertised `D-Robotics-AI-Lab/Uranus-SDK` still returns 404, as does
  `cair-vinuni/FoldQuantVLA` (`2609.24433`). **Watch status closed; treated as
  withdrawn, not merely unreleased.**
- **SafeStage** (`2609.21223`) — re-checked: `JinzhuLuo/SafeStage` has a
  description but is still a 0 KB, license-free stub (last push 2026-09-17).
  **Not open**; watch.
- **AWM-3DFM** (`2609.21502`) — `dtc111111/AWM-3DFM` still returns 404. **Watch.**
- **Toollery** (`2609.22218`) — `XiangxiTian/toollery` still resolves, still
  asserts no license; first-party provenance remains unconfirmed. **Watch.**
- **CHART** (`2609.22247`) — still no first-party artifact. **Watch.**

### Verified and rejected — artifact claimed but not reachable

- **RoboTwin-Phys: Do WAMs and VLAs Understand the Physical World?**
  (`2609.26292`, cs.RO; technical report for a benchmark): a physics-diverse
  manipulation benchmark that continuously varies 13 physical attributes within
  plausible ranges and states that it releases **more than 5,000 expert
  demonstrations with 13-dimensional ground-truth physical parameters** — a
  genuinely useful addition for condition-aware policy training and evaluation.
  But **no repository, dataset, project page, or download location appears on
  the arXiv record or anywhere in the full text**, so the release is a claim
  without an access point and is treated literally as not reachable. **Watch** —
  this would qualify immediately once the dataset is locatable.
- **HABILIS Brain 0: Geometry-Change Supervision for VLA and Residual Flow
  Recovery** (`2609.25558`, cs.LG/cs.RO): a four-stage recipe where the
  harness-relevant part is stage 4, **Geometry-Conditioned Residual Flow**: with
  GC-VLA frozen, a binary intervention router and a single bounded residual
  velocity policy are learned from closed-loop feedback, taking reported LIBERO
  success from 95.20% to 99.55%. The mechanism is a recovery primitive and worth
  tracking, but no code, weights, or project page was located — only a corporate
  domain (`tommoro.ai`, HTTP 200) with no artifact section. **Watch.**
- **MATE: Multi-Agent Virtual Teleoperation Platform for Humanoid Collaboration
  Data Collection** (`2609.26520`, cs.RO): a virtual multi-operator teleoperation
  platform producing 24.1 hours / 2,500 episodes of multi-humanoid collaboration
  data with an execution-aligned interaction sampling strategy and reported
  zero-shot transfer to a physical humanoid. The linked project page
  (`yerik-yu.github.io/MATE/`) is live but offers demonstrations only; no code or
  dataset repository was located. **Watch.**
- **DTOC: Dynamic Tool Output Compression** (`2609.26121`, cs.AI/cs.CL/cs.MA;
  Discovery Science 2026): reversible context management that keeps full tool
  outputs in external memory and inserts compact placeholders that can be
  selectively reconstructed, with an ablation showing reversibility is what
  matters. The only implementation link is a personal fork of OpenCode
  (`chaturvediabhay24/opencode`, created and last pushed 2026-06-13/15 — before
  the preprint), so first-party provenance is unconfirmed. **Watch.**

### Verified and rejected — no artifact, out of scope, or both

- **The Tasteful Agent / Taste-Bench** (`2609.25804`, cs.AI): a well-built
  benchmark of long-horizon *model judgment* (decision forks mined automatically
  from parallel attempts and detours; the best model answers 59.7%), with an MIT
  repository and a public dataset. Excluded as **out of scope**: it measures a
  model capability rather than a harness runtime, interface, or evaluation
  contract, and the fork-selection questions are answered by the model with no
  harness in the loop. Recorded here because the dataset is real and may be
  useful for harness comparisons later.
- **The Delegation Blind Spot** (`2609.26642`, cs.AI/cs.LG): an executable audit
  for deciding which product improvement an agent's choices identify, with a
  public MIT repository. The framework is decision-theoretic rather than
  harness-runtime, has no robot component, and the study has no human
  participants or real outcomes. Excluded (scope); the "decision receipt"
  primitive is noted for later audit work.
- **ActGov: Governing LLM Agent Actions via Policy-Constrained Validation**
  (`2609.24446`, cs.AI/cs.CR, **v2** updated 2026-09-22): runtime enforcement
  that validates each proposed tool action before it causes external effects,
  with an iteratively constructed policy set verified by SMT-based
  counterexample checking, evaluated on AgentDojo and AgentDyn. Genuinely
  on-contract for harness safety, but no artifact was located and the list
  already carries the adjacent authorization/composition entries (Bounded
  Agents, One Gate Is Not Enough). Excluded this round; **watch** for code.
- **WeightBridge** (`2609.25442`, cs.DC/cs.LG/cs.NI): an efficient
  trainer-to-rollout weight-transfer library with a small general API and
  reported GPU-stall reductions up to 42×. It is RL *systems infrastructure*
  rather than an agent harness (no tools, state, permissions, or verification),
  so it is out of scope for this list.
- **Toward Self-Repairing / User-Mediated Self-Repair in Ubiquitous Robots**
  (`2609.26155`, `2609.26157`, cs.RO/cs.AI): two versions of the same
  goal-oriented agentic architecture for guiding non-expert users through
  hardware repair (95% completion, twenty participants). The "self-repair" here
  is user-mediated maintenance of the device, not runtime recovery of an agent
  or policy; no artifact was located. Excluded (scope).
- **Skill Sequence Planning for Collaborative Multi-Robot Construction**
  (`2609.25649`, cs.RO): reusable preprogrammed skills sequenced by a central
  controller over a construction relationship graph, with human approval of the
  plan through a digital twin. A reasonable skill-composition instantiation, but
  the domain is construction-specific, there is no released artifact, and the
  contribution is a planner rather than a harness contract. Excluded.
- **PatchWAM** (`2609.25961`, cs.RO), **CausalWM** (`2609.23184`),
  **Skytopia** (`2609.26007`), **MachEmbodied-U0** (`2609.25627`): world/action
  model variants. Each is a model or representation contribution with no located
  artifact; the WAM variants stay on the general watch list for artifact
  releases, consistent with the standing policy for this line.
- **Deployment, perception, control, medical, agricultural, and driving
  records** in the block (including `2609.26490` benchmarking robots in public
  environments, `2609.25688` MatcherCompass, `2609.25511` identity-gated drone
  gesture control, and the ϕ-RIE interactive-environment release `2609.26795`):
  excluded per the standing scope policy unless they introduce a reusable
  agent/VLA harness, recovery, safety, or evaluation contract. Two near-exceptions
  were examined and rejected: **`2609.26490`** is a thorough three-year
  real-world benchmarking framework (Frontiers in Robotics and AI) but is
  domain-operational rather than a reusable harness contract, and **`2609.26777`
  SWE-Serve** benchmarks agentic engineering for production inference serving,
  which is digital infrastructure rather than a robot or general agent harness.
- **`2609.25052` Self-Cleaning and Captured Anyway**, **`2609.25853`
  MemoryAthena**, **`2609.26219` PatchKV**, **`2609.25960` CausalLoss-Fin**,
  **`2609.26642`**, **`2609.26135` VACS**, **`2609.25570`**, **`2609.25194`**:
  memory-, KV-, attribution-, shielding-, and agent-population papers surfaced by
  the second pass. Each is either a digital-agent result with no located
  artifact or a security/economics study outside this list's harness contract.
  Excluded; the memory and shielding lines remain of interest for later rounds.

## Current Landscape additions

Five bullets added to `README.md`:

1. Harness self-improvement is turning from prompt/context edits into **program
   growth with admission control** (Growing Harness, FIRE, CliffCompaction).
2. Recursive self-improvement is reaching agents that rewrite their own research
   code under hidden evaluation (AIDE²), including an unoptimized reduction in
   reward hacking.
3. The non-invasive wrapper is the emerging VLA runtime pattern: rollback
   (SafeLoop), arbitration (RouteRLT), and residual recovery (HABILIS GCRF) all
   leave the policy frozen.
4. Robot evaluation is becoming evidence-tiered and physics-diverse
   (IndustrialVLA-Bench, RoboFollow, VLAQuantBench, RoboTwin-Phys).
5. Embodied backdoor triggers are moving into the agent's own state — action
   history and contact signals — rather than prompts or visible objects.

## Validation performed

- `git diff --check` clean (no whitespace errors, no conflict markers).
- Markdown structure re-checked after every insertion: each new entry is a
  single top-level bullet with balanced brackets and parentheses; section
  headings and Contents links unchanged.
- Every added link was fetched or queried on 2026-09-23 (arXiv abstract pages
  and full-text HTML, project pages, GitHub REST API, Hugging Face dataset API,
  raw README/LICENSE files); HTTP status and the reported artifact scope are
  recorded above.
- Dates checked for cross-file consistency: the README badge, the
  "Last verified" line, and this record all carry **2026-09-23**.
- Categories checked against the arXiv record for every included entry.
- Cross-file consistency: `README.md` entries, this record, and the previous
  record agree on scope, and the withdrawn Uranus entry is recorded here while
  remaining absent from the curated list.

## Operational notes and blockers

- The SSH remote required `GIT_SSH_COMMAND='ssh -F /dev/null'` because of the
  known `/etc/ssh/ssh_config.d` ownership problem; `git ls-remote` succeeded with
  that override and confirmed `refs/heads/main`.
- GitHub REST API rate limits were not hit during repository verification; the
  Hugging Face dataset API was reachable for all four dataset checks.
- The previous run's export-API sample for the 2026-09-22 `submittedDate` window
  returned 0 for all five categories; that window is now indexed (cs.RO 81,
  cs.AI 125, cs.CL 54, cs.CV 92, cs.LG 123), confirming the announcement lag
  pattern rather than any retrieval failure.

## Commit

Task-owned files committed on `main`: `README.md`,
`sources/daily-arxiv-2026-09-23.md`. Unrelated user changes
(`docs/reference-architecture.md`, `docs/ring-harness.png`, `handoff.md`) were
left untouched and unstaged.
