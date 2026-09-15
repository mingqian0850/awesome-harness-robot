# Daily arXiv scan — 2026-09-15

## Scope

- **Interval:** everything announced after the 2026-09-14 run's cutoff. That run
  (started ~07:05 UTC on Monday 2026-09-14) screened the block whose OAI header
  datestamp is **2026-09-14** — 935 set memberships / 697 unique base IDs, newest
  base ID `2609.13146` — covering the 09-10 and 09-11 submission blocks. This run
  started ~04:00 UTC on Tuesday 2026-09-15 and covers everything announced since.
- **A new announcement block landed.** Datestamp **2026-09-15**:
  **2,123 set memberships = 1,563 unique base IDs** across the five target
  categories. Newest base ID **`2609.15989`** (`2609.15990` returns HTTP 404, so
  the frontier is at `2609.1598x`; ID-existence bisect on `arxiv.org/abs/<id>`,
  2026-09-15).
- **Composition of the new block (1,563 unique IDs), by the submission date of
  the announced version** (the OAI metadata `<created>` field, which carries the
  latest announced version's date — semantics re-confirmed this run against
  `abs` submission histories):
  - **1,262 records carry 2026-09-12 (337), 2026-09-13 (338), or 2026-09-14
    (587)** — the Saturday, Sunday, and Monday submission blocks. arXiv announces
    Monday–Friday, so the weekend submissions plus Monday's are released
    together in the Tuesday block; this matches the 09-14 block's structure
    (Thursday + Friday submissions).
  - **133 records carry 2026-09-11**, and **168 carry older dates** — the
    newly announced backlog (held submissions whose ID was assigned at
    announcement) plus cross-lists of older papers.
  - **194 records carry cs.RO.**
- **The export API was again unavailable for the entire run.** Every request to
  `https://export.arxiv.org/api/query` returned **HTTP 429 "Rate exceeded."**
  (14 bytes) — on probes for 20260912, 20260913, 20260914, and 20260915. This is
  the **second consecutive run** with the documented primary query path blocked,
  and is recorded as a blocker for that path, **not** as evidence about the
  index.
- **Substitute evidence (official arXiv infrastructure):**
  - **OAI-PMH**, `https://oaipmh.arxiv.org/oai`,
    `verb=ListRecords&metadataPrefix=arXiv&set=cs:cs:<CAT>&from=2026-09-11&until=2026-09-15`
    for cs.RO, cs.AI, cs.CL, cs.CV, cs.LG; every response was a single complete
    page with no resumption token outstanding: cs.RO **360**, cs.AI **1,213**,
    cs.CL **605**, cs.CV **662**, cs.LG **1,129** = **3,969 memberships =
    2,925 unique base IDs**. Datestamps: 2026-09-11 (926 memberships),
    2026-09-14 (920), **2026-09-15 (2,123)**; no records carry 09-12 or 09-13,
    as expected on a weekend.
  - **ID-existence bisect** on `arxiv.org/abs/<id>` for the frontier.
  - **Direct record fetches** (`arxiv.org/abs/<id>`, `arxiv.org/html/<id>v<n>`)
    for every candidate, plus live artifact checks through the GitHub REST API,
    the Hugging Face Hub API, the **Zenodo API**, project pages, and the
    `abs`-page version histories.
- **Continuity check against the previous run.** The previous run counted 935
  memberships / 697 unique IDs for datestamp 2026-09-14; this harvest sees **920 /
  684** for the same datestamp. A narrower re-harvest
  (`from=2026-09-11&until=2026-09-14`) reproduces 920 memberships / 1,362 unique
  across both datestamps, so the pipeline is internally consistent and the
  13-ID difference is **consistent with records re-announced with new versions
  in this block**: an OAI record carries a single datestamp, which follows the
  latest announcement, so a replacement announced on 09-15 moves out of the 09-14
  group. Four of the re-announcements were identified and read as revisions
  (below); the rest are older-paper replacements that were screened with the
  block.
- **"Unchanged batch" re-screen:** not applicable in the required sense — a
  genuinely new block landed (1,563 IDs, none of which is the previous run's
  batch except the re-announcements above). The **entire** 1,563-record block was
  screened regardless, including the 301 records with pre-09-12 dates that no
  earlier new-submission window covered.
- **Revisions in scope.** The block contains replacement versions the previous
  screen could not see. Each was version-diffed or read before a verdict:
  **No Free Checker v2** (`2609.09250`, 2026-09-13), **vla-eval v3**
  (`2603.13966`, 2026-09-14), **DroneServer v3** (`2601.15486`, 2026-09-11),
  **LIBERO-RECOVER v2** (`2609.05178`, 2026-09-14), and the camera-ready
  re-announcement of **Colosseum V2** (`2605.27759`, 2026-09-12).

## Method

- **Concept sweeps over all 1,563 records** (title + abstract) for twelve concept
  families: harness/scaffold/orchestration/runtime; self-improvement and
  co-evolution; robot agent / embodied agent; VLA; robot foundation and
  world/action models; code-as-policy; skill discovery/selection/generation and
  tool libraries; memory; recovery/rollback/retry/resume; evaluation/benchmark;
  safety/monitoring/verification/permissions/attacks; manipulation. Matches:
  evaluation 1,024, safety 603, memory 215, harness 210, recovery 185,
  manipulation 141, foundation 98, self-improvement 63, VLA 34, robot-agent 18,
  code-as-policy 8, skills 6. Union "strong" pool: **366** records.
- **Full title read of all 194 cs.RO records** and of every record whose title
  contains "harness" (15), then abstract reads of the strong pool and of every
  evaluation/safety candidate that names an artifact, a protocol, or a measurable
  contract.
