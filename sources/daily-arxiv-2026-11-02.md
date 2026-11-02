# Daily arXiv scan — 2026-11-02 (the four-day announcement stall breaks: a 122-record 2026-10-29 block that had never been visible plus the full 361-record 2026-10-30 batch; fourteen inclusions)

## Scope

- **Interval owed.** The previous successful run started **2026-11-01 10:04 UTC** and committed at **2026-11-01 10:12 UTC**
  (`2026-11-01T11:12:16+01:00`) on `main` at `3a714cc` with record `sources/daily-arxiv-2026-11-01.md`. That record closed
  2026-10-29 at the then-frontier `2610.40646` / `2610.40655` and explicitly **owed the three unannounced days
  2026-10-30, 2026-10-31 and 2026-11-01 to the next run**. This run started **2026-11-02 13:37 UTC** on `main` at
  `3a714cc`, so the interval it owes is **submission dates 2026-10-30 → 2026-11-02 inclusive**, together with the
  2026-10-29 remainder the previous record named.
- **The frontier moved, and it moved by more than one day.** An unrestricted descending `cs.RO` probe now returns
  **`2610.42146`** (`2026-10-30T17:59:25Z`), where the previous three runs all returned **`2610.40646`**
  (`2026-10-29T17:59:20Z`). A union probe over all five target categories returns **`2610.42152`**
  (`2026-10-30T17:59:59Z`). The 2026-10-30 batch has now been announced.

  | query (one parenthesised OR-query over cs.RO/cs.AI/cs.CL/cs.CV/cs.LG per day) | 2026-10-29 | 2026-10-30 | 2026-10-31 | 2026-11-01 | 2026-11-02 |
  | --- | ---: | ---: | ---: | ---: | ---: |
  | `submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` | **620** | **361** | **0** | **0** | **0** |
  | recorded by the previous run | 498 | 0 | 0 | 0 | — |

- **2026-10-29 grew by 122 records; this is the part that had never been visible, not a recount.** The previous harvest
  is still on disk at `.scratch/arxiv-2026-11-01/day-20261029.xml` (uncommitted), so the diff is exact rather than
  inferred: **620 − 498 = 122 added, 0 removed**. The added IDs run **`2610.40713`–`2610.41086`** — strictly above the
  old maximum `2610.40655`, so no record was replaced or re-versioned — but their `published` timestamps span
  `2026-10-29T00:44:29Z`–`2026-10-29T23:57:02Z`, and **25 of the 122 predate the old batch cutoff** `17:59:53Z`.
  The correct description is therefore *a withheld ID block*, not *an evening tail*: those 25 records were submitted
  before the previous run's own frontier and were still invisible to it. They are screened here for the first time.
- **The 2026-10-31 and 2026-11-01 zeros are still owed.** 2026-10-31 was a Saturday and 2026-11-01 a Sunday, and arXiv
  announces on a weekday schedule, so nothing from either date has reached the export API. 2026-11-02 (Monday) also
  returns 0 because its batch had not closed at the time of the query. **All three dates remain owed to the next run**,
  which must query them again rather than treat them as screened.
- **The batch is 483 records, 483 of them new to this repository.** **0 of 483** were already named anywhere in
  `README.md`, `docs/*.md` or `sources/*.md`, so unlike the previous three runs this screen started from a genuinely
  untouched set and no re-screen of old bytes was needed.
- **Coverage.** Every record carries at least one of the five target categories. Primary categories — 2026-10-29 withheld
  block (122): `cs.LG` 33, `cs.AI` 24, `cs.CV` 19, `cs.CL` 11, `cs.CR` 9, `cs.RO` 8, `stat.ML` 3, `math.OC` 3;
  2026-10-30 (361): `cs.LG` 77, `cs.AI` 74, `cs.CV` 57, `cs.RO` **52**, `cs.CL` 35, `cs.CR` 10, `stat.ML` 8,
  `eess.IV` 7. `cs.RO` appears as primary or cross-list on **72** of 483 records (**13** + **59**).
