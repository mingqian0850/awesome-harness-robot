# Daily arXiv scan — 2026-10-24 (21-day catch-up)

## Scope

- **Interval covered:** everything announced or submitted after the 2026-10-02 run's
  cutoff — **submission dates 2026-10-02 through 2026-10-22 inclusive**
  (announcement blocks 2026-10-05 through 2026-10-23). This is the first
  successful scan since **2026-10-02 10:53 CEST**, so it is a catch-up run, not a
  daily one.
- **Why the gap exists.** The maintenance log
  (`/home/chenm/.dsh/arxiv-maintenance.log`) shows every scheduled slot from
  **2026-10-05** through **2026-10-23** failing with
  `dsh: QUOTA: Insufficient Balance` and `state NOT updated`; the state file
  (`/home/chenm/.dsh/arxiv-maintenance.state`, epoch `1790931200` =
  2026-10-02 10:53 CEST) confirms no successful run in between. The
  2026-10-05 10:12 run harvested the 2026-10-05 block and then exited 1 without
  screening it; that harvest was reused here (2,376 records, 1,028 of them
  announced 2026-10-05) rather than re-fetched.
- **This run started** 2026-10-24T00:05 CEST (2026-10-23T22:05 UTC) on `main` at
  `75ce179`.
- **Newest visible batch.** The export API indexes submissions with roughly a
  two-day lag: `submittedDate:[20261023 TO 20261023]` and `[20261024 TO 20261024]`
  return **0** for all five categories, while 2026-10-22 returns 443 unique IDs.
  So submissions from 2026-10-23 onward are **not yet visible** and are explicitly
  left for the next run.
- **No missed announcement is silently skipped.** Coverage was taken from the
  submission-date axis over the whole interval rather than from one announcement
  block, so the weekend gaps (2026-10-10/11, 10-17/18) and the late 2026-10-05
  block are inside the window by construction.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query` over
  `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, `cs.LG`, in 3-day batches, `max_results=2000`,
  with **two sweeps**: `submittedDate:[lo TO hi]` for new submissions and
  `lastUpdatedDate:[lo TO hi]` to catch records whose latest version landed in the
  window (revisions). Memberships fetched:

  | sweep | cs.RO | cs.AI | cs.CL | cs.CV | cs.LG | total |
  | --- | --- | --- | --- | --- | --- | --- |
  | `submittedDate` | 1,608 | 4,556 | 2,378 | 2,735 | 4,845 | **16,122** |
  | `lastUpdatedDate` | 1,528 | 4,306 | 2,261 | 2,598 | 4,557 | **15,250** |

  Merged and de-duplicated by base ID: **13,769 unique records** (plus the reused
  2026-10-05 OAI block). Per-day submission counts are continuous from
  2026-10-02 (893) through 2026-10-22 (443), with the expected weekend troughs.
- **Rate-limit incident and workaround.** After roughly fifteen requests the API
  began answering this IP with **HTTP 429** (and arXiv's HTML listing pages did the
  same). Harvest requests therefore fell back to the `r.jina.ai` reader with
  `X-Return-Format: html`, which returns the raw Atom XML unchanged; the script
  retried the direct endpoint periodically and switched back as soon as the block
  cleared. OAI-PMH was attempted first and abandoned: `oaipmh.arxiv.org`
  `ListRecords` stalled or returned **HTTP 503**, so it is not usable on this host
  today.
- **Screening.** Keyword/profile screening over title, abstract and comment
  (harness, agentic loop, self-evolution, skill, memory, recovery, runtime, safety,
  evaluation × robot/embodied/manipulation/VLA/world-model), then title triage of
  **2,210** robot-ish records and of **all 117 records whose title contains
  "harness"**, then abstract-level review of 67 shortlisted papers and
  artifact-level verification of all 19 included ones.
- **Deduplication.** 116 harvested IDs already exist in `README.md`, `docs/` or
  `sources/`; none was re-added. Every included paper was additionally grepped by
  title across the repository before insertion.
- **Revisions.** The `lastUpdatedDate` sweep found **no** revision of an
  already-curated ID inside the window, so no existing entry needed updating. One
  included paper (`2606.10577`) is itself a v3 revision.