- **Version-history check** (`abs` submission history) for every included or
  shortlisted candidate; **HTML text diffs** for the four substantive/cosmetic
  revision questions.
- **Duplicate check:** all shortlisted names and IDs were grepped across
  `README.md`, `docs/`, and `sources/`. Results: **`vla-eval` /
  `vla-evaluation-harness` is already curated** from its code (README Unified
  Evaluation and Current Landscape, `docs/landscape.md`,
  `sources/research-notes-2026-07-25.md`) — its paper had never been screened,
  and v3 is cosmetic, so the existing entry was updated rather than duplicated;
  **`No Free Checker` (`2609.09250`) is already curated** (2026-09-10) — v2 is
  substantive, so it was updated in place; **`LIBERO-RECOVER` (`2609.05178`) is
  already curated** (2026-09-07, with code and ModelScope datasets) — v2 is a
  camera-ready formatting revision, so no change. Every other inclusion is a
  first exposure.
- All performance numbers below are **author-reported** unless stated otherwise.
  Artifact status is literal and dated to this run (2026-09-15).

## Included (README updates)

Thirteen new entries, two in-place entry updates, and five Current Landscape
bullets.

### HarnessVLN: Unifying Training-Free Embodied Navigation through an Agent Harness

- Paper: https://arxiv.org/abs/2609.15195 (v1 2026-09-14T08:15:56Z, announced in
  this block; cs.RO; Yang Chen, Lirong Che, Zhenyu Huang, Wenbo Fu, Chuang Wang, Xu Cao, and co-authors, 10 in total).
  Project page: https://harnessvln.netlify.app/ (live, HTTP 200).
- Artifacts: **not open, treated literally.** The project page is an anonymized
  double-blind submission ("Anonymous authors · Paper under double-blind
  review"); its only links are the paper PDF and figures, and the navigation's
  "Code" item carries **no href** (verified 2026-09-15). No code, weights, or
  data release exists on the page or the record.
- Classification: Agentic Robot/VLA Harness.
- Why included: it is an embodied **harness** rather than another navigation
  policy. A zero-shot, training-free MLLM navigator sits behind one unified tool
  interface, and the harness's job is adjudication — planner proposals are
  validated against spatial evidence, geometric feasibility, and subgoal
  consistency, with structured tool feedback folded back into later decisions.
  Two state structures carry the loop: a hierarchical event memory for task
  progress and execution history, and a persistent spatiotemporal graph that
  keeps reusable spatial evidence **and failure annotations** for verification
  and recovery. A replaceable Navigation Executor converts only validated
  targets into motion, so the same protocol covers instruction-following and
  object-goal navigation.
- Evidence: the authors report 60.8% (R2R), 53.9% (RxR), 76.0% (HM3D-v2), and
  59.3% (HM3D-OVON) success, above prior training-free results, plus a humanoid
  deployment demonstrating both task types in real environments.
  Simulation plus author-reported real-robot deployment; no independent
  reproduction.

### REVOLVE: An Automated Closed-Loop Framework for Evolving Robot Manipulation

- Paper: https://arxiv.org/abs/2609.14633 (v1 2026-09-13T16:08:03Z, announced in
  this block; cs.RO; Hanyu Liu, Qian Li, Yizhu Ding, and co-authors, 11 in total).
- Artifacts: **none located** — no link on the record, none in the HTML
  (verified 2026-09-15).
- Classification: Agentic Robot/VLA Harness (closed-loop deployment runtime).
- Why included: it attacks the **human cost** that keeps real deployment from
  being a learning loop. Failure assessment, correction, and environment reset
  are normally done by people, and models rarely absorb the resulting corrective
  experience; REVOLVE integrates data collection, policy training and deployment,
  failure recovery, and continual learning on one platform, with an **Automated
  Reset and Collection** architecture that resets and intervenes without an
  operator and **Dual-Loop Evolution** that feeds interaction and
  failure–correction data back into policy learning while an external *mismatch
  memory* refines the supervising agent's judgments — both the policy and the
  agent that judges it improve.
- Evidence: across four real-world manipulation tasks, five iterations, the
  authors report +18.5% average policy success and +8.5% agent-judgment accuracy,
  with human data-collection effort down 94.4% and deployment-testing effort down
  95.1%. Real-robot evidence, author-reported, no artifacts and no independent
  reproduction.

### DroneServer: An LLM-Agnostic, MAVLink-Based Agentic Harness over MCP (v3)

- Paper: https://arxiv.org/abs/2601.15486 (v1 2026-01-21; v2 2026-05-06; **v3
  2026-09-11T14:01:42Z**, announced in this block; cs.RO; Javier Noé Ramos Silva, Peter J. Burke). Code: https://github.com/PeterJBurke/droneserver (MIT,
  doi:10.5281/zenodo.22309419). Data: doi:10.5281/zenodo.22310050.
- Artifacts: **verified open.** The MIT-licensed repository (6 stars, created
  2025-11-02, pushed 2026-09-04) contains the server, deployment units, Docker
  definitions, tests, `SECURITY.md`, and extensive operational docs. The Zenodo
  API confirms record **22309419** ("DroneServer v2.0.2 … MCP server", MIT, 1
  file, 2026-09-04) and record **22310050** ("Supplementary data", CC-BY-4.0, 62
  files, 2026-09-04). Note: `doi.org` redirects to `zenodo.org/doi/…`, which
  returns **HTTP 403** to non-browser clients; verification was done through the
  official Zenodo API instead.
