# Daily arXiv scan — 2026-11-14 (the freeze holds for a third day: 2026-11-13 and 2026-11-14 both empty, the frontier unmoved, and a second screen of the unchanged 725-record batch recovers five entries — a gated self-improver priced against native harnesses, robot software as an evaluation surface, the judge/teacher split, recovery that names its cause, and a simulator offered to agents as tools)

## Scope

- **Interval owed.** The previous successful run committed at **2026-11-13** (`474e0c7`) and produced
  `sources/daily-arxiv-2026-11-13.md`. That record closed with an explicit debt: **"Query 2026-11-13 and
  2026-11-14 … and re-harvest 2026-11-09 through 2026-11-12 fully, since 2026-11-10 gained 70 records after
  it had already been screened once and 2026-11-09 gained 4."**
- **This run** started **2026-11-14 17:52 CET** (**16:52 UTC**) on `main` at `474e0c7`, with `origin/main`
  also at `474e0c7`. The interval owed is submission dates **2026-11-13 → 2026-11-14 inclusive**, plus the
  re-harvest and diff of **2026-11-09 → 2026-11-12**.
- **Headline: the index did not move at all.** 2026-11-13 returns **0** — it returned 0 at 14:22 UTC
  mid-day on Friday, so it was owed rather than closed — and 2026-11-14 also returns **0**. The newest id
  visible anywhere in the index is still **`2611.17330` at 2026-11-12T18:59:49Z**, the same frontier the
  previous run recorded **26 hours earlier**. Re-harvesting 2026-11-09 → 2026-11-12 and diffing it against
  the previous run's XML gives **0 added, 0 removed, 0 changed** over ids and six metadata fields, so the
  unchanged counts (**568 / 449 / 383 / 342**) conceal no late record and no revision.
- **Five entries, all recovered by re-screening, not by new arrivals.** Because nothing new arrived, the
  whole unchanged batch (2026-11-11 and 2026-11-12, **725** records) was screened a second time — titles in
  full, then **42** abstracts, then **9** full-text renderings — and yielded five entries that the previous
  pass had not carried: a self-improving harness with a **validation gate** measured against the models'
  own **native harnesses**, an evaluation surface that is **robot software**, a decision model's split into
  **actor / judge / teacher**, a robot **recovery** layer that names the *cause* of the deviation, and a
  multi-material **simulator** that exposes itself to coding agents over MCP.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`, one parenthesised OR-query per day:
  `(cat:cs.RO OR cat:cs.AI OR cat:cs.CL OR cat:cs.CV OR cat:cs.LG) AND
  submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]`. **22 export-API requests, every one of them 200 on the
  first attempt** — **one** connectivity probe, **six** day counts (2026-11-09 → 11-14), **six** full-day
  harvests at `max_results=2000&sortBy=submittedDate&sortOrder=ascending`, **two** descending frontier probes
  (`cs.RO`, `cs.AI`) and **one** header probe. No `429` and no `503`: unlike the previous run, which was
  penalised after an initial burst, every range query here was spaced **8 s** from the start. The four
  repeated requests each on 2026-11-13 and 2026-11-14 are the harvest loop's own condition (`entries > 0`)
  re-asking an empty day, not retries on failure.
- **No request returned the API's internal-error entry.** `grep -c 'api/errors'` on all six counts and all
  six day harvests: **0** everywhere.
- **Full-day harvests return exactly their counted totals**, so truncation is excluded: 2026-11-09 **568**
  (1.40 MB), 11-10 **449** (1.11 MB), 11-11 **383** (0.96 MB), 11-12 **342** (0.86 MB), 11-13 **0**, 11-14
  **0** — **1,742** entries re-harvested, identical in size to the previous run's files.
- **Differential diff against the previous harvest, as a parsed set difference.** The previous run's
  `day-20261109.xml` … `day-20261112.xml` (from `.scratch/arxiv-2026-11-13/`) were re-parsed and compared over
  bare numeric ids and over `title`, `published`, `updated`, `summary`, `comment` and `journal_ref`. Result on
  all four days: **0 added, 0 removed, 0 changed** — the strongest form of "nothing is hiding here", since it
  is a field-level comparison and not a count.
- **The API is serving origin, not a stale cache.** The frontier probe returned `age: 0` and
  `x-cache: MISS, MISS, MISS` with `date: Sat, 14 Nov 2026 16:58:30 GMT`, so a frozen index could not be
  mistaken for a cached page. An **independent surface** agrees: `https://arxiv.org/list/cs.RO/recent`
  headlines the same batch — **"Fri, 13 Nov 2026 (showing first 50 of 106 entries)"** — and its newest id is
  again **`2611.17330`**. Two surfaces, one answer.