## Included (README updates)

19 entries were added, grouped as they appear in the README.

### Harnesses and Development Platforms — General Harness Design and Self-Improvement

- **What Does a Harness Buy? Tokens, Mostly** (`2610.04433`, cs.AI/cs.LG/cs.SE;
  v1 2026-10-03) — calibrates the measurement before making the comparison: five
  models × three production harnesses (Claude Code, mini-SWE-agent, OpenCode) on
  SWE-bench Verified with same-configuration reruns as the noise floor. Reported:
  on 447 tasks (two models) the heaviest and lightest harness are equivalent within
  five points; on a 45-task hard subset (five models) a harness swap flips 13% of
  tasks, the same rate as rerunning the same harness; the single noise-clearing
  harness effect is a loss (OpenCode up to 9 points behind, half of it in runs its
  output cap truncated); cost per task varies up to 3× and is set by the
  system-prompt/tool-schema preamble scaled by step count. **Artifact:** MIT
  repository `YangzeLiu/what-does-a-harness-buy` ships `data/trials.csv`, the
  analysis scripts and `reference_outputs/`, with an offline `reproduce.sh` that
  regenerates every number, table and figure — verified live 2026-10-24. Digital
  agents only; no robot experiment.

- **What Does a Harness Repair? A Preregistered Study of Visibility, Baseline
  Adequacy and Evaluation Defects** (`2610.05533`, cs.AI/cs.CL/cs.LG; v1
  2026-10-04) — preregistered decomposition of a harness-search gain into
  unreadable answers, weak comparisons and evaluation defects: three small models
  × three benchmarks, replication/test partitions, a GEPA search arm, six injected
  evaluation defects applied one at a time, all 47 primary endpoints reported.
  Reported: thinking-off beat a capped-thinking setting in 5 of 9 model–benchmark
  cells, each time mostly on questions where the capped setting produced no
  readable answer; thinking-off was not meaningfully worse than a rescue
  configuration or four GEPA-selected harnesses in 11 of 13 comparisons (losing on
  GSM8K for two models); a serving-engine thinking budget cut truncation and raised
  the parse rate in 6 of 9 cells. **Artifact:** frozen preregistration, addenda
  (five row keys withheld) and aggregate analysis report as arXiv ancillary files;
  no code release.

- **Harness-Delta Attribution (HDA)** (`2610.22466`, cs.AI; v1 2026-10-15) — an
  evaluation protocol that splits an evolution gain into artifact-driven
  overfitting, gain explained by added test-time compute, and a residual upper bound
  on transferable improvement. Applied to 11 executor models × three
  harness-evolution frameworks over math, coding, creativity and agentic tasks:
  21 settings show a statistically significant search-set gain, of which
  overfitting explains **54%** and test-time scaling **14%** on average (combined
  >50% in 14 settings), with poor held-out transfer. **Artifact:** none located
  as of 2026-10-24.

- **Elicitation or Expansion? Demystifying Gains from Agent Harness Optimization**
  (`2610.27889`, cs.LG; v1 2026-10-20) — distinguishes more reliable success on
  reachable tasks (elicitation) from a wider reachable-task set (expansion) by
  porting pass@k coverage analysis from RL to optimized-vs-pre-optimization
  harnesses, contrasting harness gains with model scaling and extra inference-time
  compute, and conditioning on task difficulty. Reported finding: a harness's
  pass@1 advantage can reverse at larger attempt budgets, and aggregate gains hide
  shortfalls on hard tasks. **Artifact:** MIT repository
  `furiosa-ai/elicitation-or-expansion` plus a Hugging Face trajectory dataset,
  both verified 2026-10-24.

- **Harness Evolution Hits a Ceiling: When Weight Training Should Begin**
  (`2610.11655`, cs.AI/cs.CL/cs.LG; v1 2026-10-08) — labels failures by the first
  signal that fires to separate process failures (blocked calls, loops, exhausted
  step budgets) from content failures (a poor delivered plan), and shows harness
  evolution repairs the former while the latter is what weight training is for.
  Reported on DeepPlanning: held-out score 0.16→0.30 (Qwen3.5-4B) and 0.32→0.44
  (Qwen3.5-9B); LoRA adapters trained on evolved-harness trajectories add +0.13
  held-out under the original harness, stack with the harness past 2× on 4B, and on
  9B match the full evolution line alone. **Artifact:** none located as of
  2026-10-24.