- **Revisions.** **Zero.** All **483** entries are `v1` with `<updated>` equal to `<published>`, and the `id_list` sweep
  in the method section found no watch-list paper that moved version in this interval.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`. **10 export-API requests**: one `cs.RO`
  frontier probe, one five-category union probe (`max_results=3`, descending), five per-day counts, two full-day
  harvests (`max_results=2000`, ascending; 361 and 620 entries, 882 KB and 1.5 MB), and one `id_list` revision sweep.
  All at 3–5 s spacing, **zero HTTP 429**.
- **`/tmp` is still not durable between tool calls.** A directory created under `/tmp` in this run's first call was gone
  in the second, reproducing the previous three runs' finding. All XML, HTML, JSON and page captures are therefore in
  the untracked `.scratch/arxiv-2026-11-02/` directory, which is **not staged and not committed**.
- **Query syntax matters.** The unparenthesised form drops the date filter and returns a constant site-wide count; the
  counts above use `(cat:cs.RO OR cat:cs.AI OR cat:cs.CL OR cat:cs.CV OR cat:cs.LG) AND submittedDate:[...]` and were
  checked against the previous run's recorded 498 for 2026-10-29 (now 620, the difference being the 122).
- **Screening.** (a) All **483** titles were read in ascending ID order. (b) Candidates were grouped by robot-anchor
  (VLA, manipulation, world-action model, embodied, humanoid, grasp, dexterous) and harness-anchor (harness, scaffold,
  runtime, recovery, oversight, memory, skill, tool-use, evaluation, safety, verifier, agentic) terms over
  title + abstract, which selects **56** robot-anchored (49 of them `cs.RO`), **185** harness-anchored and **26** with
  both, a union of **215**; these were ranked rather than read. (c) **29** abstracts were read in full. (d) **7**
  full-text HTML renderings were requested (`arxiv.org/html/<id>v1`; 6 returned, 1 had no HTML) and read for artifact
  claims, author statements about code release, and citation-versus-release disambiguation. (e) Every claimed artifact
  was then checked at the source: **16 GitHub REST calls**, **3 Hugging Face API calls**, **5 project pages**, **4
  repository `README.md` fetches**, and **1** anonymous-mirror probe.
- **Two `Code:` claims were disambiguated against the paper's own text, and one of them changed the verdict.**
  `2610.40799` (CaPtain) contains a single GitHub URL, `WillWangZiHuan/SenseNova-RoboRSI`, which the reference list shows
  to be **citation [64]**, a *competitor's* repository — the paper's own release is in its `\metadata` block
  (`waltstephen/CaPtain`, `waltstephen/CaPtain-Harness`, a Hugging Face collection). `2610.41868` (META-Robot) contains
  `deepseek-ai/deepseek-harness`, which is likewise a **citation**. Grepping for a repository URL in the full text and
  stopping there would have credited META-Robot with an artifact it does not have and missed the one CaPtain does.

## Included (14)

### General Harness Design and Self-Improvement (3)

- **`2610.40917` — Towards Portable Specification of the Agent Harness** (cs.AI; Gertler, Novelli, Floridi). Names the
  missing object in agent reporting — the harness has no settled definition, so a score is not attributable to a
  configuration — decomposes it into **eight functional components**, and proposes a **portable specification framework**
  recording which model ran in which harness. The case for versioning is empirical as well as legal: several-fold score
  differences for the same model across harness configurations, and much of the attack surface *is* the harness;
  harnesses change between releases, between runs, and — where agents edit their own tools and configuration — within a
  run. Model and agent cards record release and operational facts but not the sequence of effective configurations.
  `cs.AI` only, no robot experiment.
  **Artifact status literal (2026-11-02): no artifact.** No code, schema, or template appears on the arXiv record or in
  the text. The specification exists as prose.

- **`2610.41130` — One Verdict Is Not Enough: Regression Testing Learned World Models and Their Test Harnesses**
  (cs.SE/cs.AI/cs.CV). Puts the **test harness inside the system under test**: a harness fault can produce erroneous
  scores and wrong verdicts *without a crash*, and a test can pass under one configuration and fail under another.
  TetraState runs each paired possible/impossible video test under four fixed model-native configurations, assigns one
  of four outcome classes, reports inconclusive tests and failed executions separately, and compares revisions by
  per-test **outcome transitions**. On all 506 IntPhys 2 Main tests, two V-JEPA 2 harness revisions move modal accuracy
  by only **0.2 and 2.6 points** yet change **11 and 214 test labels**; in the second, accuracy **rises** although fewer
  tests are judged correctly, and **43 regressions keep the modal verdict**, so a modal-flip check misses them.
  Simulation/video tests only; author-reported.
  **Artifact status literal (2026-11-02): not accessible.** The paper cites
  `anonymous.4open.science/r/tetrastate-artifact-5C1E/`, which returns **HTTP 401 `{"error":"not_connected"}`** — the same
  disposition recorded for SkillSense on 2026-10-31. No code is counted as open.

- **`2610.40747` — Mage-VL-Cowork: A Full Training Recipe to Learn Hybrid Computer Use From Evolved Harnesses**
  (cs.CL). The batch's clearest statement that **the harness, not a fixed teacher, is the thing to evolve**: rather than
  distilling a default teacher, the pipeline evolves the teacher harness and instantiates specialised CUA harnesses to
  produce both high-quality trajectories and stage-specific supervision. Mage-CUA-Forge replaces per-task agent
  construction with reusable skill-conditioned generation workflows, synthesising **293,806** desktop and web tasks
  across **6,915** skills at **82.6%** lower recurring per-task cost; a five-stage curriculum over a 4B hybrid
  GUI-plus-terminal policy reaches **66.0% on OSWorld-Verified**. Digital computer-use, no robot experiment;
  author-reported.
  **Verified open (2026-11-02): both code and weights.** `microsoft/Mage` is **MIT**, 233.7 MB, 1,757 stars, pushed
  2026-10-30, with a populated `mage_vl_cowork/` subtree (`README.md` 39 KB, `assets/`, `osworld_eval/`); the project
  page additionally links collection `microsoft/mage`, and `microsoft/Mage-VL-Cowork` weights are public on Hugging Face
  (safetensors, 1,152 downloads, 130 likes).

### Agentic Robot and VLA Harnesses (5)

- **`2610.40799` — CaPtain: A Pipeline for Post-Training Code-as-Policy Robot Agents** (cs.RO; Fan, He, Jiang, Wu, Wang,
  Yue — CUHK). Applies the standard agent recipe — **SFT to learn the interface, multi-turn RL to master it** — to
  code-as-policy manipulation with no VLA, no frontier model and no privileged state at test time. **CaPtain-Harness**
  is a persistent sandboxed Python session with **22 low-level perception / geometric-measurement / guarded-motion
  primitives** plus `finish()`, deliberately without high-level skills; every call returns a text receipt and the same
  API spans LIBERO/‑PRO/Plus/MAX, RoboTwin 2.0, RoboDojo, SimplerEnv, CaP-Bench and real robots. Because multi-turn RL
  must credit tens of requests whose physical effects surface turns later, a **physical critic** reads privileged
  simulator state the policy never sees and scores each request by its change in predicted success. From Qwen3.5-4B at
  0% zero-shot the authors report **55.4%** held-out LIBERO after SFT and **73.8%** after RL, **78.5%** on LIBERO-PRO,
  RL gains persisting across three perturbation suites (**+17.6 to +21.2**), **83.3%** on SimplerEnv WidowX without
  target-domain training, and real-robot execution after simulation-only adaptation.
  **Verified open (2026-11-02).** `waltstephen/CaPtain` is **MIT**, 8.3 MB, 48 stars, pushed 2026-10-30, containing
  `harness/` (a pinned submodule of the separate MIT `waltstephen/CaPtain-Harness`, 2.1 MB), `sft/`, `rl/`, `docs/`,
  `AGENTS.md`, `CITATION.cff`, `RELEASE_NOTES.md`, `THIRD_PARTY_LICENSES.md`. The Hugging Face collection
  `YijiaFan/captain-6ae3314bcf088fae95904168` resolves with **4 items**: `CaPtain-4B`, `CaPtain-4B-SFT` (models) and
  `CaPtain-SFT-Data`, `CaPtain-artifacts` (datasets). Real-robot claims remain author-reported.

- **`2610.42141` — RoboArchitect: Automating the Design of Robotic Agents with Persistent Memory** (cs.RO/cs.AI/cs.CV;
  Wu, Huang, Ho, Doshi, Zhu, Yeung-Levy — Stanford). Turns harness construction into the search target: a coding model
  observes a frozen policy's rollouts (video, a step-by-step trace of observations/actions/model calls, and the memory),
  diagnoses failures, and rewrites the harness, with revisions grounded in observed behaviour and the result evaluated
  on held-out scenes and tasks. With no weight update, average success rises **9.7% → 43.3%** on unseen scenes (4.5×)
  and **12.7% → 40.2%** on unseen tasks (3.2×) across benchmarks and policies, with the largest gains where the
  benchmark is farthest from the policy's training data — a general-purpose policy in an evolved harness often beats its
  own finetuned version. Evolved agents converge, unprompted, on a **dynamic VLA/code split**, and the harness evolves
  its own **persistent memory** carrying working grasp depths and per-object grip choices forward. Author-reported.
  **Public but unlicensed (2026-11-02): not open source.** `roboarchitect-ai/RoboArchitect` (5.5 MB, 0 stars, created
  2026-10-29, pushed 2026-11-02) has no license in the GitHub API and no `LICENSE` in its root; it does contain
  `roboarchitect/`, `policies/`, `benchmarks/`, `setups/`, `splits/`, `auxiliary/`, `media/`, `paths.example.json` and a
  14 KB README with setup and checkpoint instructions. Public access is recorded; an open-source release is not claimed.

- **`2610.41216` — TinyAstral: Transferring Self-Evolving Harnesses to Tiny Generalist Manipulation Agents**
  (cs.RO/cs.CV; Gong, Zheng, Hou, Lee, Guo, Tan, Wen, Wang, He, Zhang, Bi Mi). Asks whether a large teacher's **harness
  improvements can be carried into a small agent**, and answers with two-stage transfer. Stage one freezes an open-weight
  ~180B teacher and improves *only* its harness by recursive self-improvement — frontier LLM agents read the teacher's
  trajectories and propose edits, each kept only if it passes a **paired statistical test**. Stage two freezes the final
  harness, collects teacher trajectories including reasoning and failure recovery, and distils 0.8B–4B students that
  replace the teacher in the unchanged harness. On RoboTwin harness evolution raises the teacher **33.4% → 63.6%** and
  the final 4B student reaches **62.8%**; across four harness versions each student tracks its own teacher, so later
  harness versions yield stronger students, and the gains transfer to other frozen planners without retraining. On
  identical episodes the 4B student beats open 4B agents, the open 27B harness-RSI model SenseNova-RoboRSI, and its own
  teacher placed in three competitors' harnesses; on three benchmarks untouched by RSI or training it scores close to a
  552B planner running the same harness, at about **one fifth** of the teacher's accelerator time. Author-reported.
  **Artifact status literal (2026-11-02): project page only.** `garfield-kh.github.io/TinyAstral/` returns HTTP 200 with
  video, results and a BibTeX block, but the page contains **no repository, weights or dataset URL of any kind**, and the
  arXiv comment lists only the project page. Nothing is counted as open.

- **`2610.42053` — REA: A Routed Embodied Agent Runtime for Asynchronous Brain-Cerebellum Control** (cs.RO/cs.CV; Zhou
  et al., 32 pages). Makes the brain/cerebellum coordination failure a runtime contract: an agent that calls the VLA
  inside its own loop stalls fast execution, while a VLA that calls the agent on errors re-triggers fallbacks or misses
  silent failures. REA's golden rule is **no running policy ever waits on the brain**, implemented as a **route memory**
  of pre-measured per-policy success, a **boundary-triggered judge gate** that consults the deliberative System 2 on
  execution, and a **shared-budget escalation ladder** bounding recovery. The authors report **95.9%** over 3,000
  episodes on 60 LIBERO-Pro tasks against **94.7%** for the best single-agent baseline in its own policy pool, with
  **25%** fewer control steps and **17%** less control time; **84%/86%** on a separate 100-episode RoboTwin 2.0 study
  against **80%** for MEDex; and transfer to BEHAVIOR-1K and a real Unitree G1.
  **Artifact status literal (2026-11-02): none located.** No code, model, dataset or project link on the arXiv record or
  in the full text. Nothing is open.

- **`2610.41868` — META-Robot: Self-Cognition and Capability Scaling in Agentic Robots** (cs.RO; Feng et al.). Attacks
  the part of an agentic robot that scaffolding does not supply: knowing **what it currently cannot do**. **Action-level
  predicates** make execution outcomes explicit and available to LLM reasoning; the agent uses those assessments to
  identify its own limitations, extends the evaluation criteria through which it judges itself, and expands its skills
  along the criteria it just added, with proprioceptive feedback grounding the assessment in physical state rather than
  vision alone. Simulation plus real robots; author-reported.
  **Artifact status literal (2026-11-02): none.** The only GitHub URL in the full text,
  `deepseek-ai/deepseek-harness`, is **citation [DeepSeek-AI 2026]**, a third-party harness reference — not this work's
  release. No first-party code, weights or data is linked.

### Runtime and Safety (1)

- **`2610.41609` — VLA Policies Do Not Know When Not to Act: Unintended Task Execution and Start Suppression**
  (cs.RO/cs.AI; Jang, Choi, Yoon, Kim). Establishes an **unprompted-actuation** failure. A paired protocol holds the
  initial state fixed and varies only the instruction across six checkpoints in LIBERO-Goal: with an **empty**
  instruction the robot started moving in **every episode with all six** checkpoints, and five of six often completed a
  task the instruction had not specified; on two GR00T checkpoints the instruction determined *which* task ran but not
  *whether* motion started, and the robot moved even on infeasible instructions. The three mitigations separate by what
  they can see: the "if no task is given do not move" prefix suppressed nothing in **6 of 6**; a gate holding the robot
  when the first action chunk nearly equals the empty-instruction chunk held it on non-task inputs in **5 of 6**; and
  fine-tuning never completed a task on non-task inputs but did not reliably keep the robot still and failed on
  infeasible instructions. Simulation-only; author-reported.
  **Artifact status literal (2026-11-02): claimed code is 404.** The record's `Code:` link resolves to
  `github.com/zebehn/vla-start-suppression`, which returns **HTTP 404**; the owner account `zebehn` exists (33 public
  repos) but a GitHub repository search for `vla-start-suppression` returns **total_count 0**. The paper's claim of a
  code release is not met, so nothing is counted as open.

### Observability and Replay (1)

- **`2610.41025` — ReplayRCA: Measuring Post-hoc Diagnosis and Live Oversight of Agent Failures by Counterfactual
  Replay** (cs.AI; Canale, Soulis). Scores failure diagnosis **by intervention rather than by agreement with
  annotations**: restore the environment to the state before the chosen step, substitute the proposed action, let the
  same agent continue, and subtract a paired placebo replaying the agent's own action — the difference is *causal
  repair*. Two tracks share the protocol: a **post-hoc autopsy** given the whole failed run and told it failed, and
  **live oversight**, where a supervisor watches silently and may intervene once before an irreversible action without
  knowing whether the run will fail. On 62 calibrated systematic failures the best of 13 frontier models reaches
  **+51.3**, the top eight are statistically indistinguishable, the flagship models of the two largest vendors rank 8th
  and 12th, and five items are repaired by no model. Forcing a strong model to intervene at the last step costs it
  **17.1 points**, so the last-step shortcut of annotation-based attribution does not transfer. The harness-relevant
  finding is that **post-hoc diagnostic skill is a poor proxy for live oversight**: the supervisor among the best
  post-hoc diagnosers is the **weakest live** one (**−14.5**, p = 0.002) while a small open-weight model is
  statistically tied for best. Digital back-office environment (OpsWorld); no robot experiment.
  **Verified open (2026-11-02):** `jeremy1392/RCAreplay`, **MIT**, 92 KB, created and pushed 2026-10-29, 0 stars — small,
  but the environment and evaluation code are what the paper says it releases.

### Simulation and World-Model Fidelity (2)

- **`2610.41857` — When the Simulator Fails, Not the Policy: Auditing Contact Artifacts in Simulated Robot Benchmarks**
  (cs.RO; Zhou et al.). Traces a measurable share of recorded policy failure to **one constant**: in the physics engine's
  distance algorithm triangles below a fixed area threshold are discarded, and at centimetre scale valid triangles fall
  below it, so geometry 1–2 cm apart reports ~2 cm penetration and the solver ejects the object at metres per second.
  **SimuGuard** is an auditing plugin for LIBERO, RoboTwin, RoboCasa and RoboDojo that leaves evaluation unchanged,
  records state every physics step, flags objects moving at contact faster than robot contact or gravity accounts for,
  replays an episode bit for bit, and re-runs it with one parameter changed. Changing only that constant takes artefact
  episodes from **12 in 100 to none** and five container tasks from **89% to 98%**; the benchmark's own scripted
  demonstrations fail the same way. The authors recommend re-running PhysX ≤ 5.3.1 benchmarks in a repaired engine, or
  reporting the range a success rate spans (75.8%–98.5% around a reported 83.1% for LingBot-VA). Simulation-only.
  **Public but unlicensed (2026-11-02): not open source.** `ALEXander22568/simuguard` returns no license from the GitHub
  API, has **no `LICENSE` file and no licence statement in its README**, and is 50.6 MB, 14 stars, pushed 2026-10-31,
  with `simuguard/`, `compat/`, `configs/`, `scripts/`, `tests/` and `paper/`; project page
  `alexander22568.github.io/simuguard/` is live (HTTP 200).

- **`2610.41970` — When Does Test-Time Search Help World Action Models? An Empirical Audit of Candidate Selection and
  Simulator Integrity** (cs.AI). Audits test-time scaling as a **chain of premises** — decision signal, realized
  efficiency, evaluation integrity — under a premise-first pre-registered protocol on Cosmos Policy and RoboCasa, rather
  than reading a success-rate gain. In the tested configuration all three fail: a learned value selector is **1.66 points
  worse than random**; a hindsight oracle gains only **1.40** points with an episode-level interval spanning zero; and
  **82%** of states show identical candidate outcomes, leaving little headroom. Token pruning cuts per-forward FLOPs by
  up to **47%** yet yields at most a measured **1.15× end-to-end** speedup on the deployed path against a 1.5× target
  (1.25× on a decode-excluded path), and the adaptive-allocation experiment is **inconclusive** under its own power gate.
  The audit also finds and fixes a **silent rendering corruption** whose rate rises along compute-heavy planning paths
  while leaving the official direct-policy result unaffected. Authors scope the conclusions to the tested configuration;
  simulation.
  **Artifact status literal (2026-11-02): not located.** The paper states it releases "a detector, fix, and scripts", but
  **no repository URL appears in the arXiv record or in the full text**. Nothing is counted as open.

### Robot Foundation and World Models (1)

- **`2610.40748` — WorldBridge: Bridging Video and Action for Task Generalization in World Action Models** (cs.RO;
  alphabetical author list, Joy Future Academy). Turns WAM generalization into a supervision question with an ablation
  worth reusing: on RoboTwin 2.0, in-distribution video-action supervision only gives **25.07%** on out-of-distribution
  tasks; adding videos of those tasks **without their action labels** raises it to **55.90%**, and one action-labelled
  demonstration per task raises it to **70.70%** — reading as an **existing inverse-dynamics capability** that video
  coverage recruits and sparse action supervision sharpens. WorldBridge itself separates video and action experts joined
  by structured attention with joint flow matching, adds two-scale visual memory and action forcing, and reports 96.23%
  (RoboTwin 2.0 full-data), 80.9% (clean-to-random), 82.8% (RoboCasa GR1 TableTop) and 57.6% (DOMINO Level-1), all
  author-reported.
  **Artifact status literal (2026-11-02): project page only.** `worldbridge-jd.github.io/` returns HTTP 200 with a demo
  browser and BibTeX but **links no code, weights, or data**. Nothing is open.

### Surveys and Reading Lists (1)

- **`2610.41688` — When the Past Shapes Action: Understanding Embodied Memory** (cs.RO/cs.AI/cs.LG; Li, Bai, Xu, Huang,
  Karlsson). Gives the memory component of an embodied harness an explicit vocabulary instead of a model list: **six
  memory requirements** — including world-state persistence, task progress, belief maintenance and experiential
  learning — crossed with **eight architectural categories**, including recurrent states, fast weights, visual and
  symbolic representations, spatial memory and experience-based systems. Its transferable move is methodological: it
  separates benchmarks that **test memory dependence** from benchmarks that merely reward task success — the same
  distinction this repository's evaluation entries draw for verification and recovery. Named open problems are the ones
  the runtime entries keep hitting: selecting relevant experience, keeping a belief consistent as the environment
  changes, and consolidating across episodes.
  **Artifact status literal (2026-11-02): none.** No code, dataset, or reading-list repository is linked on the record.

## Revision handling (no entry changed)

- **None in-batch.** All **483** new entries are `v1`; `<updated>` equals `<published>` for every one, so the 122
  withheld 10-29 records and the 361 10-30 records are single-vintage blocks.
- **None on the watch list either.** An `id_list` sweep over seven tracked papers returned no version that had moved
  since the interval: `2608.10669` and `2608.02276` and `2608.14380` are unchanged; `2606.09498` (v3, 2026-08-20),
  `2607.08448` (v5, 2026-09-24), `2608.09516` (v2, 2026-10-10) and `2608.12851` (v2, 2026-10-07) all carry
  update dates **older than this interval** and were already recorded by earlier runs. No `README` entry was re-worded
  for a revision this run.

## Rejected, deferred and watch list

- **On-scope but deferred this run (no artifact located, or a family already represented).**
  `2610.40721` **MetaVul** (agentic harness discovery for vulnerability detection; a self-improving harness is exactly
  this list's subject, but no repository or artifact is linked, the domain is digital security with no robot experiment,
  and the Harness-R1 / Self-Harness family already covers trained harness editors; the arXiv comment "Originally
  announced September 2026" on an October ID was not resolvable);
  `2610.41397` **Cross-Interface Authorization for AI Computer-Use Agents** (a genuinely strong permissions contract —
  route-enumerated effect sinks, route-bound prepare tickets, commit-time re-verification — but digital computer-use and
  no artifact located); `2610.41453` **Evidence-Guided Recovery** (recovery over ambiguous tool outcomes; a 37-assignment
  finite study, no artifact); `2610.41479` **EvoMCP** (agent–data mutual evolution, no artifact);
  `2610.41056` **Accept Changes, Reject Attacks** (memory management robustness, no artifact);
  `2610.41604` **Real Computation or Merely Naming** (pre-registered audit of 25 cognitive-architecture modules; a
  useful verification method, no artifact, and its subject is claim-versus-implementation rather than runtime);
  `2610.41172` **Context as Infrastructure** (harness-side retrieval; digital, no artifact);
  `2610.40901` **TSFLab** (self-improving agent-agnostic workbench, code at `D2I-Group/TSFLab`, but time-series research
  is outside scope); `2610.42043` **Proof-Carrying Evolution** (certified controller synthesis; no robot experiment);
  `2610.40798` **Beyond Plausible Prediction** (safety sufficiency in *web* world models).
- **On-scope but held for name collision or overlap rather than merit.** `2610.41275` **TRACE: Target-State
  Risk-Aligned Conditional Execution for Embodied Tracking** is the kind of execution-time safety gate this list tracks —
  it shows PPO on the planner head *raises* the collision rate by converting target-loss episodes into contact, then
  gates on a calibrated 84-cue risk head — but a different **TRACE** is already listed at `2610.38400` (the HRI
  *moot success* study, disambiguated in the 2026-11-01 record). Adding a second identically-named TRACE needs its own
  disambiguation pass; deferred deliberately, not rejected on quality. `2610.41844` **SketchMate** (agentic geometric
  reasoning with a sketch memory that transfers stored sketches by point-cloud registration) overlaps the memory
  coverage taken this run by the embodied-memory survey and has no artifact.
- **Model contributions, excluded because this repository does not treat a model paper as a harness paper** — the
  explicit curation rule. Representative: `2610.42146` ID-WM (hierarchical world models with implicit dynamics,
  +38.5% over prompt/diffusion planners), `2610.42096` CausalWAM (causal identification for action-relevant latents),
  `2610.42021` Nexus-VLA (cross-embodiment shared semantic latents), `2610.41408` SACQ (ultra-low-bit VLA
  quantization), `2610.41625` RTPolicy, `2610.41643` Direct Action-Manifold Modeling, `2610.41654` ∇Mimic,
  `2610.41663` ResMerge, `2610.41877` Movens-Zero, `2610.41009` STAR, `2610.41320`, `2610.41330` cross-embodiment
  motion planning. `2610.42097` **ROOF** (humanoid fall recovery) is robot recovery but a learned controller rather than
  a runtime contract.
- **Off-scope remainder (representative, from the 483).** SLAM, odometry, calibration and 3D reconstruction
  (`2610.41671`, `2610.41602`, `2610.41768`, `2610.41352`, `2610.41833`); motion planning and control geometry
  (`2610.41548`, `2610.41558`, `2610.41667`, `2610.41676`, `2610.41752`); legged/humanoid control and hardware including
  exoskeletons and suspended-fabric surfaces (`2610.41981`, `2610.41982`, `2610.41679`, `2610.41053` embedded UAV
  firmware); safety filters and CBF navigation (`2610.40922`, `2610.41188`, `2610.41214`, `2610.41477`); on-orbit
  servicing and inspection RL (`2610.41639`, `2610.41647`, `2610.41839`); autonomous-driving auditing and work-zone
  datasets (`2610.41185`, `2610.41772`); medical imaging, pathology and clinical agents; UAV, ISAC and network control;
  and the large LLM remainder (post-training, quantization, KV-cache, speculative decoding, serving, RLVR, unlearning,
  RAG evaluation, speech).
- **Watch list carried forward, not re-checked at link level this run** (no new version and no visible release activity
  in the harvest): **RoboSeed** weights, **ReSTEP** dataset and code, **EvoLife** ("Code is coming soon"),
  **Sandman**, **LOGIV**, **AgentHeal** (public, unlicensed), **METATRON** and **ELEVATE** (project page and demos only),
  **Scan Before You Install** ("upon acceptance"), **SkillSense** (`anonymous.4open` 401), **PASA-KV**, **Soei**,
  **Assurance Stack** agenda, and nothing-located **KARMA**, **VEvo-Harness**, **Masked Erosion**, **Prefix
  Executability**, **REALM**, **JevAct**, **PrimaCons**, **Minimum Sufficient Communication**. **New watch-list entries
  from this run:** `2610.40917` (no specification artifact), `2610.41868` META-Robot, `2610.42053` REA and
  `2610.40748` WorldBridge (all included on paper merits, all with no located artifact),
  `2610.41970` (announced release not found), and `2610.41609` (see below).

## Validation performed

- **Markdown structure.** 14 new single-line list items appended under existing `###` headings at seven insertion
  points; 1 new bullet added under `## Current Landscape` in the same `- **claim.** [entry] …` shape as its neighbours.
  No heading levels changed, no list nesting introduced, no HTML added, no table modified.
