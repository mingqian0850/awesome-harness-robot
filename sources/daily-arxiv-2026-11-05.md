# Daily arXiv scan — 2026-11-05 (the owed 2026-11-04 batch arrives, 2026-11-03 grows by 115 records after its own record said "screened", and fifteen inclusions)

## Scope

- **Interval owed.** The previous successful run started **2026-11-04 11:31 UTC** and committed at
  **`2026-11-04T12:39:37+01:00`** (11:39 UTC) on `main`, producing `f314974` and record
  `sources/daily-arxiv-2026-11-04.md`. That record screened the 2026-11-03 batch and 136 late arrivals from
  2026-10-29 → 2026-11-02, and closed with **"Still owed: 2026-11-04. The next run must query it by
  `submittedDate` again. And it must re-count 2026-11-03 and its neighbours."** This run started
  **2026-11-05 09:02 UTC** on `main` at `f314974`, so the interval it owes is **submission dates
  2026-11-04 → 2026-11-05 inclusive**, with 2026-11-04 the explicit debt.
- **2026-11-04 arrived, and it is a complete batch.** It returned **0** at the previous run's 11:31 UTC probe
  because the batch had not closed; at 09:02 UTC today it returns **399**, `<published>` spanning
  **`2026-11-04T00:02:41Z`–`2026-11-04T18:59:58Z`**. Every one of those is new to this repository.
- **The previous run's warning was correct again — for the third day running.** Recounting every day in the
  previous record's window found **120 records that appeared after that record was written**:

  | query (one parenthesised OR-query over cs.RO/cs.AI/cs.CL/cs.CV/cs.LG per day) | 2026-10-31 | 2026-11-01 | 2026-11-02 | 2026-11-03 | 2026-11-04 | 2026-11-05 |
  | --- | ---: | ---: | ---: | ---: | ---: | ---: |
  | count at 2026-11-05 09:02 UTC | **335** | **364** | **642** | **582** | **399** | **0** |
  | recorded by the previous run | 335 | 362 | 639 | 467 | 0 | — |
  | **late arrivals screened by this run** | **0** | **2** | **3** | **115** | **399** | — |

  **519 records** are new to this repository. Zero records from the previous harvest disappeared.
- **The late arrivals are, once more, above the declared frontier.** The 115 records added to 2026-11-03 occupy
  **`2611.05436`–`2611.05755`**, i.e. immediately *above* `2611.05377`, the ID the previous record declared the
  2026-11-03 frontier. 2026-11-02's three additions sit at `2611.05419`–`2611.05427` and 2026-11-01's two at
  `2611.02216`/`2611.05418` — the same pattern of a contiguous block becoming visible after the day it is
  attributed to. **2026-10-31 is now stable at 335 for two consecutive runs** and should not be re-harvested.
- **2026-11-05 is still owed.** It returns **0** because its batch had not closed at 09:02 UTC. It is carried
  forward, not screened.
- **Frontier probes.** An unrestricted descending `cs.RO` probe returns **`2611.06913`**
  (`2026-11-04T18:58:20Z`); the five-category union probe's newest 2026-11-04 record is `2611.06918` at
  `2026-11-04T18:59:58Z`. The previous run's frontier was `2611.05377` (`2026-11-03T18:59:56Z`), so the visible
  frontier moved by **1,536 IDs and one day**.
- **Revisions: 87 of the 2,322 harvested entries carry `<updated>` ≠ `<published>`; none of the 519 new records
  is a revision** — every new record is `v1` with `<updated>` equal to `<published>`. Fifteen revisions moved
  inside this run's window (`<updated>` ≥ `2026-11-04T10:00`); they are handled under **revision handling**.