### Robot Agent Systems — Agentic Robot and VLA Harnesses

- **RobotUse: Allocating Computation, Context, and Decisions** (`2610.04929`,
  cs.RO; v1 2026-10-04, v2 2026-10-06) — a robot agent harness that relocates the
  decision boundary: the agent picks targets and poses visually, the backend owns
  geometry/planning/control, subagents keep subgoal-local interaction detail and
  return only what later decisions need, and a persistent playbook is updated from
  execution ("continual harnessing"). Each tool declares its contract in its own
  `tool.py`, discovered and validated at startup. Reported: **45%** task success on
  RoboLab, **+6.7 points over CaP-X**, compact decision contexts, and learning that
  transfers from imperfect real-world feedback. **Artifact:** Apache-2.0
  `robotuse-team/RobotUse` with agent roles, tools, runtime, RoboLab integration,
  task verifier, setup scripts and CPU regression tests; project page live —
  verified 2026-10-24.

- **HexaShell: A Thin Harness for Exploring the Limits of VLM Agents as Robot
  Coding Agents** (`2610.23479`, cs.RO; v1 2026-10-16, v2 2026-10-20) — the harness
  reduced to one command line that makes no decisions, with an acting agent per
  episode and an optimizer agent that reads the full episode record after a failure
  and writes or repairs tested tools; tools use only RGB, calibration and arm state
  so they port to a different real arm. Reported on RoboDojo's official hidden seed
  (42 tasks × 50 episodes): **46.9% success, 53.2 progress**, described as SOTA
  there, with memory tasks at 81.0% and precision tasks at 32.0% — the stated limit
  is contact, not reasoning. **Artifact:** Apache-2.0
  `HexaFuture/HexaShell` (128★, evolved tools included) verified 2026-10-24.

- **MobiAgent: Dual-Loop Recursive Policy Self-Improvement for Long-Horizon Mobile
  Manipulation** (`2610.03476`, cs.AI/cs.RO; v1 2026-10-02; CoRL 2026) — an inner
  deployment loop (receding-horizon VLM planning, composable atomic skills,
  flow-matching experts over a shared VLM backbone, visual reflection) and an outer
  loop that segments and verifies its own rollouts, clusters them into new skills
  and fine-tunes the library without human annotation. Reported: **+22.5 points
  over π0.5-TA on BEHAVIOR-1K**; data recycling raises success **7.50% → 27.50%**
  (RoboCasa) and **32.5% → 57.5%** (Astribot S1). **Artifact status is literal:**
  repository public but asserting **no licence**; Hugging Face checkpoint gated
  behind manual approval — source-available code, not open source, and weights not
  openly downloadable (verified 2026-10-24).

- **FastHarness: Experience-Driven Review Scheduling for Vision-Language-Action
  Policies** (`2610.31427`, cs.RO; v1 2026-10-22) — makes reviewer frequency the
  controlled variable: a fast path shrinks GPT-6 Astra review of a frozen π0.5 as
  stage memory accumulates, a recovery path spends supervision only on corrections
  and resumes routine execution once a repair is verified. Reported over four
  simulated tasks: reviewer tokens **−57.2%** per task (first evaluated episode vs
  mean of final five), mean episode time **85.5 → 52.0 min**, success
  **80% → 97.19%**. **Artifact:** MIT `FastHarness/fast-harness` with `tests/` and
  packaging, verified 2026-10-24.