- **`git diff --check`.** Clean — no whitespace errors, no conflict markers, no trailing whitespace.
- **Links.** Every added arXiv `abs` URL was fetched during verification (HTTP 200), and the `abs` page was used for the
  title, category list, comment and author list quoted in this record. Every non-arXiv link added to `README.md` was
  resolved at the source this run: `waltstephen.github.io/captain/`, `waltstephen/CaPtain`,
  `waltstephen/CaPtain-Harness`, the Hugging Face collection, `roboarchitect-ai.github.io/`,
  `roboarchitect-ai/RoboArchitect`, `garfield-kh.github.io/TinyAstral/`, `microsoft.github.io/Mage`, `microsoft/Mage`,
  `huggingface.co/microsoft/Mage-VL-Cowork`, `github.com/jeremy1392/RCAreplay`, `ALEXander22568/simuguard`,
  `alexander22568.github.io/simuguard/`, `worldbridge-jd.github.io/`. The one **404** —
  `github.com/zebehn/vla-start-suppression` — is deliberately still cited in the entry, because the entry's point is
  that the paper's own claim does not resolve.
- **Duplicate IDs.** All 14 new IDs were checked against `README.md`, `docs/*.md` and `sources/*.md` before insertion
  (**0 of 483** batch records were previously named), and after insertion each new ID occurs exactly twice — once in the
  landscape bullet where applicable and once in the main list — which is this repository's established pattern. The
  `TRACE` name collision with the existing `2610.38400` entry was the reason `2610.41275` was deferred rather than added.
