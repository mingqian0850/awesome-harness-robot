# Daily arXiv scan — 2026-11-03 (the owed three days arrive as three full batches, plus a 125-record withheld block that crossed the monthly ID rollover; twenty-two inclusions and one artifact promotion)

## Scope

- **Interval owed.** The previous successful run started **2026-11-02 13:37 UTC** and committed at **2026-11-02 14:46 UTC**
  (`2026-11-02T14:46:54+01:00`) on `main` at `2feae39` with record `sources/daily-arxiv-2026-11-02.md`. That record screened
  the withheld 2026-10-29 block and the full 2026-10-30 batch and explicitly closed with **"Still owed: 2026-10-31,
  2026-11-01 and 2026-11-02. The next run must query all three by `submittedDate` again."** This run started
  **2026-11-03 16:05 UTC** on `main` at `2feae39`, so the interval it owes is **submission dates 2026-10-29 → 2026-11-03
  inclusive**, with 2026-10-31, 2026-11-01 and 2026-11-02 the explicit debt.
- **All three owed days arrived, and two of them are among the largest batches this record series has screened.**

  | query (one parenthesised OR-query over cs.RO/cs.AI/cs.CL/cs.CV/cs.LG per day) | 2026-10-29 | 2026-10-30 | 2026-10-31 | 2026-11-01 | 2026-11-02 | 2026-11-03 |
  | --- | ---: | ---: | ---: | ---: | ---: | ---: |
  | `submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` | **626** | **480** | **331** | **350** | **522** | **0** |
  | recorded by the previous run | 620 | 361 | 0 | 0 | 0 | — |

- **The frontier moved by three days.** An unrestricted descending `cs.RO` probe returns **`2611.03647`**
  (`2026-11-02T18:59:32Z`) and the five-category union probe returns **`2611.03647` / `2611.03648`** at the same timestamp,
  where every run since 2026-10-30 was pinned at `2610.42146` / `2610.42152` on `2026-10-30T17:59:xxZ`.
- **125 records became visible with IDs *below* the prefix the previous run was watching.** Set difference against the
  previous run's on-disk harvest is exact: **10-29 626 − 620 = 6 added; 10-30 480 − 361 = 119 added; 0 removed.** The
  added IDs run **`2611.00120`–`2611.00473`** — the **November** ID range, not the `2610.41xxx`–`2610.42xxx` range the
  previous run was diffing — while their `published` timestamps span **`2026-10-29T08:01:44Z`–`2026-10-30T23:57:59Z`**.
  This is a withheld block that **straddled the monthly ID-prefix rollover**: the timestamps are October, the identifiers
  are November. Watching the descending frontier by ID prefix would have shown "nothing new"; the `submittedDate` range
  query is what surfaced them.
- **2026-11-03 is still owed.** It returns **0** because its batch had not closed at 16:05 UTC. It is carried forward, not
  screened.
- **Revisions: 49 of the 2,309 harvested entries carry `<updated>` ≠ `<published>`**, all of them records first announced
  in earlier batches whose versions moved inside this window. **None of the 1,328 records new to this repository is a
  revision** — every one is `v1` with `<updated>` equal to `<published>`.
