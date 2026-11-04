# Daily arXiv scan — 2026-11-04 (the owed 2026-11-03 batch arrives in full, 117 of 2026-11-02's records appear *after* that batch was declared closed, and thirteen inclusions)

## Scope

- **Interval owed.** The previous successful run reports starting at **2026-11-03 16:05 UTC** and committed at
  **`2026-11-03T17:18:26+01:00`** (16:18 UTC) on `main`, producing `c1fac22` and record
  `sources/daily-arxiv-2026-11-03.md`. That record screened the withheld 2026-10-29 block and
  the full 2026-10-30/10-31/11-01/11-02 batches and closed with **"Still owed: 2026-11-03. The next run must query it by
  `submittedDate` again."** This run started **2026-11-04 11:31 UTC** on `main` at `c1fac22`, so the interval it owes is
  **submission dates 2026-11-03 → 2026-11-04 inclusive**, with 2026-11-03 the explicit debt.
- **2026-11-03 arrived, and it is a full batch.** It returned **0** at the previous run's 16:05 UTC probe because the batch
  had not closed; at 11:31 UTC today it returns **467**. Every one of those is new to this repository.
- **The 2026-11-02 batch was not closed either.** Recounting every day in the previous record's window found **136 records
  that appeared after that record was written**:

  | query (one parenthesised OR-query over cs.RO/cs.AI/cs.CL/cs.CV/cs.LG per day) | 2026-10-29 | 2026-10-30 | 2026-10-31 | 2026-11-01 | 2026-11-02 | 2026-11-03 | 2026-11-04 |
  | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
  | count at 2026-11-04 11:31 UTC | **628** | **481** | **335** | **362** | **639** | **467** | **0** |
  | recorded by the previous run | 626 | 480 | 331 | 350 | 522 | 0 | — |
  | **late arrivals screened by this run** | **2** | **1** | **4** | **12** | **117** | **467** | — |

  **603 records** are new to this repository. Zero records from the previous harvest disappeared.
- **The late arrivals are one contiguous ID block, not scattered leakage.** Every one of the 136 late records has an ID in
  **`2611.03698`–`2611.04087`** — i.e. immediately *above* `2611.03648`, the ID the previous record declared the 2026-11-02
  frontier (`2026-11-02T18:59:32Z`). Their `<published>` timestamps span **`2026-10-29T10:27Z`–`2026-11-02T23:55Z`**. So the
  batch did not end where the frontier said it did: `2611.03698`–`2611.04087` is the rest of it, and it only became visible
  after the day it was attributed to. **A "closed batch" declaration made on the same day is a snapshot claim, not a fact
  about the batch.**
- **2026-11-04 is still owed.** It returns **0** because its batch had not closed at 11:31 UTC. It is carried forward, not
  screened.
- **Frontier probes.** An unrestricted descending `cs.RO` probe returns **`2611.05377`** (`2026-11-03T18:59:56Z`); the
  five-category union probe is pinned to the same timestamp. The previous run's frontier was `2611.03648`
  (`2026-11-02T18:59:32Z`), so the visible frontier moved by **1,729 IDs and one day**.
- **Revisions: 109 of the 2,912 harvested entries carry `<updated>` ≠ `<published>`; none of the 603 new records is a
  revision** — every new record is `v1` with `<updated>` equal to `<published>`. Twelve of the 109 revisions moved inside this
  run's window (`<updated>` ≥ `2026-11-03T16:00`); they are handled under **revision handling** below.