- **Dates and categories.** Every entry states its arXiv ID; category placement follows each paper's actual contribution
  (for example `2610.40747` is `cs.CL` on arXiv and is placed under General Harness Design because its contribution is
  harness evolution, while `2610.40748` is `cs.RO` and is placed under World Models). All 483 records were published
  **inside** the 2026-10-29/2026-10-30 window that this run harvested; none came from a 10-31/11-01/11-02 batch, because
  none exists yet.
- **Cross-file consistency.** The README badge and `Last verified:` now read **2026-11-02**, matching this record and the
  `sources/daily-arxiv-2026-11-02.md` filename. The previous record and the README's earlier history are unchanged.
- **Artifact-status discipline.** Five distinct literal statuses are recorded, and each is stated in the entry itself:
  **fully open** (CaPtain: MIT code + models + data; Mage-VL-Cowork: MIT code + weights), **public but unlicensed**
  (RoboArchitect, SimuGuard — neither counted as open source), **project page only** (TinyAstral, WorldBridge),
  **claimed link does not resolve** (`2610.41609`, 404), **claimed anonymous artifact inaccessible** (`2610.41130`, 401),
  **announced release not located** (`2610.41970`), and **none** (`2610.40917`, `2610.41868`, `2610.42053`, `2610.41688`).
  No entry describes an unverified artifact as open source.
