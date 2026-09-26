# Daily arXiv scan — 2026-09-26

## Scope

- **Interval:** everything announced after the 2026-09-25 run's cutoff. That run
  (started ~07:03 UTC, committed 09:13 local on **Friday 2026-09-25**) screened
  the announcement block whose OAI datestamp is **2026-09-25** — 901 unique base
  IDs, newest base ID `2609.30266`. This run started ~08:00 UTC on **Saturday
  2026-09-26** and covers everything announced since. There are **no missed
  dates**: the 2026-09-25 record was yesterday's block, and no cron slot between
  the two runs was skipped.
- **No new announcement block landed.** The OAI-PMH harvest for
  `from=2026-09-26&until=2026-09-26` returned `noRecordsMatch` for **all five
  categories** (cs.RO, cs.AI, cs.CL, cs.CV, cs.LG) — zero records. This is the
  expected weekend gap: arXiv's announcement batches run Sunday-through-Thursday
  nights US Eastern, so the block after the one dated 2026-09-25 is the
  Sunday-night batch, which will carry datestamp 2026-09-28. The same
  no-new-block situation was recorded on Saturday **2026-09-19** and Sunday
  **2026-09-20** of the previous weekend.
- **This is the required "unchanged batch" case.** The newest visible block is
  the same 2026-09-25 block yesterday's run screened, so — as the procedure
  requires — the **entire block was re-screened once** with a deliberately
  different ranking emphasis (see Method step 4) rather than assumed settled.
  The re-screen produced **seven README additions** that the first pass did not
  carry; no earlier verdict was reversed.
- **Wider window, for the delta:** `from=2026-09-25&until=2026-09-26` returns
  **1,246 memberships = 902 unique base IDs, every one carrying datestamp
  2026-09-25** (memberships: cs.AI 365, cs.LG 341, cs.CV 206, cs.CL 193,
  **cs.RO 141**). No record in the window carries datestamp 2026-09-26.
- **Delta against the previous run's harvest (exact, with an entity-normalisation
  control).** Previous window (2026-09-24…2026-09-25) 1,700 unique IDs → current
  window (2026-09-25…2026-09-26) 902 unique IDs. **1 ID appeared, 0 disappeared
  from the 09-25 block, and 0 records changed** in datestamp, title, abstract,
  comments, journal-ref, DOI, created date, or category set. The 799 "missing"
  IDs are a **window shift, not retractions** — they carried datestamp
  2026-09-24 and are simply outside `from=2026-09-25`. The one added ID is
  `2609.12997` (SV-Cine, diagnosis-conditioned segmentation of single-ventricle
  physiology, cs.CV; created 2026-09-11), re-datestamped into the block and out
  of scope; it also explains the block growing 901 → 902 and cs.CV 205 → 206.
  A first pass of the comparison showed 33 "changed" records, all of which
  vanished once the previous run's parser was matched on HTML-entity handling
  (`&#34;` versus `"`) — they were artefacts of this run's parser, not record
  changes, and are **not** reported as edits.
- **Block composition is unchanged.** 616 records carry a created date of
  2026-09-24, 144 of 2026-09-23, and the tail reaches back through August and
  July to 2025 (re-announced replacement versions); **198 records carry pre-`2609.`
  IDs**. Newest base ID is still **`2609.30266`**, so the ID frontier did not
  move and nothing is being held back.
- **Withdrawals in this block: three, all out of scope and all already recorded
  on 2026-09-25.** `2608.06825` (multiscale reward hedging), `2608.24098`
  (uniform stability), `2608.25326` (transductive learning) carry the arXiv admin
  note "withdrawn by arXiv due to unverifiable authorship and affiliation". No
  author withdrawal appears. (`2609.30074` matches a raw text search for
  "withdrawn" only because the word occurs in its abstract; it is not a
  withdrawal.)
- **Export-API cross-check returns nothing usable this window.** With the range
  URL-encoded, `submittedDate` windows return **0 for all five categories on both
  2026-09-25 and 2026-09-26** — Friday's submissions were still announced
  without being export-indexed, the same announcement-versus-indexing lag seen in
  previous runs. The export API is therefore used strictly as a boundary check;
  the OAI harvest is the authoritative source.
