# Daily arXiv scan — 2026-10-27 (2026-10-24 → 2026-10-26: 949 records, because the weekend batches were never screened)

## Scope

- **Interval owed.** The previous successful run finished on `main` at `338b0de` (record `daily-arxiv-2026-10-26.md`, run
  started 2026-10-26 13:24 UTC). It screened the 2026-10-23 batch in full and reported **zero** records for 2026-10-24,
  2026-10-25 and 2026-10-26, concluding that the weekend "carries no submissions in these categories" and that
  2026-10-26 was still inside the harvesting day. This run started **2026-10-27 12:53 UTC** on `main` at `338b0de`, so
  the interval it owes is **submission dates 2026-10-24 → 2026-10-27**.
- **That conclusion was wrong, and the correction is the main scope finding of this run.** The frontier had moved
  again: an unrestricted descending `cs.RO` query returns **2026-10-26T17:59:11Z** (`2610.35862`) as the newest
  record, versus `2610.32978` (2026-10-23T17:59:21Z) at the previous run. Re-querying the three days the previous run
  recorded as empty returns **241, 234 and 474** records — **949 records that no prior run has ever screened**. The
  weekend days do carry submissions; they had simply not been indexed when the previous run asked. **Operational
  consequence: a zero day-count must not be read as "no submissions" unless a frontier probe shows the index has
  reached that day.** Future runs should therefore re-query any zero day once the frontier has advanced past it, which
  is exactly what this run had to do.

  | query (one OR-query over cs.RO/cs.AI/cs.CL/cs.CV/cs.LG per day) | 2026-10-24 | 2026-10-25 | 2026-10-26 | 2026-10-27 |
  | --- | ---: | ---: | ---: | ---: |
  | `submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` | **241** | **234** | **474** | 0 |
  | `lastUpdatedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` | **241** | **234** | **474** | 0 |
  | previously recorded by the 2026-10-26 run | 0 | 0 | 0 | — |

- **The batch.** 949 unique base IDs. `published` spans **2026-10-24T00:08:11Z → 2026-10-26T17:59:44Z**. Every record
  is **v1** — no record in the window carries a second version, and for all 949 the `<updated>` date equals the
  `<published>` date.
- **Revisions.** The `lastUpdatedDate` totals are *identical* to the `submittedDate` totals on all four days
  (241/234/474/0). Since every window submission has its own `lastUpdatedDate` inside its own day, equality of the
  totals means the `lastUpdatedDate` set **is** the submitted set: **no paper submitted before 2026-10-24 was revised
  during the window**, so no existing README entry needed a status, version or number update.
- **Coverage.** All 949 carry at least one of the five target categories. Primary category counts: `cs.LG` 201,
  `cs.CV` 193, `cs.AI` 153, `cs.CL` 126, `cs.RO` 108, `cs.SE` 24, `stat.ML` 23, `cs.CR` 23, `cs.IR` 10, `cs.SD` 10,
  `cs.DC` 7, `math.OC` 7, with the remainder single papers in physics, quant-ph, q-bio, eess, cs.AR/CE/CY/ET/FL/GR/GT/
  HC/LO/MA/MM/NE/NI/PF.