- **Author-versus-independent separation.** Every number in all 14 entries is marked author-reported; no paper in this
  batch has an independent reproduction. Simulation-only evidence is stated as such for `2610.41130`, `2610.41609`,
  `2610.41857`, `2610.41970` and `2610.40748`; digital-agent-only for `2610.40747`, `2610.41025` and (partly) `2610.40917`;
  real-robot evidence for `2610.40799`, `2610.41216` (1.5), `2610.42053` and `2610.41868` is author-reported.

## Operational notes and blockers

- **A four-day stall does not mean a thin batch; it means a bunched one.** The backlog arrived as 122 withheld records
  plus a full 361-record day, and it carried 14 qualifying papers against the 2 and 10 that three consecutive
  re-screens of 815 unchanged records could find. Waiting for the frontier to move was the right call, and the 14
  inclusions are the return on it.
- **The previous run's "evening tail" framing was wrong and would have mis-screened 25 records.** The 122 added 10-29
  records have IDs above the old maximum but `published` timestamps going back to `00:44:29Z`; 25 of them predate the
  batch cutoff the previous run recorded. Reasoning from ID order alone would have assumed they were all late-evening
  submissions. **Set-difference against the previous run's on-disk harvest is the only reliable test**, and it is only
  available because earlier runs left their XML in `.scratch/`.
- **A repository URL inside the full text is not the paper's repository.** Two papers in this batch cite a third-party
  GitHub repository that looks like an artifact (`SenseNova-RoboRSI` in CaPtain, `deepseek-harness` in META-Robot).
  In CaPtain's case the real release was in a `\metadata` block that a plain text dump renders as the words "Project
  Page · Code · Models & Data"; recovering it required reading the HTML anchors, not the text.
- **"We release all code, data and model weights" is a claim, not a status.** Both `2610.40799` (true) and `2610.41609`
  (404) say something of the sort. The only check that settles it is resolving each URL.
- **`/tmp` is still non-durable across tool calls**, confirmed again this run; `.scratch/arxiv-2026-11-02/` (untracked,
  never committed) holds every response.
- **OpenSSH workaround still required.** `git fetch`/`ls-remote` fail with `Bad owner or permissions on
  /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf`; `GIT_SSH_COMMAND='ssh -F /dev/null'` restores access with the user's
  key and `known_hosts` unaffected.
- **Still owed: 2026-10-31, 2026-11-01 and 2026-11-02.** The next run must query all three by `submittedDate` again. The
  2026-10-30 batch is now fully screened and should **not** be re-screened wholesale.