- Classification: Agentic Robot Harness; Evaluation/Safety.
- Why included: it is an **agentic harness for a physical platform** outside the
  arm/humanoid mainstream, and its safety model is the one this repository keeps
  arguing for. MCP is joined to MAVLink so any MCP-capable LLM gets command,
  telemetry, mission, and safety functions over ArduPilot and PX4 through **98
  tools covering 223 of 238 client-side MavSDK methods**; the LLM is an
  **untrusted commander** (zero public ports, server-side validation tiers,
  confirmation handshakes, an independent geofence the model cannot widen, an
  audit log, and an adversarial safety suite), and server-side mission state
  resolves the fire-and-forget mismatch between chat turns and flight time — a
  scripted client's 37.8-minute mission survived a 4-minute disconnection.
- Evidence: >1,000 simulated flights on a ten-mission benchmark (8 of 11 models
  complete 90.0–100% of its six flying missions; no aircraft left the permitted
  zone in 110 geofence-violation trials); on real hardware the unmodified server
  commanded three quadcopters, GPS-guided and GPS-denied, and five providers'
  models flew an unseen mission 10 of 10. Drone domain; results author-reported.
  The v3 is a substantive expansion (67 pages plus an 88-page supplement).

### ModularRSI: Modular and Generalizable Recursive Harness Self-Improvement

- Paper: https://arxiv.org/abs/2609.14857 (v1 2026-09-14T00:10:45Z, announced in
  this block; cs.CL; Siwei Wu, Jincheng Ren, Yizhi Li, and co-authors, 14 in total).
  Code: https://github.com/IQuestLab/ModularRSI.
- Artifacts: **verified open.** Apache-2.0 repository (10 stars, created
  2026-08-28, pushed 2026-08-31) with modules, adapters, apps, docs, registry,
  skills, trajectories, tests, and a CITATION file. Its README states
  Terminal-Bench 2.0 improving from **47.57% to 52.43%**.
- Classification: General Harness Methodology.
- Why included: it names and fixes the three reasons harness RSI does not
  generalize — evolving on the evaluation benchmark blurs reusable improvement
  with benchmark adaptation; single-trajectory updates conflate systematic
  harness deficiencies with instance-specific reasoning; and localizing a
  deficiency inside a monolithic harness entangles unrelated mechanisms. The
  answer is **benchmark-disjoint** (2,000 executable evolution tasks curated from
  external sources, disjoint from downstream evaluation), **contrastive** (it
  contrasts successful and failed trajectories *for the same task* and aggregates
  evidence across tasks), and **modular** (five evolvable modules — Agent Loop,
  Tool Use, Observation Management, Context Management, Task Completion
  Detection — each evolved within a restricted scope, then integrated with
  conflict resolution). This is the clearest statement so far of *what a harness
  module is* as an evolvable unit, which the repository's harness-evolution line
  (Ecdysis, RobustSGPO, HarnessEvolve, EvoUndo) has treated as a whole.
- Evidence: the authors report consistent gains on unseen in-domain and
  cross-domain tasks on Terminal-Bench 2.0 and SWE-Bench Verified, with the
  evolved harness transferring across foundation models. Digital agents, no robot
  experiment; author-reported, no independent reproduction.

### HarnessBandit: Joint Learnability-Transferability Scheduling for Multi-Harness Agentic RL

- Paper: https://arxiv.org/abs/2609.13739 (v1 2026-09-12T06:22:45Z, announced in
  this block; cs.AI, cs.CL, cs.LG; Hongliang Wei, Xiaobing Tu, and co-authors).
- Artifacts: **none located.** The HTML links only third-party benchmarks
  (`claw-eval/claw-eval`, `pinchbench/skill`, the ClawGym dataset on Hugging
  Face); no implementation or data release (verified 2026-09-15).
- Classification: General Harness Methodology (harness-variation robustness).
- Why included: it makes harness selection an explicit **scheduling** decision
  inside reinforcement learning, which sits directly beside the repository's
  harness-effects and multi-harness entries (ClawGym II, What Does Multi-Harness
  RL Learn?, Same Model, Different Harness). Training one policy across harnesses
  that differ in system prompts, tool schemas, control loops, and trajectory
  formats raises the question of which harness each optimizer step should use —
  one that currently teaches something useful *and* whose update helps the
  others. After each GRPO update HarnessBandit observes **learnability** (mean
  absolute advantage on the batch) and **transferability** (cosine between a
  low-dimensional gradient sketch of the current harness and exponential moving
  averages of the remaining harnesses), fuses them after pooled sliding-window
  min-max normalization, and samples with a visit-dependent bonus plus an
  explicit exploration floor.
- Evidence: training Qwen3.5-2B across six harnesses on ClawGym and evaluating on
  PinchBench (held-out tasks, in-distribution harness) and ClawEval (held-out
  tasks *and* harness), the authors report improvements over mixed-batch
  multi-harness training, with diagnostics indicating the two signals are
  distinct and evolve at different rates. Digital agents, no robot experiment;
  author-reported.

### Recoverability as a System Primitive for Long-Horizon AI Agents

- Paper: https://arxiv.org/abs/2609.13672 (v1 2026-09-12T02:54:33Z, announced in
  this block; cs.AI; Zhihui Zhang, Wei Liu).
- Artifacts: **none located** (verified 2026-09-15).
- Classification: General Harness Methodology (recovery/reuse contract).
- Why included: the repository's recovery line (AgentRewind, REVISE, EvoUndo,
  GuardrailLoop, ParaRecover) measures rollback, recomputation, and crash
  conformance, but leaves one decision implicit — **whether a saved state is a
  suitable place to resume at all**. This paper makes reuse an explicit decision
  (select a supported starting point and a permitted recovery action, or withhold
  automatic continuation) and binds it to a behavioral contract covering
  supporting evidence, execution, and independent checks, with a reference
  architecture connecting persistence, validation, and control. The measured
  result is the reason it belongs here: four deterministic and 20 paired file
  challenges show that **accurate restoration and successful completion can each
  conceal disallowed starting points**, so neither restored bytes nor final task
  success validates a recovery decision; event-time tests additionally show
  permission must constrain the action itself and that independently held policy
  evidence can expose a violation after an effect has occurred.