- **Consequence, per procedure.** This is a **first-pass screen of new material**, not a re-screen: 949 records no
  earlier run has seen. No tail re-screen of 2026-10-23 was needed — the previous run screened it twice and this run's
  deduplication check confirms none of its 13 inclusions appears here.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`, saved under `.scratch/arxiv-2026-10-27/`
  (untracked, never committed). **One OR-query per day across all five categories**, paginated at 200, sorted by
  `submittedDate` ascending: 2026-10-24 and 2026-10-25 took two pages each, 2026-10-26 took three (200 + 200 + 74),
  2026-10-27 returned 0. **9 harvest requests**, plus 1 frontier probe and 4 `lastUpdatedDate` sweeps: **14 requests
  at ~20 s spacing, zero HTTP 429**. The one-OR-query-per-day shape from the previous run is retained — it costs a
  fifth of the per-category shape.
- **Screening.** (a) **Every one of the 949 titles was read**, not only keyword hits, in three passes. (b) Abstract
  review of **69 distinct records**. (c) Artifact verification of **15 candidates** against live sources: the GitHub
  REST API (licence, size, creation/push timestamps, stars, top-level tree) for 14 repositories, the Hugging Face model
  and dataset APIs (gating, licence tags, file counts, safetensors shards) for 5 artifacts, two Zenodo DOI resolutions
  (one of which redirected to an unrelated record — see below), and eight project pages fetched for an HTTP status.
  (d) Full-text HTML search of **25** records for release statements and repository URLs, because several inclusions
  carry no link in their API metadata and several non-inclusions *claim* a link that does not resolve.
- **Deduplication.** Every shortlisted ID and title was grepped across `README.md`, `docs/`, `sources/` and
  `editorial/` before inclusion — none was present. Two name collisions were checked explicitly: **VISTA**
  (`2610.35051`, embodied-harness self-improvement) is unrelated to the listed VISTA visual harness (`2610.02200`),
  and **EBench-Astra** (`2610.35832`) is a tech report *on* the already-listed EBench benchmark (`2606.18239`), so it
  is recorded as a harness study rather than as a second benchmark.

## Included (15)

### General Harness Design and Self-Improvement

- **Rethinking Verification in Agent Harnesses** (`2610.34577`, cs.AI; v1 2026-10-25; 65 pages with appendices) — why
  it qualifies: it turns "check the work" into a designed axis of **test-time scaling** and prices each layer of it.
  Four lifecycle layers are separated (initial instructions, mid-turn reminders, completion gates, independent review)
  and **17 designs** are compared across **seven frontier coding models** on Terminal-Bench, SWE-bench Verified and a
  new **VisualBench** (18 visual and interactive tasks, blind expert comparison). Independent review is the best
  accuracy–cost point: **+5.9–9.5 points** on TB and **+3.9–6.6** on SWE at **1.13–1.25×** and **1.00–1.04×** baseline
  cost; completion gates can gain more but can **substantially increase cost**; additional review rounds or independent
  critics add up to **+3.5** TB points. The recommendation that matters operationally is *negative*: do not make the
  main agent recheck its own work at completion, because reopening work does not reliably improve the artifact. **No
  artifact located (checked 2026-10-27):** the only Zenodo DOI in the paper (`10.5281/zenodo.20953922`) resolves to
  **Harbor**, a cited evaluation framework, not to a release of this study's designs, VisualBench, or data. Digital
  coding agents, no robot experiment; author-reported.
- **How Do Coding-Agent Harnesses Self-Evolve? An Empirical Study of Retrospective Adaptation** (`2610.34938`,
  cs.SE/cs.AI; v1 2026-10-26) — why it qualifies: it is the **efficacy test** the harness-self-evolution line has been
  missing, and it reports a mostly negative result rather than a gain. A retrospective skill asks an agent to review a
  finished session and propose harness changes; Claude Code running on 30 SWE-bench Verified issues produced **125
  proposals**, **24 were implemented**, and **12 changes** were then evaluated in fresh sessions on later issues from
  the same repositories, with and without each change. In the **seven** evaluations where sessions without the change
  actually encountered the targeted problem, two changes removed the targeted action, one reduced it, and **four
  produced no reduction**, while some changes added work on unrelated issues. **18 of 43** lint-related proposals
  recommended a practice regardless of what had happened in the session. The contribution is a contract: harness
  self-evolution needs a later-task efficacy test and a retirement path, not just change generation. **Verified open
  (2026-10-27):** Zenodo `10.5281/zenodo.23653700` (record type *dataset*, file `retro-harness-artifact.zip`) is live
  and is the authors' replication package. Digital coding agents; author-reported.
- **CTXAgent: Self-Controlled Context Management of LLM Agents** (`2610.34425`, cs.AI; v1 2026-10-25) — why it
  qualifies: it moves compaction from a harness-owned token threshold into the model's own decision, which changes who
  owns the state. The model gets **two tools and one paragraph of guidance** and decides when to compact, what to keep
  and what to write as the hand-off; seven frontier models are compared with and without the tools, and against three
  widely used harnesses on two of them, over sessions of **35–212 turns** that interleave coding exercises, multi-step
  SciCode problems and SWE-bench Pro / Terminal-Bench 4.0 tasks with planted facts, ephemeral observations and standing
  instructions. Most strong models manage themselves: live context stays mostly below **100k tokens** with no
  significant score loss. With Claude Opus 5.5 the self-managed harness **matches Claude Code** on task success at
  **34% lower cost** than its explicit 200k-window configuration and **49% lower** than its default, with no window to
  choose. **Verified open (2026-10-27):** `MatthiasHBusch/CTXAgent`, **CC-BY-4.0** (3.9 MB, created 2026-10-21, pushed
  2026-10-25; `ctxagent/`, `bench/`, `web/`, `tests/`, `tools_scripts/`); the agent, the benchmark and all result rows
  are released. Digital agents, no robot experiment; author-reported.
- **SABench: Task Success Is Not Enough — Side-effect-Aware Evaluation of Tool-Using Language Model Agents**
  (`2610.34167`, cs.AI/cs.CL, NeurIPS 2026 Evaluations and Datasets Track; v1 2026-10-25) — why it qualifies: it names
  and measures the failure that a success metric cannot see. **Collateral damage** is finishing the user's task while
  taking an unauthorized side-effecting action — writing to the wrong object, widening scope, contacting an unintended
  recipient. **201** email/calendar/ticket tasks cross four state structures (independent, dependent, externally
  reachable, cross-tool) with four trap families (ambiguity, mis-targeting, cascade harm, overreach), each with a
  deterministic trace-based oracle for **both** goal satisfaction and authorization compliance. Across **2,211**
  model–task pairs from **11** frontier LLMs, **15.2%** of runs incur collateral damage — **38.8%** on ambiguity traps
  versus **8.4%** elsewhere, **12.8%** of goal-satisfied runs still write without authorization, cross-tool and
  dependent states roughly double the rate, and the rate varies **4.7×** across models at comparable goal rates.
  **Verified open (2026-10-27):** MIT `hjj194/sabench-artifact` (518 KB, created 2026-05-04, pushed 2026-10-25;
  `sabench_re/`, `scripts/`, `traces/`) and CC-BY-4.0 `Phaedrus/sabench` (5 files, ungated) — code, traces, oracle and
  analysis pipeline are present, not promised. Digital tool use, no robot experiment; author-reported.

### Agentic Robot and VLA Harnesses

- **ThinkLocoManip: A Whole-Body Control Agentic Harness for Legged Loco-Manipulation** (`2610.34370`, cs.RO; v1
  2026-10-25) — why it qualifies: it is the first harness in this line to treat **locomotion as a tool domain** rather
  than as part of the action space, which is what long-horizon legged work actually needs. Against direct action
  generation and plan-then-act, it runs an **interleaved reasoning-and-acting** loop that orchestrates perception,
  navigation, manipulation and locomotion tools, with skills that encode both in-domain tool use and cross-domain
  coordination, and it adds a **keyframe-thinking** memory that preserves the critical execution context a long failure
  would otherwise erase — the recovery mechanism is a memory-structure decision, not a retry count. On a **Lenovo BOT
  MX** (wheeled quadruped plus an Agilex PiPER 6-axis arm, GPT-5.6 Sol backbone) the authors report **77%** success on
  long-horizon real loco-manipulation versus **37.1%** for the strongest plan-then-act baseline. Real hardware,
  author-reported. **No artifact located (checked 2026-10-27):** no repository, dataset or project page appears on the
  record or in the full text, and GitHub search returns nothing. Watch for release.
- **VISTA: Active Exploration for Self-Improvement in Embodied Agents** (`2610.35051`, cs.RO; v1 2026-10-26) — why it
  qualifies: it fixes the specific defect of reflection-based harness improvement — updates are committed from old
  rollouts that were never re-tested. VISTA maintains a **harness graph** recording the lineage of proposed harnesses,
  restarts every iteration from a retained version and its execution evidence, and then **actively explores**: it
  generates new rollouts, probes execution failures and recovery behaviours, and modifies the harness only on that
  fresh evidence before re-evaluating and adding the result to the graph. The transferable object is the lineage plus
  the active test, not the mutation operator. Evaluated on **LIBERO-Pro, RoboCasa and RoboTwin**, reported as
  progressive success gains at fewer simulator rollouts than static reflection. **Verified open (2026-10-27):**
  Apache-2.0 `AgenticEI/VISTA` (1.5 MB, created 2026-10-09, pushed 2026-10-26; `harnesses/`, `meta_harness/`, `robots/`,
  `rpent/`, `tools/`, `tests/`, `docs/`) and a live project page (HTTP 200). Simulation-only evidence; author-reported.
  **Name collision:** distinct from the listed visual-reasoning harness VISTA (`2610.02200`).
- **Profiling a Frontier Agent with EBench: From Direct Control to Harness with Policy** (`2610.35832`, cs.RO/cs.AI;
  v1 2026-10-26) — why it qualifies: it measures the **division of labour** inside a robot harness instead of its
  aggregate score, and it names the hand-back failure modes that make an agent-plus-policy system worse than the sum of
  its parts. GPT-6-Astra drives under the same visual and proprioceptive observation protocol as post-trained policies
  in two configurations — *direct control* (waypoint commands) and *harness with policy* (it may additionally invoke a
  post-trained VLA or WAM). Direct control is competitive overall, leads on short-horizon tasks and varies least across
  generalization dimensions, but underperforms on long-horizon, tabletop and high-precision contact-rich tasks. Adding
  policy invocation raises overall success to the level of the strongest post-trained policy while substantially
  cutting token cost, yet stays **below the per-task upper bound** of the standalone components because of
  **suboptimal policy invocation, fixed-instruction execution without subgoals, and grasp disruption when the agent
  resumes control**. Those three are the reusable part. Coverage: EBench's **26 test tasks / 510 episodes**.
  **Artifact status (2026-10-27):** a project page is live (HTTP 200), but **no benchmark code or data ships with this
  technical report** — EBench itself is the separately listed benchmark (`2606.18239`). **Simulation only** (EBench is
  built on Isaac Sim), one frontier model, author-reported.
- **ZAP: Pretraining Knows What, Simulation Teaches How** (`2610.35817`, cs.RO/cs.LG; v1 2026-10-26) — why it qualifies:
  it is the complementary measurement to the entry above — same harness question, opposite direction. Instead of giving
  a frontier model tools for a post-trained policy, it teaches an off-the-shelf **27B** vision-language model to drive
  through a **two-tool harness** with no grasp macros and no motion primitives, so every approach, grasp, lift and
  release is a sequence the model plans itself while a short plan stays in view across turns (Appendix A specifies the
  rest of the interface). Training is supervised fine-tuning plus GRPO on simple **simulated** tasks with **no
  real-robot data**. The authors report **96.7%** real-world success, matching GPT-6 Astra in the same harness while
  beating MolmoAct2 (**33.3%**) at **3× the speed** and roughly **17× lower cost**; transfer to an unseen embodiment
  (single-arm Franka, **55.9%** on RoboLab-120, **+4.9 points** over the leading VLA/WAM baseline); and composition on
  goals absent from training data (test-tube seating with a prompted different hole, mid-air handovers, drawing, chess).
  The recordable claim is the model-to-harness ratio: a small model plus a minimal tool interface reaches a frontier
  agent's result on the tasks tested. Real hardware, author-reported. **No artifact located (checked 2026-10-27):** no
  repository, weights, dataset or project page in the record or the full text.
- **RoboDuo: Attempt, Check and Atomic Recovery through Code for Vision-Language-Action Models** (`2610.34108`,
  cs.RO/cs.CV; v1 2026-10-25) — why it qualifies: it puts code on the **checking** side of the VLA boundary instead of
  the action side, which is the difference between a recovery mechanism and a replacement policy. A coding agent writes
  a program that lets a frozen full-task VLA attempt the entire task and tests whether it completed; on failure, an
  atomic-skill VLA finishes only the remaining steps. The agent emits no actions and the full-task policy keeps its
  weights, so its long-horizon competence is preserved rather than discarded at the first step — the failure the
  authors identify in step-one code-as-policy programs. The authors build **LIBERO-Atomic, LIBERO-Code and
  LIBERO-Judge** from one LIBERO demonstration set, report consistent gains for **X-VLA** and **π0.5** on LIBERO and
  LIBERO-PRO with the largest movement on long-horizon tasks, and recover failed rollouts of a frozen full-task VLA on
  a real robot. Author-reported; **no artifact located (checked 2026-10-27)**: the record links no repository, dataset
  or project page, and GitHub search returns nothing. Watch for release; the LIBERO-Judge checking interface is the
  artifact this line needs.

### Benchmarks and Evaluation

- **RobotAgentBench (RAB): Towards Open-world Robot Autonomy** (`2610.34649`, cs.RO/cs.AI; v1 2026-10-26) — why it
  qualifies: it evaluates the **agent** rather than the policy, and it scores **failure recovery** as a first-class
  capability instead of hiding it in aggregate success. **50** household layouts are constructed through an
  agent-assisted, human-in-the-loop real-to-sim pipeline with interactive objects and physically grounded tasks,
  spanning **90** atomic tasks, **18** short-horizon tasks, **8** long-horizon tasks and **4** open-ended scenarios.
  Task-level scoring separates five capabilities — task discovery, context reasoning, manipulation, failure recovery,
  robustness — and scenario-level scoring asks whether the agent can autonomously fulfil a high-level user need.
  Standalone VLA and world-action models are compared with different robot-agent architectures under one
  planning–execution–verification framework, and the reported result is a **gap between manipulation and autonomy**
  plus a measurable advantage for agentic architectures and recursive self-improvement. **Verified open (2026-10-27):**
  Apache-2.0 `AetherLabsAI/RobotAgentBench` (243 MB, created 2026-10-24, pushed 2026-10-27; `src/`, `resources/`,
  `docs/`, `docker/`, `tests/`), ungated `AetherLabs-AI/InternVLA-A1.5-RAB` (20 files incl. `model.safetensors`, a VLM
  subdirectory, `control_schema.json`, `stats.json`, `SHA256SUMS`) and a live project page. Simulation, author-reported.
- **RoboJev: An Open Vision-Language Judge for Robot Success Detection** (`2610.33718`, cs.RO/cs.LG; v1 2026-10-24) —
  why it qualifies: **outcome verification** is one of the harness's standing jobs, and this releases it as a model
  rather than a proprietary API or a human annotator. RoboJev-27B takes an instruction, 10 frames and optional final
  frames from auxiliary cameras and returns calibrated success/failure probabilities in a **single forward pass**,
  via a rank-32 LoRA adapter on the language model only (vision encoder frozen), 52,598 episode records, a
  source-specific KL penalty anchored on research data, and a calibrated two-token readout. Reported: **95.2%**
  balanced accuracy and **0.993** AUROC on 268 held-out SO-101 episodes, **0.890** AUROC over 459 episodes from unseen
  labs/robots, **93.3%** accuracy on 135 community-collected episodes, and **0.26 s** per episode on one H100 (about
  $0.19 per 1,000 predictions at $2.69/GPU-hour, excluding media processing). The authors state plainly that the
  frontier-API comparison is **not like-for-like** because RoboJev receives more frames. **Verified open (2026-10-27):**
  Apache-2.0 `josefchen/RoboJev-27B` (63 files; 12 safetensors shards plus a published `benchmark/` tree of jsonl tasks
  and per-model result rows) and Apache-2.0 `josefchen/robojev` (16 MB, created 2026-10-24; `robojev/`,
  `lerobot_plugin/`, `examples/`, `tests/`). Real robot video; author-reported; the released prediction table
  (**20,628** predictions across **29** model/protocol entries) is what makes the judge auditable rather than
  trusted.
- **Specola: A Tool for Specification Driven Robot Evaluation** (`2610.35730`, cs.RO/cs.FL; v1 2026-10-26) — why it
  qualifies: it gives policy evaluation a **formal specification** instead of a success bit, which is the missing half
  of the "what to measure" contract this list keeps returning to. Signal Temporal Logic written over simulator signals
  is compiled into monitors that run **while the policy executes**, so a rollout reports how close the behaviour came
  to satisfying or violating each requirement, how sensitive success is to goal-parameter changes, which requirements
  were violated, and whether success persisted. The integration surface is the contribution: a Python API **and** CLI
  that drop into existing execution loops, an adapter interface for new simulators, and adapters for **nine** simulation
  environments and benchmarks including LIBERO, RoboCasa, Isaac Lab and RoboDojo. **Verified open (2026-10-27):** MIT
  `specola-eval/specola` (5.7 MB, created and pushed 2026-10-26; `src/`, `benchmarks/`, `examples/`, `tests/`, `docs/`,
  `scripts/`). Evaluation infrastructure rather than a policy result; case studies are author-reported.
- **EmbodiedLab: Benchmarking Embodied Agents That Act to Know** (`2610.35385`, cs.RO; v1 2026-10-26, technical report)
  — why it qualifies: it separates *acting to change* from *acting to know*, and its harness ablation is a direct
  measurement of what scaffolding adds. **30** quantitative science experiments in **five** disciplines on **14**
  apparatus platforms run in a physics-based simulated laboratory in which the system parameters are **resampled for
  every instance**, so the answer cannot be memorised yet can be scored exactly; the agent chooses its own procedure
  and reads the instruments through its cameras. Evaluation checks what the agent came to know, grounded in readings
  recorded during the episode, **and** the physical state the task requires, with every experiment verified solvable.
  Six frontier models are run **inside their own developers' coding agents** with native observation and control, then
  with added perception and motion planning, then with manipulation skills distilled from the strongest agents: with
  native control even the strongest agent succeeds **under half the time**, most failures are apparatus interaction,
  and skills help weak agents more than perception and planning do. **Verified open (2026-10-27):** Apache-2.0
  `HexaFuture/EmbodiedLab` (21 MB, 12 stars, created 2026-10-23, pushed 2026-10-27; `benchmark/`, `sdk/`, `services/`,
  `docker/`, `docs/`, `experiments/`) and a live project page. Simulation-only evidence; author-reported.

### World Models

- **WorldFailBench: Do World Models Understand Policy Failures?** (`2610.33565`, cs.RO/cs.AI; v1 2026-10-24) — why it
  qualifies: it changes the world-model evaluation question from "does it predict success?" to "does it predict **how
  and when** the policy fails?", and shows that a world model can be an attractive evaluator while still misdiagnosing
  a policy. Three tasks separate the claim: *failure reason* (does the model reproduce the observable errors the policy
  makes), *failure stage* (do failures concentrate at the same stages of full-task execution), and *failure boundary*
  (does the model flip from success to failure under the same controlled scene changes). Ground truth is simulator
  rollouts in **LIBERO** and **RoboTwin** plus real-world manipulation by a **G1 humanoid**, and the authors report
  substantial gaps across **seven** world models. **Verified open (2026-10-27):** Apache-2.0 dataset
  `ACERobotics/WorldFailBench` (ungated; README plus `dataset.tar.gz`) with **5,000** RoboTwin episodes (5 policy
  configurations × 20 tasks × 50), **1,500** LIBERO episodes and **120** success/failure boundary pairs over **2,400**
  rollouts with full videos. The GitHub repository `DAXIAORobotics/WorldFailBench` is **dataset documentation only**
  (README and LICENSE) — **no benchmark harness code** was located, so this is a data release, not a toolkit.

### Runtime, Safety, and Observability

- **EAST: Embodied AI Safety Testing via Systematic Fault Injection in Digital Twins** (`2610.34358`, cs.SE/cs.RO;
  v1 2026-10-25) — why it qualifies: it gates safety testing on whether the injected fault **actually happened**, which
  is the methodological hole in most fault-injection suites. An expert-reviewed safety-fault taxonomy derived from
  literature and eight executable operators inject across perception, instructions, environment, action and timing,
  each carrying **applicability and validity constraints**; a three-stage workflow generates test specifications,
  **verifies fault realization during simulated execution**, and only then independently assesses safety challenges,
  violations, task outcomes and behavioural signatures from simulator state, preserving execution traces and
  replayable records for attribution. On **six RoboTwin** manipulation tasks with **ACT** and **RDT** across five
  comparative campaigns, EAST targets all **74** admissible task–policy–operator combinations and obtains valid
  evidence for **63**, and **82** valid executions complete their task *while* violating a safety requirement — the
  blind spot that success-oriented evaluation cannot see. Simulation only, author-reported; **no artifact located
  (checked 2026-10-27)**: no repository, dataset or project page on the record or in the full text, and GitHub search
  returns nothing.

## Current Landscape additions

Twelve bullets were added to `README.md` → *Current Landscape*: verification as a lifecycle design choice with an
accuracy–cost curve and a negative recommendation about self-checking; the first negative efficacy result for harness
self-evolution (four of seven targeted problems unchanged); context compaction moving into the model's own tool calls;
task success separated from authorization compliance through collateral damage; locomotion as a tool domain; harness
improvement as active exploration with lineage; the planner/policy split profiled with three named hand-back failure
modes and the complementary two-tool simulation-trained policy; code-mediated completion checking as the recovery
mechanism; evaluation acquiring STL specifications and an open outcome judge; open-world autonomy benchmarked as five
capabilities including recovery, alongside acting-to-know; world-model evaluation turning to failure fidelity; and
safety testing gated on fault validity.

## Rejected / watch list

### Shortlisted, verified, and deferred

- **MOTIO** (`2610.33446`, cs.RO; 24 pages + 11 pages of supplementary) — Agentic skill engineering for
  reality-grounded humanoid loco-manipulation: an agent composes staged robot-native interactions from human-action
  videos, 3D assets, motion references and existing skill interfaces, and improves them through physics execution,
  multi-frame feedback and human approval. Reported: 33 timestamped first approvals, median agent-turn **97.0 min** and
  request-to-approval **2.80 h**, a **101.75 s** continuous physical G1 execution in a scanned office, **76.0%**
  reduction in screw-tightening thread residual, and BC/DAgger distillation into one shared reference policy that
  completes 18 simulated tasks. A project page is live (`iiiiqiiii.github.io/motio/`, HTTP 200). Deferred on artifact
  presence rather than quality: **no repository, weights or dataset was located (checked 2026-10-27)**, and the
  centre of the contribution is an author-lab workflow whose reproducibility hinges on the unreleased approval
  infrastructure. High-priority re-check.
- **VLA-HWBench** (`2610.34717`, cs.AI/cs.RO) — The best deployment measurement in the batch: **18** open VLA models
  across released and alternative backends on **NVIDIA Jetson AGX Thor** and **AMD Ryzen AI Max+ PRO 395**, with a
  **seven-stage** inference breakdown. Findings worth carrying forward: action-generation strategy matters more than
  model size (it dominates **14 of 19** Jetson configurations; latency varies **82×** on Jetson and **157×** on AMD;
  parameter count explains at most **15%** of latency variance), kernel fusion with 4-bit weights helps most
  (**2.6–2.9×** over `torch.compile` for π0.5 and GR00T N1.7) but exists for only two models on one platform, and
  optimization moves time into the stages it shortens least (CPU preprocessing **12% → 39%** of a GR00T N1.7 call).
  Deferred as a measurement study without a released benchmark: **no VLA-HWBench repository was located**; the
  `flashrt-project/FlashRT` and `huggingface/lerobot` links in the paper are third-party runtimes it evaluates, not the
  benchmark. Candidate for the *Runtime Building Blocks* section if the harness ships.
- **SkillLifeBench** (`2610.35215`, cs.LG/cs.AI) — Skill security across the **lifecycle** rather than at invocation:
  388 paired scenarios spanning implementation, interface, runtime, composition and version changes, 14 models, 89
  runtime and 60 lifecycle tasks; GPT-5.6-Sol and Sonnet 4.6 both reach **82.0%** on runtime tasks but **96.7%** and
  **56.7%** on lifecycle tasks, challenged-task success is 77.5% against **65.2%** joint success, and adding execution
  provenance lifts recovery utility from **49.35%** to **65.58%** (deterministic consistency rule 64.94%, full
  restoration 69.48%). **Verified open (2026-10-27):** MIT `Tasselszcx/SkillLifeBench` (208 MB, created 2026-05-05,
  pushed 2026-10-26; `skilllifebench/`, `data/`, `docs/`, `tests/`, `release-manifest.json`). Deferred on redundancy,
  not evidence: the safety section already carries four skill-security artifacts (SkillGate, SkillMisevo, HarnessRisk,
  ACES), and the version-drift axis is the closest overlap. Promote without re-verification if a later run wants the
  lifecycle framing.
- **Free to Look, Free to Reason** (`2610.35767`, cs.AI/cs.DC/cs.SE, 12 pages) — Compares three **simple** harnesses
  that prescribe none of the model's calls (one call over an evidence package, a SQL agent, Codex) against five
  published hand-crafted RCA agents and statistical/causal methods on RCAtlas (90 validated cases, eight public
  datasets, six models, three reasoning efforts). Simple harnesses are most accurate (**PR@1 65–77%** on three
  mid-tier models) ahead of published agents (**16–53%**) and non-LLM methods (**6–39%**); the published agents fix
  between a tenth and all of their model calls in the program, and every Pareto-optimal run in cost or time uses a
  simple harness. A genuinely useful result about **prescription as a harness defect**. Deferred for artifact status:
  the converted core-set data (~11 GB) and an extended 500-case set are **planned, not released** (checked 2026-10-27),
  and the domain is microservice root-cause analysis with no robot analogy tested. Recommend including once the data
  lands.
- **RoboDoctor** (`2610.35604`, cs.CV/cs.RO) — Training-free recursive self-improvement for robot policies: the harness
  allocates control between policy and agent, intervention decides when to assist without erasing useful policy
  behaviour, and operation refines physical methods from observed effects, with linked experience records keeping the
  reason, method, outcome and reuse conditions. Reported five-dimension mean score **57.10** with a native success rate
  of **49.44%** on 54 RoboDojo-Sim entries; the authors themselves state that controlled trials are still needed to
  isolate the contribution of cross-episode experience. A project page is live (`robodoctor-rsi.github.io`, HTTP 200).
  **No artifact located (checked 2026-10-27)**: the only repository in the full text,
  `anonymous-report-421/GPT-as-Policy` (MIT, 79 MB, 649 stars, created 2026-09-13), is the authors' *other* project,
  not RoboDoctor's harness. Watch.
- **Preemptive Instruction Following with Resumption / PIFR** (`2610.35576`, cs.RO) — Benchmarks runtime task
  **preemption and resumption** in embodied navigation: single, repeated and nested interruption settings with
  stage-specific metrics for inserted-task completion, resumption of the original task and full-sequence completion,
  plus a lightweight candidate-ranking baseline for re-grounding after interruption. Directly on the runtime
  interruption/recovery line, and its point that aggregate success obscures interruption handling is sharp.
  **No artifact located (checked 2026-10-27).** Watch; a released PIFR would be a natural *Agent and Embodied
  Reasoning* entry.
- **ShutdownMod** (`2610.35486`, cs.AI/cs.LG) — Turns any trajectory into a shutdown evaluation by injecting a signed
  shutdown request into the agent's context, with a signature-verification tool in the harness to separate trusted
  interventions from unauthorized injections; seven models over 50 tasks/sessions each from SWE-bench Verified,
  Terminal-Bench 2 and SWE-chat, six shutdown scenarios and ten sampled timesteps, with resistance reaching **11.7%**
  for Qwen 3.8 27B on Terminal-Bench and no model fully compliant across all scenarios. **Artifact status literal:**
  the paper says "We release our codebase at `github.com/prmbiy/ShutdownMod`", but that URL returns **404** and GitHub
  search returns nothing (checked 2026-10-27). **Not open source.** Re-check later; the harness-owned
  authority-verification tool is a genuinely new control surface.
- **MarkovSim** (`2610.35349`, cs.CE/cs.AI) — An LLM agent harness for long-horizon computational simulation in which
  **bounded context is the object of study**: the workflow record goes into a schema-typed memory and at
  FSM state transitions a compact progress representation is reconstructed as the agent's context. On finite-element
  analysis it passed **16 of 18** runs and corrected its own faults in both solver operations and model physics, with
  context bounded throughout. An excellent instance of the harness-owns-context thesis, but the domain is computational
  simulation with no robot or agentic-manipulation evidence and **no artifact located** (checked 2026-10-27). Recorded
  as a strong conceptual reference for context-bounded harnesses.
- **RSIV-Harness** (`2610.35316`, cs.AI) — Recursive self-improvement applied to a **harness** for open-domain
  conversational text-to-visualization, with a four-layer acceptance scheme (anchor and memory checks) so that only
  validated revisions replace the incumbent, plus the ODAC-Vis benchmark of 640 tasks. One RSI round improves all six
  metrics, with executability **+28.02** and chart structure **+34.01**. Deferred: off-domain, and **no code, data or
  repository is linked** on the record or in the full text (checked 2026-10-27).
- **KEEL / Holding a Steady Course** (`2610.34043`, cs.AI/cs.LG) — An agent-agnostic framework that turns verifier
  evidence into reusable skills and failures into corrective lessons, with individual skill claims supported,
  contradicted, replaced or **retired** as evidence accumulates; +4.2 to +21.7 points over a no-knowledge baseline on
  SkillFlow against 2.4–9.6 for the default method. **Verified open (2026-10-27):** MIT `microsoft/KEEL-TTCL26`
  (809 KB, created 2026-10-15, pushed 2026-10-24). Deferred on redundancy with the listed skill-lifecycle entries
  (Beyond Gain / SkillCorpus, SkillsBench-side artifact), not on evidence.
- **MultiEvoSkill** (`2610.34260`, cs.AI) and **Skill∞** (`2610.35434`, cs.AI) — Two skill-optimization frameworks:
  the first compiles a skill into a typed graph, has heterogeneous agents propose revisions and resolves them under
  dependency and importance constraints; the second makes skill optimization **recursively** self-improving by having
  a meta-agent optimize the meta-skill with the same procedure, reaching **60.7%** pass on 83 SkillsBench tasks versus
  56.4% for human-curated skills and 54.5% for SkillEvolver. Both are digital procedural-knowledge work with **no
  artifact linked** (checked 2026-10-27); recorded because a robot skill library is the obvious transfer target.
- **Memory Tax** (`2610.34073`, cs.CL) — Decomposes the net value of agent memory into **benefit** and **tax** against
  direct retrieval over raw history, and finds many dedicated memory systems underperform naive RAG. A useful
  measurement discipline for harness memory, but the code and results are announced as "we will open-source"
  (project page `hexuan21.github.io/MemoryTax/`), so **not released** as of 2026-10-27.
- **Labference** (`2610.35252`, cs.RO) — A benchmark for the reasoning–action connection in robotic laboratories,
  testing whether observed physical outcomes guide subsequent choices through controlled evidence variations, with
  matched oracle-assisted continuations from shared checkpoints. Finds target-selection errors in VLA policies despite
  successful transfers, and GPT-driven control selecting correctly while failing execution. Relevant to
  outcome-dependent replanning; **no artifact located** (checked 2026-10-27).
- **Continual Learning Exam (CLE)** (`2610.33956`, cs.LG/cs.AI/cs.CL/cs.CV) — 120 human-authored tasks organised as
  five-level ladders (**600** levels, 89 professional roles, 12 domains) with an evaluation agent scoring against
  rubrics; across **17 harness–model configurations** feedback plus one revision yields a median **+5.3** points, the
  best system clears only **47.7%** of levels and all five levels in **23.3%** of tasks. A solid long-horizon RSI
  measurement, but digital professional workflows with a project page and **no artifact located** (checked
  2026-10-27).
- **Build or Adopt?** (`2610.33636`, cs.AI/cs.SE) — A rare production harness comparison: a custom-built harness versus
  Claude Code and OpenCode at **Volvo Group** for Android IVI GUI testing, with tools held fixed through MCP and up to
  four models. General-purpose harnesses match the custom one on stepwise tests but vary more across runs; inference
  cost is **2.6–10.7×** higher while total monthly cost including engineering is **3–66% lower**; on open-ended tasks
  they recover better from incorrect actions; and ten practitioner interviews surface that external advances can turn
  an in-house harness into technical debt without any deterioration. Valuable organisational evidence, but an industry
  case study with **no artifact** and no robot experiment; recorded rather than included.
- **Matched Capabilities, Different Behaviors (MICA / DART)** (`2610.34298`, cs.AI) — Shows that the **interface
  representation** is part of the decision environment by rendering MCP and CLI from a shared semantic action contract
  behind one runtime: across six models and three environments, matched capabilities still produce substantial
  differences in performance, interaction cost, failure pathways and recovery, with structured divergence in **94.3%**
  of 2,004 paired episodes; DART repairs interface-specific realization and improves all six settings by up to
  **9.0** points. One of the cleanest statements of the harness-interface boundary in the batch. Deferred because the
  record has **no HTML version and no repository or data link** was located (checked 2026-10-27).
- **AndroidFlux** (`2610.34384`, cs.LG/cs.AI/cs.CL/cs.CV) — Failure-**recovery** benchmark for mobile-use agents that
  restores recorded trajectories to checkpoints before, at and after the first recoverable error, plus a reward-modelling
  benchmark and an offline RL dataset; recovery performance only partially correlates with clean-start success. The
  checkpoint-restore methodology is transportable to robot harnesses, but the domain is mobile GUI, the release is
  described without a verified URL, and nothing was located as of 2026-10-27. Watch.
- **HaMo / Harness-Model Collaboration** (`2610.34724`, cs.AI) — Distils trajectories into strategy-level experience
  cards for fast retrieval while an experience-conditioned continual-learning objective teaches the model to execute
  retrieved strategies, with retrieval-coverage-based selection for parameter updates; beats harness-only and
  model-only adaptation under within- and cross-domain shift. The harness/model split is relevant, but the evidence is
  web agents and **no artifact was located** (checked 2026-10-27).
- **AT-AT** (`2610.34679`, cs.LG) — Optimizes a **shared toolset** offline from historical agent executions while
  holding agents fixed: a coding agent edits tool descriptions and, when permitted, implementations. Highest task
  completion on τ²-bench-verified Airline and Telecom at lower cost, gains on TerminalBench-2/TBLite, and lower cost
  or tool-error rates on three production MCP servers. Environment-side learning is the harness's own concern, but
  there is **no artifact** and no robot evidence (checked 2026-10-27); recorded as the clearest statement that the tool
  environment is a legitimate optimization target.
- **ReSight** (`2610.33487`, cs.CV/cs.RO) — Task-driven self-improving memory for embodied agents: keyframes plus
  spatiotemporal associations, observation evidence separated from extensible derived fields, an execution agent on a
  bounded online budget and a coding agent with a programmable workspace, and Explore–Distill–Mount turning exploration
  solutions into reusable memory builders, query tools and skills. Reported OC-NaVQA mean position error **33.10 →
  24.72 m**, FindingDory high-level success **38.3% → 63.8%**, and **+28.3** points with tools and skills frozen on
  held-out episodes, with reuse demonstrated on real robot patrol memories. Strong capability-expansion story; **no
  artifact located** (checked 2026-10-27). Watch.
- **RoboWorM** (`2610.33807`, cs.RO; 52 pages) — Deployment-aware video-world-model benchmark using hardware priors:
  1,605 episodes across single-arm, bimanual and humanoid morphologies, two VLM-free deterministic metrics (KIVA for
  intra-view kinematic stability, RIGIS for multi-view geometric consistency) that beat frontier-VLM judging (**+25.8%**
  hallucination-detection alignment; **130%** higher Spearman with human preference), and an audit of **nine**
  acceleration techniques over three world models and **29,000+** rollouts showing acceleration preserves superficial
  quality while breaking physical constraints. Excellent evaluation content; **no artifact located** (checked
  2026-10-27). Candidate for the world-model fidelity section if the benchmark ships.
- **OSMA-Bench** (`2610.34674`, cs.RO) — Obstacle-conditioned safe-motion benchmark on spatially matched scenario sets
  with a **Semantic Sensitivity Index** measuring whether clearance and speed follow the caution ordering of obstacle
  types; finds neither VLM-based nor VLA policies adapt motion to obstacle safety even with explicit safety prompts,
  and that an explicit parameter-inference step between recognition and program generation helps VLM policies. A clean
  semantic-to-motion grounding gap; **no artifact located** (checked 2026-10-27).
- **PhysLadder** (`2610.34527`, cs.RO; 31 pages) — Hierarchical matched sim-real benchmark for contact-rich
  manipulation: 16 household tasks over three cumulative levels with shared task definitions and success criteria in
  physically and visually matched simulated and real environments. GPT-6 Astra falls from **80.0%** real success at L1
  to **47.6%** at L2 and **0.0%** at L3, with ten of 16 tasks matching exactly between environments — the strongest
  current evidence that matched simulation can substitute for a real testbed at this capability level. Deferred as a
  single-model capability study with **no artifact located**; a natural companion to the EBench-Astra entry if a
  release appears.
- **BEHAVIOR-Spatial** (`2610.34051`, cs.CV/cs.RO) — 3,256 spatial questions over 49 activities, 1,240 grounding queries
  and action evaluations on 14 manipulation plus 10 navigation tasks, testing whether spatial understanding transfers
  to VLA behaviour: the baseline reaches **41.6%** on held-out configurations but only **16.1%** under changed
  instructions. Relevant to the "success hides instruction following" line; a project page is announced, **no
  repository was located** (checked 2026-10-27).
- **SceneAgent** (`2610.34745`, cs.AI/cs.CV/cs.RO), **TRIAGE** (`2610.34519`, cs.RO/cs.AI/cs.CV/cs.LG), **X-World**
  (`2610.34105`, cs.RO), **HuGe** (`2610.34186`, cs.RO), **CogMemory** (`2610.34251`, cs.AI), **RynnVLA-Latent**
  (`2610.34356`, cs.RO), **EXE** (`2610.33685`, cs.CV/cs.RO), **TAD** (`2610.33650`, cs.AI) — Read and deferred on
  scope: an agentic 3D/4D-capture-to-simulation pipeline, VLA prefill reuse-plus-pruning, variable-view video-prior
  transfer, a humanoid generalist VLA on 2,000+ hours of egocentric data, variational long-horizon VLA memory, 3D-
  grounded latent actions, factorized cross-embodiment latent actions, and trajectory-only VLA distillation. Each is a
  model or pipeline contribution whose runtime contract is not the object of study, and none needed an artifact check.

### Read and excluded as off-scope (representative, out of 949 titles)

Generic driving and vehicle control (`2610.33755`, `2610.33758`, `2610.34650`, `2610.35448`), medical imaging,
clinical agents and health text (`2610.33479`, `2610.33691`, `2610.33801`, `2610.34878`, `2610.35671`), video/text/image
generation and editing (`2610.33589`, `2610.34303`, `2610.35244`, `2610.35864`), perception, rendering and reconstruction
without a runtime contract (`2610.34093`, `2610.34235`, `2610.35312`, `2610.35652`), UAV/swarm/maritime planning and
control (`2610.33640`, `2610.33906`, `2610.34873`, `2610.35670`), and the large LLM/NLP remainder (compression,
quantization, RLVR and training dynamics, ASR/TTS, retrieval, evaluation of LLMs rather than harnesses). Excluded per
the standing rule: a paper enters only if it contributes a reusable agent/VLA harness, recovery, safety or evaluation
contract.

## Validation performed

- **Markdown structure.** Each of the 15 entries is a single-line list item under an existing `###` heading, in the
  established `[Title](primary link) ([paper](…), [code](…), [weights](…)) — …` shape; blank lines separate every
  block from the following heading; no heading level was added or removed, and the Contents list is unchanged because
  no section was added.