- **SpatialHarness: Test-Time Spatial Scaffolding for Fine Robotic Manipulation**
  (`2610.12457`, cs.RO; v1 2026-10-08) — treats fine-manipulation failure as
  insufficient *spatial observability*: an online simulated scene is kept
  synchronized with execution (static/held/transition modes) and complementary
  virtual views are rendered to a frozen policy with no fine-tuning and no sensor
  change. Reported on four real-robot tasks with frozen GPT-6 Astra: plug insertion
  **26.7% → 66.7%**, Tower of Hanoi **0% → 100%**. **Artifact:** no first-party
  release located; the full text links only the third-party
  `robocurve/inspect-robots` evaluation framework. Real-robot evidence,
  author-reported.

- **EnactOpt: Diagnosis-Guided Harness Evolution for Embodied Agents**
  (`2610.14977`, cs.RO; v1 2026-10-10) — separates evidence *missing from the
  input* from evidence *present but unused*, then jointly evolves reusable textual
  skills and context-construction code with models and runtime frozen, diagnosing
  adjacent decisions by relating action feedback to the actual input and following
  action, with compatibility checks and validation-based retention. Reported:
  roughly **+10 to +27 points** success over initial harnesses on VLABench,
  ALFWorld and Embodied Agent Interface, ahead of the evaluated GEPA, TextGrad and
  Trace2Skill baselines. **Artifact:** none of the authors' own; the paper links
  third-party VLABench assets and a π0-style checkpoint.

- **From VLM Planners to Robotic Agents: Self-Evolving Harnesses for
  History-Dependent Manipulation (SERA)** (`2610.29560`, cs.RO; v1 2026-10-21) —
  makes non-Markovian task context the harness's responsibility: a fixed VLA
  executor acts, an editable harness maintains progress and past outcomes, and
  self-evolution proposes/evaluates revisions on rollouts with the selection frozen
  before held-out testing. Reported: **51.38%** overall success on RoboMME with
  autonomous selection vs **44.38%** without self-evolution. Simulation-only;
  no artifact located.

- **Code2Worlds-2: Physically Plausible Agent Harness for Embodied World
  Simulation** (`2610.17651`, cs.CV/cs.RO; v1 2026-10-13) — puts the simulator
  inside the agent loop: a SimReady Scene Compiler assigns collision meshes,
  estimates physical attributes with a VLM, infers a support graph, instantiates
  contact/friction/dynamics constraints, gravity-settles and exports MuJoCo MJCF,
  after which a compile–simulate–critique–refine harness evolves physical settings
  as well as code. **Artifact:** `AIGeeksGroup/Code2Worlds-2` (modules, benchmark,
  demo scene, third-party tree) public but asserting **no licence**; project page
  live — verified 2026-10-24.

- **AgenticNav: Zero-Shot Vision-and-Language Navigation as a Tool-Calling
  Harness** (`2606.10577`, cs.RO; **v1 2026-06-09, v2 2026-07-26, v3
  2026-10-02**) — the v3 revision inside this window; a lightweight navigation
  harness exposing action (direct target-pixel selection), on-demand pixel-depth and
  recall-hooked memory (reasoning history plus a compact map image) as callable
  tools. Reported: **76% success / 66.50% SPL** on R2R-CE under same-VLM
  comparison, plus real-world runs on two robotic platforms. **Artifact:** no
  official code link on the arXiv record as of 2026-10-24.

### Benchmarks and Evaluation — Manipulation and VLA

- **Video2World: Benchmarking Coding Agents for Interactive World Modeling from
  Embodied Videos** (`2610.04432`, cs.AI/cs.CV/cs.RO; v1 2026-10-03, v2
  2026-10-09) — a sandboxed software-engineering benchmark: the agent gets the
  video, simulator APIs, docs and assets but no scene model, trajectory, robot state
  or task spec, and the evaluator runs the submitted environment from a fresh
  initialization so every object-state change must arise physically. **222
  instances / 39 task families / 189 videos**; scored on geometric fidelity,
  dynamic fidelity, functional correctness. Reported across **9** frontier
  coding-agent systems: task success rises from **<5% to >15%** starting with
  Claude Opus 5, large gap to human-assisted reconstruction, and better visual
  fidelity does **not** predict task success. **Artifact:** Apache-2.0
  `AetherLabsAI/Video2World` (199★), public dataset and live leaderboard — verified
  2026-10-24.