- Evidence: small deterministic challenge suites, digital agents, no robot
  experiment; author-reported.

### Dream-RSI: Recursive Self-Improvement through Evolving Worlds

- Paper: https://arxiv.org/abs/2609.14858 (v1 2026-09-14T00:10:47Z, announced in
  this block; cs.CL; Tong Zheng, Xidong Wu, Zheng Zhang, and co-authors, 17 in total).
  Code: https://github.com/zhengkid/Dream-RSI.
- Artifacts: **repository live, no license asserted.** `zhengkid/Dream-RSI` (15
  stars, created 2026-09-13, pushed 2026-09-14) describes itself as the official
  repository for the paper; no LICENSE file is asserted (verified 2026-09-15), so
  it is reported as live-but-unlicensed rather than open.
- Classification: General Harness Methodology.
- Why included: it moves recursive self-improvement from the harness to the
  **exploration policy**. Fixed search strategies stop fitting as the search
  space scales, while online policy optimization over meta-search spaces needs
  long-horizon rollouts with delayed, expensive feedback. Dream-RSI makes
  exploration explicit and programmable in a lightweight orchestration layer that
  leaves the coding agent unchanged, then reuses accumulated discovery history as
  a **replay simulator over the realized search space**: "dreaming" in that
  simulator supplies immediate, low-cost off-policy feedback to evaluate and
  refine exploration policies without repeated online evaluation, and the
  improved policy is redeployed, expanding the simulator pool in a self-improving
  loop. It is distinct from harness-level RSI (Self-Harness, ModularRSI) and from
  harness–policy co-evolution: the harness orchestrates discovery and the evolved
  artifact is the search policy.
- Evidence: reported across algorithm engineering, mathematical optimization, and
  GPU kernel engineering, with competitive or improved discovery quality at
  substantially reduced discovery cost in several settings. Digital agents, no
  robot experiment; author-reported.

### LIBERO-CTRL: Paired Evaluation of Compound Robustness in VLA Policies

- Paper: https://arxiv.org/abs/2609.15940 (v1 2026-09-14T17:44:54Z, announced in
  this block; cs.RO; Hiroki Sawada, Shunichi Kasahara).
- Artifacts: **none located** (verified 2026-09-15).
- Classification: Evaluation/Safety.
- Why included: it attacks a measurement assumption rather than a policy.
  Single-axis perturbation studies diagnose sensitivity to one shift at a time,
  but deployment combines shifts, and it is not known whether those measurements
  compose. LIBERO-CTRL pairs **each initial state** across six single-axis
  conditions and a matched simultaneous condition, separating two opposing
  outcome changes that aggregate success rates cannot distinguish: *emergent
  failures* (every single-axis rollout succeeds, the simultaneous rollout fails)
  and *compensated successes* (a single-axis rollout fails, the simultaneous
  rollout succeeds). Because the two cancel, aggregate compound performance can
  look consistent with single-axis measurements while individual outcomes change
  substantially — up to **29.0% of matched initial states** change outcome even
  when the difference between transition rates is not statistically
  distinguishable from zero, reaching 34.5% in the most affected condition across
  six policies and three severity levels, with transition rates stable under
  independent re-evaluation of stochastic policies.
- Evidence: LIBERO simulation only; author-reported. The transferable claim is
  an evaluation contract — compound robustness requires matched per-instance
  comparison, not aggregate single-axis success — which complements the paired
  protocols already curated (VLA-Arena, MANIGUARD, EBench, ROBORMBENCH).

### Bench2Dex: Visuo-Tactile Bimanual Dexterous Manipulation Across Dexterous Hands

- Paper: https://arxiv.org/abs/2609.15726 (v1 2026-09-14T15:22:59Z, announced in
  this block; cs.AI, cs.CV, cs.RO; Zhenjie Yang, Yideng Zhang, Dongjie Zhang, and co-authors, 24 in total).
  Project: https://bench2dex.github.io/ (live) and
  https://bench2dex.github.io/doc/.
- Artifacts: **verified open.** MIT-licensed `Bench2Dex/Bench2Dex` (9 stars,
  created 2026-08-14, **pushed 2026-09-15**) ships `benchmark/`, `collector/`,
  `policy/`, `robots/`, `scenes/`, `success/`, `teleop/`, `tools/`,
  `run_policy.py`, `replay.py`, configs, and tests. Public Hugging Face datasets
  `Bench2Dex/teleopdata` and `Bench2Dex/Assets`, plus a ModelScope organization,
  are linked from the project page.
- Classification: Evaluation/Safety (robot benchmark with released assets).
- Why included: it makes **dexterous-hand hardware heterogeneity** the controlled
  variable, which no other benchmark in this list does. Tactile hardware has not
  converged (hands differ in finger structure, contact surfaces, and sensor
  layout) and simulated tactile signals differ from physical sensors, so
  Bench2Dex adapts 12 existing dexterous hands to a **shared simulated tactile
  interface** that converts local contact geometry into image-like observations —
  a consistent format across morphologies that explicitly does not claim to
  reproduce a specific physical sensor. It contributes 26 bimanual tasks (tool
  use, articulated objects, multi-stage manipulation), ~1.3K human-teleoperated
  demonstrations, synchronized visual/tactile/proprioceptive/action/object-state
  streams, executable task metrics, and a robustness design that splits seven
  perturbation types into an **invariance axis** (correct action unchanged) and
  an **equivariance axis** (correct action changes with the perturbation) instead
  of one aggregate score.