- **Coverage.** Every record carries at least one of the five target categories. Primary categories over the 519
  new records: `cs.CV` 123, `cs.LG` 110, `cs.AI` 87, `cs.CL` 54, `cs.RO` **53**, `cs.CR` 10, `cs.SE` 8,
  `stat.ML` 7, `cs.HC` 7, `cs.AR` 4, `cs.GT` 4, `cs.DB` 4, `quant-ph` 3, `eess.IV` 3, `cs.CY` 3, `cs.DS` 3,
  `cs.DC` 3. `cs.RO` appears as primary or cross-list on **61**. Per day: 2026-11-03 late block (115)
  `cs.AI` 24, `cs.CV` 21, `cs.LG` 20, `cs.RO` 12, `cs.CL` 8; 2026-11-04 (399) `cs.CV` 102, `cs.LG` 90,
  `cs.AI` 62, `cs.CL` 45, `cs.RO` **40**.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`. **12 export-API requests**: one
  `cs.RO` descending frontier probe, **six** per-day count queries (2026-11-02 → 2026-11-05, then 2026-10-31 and
  2026-11-01 against the previous run's harvest) and **five** full-day harvests at
  `max_results=2000&sortBy=submittedDate&sortOrder=ascending` returning **335 / 364 / 642 / 582 / 399**
  entries (813 KB / 907 KB / 1.60 MB / 1.42 MB / 997 KB). Spaced 3–5 s; **zero HTTP 429**.
- **No re-download of the previous run's harvest was needed:** the untracked `.scratch/arxiv-2026-11-04/`
  directory is still on disk, so `per_day.json` from that run was diffed directly against today's parse. That
  is the reliable way to produce the late-arrival table — **diff against the on-disk harvest, never against the
  previous record's prose.**
- **`/tmp` is still not durable between tool calls**, reproducing every previous run's finding. All XML, HTML
  and JSON captures are in the untracked `.scratch/arxiv-2026-11-05/` directory, which is **not staged and not
  committed**.
- **Query syntax still matters.** The per-day counts use
  `(cat:cs.RO OR cat:cs.AI OR cat:cs.CL OR cat:cs.CV OR cat:cs.LG) AND submittedDate:[...]`; the unparenthesised
  form drops the date filter and returns a constant site-wide count.
- **Screening.** (a) All **519** titles were read in ascending ID order. (b) Candidates were split by anchor:
  **61** cs.RO-touching records read in full, plus harness-anchored titles in the non-`cs.RO` remainder
  (harness|agent|runtime|recover|memory|skill|tool|evaluat|benchmark|safety|verif|monitor|permission|sandbox|
  replay|audit|guardrail|self-improv|workflow|serving|latency|deploy|contract); the remainder was read to
  confirm it is off-scope. (c) **40 abstracts** were read in full. (d) **12 full-text HTML renderings**
  (`arxiv.org/html/<id>v1`) were fetched for artifact claims and for disambiguation of citation-versus-release.
  (e) Every claimed artifact was resolved at the source: **13 arXiv abstract pages** (eight candidates plus the
  five revised records re-checked for artifact links), **8 GitHub REST calls** (seven repositories; one repeated
  after a 504), **14 raw `README`/`LICENSE`/branch-file fetches**, **5 Hugging Face API calls** (dataset
  metadata and file tree, two collections, one listing re-read), **2 ModelScope checks** and **24 direct HTTP
  status checks**.
- **Model papers remain excluded by rule**, even when they are the batch's headline results, unless the
  contribution is an external runtime, interface, or evaluation/optimization target. The rejections below name
  the ones that came closest.

## Included (15)

### General Harness Methodology and Verification (3)

- **`2611.05650` — Attributing Agent Failures to Their History Needs an Executed Counterfactual: Toward a
  Reporting Standard** (cs.AI; NeurIPS 2026 IAB workshop, non-archival). Turns failure attribution from a
  taxonomy label over a transcript into **an executed intervention**: freeze an executable mid-trajectory
  state, alter **only the history the agent sees**, continue, and report the result per instance or through a
  validated estimator, at a state whose restoration has been *measured*, with a **replay certificate matched to
  the estimand**. Four manipulation classes are named and each is shown to have a precedent (DoVer, REFLECT,
  Min et al., Who&When Pro, Causal Agent Replay), yet **none reports a measured replay certificate** — the gap
  is measurement, not machinery. The supporting measurements are why this is the batch's strongest general
  result: a carrier taxonomy whose "concentration" could not be separated from benchmark construction
  (adjusted odds ratio **1.15**, interval crossing 1), and **hash-identical snapshots restored into identically
  seeded processes that agreed with one another in 24 of 24 states while differing from the live continuation
  in 9 and 20 of 24** — restoring the captured position of the random stream made all 24 agree. For a harness
  list this is the recording/verification contract written as a reporting rule: a checkpoint that cannot
  reproduce the continuation cannot support an attribution. Digital agents; author-reported; **no artifact
  claimed.** Placed in *General Harness Design and Self-Improvement*; cross-referenced to Post-Audit Mirage
  (`2610.31130`) and ReplayRCA (`2610.41025`).
- **`2611.06545` — When Do Summaries Become Instructions? Unsupported Policy in LLM Agents' Self-Written
  Handoff Summaries** (cs.CL). Treats context compaction as a **permissions event**, because the compactor is
  the agent itself: a summary can state a permission or requirement the record does not support, and the
  successor cannot distinguish a remembered rule from an invented one. The setup carries a **ground-truth
  permission ledger** and uses **same-snapshot replay** with rule-flagged sentences replaced by neutral filler.
  Two AI readers confirmed unsupported **restrictions** in **1.2%** and **2.4%** of all readable summaries from
  two open-weight models and unsupported **authorizations** in **0.1%** and **1.3%**. The effect testing is
  reported with the discipline this repository keeps asking for: for restrictions the **pre-specified** replay
  test was **not significant (p = 0.58)**, and only a follow-up planned afterwards found a fragile effect
  (**p = 0.027**); neutralizing false approvals cut unauthorized actions **24 → 7 of 120** held-out runs (post
  hoc; later set 17 → 11 of 48). The single pre-specified intervention that worked is a harness change, not a
  model change: replacing the system prompt's *"Some actions need approval."* with one stating **where
  requirements come from** cut unsupported restrictions **5.0% → 1.8%** (development) and **1.8% → 0.3%**
  (held-out templates), both p < 10⁻⁹, while barely changing requests for approvals the ledger requires.
  **Artifact status literal (verified this run): verified open, MIT.** `Chuw1/handoff-audit` (412 KB, created
  2026-10-10, pushed 2026-11-03) ships the diagnose and replay pipeline, a no-network selftest, hashed labels
  and a `LICENSE`; its README states the limits (tested end to end only against scripted servers; a diagnostic
  plus a 60-task benchmark, not a trained detector). Digital-agent evidence.
- **`2611.06444` — EvoGuard: Mitigating Degradation in Online Skill Evolution** (cs.CL). Gives skill evolution
  the **admission decision** the line assumes: adopting every update lets skills degrade over time, and gating
  on a held-out validation run is too slow and costly to do after every update. EvoGuard trains a **pairwise
  selector** on skills scored against a **private test set** (utility labels), with rejection sampling keeping
  only pairs that have a clear utility gap, and reads the logits of two label tokens so that **deployment decides
  in a single forward pass**. Against adopting every new skill, success rises **11.0%** on ALFWorld, **5.9%** on
  ScienceWorld and **2.7%** on LiveCodeBench. The cost model is the harness point: the alternative gates are
  paid in full evaluation runs. Digital-agent evidence; author-reported; **no artifact located** — the full text
  contains no first-party repository, and the only URLs in it are citations.

### Agentic Robot and VLA Harnesses (3)

- **`2611.06863` — Rethinking Self-Repair in Code-as-Policy Agents: What Drives Retry Success?** (cs.RO; CoRL
  2026 Learn@Deploy workshop). Asks what part of a retry loop does the work, and supplies the three baselines
  self-repair evaluations omit. Everything is fixed except the feedback: a **fixed lookup** selects feedback
  templates from failure labels and the API variant, a **frozen code model** writes a fresh program after each
  scene reset, and every condition starts from the **same 252 initial failures**. Qwen2.5-Coder-7B recovers
  **85.3%** within five retries against **68.3%** for resampling at the same cap, and the advantage holds under
  a **fixed total-token budget**. Then the ablation takes most of the credit back: feedback from **other
  episodes** and **fixed task guidance** reach similar point estimates and matching the **coarse failure label**
  yields no detected benefit; a fixed guided prompt used from the *first* attempt reaches an observed overall
  success rate similar to template feedback. One task regresses — nut insertion needing a rim grasp, where
  template feedback is consistent with an incompatible center-grasp instruction — so a feedback library can
  inject a wrong plan while looking like diagnosis. The stated consequence is a contract: self-repair
  evaluations should carry **resampling, mismatched feedback and concise task-compatible static guidance** as
  baselines. Simulation-only (robosuite-style manipulation); author-reported; **no artifact located.**
- **`2611.06654` — Compounding Physical RSI: Turning Execution Failure into Inherited Capability** (cs.RO).
  Makes **the failing stage, not the failed episode**, the update unit: because completion behaves like a
  product of stage pass rates, recursive gains can multiply — but terminal feedback rarely localizes the stage,
  a failed trajectory does not contain the corrective action, and shared-parameter updates degrade mastered
  skills. The three answers map one-to-one: **capability-gap assessment** localizes the stage and writes a
  structured improvement objective; the **remedy** stage refines the policy with **behavior-regularized offline
  RL** on validated, stage-relevant experience (no online rollouts per optimization step); the **retention**
  stage builds **weight-space candidates between the inherited reference policy and the refined one**,
  evaluates them across the task suite, and promotes the best as the next generation's reference. On all **42**
  RoboDojo-Sim tasks the submitted agent ranks first with a three-seed mean Overall Score **26.15** and success
  rate **20.70%**, **+5.92** points and **+5.82** pp over the next entry. Simulation-only; author-reported;
  **no artifact located** — the only repository link in the full text is a third-party dependency
  (`NVIDIA/Isaac-GR00T`).
- **`2611.06768` — PyRUA-REAL: Robot Exploration and Adaptive Learning through RSI in the Real World** (cs.RO).
  Third in the PyRUA line and the one that moves the loop onto physical interaction: coding agents keep
  interaction experience as **persistent memory** and reusable **executable skills**, and the study asks which
  execution feedback supports code refinement, how agents collaborate, and how task specifications and prior
  skills change robustness and transfer. The harness-relevant half measures the resulting programs **deployed
  with no online agent in the decision loop** — what survives the exploration is frozen code, not the agent.
  The authors report **91.1%** average success on **eight real-world tasks** and **84.4%** on **42 RoboDojo**
  tasks. **Artifact status literal (verified this run):** the Apache-2.0 `DAGroup-PKU/PyRUA-Lean` repository
  (914 MB, 188★, pushed 2026-11-05) now carries
  `integrations/xpolicylab/evaluation/robodojo_summary_rsi.md`, whose **seed-0** RoboDojo summary for the
  frozen-program policy reports Average SR **82.82** / Score **87.16** at 42 of 126 cells (33.3% of the intended
  grid) and **80.73 / 85.36** on eval seed 2, which was never used in development. The abstract's **84.4%** is
  a different aggregation over the same 42 task names, so the two should not be read as one measurement. Real
  robots, author-reported. Continues `2610.01939` (harness) and `2611.05320` (data collection) rather than
  replacing them.

### Evaluation and Safety (6)

- **`2611.05567` — MESA: An Evaluation Framework for Compositional, Semantic, and Spatial Generalization in
  Robotics** (cs.RO/cs.AI; CoRL 2026). Makes generalization a set of **separately measurable axes**: five suites
  isolate in-distribution performance and four axes — **spatial configuration, object instance, object
  category, subtask composition** — so a result says *where and why* a policy fails. **MESA-Gen** (built by
  improving MimicGen) populates the suites at scale, and **both platforms are released**: the single-arm MESA
  suite and **BiMESA**, described as the first large-scale bimanual generalization benchmark. Seven baselines
  run; the strongest reaches **52%** on MESA and **49%** on BiMESA, and MESA rankings are broadly consistent
  with **RoboArena**. **Artifact status literal (verified this run): verified open, MIT, with weights and
  data.** `pairlab/mesa-env` (12.8 MB, created 2026-03-13, pushed 2026-10-30) carries MESA-Gen, MESA-Bench,
  setup scripts and docs; the project site resolves (HTTP 200); the Hugging Face collection
  `albertwilcox/mesa` holds **15 ungated items** — eight checkpoints (`mesa-pi0`, `mesa-pi05`, `mesa-pi0-fast`,
  `mesa-gr00t-n1.6` and their `bimesa` counterparts) and seven datasets (HDF5/LIBERO exports, train splits,
  initial states, assets). Simulation-only; author-reported.
- **`2611.06475` — Robocasa-Mem: Benchmarking Environment and Process Memory in Long-Horizon Mobile
  Manipulation** (cs.RO). Separates **environment memory** (locations, attributes, relationships) from
  **process memory** (single-task progress, multi-task scheduling) across nine RoboCasa mobile-manipulation
  tasks. The methodological contribution is **Behavior-Conditioned Evaluation**: where several execution paths
  are valid, the evaluator **derives later task constraints from the robot's own earlier actions** instead of
  fixing a target or a trajectory, which removes the shortcut of passing a memory task by aiming at a known
  fixed goal. Ships **1,800** successful demonstrations and the **HMAP** baseline (planner over structured
  entity/process memory; VLA controller with subgoal-conditioned VGGT-Omega geometry). **Artifact status
  literal (verified this run): verified open, MIT, partial packaging.** `SiHhangChen/RoboCasa-Mem` (43.6 MB,
  created 2026-10-14, pushed 2026-11-05) releases evaluation code and **27 eval suites** (ACT, Diffusion
  Policy and π0.5 × nine tasks) with summarization tooling; simulator sources, assets, model implementations
  and checkpoints install separately, with simulator assets and the dataset as separate ModelScope downloads
  (both resolve, HTTP 200); the README states this is version **`0.1.0rc1`** that still "needs installed-wheel
  acceptance after the removal of real-robot integration". Simulation-only; author-reported.
- **`2611.06881` — VABAR: A Variational Automation Benchmark for Agentic Robotics** (cs.RO). Names the regime
  benchmarks skip — **Variational Automation**, one known job across a *known distribution* of object shape and
  pose — and writes the requirements down before instantiating them (accessibility, repeatability, diversity,
  relevance, physical realizability, **saturation resistance**, computational efficiency). Six task classes
  cover industrial, medical and infrastructure work (tool sorting, tool hanging, gear meshing, syringe packing,
  cable insertion, cable routing) on the **Newton** engine, with standardized variations, difficulty tiers and
  a **33-skill library** whose entries come with human-authored programmatic reference solutions, so an agentic
  system can be scored against a written solution. Baselines run inside two harnesses — the open agentic
  manipulation harness **Graph-as-Policy (GaP)** and **Isaac Lab Arena** — placing it in the harness-comparison
  line. **Artifact status literal (verified this run): announced, not released.** The project site resolves
  (HTTP 200) and names exactly two repositories, `github.com/BerkeleyAutomation/VABAR` marked **"(available by
  Nov 9)"** and the two simulation platforms; the VABAR repository returns **404** from the GitHub API on
  2026-11-05, `raw.githubusercontent.com` returns 404 for `README.md` on both `main` and `master`, and the arXiv
  record carries no code link. Simulation-only; author-reported.
- **`2611.06653` — Detecting Silent Action-Interface Mismatch with Calibrated Relational World Models**
  (cs.RO/cs.LG; CoRL 2026 UncertainWM workshop). Isolates a failure that passes every constraint check: an
  action can satisfy **shape and range** yet carry the wrong **semantics** (axes permuted, signs inverted,
  rotations rescaled, gripper polarity reversed), because the harness validated the tensor and not the meaning.
  Learned **state-action-effect relations** become an **offline contract**: clean held-out transitions calibrate
  the alarm, a fixed inverse vocabulary gives **closed-set localization** of the offending fields. Capacity-
  controlled, at **2.5% empirical clean false-positive rate**, over ten LIBERO Spatial tasks and **432** source
  episodes: ridge forward residual **46.1%** recall, four-component joint Gaussian mixture **61.0%**, exact
  conditional of that same mixture **65.7%** — **+14.9 pp** for the joint score, **+4.7** more from
  conditioning, with localization **95.8% vs 94.6%**. The negative results are reported with the positive ones:
  **DROID, all three HYDRA tasks, short temporal faults, small offsets, exact repair and simulated mitigation
  fail their gates**, and xArm contributes one task, so the claim is a bounded offline diagnostic and a
  **detection-localization tradeoff**, not joint-density superiority or cross-robot safety. Simulation-only;
  author-reported; **no artifact located.**
- **`2611.06585` — Signed, Sealed, Leaked: Trust Beyond Signature for MCP Toolchains** (cs.CR/cs.AI/cs.MA).
  Shows **authenticity is not behaviour**: a signed MCP tool performs its advertised function while embedding
  sensitive content in requests to a destination the runtime policy **allows**, so no blocked egress exists to
  catch, and Meta's **MCPGuard-Dynamic** blocks a direct exfiltration attempt in the same run yet **fails to
  prevent this attack in every trial**. **Germline execution** moves the boundary from inspection to **data
  availability**: each invocation runs in an ephemeral copy forked from a **data-free** germline process and
  receives only information authorised for its destination, so executing malware can disclose no more than its
  copy holds. Under explicit assumptions the paper proves **sink noninterference** for every signed release and,
  for releases deterministic within its replay model, shows a verified witness establishes a flow violation.
  Across **153** attack runs no unauthorised disclosure was detected against **5.9–74.5%** of runs under
  baselines and an ablation; served invocations complete in **10–49 ms**, **2–28×** faster than restarting the
  tool per call. Digital-agent evidence; author-reported; **no first-party artifact located** — the two
  repositories in the text are the baselines' own (`facebook/mcpguard-dynamic`,
  `microsoft/agent-governance-toolkit`).
- **`2611.06453` — Beyond Infraction Scores: Failure-Boundary Profiles for Closed-Loop Evaluation of Driving
  and Robot Navigation Agents** (cs.RO/cs.CV/cs.SE). Replaces a composite score with a **measured boundary**,
  motivated by a sign error in ranking: in a corridor closed by two parked cars, the agent that **drives
  through with contact** averages **52.2** on the blocked widths while the agents that **stop** average
  **37.8–40.6**. Each **scenario family** varies one measured physical factor, each variant is **certified from
  measurement** as proceed-or-stop, and two curves are estimated per agent along that axis — **commitment** and
  **competence** — with intervals carrying the simulator's run-to-run variance; the certified labels also give
  a summary metric, **decision accuracy**. Seven driving agents over seven CARLA families: the rule-based expert
  **0.74**, the highest-scoring learned agent **0.60**; agents with similar driving scores fail differently, and
  agents that stop at a blocked corridor still rarely wait for a gap in oncoming traffic. Applied **unchanged**
  to a warehouse robot in Isaac Sim: it stops at blocked gaps in **one of three** families, and two fine-tuned
  versions pass more gaps cleanly while **no longer stopping at all**. **Artifact status literal (verified this
  run): public with split licensing, and a stated partial release.** `speedym/failure-boundary-profiles`
  (106 MB, created 2026-08-24, pushed 2026-10-27) carries ten certified families with `SHA256SUMS`, a
  byte-identical export of the measurement chain that renders every figure, table and number,
  `PUBLIC_EXPORT.json` (per-file hashes plus the private commit), `PUBLIC_PROOF.json`, a
  `verify_scl_export.py` checker, and the **Fail2Drive** harness vendored at upstream `bceb18a` with the fork's
  two patches kept in-tree; the README states three kinds of material under **two** licenses, so the GitHub API
  reports `NOASSERTION`, and the **warehouse families ship as manifests and catalogs only because their runner
  is not released**. Dataset DOI `10.57967/hf/11195` resolves (HTTP 200). The repository README titles the paper
  "Beyond Driving Scores" while the arXiv title reads "Beyond Infraction Scores". Simulation; author-reported.

### Measurement, Deployment and Foundation Models (3)

- **`2611.05838` — RACE: Calibrated Cumulative Scoring for Rollout-Free Robot Policy Checkpoint Selection**
  (cs.RO/cs.AI/cs.LG; NeurIPS 2026 Robot Learning Workshop). Attacks the criterion this repository flagged as
  unsound — averaged validation loss as a rollout-free stand-in for deployment success — and names two reasons
  that are harness claims: the criterion **penalizes action deviations without asking whether the policy can
  recover**, and **weights later reference predictions equally after an earlier deviation has moved execution
  off the reference**. RACE calibrates deviations against **state-conditional thresholds** derived from action
  differences at similar states in held-out successful trajectories and aggregates them with a **running
  product**, so a large deviation discounts everything after it. Project-page numbers: **0.83** rank correlation
  with deployment success in simulation (95% CI **0.72–0.92**; 8 tasks × 10 seeds, 12 checkpoints per run, three
  reference sources) and **1,800 real-robot trials** (six tasks, two policy families, 30 trials per checkpoint).
  **Artifact status literal (verified this run): project page only.** `changyeon.site/race` resolves (HTTP 200)
  with videos, per-task plots and a paper PDF, and its code control reads **"Code (coming soon)"** — nothing
  counted as open. Simulation plus real robots; author-reported.
- **`2611.06710` — Where Is VLA Inference Time Spent? Bottlenecks and Optimization Interactions Across GPUs**
  (cs.RO). Turns VLA latency from a reported number into a **hardware-dependent attribution**:
  **seven families** of inference optimization on **seven VLAs** across **four GPU families** (H100 to Jetson
  Thor), separating **CPU launch overhead, memory bandwidth and kernel efficiency**, and measuring how one
  method changes another's benefit. The deliverable is **VLA-Triage**, stacking guidelines under a latency
  budget that identify which stages can supply the required savings and which combinations must be retested in
  the **deployed execution mode**. Measurement study; author-reported; **no artifact located** — the repositories
  cited are optimization toolkits (CUTLASS, FlashInfer, torch-ao, gemlite), not releases of this work.
- **`2611.06817` — Open-HumanScale: An Open Recipe for Scaling Human and Robot Experience for Embodied
  Foundation Models** (cs.CV). Publishes the **recipe and the scaling law**: pretraining is scaled from **100 h to
  50,000 h** of curated egocentric video (hand motion and natural-language annotations), and pure-human
  pretraining shows an empirical **log-linear** relation with downstream robot success — scene-OOD success
  **49.1% → 73.2%** and random success **21.5% → 42.9%** across that range, against **29.2% / 7.7%** without
  egocentric pretraining. **HumanWAM-Vanilla** (50k hours, human only) reaches **92.08%** Clean2Clean and
  **68.88%** Clean2Random on RoboTwin 2.0 post-training, plus **14.17%** mean success / **19.00** progress on
  RoboDojo. Joint human–robot pretraining adds what human data lacks: **HumanWAM-Goal** reaches **92.70% /
  84.60%**, moving RoboTwin zero-shot success **11.48% → 13.94%** and 20 real-world tasks **20.0% → 30.5%**, and
  is reported to adapt to unseen visual goals on physical robots with **no parameter update**. **Artifact status
  literal (verified this run): a precisely stateable partial release.** The `open-humanscale` branch of
  `DAGroup-PKU/HumanNet` is public and documents the recipe but the repository **declares no licence** (GitHub
  reports a null license; no `LICENSE` on the branch), so the code is source-available rather than open; the
  corpus repository `DAGroup-PKU/Open-HumanScale-50K` is public and ungated but its card states **"DATA UPLOAD
  IN PROGRESS"** at **4.71 TB of 26.52 TB (17.76%)**, ETA 2026-11-11; the weights collection
  `DAGroup-PKU/open-humanscale` exists and is **empty (zero items)**; the README says weights and corpus "are
  being uploaded" while code and documentation are still being organised. Simulation plus real robots;
  author-reported.

## Revision handling

- **87 revisions** in the harvest, **15** with `<updated>` ≥ `2026-11-04T10:00`. None adds an artifact link, and
  none changes a prior verdict: `2611.04527` (Future Representation Distillation for Imagination-free WAM, v2)
  and `2611.02474` (Gnomon4D, v2) remain model papers rejected in the 2026-11-04 and 2026-11-03 records
  respectively — their `arxiv.org/abs` pages carry no repository or weight link beyond page boilerplate —
  `2611.05163` (RoboTryOn, v2) remains an excluded demonstration-synthesis paper, and
  `2611.05266`/`2611.03501`/`2611.05244` (agentic-RL stabilization, inertia mismatch, swarm robotics) are new
  revisions of records screened earlier without a qualifying contribution. **No revision produced a repository
  update this run.**

## Excluded and watch-listed

Read and rejected; the reason is recorded so a later run can tell deliberate rejection from oversight.

- **`2611.06184` Can Astra Solve RoboMME without Breaking the Bank?** (cs.RO; CoRL 2026 workshops) — rejected
  as **duplicate coverage, not as weak work.** The three-tier system it reports (GPT-6 Astra planner, π0.5
  executor, fine-tuned Qwen3-VL-4B completion monitor; **79.13%** RoboMME with **3.63** Astra calls and
  **80.84 s** per episode; +34.62 points over the best non-oracle baseline, −4.95 from GroundSG+Oracle) is the
  same system already curated as **LogLook** (`2610.32348`), which shares the `bingaochen/Astra-on-RoboMME`
  artifact and states the same 79.13% figure for the full loop. The project page resolves (HTTP 200) and adds a
  different system configuration, not a different system.
- **`2611.05632` Assurance-Conditioned Continuous Authorization for Autonomous AI Agents** (cs.AI/cs.CR; 81
  pages) — a harness-independent authority design (evidence validity vs contextual applicability, a typed
  dependency graph for selective re-evaluation, authority epochs, effect-time enforcement) evaluated on
  controlled scenario-regression suites. **Watch-list:** the permissions line already carries One Gate Is Not
  Enough, HarnessRisk and now germline execution and handoff-summary policy; this adds a fourth framing without
  an artifact or a robot experiment, so it is held until an implementation appears.
- **`2611.06803` Task-Efficient Agent Evaluation Using Cached Traces** (cs.AI) — good measurement idea (an LLM
  summary of each execution trace, embedded, with system-identity/harness signal removed; MAE 0.13 → 0.08 on
  SWE-bench Verified at one executed task). **Watch-list:** no artifact, and it optimizes benchmark *throughput*
  rather than the harness contract.
- **`2611.06876` SpecCompact** (cs.CL) — slot-parallel speculative compaction with **2.04×–5.63×** speedups and
  an **8.68×** median user-study gain; **watch-list**, because context-compaction latency is already covered by
  this list's serving line and the paper reports model- and task-dependent degradation without an artifact.
- **`2611.06483` What Does Multi-Turn RL Really Improve in LLM Agents?** (cs.LG/cs.CL) — the finding is
  relevant ("strength concentrates in error recovery and long-horizon resilience"; gains depend on turn budget),
  but it is an **RL-training** result, not a runtime/interface contribution; watch-list with the project page
  (`rethinking-agentic-rl.github.io`, HTTP 200) noted for a future artifact release.
- **`2611.05450` ReScopeBench** (cs.AI) — experience-lifecycle benchmark over 18 environments with **silent**
  environmental changes and 20 evaluated models; excluded because no artifact was located and the benchmark
  concerns digital agents' experience retention rather than robot execution.
- **`2611.05590` Verified but Vacuous** (cs.AI) — the three-surface distinction (verifier acceptance / spec
  binding / spec fidelity; **99.1%** acceptance vs **1.9%** binding on HumanEval) is a good audit; excluded as a
  software-verification result with no harness artifact, watch-listed for the next pass over verification claims.
- **`2611.06868` Turn-level best-of-N sampling for LLM agents** (cs.AI), **`2611.06674` AgentTrustCI** (cs.SE),
  **`2611.06414` LExMem** (cs.CL), **`2611.06910` FrontierPhysics**, **`2611.06841` ServerBench**,
  **`2611.06858` CompressionBench** — read; each is either an inference-time scaling study, a CI framework for
  non-robot Java agents, or a benchmark for coding agents' systems engineering. Rejected as adjacent to
  harness work but not about robot or VLA execution contracts. Note `2611.06414`'s own cited repository
  (`github.com/mbzuai-nlp/LExMem`) **returns 404**, so its artifact claim is unbacked.
- **Robot and VLA model papers excluded by rule** (no external runtime, interface, or evaluation/optimization
  target): `2611.05448` Jev with Foresight, `2611.05507` Ctrl-F, `2611.05571` ELA, `2611.05585` contact-implicit
  humanoid locomotion, `2611.05615` C∞ trajectory planning, `2611.05667` proactive HITL intervention,
  `2611.05738` OdorSpaces, `2611.05754` GRASP, `2611.05847` Time-to-Goal (JEPA temporal metric), `2611.05885`
  RRT* on terrain meshes, `2611.05897` FWIK, `2611.05929` experience replay, `2611.05936` LumiTac, `2611.05938`
  VIO, `2611.05979` motion priors, `2611.06028` sliding-mode control, `2611.06118` online adaptation,
  `2611.06121` projection-space pose, `2611.06162` LLM support in underwater robotics, `2611.06196` learned safe
  sets, `2611.06214` continuum robots, `2611.06287` corridor monitoring, `2611.06308`/`2611.06378` SLAM and loop
  closure, `2611.06359` soft-object grasping, `2611.06447` A³-VLA, `2611.06523` H²MARL, `2611.06564` humanoid
  soccer, `2611.06588` FoSco-Net, `2611.06625` soft trunk control, `2611.06649` SCRUB, `2611.06673` tactile cues,
  `2611.06689` ROV teleoperation, `2611.06702` aerial refinement, `2611.06802` ConDR, `2611.06905` RL scaling of
  generalist policies, `2611.06908` perceptive humanoid motion, `2611.06913` cuRoboV2-Fast, `2611.06918` NEO-VLA
  (code link `EvolvingLMMs-Lab/NEO/tree/main/VLAKit` noted — a model contribution, not a harness).
- **`2611.06724` Dexterous Robotic Hands in the Foundation-Model Era** (cs.RO; 45 pages) — substantial survey,
  but a survey; the list's *Surveys and Reading Lists* section is curated separately and this one was not
  promoted on a first pass.
- **`2611.06627` Interactive World Simulation with Cosmos for Physical AI** (cs.AI/cs.RO) — NVIDIA's framework
  paper for real-time interactive Cosmos simulation, with code and checkpoints at `github.com/NVIDIA/Cosmos` and
  a Hugging Face collection. Excluded as an **extension of a platform this list already carries** (NVIDIA Cosmos
  is an existing entry) rather than a new harness contract; watch-listed for an update if the interactive
  interface ships separately.
- **`2611.06704` Where It Breaks (Pac-Man agent diagnosis)**, **`2611.06434` EviSeek** (embodied evidence
  seeking, ESI-Bench +7.1 points), **`2611.06911` BEAKER** (self-driving-lab benchmark), **`2611.06890`**
  procedural adversarial learning, **`2611.06918`** above — read; each is a diagnosis, benchmark or model
  result without a runtime/interface contract this list would own.
- **Out-of-scope remainder** (driving-perception, medical imaging, video generation, molecules, weather,
  time-series, quantum, federated learning, recommendation, database and ML-theory records among the 519) — read
  at title level and rejected as conventional model/application work under the standing scope rule.

## Validation performed

- **Markdown structure.** `README.md` renders as valid Markdown: the four new insertion points sit inside their
  existing `###` sections, each new entry is a single top-level bullet with no nested list breakage, and the
  table of contents anchors are unchanged. `sources/daily-arxiv-2026-11-05.md` has one `#` heading and uses only
  `##`/`###` below it.