- **Does a Learned Corrector Beat a Simple Retreat? Evidence from a Frozen VLA**
  (`2610.06921`, cs.RO; v1 2026-10-02) — supplies the control runtime-recovery
  claims usually lack: per-seed paired rollouts with and without correction plus a
  same-seed base-policy re-run as **placebo**, seed-cluster intervals, prespecified
  comparison rules. Reported over **3,888 paired episodes** on frozen π0.5 across
  four RoboTwin tasks: **+13.5 pp** (95% interval [+9.4, +17.7]) on
  beat/block/hammer and **no detectable gain on the other three**; **43.2%** of
  failed base episodes recover on a plain re-run vs **63.5%** after correction,
  i.e. many nominal rescues are base-policy variance; a fixed-time trigger with
  scripted retreat to an earlier joint configuration matches the learned pipeline
  within the study's resolution. **Artifact:** none located.

### Surveys and Reading Lists

- **Towards Agentic Robots: Model, Data, Environment, and Harness** (`2610.20442`,
  cs.RO; v1 2026-10-14) — organizes agentic robotics into **17 categories across
  four functional domains** (Model, Data, Environment, **Harness**) after tracing
  VLAs and world/world-action models to agents that reason, execute, collect data,
  rectify errors and adjust strategy. Literature synthesis; no artifact asserted.

### Runtime, Safety, and Observability — Runtime Building Blocks

- **HEAR: Harness–Engine Pairing Protocol for Agentic LLM Serving**
  (`2610.06597`, cs.AI; v1 2026-10-05) — turns the harness↔inference-engine
  boundary into a protocol: the harness declares workflow dependencies, context
  lifecycle and execution requirements; the engine returns queue/KV-cache/resource/
  capability state and outcomes; protocol semantics are separated from optimization
  policy. Instantiated for online cache-aware coordination and workload-aware
  execution-mode selection. Reported: **1.61× batch speedup** and **2.23×** lower
  median time-to-first-token on SCBench under memory-constrained concurrent
  serving. **Artifact status:** the anonymised link
  (`anonymous.4open.science/r/hear-3D1E/`) returns **HTTP 401**, so nothing is
  recorded as open.

### Runtime, Safety, and Observability — Safety

- **From Approval to Abuse: Privilege Escalation Attacks on Harness-Protected
  Coding Agents** (`2610.22987`, cs.AI; v1 2026-10-16) — attacks the
  *permission-update* path: adversarial repository context steers an agent into
  proposing a plausible but broad allow-rule, and the approved rule then suppresses
  prompting for all future matches. Reported: target allow-rules elicited in **58%**
  of experimental instances on production platforms including Claude Code and Codex
  CLI, with acquired rules covering **56.9%** of category-specific capability test
  cases. **Artifact:** none linked.

## Revision edits to existing entries

None. The `lastUpdatedDate` sweep found **no** revision of an already-curated ID
between 2026-10-03 and 2026-10-22, so no existing README entry needed a status or
number update.

## Current Landscape additions

Five bullets were appended:

1. Harness gains dissected rather than asserted (Harness Buy, HDA, Harness Repair,
   Ceiling).
2. Harness–serving protocol and the harness permission surface as an attack target
   (HEAR, From Approval to Abuse).
3. Video-to-simulation as a coding-agent benchmark (Video2World, Code2Worlds-2).
4. Recovery and observability as measurable harness contracts (Learned Corrector,
   SpatialHarness, FastHarness).
5. Robot harnesses published as runnable runtimes with tool contracts and learnable
   playbooks (RobotUse, HexaShell, MobiAgent) plus the field-mapping survey
   (Towards Agentic Robots).

## Rejected / watch list

### Verified and rejected / held for artifacts

- **CarryOn: A Dual-Loop Harness for Personalized and Reliable Robot Assistance**
  (`2610.21453`, cs.CV/cs.RO) — on topic (planning loop adapting tasks to the user,
  execution loop checking contact risk and repairing failures, around a frozen VLA)
  but the repository `JIA-Lab-research/CarryOn` is **8 KB** and its README states
  "**Code will be released soon in three weeks**" (news entry 2026-10-15); the
  project page is live. Treated literally as not open. **Watch.**
