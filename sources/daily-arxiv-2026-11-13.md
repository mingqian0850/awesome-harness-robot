# Daily arXiv scan — 2026-11-13 (the stall breaks: 2026-11-11 and 2026-11-12 both arrive at once with 383 and 342 records, 2026-11-10 grows by 70, the frontier jumps two days — and the harness becomes something that can be measured, split and priced)

## Scope

- **Interval owed.** The previous successful run committed at **2026-11-12** (`7d29ce0`) and produced
  `sources/daily-arxiv-2026-11-12.md`. That record closed with an explicit debt:
  **"Query 2026-11-11, 2026-11-12 and 2026-11-13 … and re-harvest 2026-11-06 through 2026-11-10 fully.
  If the index is still frozen at the 2026-11-10 frontier, treat it as an announcement stall, record the
  frontier timestamp again, and re-screen the 2026-11-09 and 2026-11-08 batches with abstracts."**
- **This run** started **2026-11-13 14:22 UTC** (**15:22 CET**) on `main` at `7d29ce0`, with `origin/main`
  also at `7d29ce0`. The interval owed is therefore submission dates **2026-11-11 → 2026-11-13
  inclusive**, plus full re-harvests of **2026-11-06, 11-07, 11-08, 11-09 and 11-10**.
- **Headline: the index had not stopped, it had been held.** Both owed days returned **0** on Thursday at
  11:19 UTC and return **383** and **342** on Friday; the newest record anywhere in the index moved from
  `2611.14519` at **2026-11-10T18:59:54Z** to **`2611.17330` at 2026-11-12T18:59:49Z**, a two-day jump in
  one interval. The earlier days grew unevenly and the split is diagnostic: 2026-11-09 gained **4** and
  2026-11-10 gained **70** late records, while 2026-11-06, 11-07 and 11-08 added **none** and only revised
  (11, 7 and 5 records with a later `<updated>`, 12 of them substantive). Late indexing and revision are
  therefore separable by evidence, not by assumption.