- **`git diff --check`** reports no whitespace or conflict-marker problems.
- **Links.** Every non-arXiv link added to `README.md` was resolved at the source this run:
  `github.com/Chuw1/handoff-audit` (200, MIT), `github.com/pairlab/mesa-env` (200, MIT),
  `pairlab.github.io/MESA` (200), `huggingface.co/collections/albertwilcox/mesa` (200, 15 ungated items),
  `github.com/SiHhangChen/RoboCasa-Mem` (200, MIT), `modelscope.cn/datasets/ouhebai/RoboCasa-Mem` (200),
  `berkeleyautomation.github.io/vabar-website/` (200), `github.com/BerkeleyAutomation/VABAR` (**404 via the
  GitHub API** and 404 on `raw` for both default branches — cited only in a rejection statement, never as an
  open artifact), `changyeon.site/race` (200, code marked "coming soon"),
  `github.com/speedym/failure-boundary-profiles` (public; web front end returned 504 twice, but the REST API and
  `raw` both resolve, license `NOASSERTION`), `doi.org/10.57967/hf/11195` (200),
  `github.com/DAGroup-PKU/HumanNet/tree/open-humanscale` (200, no license),
  `dagroup-pku.github.io/open-humanscale` (200),
  `huggingface.co/datasets/DAGroup-PKU/Open-HumanScale-50K` (200, upload 17.76%),
  `huggingface.co/collections/DAGroup-PKU/open-humanscale` (200, **zero items**),
  `modelscope.cn/datasets/DAGroup-PKU/EgoCorpus-50kH-LeRobot-v3` (200), and
  `github.com/DAGroup-PKU/PyRUA-Lean` (200, Apache-2.0). The 404s and the empty collection are named in the
  entries and in this record; none is described as open source.