- **EMHO: EMbodied Agent Harness Optimization via Experience Traces**
  (`2610.08432`, cs.AI) — a genuine harness-optimization-for-embodied-agents paper,
  but no first-party artifact: the full text links only a third-party Qwen model
  card. **Watch.**
- **Self-Evolving Agent Harness** (`2610.18813`, cs.AI) — links
  `zgcagi/Harness-RSI` (404★, pushed 2026-10-23) that asserts **CC-BY-4.0**, which is
  a document licence rather than a code grant; contents were not verified in this
  pass. **Watch** on licence/contents.
- **Skill2Real** (`2610.02788`), **RoboBridge** (`2610.02717`), **Recursive
  Self-Improvement of Visuomotor Policies through Local Recovery Supervision**
  (`2610.05151`), **AgenticTactileVLA** (`2610.04391`), **DiVeR** (`2610.04933`),
  **Flow Policies as Actions of Skill-Level World Models** (`2610.04767`),
  **R²-WAM** (`2610.04913`), **UniIntervene++** (`2610.03620`) — all substantive
  agentic/recovery/skill/verifier results inside the window, but no artifact URL
  appears on the record or in the comment of any of them (R²-WAM and the flow-skill
  paper have project pages only). Recorded so a later run does not re-litigate them
  from title alone. **Watch.**