- **Coverage.** Every record carries at least one of the five target categories. Primary categories over the 603 new
  records: `cs.LG` 142, `cs.CV` 141, `cs.AI` 110, `cs.RO` **56**, `cs.CL` 53, `cs.CR` 10, `stat.ML` 9, `cs.IR` 8, `cs.DC` 8,
  `eess.IV` 7, `cs.HC` 7, `eess.SY` 6, `quant-ph` 5, `cs.SD` 3, `math.OC` 3. `cs.RO` appears as primary or cross-list on
  **72**. Per day: 2026-11-02 late block (117) `cs.LG` 32, `cs.AI` 24, `cs.CV` 13, `cs.RO` 12, `cs.CL` 10, `cs.CR` 5;
  2026-11-03 (467) `cs.CV` 127, `cs.LG` 105, `cs.AI` 81, `cs.RO` **44**, `cs.CL` 43, `stat.ML` 9, `cs.DC` 6, `cs.HC` 6.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`. **14 export-API requests**: one `cs.RO` descending
  frontier probe, **seven** per-day count queries (2026-11-03 and 2026-11-04, then re-counts of 2026-10-29, 2026-10-30,
  2026-10-31, 2026-11-01 and 2026-11-02 against the previous run's harvest) and six full-day harvests at
  `max_results=2000&sortBy=submittedDate&sortOrder=ascending` returning **628 / 481 / 335 / 362 / 639 / 467** entries
  (1.56 MB / 1.18 MB / 813 KB / 898 KB / 1.59 MB / 1.14 MB). Spaced 3–5 s; **zero HTTP 429**.
- **`/tmp` is still not durable between tool calls**, reproducing every previous run's finding. All XML, HTML and JSON
  captures are in the untracked `.scratch/arxiv-2026-11-04/` directory, which is **not staged and not committed**.
- **Query syntax still matters.** The per-day counts use
  `(cat:cs.RO OR cat:cs.AI OR cat:cs.CL OR cat:cs.CV OR cat:cs.LG) AND submittedDate:[...]`; the unparenthesised form drops
  the date filter and returns a constant site-wide count.
- **Screening.** (a) All **603** titles were read in ascending ID order. (b) Candidates were split by anchor: **72**
  cs.RO-touching records, **146** harness-anchored records in the non-`cs.RO` remainder (harness|scaffold|runtime|recover|
  oversight|memory|skill|tool-use|agent|evaluat|benchmark|safety|verif|monitor|permission|sandbox|replay|audit|guardrail|
  reward hack|self-improv|orchestrat|workflow|serving|latency|deploy|contract|test), and **13** robot-anchored records that are
  not cross-listed `cs.RO`; the remaining **398** unmatched titles were then read in full to confirm they are off-scope.
  (c) **31** abstracts were read in full. (d) **6** full-text HTML renderings (`arxiv.org/html/<id>v1|v2`) were fetched for
  artifact claims and for the disambiguation of citation-versus-release. (e) Every claimed artifact was resolved at the
  source: **12 GitHub REST calls**, **1 PyPI JSON call**, **6 project pages**, **3 raw `README`/`LICENSE` fetches**, and
  **12** direct HTTP status checks.
- **Model papers remain excluded by rule**, even when they are the batch's headline results, unless the contribution is an
  external runtime, interface, or evaluation/optimization target. The rejections below name the ones that came closest.

## Included (13)

### General Harness Methodology and Verification (5)

- **`2611.05139` — Meta$^2$: Precomputing Reusable Knowledge for Online Harness Evolution in Long-Horizon Agents** (cs.AI).
  Moves harness optimization **one level up**. Offline harness optimization
  produces one shared harness that can overfit training tasks and cannot react to evidence revealed during a run; online
  evolution can react, but it must reason about every intervention from scratch and usually has no verifier at test time.
  Meta$^2$ learns neither the harness nor the edit but **reusable guidance about evolving**: a textual **Meta-Skill** `M`
  refined by contrasting rollouts of the same task agent under three configurations — no evolver, an evolver guided by the
  current `M`, and an evolver given **privileged verifier information available only in training**. The contrasts aggregate
  across tasks into guidance on when and how to evolve, when to leave the harness unchanged, and which interventions to
  avoid. Online, the evolver combines the frozen `M` with emerging trajectory evidence and chooses **KEEP** or **EVOLVE**;
  each edit applies to the rest of the same run **without resetting the environment or its progress**. With GPT-5.6-Terra it
  reaches **81.00%** on Terminal-Bench 2.1 and **44.44%** on DeepSWE 1.1, the highest of the evaluated methods, and ablations
  support both the contrastive guidance and keeping edits task-local. Digital-agent evidence only. **Artifact status literal
  (verified 2026-11-04): none claimed, none located.** The record carries no code, model or data link and the full-text link
  scan returns only references to other harnesses (`openai/codex`, `deepseek-ai/deepseek-harness`, `earendil-works/pi`,
  `nex-agi/NexAU`).
- **`2611.04277` — Harness-S: Learning from Sibling Harnesses for Generalizable Agent Training** (cs.AI; comment gives
  `Code: https://github.com/DeepExperience/Harness-S`). Treats harness evolution as a **pool** problem instead of a search
  for one superior harness, and names the three properties that make a single shared harness the wrong target: **locality**
  (an edit repairs some failures while regressing others), **complementarity** (siblings recover one another's errors, with
  an oracle union above individual variants or repeated base sampling at matched rollout count), and **nonstationarity**
  (a fixed harness's benefit changes as the policy learns, and sibling rankings can reverse across checkpoints).
  Directional evolution builds a **Sibling Harness Pool** that stays fixed during policy training; complementary rescue
  training invokes it **only when all initial attempts under the base harness fail**, with the same policy generating fresh
  rollouts under selected auxiliaries whose trajectories replace the failed group for RL; an adaptive router selects Top-k
  from Harness Behavior Profiles and LLM voting, re-anchored as the policy learns. Across AppWorld, SWE-Gym and DBBench the
  authors report the trained policy substantially outperforms policy-only training when returned to the original base
  harness, and that reusing the router for Rank-1 inference adds further gains at the same executor rollout count.
  **Artifact status literal (verified 2026-11-04): verified open, Apache-2.0.** The repository is real and inspectable —
  `src/`, `runtime/`, `harness_pool/`, `configs/`, `tests/`, `docs/`, a `LICENSE` that is the Apache License 2.0 — and its
  README documents a **20-member AppWorld harness pool** (H0 plus 19 evolved siblings over prompts, skills, context,
  memory, hooks and tool views), rule-based rescue, optional profiling/voting router, and training and evaluation scripts.
  The offline harness search that produced the pool is explicitly **out of scope** for this release, so the pool is shipped
  prefabricated rather than regenerable from the release alone.
- **`2611.03868` — EvoGroup: A Self-Evolving Harness for Group Agents in Shared Contexts** (cs.CL; comment says "With code
  to be open-sourced"). Points self-evolving harnesses at **shared** context — the multi-user memory a group agent
  accumulates — which individual memory systems do not model, including group relationships and collaboratively formed
  knowledge. It cold-starts from a **minimal six-tool** harness and evolves it: confidence and task outcomes select
  informative trajectories, low-confidence cases elicit self-reflections on uncertainty, an analysis agent reads the
  selection under **adaptive progressive disclosure**, and bucketed analysis synthesises findings that guide an evolve agent
  revising the harness. Pass@1 on the held-out EverMemBench split moves **71.07% → 89.58%**, above a human-designed Claude
  Code harness at **85.83%**; the frozen evolved harness then transfers to the unseen GroupMemBench with GPT-5.5 and
  outperforms both memory baselines and human-designed harnesses. Digital-agent evidence only. **Artifact status literal
  (verified 2026-11-04): promised, not existing.** The only artifact statement is the comment's "code to be open-sourced";
  no repository, page or identifier is given, and none was located. This is a promise, not an open artifact.
- **`2611.03772` — RuntimeTickets: Source-Grounded Security Obligations for Agent Skills** (cs.CR/cs.AI; submitted to ICLR
  2027, under review). Moves Skill security from description and permission grants to **runtime enforcement**. From the
  task's delivery contract an LLM proposes security obligations, each **citing the Skill lines it answers**; the ticket
  **tightens the host's approvals into fine-grained allowed actions but never loosens them**; a runtime gate checks the
  recipient, object and amount of every outbound send before forwarding it; trusted monitors record effects within their
  observation scope; a deterministic decoder turns ticket status into governable data flows and verifies each obligation.
  The design point is explicit and is the reason this entry is here rather than in a general security list: **neither
  enforcement nor decoding involves a language model**, so text injected by a Skill can change the *proposal* but never the
  *rule that decides a send*. Evaluated at the network egress boundary of **three unmodified agent frameworks** against
  production Skill scanners and runtime baselines including an AgentBound-derived allow-list and SkillGuard: the authors
  report **0.0% overlook and 0.0% overkill**, where the scanners erred on both sides. Digital-agent evidence. **Artifact
  status literal (verified 2026-11-04): none claimed, none located.**
- **`2611.04507` — Signals: Continuous Diagnostics of AI Agents** (cs.AI). Builds the diagnosis layer for the failure class
  this list keeps circling: agents that go wrong **even though their tools and infrastructure work normally**, whose
  nondeterminism makes the failures hard to reproduce. A taxonomy of **45 problem types across eight families**, each with
  definitions and evidence requirements, separates **realized failures, risk exposures and inefficiencies**; related
  findings are grouped into severity-scored engineering issues that carry the concrete actions a developer can take and the
  checks that evaluate the change, and later executions add evidence to an existing Signal instead of opening a new one. On
  public agent executions the authors report **91.43% F1** matching Signals to reference issues, about **18 points** above a
  one-pass LLM control using the same model and taxonomy. Digital-agent evidence. **Artifact status literal (verified
  2026-11-04): none claimed, none located.**

### Agentic Robot and VLA Harnesses (3)

- **`2611.04226` — DiMuRe: Diagnosis-Guided Multilevel Recovery for Robotic Task Execution** (cs.RO/cs.AI). Recovery as a
  **decision over how much of the task program is still valid**, not a single retry primitive. Disturbances invalidate the
  conditions of the *next* action while most of the program remains useful, so the framework uses phase-specific evidence
  and method preconditions to select among **program reuse**, predefined **recovery skills** (including bounded VLA grasping),
  and — only in the full variant — **terminal replanning** in which an MLLM writes corrective code from images and execution
  feedback; after the original program exits, local verification checks each attempt's outcome before it is trusted. On
  **801** scheduled disturbed Encore development cases, program reuse plus recovery skills reaches **77.65%** complete-task
  success against **58.68%** for the original program under the same physical-step allowance, and full DiMuRe reaches
  **81.90%** with terminal replanning, an extra step allowance and early handoff, while local DiMuRe still uses fewer
  physical steps per scheduled episode than the original program. Ships **LIBERO-Disturb**, a benchmark for robustness and
  recovery under mid-execution disturbances that scores program-based agents and VLA policies against each other.
  Simulation evidence. **Artifact status literal (verified 2026-11-04): none claimed, none located** — no repository link in
  the comment, abstract or full-text link scan, so LIBERO-Disturb is described but not released.
- **`2611.05320` — PyRUA-DataFarm: LLM Agent as a Cost-Efficient Teleoperator for Real-Ready Robot Data** (cs.RO/cs.AI;
  comment gives project page and code). Asks whether a **coding agent** can take the teleoperator's job, and answers with a
  pipeline whose third stage is the interesting one. Given a data request and **one calibration episode**, the agent
  rebuilds the task in simulation — writing the scene, an expert controller and a success judge; the simulator supplies
  exact actions; a video model conditioned on real images renders every rollout as photorealistic video with all camera
  views generated consistently; and **independent judges certify both the expert and every rendered episode before
  admission**. On five real-robot tasks, policies trained only on farm data match teleoperation-trained policies
  (**93 vs 84** successes in 100 trials) and, under an unseen tablecloth and coloured light, keep **87** successes where the
  teleoperation-trained policies fall to **46**. Cost is reported per component: about **\$21** of API usage per task
  adaptation plus **\$5** for a render contract, **\$0.80** per further episode, and about **\$107** per hour of three-view
  data — compared by the authors against a **\$117** teleoperation quote in China and **\$290** in North-American labour
  alone. **Artifact status literal (verified 2026-11-04): verified open, Apache-2.0, on the `DataFarm` branch** of
  `DAGroup-PKU/PyRUA-Lean` (914 MB repository, `src/`, `tests/`, `configs/`, `prompts/`, `evaluations/`, a `LICENSE` and a
  `CITATION.cff`); the branch is real and the project page resolves. The five-task real-robot numbers are author-reported.
- **`2611.05058` — Do Agentic Policies Need to Look at Every Step? Geometric Abstraction for Reasoning and Action**
  (cs.RO/cs.AI/cs.CV; accepted to the CoRL 2026 Workshop on Agentic Robotics). Proposes that what an agentic robot policy
  *reads* should be a **refreshed 3D geometric abstraction** rather than the raw image: typed, evidence-aware symbolic
  references to object parts, grounded from calibrated RGB-D, returned as compact textual checkpoints with **metric
  residuals after each action**, so symbolic task structure is reused across decisions while metric values are rebound to
  the current scene, and direct visual reasoning is invoked only on demand. Across offline policy synthesis and continuous
  online execution the authors report better multi-step manipulation completion while substantially reducing repeated
  multimodal queries, arguing that explicit geometric state — not repeated pixel interpretation — is the substrate the
  policy should reason over. **Artifact status literal (verified 2026-11-04): verified open, MIT.** The repository
  (`Spectraorder/geometric-abstraction`) is real (`src/`, `policies/`, `tests/`, `configs/`, `scripts/`, an `AGENTS.md`,
  and a `LICENSE` that is MIT), and the project page resolves; the project page's own "arXiv coming soon" placeholder is
  stale, not a missing artifact.

### Evaluation and Safety (5)

- **`2611.04399` — Silent Instrument Errors Reorder Agent Leaderboards** (cs.AI/cs.SE; comment gives code and reproduction
  scripts). Makes the argument this repository has been circling from the other side: the code that **grades, logs, replays
  and versions** an evaluation is part of the measurement, and it fails silently. On a public leaderboard a general-purpose
  agent trails a scaffold built for an airline customer-service benchmark by **13 points** on the tasks that touch its
  database; the gap is the **grader** — the general agent often ends conversations in a way that never triggers grading, so
  only about half its episodes were scored and the rest stored as failures, and **re-grading the ungraded half under the
  benchmark's own rule closes the pooled gap without rerunning any agent**. Auditing **HAL** and **tau2-bench** from their
  published artifacts alone finds three more instrument errors: a cost column counts nested model calls twice and unevenly
  across rows; archived trajectories record grader-written actions as agent actions; and re-grading saved trajectories under
  a newer evaluator is itself a new measurement, including a **mode check that misreads four files and collapses their
  scores by 59 to 82 points while printing no error**. The HAL maintainers confirmed their three within five weeks. The
  proposed checks are cheap and disclosure-oriented: report the graded fraction, aggregate cost from root spans, tag the
  provenance of every recorded action, and pin evaluator versions with a published re-grade diff. **Artifact status literal
  (verified 2026-11-04): public but unlicensed.** `YangzeLiu/eval-instrument-disclosure` is public (116 KB, pushed
  2026-11-03) and contains `HAL_report_for_maintainers.md`, `tau-bench_calculate_reward_mutates_actions.md`,
  `tau2-bench_regrade_silent_score_changes.md`, a `corrections/` directory and a `repro_eval_instrument_2026-09-03.zip`, but
  the GitHub API reports **no license file**, so it is inspectable rather than open source.
- **`2611.05093` — LoopQ: Fair, Closed-Loop Evaluation of Quantized Vision-Language-Action Models** (cs.RO). Fixes the
  **comparison** rather than the method: every quantization policy runs on the same checkpoint, layers and engine, varying
  only the method and the kernels, so a post-training-quantization result is attributable to the method. Over
  **180,000+** LIBERO and LIBERO-plus episodes on four VLAs it compares RTN, AWQ, GPTQ, ActQuant and QuantVLA against
  QVLA's reported results on the same layers, on an H100, an L4 and a Jetson AGX Thor, and tests whether four public
  VLA-quantization releases can be reproduced. Findings, as reported: quantization reliably saves memory (**2–3×** for real
  low-bit models), but **VLA-specific PTQ methods underperform simpler generic baselines** — at 4 bits RTN, AWQ and GPTQ
  match or exceed ActQuant and QuantVLA, plain RTN matches the calibrated methods, and plain AWQ matches the success QVLA
  reports for itself. **Standard LIBERO hides the losses**: configurations with no significant LIBERO loss lose
  **1.5–4.3 points** under paired LIBERO-plus perturbations. Speed is the weaker story: at batch 1 on the H100 no quantized
  π0.5 or OpenVLA-OFT configuration is faster than its 16-bit model (**1.06–1.58×** slower on library kernels); only the L4
  with int8 kernels gains (**4–14%**); the Thor is slower on every 4-bit configuration; and the engine alone changes latency
  by **2.1×**. Profiling explains it — π0.5's quantized backbone is compute-bound while its memory-bound action expert stays
  in 16 bits at **63%** of GPU time and **13%** of parameters. Simulation/bench evidence on three accelerators. **Artifact
  status literal (verified 2026-11-04): none claimed, none located.**
- **`2611.03850` — RoboScope Kit: Unified Interfaces for Fast and Scalable Embodied-AI Evaluation** (cs.RO/cs.AI; project
  page and code in the comment). Attacks evaluation **throughput** without changing what is measured: each model is wrapped
  once in a `Policy` class and each benchmark once in an adapter **around its own evaluator**, so the benchmark's rules and
  scoring stay untouched; simulator workers and Policy replicas run as separate pools sized from measured probe episodes;
  only the observations an action-chunk policy reads are rendered; work shards deterministically across GPU instances; and
  a durable queue returns a failed worker's item and scores a finished item exactly once. The motivating profile is blunt —
  the official StarVLA evaluator keeps simulation on CPU cores while the GPU averages **14.3%** utilization, and 500
  RoboTwin Easy episodes take **359.7 minutes** on one simulator and one model. The authors report 500 LIBERO-Long episodes
  **4.45×** faster than the official evaluator (**6.16×** with CUDA MPS), **2.78×** over RLinf and **1.52×** over vla-eval
  at equivalent success rates, and RoboTwin Easy **25.04×** faster on four GPUs with identical success outcomes and terminal
  steps. **Artifact status literal (verified 2026-11-04): verified open, Apache-2.0.** `MINT-SJTU/RoboScope-Kit` is real
  (`packages/`, `benchmarks/`, `examples/`, `ci/`, an `AGENTS.md`, and an Apache-2.0 `LICENSE`), it publishes to PyPI as
  `roboscope-kit` (latest **0.8.0b14**, a beta; the PyPI metadata carries no license classifier even though the repository
  does), and adapters cover **LIBERO, LIBERO-Plus, RoboTwin, RoboDojo, RoboCasa, CALVIN and MetaWorld** with per-benchmark
  protocol documents.
- **`2611.05080` — CRONOS: Benchmarking Non-Episodic Reinforcement Learning for Robotic Manipulation** (cs.RO/cs.LG; project
  page in the abstract). Benchmarks the training loop deployment actually has: **no scene resets between tasks**, shared
  scenes and multi-task episodes — which standard episodic pipelines assume away and which costs human intervention in the
  field. It uses high-fidelity physics simulators on the specific question of non-episodic **RL fine-tuning of modern
  large-scale VLAs**, where prior non-episodic work trained small policies from scratch. Findings as reported: naive
  fine-tuning of pre-trained policies fails in non-episodic settings because object configurations and end-effector poses
  drift; **targeted reset allocation** and correcting pre-training biases mitigate it; and non-episodic training itself
  improves long-horizon manipulation and generalisation to unseen object configurations and task sequences. Simulation
  benchmark. **Artifact status literal (verified 2026-11-04): project page only.** `embodiedai-ntu.github.io/cronos`
  resolves and shows a "Code" label, but its only `href`s are anchors, author sites and template attributions — no
  repository, no dataset, no weights. Nothing is claimed as open here.
- **`2611.04166` — Say the Same, Act Differently: Diagnosing and Steering Text-Action Divergence in Reasoning VLAs**
  (cs.LG/cs.CR/cs.CV/cs.RO; NeurIPS 2026). Refutes the assumption behind reading a reasoning VLA's rationale as a safety
  monitor: **an unchanged rationale cannot certify an action**. Its text-neutral action-subspace (TNAS) diagnostic shows the
  locally most action-sensitive direction in hidden state is nearly orthogonal to the active text-loss gradients —
  **1.87%** norm overlap in Alpamayo for driving and **5.28%** in InstructVLA for manipulation — so a perturbation that
  leaves the text alone can move the action. At equal hidden-state perturbation norms, interventions along this
  text-neutral residual shift actions **4.64×–47.8×** more than text-gradient perturbations, while text-gradient
  perturbations induce **21.2×–277×** more text drift; steering visual perturbations along the same direction reproduces the
  decoupling from the image. Across **154** obstacle-annotated driving scenes, TNAS-guided attacks induce a **new collision
  under a word-for-word identical rationale in 52.6%** of scenes under full-image perturbations and **35.1%** under a
  localized forward-camera patch, against 4.55% for unguided action attacks; relative to a text-preserving baseline the
  guidance raises exact rationale preservation **83.0% → 89.5%** and cuts teacher-forced text drift by **68.4%** at a
  comparable collision rate, and in InstructVLA it cuts text drift by **40.5%** while preserving the exact response on
  **85.0%** of examples alongside a **1.73-unit** action-chunk shift. The harness consequence is stated directly: monitors
  for reasoning VLAs must verify the predicted **action** channel. Simulation/driving-benchmark evidence. **Artifact status
  literal (verified 2026-11-04): none claimed, none located.**

## Revision handling

- **Twelve revisions moved inside this run's window** (`<updated>` ≥ `2026-11-03T16:00`). All are `v2` bumps of records
  announced in earlier batches; none is a new submission, and **none of the 603 new records is a revision**.
- **One revision touched a tracked entry and was checked: `2611.01560` *Democratizing Robot-Use Agents: A Unified Harness
  with Open Models* (uni-robot-agent), `v1` → `v2` at `2026-11-03T18:09:21Z`.** The previous run added it at `v1` with the
  literal status **"none claimed or none located"** and carried it on the watch list. The `v2` record still has **no
  comment field**, its abstract is unchanged, and the only first-party-looking link in the `v2` full text is
  `github.com/anonymous-report-421/GPT-as-Policy`, a baseline named in the text rather than the authors' release. **The
  entry is therefore left as written; v2 is not an artifact release and no promotion is warranted.**
- **Seven revised records are already tracked in `README.md`** (`2611.01560`, `2610.41970`, `2610.40026`, `2610.39884`,
  `2610.40016`, `2610.39711`, `2610.39158`). Of these only `2611.01560` moved inside this window; the others were
  `v2`/`v3` bumps before `2026-11-03T16:00` and were covered by earlier runs (including the 2026-11-03 run's RoboSeed
  promotion from 404 to public-but-unlicensed). **No other tracked entry needed a correction this run.**

## Excluded and watch-listed

- **New watch-list entries from this run (artifact claim located, artifact absent or placeholder).**
  `2611.05155` **Asynchronous Robot Policies (ARP)** is the clearest case: the comment says "For code, checkpoints and more
  results, check out **arp.insait.ai**", and the page renders as **"Paper · Code (soon) · Models (soon)"** under *Anonymous
  authors* — a placeholder, not a release, despite a real page with 60 real-robot trials and a RoboLab benchmark described
  on it. `2611.05080` **CRONOS** (project page with no repository `href`), `2611.05368` **GUILD** (project page
  `robin-lab.cs.utexas.edu/guild2026/` resolves, CoRL 2026, but carries no code link), `2611.03930` **What Is This Memory
  Useful For?** (project page and `ChihHsuan-Yang/AAMAS2027-ConditionalMemory` repo resolve and the repo has real `src/`,
  `configs/`, `results/` — but **no license**), and `2611.04399` **Silent Instrument Errors** (included, but its repository
  is also **unlicensed**) all sit in this band.
- **Deferred to a later screen on merit, not rejected.** These are genuine harness or evaluation contributions that this run
  did not verify to the link level and therefore does not assert as open:
  `2611.04974` **LLMs as MDP Designers / ARPG** (NeurIPS 2026; dependency-aware agentic framework that builds RL pipelines —
  sequential MDP formulation with **localized verification and correction**, then modular simulator implementation,
  externalising intermediate artifacts; Isaac Sim evidence; no artifact link located);
  `2611.04873` **Trusting Imagined Futures** (audit gate on imagined rewards before supervision selection: a
  multidimensional reward model plus an auditor over paired imagined/real executions, accepting a candidate only when its
  advantage clears a margin **and** its audit risk clears a threshold — +3.00 points from the selector and +1.47 from the
  gate, LIBERO-only);
  `2611.04952` **Artifact-to-Capability** (defines capability formation from artifacts as five sequentially dependent
  properties with three-scale diagnosis, and shows Reflexion, Voyager and DGM all display the usual three proxy signals while
  each breaks capability formation in a different way — a strong instrument, but the self-improvement line is this list's
  densest family);
  `2611.05088` **LatentTriad** and `2611.04321` **ATC** (malicious-Skill detection from prefill hidden states; role-contract
  enforcement in black-box multi-agent systems at the point a message would enter the shared conversation — both overlap
  `MaliciousSkillBench`, `SkillGate` and `Bounded Agents` without changing the story); `2611.04972` **Caging VLN**
  (VLA planner caged by an SDF/Safety-architecture layer for legged VLN — a safety contract with no artifact located);
  `2611.03765` **VagueBench** (name `Ambient Context Illusion` and benchmark **1,303** instruction pairs in AI2-THOR for
  robustness to contextually minimal but semantically sufficient instructions); `2611.04255` **ActOR** (ambient-camera
  digital twin plus LLM agent orchestrating pre-programmed humanoid skills in operating rooms); `2611.04870` **PrimeSim**
  (30,110 hours of demonstrations across 22 platforms, multiview action-conditioned world-model simulator); `2611.05029`
  **DRAP** (trajectory-amplification-risk-weighted continual learning); `2611.04016` **Physical Experience Builds Continual
  Spatial World Models**; and `2611.04481` **Self-Recursive Hardware-Software Harness** (a software-plus-instrument harness
  that revises its own hardware and protocols against spectral measurements — real hardware and real harness, but optics
  rather than robotics).
- **Model contributions, excluded because this repository does not treat a model paper as a harness paper.** Representative
  from this batch: `2611.04368` RAFT (reasoning-aligned fine-tuning; **75.0%** OOD on a physical DROID robot against
  **27.9%** for plain fine-tuning), `2611.05377` **DreamX** (video diffusion as a universal latent-action learner — distinct
  from the watch-listed DreamX-Phi, and still a model paper with no artifact on the record), `2611.04527` Future
  Representation Distillation, `2611.05023` HiLo-WAM, `2611.04711` Cycle WAM, `2611.04493` Physical Diffusion Policy via
  MPM, `2611.04492` Hybrid Particle-Voxel Dynamics, `2611.05249` (action-token supervision and gradient routing in
  mixture-of-transformer VLAs), `2611.05163` RoboTryOn, `2611.05119` DexHOT, `2611.05113` HLO-AMP, `2611.03815` AIRoA-0,
  `2611.03779` (robot-matched soft fingers for UMI data collection), and `2611.05062` FloMo.
- **Off-scope remainder (representative, from the 603).** LiDAR/radar fusion, SLAM, calibration and 3D reconstruction
  (`2611.03936`, `2611.03940`, `2611.04352`, `2611.04397`, `2611.04912`, `2611.05084`, `2611.05172`, `2611.05361`);
  motion planning, control and reachability (`2611.04096`, `2611.04325`, `2611.04204`, `2611.04477`, `2611.05372`,
  `2611.05295`); humanoid hardware, tactile sensing and locomotion (`2611.04334`, `2611.04424`, `2611.04839`, `2611.05302`,
  `2611.04204`); autonomous-driving world models and planners (`2611.04173`, `2611.04305`, `2611.05072`, `2611.04871`,
  `2611.04949`); UAV/multi-AUV and space robotics (`2611.04518`, `2611.04748`, `2611.04176`); surgical and medical agents
  (`2611.04755`, `2611.04286`); and the large remainder of pure `cs.CL`/`cs.LG`/`cs.CV` methodology, which the 398
  title-level scan confirmed contains no robot-harness or agent-harness contribution this repository does not already
  cover.
- **Watch list carried forward, not re-checked at link level this run** (no new version and no visible release activity in
  the harvest): **ReSTEP**, **EvoLife** ("Code is coming soon"), **Sandman**, **LOGIV** (`v3`, no artifact), **AgentHeal**
  (public, unlicensed), **METATRON**, **ELEVATE**, **Scan Before You Install**, **SkillSense**, **VEvo-Harness**, **KARMA**
  (`v2`, no artifact), **PASA-KV**, **Soei**, **Assurance Stack**, **RoboSeed** (weights public, unlicensed), **Masked
  Erosion**, **Prefix Executability**, **REALM**, **JevAct**, **PrimaCons**, **Minimum Sufficient Communication**,
  **ParetoSaddler** (linked branch 404), **VLALagBench** (claimed code 404), **TAMPAgent** ("Code soon"), **SPECTRA**
  ("coming soon"), **SPARK**, **RAVEL**, **BlackSwan**, **Reflex**, **CLEO** (none claimed), plus this run's additions above
  (`2611.05155` ARP, `2611.05080` CRONOS, `2611.05368` GUILD, `2611.03930`, `2611.04399`).

## Validation performed

- **Markdown structure.** 13 new single-line list items appended under seven existing `###` headings (no heading added,
  removed or re-levelled): 4 under *General Harness Design and Self-Improvement*, 1 under *Unified Evaluation*, 3 under
  *Agentic Robot and VLA Harnesses*, 2 under *Manipulation and VLA*, 1 under *What to Measure*, 1 under *Safety*, 1 under
  *Observability and Replay*. Four new bullets were appended under `## Current Landscape` in the same
  `- **claim.** [entry] …` shape as their neighbours. No table, HTML element or list nesting was introduced or modified.
- **`git diff --check`.** Clean — no whitespace errors, no conflict markers, no trailing whitespace.
- **Links.** Every arXiv `abs` URL added was constructed from the harvested record and its `<published>`, category list,
  comment and author list were read from that record. Every non-arXiv link added to `README.md` was resolved at the source
  this run: `github.com/DeepExperience/Harness-S` (200, Apache-2.0, 20-harness pool, README and `LICENSE` read),
  `github.com/MINT-SJTU/RoboScope-Kit` (200, Apache-2.0, README read), `roboscope.pages.dev` (200),
  `pypi.org/project/roboscope-kit` (200, `0.8.0b14`), `github.com/DAGroup-PKU/PyRUA-Lean/tree/DataFarm` (200, branch
  contents listed), `dagroup-pku.github.io/PyRUA-Lean/DataFarm` (200),
  `github.com/Spectraorder/geometric-abstraction` (200, MIT), `geometric-abstraction.github.io` (200),
  `embodiedai-ntu.github.io/cronos` (200, no code `href`), and `github.com/YangzeLiu/eval-instrument-disclosure` (200, no
  license). One URL is deliberately cited in the record as *existing but placeholdered*: `arp.insait.ai` (200, "Code (soon)",
  "Models (soon)") — it is named in the rejection list, not in an entry.
- **Duplicate IDs.** All 13 new IDs were checked against `README.md`, `docs/*.md` and `sources/*.md` before insertion
  (**0 of 603** batch records were previously named in the repository). After insertion each new ID occurs once in the main
  list and at most once in a `## Current Landscape` bullet, which is this repository's established pattern.
- **Dates and categories.** Every included entry states its arXiv ID and its `<published>` date falls inside the harvested
  window (2026-10-29 → 2026-11-03); no entry cites a 2026-11-04 record, because none exists. Category placement follows each
  paper's contribution rather than its arXiv primary: `2611.04166` (cs.LG primary, cs.RO cross-list) sits under *Safety*
  because its contribution is a monitoring contract, `2611.03868` (cs.CL) and `2611.03772` (cs.CR) under general harness
  design, and `2611.05320` (cs.RO) under robot harnesses rather than datasets because its subject is the collection loop.
- **Cross-file consistency.** The README badge, the `## Current Landscape` `Last verified:` line and the Contents blurb now
  read **2026-11-04**, matching this record's filename and the newest entry's verification date. The 2026-11-03 record and
  earlier history are unmodified. `docs/landscape.md` keeps its own earlier verification date because it is a historical
  note, not a maintenance surface.
- **Artifact-status discipline.** Five distinct literal statuses are recorded and each is stated in the entry itself:
  **verified open with a license** (Harness-S Apache-2.0; RoboScope Kit Apache-2.0; PyRUA-DataFarm Apache-2.0 branch;
  geometric-abstraction MIT), **public but unlicensed** (Silent Instrument Errors), **project page only with no repository
  link** (CRONOS), **promised / "to be open-sourced"** (EvoGroup), and **none claimed or none located** (Meta², RuntimeTickets,
  Signals, DiMuRe, LoopQ, Say the Same). No entry describes an unverified artifact as open source.
- **Author-versus-independent separation.** Every number in all 13 entries is marked as what the authors report; no paper in
  this batch has an independent reproduction, and the only third-party confirmation in the batch is procedural — the HAL
  maintainers confirming three of that paper's instrument errors within five weeks, which is reported as such. Evidence scope
  is stated per entry: simulation-only for `2611.04226`, `2611.05093` (LIBERO/LIBERO-plus on three accelerators),
  `2611.03850`, `2611.05080` and `2611.04166` (driving scenes and manipulation); digital-agent-only for `2611.05139`,
  `2611.04277`, `2611.03868`, `2611.03772`, `2611.04507` and `2611.04399`; and real-robot for `2611.05320`'s five tasks
  (author-reported) with `2611.05058` reporting both offline synthesis and online execution.

## Operational notes and blockers

- **"Closed batch" is a claim about a snapshot, and this run caught the previous one being wrong.** The 2026-11-03 record
  stated that the 2026-11-02 batch was "closed at its frontier `2611.03648`". Recounting the date range today returns
  **639** for that day, and the **117** extra records occupy `2611.03739`–`2611.04087` — the contiguous block immediately
  *above* that frontier. The count for every other day in the window also rose (10-29 +2, 10-30 +1, 10-31 +4, 11-01 +12).
  **The consequence for future runs is procedural: recount the previous three days' `submittedDate` ranges before writing
  "closed", and diff against the on-disk harvest rather than trusting the recorded count.**
- **The 2026-11-03 debt was real and worth 467 records, i.e. 77% of this run's new material.** It was correctly carried
  forward rather than declared empty; the batch had simply not closed at 16:05 UTC the day before.
- **A model can be checked without being browsable.** `2611.05155` (ARP) is the batch's cleanest example of a page that
  *loads*, presents real trial videos and a benchmark, and still carries "Code (soon) · Models (soon)" under anonymous
  authorship. Resolution is not release.
- **Two of the five strongest artifact stories are unlicensed.** `2611.04399` and `2611.03930` both resolve, both carry
  reproduction material, and neither carries a `LICENSE`; the repository's rule ("public but unlicensed is not open source")
  was applied to both, and only the former was included, on the strength of its content rather than its packaging.
- **`/tmp` is still non-durable across tool calls**, confirmed again; `.scratch/arxiv-2026-11-04/` (untracked, never
  committed) holds every response, the six day harvests, the six full-text renderings, the GitHub/PyPI JSON and the parsing
  scripts.
- **OpenSSH workaround still required.** `git ls-remote`/`fetch`/`push` fail with a "Bad owner or permissions on
  /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf" error; `GIT_SSH_COMMAND='ssh -F /dev/null'` restores access with the
  user's key and `known_hosts` unaffected.
- **Still owed: 2026-11-04.** The next run must query it by `submittedDate` again. **And it must re-count 2026-11-03 and its
  neighbours**, because this run has now demonstrated twice in two days that a day's count is not final when its batch first
  appears. The 2026-10-29 → 2026-11-01 batches are closed twice over (three days of stability) and should not be re-screened
  wholesale.
