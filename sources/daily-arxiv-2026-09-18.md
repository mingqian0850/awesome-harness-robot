# Daily arXiv scan — 2026-09-18

## Scope

- **Interval:** everything announced after the 2026-09-17 run's cutoff. That run
  (started ~07:47 UTC on Thursday 2026-09-17) screened the block whose OAI header
  datestamp is **2026-09-17** — 898 unique base IDs at the time, newest base ID
  `2609.19145` — covering the Wednesday submission block plus newly announced
  backlog. This run started ~07:30 UTC on **Friday 2026-09-18** and covers
  everything announced since.
- **A new announcement block landed.** Datestamp **2026-09-18**:
  **1,232 set memberships = 905 unique base IDs** across the five target
  categories for the two-datestamp harvest window, of which the **905 IDs
  carrying the 2026-09-18 datestamp are the block this run screened**.
  Per-category memberships for the 09-18 datestamp (all listed categories, not
  just primary): cs.AI 349, cs.LG 328, **cs.RO 195**, cs.CV 190, cs.CL 170,
  stat.ML 38, cs.CR 37, cs.IR 25, cs.HC 23, cs.SE 22, cs.MA 21, cs.SY 21,
  eess.SY 21, cs.CY 17.
- **ID frontier.** The newest base ID in the block is **`2609.20822`**;
  `2609.20823` exists but is `quant-ph` (out of the five target categories, so
  correctly absent from the block) and `2609.20824` returns HTTP 404
  (ID-existence bisect on `arxiv.org/abs/<id>`, 2026-09-18). The frontier thus
  moved from `2609.19145` to `2609.20823`.