- **Coverage.** Every record carries at least one of the five target categories. Primary categories over the 1,328 new
  records: `cs.LG` 333, `cs.CV` 268, `cs.AI` 222, `cs.CL` 151, `cs.RO` **129**, `stat.ML` 33, `cs.CR` 25, `cs.SE` 19,
  `eess.IV` 16. `cs.RO` appears as primary or cross-list on **149**. Per day: 2026-10-30 withheld block (119) `cs.AI` 27,
  `cs.LG` 26, `cs.CV` 14, `cs.RO` 11, `cs.CL` 11; 2026-10-31 (331) `cs.LG` 85, `cs.AI` 56, `cs.CV` 56, `cs.CL` 42,
  `cs.RO` 32; 2026-11-01 (350) `cs.LG` 93, `cs.CV` 82, `cs.AI` 48, `cs.CL` 44, `cs.RO` 26; 2026-11-02 (522) `cs.LG` 128,
  `cs.CV` 114, `cs.AI` 91, `cs.RO` **60**, `cs.CL` 54.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`. **17 export-API requests**: two frontier probes
  (`cs.RO` descending; five-category union descending), ten per-day count queries (including the re-checks below), and
  five full-day harvests at `max_results=2000&sortBy=submittedDate&sortOrder=ascending` returning **626 / 480 / 331 / 350 /
  522** entries (1.55 MB / 1.18 MB / 802 KB / 867 KB / 1.30 MB). Spaced 3–5 s; **zero HTTP 429**.
- **`/tmp` is still not durable between tool calls.** A probe file written under `/tmp` in the first call was gone in the
  second, reproducing every previous run's finding. All XML, HTML, JSON and page captures are in the untracked
  `.scratch/arxiv-2026-11-03/` directory, which is **not staged and not committed**.
- **Query syntax still matters.** The per-day counts use
  `(cat:cs.RO OR cat:cs.AI OR cat:cs.CL OR cat:cs.CV OR cat:cs.LG) AND submittedDate:[...]`; the unparenthesised form drops
  the date filter and returns a constant site-wide count.
- **Screening.** (a) All **1,328** titles were read in ascending ID order. (b) Candidates were grouped by robot anchor
  (VLA, manipulation, world-action, embodied, humanoid, grasp, dexterous, teleop, tactile, proprioception) and harness
  anchor (harness, scaffold, runtime, recovery, oversight, memory, skill, tool-use, evaluation, safety, verifier, monitor,
  permission, sandbox, replay, audit), selecting **241** robot-anchored, **1,046** harness-anchored and **196** carrying
  both; a tighter harness regex over the non-robot remainder selected **398** for ranking. (c) **~60** abstracts were read
  in full. (d) **18** full-text HTML renderings were fetched (`arxiv.org/html/<id>v1`, plus three `v2` renderings for
  watch-list revisions) for artifact claims, release statements and citation-versus-release disambiguation. (e) Every
  claimed artifact was resolved at the source: **10 GitHub REST calls**, **8 Hugging Face API calls**, **6 project pages**,
  **3 repository `README`/registry fetches**, **2 `LICENSE` reads**, and **12** direct HTTP status checks.
- **A "3-page workshop note" and a "1-page project card" are still papers and still get checked.** The smallest inclusion
  this run is a three-page CIKM industry-day note; the largest artifact claim rejected was a paper whose entire code
  pointer is a 404.
- **Model papers remain excluded by rule**, even when they are the batch's headline results, unless the contribution is an
  external runtime, interface, or evaluation/optimization target. The rejections below name the ones that came closest.

## Included (22)

### General Harness Methodology and Verification (4)

- **`2611.02079` — ParetoSaddler: Pareto-Guided Optimization of Agent Harnesses for Quality and Cost** (cs.AI/cs.CL/cs.LG/cs.MA;
  Qian, Park, Tan, Yu, Zhang, Cavallaro, Xia, Chen). Adds the second axis harness optimization was missing. Prior harness
  evolution optimizes response quality and treats cost as a constraint or an afterthought; applying the deployment
  preference during intermediate updates then **prematurely rejects candidates that are valuable as parents**. ParetoSaddler
  separates intermediate candidate promotion from final deployment selection: an optimizer updates a parent harness, the
  parent and candidate are compared over repeated executions, and **Bayesian bootstrap Pareto support** promotes a candidate
  that shows credible progress in *either* quality or cost; promoted candidates get broader development evaluation and stay
  available as parents. On ClawMark, quality **70.86% → 81.35%** while cost **$1.17 → $1.01** per task; on GAIA2, quality
  **56.22% → 62.89%** while cost **$0.78 → $0.45** per task; the Pareto-selected harnesses beat quality-only optimization on
  both axes. The analysis states the mechanism plainly: Pareto-aware promotion keeps intermediates the deployment preference
  would discard, and the bootstrap screen removes gains that are not supported across runs. Digital agents; author-reported.
  **Artifact status literal (2026-11-03): project page only; the linked code branch does not resolve.** The record's
  comment points at `aka.ms/ParetoSaddler-website`, which resolves (HTTP 200) to
  `autosaddler-projectpage.github.io/ParetoSaddler/` and carries the full result tables. That page's code link is
  `github.com/microsoft/AutoSaddler/tree/feat/paretosaddler`, which returns **HTTP 404 / "Branch not found"** — the
  repository's branch list is `main` plus `feat/activesaddler` and dependabot branches, with **no `feat/paretosaddler`**.
  The parent `microsoft/AutoSaddler` is real and open (**MIT**, 2.5 MB, 246 stars, pushed 2026-11-02) but is the
  AutoSaddler line, not a ParetoSaddler release. Nothing from this paper is counted as open.

- **`2611.03434` — Optimize the Harness, Not the Model: Fitting a Minimal Agent to a Self-Hostable LLM for Investigative
  Analysis** (cs.AI/cs.CL/cs.MA; Gefen, Chassida, Basin, Feigenblat). A three-page CIKM 2026 industry-day note that states
  the repository's own thesis as an operational constraint: when security and cost rule out hosted frontier models and
  broad tool surfaces, the thing you are allowed to optimize is **the harness, not the model**. **CLEO** (Constrained Loop
  for Environment Optimization) takes a fixed, self-hostable LLM and an expert-validated dataset and admits **only** the
  tools, guardrails, memory policies, and loop components that improve accuracy *while remaining general and auditable* —
  a minimality-and-auditability filter rather than a search for maximum score. Author-reported: **83.8%** pass rate on the
  held-out set, **within 1.2 points** of the strongest out-of-the-box Claude Code SDK baseline, at **two orders of
  magnitude** smaller. Digital investigative analysis; no robot experiment.
  **Artifact status literal (2026-11-03): none.** Three pages and one table; no code, prompt set, or configuration is
  released, and the only repositories named in the text are third-party components it was fitted against
  (`badlogic/pi-mono`, `karpathy/autoresearch`, `anthropics/claude-agent-sdk-typescript`). The entry is included for the
  objective function, not for an artifact.

- **`2611.02155` — Decision Models as Policy Gates for Coding Agents: Explicit Policy Boundaries Improve Detection and Reduce
  False Alarms** (cs.CL/cs.CR/cs.SE; James P. Beno). The clearest measurement yet of the runtime gate this repository has
  been tracking since JevAct: a **non-generating decision model** placed in front of every tool call, scored not by whether
  it catches bad actions but by whether it catches them **without stopping good ones**. A synthetic dataset of **2,643 tool
  calls** is labelled blind against **28 rules** with a sealed holdout, and **22 hosted decision models** plus two prompted
  general models serve as the classifier of a multi-rule policy engine. The intervention is a single change in the rule
  text: writing the policy boundary as **examples and counterexamples** rather than as prose. With one shared preamble in
  both conditions, explicit boundaries raised recall by **5.5–26.1 points on all 23 available models** and reduced false
  positives on **21**. Jev 1.14's recall rose **89.3% → 98.2%** while unnecessary interruptions fell from **30.6% to 4.8%**
  of 252 clean actions; its median latency with examples was **120 ms at $0.46 per thousand requests**. Strong models kept
  the recall gains when concrete names were replaced by generic descriptions, and three largely kept them when the same
  boundaries were rewritten as prose. On **16 new scenarios written after the initial studies**, a follow-up improved recall
  on **22 of 23** models and reduced interruptions on **21**. Digital coding agents; author-reported.
  **Verified public (2026-11-03): split licence, not a single SPDX one.** `agentapprove/decision-policy-rules` is public
  (7.7 MB, created 2026-11-01, pushed 2026-11-03) and holds `rounds/`, `results/`, `src/`, `tests/`, `CITATION.cff` and a
  `LICENSE` that grants **MIT for the code** (`rounds/`, `src/`, `tests/`, `paper/scripts/`) and **CC BY 4.0 for the
  datasets, rule texts and paper data**, while explicitly excluding the vendor's proprietary policy engine. Because the
  grant is split and prose-wrapped, the GitHub API reports the licence as **`NOASSERTION`**; the release itself is real.

- **`2611.02950` — Action Verification through Conjecture and Refutation for Language Model Agents** (cs.AI/cs.CL/cs.LG;
  Oh, Heo, Noh, Kim, Son, Ok). Inverts pre-execution verification. Existing action verifiers are **confirmation-oriented**:
  they ask what evidence supports the proposed action, which leaves the blind spot that *a wrong action usually passes every
  check that asks whether it is right*. **BlackSwan** instead runs an expect–suspect–test procedure: predict the intended
  effect of each state-mutating action, formulate **specific and empirically testable hypotheses for how it could be wrong**,
  test them against existing observations or additional **non-mutating** actions, and execute only if no hypothesis survives.
  On AppWorld it improves success over the strongest baseline by **14.9** and **25.2** points with and without the backbone
  model's thinking mode; it beats stronger baselines that have thinking and in-context demonstrations even without either;
  gains transfer to WebShop (**+12.7** points). Digital tool-use agents; author-reported.
  **Artifact status literal (2026-11-03): none located.** No first-party repository, weights or dataset appears on the
  record or in the full text; the only implementation named is the third-party `ace-agent/ace` baseline it runs.

### Agentic Robot and VLA Harnesses (5)

- **`2611.01112` — Auriga Technical Report: A Task-Agnostic Agent Harness for Zero-Shot Embodied Manipulation** (cs.RO/cs.AI;
  Wang, Shi, Xu, Wang, Zhao, Xia, Liao, Peng, et al.). The batch's most complete statement of what a robot harness owns, and
  it starts by measuring the gap it closes. On RoboDojo, **removing arm execution raises GPT-6 Astra's success rate from
  22.5% under the reference control interface to 76.2%** — the planning–execution gap is the entire problem. Auriga closes
  **68.3%** of that gap **zero-shot**, with no target-task trials and no resets: the model makes every task decision over an
  explicit **task belief** and acts through direct tool calls or **synthesized multi-step Python programs**, while the harness
  supplies calibrated 3D measurements, **finger-referenced dual-arm control**, and a **single admission gateway** to the
  robot. Two of the contracts are worth copying: before motion, the model inspects a **visual trajectory preview while
  simulation is paused**; during execution, the harness **checks declared stage tolerances and grasp bounds**, stops the robot
  on divergence, and returns the measured discrepancy for in-place recovery. Developed across **13 cross-task capability
  iterations and then frozen** for evaluation — the harness is the artifact, not the loop. Authors report first place on the
  RoboDojo leaderboard at **57.7%** success (64.0 score), more than doubling the reference harness on the same model, and
  first place under scene randomization while learned policies drop by up to **33.9%**; on RoboCasa365 the same frozen
  harness reaches **73.2%**, up to **23.1%** above VLA-assisted agents on composite tasks. Simulation only
  (RoboDojo, RoboCasa365); author-reported.
  **Verified open (2026-11-03): MIT code.** `lose4578/Auriga` is **MIT**, 47.3 MB, 23 stars, created and pushed 2026-10-31,
  with `docs/` (including the technical-report PDF and an `AGENTS.md` contributor guide), `environment.yml`, and a `web/`
  Node 22 front end; its 15 KB `README.md` describes the persistent Codex session, the run-state and evidence record the
  harness keeps, and the RoboDojo / dual-arm ARX X5 default environment. The project page `auriga.stelledge.com` is live
  and links the same repository. No weights or datasets are claimed.

- **`2611.01560` — Democratizing Robot-Use Agents: A Unified Harness with Open Models** (cs.RO; Yuan, Zuo, Huang, Zhang,
  Park, Yang, Zhang, Chen, et al.). Asks what the robot-use-agent harness is worth **without a frontier model inside it**.
  `uni-robot-agent` is a five-layer harness that connects different agent runtimes to one shared toolkit spanning simulation
  and physical robots, with no privileged inputs. Out of the box, **Qwen3.8-Flash-Next** inside it reaches a RoboDojo Score
  of **31.39**, above the published **GPT-6 Astra-RoboProbe 28.97** and above many VLA/WAM baselines, and performs
  competitively across **19 real-world tasks**. Two ablations carry the transferable content: performance is **comparable
  with the skill library removed**, so the harness is not smuggling in privileged task solutions or predefined subtask
  plans, and **pipelined execution** substantially improves completion time over serial execution while the configurations
  exclude task-verifier feedback. Across model generations, families and sizes, the authors link effective robot use to four
  general capabilities that they argue should be trained jointly. Simulation (RoboDojo) plus **19 real-world tasks**;
  author-reported.
  **Artifact status literal (2026-11-03): none located.** No repository, weights or dataset URL appears on the arXiv record
  or anywhere in the full text, and the record carries no comment. Despite the title, this run found no open release to
  point at; the entry is included because the open-weight comparison and the skill-library ablation are the measurements
  the "robot-use agent" line was missing.

- **`2611.03584` — TAMPAgent: Agentic Belief-Space Planning for Mobile Manipulation** (cs.RO; Mishra, Degirmenci, Chen, Xu,
  Hermans, Garrett — NVIDIA Research, Georgia Tech, Utah). Names the structural weakness of tool-calling robot agents: they
  **struggle with geometric uncertainty** and therefore work only in fully-observable tabletop settings, while
  task-and-motion planning handles partial observability but is expensive and needs a closed world. TAMPAgent grafts the
  two: an LVLM agent is given a **belief-space planning model over an extensible API of perception, planning and control
  tools**, formulates a **partial** task plan of parameterized observation and execution actions, hands the completion to
  classical search, then **extracts the plan's geometric constraints and binds valid parameter values through its tool API**
  before committing to motion. Execution updates the belief history and the loop interleaves observation with manipulation.
  Evaluated on household mobile manipulation on **Rainbow Y1 and Unitree G1**, the authors report it significantly
  outperforms direct tool-calling agentic baselines; no numeric summary appears in the abstract. Author-reported.
  **Artifact status literal (2026-11-03): project page only.** `umishra.me/TAMPAgent/` returns HTTP 200 with the author
  list and affiliations, but its own controls read **"Paper by request · arXiv soon · Code soon"** — the page is a
  placeholder for a release that has not happened, and no repository is linked.

- **`2611.00344` — SPARK: Self-Correcting Embodied Memory via Physical Re-Observation** (cs.RO/cs.AI/cs.CL/cs.LG; Kurdydyk,
  Palnitkar, Scherer, Kim — CMU). Treats the embodied knowledge base as something that **degrades and must be repaired**,
  rather than something that accumulates. Retrieval-augmented memories in embodied settings inherit three failure classes —
  wrong semantic labels, inaccurate object positions, inconsistent spatial relations — and append-only updating preserves
  them. SPARK runs a **semantic critic** that flags KB regions, a **targeted planner** that generates revisits, and a
  **selective merge** that adds, removes, or replaces entries from the new observation. On four outdoor AirSim maps under
  two degraded viewpoint conditions, with **158 EQA questions** scored before and after correction, a **single corrective
  episode** improves downstream EQA on **every map under both conditions** while storing **13–41% fewer objects** than
  append-only updating, with gains up to **2.7×** on one map-condition pair; correction through Episode 4 brings every map
  **above** the EQA performance of its undegraded reference. The ablations matter as much as the headline: all three critic
  signals contribute, **targeted revisits beat untargeted ones at the same budget**, and selective merging matches
  append-only gains with a smaller KB. Simulation only (AirSim); author-reported.
  **Artifact status literal (2026-11-03): none located.** No code, dataset, or project link on the record or in the full
  text.

- **`2611.00734` — MOST: Memory of Object States and Task Progress for Long-Horizon Mobile Manipulation** (cs.RO; He, Guo,
  Shen, Yu, Yang, Zhang, Han, Wang). Argues that long-horizon agents do not need to remember *more*, they need to remember
  **task-relevant state changes and progress**, and makes that an explicit supervision contract: multi-view observations are
  compressed into temporal tokens, and both historical and **predicted future** tokens are supervised with **object states
  and subtask progress**, so memory is structured around what changed in the world and what has already been accomplished.
  The memory conditions action generation inside a single end-to-end policy, with no external planner and no retrieval stage.
  The authors report **65.9% → 72.1%** average success on RoboCasa365, **20.83% → 42.22%** average maximum task progress on
  the new **RoboCasa-Long** benchmark, and real-world success **6.66% → 43.33%**, beating Xiaomi-Robotics-1 (XR-1) in all
  three, and beating a matched visual-history memory variant — the claim being that *what* is remembered matters, not how
  much. `RoboCasa-Long` is a new multi-minute, multi-stage mobile-manipulation benchmark. Real-robot numbers are
  author-reported.
  **Verified open (2026-11-03): Apache-2.0 code and public data.** `diqihe666/MOST-vla` is **Apache-2.0** with a `LICENSE`
  and `NOTICE`, `modeling_most.py`, `most/`, `configs/`, `deploy/`, `eval_robocasa365/`, `eval_robocasa_long/`, `tests/`
  and `tools/`, created 2026-10-31 and pushed 2026-11-03; the benchmark data is `diqihe666/robocasa_long` on Hugging Face,
  **public, not gated**, last modified 2026-10-31.

### Evaluation Integrity (3)

- **`2611.00283` — Does Validation Loss Predict Real-Robot Success? Evidence from Four Imitation-Learning Policies on a
  Low-Cost Arm** (cs.RO; Simon Cowling). Measures the offline signal the field uses for checkpoint selection against the only
  signal that matters. **ACT, Diffusion Policy, SmolVLA and π0.5** (action expert only) are trained on nested subsets of
  **12 to 200 demonstrations** of a pick-and-place task on an **SO-101**; **82 checkpoints** are scored on 25 held-out
  demonstrations and compared with **930 bench rollouts**, and all **700 intermediate checkpoints of 70 training runs** are
  scored too, with ball positions recovered from the front camera for every training, held-out and bench episode. Within a
  policy, hold-out action error does correlate with bench success (Spearman **−0.23 to −0.88**) — but **no more strongly than
  the number of training episodes alone**, and **not at all among seeds trained on the same data**. Across policies the
  ranking **inverts** (ρ = **+0.90** at 200 episodes): the policy with the lowest hold-out error succeeded in **3 of 110**
  rollouts. The dominant failure is invisible to an offline set by construction — the arm **never leaving its rest pose**,
  which ended **96%** of Diffusion Policy rollouts trained on untrimmed demonstrations and **47%** of SmolVLA's, because a
  hold-out set made of demonstrations cannot contain an initiation failure. Among rollouts where the arm actually moved,
  SmolVLA's hold-out action error predicts success well (ρ = **−0.81**). For the diffusion and flow-matching policies,
  hold-out **loss** rises two- to five-fold during training while hold-out **action error** falls by up to 29%. The
  recommendation is a reporting contract: **report initiation failure and success-among-started rollouts as separate bench
  outcomes**. Real robots; author-reported.
  **Artifact status literal (2026-11-03): companion data only.** The record's comment names
  `huggingface.co/datasets/fysical/greenball_pool_225_v1`, which the Hub API returns as **public, not gated, Apache-2.0**
  (last modified 2026-09-22); it belongs to the companion paper `2610.37170`, which the 2026-10-30 record screened, not to
  a new release here. No code is claimed.

- **`2611.00896` — Verifying Test-State Identity in Paired Evaluation of Vision-Language-Action Models** (cs.RO; Rafiqul
  Islam). Attacks the assumption underneath every paired VLA comparison: that **matching task, seed and episode identifiers
  prove two policies faced the same test conditions**. It does not, under specific vectorized reset and autoreset conditions,
  where different termination timing changes subsequent initial-state assignment, so the same episode index need not mean
  the same test state. The contribution is an **evaluation-integrity framework** — invariants **I1–I5**, provenance logging,
  optional scene fingerprints, and an automated checker. Its discipline is in what it refuses to claim: a controlled
  **LeRobot 0.6.1 LIBERO** harness study keeps **four evidence levels separate** — (A) mixed-horizon Sync wave-1 IDs
  `[20, 5, 22, 7]` vs `[4, 5, 6, 7]`; (B) a homogeneous separate-job Sync control where **16 cells produce 0 ID/fingerprint
  mismatches**; (C) Async+Freeze batch-size-map differences with **12 I3 mismatches**, bs=8 incomplete and **the mechanism
  unisolated**; and (D) a synthetic McNemar illustration only. Controllers were **emulated**, no real-VLA shared-vector
  experiment was completed, and the author explicitly **does not claim published VLA leaderboards are generally invalid**.
  Simulation harness study; author-reported.
  **Artifact status literal (2026-11-03): none claimed or located.** No checker, harness or log release is linked.

- **`2611.01250` — Shades of Success: Evaluating Manipulation Quality from Object-Centric Constraints** (cs.RO; Genua,
  Prajapati, Trivedi, Padir). Binary success cannot distinguish two executions that both "succeed" at different geometric
  precision, and the fix here is a **learned constraint reference rather than a hand-written rubric**. From a small set of
  oracle simulation demonstrations, **SPECTRA** represents object interactions as **progress-aligned relative poses**,
  identifies which relative-pose directions are **constrained** and how far they may vary, and scores an unseen execution by
  how far it leaves the admissible set, summarised as a Task Quality Index **Q ∈ [0, 1]**. Across seven simulated tasks it
  recovers constraints with **94%** accuracy from **30 demonstrations per task**; on rollouts from three learned policies its
  scores reach pooled Spearman **ρ = 0.81** with human judgments over all rollouts and **ρ = 0.61** among successful rollouts,
  exceeding all baselines in both subsets. The closing experiment uses Q to **select the highest-scoring demonstrations** for
  training a diffusion policy on hardware, with exploratory results showing higher success than random selection.
  Simulation-derived reference; author-reported.
  **Artifact status literal (2026-11-03): placeholder, not open.** `spectra-manip.github.io` returns HTTP 200 but the page
  is listed under **"Anonymous Authors"** and its own links read **"Paper (coming soon) · arXiv (coming soon) · Code (coming
  soon) · BibTeX (coming soon)"**. A coming-soon link is not a release; nothing is counted as open.

### Released Real-Robot Evaluation Data (1)

- **`2611.03168` — Rollout then Recycle: 3,718 Labelled Real-Robot Episodes from Benchmarking 7 Policies on 12 Tasks**
  (cs.RO; Selvaraj, Uttini, Kuosmanen; CoRL 2026 "Oops, I Erred" workshop, 4 pages). Releases the **failures** of a
  benchmark instead of its leaderboard, which is the scarce resource for anyone training a monitor, a reward model, or a
  recovery policy. The corpus is all **3,718 episodes** from the **ArmnetBench v0.1** evaluation (`arXiv:2607.24481`):
  **600 teleoperated demonstrations** and **3,118 rollouts** of **seven** imitation-learning and VLA policies on **12**
  tabletop tasks on low-cost **SO-101** arms, of which **73% are failures**. The structural property that makes it usable is
  a shared cell: every policy for a task was trained or fine-tuned on the **same 50 demonstrations** and rolled out on the
  **same** arm, so the corpus contains many policies' failures on identical tasks rather than one policy per scene. An
  operator labelled **every** rollout successful / suboptimal / failure; each episode carries **three camera views**, joint
  states and actions, in **LeRobot v3.0** and a reward-model format. The note is explicit about collection limits rather than
  overselling the data.
  **Verified open (2026-11-03): Apache-2.0 data.** The release resolves on Hugging Face as
  `armnet/armnetbench_v01_robometer` (also `_lerobot_so101` and `_lerobot_bimanual_so101`), **public and not gated**,
  **Apache-2.0**; the dataset card states the full release contains **3,718 labelled episodes — the 3,118 core benchmark
  episodes plus 600 from extra runs** — which matches the paper's counts exactly.

### Memory and Evidence Audits (2)

- **`2611.02056` — Do Embodied VLMs Actually Use Their Scene-Graph Memory?** (cs.RO/cs.AI/cs.CV; Garg, Pandey, Khandelwal,
  Rawal, Sheth). Supplies the missing measurement for every "memory-augmented embodied agent" claim: **accuracy cannot show
  that the graph holds the episode's facts, that it reaches the model, or that it is relied on.** The instrument is a
  **graph-swap test** — each episode is given *another* episode's graph, and the metric is **how many answers change**. On
  an embodied question-answering benchmark with routes through two to seven rooms, delivered as **text** the memory is used
  strongly, adding **30 points** on route questions; delivered as a **graph** it is not, and the analysis traces why — the
  encoder preserves the information but **standard pooled and late fusion discard it** — while giving each node its own input
  token recovers part of it (**+8.3 points** on route questions, confirmed by the swap test to come from the graph rather
  than the text). It also exposes a shortcut that accuracy hides: **when the same facts appear in the text, the model ignores
  its graph.** Author-reported; digital benchmark with an embodied VLM, no physical robot.
  **Artifact status literal (2026-11-03): none located.** No code, benchmark release, or dataset link appears on the record
  or in the full text.

- **`2611.01958` — Answering Without Arriving: An Evidence Audit for Embodied Question Answering** (cs.RO/cs.CV; Garg, Pandey,
  Rawal, Khandelwal, Sheth). The most uncomfortable number in this batch: a closed-loop vision-language agent **reaches no
  target in 200 episodes yet answers 45.5% of the questions**, within **0.1 points** of the same model given **no images at
  all**. The design is an **evaluation ladder that holds the questions fixed and varies only the evidence supplied to the
  answerer** — text-only probes, closed-loop agents, and replay of the reference trajectory — over 200 test episodes and 800
  questions stratified by route extent, with questions generated from **symbolic records of rooms, visible objects and
  frames** so that every answer can be traced to the observations that support it. Replaying the reference trajectory raises
  the same model to **57.4%**, so the evidence exists along the route but is **not gathered**. The family-level audit then
  separates *access to* evidence from *use of* it: four of eight families are largely answerable from text, one cites frames
  that cannot establish its own answer, and oracle conditions reach **59.5%** on Rooms-visited by counting the supplied
  images. The stated requirement — establish evidence acquisition, answer coverage and shortcut sensitivity *before*
  crediting an accuracy gain to memory — is the same contract this record applies to harness and recovery claims.
  Author-reported; simulation.
  **Artifact status literal (2026-11-03): claimed dataset does not resolve.** The full text points at
  `huggingface.co/datasets/Divyansh0108/Vroom-To-Room-200`; the page returns **HTTP 401** and the Hub API returns
  `{"error":"Invalid username or password."}`, the Hub's response for a repository that is not public. Nothing is counted
  as open.

### World-Model Auditing and Test-Time Selection (2)

- **`2611.01139` — World Models Need Not Be Right to Rank Policies Right: Relative Bounds and Audit Limits** (cs.LG/cs.AI;
  Ariq, Ahmad; NeurIPS 2026 "World Models in Physical AI" workshop poster). Separates three properties that model-based policy
  selection routinely conflates. An exact **pairwise simulation identity** and a **relative bound** split a shared model's
  error into **common-mode Bellman residual** and **policy-specific** error, which motivates **RAVEL** — a selective policy
  tournament whose eliminations use held-out *real* outcomes. Testing 45 SAC/TD3/PPO policies per task on DMC cheetah-run
  and walker-walk: at rollout horizon 500, **model rankings are strong** (Kendall τ **0.962** and **0.800**; same-run
  differential-error ratios 0.674 and 0.106), yet **mean paired audit-error correlations are 0.007 and 0.090**, pairing
  changes the standard deviation by **−2.5%** and **+1.9%**, and simultaneous intervals with **99.98% / 99.75%** coverage
  certify only **13.2% / 0%** of decisions at eight audits. RAVEL improves low-budget walker-walk regret, but real-only
  successive halving wins at the largest budget and the precommitted **twofold efficiency target is rejected**; optimizing
  against a frozen model lowers real return without raising predicted return. Conclusion: **ranking fidelity, useful audit
  covariance, and upward model exploitation are distinct properties.** Simulation; author-reported.
  **Artifact status literal (2026-11-03): none located.** No code or protocol release is linked on the record or in the text.

- **`2611.03585` — Seeing Through the Illusion: Test-Time Scaling for World Action Models** (cs.RO; Ahmed, Mostajabdaveh,
  Yazdani, Zhou; CoRL 2026 "From Generative Models to Robot Actions" workshop). The constructive counterpart to the
  test-time-search audit recorded on 2026-11-02, which found a learned value selector scoring **1.66 points worse than
  random**. It states the premise the earlier audit tested — "a plausible prediction need not be realized by its
  co-generated action" — and answers it with a **pre-execution selector on the sample distribution rather than on a learned
  value head**: **WISE** scores each sampled rollout on **action validity and temporal regularity**, on **inverse-dynamics
  consistency between the proposed action and the imagined motion**, and on **instruction-conditioned task progress**, then
  executes the highest-scoring candidate with the WAM **frozen**. With four candidates per decision it raises closed-loop
  success from **6.0% to 24.0%** on RoboLab with Cosmos 3 Edge and from **75.1% to 78.3%** mean success on LIBERO with
  DreamZero, exceeding the best existing selector by **4.0** and **0.9** points in aggregate; ablations show the three
  scores rank candidates differently and that **task progress carries the largest weight**. Simulation; author-reported.
  **Artifact status literal (2026-11-03): none located.** No first-party repository appears on the record or in the text;
  the published DreamZero checkpoint the experiments use is a third-party release.

### Runtime, Serving and Recovery (5)

- **`2611.03103` — VLALagBench: Understanding When and Why VLA Inference Latency Matters** (cs.RO; Wen, Xu, Amro, Tan, Lin,
  Ruan, Liu, Yang, Liu, Duan, He, Clemons, Kozyrakis, Jiang). Makes latency a **first-class evaluation axis** instead of a
  deployment footnote, across task types, object speeds, synchronous and asynchronous execution, and VLA policies. The
  headline finding is a cliff rather than a slope: **even a modest 100 ms of inference latency can nearly halve task success
  on moving targets**, and sensitivity varies substantially across tasks and policies, reflecting task-specific requirements
  for precision and continuous motion. Overlapping inference with action execution recovers some of the lost success, at the
  cost of **more frequent inference requests** — the execution-mode trade this repository's runtime entries keep rediscovering
  one system at a time. The authors then fit an empirical model predicting task success from limited calibration data
  (RMSE **5.7–7.8%** across VLA policies) and use it to choose algorithm configurations given a system's latency and
  per-robot throughput. The primary benchmark is **MuJoCo**, with a second simulator (**Isaac Sim**, DynamicVLA) and
  **real-robot** experiments carried in the appendices; author-reported.
  **Artifact status literal (2026-11-03): claimed code is 404.** The record's comment reads "Code will be released at
  `github.com/codestrongestx/VLALagBench`", which returns **HTTP 404**. The paper's own claim is not met, so nothing is
  counted as open.

- **`2611.01404` — Serving Agentic Robot Fleets: Workload Characterization, KV-Cache Thrashing, and Admission Control**
  (cs.RO/cs.DC; Sida He). Treats the agentic robot as a **two-layer serving workload with two clashing deadlines** — a
  vision-language brain that keeps revisiting long, growing KV caches, and a VLA that must deliver action chunks inside
  control deadlines — and measures the clash. Characterizing **80 LIBERO-PRO episodes** from GPT-6-Astra, RPent and π0.5
  (**1,862 brain turns, 1,582 VLA chunks**) on H800s and replaying the resulting agent programs on vLLM: brain contexts reach
  **215k tokens**, the **median Astra request takes 7.78 s** while the intervening environment-step gap is **1.0 s**. Under a
  **0.25 s** action-chunk deadline one VLA GPU meets the **1%** starvation target for one robot; under **1.0 s** its measured
  capacity reaches **at least 32**. In sustained brain replay, raising concurrency from **20 to 32 robots collapses throughput
  from ~2.4 to 0.6 requests/s** as repetitive prefill displaces generation — the *KV-cache thrashing* of the title — and a
  **static active-episode cap** restores **2.24–2.25 requests/s** and raises completed trace-program throughput from **1.30 to
  5.00–5.55 episodes/min**. Simulation replay of measured traces; author-reported.
  **Artifact status literal (2026-11-03): none.** No code, traces, or data are released; the agent harness whose traces were
  replayed is the third-party `RLinf/RPent`.

- **`2611.02101` — When Delay Meets Drift: Physical Safety of Robot Foundation Models Is a Cross-Layer Problem** (cs.RO/cs.CV/
  cs.ET; Liu, Thapa Magar). Replaces "robustness benchmark plus real-time method" with a **cross-layer variation
  propagation** account spanning the physical environment, the model, the computing platform and execution in closed loop,
  and an **interaction contrast** that measures when joint variation creates more risk than the sum of its parts. The
  measurement is stark: over **4,000 paired closed-loop episodes** of π0.5 on LIBERO, a **400 ms** inference delay raises the
  safety-violation rate by **3.5 points** under the training viewpoint but by **37 points** under a 60° camera shift —
  **33.5 points (95% CI 25–43) more than the two effects combined**. The paper is a measurement plus a research agenda
  ("predict when variation becomes risk, and adapt compute, power, and control accordingly") rather than a system.
  Simulation; author-reported.
  **Artifact status literal (2026-11-03): none.** No code, data, or project link on the record or in the text.

- **`2611.00524` — Reflex: Fast Robot Policy Serving by Reusing and Correcting Past Actions** (cs.RO/cs.AI; Lin, Sun, Nasser,
  Jin, Li, Srinivasa, Lai). A serving layer that is honest about a negative result first: **reusing intermediate denoising
  states gives little advantage over simply running fewer denoising steps from fresh noise**, and accelerating denoising
  leaves the expensive **policy backbone** — up to **70% of GPU time** — untouched. Reflex therefore caches **complete action
  chunks**, bypassing backbone and action head, and pays for the risk with **two-level correction**: at the action level it
  learns action-aware similarity from past policy decisions, assembles compatible chunks and applies lightweight **residual
  correction**; at the execution level it captures **task progress** and adaptively refreshes the policy to bound accumulated
  error. Without modifying the underlying policy it improves inference latency by **1.4×–11×** and throughput by **2.0×–2.2×**
  on representative workloads and **real robots**, at comparable task success. Real robots; author-reported.
  **Artifact status literal (2026-11-03): none located.** No first-party repository, cache format, or checkpoint is linked;
  the model and hardware repositories named in the text (openpi, GR00T, SO-ARM100) are third-party.

- **`2611.02555` — Sequential Recovery Updates for Learned Controllers under Successful-Trajectory Preservation** (cs.RO/cs.AI;
  Li, Liu, Zhao; submitted to ICRA 2027). Gives recovery the guarantee that the rest of this repository's recovery entries
  only approximate. Adding a recovery to a competent controller means "learning a correction, and learning when not to apply
  it", and in a router over frozen skills a new entry rule can **redirect states an old skill succeeded on**. The design
  therefore constrains **entry, not parameters**: each update searches for a feedback skill at a **measured failure**, fits an
  entry rule that **rejects every state on an accumulated successful trajectory**, and admits the skill only if the composed
  controller **reproduces the new recovery on its own** and **replays every protected trajectory exactly** — with a
  finite-trajectory condition stating when this preserves the previous controller. One mechanism runs on four streams (two
  motions on each of two humanoid systems); two updates per stream add **62 recoveries over 192 withheld perturbations** and
  **lose none** of the base controller's successes. It then prices the guarantee rather than hiding the cost: removing the
  constraint recovers **33 more conditions** but stops **eight protected ones** from completing at all, and excluding only
  what admission proves necessary recovers none of that. Simulation; author-reported.
  **Artifact status literal (2026-11-03): none located.** No code or data release is linked on the record or in the text.

## Revision handling (one entry changed)

- **No new record is a revision.** All **1,328** records new to this repository are `v1` with `<updated>` equal to
  `<published>`.
- **49 previously-known records moved version inside the window.** Three matter to the watch list, and none changed the
  artifact picture except one: `2610.39158` **VEvo-Harness** → v2 (2026-10-30), `2610.39711` **Sandman** → v2 (2026-10-31),
  `2610.39884` **KARMA** → v2 (2026-11-02). Their v2 renderings contain **no first-party repository or dataset link**, only
  third-party references (`MiniMax-H3`, `deepseek-harness`, `CubeSandbox`, `E2B`, `daytona`), so their watch status is
  unchanged. `2610.40016` **LOGIV** → v3 is a related-work/reference/affiliation update; `2610.41275` **TRACE** → v2 states
  in its comment that it changed **only affiliations, "No changes to the scientific content"**, so the 2026-11-02 deferral
  for the `TRACE` name collision with `2610.38400` stands unchanged.
- **One promotion: `2610.40026` RoboSeed.** The v2 comment now carries a project-repository link
  (`https://github.com/PrimitiveX/RoboSeed`) that the paper did not have on 2026-11-02, and the registry the 2026-10-30
  verification recorded as nine checkpoints "Pending release" now lists **17 packages**, with a changelog entry dated
  **2026-10-29** reading "Release all 17 model-weight packages, covering ABot-M0, LingBot-VLA 2.0, OpenWAM and LiLa-WAM".
  Every package resolves on **ModelScope** under the organisation **`InPeerReview/RoboSeed`** (the organisation page and a
  spot-checked package both return HTTP 200), and `data/checkpoints.json` plus `data/modelscope_packages.json` give a
  machine-readable registry alongside the reproduction tables. The repository still declares **no licence**, so it remains
  **public but not open source**; the README entry's artifact paragraph was corrected rather than the entry rewritten.