- Evidence: ACT, Diffusion Policy, π0.5, and GR00T N1.5 are evaluated with
  reported failure modes. Simulation-only by design (stated by the authors);
  results author-reported.

### Colosseum V2: Benchmarking Generalization for Vision-Language-Action Models

- Paper: https://arxiv.org/abs/2605.27759 (**v2 2026-09-12**, announced in this
  block; cs.RO; accepted to IEEE RA-L; Jeremy Morgan, Hyeonho Oh, Prajwal Vijay, Jincen Song,
  and co-authors, 10 in total). Project: https://colosseum-v2.github.io/.
- Artifacts: **verified open.** Apache-2.0 `jstmn/ColosseumV2` (12 stars, created
  2025-02-25, pushed 2026-09-12, ~710 MB) with benchmark tasks, Docker setup,
  docs, examples, tests, and CITATION metadata.
- Classification: Evaluation/Safety.
- Why included: a large-scale **simulation benchmark for VLA generalization** —
  28 tasks across 13 task categories and two robot morphologies, spanning
  manipulation primitives and long-horizon behaviors, with in-domain and
  out-of-domain testing at scale on a GPU-parallel ManiSkill foundation,
  standardized tasks/metrics/protocols, and reported simulation–real-world
  correlations supporting ecological validity; ACT and π0.5 baselines expose
  limits in both base performance and generalization. It had never been screened
  by this repository (it predates the daily runs) and was surfaced by an
  announcement in this block.
- Caveat recorded for the record: **the v2 is a camera-ready re-announcement** —
  the abstract is the v1 text plus the RA-L acceptance, and the version was
  published 2026-09-12 rather than this week's submissions. The entry therefore
  rests on the benchmark, its protocol, and its live artifacts, not on new
  claims. Simulation-only; results author-reported.

### PhysBrain 1.5: From Vision-Language Models to Physical Foundation Models

- Paper: https://arxiv.org/abs/2609.14973 (v1 2026-09-14T03:25:46Z, announced in
  this block; cs.CV, cs.RO; DeepCybo Team and 53 co-authors). Project:
  https://deepcybo-physai.github.io/PhysBrain-1.5/.
- Artifacts: **verified open (weights + eval toolkit).** Hugging Face
  `DeepCybo/PhysBrain1.5-8B` (144 downloads) and `DeepCybo/PhysBrain1.5-2B` (101);
  `DeepCybo-PhysAI/PhysBrainEvalKit` (20 stars, created 2026-09-08, pushed
  2026-09-11) describes itself as "a reproducible evaluation toolkit on spatial
  and embodied-intelligence benchmarks". Both weight repositories and the eval
  kit assert **no license**, and the 2B/8B weights carry no model card
  restrictions — recorded literally, so reuse requires checking terms.
- Classification: Robot Foundation/World Model.
- Why included: one autoregressive model covers the physical loop of observation,
  interaction, and environmental change — language responses, end-effector
  motion, and dense visual targets are encoded as discrete sequences and jointly
  optimized by next-token prediction, with embodied pretraining supervision drawn
  entirely from human interaction videos before supervised fine-tuning on human
  demonstrations, robot trajectories, and simulated experience. The authors
  report an 8B model averaging **72.5 across 28 embodied-understanding
  benchmarks** (best open-source on 14), and the released evaluation toolkit is
  the reusable artifact for the harness line.
- Evidence limitation stated explicitly: end-effector-trajectory generation and
  future-scene prediction are shown as **qualitative examples only**, so the
  action and world-prediction claims are weaker than the understanding scores.
  Author-reported, no independent reproduction.

### Self-Evolving AI for Humanoids: Mechanisms, Safety, and Evaluation

- Paper: https://arxiv.org/abs/2609.13236 (v1 2026-09-02T13:38:43Z, announced in
  this block as newly indexed backlog; cs.DC, cs.RO; Loc X. Nguyen, Avi Deb Raha,
  Huy Q. Le, Eui-Nam Huh, Dusit Niyato, Choong Seon Hong; 30 pages, 9 figures,
  5 tables).
- Artifacts: **none** (survey); no repository or dataset was located (verified
  2026-09-15).
- Classification: General Harness Methodology; Robot Foundation/World Model
  (survey).
- Why included: it is the first curation-window treatment of self-evolution for
  an agent that has a **body**. Most self-evolution work concerns disembodied
  software agents, while deployed humanoids freeze a policy trained offline for a
  fixed objective even as tasks, environments, and their own bodies drift. The
  paper represents a deployed robot as a state tuple — policy, perception,
  memory, workflow, and body — updated by an *evolution operator* in a slow outer
  loop under a lifelong objective, and organizes mechanisms by increasing
  autonomy: self-learning, self-adaptation, self-optimization, self-generation.
  Two harness-relevant contributions: safety is treated as a design dimension of
  the operator rather than a post-hoc filter, with admissible evolution enforced
  by a **world-model verification gate inside a human-oversight envelope**; and
  evaluation should track the robot's evolving trajectory rather than a fixed
  checkpoint, which is why the authors name the absence of a self-evolving-
  humanoid benchmark as an open gap. It gives the repository's embodied
  self-evolution entries (Zetta, SHAPER, PRACTICE, HROS) a shared vocabulary and
  a safety constraint.
- Evidence: synthesis only — no deployed self-evolving humanoid is demonstrated.

### GzDRL: Reproducible and Scalable Deep RL with Gazebo

- Paper: https://arxiv.org/abs/2609.13243 (v1 2026-09-03T17:35:13Z, announced in
  this block as newly indexed backlog; cs.LG, cs.RO; Amal Dev Haridevan, Junjie
  Kang, Jinjun Shan). Code: https://github.com/amaldevh/gz-drl.