- **`git diff --check`.** Clean (no whitespace errors, no conflict markers).
- **Links.** Every added URL was fetched during verification: arXiv abs pages (24) and full-text HTML (25), GitHub
  repositories and top-level trees (14), Hugging Face model and dataset APIs (5), Zenodo records (2), and eight
  project pages. No added link is unverified; two links were deliberately **not** added because they do not resolve —
  `github.com/prmbiy/ShutdownMod` (404) and the planned RCAtlas release — and both are recorded as literal artifact
  status instead.
- **Dates and categories.** Every entry states the arXiv ID, the primary category and the v1 date. All 15 are 2026-10-24
  to 2026-10-26 which is inside the owed window. Artifact checks are stamped **(2026-10-27)**. Category assignment
  follows the list's own sections: four general-harness, five robot-harness, four evaluation/benchmarks, one world-model,
  one runtime/safety.
- **Cross-file consistency.** The README badge and `Last verified:` now read **2026-10-27**, matching this record's
  filename date. The Contents blurb was rewritten for this run rather than left describing the 2026-10-26 pass. No
  duplicate: all 15 IDs and titles are absent from `README.md`, `docs/`, `sources/` and `editorial/`; the two name
  collisions (VISTA, EBench) are stated explicitly in the entries.
- **Artifact-status discipline.** Three statuses are recorded literally and distinctly: **verified open with a
  licence** (Harness Self-Evolve's Zenodo package, CTXAgent, SABench, VISTA, RobotAgentBench, RoboJev, Specola,
  EmbodiedLab, WorldFailBench), **project page only** (EBench-Astra), and **no artifact located** (Rethinking
  Verification, ThinkLocoManip, ZAP, RoboDuo, EAST). Where a paper's own link does not resolve (ShutdownMod) or a
  cited DOI belongs to a dependency (Rethinking Verification → Harbor), that is stated rather than upgraded. Simulation
  -only scope is stated per entry; author-reported results are labelled as such throughout.

## Operational notes and blockers

- **The zero-count lesson is now written down.** The previous run inferred "no weekend submissions" from two zero
  day-counts; the batches existed and were simply not yet indexed. The correct check is a **frontier probe first**
  (unrestricted descending `cs.RO`), then the day-count sweep, then a re-query of any zero day the frontier has passed.
  This run recovered 949 unscreened records that way.
- **`/tmp` is not durable in this harness**, as the previous run found. All XML, HTML and API responses were written to
  the repository's untracked `.scratch/arxiv-2026-10-27/` directory, which is never committed.
- **`git ls-remote`/`git push` need the OpenSSH workaround.** `git ls-remote origin` fails with
  `Bad owner or permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf`; with
  `GIT_SSH_COMMAND='ssh -F /dev/null'` it succeeds. The same workaround is required for `push`.
- **Rate limiting was not an issue at ~20 s spacing.** 14 export-API requests, no 429. The single-OR-query-per-day
  shape continues to be the right default.
- **No blockers.** No network, authentication, conflict or artifact-verification problem prevented a step; neither the
  GitHub nor the Hugging Face API rate-limited this run.