- **No `README` entry was re-worded for any other revision.**

## Rejected, deferred and watch list

- **On-scope but not taken this run.**
  `2611.00158` **Scaling Trusted Autonomy** (a single-author thesis that does assemble the three pieces this repository cares
  about — instruction grounding with explicit task constraints, an **editable** 3D Gaussian scene that is locally reconciled
  after predicted manipulation, and visuomotor skills that predict their own **symbolic effects** for progress monitoring and
  replanning — but a thesis with no artifact and no new measured system);
  `2611.00180` **Learning to Live / LIFE** (continuous observation, streaming gates, Plastic Memory that keeps source
  evidence, and Explore–Distill–Mount turning solved problems into reusable tools; genuinely on-theme, but the paper is
  explicit that the 24-hour real-robot study is "a case study of limited scale" and no artifact is linked, so memory coverage
  went to three entries with measured claims instead);
  `2611.00634` **PREM** (action-conditioned future prediction regulates memory updates at each spatial location;
  14.4% → 83.8% on RMBench M(1) and 7.7 ms inference overhead — a model contribution excluded under the curation rule, but
  its *update* contract is the most transferable part);
  `2611.02950`'s neighbours in verification are covered, so `2611.01620` **Chain-of-Thought Oversight** (an evidential-chain
  reporting standard that labels each claim's required link supported / unsupported / assumed — a good publishing contract,
  no artifact, and its subject is interpretability evidence rather than runtime state);
  `2611.03227` **What Structurally Breaks an Agent** (an executable audit instrument over dependency depth, candidate count,
  state width and snapshot access: 49,152 tasks and 393,216 reference-controller rollouts, plus 295,936 in a frozen follow-on;
  careful about its own limits, including a "descriptive only" 16-episode study — deferred because the harness-relevant
  finding, that hidden state resampling imposes an exact success ceiling no amount of state tracking can overcome, is
  already carried by the 2026-10-28 durable-state entries);
  `2611.00884` **When 200 OK Is Not an Answer** (20.9% of static-client HTTP-200 fetches contain no main content; treating 200
  as content inflates an apparent silent-citation-drift rate from 5.8% to 21.9%; the web-agent analogue of this repository's
  "receipt, not report" line, but digital-only, and the release is announced, not yet resolvable);
  `2611.02218` **Beyond Ensemble Disagreement** (predictive STL robustness ranks future locomotion violations while ensemble
  disagreement does **not** track prediction error or violations, and current-state robustness comes within 0.03 AUROC of the
  predictive version — a useful negative result, but simulation-only with no artifact and a single-sentence role in the
  monitoring story already carried by the runtime entries);
  `2611.01815` **When Does Latent Prediction Error Support Physical Surprise Claims** (impossible events can produce **lower**
  prediction error than possible ones; a fixed threshold mislabels many possible events — the third world-model audit entry
  this run, and two is enough);
  `2611.00407` **Hacking or Just Failing** (monitor capability, not prompting, separates reward hacking from honest failure;
  pre-registered and logged, but digital-only and one more monitor entry than this batch's argument needs);
  `2611.01976` **The Tool Update Gap**, `2611.01953` **Step-Zero Attack**, `2611.03555` **MCP Tool Poisoning** and
  `2611.02815` **Predictive Safety Guardrails Do Not Always Make LLM Agents Safer** (a coherent security cluster for digital
  agents, none with a robot experiment and none with a located artifact; carried to the watch list rather than added);
  `2611.02332` **LEANGUARD** (Lean-verified guardrails for tool-using agents — interesting, no artifact located);
  `2611.01386` **Learning by Doing** (autonomously compiling skills from verified traces), `2611.00544` **Skill++**,
  `2611.02797` **SelfImprove-Bench**, `2611.01736` **RSI-Env**, `2611.01834` **LegoEnv**, `2611.01840` **Self-Evolving
  Agentic Workflows**, `2611.00463` **Exo** and `2611.00452` **Compositional Self-Improvement** (the self-improvement line is
  already the most densely covered family in this list; none of these changed the shape of the story this run).
- **Model contributions, excluded because this repository does not treat a model paper as a harness paper** — the explicit
  curation rule, applied here to a very strong model batch. Representative: `2611.01011` Leap-VLA (high-rate control from a
  low-rate VLM backbone), `2611.01215` JADE (VLM and action expert as joint denoisers), `2611.02777` (distilling RL
  specialists into VLA policies), `2611.02600` W2A, `2611.01729` ODE Policy, `2611.02930` PointPilot, `2611.02474` Gnomon4D,
  `2611.01844` OpenNWM, `2611.03648` Pixel and `2611.03647` Pixel-UMI, `2611.02660` HeLM, `2611.00602` (SAE steering of π0.5),
  `2611.00522` (behavioural steering of VLAs), `2611.00394` CalVLA, `2611.01713` (Route-Guided CBF-QP repair — a post-hoc
  safety module rather than a runtime contract), `2611.02871` ReGATE, `2611.01659` HuG, `2611.03163` FineCraft,
  `2611.00955`, `2611.02171` GoalDex, `2611.02677`, and `2611.01814`/`2611.03307` control-barrier-function planners.
- **Benchmark and platform releases deferred to a later screen rather than skipped on merit.**
  `2611.02629` **DexArena** (unified long-horizon dexterous-manipulation benchmark), `2611.02497` **RoboColosseum**
  (physical-intelligence evaluation "in-the-wild"), `2611.01934` **TriPhysBench** (physical intelligence for embodied agents),
  `2611.03138` **KlaskTron** and `2611.03091` **KlaskTron-IL** (an open-source physical adversarial multi-agent RL platform
  with a validated sim-to-real bridge), `2611.00455` **TacMind**, `2611.01190` **ClothDojo**, `2611.00957` (tactile
  foundation-model evaluation), and `2611.03646` **EgoActionBench**. Each is a genuine artifact; none of them was verified to
  the link level in this run, so none is asserted as open here.
- **Off-scope remainder (representative, from the 1,328).** SLAM, calibration and 3D reconstruction (`2611.03106`,
  `2611.02884`, `2611.02999`, `2611.01627`, `2611.01645`); motion planning, control and swarm geometry (`2611.00324`,
  `2611.01038`, `2611.01220`, `2611.01442`, `2611.03016`, `2611.02189`, `2611.03307`); legged/humanoid hardware and tactile
  sensor design (`2611.00193`, `2611.01748`, `2611.02140`, `2611.01609`, `2611.03520`, `2611.00879`, `2611.01277`);
  safety filters and CBF navigation; autonomous-driving world models and VLAs (`2611.01677`, `2611.01663`, `2611.02526`,
  `2611.01996`, `2611.00718`, `2611.02240`); ornithopters, UAV swarms and ISAC; on-orbit and space robotics (`2611.02922`);
  medical imaging, surgical agents and clinical LLM pipelines; and the large LLM remainder (post-training, quantization,
  KV-cache, speculative decoding, serving, RLVR, unlearning, RAG evaluation, speech, and the whole of the batch's pure
  `cs.CL`/`cs.LG` methodology).
- **Watch list carried forward, not re-checked at link level this run** (no new version and no visible release activity in
  the harvest): **ReSTEP** dataset and code, **EvoLife** ("Code is coming soon"), **Sandman** (v2, no artifact), **LOGIV**
  (v3, no artifact), **AgentHeal** (public, unlicensed), **METATRON** and **ELEVATE** (project page and demos only),
  **Scan Before You Install** ("upon acceptance"), **SkillSense** (`anonymous.4open` 401), **VEvo-Harness** (v2, no
  artifact), **KARMA** (v2, no artifact), **PASA-KV**, **Soei**, **Assurance Stack** agenda, **RoboSeed** weights (**now
  promoted — see revision handling**), and still nothing-located **Masked Erosion**, **Prefix Executability**, **REALM**,
  **JevAct**, **PrimaCons**, **Minimum Sufficient Communication**. **New watch-list entries from this run:** `2611.02079`
  ParetoSaddler (linked branch 404), `2611.03103` VLALagBench (claimed code 404), `2611.01560` uni-robot-agent (no release),
  `2611.03584` TAMPAgent ("Code soon"), `2611.01250` SPECTRA ("coming soon"), `2611.01958` (dataset 401), `2611.00344` SPARK,
  `2611.01139` RAVEL, `2611.02950` BlackSwan, `2611.02555`, `2611.00524` Reflex, `2611.01404` and `2611.02101` (none
  located), and `2611.03434` CLEO (none claimed).

## Validation performed

- **Markdown structure.** 22 new single-line list items appended under eight existing `###` headings (no heading added,
  removed or re-levelled); 4 new bullets added under `## Current Landscape` in the same `- **claim.** [entry] …` shape as
  their neighbours; one artifact paragraph **replaced in place** inside the existing RoboSeed entry; no table, HTML element
  or list nesting introduced or modified.
- **`git diff --check`.** Clean — no whitespace errors, no conflict markers, no trailing whitespace.
- **Links.** Every arXiv `abs` URL added was fetched during verification (HTTP 200) and used for the title, category list,
  comment and author list quoted in this record. Every non-arXiv link added to `README.md` was resolved at the source this
  run: `auriga.stelledge.com` (200), `github.com/lose4578/Auriga` (200), `umishra.me/TAMPAgent/` (200),
  `spectra-manip.github.io` (200, placeholder), `aka.ms/ParetoSaddler-website` → `autosaddler-projectpage.github.io` (200),
  `github.com/microsoft/AutoSaddler` (200), `github.com/agentapprove/decision-policy-rules` (200),
  `github.com/diqihe666/MOST-vla` (200), `huggingface.co/datasets/diqihe666/robocasa_long` (public, ungated),
  `huggingface.co/datasets/armnet/armnetbench_v01_robometer` (public, ungated), and
  `www.modelscope.cn/models/InPeerReview/RoboSeed` (200). The **three URLs deliberately still cited as broken** are
  `github.com/codestrongestx/VLALagBench` (404), `github.com/microsoft/AutoSaddler/tree/feat/paretosaddler` (404) and
  `huggingface.co/datasets/Divyansh0108/Vroom-To-Room-200` (401), because in each case the entry's point is that the claimed
  release does not resolve.
- **Duplicate IDs.** All 22 new IDs were checked against `README.md`, `docs/*.md` and `sources/*.md` before insertion
  (**0 of 1,328** batch records were previously named). After insertion each new ID occurs exactly once in the main list and
  at most once in a `## Current Landscape` bullet, which is this repository's established pattern. `2610.40026` was already
  listed and was **updated, not re-added**.
- **Dates and categories.** Every entry states its arXiv ID and its `<published>` date falls inside the harvested window
  (2026-10-29 → 2026-11-02); none came from a 2026-11-03 batch, because none exists. Category placement follows each paper's
  contribution rather than its arXiv primary: `2611.00734` (cs.RO, a policy-memory model) and `2611.00344` (cs.RO, an
  embodied RAG loop) sit under robot-harness entries, `2611.02079` (cs.AI) and `2611.03434` (cs.AI) under general harness
  design, and `2611.01139` (cs.LG) under simulation and world-model fidelity.
- **Cross-file consistency.** The README badge and the `## Current Landscape` `Last verified:` line now read **2026-11-03**,
  matching this record's filename and the newest entry's verification date. The 2026-11-02 record and earlier history are
  unmodified. `docs/landscape.md` keeps its own 2026-08-25 verification date because it is a historical note, not a
  maintenance surface.
- **Artifact-status discipline.** Six distinct literal statuses are recorded and each is stated in the entry itself:
  **verified open** (Auriga MIT code; MOST Apache-2.0 code + ungated data; decision-policy-rules split MIT/CC-BY grant;
  ArmnetBench Apache-2.0 data), **public but unlicensed** (RoboSeed — corrected, still not open source), **project page
  only** (TAMPAgent, ParetoSaddler's resolved branch), **placeholder / coming soon** (SPECTRA), **claimed link does not
  resolve** (VLALagBench 404, ParetoSaddler branch 404, Vroom-To-Room-200 401), and **none claimed or none located**
  (CLEO, uni-robot-agent, BlackSwan, SPARK, RAVEL, WISE, VLALagBench's paper itself aside, Serving Robot Fleets,
  When Delay Meets Drift, Reflex, Sequential Recovery Updates, the two evidence audits, Validation Loss, Test-State
  Identity). No entry describes an unverified artifact as open source.
- **Author-versus-independent separation.** Every number in all 22 entries is marked author-reported; no paper in this batch
  has an independent reproduction. Simulation-only evidence is stated as such for `2611.01112`, `2611.00344`, `2611.01139`,
  `2611.03585`, `2611.02101`, `2611.01404`, `2611.01958`, `2611.01250`, `2611.00896` and `2611.02555`; simulation-primary
  with a second simulator and real-robot appendix for `2611.03103`; digital-agent-only for `2611.02079`, `2611.03434`,
  `2611.02155` and `2611.02950`; and real-robot evidence
  for `2611.00283`, `2611.00524`, `2611.00734`, `2611.03168` and `2611.01560` (19 tasks) is author-reported.

## Operational notes and blockers

- **A stale frontier probe and a correct date query disagree, and the date query wins.** The previous run read "the frontier
  had not moved from `2610.40646`" as "no new submissions", and three consecutive re-screens of 815 unchanged records found
  2 then 0 qualifying papers. The withheld block behind that stall was **not above** the watched ID — it was in the next
  month's numbering (`2611.00120`–`2611.00473`) carrying October timestamps. **ID prefix is not a clock**; the
  `submittedDate` range is the only field that reliably answers "what was submitted".
- **The 125 withheld records account for 9 of the 22 inclusions**, including the two strongest harness papers in the batch
  (Auriga, uni-robot-agent) and the only fully open robot-harness code release. A run that had trusted the ID frontier would
  have missed all nine.
- **Three days in one interval is 3.6× the previous record's batch and it shows in the shape of the result.** The batch does
  not contain fourteen variants of one idea: it contains four coherent themes — deadline-respecting serving, harness-as-object
  optimization, evaluation-integrity accounting, and memory/evidence audits — each of which needed its own verification pass.
- **A `Code:` comment is still only a claim, and a project page is not a repository.** Three of the batch's most confident
  artifact statements failed on resolution: a comment promising code at a URL that 404s (VLALagBench), a project site whose
  only code link is a branch that does not exist (ParetoSaddler), and a "Code (coming soon) · arXiv (coming soon)" card under
  "Anonymous Authors" for a paper that is already on arXiv (SPECTRA). Conversely the two largest real releases (Auriga, MOST)
  advertise their artifacts only inside abstract prose or a README, not in the comment field.
- **`/tmp` is still non-durable across tool calls**, confirmed again; `.scratch/arxiv-2026-11-03/` (untracked, never
  committed) holds every response, the five day harvests, the 18 full-text renderings, the GitHub/Hugging Face/ModelScope
  JSON, and the screening scripts.
- **OpenSSH workaround still required.** `git ls-remote`/`fetch`/`push` fail with a "Bad owner or permissions on
  /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf" error; `GIT_SSH_COMMAND='ssh -F /dev/null'` restores access with the
  user's key and `known_hosts` unaffected.
- **Still owed: 2026-11-03.** The next run must query it by `submittedDate` again. The 2026-10-29, 2026-10-30, 2026-10-31 and
  2026-11-01 batches are now closed and should **not** be re-screened wholesale; the 2026-11-02 batch is likewise closed at
  its frontier `2611.03648` (2026-11-02T18:59:32Z).