- Artifacts: **verified open.** MIT-licensed repository (created 2026-09-03,
  pushed 2026-09-03) with CMake build, Dockerfile, `GzDRL/`, `envs/`,
  `experiments_paper/`, `sitl/`, tests, and the manuscript PDF.
- Classification: Evaluation/Safety (reproducible experimentation
  infrastructure); Simulation.
- Why included: it targets the reproducibility property that middleware-based
  RL–Gazebo integrations give up. Conventional bridges step the physics engine
  and the agent through separate middleware paths, so data collection is
  nondeterministic; GzDRL is a single-process framework that steps the
  environment directly and synchronizes agent actions with physics updates,
  which yields deterministic high-throughput collection, vectorization, and
  reproducible training/evaluation on top of an unchanged, widely used simulator
  (Gazebo Sim). Determinism is what makes an RL result an evaluation rather than
  an anecdote, which is the same concern behind the repository's benchmark and
  observability sections.
- Evidence: author-reported highest workstation throughput among the evaluated
  frameworks, competitive with GPU-accelerated simulators on laptop hardware,
  multi-agent scalability, experiment-level reproducibility, and sim-to-real by
  flying a learned quadrotor policy without fine-tuning. Submitted to IEEE
  (copyright-transfer notice on the record); no independent reproduction.

### Revisions handled in place (no new entries)

- **No Free Checker v2** (`2609.09250`, v1 2026-09-08 → **v2 2026-09-13**).
  **Substantive**, verified by normalized sentence-level HTML diff: the survey
  grows from 31 to 33 pages and from 187 to **202** references; the
  rule-based/formal family gains content on invariance certificates, reachability
  checks, and symbolic feasibility checks as three groups ordered by how many
  trajectories one verdict covers; the intervention taxonomy is rewritten around
  *who decides the moment to take over* (a person in the human-gated setting, a
  learned verifier in the robot-gated setting); and Table 2 gains a seventh method
  (six in v1). The abstract is unchanged. README action: the existing entry was
  updated in place with the v2 numbers and the revision description.
- **vla-eval v3** (`2603.13966`, v1 2026-03-14 → v2 2026-04-17 → **v3
  2026-09-14**). **Cosmetic**, verified by sentence-level diff: the only added
  text is an acknowledgments paragraph (plus an unchanged table of contents), and
  the abstract is byte-identical to v2. The framework itself — a single
  `predict()` model integration, a four-method benchmark interface, Docker
  isolation over a WebSocket+msgpack protocol, 14 simulation benchmarks and six
  model servers, up to 47× speedup, 2,000 LIBERO episodes in ~18 minutes, and a
  leaderboard of 657 results across 17 benchmarks — is unchanged. The paper had
  never been screened by this repository (the entry came from the code), so the
  README entry was **updated in place** with the paper citation and documented
  numbers, and the cosmetic nature of v3 is stated in the entry.
- **LIBERO-RECOVER v2** (`2609.05178`): **already curated** on 2026-09-07 with
  its code and ModelScope datasets. The v2 (2026-09-14) is a camera-ready
  formatting revision — the body is the same length (41,487 → 41,204 characters),
  references lose their author lists, and affiliations are added. No new claims,
  no README change.
- **DroneServer v3** (`2601.15486`): substantive revision (see the entry above);
  the paper had never been curated, so it entered as a new README entry.

## Rejected / watch list

- **AcquireBound: Runtime Authorization for Resources Acquired by AI Agents**
  (`2609.14744`, cs.AI/cs.CR): 54-page preprint formalizing the *post-fulfillment
  activation gap* — a returned resource (compute, credential, account, service,
  another agent) can become usable authority even when payment, budget, OAuth,
  mandate, and fulfillment checks all passed. Proposes provenance-bounded runtime
  authorization with quarantined outputs, a versioned capability resolver, a
  downward-closed relational envelope over a typed resource–capability
  hypergraph, and single-use effect permits, with eight proved safety properties
  and a staged 18-case MCP-to-Docker composition (20/20 benign accepted, 40/40
  registered unsafe rejected, 89/89 tamper tests rejected). Genuinely relevant to
  the permissions/authorization line (Bounded Agents, One Gate Is Not Enough),
  but it is a single-author preprint with **no artifacts located** and no robot
  experiment. **Watch for an implementation or a replication.**
- **X-WBC: A Cross-Embodiment Foundation Model for Humanoid Whole-Body Control**
  (`2609.15213`, cs.RO, CoRL 2026): cross-embodiment humanoid framework with a
  project page and a repository, but `LogosRoboticsGroup/x-wbc` contains only
  `README.md`, `docs/`, and `.gitignore` — **no code and no weights** (verified
  2026-09-15). Treated literally as not open. **Watch for the implementation.**
- **When Malicious Instructions Persist: Persistent Memory Poisoning Attack on
  Harness-Based Agents** (`2609.13889`, cs.AI/cs.CR): attack on harness-based
  agents where poisoned memory survives across sessions. The repository named in
  the paper's HTML, `github.com/hsh754/PMPA`, returns **404** (verified
  2026-09-15), so the attack code is not available. **Watch**; the safety line
  already carries SkillMisevo, MaliciousSkillBench, and HarnessRisk.
- **SkillAtlas: An Attack Trace Library for Agent Skills** (`2609.13353`,
  cs.AI/cs.CR, REALM @ EMNLP 2026 non-archival): hosted library converting
  private agent-skill security bundles into reviewed, redacted, searchable cases
  (3,014 cases, 6,589 traces, 151,131 steps, 233 affected skills, 8 risk
  categories; trajectory-grounded labels raise pre-execution guard accuracy to
  0.770). No URL, license, or access path appears on the record (verified
  2026-09-15), so the "hosted library" is not locatable. **Watch for the public
  endpoint.**
