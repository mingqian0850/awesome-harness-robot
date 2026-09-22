# Daily arXiv scan — 2026-09-22

## Scope

- **Interval:** everything announced after the 2026-09-21 run's cutoff. That run
  (started ~06:36 UTC on Monday 2026-09-21) screened the announcement block whose
  OAI datestamp is **2026-09-21** — 708 unique base IDs, newest base ID
  `2609.22086`. This run started ~17:08 UTC on **Tuesday 2026-09-22** and covers
  everything announced since.
- **A new, much larger announcement block landed.** The OAI-PMH harvest for
  `from=2026-09-22&until=2026-09-22` returned **1,736 unique base IDs, every one
  carrying datestamp 2026-09-22**. This is the Monday-night (US Eastern)
  announcement: it clears the 2026-09-19/20/21 submission days plus re-announced
  replacement versions, and it is the largest single block screened in this
  repository's recent history (the previous block was 708 IDs).
- **Wider window, for the previous block and the delta:** `from=2026-09-21&until=2026-09-22`
  returns **2,427 unique base IDs**, split as **691 carrying datestamp 2026-09-21**
  and **1,736 carrying datestamp 2026-09-22** (no other datestamp is present in
  the window).
- **New-block composition (1,736 unique IDs), by listed category:** cs.LG 675,
  cs.AI 626, **cs.RO 314**, cs.CV 398, cs.CL 367, plus cross-listed stat.ML 75,
  cs.CR 54, cs.SY 52, eess.SY 52, cs.IR 42, cs.SE 40, cs.HC 40 among others.
  **490 records carry pre-`2609.` IDs**, the signature of replacement versions.
- **Composition by the submission date of the announced version** (OAI
  `<created>`): **594 carry 2026-09-21**, 388 carry 2026-09-20, 356 carry
  2026-09-19, 128 carry 2026-09-18, and the remainder reach back through August
  and July to 2022 — the tail is re-announced replacement versions (including the
  oldest ID in the block, `1812.09632`).
- **Delta against the previous run's screened window (exact).** Previous window
  (2026-09-18…2026-09-21) 1,578 unique IDs → this window (2026-09-21…2026-09-22)
  2,427 unique IDs. **1,691 IDs appeared, 842 disappeared**, and 117 records
  changed in at least one parsed field. The disappearances are a **window shift,
  not retractions**: 870 records carried datestamp 2026-09-18 in the previous
  window, 28 of them moved their datestamp into the new window, and the remaining
  842 are simply outside `from=2026-09-21`. Of the previous run's 708-record
  2026-09-21 block, **691 still carry datestamp 2026-09-21 and 17 were
  re-datestamped to 2026-09-22**; all 17 are unrelated to this list's topics
  (medical, agricultural, battery, and speech papers), with two exceptions that
  were already screened or already excluded (`2609.21449` ME-Dex 1.0, tactile
  WAM, no artifact located; `2609.22000` RecreationWorld, watch-listed on
  2026-09-21 for an unreachable release). Only **45 of the 1,736 block IDs were
  present in the previous run's window**, so **1,691 block IDs are new to
  screening**.