- **Screening (second pass over the unchanged batch), in this order.** (a) All **725** titles of the
  2026-11-11 and 2026-11-12 records read in full, in ascending id order; (b) **42** abstracts read in full for
  every record carrying a harness, agent-runtime, self-improvement, skill-library, decision-model, memory,
  evaluation, recovery, simulator or robot-execution signal; (c) **9** full-text HTML renderings
  (`arxiv.org/html/<id>v1`, all returned article bodies) read for artifact links, evidence scope and the
  sentences the abstracts omit; (d) **8** arXiv abstract pages fetched to catch artifact links the export
  feed does not carry.
- **Artifact resolution at the source.** **13** `api.github.com` calls (repository metadata and contents
  listings), **8** Hugging Face Hub API calls (dataset metadata, trees, dataset-card README), **3** project
  pages, **2** Zenodo DOI resolutions and **1** arXiv listing page — HTTP status *and* metadata *and*, where
  emptiness mattered, a contents listing.
- **Duplicate check before insertion.** All five ids were grepped against `README.md`, `docs/*.md`,
  `sources/*.md` and `CONTRIBUTING.md` first; none appeared in an inclusion, an exclusion or a watch list in
  any earlier record.

## What the index did

| query (one parenthesised OR-query per day) | 11-09 | 11-10 | 11-11 | 11-12 | 11-13 | 11-14 |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| count at 2026-11-13 14:2x UTC (previous run) | 568 | 449 | 383 | 342 | **0** | — |
| count at 2026-11-14 16:57 UTC (this run) | **568** | **449** | **383** | **342** | **0** | **0** |
| **new to this run** | **0** | **0** | **0** | **0** | — | — |

- **A third consecutive zero day is now the base case, not an anomaly, and the ledger of states is
  complete.** A zero can mean *not yet announced*, *unindexed*, or *announcement stalled*; yesterday's record
  showed a hold resolving into two full batches, and this run shows the other stable outcome — Friday and
  Saturday submissions sitting behind an unmoved frontier while the already-published days neither grow nor
  revise. The practical rule for the next agent is unchanged and now has three consecutive days of evidence
  behind it: **a zero is never closed until the batch appears, and a day whose count did not change is still
  worth re-screening once**, because that is where all five of today's entries came from.
- **Stasis is a real measurement here because the previous run also measured motion.** The same descending
  frontier probe that recorded `2611.17330` at 2026-11-12T18:59:49Z yesterday now records the identical id
  and timestamp; `cs.AI` likewise holds at `2611.17323` / `18:59:06Z`. Two probes 26 hours apart, same answer.
- **The re-screen was not a formality: 5 of 5 entries came from it, and four of them sat outside the top 50
  of the keyword pass.** Scored over title, abstract and comments with this repository's usual term set, the
  five included records rank **3rd** (`2611.14949`), **59th** (`2611.15651`), **108th** (`2611.16116`),
  **172nd** (`2611.16312`) and **198th** (`2611.17167`) of **725**. A rank-first triage would have discarded
  four of the five: RSI-PDE's deciding property (baselines are the models' **native harnesses**) is in the
  abstract, Robo-SWE's (a **1,567-PR provenance ledger**) and Alloy's (**MCP tools**, health grades) are in
  the body, and "Recover, Don't Just Continue" never uses the word *harness* at all. The lesson is the same
  one the previous two runs recorded and it deserves restating: conclusions come from **reading the batch**,
  and the keyword pass is a reading-order heuristic, not a filter.

## Included entries (5, one per subsection)

### General harness methodology