- **How to Better Train VLAs: Lessons Learned From the REAL-I Challenge at ICRA
  2026** (`2609.13679`, cs.RO): competition report describing tasks, data and
  deployment interfaces, and the three finalist systems, with the transferable
  finding that offline action-prediction metrics poorly predict closed-loop
  success. No artifacts located; a challenge report with team-specific lessons
  rather than a reusable harness contract. **Watch** (would qualify if the
  evaluation interface or challenge data is released).
- **Bridging Thought and Action: MetaTool-Enhanced ROS Framework** (`2609.13335`,
  cs.AI/cs.RO): ROS-Agent architecture in which a MetaTool forces a pseudo-code
  plan into the scratchpad before execution, reporting ~24% gains on complex
  tasks on a real mobile robot. Relevant agentic-robot harness content, but an
  8-page submission with **no code links in the HTML** and no released interface.
  **Watch.**
- **ShieldVLA** (`2609.13231`, cs.AI/cs.RO): HJ-reachability safety alignment for
  VLA policies (57% lower cumulative safety cost, +0.13 success over SafeVLA
  across five benchmarks). A model-level safety method with no external runtime
  or interface; under the standing policy a model paper is not a harness paper
  unless it contributes a runtime/interface or artifact. No artifacts located.
  Excluded.
- **LG-VLN** (`2609.15098`): zero-shot VLN with LangGraph state orchestration —
  harness-shaped, but incremental against HarnessVLN and the orchestration line,
  with no artifacts located. Excluded for this window.
- **MessyMem** (`2609.15976`, CoRL 2026): learning-from-doing memory for mobile
  manipulation with a project page but no code link located. Model/memory method;
  excluded (the memory line is already well covered).
- **Stellar Colosseum** (`2609.15983`): many-agent harness for long-horizon
  research in mathematics and TCS. A harness, but domain-specific (research
  agents) and no artifacts located; the repository already carries the
  research-harness line (ScienceFlow, Harness-of-Harness). Excluded for this
  window.
- **CoArena** (`2609.14239`): real-time computer-use / multi-agent evaluation
  behind a commercial leaderboard (coarena.ai). Commercial, digital-only, outside
  the repository's physical-agent scope. Excluded.
- **Benchmark Radar** (`2609.11115`): living database and search engine for AI
  benchmarks. Supporting infrastructure for digital evaluation, no robot contract
  or evaluation protocol. Excluded.
- **Manipulation/benchmark near-misses screened and not selected this window:**
  `2609.08292` (EvoNav-Bench, lifelong navigation — no artifacts located),
  `2609.05260` (One Word, Different Action — real-robot language-conditioned
  benchmark, no artifacts located), `2609.13308` (GroundBench), `2609.06424`
  (OVMAN), `2609.13711` (degraded-communication multi-robot task allocation),
  `2609.15455` (InterSocialBench), `2609.14183` (DreamSat-Bench — a pose-
  estimation testbed rather than a policy benchmark).
- **Robot policy/model papers screened and excluded** under the standing rule
  that a model paper is not a harness paper without an external runtime,
  interface, or released artifact: `2601.14945` (TIDAL), `2511.12101` (action
  backbone), `2605.27759` is included above, `2606.08653` (FiberTune),
  `2606.20754` (epistemic uncertainty for VLA failure detection), `2606.24815`
  (MANGO test oracles), `2607.04816` (CAC-VLA), `2608.06965` (cross-view action
  consistency), `2609.07534` (proprioception-anchored pretraining),
  `2609.09148` (proxy policy steering), `2609.10050` (video plans → dexterous
  controllers), `2609.11697` (ActSafeGuard), `2609.12081` (MoPA), `2609.12103`
  (RodForesight), `2609.12245` (DIA), `2609.13235` (outcome bottlenecks),
  `2609.13244` (physical kernel), `2609.13318` (Attention-DP3), `2609.13695`
  (GROOVE), `2609.13812` (GeomVLA), `2609.13845` (LePlanner), `2609.13851`
  (ReWeight), `2609.13984` (efficient-VLA study), `2609.14073` (LPA-CWM),
  `2609.14146` (SmolVLA latency), `2609.14156` (visible touch), `2609.14219`
  (metrological inspection), `2609.14261` (VGFM), `2609.14310` (VLBiMan++),
  `2609.15005` (IMPACT-VLA), `2609.15012` (atomic motion coordinate),
  `2609.15014` (lexicographic preferences), `2609.15162` (LieSpline-DP),
  `2609.15198` (PredTac), `2609.15322` (DiffAdapterVLA), `2609.15570` (DIDO),
  `2609.15840` (uncertainty-guided refinement), `2609.15870` (WLA³), `2609.15910`
  (SlipSense), `2609.15921` (Touch2Trace), `2609.15988` (ResSafe).