- **Beyond LLM Serving: Characterizing VLA Workloads for Embodied AI System
  Design** (`2610.05062`, cs.AR/cs.RO, ASPLOS '27) — VLA serving characterization
  with real per-control-period deadline analysis; adjacent to the runtime section
  but it contributes no harness contract or artifact, and serving-characterization
  work is already represented. **Deferred.**

### Screened and deferred: another harness-evolution / harness-search variant

The window contains **117 records whose title contains "harness"**. Beyond the
ten included papers whose titles name a harness, the following were screened at
title+abstract level and deferred: each optimizes prompts/tools/workflows of a
**digital** agent without a distinct runtime contract, a decisive artifact, or a
robot experiment, and the list already carries this family (Meta-Harness,
Darwin–Gödel Machine-style search, Self-Harness, HarnessLens, ModularRSI,
HGM-style lineage search, …):

- Recursive/self-evolving harness search: `2610.05300` (MESH-Harness),
  `2610.15150` (Harmony Harness), `2610.15221` (HarnessNet), `2610.26770`
  (HarnessPro), `2610.28201` (MaD-RSI), `2610.28380` (Object-Oriented Harness),
  `2610.14224` (Harness²), `2610.17370` (HarnessTTT), `2610.17609`
  (Verifier–Harness Co-Evolution), `2610.28815` (Successive Scale Racing),
  `2610.30042` (HGM-Dual), `2610.25568` (Design and Cross-Domain Evaluation),
  `2610.16725` (Trace-to-Architecture), `2610.26632` (When the harness decides
  success), `2610.28798` (Feedback Contract), `2610.11231` (Harness Compilation).
- Harness–training co-evolution: `2610.26426` (HarnessRL), `2610.29458` (Harness
  Scaling), `2610.31401` (HERD), `2610.25078` (CoEvo-RSI), `2610.14118` (ClawLoop),
  `2610.13025` (Evo²), `2610.20261` (Harness Policy Distillation), `2610.26342`
  (VibeTraining-Bench), `2610.27435` (RiseHarness).
- Harness-engineering benchmarks/diagnostics: `2610.20344` (Doc2Harness),
  `2610.18090` (AM-Bench), `2610.21098` (ToolBottleneckBench), `2610.17616`
  (StateHarness), `2610.13075` (LongHarness).
- Single-domain harness instantiations (medical, chemistry, finance, CAD, charts,
  security-ops, GUI): `2610.05778`, `2610.15714`, `2610.03056`, `2610.23791`,
  `2610.24744`, `2610.28282`, `2610.26101`, `2610.02617`, `2610.12274`,
  `2610.09489`, `2610.21755`, `2610.21266`.
- "Harnessing"-in-the-ordinary-sense titles, not harness engineering: `2610.10133`
  (image restoration), `2610.15431` (SPAD depth), `2610.24757` (segmentation),
  `2610.27999` (Matrix-Game 4.0), `2610.31162` (HiSTEVE), `2610.06394` (HOI
  detection), `2610.15401` (VLM reward regressor), `2610.15389`
  (VideoEvoHarness), `2610.30917` (optimization modelling).

### Carried watch list — unchanged this window

- StageWAM (still v3), ReflexVLA/ReflexBench (post-acceptance), DreamX-Phi
  (README-only), UniTexture (no code), PRISM ("Dataset soon"), GigaBrain-0.7/WBC
  (release unconfirmed), ForceU-VLA (README-only), LIBERO-VIFO, Agent Lightning,
  VLCP, Hydra-0, BATON — none of these appeared as a new version or a new artifact
  in this window.
- The thirteen IDs carried by the 2026-09-30 run plus RoboHarn-Evo (`2609.37583`),
  SelfSearch (`2609.37968`), F4R (`2609.35575`), ASENA (`2609.39207`),
  Multi-Link Safety Filtering (`2609.40007`), Blackout vs. Freeze (`2609.39145`),
  ActionGuard (`2609.39450`), Who Verifies the Graph (`2609.40027`), HIDE/SEEK
  (`2609.38886`), TeV (`2609.39038`) — no artifact news in this window; carried
  forward with their previously recorded status.

## Validation performed

- Markdown structure: heading hierarchy intact (`README.md` now 1,174 lines,
  39 section headings); all 37 added lines checked for balanced `[]`/`()` and for
  every `[text](url)` resolving to an absolute URL — zero issues.
- `git diff --check` reports no whitespace errors.
- All **36** distinct URLs added to the README were fetched: **36/36 return
  HTTP 200** (arXiv abs pages, project pages, GitHub repositories, Hugging Face
  dataset/model pages, leaderboard).
- Artifact claims were checked against the live repositories rather than the paper
  text: Apache-2.0 confirmed for RobotUse, HexaShell and Video2World; MIT confirmed
  for FastHarness, What Does a Harness Buy and Elicitation or Expansion; **no
  licence** confirmed for MobiAgent and Code2Worlds-2 (both recorded as
  source-available); manual-approval gating confirmed for the MobiAgent checkpoint;
  `anonymous.4open.science` 401 confirmed for HEAR; the CarryOn repository's
  "code in three weeks" notice read from its own README.
- Duplicate check: each of the 19 IDs was grepped across `README.md`, `docs/*.md`
  and `sources/*.md` before insertion — none was present, and none was added twice.
- Dates and categories cross-checked against the arXiv records (v1 date and latest
  version date for all 19, including the `2606.10577` v3 revision).
- Cross-file consistency: README badge, `Last verified` line and this record all
  carry 2026-10-24; the interval in this record matches the maintenance log gap.

## Operational notes and blockers

- **arXiv 429 (blocker, worked around).** The export API began returning HTTP 429 to
  this IP after ~15 requests; arXiv HTML listing pages returned 429 as well.
  Requests were routed through the `r.jina.ai` reader with `X-Return-Format: html`,
  which passes the Atom XML through unchanged. The direct endpoint recovered
  mid-run and both paths were used. Any future run that sees 429 should slow to
  ≥10 s between requests rather than retrying at 3 s.
- **OAI-PMH unusable today.** `oaipmh.arxiv.org` `ListRecords` stalled past 280 s and
  later returned **HTTP 503**; `Identify` works. The export API is the working
  source on this host.
- **Indexing lag.** Submissions dated 2026-10-23 and 2026-10-24 are not yet in the
  index (0 results for all five categories), so the next run must re-scan
  2026-10-23 at minimum.
- **`/tmp` is not persistent between tool calls on this host** — harvests and URL
  lists must live under `.scratch/`, which stays untracked and is never committed.
- **Repository hygiene.** `docs/reference-architecture.md` (modified) and
  `docs/ring-harness.png`, `handoff.md`, `.scratch/` (untracked) are pre-existing
  user state and were left untouched; only `README.md` and this file are staged.