- **Twelve entries, one in-place promotion, and a batch about what the harness *is*.** Five general-harness
  entries (EvoX, EvoTrias, Knowledge in the Harness, Verify by Doing, Cleaning an Agent's History) turn the
  runtime into a comparable, splittable object; two agentic-robot harnesses (JustRUA, Reins) measure the
  harness as the variable on simulated and real hardware; one code-as-policy entry (ACE-CaP-0) prices
  *acquiring* a program; two runtime building blocks (Metal Sandbox, XInfer) publish isolation, scheduling
  and fleet serving; two safety entries (guardscope, humanoid runtime safety) measure the permission
  surface and report what happens when the stop is pulled. Separately, a watch-list verdict reversed:
  the kNN evaluation code recorded as a **404** on 2026-11-11 is now an Apache-2.0 release, and that entry
  is corrected in place rather than duplicated.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`, one parenthesised OR-query per
  day: `(cat:cs.RO OR cat:cs.AI OR cat:cs.CL OR cat:cs.CV OR cat:cs.LG) AND
  submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` with brackets percent-encoded. **22 export-API
  requests** — **18** completed and **4** refused — comprising **one** connectivity/header probe,
  **eight** count requests (seven day counts 2026-11-06 → 11-12 and one 2026-11-13 count),
  **seven** full-day harvests at `max_results=2000&sortBy=submittedDate&sortOrder=ascending`, and **two**
  descending frontier probes (`cs.RO`, `cs.AI`).
- **The API rate-limits heavy `submittedDate` queries, and the refusals look like an outage.** The first four
  attempts returned **`429 Rate exceeded.`** (14-byte body) and **`503`** (a bare HTML page) while
  `search_query=all:test&max_results=1` returned **200** in 96 ms with `x-cache: HIT` headers — so the
  service was reachable and the refusals were per-query-cost rate limiting, not an outage. Every subsequent
  request was spaced **8 s** apart with a **25–30 s** backoff on refusal; after the cooldown every count and
  every full-day harvest returned **200** on the **first** attempt (no retries were logged by either
  script). One earlier loop was killed by the harness's 60 s tool timeout before it recorded anything, and
  its partial requests are the reason the refusal count is stated as a lower bound of four.
- **No request returned the API's internal-error entry.** `grep -c 'api/errors'` on all seven day
  harvests and all count responses: **0** everywhere. The check stays because a count that trusts
  `totalResults` alone can record a false zero.
- **Full-day harvests return exactly their counted totals**, so truncation is excluded: 2026-11-06 **511**
  (1.23 MB), 11-07 **334** (0.81 MB), 11-08 **287** (0.70 MB), 11-09 **568** (1.40 MB), 11-10 **449**
  (1.11 MB), 11-11 **383** (0.96 MB), 11-12 **342** (0.86 MB) — **2,874** distinct ids across the seven days.
- **Differential diff against the previous harvest, as a parsed set difference.** The previous runs'
  `day-20261106.xml` (from `.scratch/arxiv-2026-11-11/`) and `day-20261107.xml` … `day-20261110.xml` (from
  `.scratch/arxiv-2026-11-12/`) were re-parsed and compared over bare numeric ids and over `title`,
  `published`, `updated`, `summary`, `comment` and `journal_ref`. Result: **0 removed** on every day,
  **+0 / +0 / +0 / +4 / +70** added on 11-06 → 11-10, and **73** records with any changed field of which
  **35** changed a title, abstract or comment.
- **Frontier probe.** Descending `cs.RO` returns **`2611.17330`** at **2026-11-12T18:59:49Z** and
  descending `cs.AI` returns **`2611.17323`** at **2026-11-12T18:59:06Z**, against the previous run's
  `2611.14488` / `2611.14519` at 2026-11-10T18:47:57Z / 18:59:54Z — the same probe method, now measuring
  motion instead of stasis. 2026-11-13 returns **0** at 14:22 UTC, which is expected mid-day for a Friday
  batch that announces the next morning.
- **Screening.** (a) all **383** titles of the new 2026-11-11 batch and all **342** of 2026-11-12 read in
  full, in ascending id order; (b) all **70** late records added to 2026-11-10 read by title and
  **15** of them by abstract; (c) **34** abstracts read in full for records carrying a harness, agent
  runtime, memory, monitoring, evaluation, code-as-policy or robot-execution signal; (d) **7** full-text
  HTML renderings (`arxiv.org/html/<id>v1`, all seven returned article bodies) read for artifact claims,
  evidence scope and the sentences the abstracts omit; (e) resolution of **every** claimed artifact at the
  source — HTTP status **and** GitHub API metadata (`size`, `created_at`, `pushed_at`, `license`) **and** a
  contents listing where emptiness or private-then-public status mattered.
- **Duplicate check before insertion.** All twelve ids were grepped against `README.md`, `docs/*.md`,
  `sources/*.md` and `CONTRIBUTING.md` first; none appeared in an inclusion, an exclusion or a watch list in
  any earlier record.

## What the index did

| query (one parenthesised OR-query per day) | 11-06 | 11-07 | 11-08 | 11-09 | 11-10 | 11-11 | 11-12 | 11-13 |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| count at 2026-11-12 11:19 UTC (previous run) | 511 | 334 | 287 | 564 | 379 | **0** | **0** | — |
| count at 2026-11-13 14:2x UTC (this run) | **511** | **334** | **287** | **568** | **449** | **383** | **342** | **0** |
| **new to this run** | **0** | **0** | **0** | **+4** | **+70** | **+383** | **+342** | — |

- **The two zero days were held, not empty, and the previous record's third state is now confirmed from the
  other side.** A zero on a past day can mean *not yet announced*, *unindexed*, or *announcement stalled*;
  the previous run could show only that the frontier had not moved, and this run shows that when it does
  move it delivers **both** withheld days at once. The practical consequence for the debt-carrying rule is
  the one already written down: a zero is never closed until the batch appears, and a second zero day in a
  row is evidence of a hold rather than of scarcity.
- **Late arrivals concentrate on the newest already-screened day.** 2026-11-10 grew by **70** after its
  harvest became a full day for the first time (the previous run harvested it 36 hours after its own day
  ended), whereas 11-07 and 11-08 — harvested three and two days after their batches — added **nothing**.
  That pattern supports the standing rule that a day is not settled until it has been re-harvested after
  its own announcement, and it is the cheapest explanation for the **+70**: the batch was complete by the
  time it was first read, the *day* was not.
- **No removals and no withdrawals anywhere.** Zero ids disappeared from any re-harvested day, so no entry
  added earlier in the week rests on a record that has since been taken down.

## Revisions on the already-screened days (73 records, 35 substantive)

- **Day totals.** 2026-11-06: **11** records changed, **6** substantive. 2026-11-07: **7**, **3**.
  2026-11-08: **5**, **3**. 2026-11-09: **28**, **14**. 2026-11-10: **22**, **9**.
- **None of the 35 substantive revisions belongs to a paper this list already carries**, so no entry's
  numbers, claims or artifact statement needed correcting on revision grounds. The revisions are recorded
  because the standing duty is to check, not because any of them qualified.
- **The two most interesting, both still excluded.** `2611.09670` — *Goodhart in the Planner: Sampling-Based
  Control Exploits Rewards Learned from Human Preferences* — gained a new abstract and comment on
  2026-11-12; the claim is reward exploitation by sampling-based control, which is a policy-side finding
  without a runtime or evaluation contract to place. `2611.10453` — *BunshinRL: Whole-Workspace Forking for
  Agentic Reinforcement Learning Rollouts* — was **retitled** on 2026-11-11, and it is the nearest thing in
  this window to a harness contribution (forking an agent's whole workspace per rollout); it remains
  watch-listed because the record claims no artifact and the evaluation is digital-agent RL.
- **Three revisions touched records this list cites only in passing** (`2611.11342` Agent Self-Improvement
  with Frozen Weights, `2611.14505` Continual-ARC, `2611.14488` memory-clock): all three changed only
  `<updated>`, not text, so the cited claims stand.

## Inclusions (12)

Classification per the handoff: General Harness Methodology; Agentic Robot/VLA Harness; VLA; Robot
Foundation/World Model; Evaluation/Safety. Every number below is **author-reported**; no paper in this
window has an independent reproduction. Artifact status is stated literally, with the date it was checked.

### General harness methodology (5)

- **`2611.15006` — EvoX: Rethinking the Agent Harness as a Runtime Policy** (`cs.AI`, 2026-11-11,
  technical report). The framing is the list's own thesis made executable: the harness is an **Operational
  Policy** beside the model's **Semantic Policy**, implemented as a Rust multi-model runtime through
  **Action Realization**, **Execution Continuity** and **Outcome Grounding**. What makes it listable is the
  measurement rather than the mechanism: **Contrast** fixes task qualification, provider-bound request
  verification and evidence-based adjudication, and the comparison covers **132 tasks** (Aider Polyglot 28,
  GAIA 21, Harbor Index 28, SWE-Live 27, Terminal-Bench 2.1 28) giving **4,752** planned
  task–model–harness combinations, scored on the **691** task–model pairs all six harnesses completed.
  Reported results: **56.2%** best, **≈3.0 points** over the six-harness average, **+10.7** on GLM-5.3 and
  **+7.9** on Kimi K3 — a harness advantage that depends on the model, which is a stronger claim than
  "best harness wins". **No artifact** (verified 2026-11-13): no repository, package, archive or weight link
  anywhere in the record or full text; the only code URLs are the evaluated third-party harnesses.
  Digital-agent evidence; no robot experiment.
- **`2611.15309` — Where Autonomy Belongs: A Separation of Powers for Agentic Program Evolution**
  (`cs.AI`, 2026-11-11, 58 pages). Treats the self-improving loop as a **constitutional** problem: `govern`,
  `propose` and `judge` are three decisions, each existing design line misallocates them (controller-centric
  starves proposal, agent-centric overloads it), and **neither protects the independence of the judge**,
  which is why benchmark reliability degrades. **EvoTrias** gives each decision one owner, restricts the
  interface to **typed contracts**, enforces boundaries with **runtime hooks**, and ships as a **plugin**
  that turns an existing coding agent into an evolution system. Reported: state of the art on mathematical,
  systems and algorithm-design benchmarks; **64%** token-cost cut from separating governance from proposal;
  evolution extended to a **680K-line** repository (OR-Tools); protected judgment keeping scores faithful to
  transfer. **Verified open (2026-11-13):** Apache-2.0 `openJiuwen-ai/FAST-Evolve`, implementation in the
  `evo_trias/` tree (7.4 MB, pushed 2026-11-13), project page `evotrias.github.io` live. No robot experiment.
- **`2611.15268` — Knowledge in the Harness, Reliability in the Weights** (`cs.CL`, 2026-11-11). A
  **teacher-free** co-evolution that splits the labour by what each half proves good at. Gemma 4 12B reads
  its policy from on-demand skills; each round the deployed system serves fresh tasks, the base model
  diagnoses failures and writes rules, and admission requires **code checks, counterexample self-review, a
  paired retest and a noise-calibrated validation gate**; GRPO then trains a LoRA adapter on complete
  multi-turn tool-using conversations. Reported on **τ²-bench** (no training or selection on official
  tasks): **+12.5/+19.5** pass¹ and **+18.4/+22.0** pass⁴ after two rounds, and **+11.8/+8.0** pass¹ and
  **+22.8/+16.0** pass⁴ over the reference agent that carries the whole policy in its prompt. The 2×2
  ablation is the transferable result: **rules rescue tasks the agent never solved and carry knowledge RL
  with rules in context does not internalize; the weights make solved tasks reliable**. **Verified open
  (2026-11-13):** Apache-2.0 `Brightendata/knowledge-in-the-harness` (83 MB, 6 stars, created 2026-11-10,
  pushed 2026-11-11) with code, data and run records. Digital agents.
- **`2611.15276` — Verify by Doing** (`cs.AI`, 2026-11-11, 38 pages). Co-evolves **Actor** and **Evaluator**
  harnesses inside one task with the weights frozen: the Evaluator **inspects artifacts and environment
  state with tools**, its findings drive both output selection and updates to the execution and verification
  procedures, proposed Evaluators face comparative review on common candidates, and Actor updates need
  support from the actor's own fresh outputs. Reported on AutomationBench, **τ³** Banking and selected
  GDPval tasks: highest overall score among methods operating **without external feedback**, **+3.3/+4.1/
  +2.3** points over the strongest baselines, with the fixed-pool experiment showing better verification can
  recover outputs the actor already produced. **No artifact** (verified 2026-11-13): no repository, dataset
  or checkpoint; Appendix E documents the harness specification (role policies, verification, update and
  selection, executable checks) without releasing it. This is the entry that makes "harness gain" partly a
  *selection* gain — a useful bound on what co-evolution can claim with no gradient step.
- **`2611.14654` — Cleaning an Agent's History Hurts When the World Still Holds the Error** (`cs.AI`,
  2026-11-10, a late arrival in this window). Supplies the missing premise for every compaction, repair and
  hand-off entry already in this list: **an edit to the history is a claim about the world, and it can be
  false.** Holding the world fixed and varying only how a correct value is stated, a truthful note beside
  the preserved error lifts recovery **51% → 66%** while the same value stated as the agent's own completed
  action drops it **below the unedited history, 50% → 44%**. Holding the history fixed and varying the
  world on **AppWorld**, a success receipt lowers whole-task success **45% → 28%** with the effect absent and
  raises it **38% → 49%** with the effect present — a **27-point** swing pooled over two receivers, positive
  in six of eight receivers at 97.5% and seven at 95%. **Artifact real but unconventional (verified
  2026-11-13):** **139 arXiv ancillary files** — run packets, sealed collection plans, analysis contracts,
  readouts and recomputation scripts (`MANIFEST.tsv` 2.3 MB; run-packet archives 4.8 MB and 361 KB) — and
  no repository. Digital agents; the paper's evidence is a frozen-receiver design, not a deployed agent.

### Agentic robot and VLA harnesses (2)

- **`2611.16009` — JustRUA: A Controlled Study of GPT-6 Robot-Use Agents** (`cs.RO`, 2026-11-11,
  30 pages). The ablation this list keeps asking for: instead of another robot-use agent, it **takes the
  design apart one axis at a time** — the harness around the model, the action interface, the reuse of
  experience, and the scene geometry given to the model — inside one framework, starting from the official
  **L3** harness. Reported: a **lean harness that keeps the full episode history and stops each motion
  early** goes a long way; **experience reuse changes how the model acts but is not a free lunch**; **scene
  geometry helps most**, even when depth is predicted from RGB alone; and the simplest configuration with
  **no learned policy, no self-improvement loop and no task-specific tuning** raises **RoboDojo** from
  **29.7 to 50.3**. **Verified open (2026-11-13):** Apache-2.0 `ZefanW/JustRUA` (51 MB, created 2026-11-06,
  pushed 2026-11-13) with the policy, the predicted-depth and gripper-feedback path and the persistent
  context; project page `zefanw.github.io/JustRUA` live with the harness-vs-L3 breakdown; a public
  RoboDojo replay-explorer Hugging Face Space also runs. **Simulation only**, author-reported.
- **`2611.16632` — Reins: Frontier Multimodal Models Driving Real Robot Arms, Zero-Shot** (`cs.RO`,
  2026-11-12). The cleanest harness-versus-no-harness control on physical hardware in this window: a
  frontier model drives a dual-arm robot through **3D perception tools and atomic actions that report
  contact and grip**, chains actions **in one call** with a **condition on each** that must hold, stops at
  the first failure, and is observed by a second **narrator** instance that watches the cameras and reports
  what changed; from images Reins also builds a **simulation-ready twin** for continual policy learning.
  Reported: **GPT-6.1 Sol** succeeds in **88%** of real episodes on five tasks, and **without the harness it
  fails every shell game while taking eight times as long**; three frontier models behave alike through
  Reins but differ in tracking a scene that changes while the actor thinks. **Artifact status literal
  (verified 2026-11-13): not released** — "code will be released upon publication", and no project page,
  repository or archive was locatable. Real-robot evidence, author-reported.

### Code as policy (1)

- **`2611.16872` — ACE-CaP-0: Real-World Code-as-Policies in the Era of Experience** (`cs.RO`, 2026-11-12,
  28 authors under "ACE-CaP Team"). Moves code-as-policy's decision from *what to write* to **when to buy
  information**, and reports which purchases pay: **Demonstrations on Request** (a human supplies a
  demonstration when the agent lacks a strategy) and **Simulation on Demand** (simulation used *selectively*
  during real-world refinement), with robot demonstrations favoured over indiscriminate simulation. The
  deployed artifact is a **frozen program** that runs with runtime perception and control and **no human or
  coding agent in the loop**: **92.1%** (**221 of 240**) trials across **eight** real-world manipulation
  tasks spanning alignment, multi-object handling and bimanual coordination, mean execution **0.97–2.27**
  minutes per task. **Artifact status literal (verified 2026-11-13): promised at a live address** — the
  project page `acerobotics2025.github.io/ACE-CaP/` resolves and serves the abstract, recipe, per-task
  results and all eight rollout videos, but carries **no repository, archive, weight or dataset link**, and
  neither the record nor the full text adds one.

### Runtime building blocks (2)

- **`2611.17294` — Metal Sandbox: Isolated Execution and Command Scheduling for GPU Coding Agents on Apple
  Silicon** (`cs.LG`, 2026-11-12). Buys **isolation and utilization together**: restricted per-instance
  file access, native command execution, a private persistent development environment per agent, and a
  **command-level scheduler** across interconnected Macs where parallel commands run concurrently, serial
  commands share one execution slot per node, and **eligible waiting commands migrate to an available Mac
  and return results to the original agent**. Reported on four **Mac mini M4** nodes: **1.68×–2.05×** command
  throughput under concentrated demand; **2.14×** estimated sustained throughput at four sessions per node
  from sixteen static traces; replaying those traces under dynamic scheduling cuts mean tool waiting by
  **18.8%** and batch elapsed time by **22.3%**. **Artifact verified open without a licence (2026-11-13):**
  `WangHaoZhe/metal-sandbox` (5.9 MB, created and pushed 2026-11-12) **declares no licence**; the Harbor
  fork it integrates with, `WangHaoZhe/harbor`, is Apache-2.0 (83 MB). Digital agents (on-device kernel
  coding), author-reported with the replay assumptions stated.
- **`2611.14969` — Scaling VLA Serving Under Robotic Execution Constraints (XInfer)** (`cs.RO`, 2026-11-11,
  17 authors). States the fleet problem as an operator meets it — the constraint is each executing robot's
  **evolving response requirement**, not average throughput — and reports a deployment in which "hundreds of
  VLA-powered robots are already operating". **XInfer** coordinates heterogeneous VLA computation with those
  requirements, exploiting parallelism while **managing interference** so time-critical feedback is not
  queued behind bulk work. Reported: **1.26–3.75×** the **effective robot capacity (ERC)** of
  state-of-the-art systems *under physical-execution constraints*, across three VLA models on
  production-derived workloads. **Artifact status literal (verified 2026-11-13): the claimed repository
  returns 404.** The abstract names `github.com/X-Square-Robot/xinfer`; that path returns **HTTP 404**, the
  organisation page resolves with **22 public repositories** (`Xplanner`, `policy-space`, `x2real`, …) and
  **none of them is `xinfer`**. Production evidence, author-reported.

### Safety (2)

- **`2611.15223` — guardscope: Measuring Which Actions Runtime Controls Check in LLM Agent Systems**
  (`cs.AI`, 2026-11-11). Converts a vendor claim into a **coverage number**: a control can only evaluate
  actions crossing its placement, and an agent acts through more paths than the tool-call channel, so
  guardscope (i) **statically** predicts which of an agent's **15 action paths** a control can inspect from
  its source and (ii) **empirically** runs a fixed workload on a deployment realizing one of **eight
  topology designs**, classifying every executed action *checked*, *bypassed* or *prevented* by reconciling
  the control's decision records against **kernel-level ground truth** rather than the agent's own log.
  Experiments use **two agent harnesses** and real runtime controls including **Microsoft's agent governance
  toolkit** and **agentgateway**. **Verified open (2026-11-13):** Apache-2.0 `VulcanLab/guardscope` (5.0 MB,
  created 2026-11-06, pushed 2026-11-13). Digital agents; no robot experiment.
- **`2611.15928` — Runtime Safety and Emergency Stops on a Full-Size Humanoid** (`cs.RO`, 2026-11-11).
  Reports what usually stays in an incident log. On a **Unitree H1-2**, **all joint commands pass through a
  safety layer running separately from the control software**, enforcing joint position, velocity and
  torque limits and stopping when a **state estimate goes stale** or when the **hard-wired e-stop is pressed
  or disconnected**. Pushed until it lost balance, the robot under a model-predictive lower-body controller
  **never recovered and the operator pressed the e-stop in every trial** — and because the stop **removes
  balance control, the robot falls**; separately, limits chosen for manipulation **were exceeded during
  ordinary walking**, and the vendor's own walking policy sometimes exceeds the limits in the released URDF.
  The argument therefore moves from limits to a contract: joint limits bound **how much force** may be
  applied but not **when**, so force authority should be granted in **stages** with a named responsible
  person or organisation. **No artifact located (verified 2026-11-13):** the text references an
  "open-source autonomy stack" but neither the record nor the full text links a repository, package or
  configuration; CC BY-NC-ND 4.0. Real hardware, one platform, deployment report rather than a study.

### In-place correction (1, not a new inclusion)

- **`2611.13160` — How Much of Your VLA is kNN.** Recorded by the 2026-11-11 run as **"the claimed
  repository is a 404"**, re-verified today and **reversed**: Apache-2.0 `sereact/lerobot_knn` (26.6 MB,
  2 stars, **created 2026-10-01**, pushed 2026-11-12) is an out-of-tree **LeRobot policy plugin**
  (`--policy.type=knn`, `lerobot-knn-fit`, `lerobot-eval`) whose `src/lerobot_policy_knn/` carries the
  policy, memory/encoder construction, the adapter turning nine VLA backbones into retrieval keys and an
  `experiments/` tree with the LIBERO and LIBERO-Plus evaluation scripts, beside `docs/policy.md`; the
  project page `sereact.github.io/lerobot_knn` is live. The `created_at` date is the interesting part:
  the repository **existed while the URL was dark**, so the earlier 404 measured a publication state, not an
  absent artifact. The existing entry is corrected rather than duplicated, and the pair with RoboACE is
  recorded below.

## Excluded and watch-listed

**Harness-shaped but not listable this run:**

- **`2611.16378` — Evidence-Governed Evolution (EGE)** (`cs.AI`, single author). Separates candidate
  generation from confirmatory adoption, evaluates frozen candidates on fresh paired audits, requires a
  prespecified primary objective plus reference-relative non-inferiority, and adds a canary deployment gate
  — with a formal evaluator-calibrated simultaneous lower bound and a cumulative degradation bound along a
  versioned reference lineage. It is the best *statistical* statement of the admission problem in this
  window, and it says of itself that the stress test "is a stylised statistical experiment, **not** an
  evaluation of a deployed agent". Excluded for evidence scope plus **no artifact**: no repository, no
  harness, no runs. Watch it if it is ever instantiated on a real harness.
- **`2611.17068` — MAPE: Discovering Effective Agent Memory Designs through Program Evolution** (`cs.CL`).
  Evolves whole memory programs from minimal seeds through a standardized lifecycle interface, reporting up
  to **+12%** absolute accuracy at **35%** of serving cost, and transplanting its composition module into
  five existing memory systems with gains in all five. Apache-2.0 `ash-ding/MAPE` is live, so the artifact
  qualifies; the **duplicate rule** excludes it — memory admission and memory-as-harness-state are already
  carried by FGSE, WikiTrace, Continuing-ARC and Cleaning an Agent's History in this same window.
- **`2611.14960` — Termination Lag: Coding Agents Solve Early but Stop Late** (`cs.AI`). A clean measurement
  of stop policy — instrumented per-step patch recording shows **38–82%** of successful runs get a correct
  patch on the first edit, post-solution steps are a median **35–37%** (Qwen3-Coder) and **54–60%**
  (Laguna-XS.2) of successful runs, and **1–8%** of runs undo their own correct patch. Directly relevant to
  the harness's stop gate, but **no artifact** and no mechanism is proposed; watch-listed as evidence for a
  future stop-gate entry rather than as an entry.
- **`2611.15491` — What Makes a VLA Reactive?** (CoRL 2026 workshop). Varies prediction and execution
  horizons, sync vs async inference, chunk-continuity methods and policy adaptation on two simulated
  dynamic benchmarks and three real tasks, concluding that **reactivity depends on the execution pipeline as
  much as on the policy**. The claim is exactly this list's thesis, but the contribution is a study of a
  policy's adaptation options with **no runtime, interface or artifact**; excluded, watch-listed.
- **`2611.14834` — HDH: Low-Latency Action Verification for Frozen VLAs** (`cs.RO`). Trains a compact
  verifier on the frozen VLA's own deployments and selects the best-scoring action chunk: **82.3%** on four
  LIBERO-10 tasks and **+22.6%** on three real manipulation tasks. Excluded on the duplicate rule (the
  runtime-monitor and candidate-selection line is already dense: SAFECAST, P², ContactGuard, VLA task-progress
  probe, and Verify by Doing in this batch) and because **no artifact** is linked.
- **`2611.16493` — ReA-Bench**, **`2611.16591` — Singularity-Bench** (`cs.AI`). The first is a six-tier
  diagnostic benchmark for auditing agent execution trajectories, motivated by risks that are "repeatedly
  missed across auditing configurations"; the second is the benchmark that puts **the agent harness itself**
  among its six tasks and reports a red-team pass that exposes reward hacking in the conventional Harbor
  framework, fixed by a **Breakwater** extension blocking all attacks tested. Both are strong evaluation
  contracts and both are **watch-listed on artifact**: no repository, dataset or released benchmark was
  locatable on either record as of 2026-11-13.
- **`2611.16803` — PREWARN: Detection Is Not Early Warning** (`cs.LG`, NeurIPS 2026 MI workshop). An
  evaluation protocol for monitors with an annotated onset, calibrated to an application-chosen error rate;
  code is live at `swiss-ai/degeneration-probe`. Excluded on scope: the failure studied is repetitive
  degeneration in open language models, with no agent runtime, tool loop or robot.
- **`2611.15515` — Incentive-Responsive Agents** (`cs.AI`, single author). Formalizes an incentive as a
  **runtime contract over the operating point** (model tier, test-time compute, memory budget, tool
  authority, verification, compensation) that the policy must causally consume, and reports that prompt-level
  incentive framing has **no detectable effect (p = 0.62)** while runtime allocation moves the hardest task
  family **0.60 → 1.00** at **4.8×** price-weighted cost. Code, simulator and results are released. Excluded
  this run for scope shape rather than quality: the subject is the economics of a long-lived personal agent,
  not a robot or VLA harness, and the deployment is explicitly future work.
- **`2611.16590` — AstroMemBench**, **`2611.16487` — HeMeR**, **`2611.16627` — Folio** (`cs.AI`). A memory
  benchmark for embodied spacecraft agents (MIT code, live), a KV-reconciliation inference framework for
  heterogeneous memory with real-robot deployment, and a context-folding manager with recovery handles
  (**66 of 75** LOCA-bench tasks against 54). All three are relevant to harness-owned state; all three are
  **watch-listed**: memory admission and compaction are represented in this window already (see the
  duplicate note above), and Folio and HeMeR link **no artifact**.
- **`2611.16409` — ACE-RL**, **`2611.15020` — Genie Sim Benchmark**, **`2611.16374` — ManiLadder**.
  Respectively: training a model to act, reflect and curate playbooks inside a **fixed** ACE harness
  (weights, not runtime), a real-to-sim fidelity study over **22** robot-task pairs (score correlation
  **0.9686**), and a **112-task** manipulation benchmark stratified into four difficulty levels calibrated
  with a single flow-matching policy. Excluded for artifact or duplicate reasons: ACE-RL trains weights;
  neither Genie Sim Benchmark nor ManiLadder published a located artifact as of 2026-11-13.
- **`2611.16221` — BWM-Forge**, **`2611.17267` — LiveGeo**, **`2611.16565` — Look Here, Then Decide**.
  A multi-agent harness for robot video generation ("BWM Team", no links, 15 tasks), a geographic-agent
  benchmark in continuously evolving worlds, and a genuinely harness-shaped vulnerability campaign
  (deterministic incomplete-fix miners select candidates, a **4B** decision model ranks them, **187**
  confirmed memory-safety findings) whose artifact is withheld under coordinated disclosure and whose domain
  is security auditing rather than agents. All three watch-listed; BWM-Forge also anonymous.
- **Digital-agent safety and security in this window, excluded by the duplicate rule:** `2611.16123`
  SwarmSafe-Delta, `2611.16744` scale-induced semantic shift in visual prompt injection, `2611.17115`
  CapLite, `2611.16693` MobileProbe, `2611.16861` Visual Cipher Bench, `2611.16485` Contain/Conform/Compare,
  and the leaderboard-hacking and evaluator-disagreement instruments (`2611.14769`, `2611.14581`). The
  harness-safety surface is already carried by HarnessRisk, LEASH, ArmedAgent, Authorization Drift, Say
  Shibboleth, Harness Matters and ActiveSecHarness; none of these adds an interface this list lacks. The one
  worth naming for a future run is **`2611.14581`** — 2,160 τ²-bench episodes scored four ways, where an LLM
  judge that repeats its own verdict **98.2%** of the time agrees with native scoring only **56.2%**, passes
  **10 of 11** forbidden writes, and a rule-based boundary evaluator with **no model call** catches all
  eleven — because it is a clean instrument for the judge-versus-execution question the evaluation section
  already carries.
- **Out of scope by domain:** `2611.14794` (proof-step verification for mathematical reasoning),
  `2611.16991`/`2611.17042` (agentic analog-layout benchmark, verifiable-AI infrastructure), `2611.16304`
  (RL that internalizes state tracking into weights), `2611.16579` (driving world-model latent planning),
  `2611.17129` (paper-faithful federated learning), and the perception, medical, remote-sensing, speech,
  music, climate and quantum clusters that dominate the raw counts of both new days.

**Watch list carried forward (re-checked where cheap this run):**

- **`2611.13034` RoboACE — upheld.** `DeLin1001/RoboACE` still reports size **0**, and the API still answers
  *"This repository is empty."* (verified 2026-11-13).
- **`2611.13160` kNN — reversed.** The repository is now public and complete; see the in-place correction
  above. The pair is why "not open yet" and "not open" are recorded separately.
- `2611.14320` WikiTrace (Apache-2.0 `yukyeongleee/wikitrace` live), `2611.13355` DEPUTYBENCH (code and data
  "at a later date"), `2611.14257` Elastic-WAM and `2611.14365` MetaWAM — not re-checked today; the
  2026-11-11 record remains authoritative for their status.

## Artifact findings that changed an entry

- **A 404 can be a publication state rather than an absent artifact.** `sereact/lerobot_knn` is now the
  third distinct explanation for a failed artifact check in this repository, beside *never existed*
  (`X-Square-Robot/xinfer` today, `zebehn/vla-start-suppression` on 2026-11-02) and *empty placeholder*
  (`DeLin1001/RoboACE`). Repository metadata distinguishes them only if it is recorded: `created_at`
  2026-10-01 with a 2026-11-12 push and no public page on 2026-11-11 is what proves the repository was
  **private**, and that is why the entry is corrected rather than re-verified from scratch.
- **A working project page is still not a release.** ACE-CaP-0's page serves the abstract, the recipe,
  per-task results and eight rollout videos and links **no download**, so the entry records it as promised
  at a live address — the same state this list named for SFF on 2026-11-12 and a distinct state from 404.
- **A subdirectory in a third-party repository is an artifact, but a weak one.** EvoTrias releases through
  `openJiuwen-ai/FAST-Evolve/tree/main/evo_trias` rather than its own repository: the path resolves (HTTP
  200) and the parent is Apache-2.0, so it is open, but its provenance is a subdirectory of a larger project
  rather than a versioned release of this paper's system.
- **Ancillary files are obtainable and unversioned.** `2611.14654` ships **139** ancillary files, including
  sealed collection plans, analysis contracts and recomputation scripts, and no repository. That is stronger
  than "claimed but unlocatable" and weaker than "open source", and it is recorded as its own state.

## Files modified

- `README.md` — badge to **2026-11-13**; Contents blurb rewritten for this run; `## Current Landscape`
  verification date, **four** new bullets and **one** watch-list-reversal bullet; **twelve** new entries
  across five subsections (*General Harness Design and Self-Improvement*: `2611.15006`, `2611.15309`,
  `2611.15268`, `2611.15276`, `2611.14654`; *Agentic Robot and VLA Harnesses*: `2611.16009`, `2611.16632`;
  *Planning, Tool Use, and Skill Composition*: `2611.16872`; *Runtime Building Blocks*: `2611.17294`,
  `2611.14969`; *Safety*: `2611.15223`, `2611.15928`); and **one** existing entry corrected in place
  (`2611.13160`, kNN: 404 → live).
- `sources/daily-arxiv-2026-11-13.md` — this record (new).
- Untouched by design: `docs/reference-architecture.md` (user modification), `docs/ring-harness.png`
  (untracked user file), `handoff.md` (untracked by design), `.scratch/` (working data, never staged).

## Validation performed

- **Markdown structure.** Heading inventory unchanged: **17** `##` sections and **22** `###` subsections
  before and after; every new entry is a single list item under the correct subsection; no heading is
  preceded by a non-blank line, no list item sits directly under a heading, and no line carries trailing
  whitespace.
- **`git diff --check`** — no whitespace errors.
- **Link resolution.** Every one of the **23** URLs added by this run resolves: twelve
  `arxiv.org/abs/` records, `evotrias.github.io`, `github.com/openJiuwen-ai/FAST-Evolve/tree/main/evo_trias`,
  `github.com/Brightendata/knowledge-in-the-harness`, `zefanw.github.io/JustRUA`,
  `github.com/ZefanW/JustRUA`, `acerobotics2025.github.io/ACE-CaP/`,
  `github.com/WangHaoZhe/metal-sandbox`, `github.com/WangHaoZhe/harbor`,
  `github.com/VulcanLab/guardscope` and `github.com/sereact/lerobot_knn` all returned **200**; the corrected
  entry's project page `sereact.github.io/lerobot_knn` returns **301** to its trailing-slash form, which
  returns **200**. The one URL this run found broken —
  `github.com/X-Square-Robot/xinfer` — is deliberately **not** hyperlinked in the entry text and is recorded
  as a 404.
- **Duplicate check.** All twelve ids were grepped against `README.md`, `docs/*.md`, `sources/*.md` and
  `CONTRIBUTING.md` **before** insertion; none was present. After insertion each id appears at least twice —
  in its entry and in at least one of the Landscape bullet or Contents blurb — and no id appears in more
  than one entry.
- **Dates and categories.** Every included entry names its arXiv ID and its `<published>` date — eleven on
  2026-11-11/2026-11-12 and one (`2611.14654`) on 2026-11-10 as a late arrival — and the README badge, the
  `## Current Landscape` `Last verified:` line, the Contents blurb and this record's filename all read
  **2026-11-13**. The badge had been left at **2026-11-11** by the previous run while the prose read
  2026-11-12; it is now consistent at 2026-11-13, which is recorded here because a stale badge is exactly the
  kind of cross-file drift this check exists to catch.
- **Cross-file consistency.** `docs/landscape.md` keeps its earlier verification date as a historical note
  rather than a maintenance surface, unchanged by this run. No other file cites a date for this batch.
- **Evidence-scope discipline, stated per entry:** digital agents only (`2611.15006`, `2611.15309`,
  `2611.15268`, `2611.15276`, `2611.14654`, `2611.17294`, `2611.15223`); simulation only for the robot
  domain (`2611.16009`, RoboDojo); real robots (`2611.16632`, `2611.16872`, `2611.15928`, the latter a
  deployment report on one platform); production fleet (`2611.14969`). No entry implies independent
  reproduction, and no entry describes a promise or a placeholder as open.

## Operational notes and blockers

- **arXiv rate-limits expensive `submittedDate` queries while cheap ones succeed, and the refusal looks like
  an outage.** The first four attempts returned `429 Rate exceeded.` and `503` with a bare HTML body, while
  `search_query=all:test&max_results=1` returned 200 in under a second. Spacing every request **8 s** apart
  (25–30 s backoff on refusal) made all 18 subsequent requests succeed first time. The lesson for the next
  run is to space *submittedDate range* queries from the start rather than to diagnose an outage: an
  initial burst is what triggers the penalty.
- **The stall ended between runs, and the debt bookkeeping was right to keep the days open.** Both zero days
  became full batches worth **725** records, and 2026-11-10 — already screened once — grew by **70**. Had
  the previous run written the two zeros off, this run would have had no reason to query them and the batch
  would have entered the record only as a footnote.
- **The proportional cost of a big batch is artifact verification, not screening.** Twelve entries needed
  **eighteen** GitHub API calls (metadata, contents listings and emptiness checks), **12** arXiv abstract pages,
  **7** full-text renderings and **3** project-page fetches, against **7** day harvests. Two of those checks changed the entry text (kNN reversed, xinfer
  404 confirmed), which is why the checks are done at the source rather than from the abstract.
- **No API internal errors, no OpenSSH failure on the first attempt for `ls-remote` under the workaround.**
  `git ls-remote` needed `GIT_SSH_COMMAND='ssh -F /dev/null'` (the known `/etc/ssh/ssh_config.d` ownership
  issue); it was not needed again for `push`. No GitHub rate limit (403) was encountered: eighteen
  `api.github.com` calls (repository metadata, organisation listings and contents listings) completed
  without one.
- **Debt for the next run.** Query **2026-11-13 and 2026-11-14** — 2026-11-13 returned **0** at 14:22 UTC,
  which is mid-day for a Friday batch, so it is owed rather than closed — and re-harvest **2026-11-09
  through 2026-11-12** fully, since 2026-11-10 gained **70** records after it had already been screened once
  and 2026-11-09 gained **4**. If 2026-11-11 or 2026-11-12 is still growing, re-screen those two batches with
  abstracts, not titles: every recovery in this run came from reading the two never-screened days in full.