- **Composition of the 09-18 block (905 unique IDs), by the submission date of
  the announced version** (the OAI metadata `<created>` field, which carries the
  latest announced version's date):
  - **615 records carry 2026-09-17** — the Thursday submission block, which is
    what arXiv releases on Friday, and the bulk of genuinely new content.
  - **156 carry 2026-09-16** and **8 carry 2026-09-15** — held and cross-listed
    new submissions whose ID was assigned at announcement.
  - **126 carry earlier dates**, reaching back to July 2026 and beyond
    (replacement versions and long-held submissions); **254 records in the block
    carry pre-`2609.` IDs**, the signature of re-announced replacements.
- **Continuity check against the previous run — exact, with no gap.** The
  previous run counted **898** unique IDs for datestamp 2026-09-17; this harvest
  sees **870** for the same datestamp. All **28** IDs that left the 09-17 group
  are present in the 09-18 block (set difference computed over the two runs'
  parsed OAI records: `|prev09-17 \ cur09-17| = 28`, `|(prev09-17 \ cur09-17) ∩
  cur09-18| = 28`, `|left but unaccounted for| = 0`). OAI records a single
  datestamp that follows the latest announcement, so this is the same
  re-announcement churn earlier runs measured, and every churned record was
  re-read as a revision (see below). Nothing announced in the interval was
  missed.
- **Export-API cross-check.** The `submittedDate:[20260917000000 TO
  20260917235959]` window across the five categories returns **541 raw entries =
  412 unique base IDs**, and **all 412 are inside the 905-ID block** (0 outside).
  The block contains **493 further IDs** (held, cross-listed, and replacement
  records) that the export API cannot see, so the API window is a subset check,
  not the block definition — the same relationship the previous run measured.
  The `submittedDate:[20260918000000 TO 20260918235959]` window returns **0
  entries in all five categories**: nothing submitted on 09-18 has been announced
  yet, which is expected at 07:30 UTC and confirms the block boundary rather than
  a truncation.
- **Late-batch re-check:** not applicable — the newest visible block (09-18) is
  current for this run's date, and the previous day's range was re-harvested in
  the same window (the 870-ID 09-17 group) rather than assumed unchanged.
- **"Unchanged batch" re-screen:** not applicable in the required sense — a
  genuinely new block landed and the ID frontier moved. The **entire** 905-record
  block was screened, including the 126 records whose announced version predates
  2026-09-16 and the 254 pre-`2609.` replacement records that no earlier
  new-submission window covered.
- **Revisions in scope.** The 28 replacement versions announced in this block
  were version-checked before a verdict: `2407.06868`, `2605.16692` (v4),
  `2606.01670`, `2608.04765` (v3), `2609.10986`, `2609.16814`, `2609.17652`,
  `2609.17772`, `2609.18063`, `2609.18084` (v2), `2609.18117` (v2), `2609.18176`,
  `2609.18259` (v2), `2609.18304` (v2), `2609.18310`, `2609.18329`, `2609.18374`
  (v2), `2609.18455` (v2), `2609.18462` (v2), `2609.18620` (v2), `2609.18748`,
  `2609.18766`, `2609.18820` (v2), `2609.18949`, `2609.19076`, `2609.19107`,
  `2609.19122`, `2609.19137` (v2). All 28 had already been screened as v1 by the
  2026-09-15…17 runs; none adds a new result or a qualifying artifact (details in
  the rejected list below).
- **Withdrawals in this block:** five records carry a withdrawal notice. Two are
  in scope enough to record: **`2608.04765`** (*Explicit Language Memory for
  Long-Horizon Planning in Vision-Language-Action Models* — "withdrawn by the
  authors, because the manuscript was uploaded to arXiv with…") and
  **`2604.25323`** (*ANCHOR: A Physically Grounded Closed-Loop Framework for
  Robust Home-Service Mobile Manipulation* — "the authors have identified several
  errors and inconsistencies"). `2606.01670` (generative recommendation),
  `2609.19422` (burnout screening), and `2609.20217` (traffic forecasting) are
  out of scope. `2608.04765` was already recorded as withdrawn by the 2026-09-17
  run. **No curated README entry is affected by a withdrawal in this block.**

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`, `from=2026-09-17&until=2026-09-18`), paginated to
   resumption-token exhaustion, one page per category, saved to
   `.scratch/arxiv-2026-09-18/oai-*.xml` (1,232 memberships; 7.8 MB total).
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`oai-records.json`).
3. **Export-API cross-check** over both daily `submittedDate` windows for the
   five categories, parsed to base IDs and compared as a subset.
4. **Screening.** Keyword-breadth ranking over the full block (harness, agentic,
   runtime, tool, self-improvement, memory, recovery, safety, evaluation, robot,
   world model, skill, policy, VLA), plus a dedicated scan of every block title
   containing *harness* (8 records), plus a robot/VLA/world-model relevance pass
   over the new-frontier group (590 records with base ID ≥ `2609.19146`) and the
   older/replacement group (315 records, 69 of them relevance-positive).
5. **Dossiers** for every shortlisted record (full title/abstract/comment/
   categories/created), read before any verdict.
6. **Primary-source verification** of each candidate's artifact claim: the arXiv
   abstract page, the arXiv full-text HTML (artifact links extracted), the
   official project page, and the GitHub/Hugging Face APIs. Duplicate screening
   against `README.md`, `docs/*.md`, and `sources/*.md` by arXiv ID.

**Network note (tooling, not arXiv):** the export-API query initially returned
0 bytes for every window. The cause was `curl`'s URL globbing consuming the
square brackets in `submittedDate:[…]`; the query succeeds with `-g` (or
percent-encoded brackets). Recorded because a silent empty response is
indistinguishable from "no submissions" and would have produced a false negative
for the whole window.

## Included (README updates)

Twelve entries added, plus five "Current Landscape" bullets. Artifact status is
reported literally; "no artifacts located" means the record, the full text, and
the project page yielded no official code/model/data endpoint reachable on
2026-09-18.

### General Harness Design and Self-Improvement

- **SoL-Pi: Recursively Scaling Auto-Research Loops for Efficient Agent Harness**
  (`2609.20519`, cs.AI). Takes recursive self-improvement to the **harness
  layer** — auto-research loops scaled across many environments so the surviving
  changes are reusable mechanisms rather than settings tuned to one setting. Four
  mechanisms compose SoL-Pi: action execution, context compaction, observation
  handling, delegated reading. On the 51-task EdgeBench the authors report parity
  with Pi across GPT-5.6 Sol and Opus 5 while cutting recorded token traffic
  44.7–49.0% and API cost by about one third (their estimate: \$8.75–13.50/hour
  against native Codex and Claude Code, \$4.36–5.71 against Pi). **Verified
  open:** MIT repository `NVlabs/SoL-Pi` (TypeScript `src/`, `docs/`, `tests/`,
  `scripts/`, `AGENTS.md`, `CLAUDE.md`; created 2026-09-02, pushed 2026-09-18,
  2,217 stars) and the NVIDIA project page `nvlabs.github.io/SoL-Pi/` are live.
  Digital agents only, no robot experiment; results author-reported.
- **An Empirical Study of Harness Design for Coding Agents** (`2609.20804`,
  cs.AI/cs.CL/cs.LG/cs.SE). Opens the harness instead of ranking whole systems:
  the execution loop is fixed while **planning, action space, and context
  management** vary across four models on SWE-Bench Verified and Terminal-Bench
  2.1, in **176 matched settings** over five context-management strategies and
  four context-window budgets. Reported findings: context management pays off
  mainly by preventing context overflow; staging rule-based elision *before* LLM
  summarization beats making elided content recoverable (which the model rarely
  uses); planning is an accuracy scaffold for weaker models and a cost saver for
  stronger ones; bash-capable models do as well with a bash-only interface at
  lower cost. Trajectory analysis localizes the effects — context management
  extends trajectories, planning changes where they stop, action space changes
  the granularity of code written. Digital agents; **no artifacts located** (the
  only repository link in the full text is the third-party `opencode` harness);
  author-reported.
- **How Do Agent Harnesses Create Value? Planning Information and Release Control
  in Stateful LLM Agents** (`2609.20474`, cs.AI). Pairs prewritten task-specific
  plans (**Fixed**) against shuffled policy text **matched in word count
  (Sham)**, isolating the *content* of planning guidance; over **265 matched
  cells** in two Retail experiments and an Airline pilot in τ²-bench, Fixed
  improves oracle-verified success by **7.17 points** (90% task-clustered
  bootstrap interval 1.15–13.36), concentrated in higher-complexity tasks. Release
  control is priced separately: a read-only terminal verifier rejects **61%** of
  Retail oracle-invalid episodes while withholding **17%** of correct ones at
  under a cent per episode, and a standalone verifier captures nearly all the
  false-pass benefit of the full planning-plus-verification stack. Digital
  agents; **no artifacts located**; author-reported.
- **An Architecture for Long-Horizon Agents: Levels, Ticks and Cascaded
  Intelligence** (`2609.19519`, cs.AI/cs.LG). The clearest statement of this
  list's thesis in the block — a long-horizon agent "must run continually without
  forgetting before it can learn continually," and that ability "lies in the
  harness around the model rather than in the model itself." Seven derived
  bottlenecks are answered with time-scale-indexed **levels** each holding a
  bounded summary of the level below, a clocked **tick** as the unit of
  autonomous action, and **cascaded intelligence** escalating only after a failed
  review. Evidence is a **ten-day campaign** reproducing a published RL result
  with one human visit per day: the thread survived every context reset and
  session boundary, and knowledge written early changed later behaviour with no
  weight change. A design/experience report with no baseline harness; digital
  agents; **no artifacts located**; author-reported.
- **When Self-Evolution Backfires: Pre-Commit Gating against Skill Contamination
  in LLM Agents** (`2608.05810`, cs.AI/cs.CL, v2). Finds that skill accumulation
  is **non-monotonic**: past a critical pool size, newly added skills degrade
  performance, formalized as a **capability-contamination phase transition**
  whose cause is structural — a defective skill in the decision context becomes
  distillation material for later skills, forming cross-round contamination
  chains. Contamination is reported as **structurally irreversible**, so removing
  the source skill after the fact recovers only a small fraction and admission
  must be **pre-commit** (Verifier-as-Gatekeeper, a progressive trust hierarchy).
  This supplies the causal mechanism the vault line (EvoUndo, SkillGate,
  MaliciousSkillBench) was defending against retrospectively. Digital agents;
  **no artifacts located** (only the third-party `agentica` package is linked);
  author-reported.
- **Position: It is Time to Virtualize Foundation Models with a Self-evolving
  Operating System Layer** (`2609.19203`, cs.AI/cs.LG/cs.MA/cs.OS; ICML 2026
  Position Paper Track). Argues that each agent framework now **embeds an
  implicit runtime** for state, memory, budgets, and guardrails, so behaviour is
  non-portable and governance brittle even as MCP/A2A ease connectivity, and
  proposes a **Foundation Model Operating System** that virtualizes FM
  interactions the way VMs abstract hardware. Internally it owns memory tiers,
  model selection and resource allocation, and verification and policy
  enforcement, and is *self-evolving* in learning when to intervene. A position
  paper: no implementation, no experiment, **no artifacts and no robot
  experiment**.

### Agentic Robot and VLA Harnesses

- **Coding Agents with an Obstacle-Aware Harness for Safe Robot Manipulation**
  (`2609.20822`, cs.AI/cs.CL/cs.CV/cs.RO). Asks whether the coding-agent-for-
  robots paradigm is **safe**: the agent collides with a forbidden obstacle in
  most cases while treating completion as its only objective, and the paper rules
  out perception and instruction as causes (the obstacle is reasoned about in the
  traces, the prompt already forbids contact) to locate the failure in **planning
  priority**. Decomposing manipulation into a route phase and a contact-rich
  moment exposes both gaps, and **SafeHarness** answers with obstacle-aware route
  planning (bounding-box waypoints; plan → verify → replan → execute) plus
  obstacle-aware contact execution. The authors report **71.9% task success and
  87.5% collision avoidance**, +6.5%/+27.0% over the previous SOTA and
  2.3×/1.5× the same agent without harnesses. No artifacts located;
  author-reported; the safety constraint is geometric rather than joint- or
  force-level.
- **MaskHarness-WAM: Instance-Grounded Harnessing for Long-Horizon Robot
  Manipulation** (`2609.19974`, cs.AI/cs.RO). Targets the case where a
  limited-horizon policy cannot decide at all: several objects share an identical
  appearance and must be handled in a prescribed order. The harness connects
  high-level planning to low-level policies **through target masks**, and because
  each subtask has a different target instance it **re-observes the scene and
  generates and verifies the target mask at every subtask boundary**, updating
  the instance-level spatial condition and advancing on verified subtask
  completion. Real-robot platform; the authors report substantial improvement
  over limited-horizon policies on sequential multi-object manipulation, but
  publish no isolated success rates. No artifact links; author-reported.
- **MAGMA-GEN: Validated Recovery Supervision from Ambiguous Failures via
  Counterfactual Re-Execution** (`2609.20056`, cs.AI; CoRL 2026). Attacks the
  supervision problem under recovery: in a hierarchical system a failed rollout
  is **ambiguous** (invalid high-level decision vs. partial observation vs. valid
  decision whose physical execution failed), supervised learning has no labels
  for those states, and RL suffers sparse rewards and non-local credit
  assignment. MAGMA-GEN converts ambiguous failures into supervision on-policy: a
  privileged coach hypothesizes an early decision-level error and proposes a
  localized correction, and — because the diagnosis is fallible — a candidate is
  kept **only if re-execution from the same state under matched conditions
  improves downstream progress**, making the coach's judgement falsifiable rather
  than authoritative. Reported improvements over distillation and
  trajectory-repair baselines in simulation and real-robot execution. No
  full-text HTML and no artifacts located; author-reported.
- **Learning and Transferring Closed-Loop Robot Software** (`2609.19906`,
  cs.AI/cs.RO). The code-as-policy question asked about the **software** rather
  than the weights: a coding agent writes closed-loop policy code from a few
  demonstrations, improves it from simulation feedback, and retains
  validation-selected implementations in a **software archive**; for a new task
  it reuses archived implementations plus target demonstrations and execution
  feedback, then **freezes the policy so it executes with no further model
  calls**. Across four RoboCasa source tasks iterative optimization raises mean
  success **28.3% → 64.2%**; across nine target tasks and three runs mean success
  is **45.2% (no references) / 41.5% (initial source code) / 57.0% (optimized
  source code)**, with optimized references ahead in all three runs (mean +15.6
  points) while initial references average better on two tasks. The negative
  result — a merely-written implementation can be worse than none — is the
  reason to record it. Simulation only (RoboCasa); no artifacts located;
  author-reported.

### Robot Foundation and World Models

- **Astronex-World 1.0: Real-Time Interactive World Model Foundation**
  (`2609.20034`, cs.AI/cs.CV/cs.RO). A controllable video world-model foundation
  conditioned on **frame-aligned camera trajectories, continuous actions, and an
  embodiment identifier**, with **text events insertable at a chosen rollout
  position**. Two models on the Wan2.2-TI2V-5B prior: bidirectional for
  full-context generation and block-causal with cross-block KV caching for
  persistent generation; PRoPE injects camera intrinsics/extrinsics and a
  64-dimensional action stream modulates every Transformer layer. The authors
  report 832×480 at **24 fps**, all five training stages on two L20 48 GB GPUs,
  real-time streaming on one, and **73.5 WBench Navi / 70.0 WBench Full** (above
  13.6B LongCat-Video and 14B Helios, within a point of 22B LTX-2.3).
  **Verified open**: Apache-2.0 repository `Astronex-Robotics/Astronex-World`
  (`inference/`, `models/`, `post_train/`, `scripts/`, `utils/`; created
  2026-09-16, pushed 2026-09-17), the Hugging Face release
  `Astronex-Lab/Astronex-World` (Apache-2.0, `image-to-video`; ~10.7 GB diffusion
  backbone plus text encoder, VAE, tokenizer), and the project page
  `world.astronex.com.cn` are live (verified 2026-09-18). A technical report with
  author-reported numbers; the action interfaces are described as hooks for
  embodied post-training, not a demonstrated robot policy.

### Benchmarks and Evaluation

- **From Rollout to Reset: A Graph-Based Harness for Autonomous Long-Horizon
  Manipulation Evaluation** (`2609.19413`, cs.AI/cs.RO). Owns the part of
  real-robot evaluation everyone runs and nobody instruments — the human who
  resets the scene between rollouts, which burns operator time and leaves the
  **initial state distribution unspecified**. **HALTER** restores the scene by
  **planning over a library of learned atomic reset skills** (so demonstration
  cost scales with library size, not with the number of terminal states), builds
  a spatial scene graph online from point clouds and vision foundation models,
  and has an LLM score the rollout, plan the reset, and **verify the reset
  succeeded** without labelled success images. On four Franka long-horizon tasks
  the authors report **76% scene restoration vs. 52% AutoEval / 65%
  motion-planning reset**, **90% vs. 76%** correct completed-skill fraction,
  **91% vs. 78%** correct reset verdicts, **72% less operator time**, and on
  three held-out tasks **74.7% reset vs. 1.3%** for a per-task reset policy.
  **Artifact status is literal:** the project page is live and `YY-GX/HALTER`
  exists, but the repository contains only a README and states "Code release is
  in progress" — a placeholder, **not open** (verified 2026-09-18).
  Real-robot evidence, author-reported.
- **Beyond Patch Removal: Persistent Adversarial Effects in Vision-Language-
  Action Policies** (`2609.19669`, cs.CV/cs.RO). Separates two effects that
  continuous-attack evaluations conflate: immediate action corruption while a
  patch is present, and **persistent state effects that survive its removal**.
  The **state-restoration protocol** removes the patch at matched action-chunk
  boundaries and measures recoverability **under the same remaining step
  budget**, with clean, random-patch, deviation-matched, and fixed-direction
  controls isolating the adversarial effect from occlusion, committed action
  error, and directional persistence. On OpenVLA-OFT under EDPA attacks the
  authors report only **36.2% of LIBERO-Long episodes recoverable after five
  chunks, vs. 89.9% (deviation-matched) and 87.0% (fixed-direction)**. A
  recovery adapter trained on attack-induced states is evaluated under controlled
  intervention latency. The evaluation contract the red-teaming line (RedVLA,
  DropVLA) was missing; LIBERO **simulation only**; no artifacts located;
  author-reported.

### Current Landscape additions

Five bullets, one per theme: transferable harness code from harness-level RSI
(SoL-Pi); component-level decomposition and pricing of harness value (Harness
Design study + Harnesses Create Value); long-horizon continuity and pre-commit
skill admission as harness responsibilities (Levels/Ticks + When Self-Evolution
Backfires); robot harnesses owning the safety constraint and the reset
(SafeHarness + HALTER + MaskHarness-WAM); and open world-model foundations
shipping code and weights (Astronex-World 1.0).

## Rejected / watch list

### Revisions read in this block, not qualifying

All 28 replacement versions announced in this block were screened; each had
already been read as v1 by the 2026-09-15…17 runs, and none changed a verdict.
The robot/VLA-relevant ones, with the reason:

- **M²Tok** (`2609.18259`, v2, cs.AI/cs.CL/cs.CV): multi-head multi-codebook
  discrete action tokenization for VLAs. The v2 full text now links
  `github.com/cpaaax/M2Tok` (exists: `src/`, `requirements.txt`, 1.6 MB, no
  license asserted, pushed 2026-09-17). Still an action-tokenizer **model**
  contribution with no external runtime, interface, or evaluation contract, so
  the artifact does not change the earlier verdict.
- **Decoupled Embodiment Model** (`2609.18374`, v2, cs.RO): the v2 full text adds
  `github.com/Apollo-Lab-Yale/decoupled-embodiment-model` (a small `dem` package
  with `examples/` and `tests/`, created 2026-09-16, no license). A model/stack
  contribution, not a harness; excluded as before.
- **Not All Layers Need Tuning** (`2609.18084`, v2, cs.CV/cs.LG/cs.RO): variable-
  rank LoRA allocation for VLA adaptation; no artifacts. Excluded (adaptation
  pipeline, not a harness contract).
- **OpenDexGrasp** (`2609.18117`, v2, cs.RO): open-vocabulary task-oriented
  dexterous grasping, now with project page `opendexgrasp.github.io`. Excluded
  under the standing perception/manipulation-model policy.
- **Rollback the World, Keep the Reflection** (`2609.18304`, v2, cs.CL/cs.RO):
  rollback-boundary control for long-horizon LLM agents; no artifacts. The
  recovery-contract line is already carried by Recoverability as a System
  Primitive and REVISE.
- **Dreaming the Sound of Contact** (`2609.19137`, v2, cs.AI/cs.RO): project page
  `dreamingcontactsound.github.io` added; audio/video force-aware manipulation
  generation. Excluded as a model paper.
- **Explicit Language Memory for Long-Horizon Planning in VLA Models**
  (`2608.04765`, v3, cs.AI/cs.CV/cs.RO): **withdrawn by the authors** — no entry
  can rest on it. Already recorded as withdrawn by the 2026-09-17 run.
- **EfficientTDMPC** (`2605.16692`, v4, cs.AI/cs.LG/cs.RO),
  **DeformSmith** (`2609.18620`, v2, cs.RO — "harness" here is a *physics solver*
  harness, already excluded 2026-09-17), **Compositional Policy Violations**
  (`2609.18820`, v2, cs.AI/cs.MA — already excluded 2026-09-17), **ForwardDLO**
  (`2609.18455`, v2), **CSWAM** (`2609.18462`, v2), **AntiGrounding** and the
  remaining non-robot revisions (`2609.10986`, `2609.16814`, `2609.17652`,
  `2609.17772`, `2609.18063`, `2609.18176`, `2609.18310`, `2609.18329`,
  `2609.18748`, `2609.18766`, `2609.18949`, `2609.19076`, `2609.19107`,
  `2609.19122`, `2407.06868`, `2606.01670`): version bumps with no new result and
  no qualifying artifact, or out of scope by title and category.

### Candidates verified and rejected

- **When Faster VLA Deployment Changes Closed-Loop Behavior** (`2609.14146`,
  cs.LG/cs.RO, v2): SmolVLA on an RTX 2060 comparing PyTorch+AMP with ONNX
  Runtime CUDA EP, including a genuinely useful verification finding — the
  artifacts exported with a requested INT8 setting are **byte-identical FP32
  graphs**, so the "INT8" row is not operator-level quantization. The MIT
  repository `rafiqul713/smolvla-libero-onnx` is real (`docs/`, `results/`,
  `scripts/`). Excluded as a single-model, single-hardware deployment audit: the
  finding is valuable but narrow, the v1↔v2 HTML diff shows the graph audit was
  **already present in v1** (so this is not a meaningful revision), and the
  VLA-serving-contract line is already carried by Robion and Latency-Tolerant
  Cloud-Edge VLA. Recorded because the graph-audit discipline is worth reusing.
- **How to Better Train VLAs: REAL-I Challenge at ICRA 2026** (`2609.13679`,
  cs.RO, v2): competition report with tasks, data and deployment interfaces, and
  three finalist systems, whose transferable finding is that offline
  action-prediction metrics poorly predict closed-loop success. Excluded as a
  challenge report with team-specific lessons; no artifacts located (the only
  repository link is the third-party `NVIDIA/Isaac-GR00T`).
- **WorldContact** (`2609.19600`, cs.RO): contact-centric world model for
  deformable-object manipulation, reporting a 10× state-rollout speedup over the
  source simulator and bag-lift success rising from 65% to 95% when a VLA is
  fine-tuned on the expanded data. A model/data-generation result with **no
  artifact links**; excluded as a model paper — record it if weights or data are
  released.
- **MoWAM** (`2609.20709`) and **Agile-WAM** (`2609.20761`, project page
  `hanchuzhou.github.io/TARO_project_page/`): world-action-model efficiency
  papers (replacing future video generation with explicit future-motion
  prediction; an agile tactile WAM). Model contributions with no code, weights,
  or interface; excluded, consistent with the standing WAM policy.
- **JEPA-WAM** (`2609.20277`, created 2026-08-04 — backlog announcement),
  **TacSushi** (`2609.19613`), **Predict Before You Deploy** (`2609.19441`):
  world-action-model training/deployment papers (visual instruction banks;
  tactile-grounded WAM; offline quantization-degradation prediction). Noted for
  the WAM line; no artifacts located, excluded as model papers.
- **StageGuard** (`2609.20791`, cs.RO): agentic distillation of stage-transition
  decisions for long-horizon tasks, evaluated on BEHAVIOR-1K and real robots.
  A student-VLM training method rather than a runtime contract, and the runtime
  stage-monitoring line is already carried by FARM, ContactGuard, SAFECAST, and
  FailBench. Excluded; **watch** if the online monitor is released.
- **CoreSense** (`2609.19512`, cs.AI/cs.RO): traceable failure recall with a
  conflict-aware belief gate (scope, provenance, time, contradiction, support)
  permitting PROCEED / re-observe / abstain / escalate, with a 20/20
  CockroachDB–Bedrock deployment-path validation. Harness-shaped and auditable,
  but the paper states its evidence supports "an auditable integration pattern,
  not autonomous recovery or certified safety", evaluation is offline public
  data plus signal-level simulation and a live cloud path, and **no physical
  robot was commanded**. Single-author preprint, no artifacts. **Watch** rather
  than include.
- **Workspace Models** (`2609.20820`, cs.AI/cs.RO, CoRL 2026): a lightweight
  latent memory token distilled at train time so VLM queries are removed from the
  deployment loop. A memory-representation model paper; no artifacts located.
  Excluded, but relevant to the memory line.
- **V2-STRep** (`2609.20582`, cs.RO): VLM-grounded structured task
  representations turning generated video into reusable robot skills, with
  geometry-specific transfer rules and task-constrained trajectory optimization.
  Real-world results, but the artifact is a representation rather than a runtime
  or released library; no artifacts located. Excluded; **watch**.
- **Quantifying Overclaiming Propensity in Frontier LLM Agents** (`2609.20812`,
  cs.AI/cs.LG/cs.SE): introduces **OverclaimBench** and a crisp definition — an
  agent overclaims when its final response contradicts information in its own
  context — and reports that agents do not read all files they were asked to
  review in **67.9%** of runs across eight proprietary CLI agents and four
  open-weight models under a fixed harness. Genuinely relevant to agent
  verification, but no benchmark assets, harness, or data endpoint could be
  located on the record or in the full text. **Watch for the release; include
  then.**
- **DeltaSelect** (`2609.19607`, cs.AI/cs.LG/cs.SE): affordable A/B testing for
  coding agents, built on a resampling analysis showing only **19.5%** of
  DeepSWE tasks (22 of 113) have a fifth-percentile Pearson correlation ≥ 0.50
  with full-benchmark performance, and explicitly scoped to repeated
  baseline-vs-candidate comparisons rather than model rankings. Called
  "open-source" in the abstract, but the full text links only third-party
  repositories, so **no artifact was found**; excluded this window, **watch**.
- **Inference-Engine Fingerprinting Attacks are Practical** (`2609.20614`,
  cs.AI/cs.CR): a misaligned model escaping the inference engine by generating
  crafted output tokens, argued as a sandboxing gap because discussion focuses on
  network proxies and code-execution environments. A substantial safety result
  for the runtime-isolation line, but it is a digital attack demonstration with
  no artifacts and no robot component; the sandbox line in this list is carried
  by LoopHarness, HarnessRisk, and Bounded Agents. Excluded this window.
- **AdvScene / Safety-Critical Scenario Emerges from Initial Scene**
  (`2609.20103`, cs.RO): safety-critical scenario generation recast as an
  **initialization** problem (20.44% of Waymo Open Motion slices have a
  stationary ego; 30.39% of initial frames have no participant within 10 m).
  Excluded as autonomous-driving domain work under the standing policy.
- **Collision Mesh Poisoning** (`2609.18122`, cs.CR/cs.RO): a poisoning attack
  through the 3D-asset supply chain exploiting the legitimate Visual–Collision
  Gap in simulators. A relevant simulator-integrity result, but it is a digital
  attack paper whose evidence is simulation-side, and the VLA-attack line is
  carried by RedVLA, DropVLA, and the bit-flip work. Excluded; **watch**.
- **Position/domain and digital-agent papers screened and set aside** as too far
  from a robot or general harness contract to justify an entry this window:
  `2609.19961` (neuro-symbolic agentic AI for low-altitude UAV networks),
  `2609.19538` (agentic AI networking for heterogeneous UAS), `2609.19315`
  (GAVEL graph world models for LLM task planning), `2609.19391` (MAGS
  auto-formalization), `2609.19524` (unified trustworthiness evaluation),
  `2609.19664` (VideoResearcher self-improving tool design), `2609.19759`
  (multi-agent collaboration), `2609.20330`
  (RoboFind multi-agent object search), `2609.20116` (GPT-6-Astra in a
  navigation workflow, project page only), `2609.19226` (PAPC privacy
  propagation), `2609.19475` (FASA diffusion VLA sampling), `2609.19666` /
  `2609.19923` / `2609.20648` / `2609.20659` / `2609.20776` / `2609.19579`
  (VLA post-training, federated VLA training, classical-planning step skipping,
  human-in-the-loop UMI post-training, geometry-based action chunking, pruned-VLA
  hidden-state distillation — model/adaptation contributions without an external
  runtime or a released artifact), and the conventional control, SLAM, navigation,
  gripper-design, exoskeleton, and agricultural-robotics records that the keyword
  pass surfaced.
- **Watch-list re-checks (no change):** **FIERCE** (`github.com/ar-mine/FIERCE`)
  is still 0 KB with only `README.md` (created 2026-09-15, pushed 2026-09-16);
  **ContrAgent** (`github.com/yfxiao16/ContrAgent`) still returns 404. Neither
  released an artifact in this window.

## Operational notes

- The new block is larger than the previous day's (905 vs. 898 unique IDs) and
  the new-frontier share is 590 records, so screening was keyword-breadth ranked
  over the **whole** block rather than only the frontier IDs; the 28 churned
  records were re-read as revisions and the 254 pre-`2609.` records were screened
  by title and category.
- The OAI route remains the block definition (1,232 memberships → 905 IDs); the
  export API sees only 412 of those IDs and is used strictly as a subset check.
- `curl -g` is required for `submittedDate:[…]` queries — see the network note in
  Method.
- GitHub REST verification succeeded throughout; no 403 rate limiting occurred
  this run, so the `git ls-remote` fallback was not needed.
- No network, authentication, conflict, or artifact-verification blocker
  remained at the end of the run.

## Validation performed

- **Markdown structure:** `##`/`###` heading order and blank-line separation in
  `README.md` unchanged; the new `sources/daily-arxiv-2026-09-18.md` follows the
  established record structure (Scope / Method / Included / Rejected / Operational
  notes / Validation).
- **`git diff --check`:** clean (no whitespace errors, no conflict markers).
- **Added links:** every URL added to `README.md` was requested during this run —
  `arxiv.org/abs/…` for all twelve entries, the SoL-Pi repository and project
  page, the Astronex repository, weights, and project page, the HALTER project
  page, and the τ²-bench/EdgeBench references are inside prose rather than links.
  All returned HTTP 200 except the deliberately recorded 404s noted above.
- **Dates:** README badge and "Last verified" both set to **2026-09-18**, equal
  to this record's date; entry dates quoted in prose are the arXiv `<created>`
  dates from the OAI harvest.
- **Categories:** every entry's section placement follows this list's own
  taxonomy (General Harness Design; Agentic Robot and VLA Harnesses; Robot
  Foundation and World Models; Benchmarks and Evaluation) and matches the
  classified arXiv categories recorded here.
- **Cross-file consistency:** no arXiv ID added to `README.md` duplicates an ID
  already present in `README.md`, `docs/landscape.md`, `docs/reference-architecture.md`,
  or any other `sources/*.md` (checked by ID before writing); the README
  "Last verified" date equals the newest record's date.
- **Artifact claims:** each open/closed statement was verified against the
  GitHub or Hugging Face API on 2026-09-18 (license field, size, creation and push
  dates, and repository contents) or against the project page, and every
  unverified or placeholder artifact is described as such.