- **`2611.14949` — RSI-PDE / RSI-PDEbench: A Benchmark for Self-Improving Agents in PDE Solving** (`cs.AI`,
  published 2026-11-11T02:55:14Z). The gate is placed on the **update**, not on the candidate: shortlist by
  residual loss, refine coefficients and numerical-solver configuration from relative **L²** feedback, and
  admit **policy and memory** updates only through a validation gate. The benchmark is **1,001** cases over
  **42** equation templates (classical and fractional) in three settings — **Full** (noise-free), **Noisy**
  (Gaussian noise at 50% of the clean-signal RMS) and **Hard-OOD** (unseen templates, shifted coefficient
  ranges) — so the loop is scored where its feedback channel is corrupted. Across **four frontier models** the
  authors report pass-rate gains of **106.5–181.9%** over single-run baselines and **3.0–19.6%** over the
  models' own **native harnesses** (**Claude Code**, **Codex**), with **46.9%** fewer recorded model calls
  across the **12** model–setting pairs. That last comparison is why the entry is here: it is a
  harness-versus-harness measurement, not a model-versus-model one. **Evidence scope:** scientific computing
  only, no robot experiment, author-reported, no independent reproduction. **Artifact status literal
  (2026-11-14):** the project page `jeremylu916.github.io/RSI-PDE/` is live (**200**), but the code repository
  it links — `jeremylu916/RSI-PDE` — returns **404** on both the web URL and `api.github.com`, and that
  account's **five** public repositories do not contain it; the linked dataset `JeremyLu1017/PDE-RSI` answers
  **401** on the Hub (private rather than merely unpopulated), while an unrelated public dataset answered
  **200** in the same session, so this is a property of that record and not of the client. **Neither the
  harness nor the benchmark is obtainable today.**
- **`2611.17167` — Actor, Judge, or Teacher? Understanding Decision Models in Long-Horizon Agents** (`cs.AI`,
  published 2026-11-12T17:42:46Z). Splits one model's job into three — **choose** an action, **evaluate** an
  action already taken, **score every action to teach a policy** — and asks whether a poor actor can be a good
  teacher. Three decision models and three general LLMs as actors on **ALFWorld** are all pulled off a
  must-come-first subgoal by an easier one; the same models, as judges seeing the whole trajectory, credit the
  steps toward it, and asking "*did this action help?*" instead of "*will the task succeed?*" is what
  separates useful actions from useless ones; as a teacher, **JEV** lifts a Qwen2.5-1.5B policy over GRPO from
  **72.8% → 82.9%** on ALFWorld and **56.8% → 71.4%** on WebShop. The harness reading is that the runtime's
  gate and its credit assigner need not be the acting model. **Evidence scope:** digital agents only, no robot
  experiment, author-reported. **Verified open (2026-11-14):** Apache-2.0 `ZJU-REAL/decision-agents` (47 MB,
  created 2026-09-23, pushed 2026-11-14) with `agent_system/`, `decision_model/`, `gigpo/`, `verl/`,
  `examples/`, `tests/` and `docs/`; the project page `zjureal.com/decision-agents/` returns **200**.

### Agentic robot/VLA harness — recovery

- **`2611.15651` — Recover, Don't Just Continue: Learning from Feedback for Physical Failure Understanding
  and Correction** (`cs.RO`, published 2026-11-11T16:01:28Z). Recovery is made to **name the cause** before it
  revises: failure data is *constructed* by injecting perturbations at **subtask boundaries of successful
  trajectories**, replaying the affected subtask, and keeping only failures that are observable from public
  evidence and recoverable from the current state; each kept episode is aligned as **Judge → Cause → Sub →
  Fine** (what deviated, why, what is still realizable, the corrected plan), trained **evaluation-first** so
  Judge and Cause precede Sub and Fine on accurate prefixes, then distilled on-policy from accurate-prefix
  plans into self-generated ones with that branch removed at deployment. Reported: **+6.1** points composite
  task success with a **frozen VLA**, **+13.9** points with fixed **GPT-5.5** execution across manipulation and
  navigation. **Evidence scope:** simulation only (LIBERO-PRO, RoboCasa365, EBAlfEnv adapters), author-reported.
  **Artifact status literal (2026-11-14):** `pm1255/embodied-corrector` is public (477 MB, created 2026-10-23,
  pushed 2026-11-13) with the SFT/OPD pipeline, benchmark adapters, `guides/`, `validation/` and a small
  EmbodiedDiag-Bench dataset — but it declares **no licence**, and it ships **no trained corrective
  checkpoint**: the base is the separate **RynnBrain-8B** release and the corrector is produced by running the
  repository. Its 12 recovery recordings are labelled by the repository itself as **GPT-authored,
  simulator-state-assisted demonstrations**, explicitly separate from the trained model's scores.