- **ID frontier moved and is not truncated.** Newest base ID in the block is
  **`2609.25001`** (the previous run's frontier was `2609.22086`). Direct
  existence probes: `2609.25002` returns **HTTP 200** (assigned, but absent from
  both harvested windows — most likely a later-datestamped or out-of-set record),
  while `2609.25010` and `2609.25050` return
  **HTTP 404**, so the harvest reached the assigned frontier and nothing is being
  held back.
- **Export-API cross-check.** With the range properly URL-encoded,
  `submittedDate` windows return `opensearch:totalResults` for **2026-09-21**
  (cs.RO 73, cs.AI 142, cs.CL 59, cs.CV 116, cs.LG 115) and **2026-09-18**
  (cs.RO 99, cs.AI 126, cs.CL 63, cs.CV 99, cs.LG 125), confirming Monday's
  submissions are indexed. The **2026-09-22** window returns **0 for all five
  categories**: today's submissions are announced in the OAI block but not yet
  indexed by the export API, exactly the announcement-versus-indexing lag seen in
  previous runs. The export API is therefore used here strictly as a boundary
  check; the OAI harvest is the authoritative source.
- **Withdrawals in this block: four** — `2609.16639`, `2609.14159`, `2601.14286`,
  `2606.14053`, each carrying an explicit withdrawal comment. None appears
  **anywhere in `README.md`, `docs/`, or `sources/`** (checked 2026-09-22), so **no
  curated entry is affected**.
- **43 of the 1,736 block IDs were already mentioned** somewhere in `README.md`,
  `docs/*.md`, or `sources/*.md`; all were re-checked. Nine of them changed at
  least one parsed field. Two are worth recording: `2608.17153` was **retitled**
  (old title "Towards Safer RAG: Only Agents Capable of System 2 Thinking may
  Access Untrusted Documents" → new title "Reasoning Reduces the Influence of
  Poisoned Context in RAG"), and `2608.17209` (Teach and Grow) changed its
  project page from `hear.irmv.top` to `tgl.changnie.top` and dropped its IJRR
  acceptance note. **Neither is in the curated README** — both appear only in the
  historical daily records (`sources/daily-arxiv-2026-08-19.md`) — so no README
  correction was required. The remaining changed curated-in-sources records
  (`2609.19923` Co-VLA, `2609.20761` Agile-WAM, `2609.20812`, `2609.10706` HuRo,
  `2609.20819`, `2609.21562`, `2609.22000`, `2609.18462`, `2609.16597`) also
  live only in daily records; their changes are abstract additions (e.g. a
  project-website sentence for Co-VLA), comment updates, or datestamp moves.

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`) for two windows: `from=2026-09-22&until=2026-09-22`
   (the new block) and `from=2026-09-21&until=2026-09-22` (previous block plus new
   block). Pages saved to `.scratch/arxiv-2026-09-22/oai-{new,full}-*.xml`.
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`.scratch/arxiv-2026-09-22/oai-records-{new,full}.json`, `parse_oai.py`).
3. **Delta against the previous run's parsed harvest**
   (`.scratch/arxiv-2026-09-21/oai-records-full.json`): set difference for
   appeared/disappeared IDs plus field-wise comparison for content change
   (`delta.json`), and a separate datestamp-movement analysis to separate window
   shift from retraction.
4. **Export-API cross-check** over the 2026-09-18, 2026-09-21, and 2026-09-22
   `submittedDate` windows per category, used only as a subset/boundary check
   (`export-crosscheck.sh`; the 2026-09-22 window is announced but unindexed).
5. **Two full passes over the 1,736-record block with different ranking emphases.**
   Pass 1 (`analyze.py`) ranked by artifact-signal language (GitHub / Hugging
   Face / GitLab / project page / "we release" / "code available") and by concept
   breadth (harness, VLA, robot foundation, world action model, code-as-policy,
   skill, memory, recovery, evaluation, safety, agentic), and additionally listed
   **every record in the block mentioning "harness"** and **all 314 cs.RO
   records**. Pass 2 (`rescreen.py`) re-ranked the whole block by lifecycle and
   contract vocabulary instead — release lifecycle, runtime
   monitor/intervention, verification/audit/provenance, failure
   diagnosis/recovery, evaluation protocol/benchmark contract, skill
   library/discovery/evolution, robot deployment/integration — and produced
   tier lists (`.scratch/arxiv-2026-09-22/tiers.json`: 64 "core + artifact", 190
   "core only", 56 "robot + artifact"). An independent third screen by this run
   re-ranked the block by harness/world-action/code-as-policy/recovery phrases in
   titles and cross-checked its hits against the two earlier passes: no
   shortlisted record was missed, and the only new names surfaced
   (`2607.18840` WorldScape Policy 2.0, `2609.22332` AffordanceWAM,
   `2609.21449` ME-Dex 1.0, `2609.22792` SelfOp) were checked and rejected for
   scope or absent artifacts.
6. **Dossiers and primary-source verification** for every shortlisted record
   (48 dossiers): full title/abstract/comments, the arXiv abstract page (version
   history and submission dates), the arXiv full-text HTML (artifact links and
   release sentences extracted), project pages fetched and inspected directly,
   and the official GitHub/GitLab/Hugging Face APIs (license field, size,
   creation and push dates, default branch, top-level tree) — all on
   **2026-09-22**. Repository trees, READMEs, changelogs, and Hugging Face
   file counts were inspected for the finalists.
7. **Duplicate screening** of every candidate against `README.md`, `docs/*.md`,
   and `sources/*.md` by arXiv ID, project name, and repository URL before
   writing.
8. **Watch-list re-check** of the artifact placeholders recorded by the two most
   recent runs (SafeStage, AWM-3DFM, and the new 404s found in this block).

## Included (README updates)

Twelve entries added, plus five "Current Landscape" bullets. Artifact status is
reported literally; every repository, dataset, and project-page claim below was
verified live on **2026-09-22**.

### Harnesses and Development Platforms — General Harness Design and Self-Improvement

- **RRSI: Regularized Recursive Self-Improvement of Agent Harnesses**
  (`2609.24972`, cs.AI/cs.CL/cs.LG; v1 announced 2026-09-22, submitted
  2026-09-21): the strongest general-harness contribution in the block. It
  reframes harness evolution as adaptive empirical optimization over a *reused*
  evolve set and regularizes the **search trajectory** rather than the edit space:
  proposal-side annealed edit budget (L0-style sparsity), evidence-aware credit
  assignment over the full edit history, and stall redirection toward unexercised
  components; selection-side leakage critic, noise-adjusted acceptance floor,
  cost rule requiring added inference tokens to be paid for by measured gain, and
  L1-style pruning of unproductive components. Candidates are drafted, screened,
  and evaluated in git worktrees, so the incumbent harness is always a commit and
  the edit history records component, hypothesis, score and cost change, and
  verdict. Across eight benchmarks in three domains (Terminal-Bench 2.1 /
  SWE-bench Verified; Harvey LAB / JobBench / GDPval / APEX-Agents; EngDesign /
  Frontier-Eng) the authors report the *smallest* evolve-set gain of any evolved
  harness but the only out-of-distribution average clearing the unevolved baseline
  by more than a point (43.6 vs. 39.7) at 2.42M tokens/trial against 3.80M for
  unregularized evolution; the harness transfers to a second policy family and to
  a weaker backbone the search never used (Gemini 3.1 Flash Lite 11.2→14.6).
  **Verified open:** the Apache-2.0 `google-research/rrsi` repository is live
  (created 2026-09-16, pushed 2026-09-22, 76 stars) with `rrsi.py`, the `rrsi/`
  package, three domain adapters, `third_party/`, and tests; the project page
  `https://regularized-rsi.com/` returns HTTP 200. Digital agents only; no robot
  experiment; results author-reported.
- **Harness-Zero: Harness Distillation via Agent-as-Harness** (`2609.24974`,
  cs.AI/cs.CL/cs.NE; v1 announced 2026-09-22, submitted 2026-09-21): asks what
  survives when an optimized harness is removed at deployment. A *harnessing
  agent* reads the optimized harness's guidance and rewrites the student's
  proposed responses **before execution** in the target harness's action space;
  the reviewed rollouts become SFT data, so harness-induced behaviors move into
  the weights. Reported results: agent-as-harness beats code-as-harness for
  frontier models (81.1% vs. 78.1% averaged over six benchmark–model settings);
  distilling into Qwen3.5-9B raises macro-average task success from 23.3% to
  44.3% under a minimal target harness, above the 41.7% the base model reaches
  with the evolved harness still attached; the distilled model recovers 82.3% of
  28 harness-exclusive behavior patterns spanning memory, skill, tool, and
  middleware sources; ablations attribute the gain to harness-guided review (30%
  vs. 3–15% for alternative supervision), and direct distillation from teacher
  trajectories fails under action-space mismatch. **Verified open:** Apache-2.0
  `metaevo-ai/harness-zero` is live (created 2026-09-20, pushed 2026-09-22) with
  `src`, `harness_bank`, `envs`, `data`, and `tests`. Digital-agent domains
  (SpreadsheetBench Verified, AppWorld, USPTO Retrosynthesis); no robot
  experiment; results author-reported.
- **Self-Healing Harness for Runtime Oversight of Agent Self-Modification**
  (`2609.24130`, cs.AI; v1 announced 2026-09-22, submitted 2026-09-21): makes
  self-modification an **admission-control** problem. The agent authors candidate
  behavioral rules into an external workspace where they receive provisional
  execution authority during evaluation and acquire persistent cross-episode
  authority only after measured improvement on the triggering failure without
  regression beyond a fixed margin on protected cases; replay provides matched
  evidence, forward trials are the weaker fallback, and a corpus-level guard
  re-tests the accumulated active rule set. In 16 matched Baseline/Harness runs
  spanning AppWorld, Terminal-Bench, and τ²-Bench the authors report the gate
  rejected 383 replay-decided proposals, of which **211 (55%) improved their
  triggering failure while degrading a case that previously worked**; task
  completion is higher under the harness in all 16 pairs (two paired bootstrap
  intervals excluding zero) and reliability is higher in 12, tied in 4, lower in
  none. **Artifact status:** no first-party artifact located as of 2026-09-22 —
  the tracing/evaluation infrastructure is the third-party Apache-2.0
  `chirpz-ai/pandaprobe` platform (created 2025-12-16, 786 stars), and no
  repository for the harness itself appears on the arXiv record or in the full
  text. Digital agents only; no robot experiment; results author-reported.

### Robot Agent Systems — Agentic Robot and VLA Harnesses

- **CARE: Experience-Guided Atomic Corrective Execution for VLA Policies**
  (`2609.24118`, cs.RO; v1 announced 2026-09-22, submitted 2026-09-21): recovery
  supervision from real failures rather than hand-designed or random
  perturbations. Failed rollouts are collected, stage-conditioned post-failure
  deviations modeled, and the empirical distributions used to synthesize
  representative failure states with corrective demonstrations; at inference,
  stage-wise planning plus physically grounded 3D monitoring triggers atomic
  adjustments or re-operation while preserving task progress. The paper
  introduces FSR-Bench (Failure State Recovery Benchmark) for intermediate
  failure states under local deviations and structural anomalies. The authors
  report average task-success gains of 14.5 points in simulation and 15.9 points
  on real dual-arm tasks across multiple VLA backbones. **Verified open:** the
  MIT-licensed `xiaojunlan/care` repository is the abstract's official link and
  is live (created 2026-09-17, pushed 2026-09-17, 36 stars), describing
  corrective data generation and FSR-Bench evaluation. Real-hardware evidence is
  author-reported.
- **RAPolicy: Stable and Efficient Real-World Online VLA Post-Training**
  (`2609.22888`, cs.RO; v1 announced 2026-09-22, submitted 2026-09-19): treats
  the online rollout-and-learn loop itself as the deployable artifact. Rollout
  and learning run concurrently while critic and actor updates are anchored in
  replayed behavior: the critic learns chunk-level values from recorded actions
  and builds Bellman targets without predicting next actions, and the one-step
  flow actor reuses the rollout's stored initial noise while learning through
  advantage-weighted conditional likelihood, so the executed action mapping is
  the supervised one. Starting from policies fine-tuned on 10 demonstrations per
  task, the authors report 1–2 hours of real-world online training reaching 86.3%
  average success across four single-task settings and 52%→88% on a joint
  five-task setting, with fewer human interventions. **Verified open:** the
  Apache-2.0 `flyfaerss/RAPolicy` repository was created and pushed 2026-09-21
  with a reproduction section (~198 MB including assets), and the project page's
  Code link resolves. Real-robot evidence, author-reported.

### Robot Foundation and World Models — World and Physical-Reasoning Models

- **D-JEPA: A Decision-Aligned Latent World Model** (`2609.24749`, cs.LG/cs.RO;
  v1 announced 2026-09-22, submitted 2026-09-21): identifies a *decision-local
  prediction gap* — among the few futures competing for execution, a candidate
  predicted closer to the goal can realize a worse outcome, so predictive
  accuracy alone does not order actions correctly. D-JEPA learns
  decision-relevant relations among candidate futures from executed outcomes with
  a bounded, permutation-equivariant operator over goal-relative predictive
  features and ordinal evidence, refines pretrained predictive geometry only
  where choices are consequential, and realizes the learned structure in
  JEPA-compatible latent distances so planning stays native. The authors report
  87.89% success on PushT, +15.04 points average on RoboTwin, +17 points on
  physical robot tasks, plus autonomous-driving evaluation. **Verified open:**
  Apache-2.0 `NEBULIS-Lab/D-JEPA` (created 2026-08-31, pushed 2026-09-21, 6 stars)
  ships `docs/ROBOTICS.md`, `docs/DRIVING.md`, `docs/PROTOCOLS.md`,
  `docs/REPRODUCING.md`, and a `real_robot/` path; public weights
  (`Shuaijun/D-JEPA`, 39 files) and dataset (`Shuaijun/D-JEPA-Dataset`, 8 files)
  exist on Hugging Face. Results author-reported.
- **Robot World Models Are Not Invariant to How the Actions Are Written**
  (`2609.23252`, cs.LG/cs.RO; v1 announced 2026-09-22, submitted 2026-09-19): an
  interface audit rather than a model. A latent dynamics model trained under one
  action parameterization (absolute joint targets vs. deltas) and handed the
  identical commanded trajectory in the other collapses: retrieval degrades
  2.6–13.4× across three robot datasets and two morphologies, goal-conditioned
  action selection falls from 53% to 15%, and on PushT the two beliefs about the
  same future are near-orthogonal (cos = 0.067, worst case −0.377) — even though
  the encodings are mutually reconstructible at R² = 0.996, so no information is
  lost. The paper gives the test that separates a valid re-parameterization from
  a lossy summary or a sensor swap, and that test rejected three of the four axes
  proposed. The repair is averaging over the two encodings, and where it is
  applied matters: objective averaging restores task performance by itself,
  output averaging is unavailable for direction-valued prediction, and a
  disagreement penalty closes the residual worst-case agreement gap
  (0.78 → 0.995). **Artifact status:** no official artifact located as of
  2026-09-22. Measurement study over existing policies and datasets;
  simulation/offline evidence; author-reported.

### Benchmarks and Evaluation — Manipulation and VLA

- **ActiveArena: Benchmarking and Understanding Active Perception in Robotic
  Manipulation** (`2609.24124`, cs.AI/cs.RO; v1 announced 2026-09-22, submitted
  2026-09-21): a simulator–benchmark–baseline loop. ActiveArena-Sim provides
  controllable viewpoints and large-scale workspaces; ActiveArena-Bench adds 35
  tasks across 5 categories that cannot be solved from passive observation,
  requiring multi-round evidence acquisition and memory-based reasoning, with
  memory annotations, standardized training data, and ID/OOD protocols over
  disjoint scenes, unseen distractor configurations, and novel backgrounds;
  ActiveArena-VLA is a modular suite of 13 VLA configurations isolating memory
  writing, memory capacity, proprioceptive state, subtask supervision, and
  high-level planning. The authors report a substantial ID–OOD gap, with uniform
  memory sampling, more capacity under reliable write policies, proprioception,
  and subtask supervision improving OOD generalization while planner-guided
  memory management approaches the best configuration with sparse memory.
  **Verified open:** MIT-licensed `leeibo/ActiveArena` (19 stars) and
  `leeibo/ActiveArena-VLA` (both pushed 2026-09-22), a 3,677-file public dataset
  (`leeibo/ActiveArena-Data`), and released checkpoints (`leeibo/ActiveArena-VLA`,
  17 files). Simulation benchmark; no real-robot evaluation.
- **Anatomy of a Closed-Loop Collapse: A Causal Case Study of a Compressed VLA
  Policy** (`2609.23048`, cs.LG/cs.RO; v1 announced 2026-09-22, submitted
  2026-09-19; accepted as a poster at the IROS 2026 ScaleInfra workshop): an
  operational counterexample to offline acceptance testing. An 8-layer
  distillation of Octo-Base keeping 86% of parameters passes every offline check
  applied (0.996 and 1.000 teacher-ratios on the family's own validation metrics)
  and then scores 0/72 against the teacher's 40/72 in closed-loop simulated
  WidowX pick-and-place. The failure is structured rather than diffuse — early
  stages degrade gradually while transport-to-target fails categorically at 0% in
  every training variant — and paired action-trace forensics isolate a negative,
  late-heavy z residual about 10× its post-repair magnitude. Four standard
  therapies fail under matched controls (continued training, in-domain offline
  data, command-level compensation, clamping the symptom), while a minimal-pair
  intervention substituting half the training stream with deployment-distribution
  teacher rollouts restores parity (18/36 vs. 17/36 held out) and removes the
  signature. The authors claim existence, not universality. **Verified open:** the
  MIT-licensed `Xanadum/closed-loop-collapse` repository holds the supplementary
  records (created 2026-09-19, pushed 2026-09-22). Simulation-only evidence;
  author-reported.

### Datasets and Data Infrastructure — Multi-Robot and Foundation-Model Data

- **REBOOT: From Failure to Recovery — A Dataset and Benchmark for Precision
  Assembly** (`2609.22591`, cs.RO; v1 announced 2026-09-22, submitted
  2026-09-18): failure as a first-class signal. 2,160 demonstrations over 18
  precision-assembly tasks, each split into five shared phases (Align(pick),
  Engage(pick), Transport, Align(place), Engage(place)); failures are injected
  across phases and paired with expert recovery trajectories that return the
  system to a valid continuation state. Matched install–remove pairs are
  annotated with rotational symmetry, engagement-clearance precision tier, and
  assembly direction; failure episodes carry phase and categorical failure-mode
  labels, enabling attribution to kinematic stage and tolerance violation; data
  includes synchronized RGB-D from four viewpoints with grounded natural-language
  phase conditions, and half the episodes are recoveries sampled from
  imitation-policy rollouts. The authors benchmark action-chunked transformer,
  diffusion, and π0-FAST policies with phase-level completion rates and report
  model-specific failure points that binary success hides. **Artifact status
  (literal):** the University of Calgary project page is live and the dataset is
  public as **38 LeRobot-format datasets** under the Hugging Face `REBOOT26`
  account (task-named install/remove/recovery splits, last modified March–May
  2026); the project page's code link points to an **anonymous** GitHub account
  (`anon-robo-account/REBOOT`, an Apache-2.0 LeRobot fork with depth
  recording/playback, 118 MB, last pushed 2026-08-21), so the code repository's
  identity is not yet confirmed first-party. Real-robot demonstrations;
  benchmark results author-reported.

### Runtime, Safety, and Observability — Runtime Building Blocks

- **vla.cpp: A Unified Inference Runtime for Vision-Language-Action Models**
  (`2606.08094`, cs.AI/cs.LG/cs.RO/cs.SY/eess.SY; **v2 announced 2026-09-22**,
  v1 was 2026-06-06): the substantive revision of the block. One C++/ggml runtime
  (built on llama.cpp) executes eleven VLA policies — SmolVLA, π0/π0.5, BitVLA,
  Evo-1, GR00T N1.5/1.6/1.7, VLA-Adapter and others — each packaged as a
  self-contained GGUF with **no Python or PyTorch at inference**, sharing model
  loading, tensor execution, and serving while retaining architecture-specific
  attention, conditioning, and action heads; iterative policies reuse
  observation-dependent computation across solver steps. Targets span CPU, Apple
  Silicon, CUDA down to Jetson-class boards, and Intel GPU/NPU through SYCL and
  OpenVINO; the authors report LIBERO-Object task success plus profiling on
  NVIDIA, Apple, and Intel hardware, and the changelog documents an OpenVINO
  backend matching an F32 CPU reference to 1e-3 on ten of the eleven
  architectures. **Verified open:** Apache-2.0 `VinRobotics/vla.cpp` (created
  2026-05-26, pushed 2026-09-22, 198 stars) with `src`, `include`, `bindings`,
  `eval`, `docs`, `tests`, `CMakeLists.txt`, and CI; a public model collection at
  `huggingface.co/vrfai`; and a live documentation site
  (`fai-modelopt-tech.github.io/learn-vla-cpp/`). Simulation benchmark plus
  hardware profiling; no real-robot deployment numbers; author-reported.

### Runtime, Safety, and Observability — Safety

- **BadWAM: When World-Action Models Dream Right but Act Wrong** (`2607.15207`,
  cs.LG/cs.RO; **v2 announced 2026-09-22**, v1 was 2026-07-16): attacks the
  assumption that coupling action generation to future prediction lets a robot
  check what it does against what it imagines. BadWAM defines World-Action Drift
  Attacks — small visual perturbations that break alignment between imagined and
  executed futures — parameterized by attack strength and stealthiness: an
  action-only attack that drives task-failing actions, and an
  imagination-preserving attack that shifts actions while keeping the predicted
  future close to the clean imagination. Across WAM variants the authors report
  closed-loop success falling from 96.5% to 43.1% under the action-only attack,
  and that moderate future-preserving regularization retains strong attack
  performance while reducing imagination drift — so imagination–action agreement
  is not by itself a sufficient runtime check. **Verified open:** the MIT-licensed
  `LiQiiiii/BadWAM` repository (created 2026-07-10, pushed 2026-07-17, 54 stars),
  the live project page (`liqiiiii.github.io/BadWAM`), and a public Hugging Face
  collection (`LIQIIIII/badwam`, last updated 2026-08-30). Author-reported;
  evaluated in closed-loop simulation, not on hardware.

## Rejected / watch list

Everything below was read against the arXiv record, the full-text HTML, and
(where a link existed) the live project page, repository API, or raw repository
content on 2026-09-22.

### Verified and rejected — artifact is a placeholder, missing, or 404

- **vla.simd: Efficient CPU Inference for Language-Conditioned Manipulation**
  (`2609.24274`, cs.AI/cs.RO/cs.SY/eess.SY): a genuinely relevant CPU inference
  engine for six policies with shared SIMD micro-kernels and an IMPACT policy with
  cached text representations, and the strongest runtime near-miss of the block.
  Its project page (HTTP 200) states plainly: **"Code and checkpoints will be
  released on acceptance."** The page's reported numbers (e.g. 65–90% success on
  SO-101 tasks at ~996 ms) rest on checkpoints that are not public. Treated
  literally as **not open**. **Watch for the release**; this would be an immediate
  inclusion once code and checkpoints appear.
- **LIBERO-VPro: Benchmarking Closed-Loop Visual Robustness of Robotic Foundation
  Models** (`2609.24350`, cs.CV/cs.RO): a well-structured robustness benchmark
  (4 dimensions, 12 challenge categories, 96 settings, 3,296 task-condition
  cases) and close to this list's evaluation contract. The project page is live
  but its artifact links are **"Dataset (soon)"** and no code repository is
  offered. **Not open**; **watch**.
- **ARSTAG: An Agentic Real2Sim2Real System for Task-Specific Robot Data
  Generation** (`2609.24563`, cs.RO): a hierarchy of language agents builds
  task-scoped simulation scenes and generates demonstrations from one RGB image
  plus an instruction. The project page links `github.com/boweili666/ARSTAG`, but
  that repository is **the project website itself** (description: "Project website
  for ARSTAG", no license, 56 MB of assets, pushed 2026-09-19) — no code artifact
  is published. **Watch**.
- **React When You Need To: Event-Triggered Asynchronous Inference for VLA
  Policies** (`2609.22587`, cs.RO): event-guided dynamic inference for action-chunk
  policies with a strong reported margin (95% average success, +55 points over the
  strongest baseline). The project page states **"The code will be made publicly
  available upon acceptance."** **Not open**; **watch**.
- **Uranus: Building the Next-Generation Simulation Infrastructure for Embodied
  AI** (`2609.24815`, cs.AI/cs.RO): a large simulation-infrastructure claim whose
  advertised SDK repository `github.com/D-Robotics-AI-Lab/Uranus-SDK` returns
  **HTTP 404**, as does the advertised `cair-vinuni/FoldQuantVLA`
  (`2609.24433`, native low-bit VLA quantization). **Not open**; **watch**.
- **SafeStage** (`2609.21223`, cs.RO) — re-checked from the 2026-09-21 watch list:
  the repository `JinzhuLuo/SafeStage` is **no longer empty but is still a stub**:
  it contains only a 114-byte `README.md` with the paper title, no license and no
  code. The 2026-09-21 verdict (treated literally as not open) **stands**; the
  three-stage safety benchmark remains the strongest un-released idea in this
  area and is worth re-checking on later runs.
- **AWM-3DFM** (`2609.21502`, cs.CV/cs.RO) — re-checked: `dtc111111/AWM-3DFM`
  still returns **HTTP 404**. Watch status unchanged.
- **Toollery: Scaling LLM Agents to Thousands of Skills and Tools**
  (`2609.22218`, cs.AI/cs.CL/cs.LG): training-free candidate compression for
  large skill/tool libraries, relevant to the skill-selection contract. The
  linked repository `XiangxiTian/toollery` does implement the paper's workflow,
  but it **asserts no license**, has 0 stars, was created 2026-05-12 (months
  before the preprint) and last pushed 2026-08-31, so first-party provenance is
  unconfirmed. **Watch**; not promoted to an entry on this evidence.
- **CHART: A Harness-Rotation Curriculum for Harness-Robust Search Agents**
  (`2609.22247`, cs.AI/cs.LG): directly on-topic (training a search agent to
  survive harness rewrites) and a good conceptual fit, but **no first-party
  artifact was located** — the only repository/dataset link in the full text is
  the MiroMind `MiroVerse-v0.1` dataset used for training, not a release of the
  curriculum or the harnesses. Excluded; **watch** if code appears.

### Verified and rejected — no artifact, out of scope, or both

- **From Capability to Assurance in Autonomous Penetration-Testing Harnesses**
  (`2609.22664`, cs.AI/cs.CR): a five-property assurance framework (evidence,
  scope, auditability, …) that explicitly locates assurance **in the harness**,
  with a content-addressed artifact bundle described and P4/P5 acceptance tests
  executed. The evaluation protocol is stated as future work, the domain is
  penetration testing rather than robots or general agent harnesses, and the
  linked framework (`JoasASantos/NeuroSploit`, MIT, 1,383 stars) is a
  pre-existing tool (created 2025-08-17) rather than this paper's artifact.
  Excluded as out of scope; the assurance-property framing is recorded here for
  later harness-safety work.
- **Trustworthy Agentic AI: Failure Modes, Mitigation Strategies, and a Lifecycle
  Framework** (`2609.22712`, cs.AI), **When the Agent Becomes the Kernel**
  (`2609.23700`, cs.AI/cs.CR/cs.OS), and **DUMA-Bench** (`2609.24662`, cs.AI):
  surveys and security benchmarks about agentic systems in general, with no robot
  component and no harness artifact; DUMA-Bench extends τ²-bench with adversarial
  environments but releases no located artifact. Excluded (scope).
- **APEXA** (`2609.24165`, cs.AI): execution-integrity enforcement ("correctness
  is a property of what executed, not of the transcript") for synchrotron data
  reduction, with a real repository
  (`AdvancedPhotonSource/APEXA-APS-Beamline-Assistant`). The contract is
  interesting, but the system is a scientific-facility automation stack rather
  than a robot or general agent harness. Excluded (scope); noted as an example of
  execution-integrity enforcement outside robotics.
- **DolphinBench** (`2609.24971`, cs.AI/cs.CL) and **AhaBench** (`2609.05435`,
  cs.CL/cs.LG): memory-cost Pareto evaluation and long-horizon continual-learning
  evaluation for language agents. Relevant to memory contracts but no robot
  component and no artifact located in this block beyond benchmark descriptions.
  Excluded (scope); **watch** for dataset releases.
- **Predictors and Orchestrators** (`2609.22251`, cs.AI/cs.LG/cs.MA): an agentic
  harness for karst-aquifer forecasting; a legitimate harness application but a
  domain science result. Excluded (scope).
- **WorldScape Policy 2.0** (`2607.18840`, cs.RO), **AffordanceWAM**
  (`2609.22332`, cs.AI/cs.CV/cs.RO), **ME-Dex 1.0** (`2609.21449`, cs.CV),
  **SelfOp** (`2609.22792`, cs.AI/cs.CR), **Vision2CAD** (`2609.22688`),
  **SyzHarness** (`2609.23889`), **Ascent** (`2609.24620`), **MedRSI**
  (`2609.24838`), **Schematize** (`2609.22209`): surfaced by the independent
  third screen; each is either a world/action-model variant without a located
  artifact, a domain application (CAD, kernel fuzzing, clinical, medical, legal),
  or a security-agent optimizer outside this list's harness contract. Excluded;
  the WAM variants stay on the general watch list for artifact releases.
- **Driving, medical, agricultural, battery, and speech records** in the block:
  excluded per the standing scope policy unless they introduce a reusable
  agent/VLA harness, recovery, safety, or evaluation contract. Two
  near-exceptions were examined and rejected: **DriveReferee** (`2609.22762`,
  cs.CV), a geometric safety-verdict rule for driving world-action models whose
  contribution is real but domain-bound, and **LD-HRI** (`2609.24055`,
  cs.HC/cs.RO), a human-in-the-loop recovery communication benchmark with a human
  corpus but no located release.

## Current Landscape additions

Five bullets added to `README.md`:

1. Harness self-improvement splitting into "does it improve" and "does the
   improvement survive" (RRSI, Harness-Zero).
2. Self-modification as admission control (Self-Healing Harness; the 55%
   collateral-regression figure).
3. VLA deployment infrastructure consolidating around runtime portability
   (vla.cpp, RAPolicy).
4. Failure and recovery becoming labeled artifacts (REBOOT, CARE, ActiveArena).
5. Imagination–action coupling is not a safety guarantee, and offline gates are
   not acceptance tests (BadWAM, D-JEPA, Anatomy of a Closed-Loop Collapse).

## Validation performed

- `git diff --check` clean (no whitespace errors, no conflict markers).
- Markdown structure re-checked after every insertion: each new entry is a single
  list item on one line; every target heading is still preceded by a blank line
  and by a list item; **all VLA table rows still have exactly five columns
  (6 pipes)** — the table was not touched.
- All **36 links added or changed** in `README.md` were fetched on 2026-09-22 and
  returned **HTTP 200** (arXiv abstract pages, project pages, GitHub repositories,
  Hugging Face model/dataset/collection pages, the shields.io badge, and the
  learn-vla.cpp documentation site).
- Date cross-file consistency: the README badge and `Last verified:` line both
  read **2026-09-22**, matching this record's filename and header; the
  `sources/daily-arxiv-*` series remains contiguous (2026-09-21 → 2026-09-22).
- No new arXiv ID appears more than twice in `README.md`, and each of the twelve
  new IDs appears once as an entry (twice only when also cited in a Current
  Landscape bullet, matching the existing convention); no candidate ID was
  already present in the curated list.
- Artifact claims were verified live on 2026-09-22 rather than trusted from the
  papers: repository license, size, creation/push dates, star counts, and
  top-level trees for the ten repositories involved; Hugging Face file counts for
  the four model/dataset artifacts; HTTP status for every project page.
- The block's four withdrawal-marked records (`2609.16639`, `2609.14159`,
  `2601.14286`, `2606.14053`) were checked against the whole repository and are
  absent, so no curated entry needed a withdrawal note.
- The 43 block IDs already mentioned in the repository were re-checked; the nine
  with changed fields live only in historical daily records, so no README
  correction was required.
- Task-owned files only were staged; `docs/reference-architecture.md` (user
  modification), `docs/ring-harness.png`, `handoff.md` (untracked), and
  `.scratch/` were left untouched.

## Operational notes and blockers

- No network, authentication, or conflict blockers. `git fetch`/`push` require the
  documented `GIT_SSH_COMMAND='ssh -F /dev/null'` workaround for the
  `nobody`-owned `/etc/ssh/ssh_config.d` file.
- The GitHub REST API hit its unauthenticated rate limit (HTTP 403) late in the
  run, during the watch-list re-check of two repository pages; those two were
  re-checked over plain HTTPS instead (SafeStage's raw README and file list,
  AWM-3DFM's 404), which is the documented acceptable fallback.
- The export API's `submittedDate` probe silently truncates range bounds unless
  the brackets are URL-encoded; unencoded requests returned empty responses for
  every window, including a control window that has data. Encoding
  `%5B…%5D` fixes it. Worth remembering: an empty export response is not evidence
  of an empty interval — check a control window first.
- The OAI datestamp is the announcement date, not the submission date: this
  block's records were submitted 2026-09-18…2026-09-21 and announced 2026-09-22.
  Treating the block as "today's submissions" would mis-date the entire window.

## Commit

- `README.md` — twelve entries and five Current Landscape bullets added, badge
  and `Last verified` date advanced to 2026-09-22.
- `sources/daily-arxiv-2026-09-22.md` — this record (new).
- Commit message: `Curate September 22 robot harness research`.