- **Domain papers** excluded under the standing policy: driving, racing, and
  aerial (`2503.07737`, `2606.20980`, `2609.13224`, `2609.13428`, `2609.13941`,
  `2609.14426`, `2609.14806`, `2609.14997`, `2609.15169`, `2609.15861`,
  `2609.15895`), medical/surgical and clinical (`2609.13572`, `2609.14198`,
  `2609.14313`), perception, SLAM, odometry, and sensor datasets (`2505.12384`,
  `2508.13488`, `2601.10814`, `2604.00634`, `2609.04411`, `2609.13675`,
  `2609.13777`, `2609.14748`, `2609.14903`), conventional control, estimation,
  gait, and locomotion (`2108.03212`, `2206.10397`, `2209.11097`, `2309.05955`,
  `2603.29050`, `2605.21138`, `2609.02079`, `2609.13289`, `2609.13290`,
  `2609.14087`, `2609.14115`, `2609.14343`, `2609.14376`, `2609.14432`,
  `2609.14539`, `2609.14642`, `2609.14756`, `2609.15399`, `2609.15447`,
  `2609.15680`), multi-robot/swarm coordination (`2603.19502`, `2609.12959`,
  `2609.13711`, `2609.14208`, `2609.14268`, `2609.14567`, `2609.14935`,
  `2609.15266`), and agricultural, construction, marine, space, exoskeleton, and
  HRI-domain work (`2609.13234`, `2609.13606`, `2609.13627`, `2609.14058`,
  `2609.14450`, `2609.14543`, `2609.14558`, `2609.14710`, `2609.14765`,
  `2609.15362`, `2609.15667`, `2609.15988`).
- **Digital-agent papers screened and set aside** as too far from a robot or
  general harness contract to justify an entry this window: `2609.13334`
  (Agentic Company OS), `2609.13436` (self-adaptive physical AI with LLM agents
  on agricultural tasks — relevant framing, no artifacts), `2609.13543`
  (Asclepius clinical harness), `2609.13760`, `2609.13889` (watch-listed above),
  `2609.14138` (LIMBO), `2609.14239` (CoArena, above), `2609.14744`
  (AcquireBound, above), `2609.14896`, `2609.14913`, `2609.15096`, `2609.15134`
  (HazardAuditor), `2609.15209`, `2609.15293`, `2609.15309`, `2609.15364`
  (RSIAgent), `2609.15383`, `2609.15779`, `2609.15820` (AlgoEvo), `2609.15938`,
  `2609.15982`, `2609.15983` (Stellar Colosseum, above).
- **Withdrawals in this block (5 records, none curated):** `2609.10387`
  (deformable-convolution CV paper — authors withdraw pending substantial
  revision), `2609.11231` (voice-interactive multi-agent system for operating
  rooms, cs.AI/cs.CL/cs.HC — "technical approach is immature and may involve
  information security concerns"; this was already in the digital-agent
  set-aside list above), `2504.06532` (time-series nowcasting — error in the
  model training setup), `2601.00900` (federated defense for SAR — withdrawn for
  major revision), and `2603.04419` (context-dependent affordance reports in
  VLMs — a substantial revision whose note withdraws specific interpretations
  rather than the paper). None is in the robot/harness scope, and no curated
  entry is affected.

## Operational notes

- **The arXiv export API was rate-limited for the entire run for the second
  consecutive day** — `HTTP 429`, body `Rate exceeded.` (14 bytes), on four
  probes covering 20260912–20260915. Recorded as a blocker for the documented
  primary query path, **not** as evidence about the index. The substitute
  (OAI-PMH block harvest + direct `abs`/`html` fetches + ID-existence bisect +
  the Zenodo and GitHub/Hugging Face APIs) is official arXiv infrastructure plus
  primary artifact sources.
- **OAI-PMH metadata semantics confirmed again:** the header `<datestamp>` is the
  announcement date of the record's latest version (a record carries exactly one,
  and it moves when a replacement is announced — this is the mechanism behind
  the 13-record difference from the previous run's 09-14 count), while the
  metadata `<created>` field carries the **announced version's** submission date
  and `<updated>` mirrors the datestamp. Verified against `abs` submission
  histories for `1304.3111` (created = v2 date), `2402.18945` (created = v5
  date), `2409.17685` (created = v3 date), and `2609.15195` (created = v1 date).
- **Zenodo `doi.org` landing pages return HTTP 403 to non-browser clients**; the
  records themselves were verified through `https://zenodo.org/api/records/<id>`
  (title, publication date, license, file count). Future runs should use the API
  for Zenodo verification rather than the DOI resolver.
- **`.scratch/arxiv-2026-09-15/`** (untracked, deliberately not staged) holds the
  harvest XML, parsed `records.json`/`newblock.json`, screening scripts, fetched
  `abs`/HTML pages, version diffs, and artifact-check output; `/tmp` is not
  persistent between shell invocations on this host.
- The working branch was `main` throughout. Only task-owned files (`README.md`
  and this record) were staged; the unrelated user changes
  (`docs/reference-architecture.md` modified; `docs/ring-harness.png` and
  `handoff.md` untracked) were left untouched.

## Validation performed

- `git diff --check` clean.
- Markdown structure re-validated by heading enumeration after editing (all
  `##`/`###` headings present, including `### Reusable Perception Foundations`,
  which an insertion initially displaced and was restored).
- Every newly added URL was fetched (**36** distinct URLs across the README
  changes): **34 returned HTTP 200**; the two exceptions are the Zenodo DOI
  links, which return 403 through the DOI resolver and were instead verified as
  live records through the Zenodo API.
- README internal consistency: the `last verified` badge and the body
  `Last verified:` line were both moved to **2026-09-15**, matching this
  record's date.
- Cross-file consistency: all names and arXiv IDs in the README changes were
  grepped against `README.md`, `docs/`, and `sources/` before editing;
  `vla-eval`/`vla-evaluation-harness`, `No Free Checker`, and `LIBERO-RECOVER`
  were confirmed already curated and handled as updates or non-changes rather
  than duplicates.
- Categories and artifact statuses in every new entry were taken from primary
  sources (arXiv record, `abs` version history, official project page, official
  code/weights/data repositories) on 2026-09-15, and each entry states the
  author-reported versus verified distinction, the simulation-only status where
  applicable, and the literal artifact status.