### Evaluation

- **`2611.16116` — Robo-SWE: An Executable Issue-Resolution Dataset Mined from Robot Software Repositories**
  (`cs.SE`, published 2026-11-11T23:51:27Z). An evaluation surface rather than a model: **236** executable
  issue-resolution tasks mined from merged pull requests in **28** public robotics repositories across four
  domains — ROS 2 systems, simulation and physics engines, robotics algorithm libraries, robot-learning
  frameworks — each with issue text, a container recipe at the pre-fix commit, the developers' fix, and tests
  that fail before and pass after in repeated offline runs. The release keeps the **provenance ledger of all
  1,567 candidate pull requests** with the rejection reason at every stage, plus **3,540** graded attempts by
  five coding-agent setups with patches and **3,529** trajectories. Reported resolve rates over three cycles:
  **43.9%** (Kimi K3.1 Pro) to **56.5%** (Opus 5.5), with GPT-6.1 Sol **54.1%** / **52.5%** (max / medium
  effort), GPT-6 Astra **51.4%**, GLM-5.3 **46.2%**, Claude Sonnet 5.5 **45.8%**. The validity claim is
  correctly scoped by the authors — verified **by execution**, with human fairness review stated as **future
  work**. **Artifact verified open (2026-11-14):** the Hub dataset `vharatian/Robo-SWE` is public and populated
  (`data/test.parquet` 1.2 MB, `data/test.jsonl` 5.7 MB, plus `provenance/`, `results/`, `tools/`, `harbor/`,
  `licenses/`) and its card carries the **leaderboard** the abstract promises; the licence is per-component
  (`other` / `robo-swe-component-licenses`), not one SPDX grant over the set.

### Simulation and digital twins

- **`2611.16312` — Alloy: A Multi-Material Physics Engine for Robot Learning** (`cs.RO`, published
  2026-11-12T05:14:10Z). Rigid robots, cloth, cables, liquids, granular material and soft bodies in one scene,
  each object routed to the solver suited to its material (MuJoCo Warp for rigid, Newton VBD for cloth and soft
  bodies, MPM for sand and pastes, Alloy's own DFSPH for liquids), coupled inside every control step and run in
  parallel worlds on one GPU (thousands for free rigid bodies, **512–4,096** for an arm on one L4, tens to
  hundreds with cloth or liquid) or on the CPU. Two properties make it a harness entry: every world carries a
  **health grade**, so a physics failure can be distinguished from a policy failure, and the engine is written
  to be **driven by coding agents** — docs inside the package, every error naming its fix, and the engine
  exposed as tools over the **Model Context Protocol**. In a cold test the authors report agents that had never
  seen Alloy installing a pre-release from the package index and training walking, cable-handling and pouring
  policies on a single GPU. **Evidence scope:** author-reported, no independent reproduction. **Verified open
  (2026-11-14):** Apache-2.0 `argonrobotics-ai/alloy` (33 MB, created 2026-11-11, pushed 2026-11-13) with
  `alloy/`, `bench/`, `CHANGELOG.md` and an `AGENTS.md`; `pip install alloy-engine` and the docs site both
  return **200**.

## Rejected and watch-listed

Everything below was read far enough to reject it on this repository's terms. **Carried-forward exclusions from
2026-11-13 stand mechanically, not by assumption:** the field-level diff over 2026-11-09 → 2026-11-12 is **0
changed**, so every watch-list verdict recorded there still describes the same metadata it described then.

- **`2611.15178` — HyperSkill** (`cs.AI`). Training-free omnimodal GRPO that swaps gradients for **auditable
  edits to a hypergraph-indexed skill library** (advantage-bearing nodes, composition hyperedges,
  counterfactually checked admission, a protocol skill learned from failures), best-or-tied on **11** text,
  vision-language and omnimodal benchmarks for **+13.0** average F1 with the backbone frozen; MIT code is live
  at `LHRLAB/HyperSkill` (7 MB, created 2026-03-17, pushed 2026-11-13, with `hyperskill/`, `configs/`,
  `scripts/`, `run.py`). **Watch-listed on the duplicate rule plus scope:** skill-library admission and
  composition are already carried densely (EvoGuard's pairwise admission selector, Graph-of-Skills'
  dependency-aware retrieval, SkillGate, Skill-α, COBRA-Skills, LiteEvo), the evidence is digital reasoning
  benchmarks rather than agentic tasks, and the contribution is a training-substitute for reasoning rather than
  a runtime contract. Revisit it if a robot or long-horizon agentic evaluation appears.
- **`2611.15521` — DecArena** (`cs.MA`). Decentralized multi-agent collaboration under graph-constrained
  communication with **100K+** synthetic tools, an explicit argument that "decentralized MAS needs more explicit
  **harness** mechanisms", and a **GraphHarness** baseline (topology-aware warmup plus self-improving
  LLM-based routing) reporting **+48.5** task success, **+30.6** collaboration quality at **−34.0** cost.
  **Watch-listed on artifact:** no repository, dataset or benchmark is linked on the record, in the abstract, or
  anywhere in the full text. The harness claim is right; there is nothing to check it against.