- **Duplicate IDs.** All 15 new IDs were checked against `README.md`, `docs/*.md` and `sources/*.md` before
  insertion (**0 of 519** batch records were previously named in the repository). After insertion each new ID
  occurs exactly once in the main list (as its entry link) and one to three times in `## Current Landscape`
  summaries, which is this repository's established pattern — the already-curated `2611.05320` appears three
  times and `2611.04226` twice.
- **Dates and categories.** Every included entry states its arXiv ID, and its `<published>` date falls inside the
  harvested window (2026-11-01 → 2026-11-04); no entry cites a 2026-11-05 record, because none exists.
  Category placement follows each paper's contribution rather than its arXiv primary: `2611.06817` (cs.CV
  primary) sits under *Datasets*, `2611.06881` (cs.RO) under *Benchmarks*, `2611.05838`/`2611.06453` under
  *What to Measure*, `2611.06710` under *Runtime Building Blocks*, and `2611.06585`/`2611.06653` under *Safety*.
- **Cross-file consistency.** The README badge, the `## Current Landscape` `Last verified:` line and the Contents
  blurb now read **2026-11-05**, matching this record's filename and the newest entry's verification date. The
  2026-11-04 record and earlier history are unmodified. `docs/landscape.md` keeps its own earlier verification
  date because it is a historical note, not a maintenance surface.