- **No curated record needed a revision edit.** 76 IDs belonging to entries
  already curated (or already recorded as rejected) in this repository appear in
  the block, but because the block is byte-stable across the two runs their
  statuses carry forward unchanged from the 2026-09-25 record.
- **Watch-list re-check:** none of the eleven carried watch IDs (`2609.24274`,
  `2609.24350`, `2609.24563`, `2609.22587`, `2609.21223`, `2609.21502`,
  `2609.22218`, `2609.22247`, `2609.26121`, `2609.26520`, `2609.25558`) is
  present in this block, so all statuses are carried forward unchanged.

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`) for two windows: `from=2026-09-26&until=2026-09-26`
   (the new block — empty) and `from=2026-09-25&until=2026-09-26` (previous block
   plus the new one). Pages saved to `.scratch/arxiv-2026-09-26/oai-{new,full}-*.xml`;
   every category returned a single complete page with no resumption token, and
   the new window returned `noRecordsMatch` for all five.
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`oai-records-{new,full}.json`, `parse_oai.py`), then **delta against the
   previous run's parsed harvest** with field-wise before/after values
   (`delta.json`), plus an **entity-normalisation control** that re-applies the
   previous run's HTML-entity handling to remove parser-induced false diffs.
   Window shift was separated from retraction explicitly.
3. **Export-API boundary cross-check** over the 2026-09-25 and 2026-09-26
   `submittedDate` windows per category.
4. **Third-pass re-screen with a deliberately different emphasis.** Yesterday's
   passes ranked by (1) artifact-signal language plus concept breadth and
   (2) lifecycle, monitoring, verification, recovery, and governance vocabulary.
   This pass instead mapped the block onto the **README's own taxonomy and its
   comparatively under-covered sections** — datasets and data infrastructure,
   simulation and digital twins, observability and replay, runtime building
   blocks, the safety envelope, world/physical-reasoning and perception
   foundations, VLA interface/adaptation angles, the benchmark lanes, surveys,
   and the harness-owned verbs from the README layer table (tools, state, timing,
   permissions, execution, recovery, recording, verification) — and additionally
   produced three independent listings: **all 146 robot/embodied records** in the
   block, **every record naming a concrete release object**, and a ranked list of
   the **826 records that appear nowhere in the repository's markdown**, scored
   by how many harness-layer vocabularies they touch (robot relevance weighted).
   This is what surfaced the additions below; the first two passes had not
   carried any of them.
5. **Dossiers and primary-source verification** for every shortlisted record:
   arXiv API version history and dates, the arXiv abstract page and full-text
   HTML for artifact links and release sentences, official project pages, and the
   GitHub API (license, size, creation and push dates, default branch, top-level
   tree, raw README) — all on **2026-09-26**. Nothing was taken from a secondary
   source.
6. **Duplicate screening** of every candidate against `README.md`, `docs/*.md`,
   and `sources/*.md` by arXiv ID, project name, and repository URL before
   writing. No included ID was already present in `README.md`.
7. **Watch-list re-check** by ID against the block (none present; see Scope).

## Included (README updates)

Seven new entries, in five README sections. Artifact status is reported
literally; every repository, license, date, project page, and tree claim below
was verified live on **2026-09-26**. No entry is a revision edit, because no
curated record changed in this window.

### Harnesses and Development Platforms — General Harness Design and Self-Improvement

- **When Search Becomes Memory / Auto-Robotist** (`2605.25832`, cs.RO/cs.AI/cs.CL/cs.CV;
  v1 2026-05-25, **v2 2026-09-23**, announced in this block, EMNLP 2026 Main,
  18 pages): makes the reusable object of a robot-design loop an **inspectable
  skill library** rather than an implicit population. Each skill stores a
  structural archetype, evidence-grounded positive and negative rules, and the
  evaluated designs that support them; during search the agent retrieves skills
  to condition LLM edits of elite bodies while retaining a genetic-algorithm
  mutation path for exploration, and after evaluation it updates the library
  through **Add, Diagnose, and Merge**. Reported: improved cold-start 5×5 EvoGym
  search and transfer of learned skills to 10×10 design spaces, where
  reference-conditioned transfer beats the GA on all seven tasks (locomotion,
  traversal, object interaction). **Verified open:** the MIT-licensed
  `wangyf9/Auto-Robotist` repository (39.7 MB, created 2026-08-07, pushed
  2026-09-09) is a real package with tests and tutorials, the contribution
  isolated in `examples/solo_leveling/` over an unchanged EvoGym. Evidence is
  simulated soft-robot morphology design, not physical robots; results
  author-reported.

### Harnesses and Development Platforms — Robot Middleware and Execution

- **A Field-Deployable GNSS-based Navigation Stack for Outdoor Mobile Robots**
  (`2609.28933`, cs.RO/eess.SY; v1 announced 2026-09-25, submitted 2026-09-24):
  publishes the **runtime contract** of a deployed waypoint-navigation stack,
  not only its tracking error. The architecture specifies coordinate
  conventions, datum initialization, asynchronous state construction, waypoint
  geometry, controller equations, quality gates, command arbitration, and
  watchdog behavior; two localization front ends (single-GNSS–IMU and
  dual-antenna GNSS) present one common local East–North–Up state interface, and
  five trackers (`pid_line`, `pure_pursuit`, `mpc_rollout`, `mpc_formal`,
  segment-aware `row_hybrid`) are interchangeable behind it — which is what makes
  the eight controller–localization combinations comparable. Reported: a
  balanced evaluation of **800 physical field runs** (100 per combination,
  ~199.6 m route) from 2025–2026 grape-vineyard deployments, with row-hybrid at
  the lowest run-averaged post-acquisition mean absolute cross-track error
  (0.00952 m single GNSS+IMU; 0.00846 m dual GNSS) — an accuracy characterization
  under the evaluated conditions, not a safety claim. **Verified open:**
  MIT-licensed `YiyuanLinXX/PPBv2` (37.8 MB, created 2025-06-30, pushed
  2026-09-25), whose `PPBv2_Navigation/` carries the ROS 2 package, an RTK safety
  monitor, the `twist_mux` pipeline, and an explicit `HANDOFF.md` for the
  GNSS/geometry/safety/control path. Real-robot field evidence; results
  author-reported.

### Datasets and Data Infrastructure — Formats and Collection

- **PolyUMI** (`2609.29760`, cs.RO; v1 announced 2026-09-25, submitted
  2026-09-24, 9 pages): treats **collection hardware and the recording schema**
  as the artifact for the modalities imitation learning usually drops. A
  lightweight wireless handheld gripper records synchronized wrist-camera,
  optical-tactile, contact-audio, and proprioceptive observations with no
  tethered workstation, and the same sensing finger transfers to the robot end
  effector so the sensing geometry is preserved between demonstration and
  execution. VisTA, a token-level multimodal policy, integrates the streams
  across sensors and time; across object inference, slip control, and
  contact-rich manipulation the authors report that touch and audio carry
  task-relevant information beyond vision and that VisTA is competitive with or
  outperforms existing multimodal policies. **Artifact status literal:** the
  project page is live and links an MIT-licensed platform repository that is
  substantive (76 MB; `ros2_ws`, `ingest`, `inference_server`, `docker`, `docs`,
  `tools`, `train_policy.sh`), but its own description states it was "replicated
  onto an anonymous account with no commit history for review purposes", so
  first-party identity and history are not yet confirmed. Real-hardware
  platform; results author-reported.

### Benchmarks and Evaluation — Agent and Embodied Reasoning

- **Causal-Plan-Bench / Causal Planner** (`2606.01810`, cs.AI; v1 2026-06-01,
  **v2 2026-09-24**, announced in this block, 84 pages): argues that embodied
  planning benchmarks inadvertently reward linguistic next-token prediction over
  physically grounded next-state reasoning. Contributes **Causal-Plan-Bench**
  (four causal dimensions, multi-stage verification), **Causal-Plan-1M** (a
  million-scale corpus of explicit causal reasoning traces from a four-stage
  annotation pipeline over egocentric video), and a training recipe producing
  **Causal Planner**, reported to raise its Qwen3-VL-8B backbone from 33.23 to
  45.28 (+36.3% relative) and to improve three external benchmarks without
  benchmark-specific adaptation, with a reported Causal-Supervision Scaling
  Trend and paired no-vision controls, cross-judge comparison, and human scoring.
  Leading models still score low on physical agency (GPT-6-astra 43.04). **The
  v2 revision is substantive**: v1 was the benchmark and corpus only; v2 adds the
  trained reasoner and its results. **Artifact status literal:** the code link
  `THUSI-Lab/Causal-Reasoner` is a real release (5.5 MB; `evaluation/`,
  `four_stage_generation/`, `qa_filtering/`, `qa_generation/`, `sft_training/`,
  `rl_training/`, `causal_trace_generation/`, 12 KB README) but **asserts no
  license** — source-available, not open source — and was last pushed 2026-06-02,
  so the v2 revision did not update it; no first-party Hugging Face release of
  the benchmark or the 1M corpus was located.

### Runtime, Safety, and Observability — Safety

- **CrossSafe: Towards Cross-Embodiment Latent Safety Filters** (`2609.28984`,
  cs.RO/cs.AI/cs.LG/eess.SY; v1 announced 2026-09-25, submitted 2026-09-24;
  Washington University in St. Louis): moves the safety filter from a per-robot
  artifact to a **shared, embodiment-conditioned contract**. The reasoning needed
  to detect an obstacle, recognize it must be avoided, and select a safe abstract
  action is largely shared, while morphology, kinematics, and dynamics decide
  which concrete action realizes it — so the same action can be safe for one
  robot and unsafe for another. CrossSafe shares a Hamilton–Jacobi
  reachability value function and its safety-maximizing policy and performs
  reachability analysis directly in a morphology-aware latent space, so safety
  concepts generalize while remaining explicitly conditioned on each robot.
  Reported: across five bimanual embodiments and five manipulation tasks with
  whole-body collision-avoidance constraints, one policy jointly trained on four
  embodiments generalizes zero-shot to a held-out embodiment and reduces the
  nominal policy's collision rate, and training on more embodiments improves
  generalization. **Artifact status literal:** the project page is live and its
  Code button reads "coming soon" — **not open**. Simulation evidence; results
  author-reported.
- **Temporal Gradient Inversion for Private Trajectory Reconstruction in
  Embodied Reinforcement Learning** (`2609.30258`, cs.LG; v1 announced
  2026-09-25, submitted 2026-09-24, NeurIPS 2026): names a trust boundary on the
  **learning channel** rather than the action path. TRACE is an amortized
  temporal gradient-inversion attack that autoregressively reconstructs the
  sequence of private observation–action trajectories from per-step
  policy-learning gradients, exploiting cross-time correlation between successive
  embodied gradients (formalized as a conditional mutual-information bound) and
  closed-form action recovery from policy-head gradient structure (proved exact
  when standard entropy regularization is sufficiently small). Reported: 18.8 dB
  PSNR with near-perfect action recovery at 3–4.5 ms per reconstructed frame on
  held-out embodied scenes, dominating the learning-based baseline on every
  reconstruction metric and exceeding optimization attacks by orders of magnitude
  in speed, with the attack extending across recurrent, residual, and
  compact-transformer victims, multi-modal inputs, and larger discrete action
  spaces; the defense experiments suggest temporal gradient streams need
  sequence-aware privacy mechanisms. **No artifact located** — the full text
  links only third-party gradient-inversion codebases. Evaluation is on recorded
  embodied scenes rather than a live robot, and no harness admission path is
  involved; results author-reported.

### Surveys and Reading Lists

- **Software Engineering for Self-Adaptive Robotics: A Research Agenda**
  (`2505.19629`, cs.SE/cs.RO, ACM TOSEM 2026, DOI 10.1145/3828754): the
  systems-engineering counterpart to this list's harness line and the clearest
  statement of what a robot's runtime adaptation layer is expected to own over
  its lifetime. It structures the agenda along the **software-engineering
  lifecycle** (requirements, design, development, testing, operations) and
  **enabling technologies** (digital twins, AI-driven adaptation) that support
  runtime monitoring, fault detection, and automated decision-making, and
  consolidates the open challenges — verifying adaptive behaviour under
  uncertainty, balancing adaptability against performance and safety, and
  integrating established self-adaptation frameworks such as MAPE-K/MAPLE-K —
  into a roadmap toward 2030. **Provenance, stated plainly:** this entry reached
  the scan through an **arXiv record update on 2026-09-25** that added the
  journal reference, not through a new preprint; the paper itself is the v3
  revision of 2026-05-06 (v1 2025-05-26). It is included as a conceptual/agenda
  contribution surfaced by that metadata change, and is labelled as such in the
  README. No artifact is expected for an agenda; the companion site publishes
  extended Tables 1–2 under CC-BY-4.0. No experiments, no robot results.

## Current Landscape additions

Three bullets added to `README.md`:

1. Deployed navigation stacks are publishing their runtime contract (measurement
   validity and timing, quality gates, command arbitration, watchdogs) rather than
   only tracking error — the field-validated instance of the middleware-harness
   argument the list already carries.
2. Safety is being factored out of the policy and conditioned on the body
   (CrossSafe's shared reachability filter over a morphology-aware latent), while
   the embodied trust boundary extends from the action path to the learning
   channel (TRACE).
3. Simulator search traces are becoming reusable, auditable design memory
   (Auto-Robotist), the robot-design analogue of the trace-to-skill line already
   curated (ASPIRE, Evo-Harness, HCL).

## Rejected / watch list

### Newly screened this pass and rejected

- **NVIDIA OmniDreams / Cosmos-Dreams** (`2606.03159`) — real-time generative
  world model for **closed-loop autonomous-vehicle simulation**, with a live
  Apache-2.0 repository (`nv-tlabs/omni-dreams`, 342 stars; now retitled
  "NVIDIA Cosmos-Dreams (fka NVIDIA OmniDreams)") and public model weights.
  Excluded by the standing scope rule for generic driving: the artifact is real
  and strong, but the contribution is a driving-simulation world model rather
  than a robot harness, recovery, safety, or evaluation contract, and the
  robotics-relevant member of the family (Cosmos-H-Dreams) is already curated.
- **Representation World Model (RWM)** (`2609.29171`) — the comment links
  `tsinghua-mars-lab.github.io/RepresentationWorldModel`, which returns
  **HTTP 404** ("Site not found"). Treated literally as no artifact located;
  excluded as a world-model/planning-in-representation contribution with a dead
  project link.
- **An Analysis of Streaming Deep RL for Adaptive Continual Learning in Robotics**
  (`2609.28807`) — a real, large code release (`tjvitchutripop/stream-rl-robotics`,
  759 MB, ManiSkill tasks, pretrained PPO checkpoints, pushed 2026-09-23) but
  **no license asserted**, and the contribution is a learning-algorithm result
  (streaming actor–critic with AdaptiveObGD/ObGD versus batch PPO under goal,
  friction, and joint-damage perturbations) rather than a harness or runtime
  contract. Excluded as a training-recipe paper under the standing rule, despite
  the genuine artifact.
- **TAPESIM** (`2609.28766`) — tape-dispensing simulator with useful physics-step
  speedups and real-motion replays, but the paper states "We will release the
  source code" — **not released** — and the contribution is simulator fidelity
  for one material interaction. Excluded pending a release.
- **A Simple Gripper Interface for Simulator-Agnostic Cloth Manipulation**
  (`2609.29340`) — the "simulator-agnostic" framing is attractive, but the
  artifact is a grasping model (pose, jaw state, attached grasping volume) with
  Google Drive links and one real cloth-fold demonstration; it is a manipulation
  method, not a simulator or harness interface. Excluded.
- **From Passive Execution to Active Exploration** (`2609.29091`) — 1st Place in
  the CVPR 2026 GigaBrain Challenge; a planning/perception/execution agent with a
  fine-grained perception–execution interleaving strategy for a realistic
  Find-and-Place task, but **no artifact of any kind** was located (no full-text
  HTML rendition, no repository, no project page) and the reported evidence is a
  single challenge task. **Watch** — it is the competition-side counterpart of
  the GigaBrain-0.7/WBC line already watch-listed, and a release would make it a
  candidate.
- **DA-GRD** (`2609.29065`) — grasp-pose recovery under a perception-to-execution
  mismatch, selecting tactile probes until the remaining hypotheses support a
  common executable grasp (84.7% lift / 57.3% task-conditioned success in MuJoCo
  over ten objects; 71.7%/38.3% on six real objects, 4.13–4.20 probes versus a
  fixed 15). A genuine recovery contract, but no artifact located, and it sits in
  the same "recovery line without a released interface" position as AquaMend
  (2026-09-25) and Body-Grounded Replanning. **Watch.**
- **Visual Representation and History Modeling for Navigation World Models**
  (`2609.29555`) and **PolyUMI-adjacent perception/collection** items that are
  otherwise model or representation contributions were excluded by the standing
  scope rule.
- **PUBG Ally** (`2609.29837`, 55 pages) — a conversational **embodied** agent
  that plays a battle-royale game as a voice teammate, with latency-constrained
  perception/action. Despite the "embodied" framing and substantial systems
  engineering, the environment is a game, not a robot or a physically grounded
  simulator, and no robot-relevant contract is contributed. Excluded as a digital
  game agent.

### Carried watch list — unchanged this window

Carried forward without status change (none appears in this block; checked by
ID): vla.simd (`2609.24274`), LIBERO-VPro (`2609.24350`), ARSTAG (`2609.24563`),
React When You Need To (`2609.22587`), SafeStage (`2609.21223`), AWM-3DFM
(`2609.21502`), Toollery (`2609.22218`), CHART (`2609.22247`), DTOC
(`2609.26121`), MATE (`2609.26520`), HABILIS Brain 0 (`2609.25558`). The
artifact placeholders recorded on 2026-09-25 (Maithili/GAP's 1 KB repository,
HEXIS's HTTP 401 anonymous link, the RoboRecover dataset "being prepared for
Hugging Face", and HarnessPAI's "not yet publicly released" code) are unchanged.

## Validation performed

- `git diff --check` clean (no whitespace errors, no conflict markers).
- Markdown structure re-checked after every insertion: each new entry is a single
  top-level bullet with balanced brackets and parentheses and balanced emphasis
  markers (scripted check over all 463 `- [` lines returned zero imbalance);
  section headings (40) and the Contents list are unchanged.
- Every added link was fetched on **2026-09-26**: **16 of 16 URLs added to
  `README.md` in this run return HTTP 200** (log:
  `.scratch/arxiv-2026-09-26/link-check.txt`). The only non-200 URL recorded in
  this record is the RWM project page, deliberately reported as a 404.
  Repository licenses, sizes, creation and push dates, default branches, and
  top-level trees were read from the GitHub API; project pages and raw READMEs
  were fetched directly.
- Dates checked for cross-file consistency: the README badge, the
  "Last verified" line, and this record all carry **2026-09-26**.
- Categories checked against the arXiv record for every included entry.
- Cross-file consistency: each of the four Current Landscape bullets refers only
  to entries now present in the main list, and the provenance caveats stated in
  the README (PolyUMI's anonymous mirror, Causal-Reasoner's missing license,
  CrossSafe's unreleased code, the TOSEM agenda's metadata-update provenance) match
  this record.

## Operational notes and blockers

- `/tmp` is per-command in this sandbox, so harvest XML, parsed JSON, dossiers,
  full texts, and logs were kept under `.scratch/arxiv-2026-09-26/` (untracked,
  never staged).
- The first delta run reported 33 changed records; the entity-normalisation
  control described in Scope showed all 33 were artefacts of this run's parser.
  Any later comparison between runs should carry the same control.
- No GitHub API rate limit, network, authentication, or conflict blockers; the
  OAI-PMH endpoint, export API, GitHub API, and all project pages were reachable.

## Commit

Task-owned files committed on `main`: `README.md`,
`sources/daily-arxiv-2026-09-26.md`. Unrelated user changes
(`docs/reference-architecture.md`, `docs/ring-harness.png`, `handoff.md`,
`.scratch/`) were left untouched and unstaged. No branches, no pull requests,
no force-push.