- **`2611.17060` — Ropedia Xperience 2** (`cs.CV`). Apache-2.0 `Ropedia/Ropedia-Xperience-2` (37 MB, created
  2026-11-11) with `scene_registration/`, docs and examples, plus a public **CC BY-NC-4.0** Hub dataset (4,408
  downloads). **Watch-listed on release shape:** the repository's own header states **"1,500+ scene scans ·
  4,000+ recordings · 74 tasks · 131 hours"** and then qualifies it — *"Scans: released set. Recordings, tasks,
  and hours: collection inventory."* — so the released artifact is the **scene scans**, not the behaviour corpus
  the title advertises. It is a data resource rather than a harness, and the partial release is exactly the
  distinction this list records literally.
- **`2611.15522` — GHAgentCI** (`cs.SE`). **15K+** workflow histories from **6K+** mined repositories,
  **159K+** revisions and **205K+** agent-evidence records, an open-source `cai-miner` extractor validated at
  **96.1%** (dedicated-action matches) and **98.8%** (GitHub Agentic Workflows) on **576** stratified records,
  and two Zenodo artifacts (both DOIs resolve). Watch-listed on scope: it is an observational corpus about
  *where* coding agents run in CI and what safeguards they declare, not a runtime interface or evaluation
  contract this list lacks — the observability line is carried by Say Shibboleth, guardscope and the
  replay/trace entries.
- **`2611.16526` — Lost in Delegation** (`cs.AI`). The most useful negative result of the batch: across
  pre-/post-cutoff splits, **delegation gives no consistent accuracy gain** over non-delegated setups, can
  **degrade** accuracy when the orchestrator lacks the knowledge, and **direct tool use with the same model
  performs much better**; the failure modes named are distorted delegated queries, inefficient tool use and
  lost information between agents. **Watch-listed as evidence, not as an entry:** no artifact, no harness, and
  the contribution is a measurement of orchestration failure that a future orchestration entry should cite.
- **`2611.15920` — Spec-Driven PBT** and **`2611.16289` — SatisFix** (`cs.SE`). Spec-derived property tests find
  a complementary bug set (nearly **60%** of **169** bugs found by only one of the two approaches, **2.4×**
  fewer property tests) and a specification-satisficing repair loop gains **+5.09%** pass@1 on DeepSWE and
  **+7.06%** on the SWE-bench Pro hard subset across five backbones and two frameworks. Rejected under the
  duplicate rule: the verification-from-specification line is already carried (Keeping LLM Agents on
  Procedure, Prefix Executability, ADAPT-style patch testing, and the decision-model gates), both are
  SWE-domain only, and neither links a located artifact.