- **Artifact-status discipline.** Six distinct literal statuses are recorded and each is stated in the entry
  itself: **verified open with a license and released weights/data** (MESA: MIT + 15 ungated HF items;
  RoboCasa-Mem: MIT + 27 eval suites at `0.1.0rc1`), **verified open, MIT, code only** (handoff-audit),
  **public with split licensing plus a stated partial release** (failure-boundary-profiles; warehouse runner not
  released), **source-available without a licence and only 17.76% of the corpus uploaded with an empty weights
  collection** (Open-HumanScale), **project page only with the code marked "coming soon"** (RACE), and
  **announced but 404** (VABAR). Seven entries claim no artifact at all and say so (`2611.05650`, `2611.06444`,
  `2611.06863`, `2611.06654`, `2611.06653`, `2611.06585`, `2611.06710` — seven, in fact). No entry describes an
  unverified artifact as open source.
- **Author-versus-independent separation.** Every number in all 15 entries is marked as what the authors
  report; no paper in this batch has an independent reproduction. Evidence scope is stated per entry:
  simulation-only for `2611.06654`, `2611.06863`, `2611.05567`, `2611.06475`, `2611.06881`, `2611.06653` and
  `2611.06453`; digital-agent-only for `2611.05650`, `2611.06545`, `2611.06444`, `2611.06585`, `2611.05838`
  (whose simulation result then transfers to real trials) and `2611.06803`'s rejected sibling; and
  simulation-plus-real-robot for `2611.06768`, `2611.05838` and `2611.06817`. One cross-source discrepancy is
  recorded rather than smoothed: PyRUA-REAL's abstract (**84.4%**) and its own published seed-0 summary
  (**82.82**) use different aggregations over the same 42 task names.