- **The typed-decision-model cluster — `2611.15777` SO-Arena, `2611.15592` Small Heads, `2611.16961` The Value
  of Thinking, `2611.16937` S1-Evo, `2611.16499`, `2611.16321`.** A benchmark for System One models across 11
  domains and five capability families, a controlled study of typed-decision readouts, a zero-or-T routing
  decision, and an evidence-competition attack on typed System-1 models. Rejected this run on the duplicate
  rule: `2611.17167` is the entry that changes the picture (it moves the decision model from *acting* to
  *judging and teaching*), and the rest restate the gate/benchmark axis already carried by Decision Models as
  Policy Gates, JevAct, LEASH and SO-Arena's predecessors.
- **`2611.16246` — H2E** (`cs.RO`). Distils **agent sessions** (planner calls, sub-instructions, detours) into a
  VLA at the task level, lifting an 8B VLA from **35.0% → 44.9%** success on all **3,189** REVERIE-CE
  val-unseen episodes. Rejected as a model contribution: the harness is consumed, not contributed, and no
  artifact is located.
- **`2611.15831` — NIBench**, **`2611.14925` — ToolTurns**, **`2611.16107` — Accessibility Debt**, **`2611.17007`
  — Skill Adapter**, **`2611.15490` — ActionCodec2**, **`2611.16671` — NSDL**, **`2611.16466` — ReSHAPE.**
  A neuroimaging agent benchmark (56 tasks, seven tool ecosystems, 19 models, skill distillation), a multi-turn
  dialogue agent benchmark, a corpus audit of the GUI accessibility trees behind computer-use agents
  (**17,374,614** elements; **17.74%** vs **57.27%** description coverage), a personalised skill-adapter and
  online-routing system (**+11.43** points over SkillRL on ALFWorld/WebShop/SkillsBench), a streaming action
  tokenizer (**8.8** tokens/s at 15 Hz across 24 embodiments), a neuro-symbolic division of labour for
  multi-robot planning, and an agent-revised-specification planning study. Rejected on domain, duplication or
  missing artifact; none contributes a runtime, interface or evaluation contract this list does not already
  carry.
- **Serving and inference-efficiency results — `2611.16661` (batch makespan for agent workloads) and
  `2611.15126` (host-tier KV for multi-model agent workflows on a static-graph NPU).** Real harness-adjacent
  numbers (**89.5%** of executed planner prompt tokens had been materialized before on the collapsed default)
  but the runtime building-block line is carried by Metal Sandbox, XInfer and guardscope, and neither links a
  general artifact.
- **Model and metric contributions, excluded by the same rule as the previous weeks — `2611.17110` SALA,
  `2611.17160` XBot-Omni, `2611.16728` EgoX, `2611.17142` FORGE, `2611.16346` MotionSkill, `2611.16188` PERA,
  `2611.16589` STVLA, `2611.15659` GraSLA, `2611.16386` (critic-gated event memory), `2611.16645` (action
  continuity priors), `2611.15215` AFTER, `2611.15368` Bind-WAM, `2611.15974` (video-action models),
  `2611.16450` (physical action model recipe), `2611.16981` (weight-space programming), `2611.15048` and
  `2611.15082` (world-model evaluation scores).** These are models and diagnostics of models. Two are worth
  naming for a future measurement entry: `2611.15048` audits whether a world model's **dynamics predictor**
  responds to counterfactual action perturbations, and `2611.15082` separates encoder quality, near-future
  persistence and the predictor's own contribution with a persistence control over **27,000** future-retrieval
  trials — both are evaluation contracts for the world-model section rather than papers with artifacts today.

**Watch list carried forward for artifact releases** (unchanged by a 0-change diff, and re-checked where cheap):
StageWAM (still v3), ReflexVLA/ReflexBench, DreamX-Phi (README-only), UniTexture, PRISM ("Dataset soon"),
GigaBrain-0.7/WBC, ForceU-VLA (README-only), LIBERO-VIFO, Agent Lightning, VLCP, Hydra-0, BATON, RoboACE
(`DeLin1001/RoboACE` still size 0 as of 2026-11-13), and RSI-PDE's code repository (**404** as of today).

## Validation

- **Markdown structure.** Heading inventory unchanged: **17** `##` sections and **22** `###` subsections before
  and after; every new entry is a single list item under the correct subsection, appended at the end of that
  subsection's list in the repository's existing order; no heading is preceded by a non-blank line, and no line
  carries trailing whitespace.
- **`git diff --check`** — no whitespace errors.
- **Link resolution.** Every URL added by this run resolves: `arxiv.org/abs/2611.14949`, `/2611.17167`,
  `/2611.15651`, `/2611.16116`, `/2611.16312`, `jeremylu916.github.io/RSI-PDE/`,
  `github.com/ZJU-REAL/decision-agents`, `zjureal.com/decision-agents/`,
  `github.com/pm1255/embodied-corrector`, `pm1255.github.io/embodied-corrector/`,
  `huggingface.co/datasets/vharatian/Robo-SWE`, `github.com/argonrobotics-ai/alloy` and
  `alloy.argonrobotics.ai/docs/` all returned **200**. The two URLs this run recorded as **not** open —
  `jeremylu916/RSI-PDE` (**404**) and `JeremyLu1017/PDE-RSI` (**401**) — are named in backticks and
  deliberately **not** hyperlinked.
- **Duplicate check.** All five ids were grepped against `README.md`, `docs/*.md`, `sources/*.md` and
  `CONTRIBUTING.md` **before** insertion; none was present. After insertion each appears in exactly one entry
  and in at least one of the Contents blurb or a Landscape bullet; no id appears in more than one entry.
- **Dates and categories.** Every included entry names its arXiv id and its `<published>` date — four on
  2026-11-11/2026-11-12 and one (`2611.14949`) on 2026-11-11 — and the README badge, the `## Current
  Landscape` `Last verified:` line, the Contents blurb and this record's filename all read **2026-11-14**.
- **Cross-file consistency.** `docs/landscape.md` keeps its earlier verification date as a historical note
  rather than a maintenance surface, unchanged by this run. No other file cites a date for this batch.
- **Evidence-scope discipline, stated per entry:** digital agents only (`2611.14949`, `2611.17167`,
  `2611.16116`); simulation only for the robot domain (`2611.15651`); simulator infrastructure with
  author-reported benchmarks (`2611.16312`). No entry implies independent reproduction, and no entry describes
  a promise or a placeholder as open.

## Operational notes and blockers

- **Spacing from the first request is what avoided the rate limiter.** The previous run lost four requests to
  `429 Rate exceeded.` / `503` after an initial burst; this run issued **22** export-API range queries at **8 s**
  spacing from the start and every one returned **200** first time. The operational rule is now simply: space
  *submittedDate* queries from the beginning rather than diagnosing an outage afterwards.
- **Empty days cost four requests each because of the harvest loop's own condition.** The loop repeats while
  `entries == 0`, which is right for a transient refusal and wrong for a genuinely empty day; the duplicate
  requests are harmless (all returned 0) but they inflate the request count. The next agent should treat
  "0 entries, HTTP 200, `api/errors` 0" as an answer rather than as a failure.
- **The API can be checked for staleness without leaving arXiv.** `age: 0` / `x-cache: MISS` on the export
  response plus `arxiv.org/list/cs.RO/recent` (which reported "Fri, 13 Nov 2026 … 106 entries") is enough to
  distinguish "frozen index" from "cached page", and it cost two requests.
- **The OpenSSH `/etc/ssh/ssh_config.d` ownership failure is real on this host and constant.**
  `git ls-remote` fails with *"Bad owner or permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf"*;
  `GIT_SSH_COMMAND='ssh -F /dev/null'` works and is used for `ls-remote`, `fetch`, `pull` and `push`.
- **No GitHub rate limiting.** **13** `api.github.com` calls completed without a `403`, including one 404
  (repository metadata) and one 404 (its contents listing), both of which are the finding rather than a
  failure.
- **Debt for the next run.** Query **2026-11-13, 2026-11-14 and 2026-11-15** — the first two returned **0**
  with an unmoved frontier, so both remain owed — and re-harvest **2026-11-11 through 2026-11-13** fully. The
  next announcement batch should carry the Friday and Saturday submissions together; if 2026-11-13 or 11-14
  arrives, screen it with abstracts, not titles, because that is where every recovery in the last two runs
  came from. Two artifacts are worth re-querying cheaply: `jeremylu916/RSI-PDE` (404 today) and the
  `JeremyLu1017/PDE-RSI` dataset (401 today).