## Operational notes and blockers

- **Three consecutive runs have now caught a "screened" day growing after the fact** (117 → 115 → 120 late
  records). The operational consequence is unchanged and should be treated as the pipeline's normal behaviour:
  **recount the previous two or three `submittedDate` ranges and diff against the on-disk harvest before writing
  "closed".** 2026-10-31 is the first day in this window to hold still across two runs (335 both times).
- **Reusing the previous run's `.scratch` harvest is what made the diff exact.** `.scratch/arxiv-2026-11-04/`
  was still on disk, so today's 519-record delta is a set difference over parsed XML, not a comparison of prose.
  `.scratch/` remains untracked and uncommitted by design.
- **The 2026-11-04 debt was real and worth 399 records, i.e. 77% of this run's new material.** At 09:02 UTC on
  its own day it returned 0; at 09:02 UTC the next day it returned a complete batch whose last `<published>`
  stamp is `2026-11-04T18:59:58Z`. **A day is only harvestable after its batch closes, roughly a day later.**
- **Two of this run's artifact checks needed persistence rather than a single status code.**
  `github.com/speedym/failure-boundary-profiles` returned **504** from the web front end twice while its REST
  API and `raw.githubusercontent.com` both resolved — a 504 is GitHub infrastructure, not a missing repository —
  and `github.com/BerkeleyAutomation/VABAR` returned 504 from the web front end while the **REST API returned a
  definitive 404**, which is the one that decides. **Prefer the API or `raw` over the HTML front end for
  existence checks.**
- **A project page can promise a repository in public.** The VABAR site prints "github.com/BerkeleyAutomation/
  VABAR (available by Nov 9)" beside an arXiv placeholder (`arXiv:XXXX.XXXXX`), which is the cleanest example
  this pipeline has recorded of a release date standing in for a release.
- **`/tmp` is still non-durable across tool calls**, confirmed again; `.scratch/arxiv-2026-11-05/` holds every
  response, the five day harvests, twelve full-text renderings, the GitHub/Hugging Face JSON and the parsing
  scripts.
- **OpenSSH workaround still required.** `git fetch`/`git ls-remote`/`git push` fail with a "Bad owner or
  permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf" error; `GIT_SSH_COMMAND='ssh -F /dev/null'`
  restores access with the user's key and `known_hosts` unaffected.
- **Still owed: 2026-11-05.** The next run must query it by `submittedDate` again, and **must recount 2026-11-04
  and 2026-11-03** before trusting either count: on the evidence of the last three days, a batch's membership is
  not final on the day it first appears.
