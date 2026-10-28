# Daily arXiv scan — 2026-10-28 (2026-10-27 batch plus the 2026-10-26 late tail: 589 previously unscreened records)

## Scope

- **Interval owed.** The previous successful run started **2026-10-27 12:53 UTC**, finished at **14:09 UTC** on `main` at
  `01e325c` with record `sources/daily-arxiv-2026-10-27.md`. It screened 2026-10-24 → 2026-10-26 (949 records) and
  recorded **0** records for **2026-10-27**, because that day's batch had not been indexed yet. This run started
  **2026-10-28 10:20 UTC** on `main` at `01e325c`, so the interval it owes is **submission dates 2026-10-27 → 2026-10-28**,
  plus **whatever of 2026-10-26 became visible after the previous run's cutoff**.
- **Both halves of that interval turned out to be non-empty, and the second one is the operational finding.**

  | query (one OR-query over cs.RO/cs.AI/cs.CL/cs.CV/cs.LG per day) | 2026-10-26 | 2026-10-27 | 2026-10-28 |
  | --- | ---: | ---: | ---: |
  | `submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` | **603** | **456** | **0** |
  | `lastUpdatedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` | **603** | **456** | **0** |
  | recorded by the previous run | 474 | 0 | — |

- **The 2026-10-26 batch grew after it was screened.** It now holds **603** records against the 474 the previous run
  harvested. Split by the previous run's cutoff (`published ≤ 2026-10-26T18:00Z`): **494** records were visible then and
  **109** carry a later publication stamp (IDs `2610.36034`–`2610.36419`). Counted the other way — by ID above the
  previous run's recorded frontier `2610.35862` — **133** records are new. The two counts differ because arXiv assigns
  the ID at submission while `<published>` moves with announcement, so an ID above the old frontier can still carry an
  earlier stamp. **The 133-record ID criterion is the one used here** for deciding what has never been screened; the
  20-record gap between 494 visible and 474 harvested cannot be reconciled further, because that record kept counts and
  not its ID list. Consequence: the previous run's day totals for a **live** day should be treated as a lower bound, and
  the frontier probe (below) — not the day count — decides whether a batch is closed.
- **The frontier has moved 1,806 IDs.** An unrestricted descending `cs.RO` query now returns **`2610.37668`**
  (`2026-10-27T17:59:59Z`) as the newest record, against `2610.35862` (`2026-10-26T17:59:11Z`) at the previous run.
  2026-10-27 is therefore **closed** (last record at 17:59:59Z) and 2026-10-28 is **not yet open** — it returns 0 in both
  date fields at 10:20 UTC, which is the normal mid-day state and not a zero-submission day.
- **The batch screened.** **589 unique records** never screened by any run: the **133** late-tail 2026-10-26 IDs
  (`2610.35864`–`2610.36419`) plus all **456** of 2026-10-27 (`2610.36420`–`2610.37668`, one contiguous ID range, so no
  gap is missing). `published` spans **2026-10-26T18:25:42Z → 2026-10-27T17:59:59Z`. The 470 records of 2026-10-26 at or
  below the old frontier belong to the previous run's screen and were not re-screened; a keyword sweep of that window
  confirmed its on-topic items (`2610.35767` Free to Look, `2610.35349` MarkovSim, `2610.35316` RSIV-Harness) are already
  recorded there.
- **Coverage.** All 589 carry at least one of the five target categories (verified programmatically). Primary category
  counts: `cs.LG` **138**, `cs.CV` **125**, `cs.AI` **96**, `cs.RO` **76**, `cs.CL` **54**, `stat.ML` 15, `cs.SE` 14,
  `cs.CR` 10, `cs.IR` 8, `cs.MA` 6, `cs.SD` 5, with the remainder single papers in `eess`, `physics`, `q-bio`, `cs.DC`,
  `cs.HC` and `cs.NI`. `cs.RO` appears as primary or cross-list on **84** records.
- **Revisions — and a correction to how this repository detects them.** `submittedDate` and `lastUpdatedDate` totals are
  identical on both days (456/456, 603/603), which earlier records read as "no older paper was revised". That inference
  is **wrong**: the previous run's record asserted it explicitly, and a direct test shows `lastUpdatedDate` on this API
  tracks the **original submission date, not the latest version date**. `2610.35817` (ZAP) carries `<updated>`
  `2026-10-27T16:21:26Z`, yet `id:2610.35817 AND lastUpdatedDate:[20261027000000 TO 20261027235959]` returns **0** while
  the 2026-10-26 range returns **1**. Equality of the two totals therefore proves nothing about revisions, and revisions
  of pre-window papers are **not discoverable through either date field**.
  The reliable check used instead: fetch current version metadata for **all 541 arXiv IDs listed in `README.md`** in
  `id_list` batches and flag any `v≥2` whose `<updated>` falls in **2026-10-26 → 2026-10-28**. Exactly **two** qualify —
  `2609.01487` (Defense-as-Skill, v4 on 2026-10-27) and `2610.35817` (ZAP, v2 on 2026-10-27). Within the 589 screened
  records, **26** carry `<updated>` > `<published>`, all same-batch v2s of papers submitted in this window; the only ones
  near this list are `2610.35817v2` (ZAP, already listed), `2610.35686v2` (FISRF), `2610.35835v2` (I-HITL) and
  `2610.35864v2`/`2610.35866v2` (benchmarks), none of which changed a listed claim.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`, saved under
  `.scratch/arxiv-2026-10-28/` (untracked, never committed). One OR-query per day across all five categories, paginated at
  200, sorted by `submittedDate` ascending: 2026-10-26 took four pages (200+200+200+3), 2026-10-27 three (200+200+56),
  2026-10-28 zero. **10 harvest requests** + 1 frontier probe + 6 day-count requests + 3 `lastUpdatedDate` semantics tests
  + 6 `id_list` version batches (541 IDs): ~26 requests at 5–6 s spacing, **zero HTTP 429**.
- **Screening.** (a) Every one of the 589 titles was read, in three passes. (b) Abstract review of **40** shortlisted
  records. (c) Full-text HTML search of **26** records for release statements, repository URLs and result tables — this is
  what separated "claims a link" from "ships code". (d) Artifact verification against live sources: the GitHub REST API
  for **6** repositories (licence, size, creation/push timestamps, stars, top-level tree), **3** project pages fetched for
  HTTP status and content, and **1** Zenodo DOI resolved through both the DOI resolver and the Zenodo API.
- **Deduplication.** Every shortlisted ID, title and project name was grepped across `README.md`, `docs/`, `sources/` and
  `editorial/` — none was present. Name collisions checked explicitly: **BiPA** (no conflict), **Concordat** (existing
  Concordat-style entries are different systems), **kw-bench** vs the listed `kw`-family benchmarks (unrelated), and
  **VISTA** (`2610.35051`, listed) vs the visual harness `2610.02200` (listed) — neither is touched here.

## Included (13)

### General Harness Design and Self-Improvement (9)

- **Context Is a Pipeline, Not a Window** (`2610.36114`, cs.SE/cs.AI; 5 pages) — why it qualifies: it makes **tool-output
  attrition** a measured harness quantity, which no entry on this list does. In a fault-diagnosing agent over Linux kernel
  traces the context window is almost empty (largest prompt in 720 runs = **5.75%**), yet **3,827 of 11,189** tool results
  are cut before the model sees them, and because the harness slices JSON text at a fixed length every cut result becomes
  **broken JSON** — one field survives **4.7%** of calls while another in the same results survives **99.3%**, so which
  evidence arrives depends on where in the trace the fault started. Controlled replay of **864** real tool results
  (including a production trace whose cut deletes all 128 fault records) shows the cost surfacing **as extra work rather
  than wrong answers**: **43%** more tool calls, twice the computation, yield **0.61 → 0.31**; a survey of **seven**
  frameworks finds **two** cutting the same unsafe way, and the authors leave causal attribution to accuracy open.
  **Verified open (2026-10-28):** Zenodo `23614755` resolves to a real replication package — resource type
  `software`, one **74.4 MB** `context-pipeline-artifact.zip`, **MIT**, published 2026-10-24 — that is
  **anonymised for ICSE 2027 NIER double-anonymous review** — public code, withheld authorship. Digital agents; no robot
  experiment; author-reported.
- **CoHARL** (`2610.36469`, cs.LG) — why it qualifies: it is the cleanest instance of **policy–harness co-evolution in one
  loop**, with the argument stated precisely (optimizing a harness for a fixed policy ignores the policy's drift; training
  under a fixed harness freezes the interface that produces the experience). Population-based RL over a shared policy plus
  a **population of executable harnesses**; a proposer revises harness code from execution traces during training.
  Reported: **27.6%** mean resolved on SWE-rebench, **+6.4 points** over harness-only search, and beats both
  *search-then-RL* and *alternate-with-one-harness* at matched budgets. Digital coding agents; **no artifact located
  (2026-10-28)**.
- **RuHarness** (`2610.36826`, cs.AI) — why it qualifies: it moves harness self-evolution's failure into the
  **supervision** — a static rubric keeps optimizing weaknesses already gone — and co-evolves a **rubric** with the harness
  over a six-coordinate representation (Equip, Instruct, Act, Observe, Remember, Terminate) with the task objective fixed.
  Reported on EvoAgentBench across five domains: **+1.7 held-in / +4.5 held-out** (Qwen3.5-35B-A3B) and **+2.7 / +5.3**
  (GLM-4.7-Flash) over the strongest baseline, i.e. the gain grows on unseen tasks. Digital agents; **no artifact located
  (2026-10-28)**.
- **Your Policy Is Secretly a Harness** (`2610.37255`, cs.LG/cs.CL) — why it qualifies: it supplies the theory that makes
  a fixed-model harness comparison interpretable. Under KL-regularized control the **log odds of an optimal policy recover
  the harness's own action preferences**, so deployed return = **harness ceiling − policy mismatch**; training moves only
  the mismatch and changing the harness moves both terms. Evidence: in MiniWoB++ **reversing the order of two observation
  fields** leaves every optimal decision unchanged yet drops success **85.0% → 19.2%**, while every harness a fixed-model
  comparison rejects but that raises the ceiling *does* win once the model trains under it. This is the strongest available
  argument that a VLA/planner benchmark measures the pair, not the policy alone. **Verified open (2026-10-28):** MIT
  `2020-qqtcg/HRP` (21.8 MB, created 2026-10-27, pushed 2026-10-28; `agents/`, `environment/`, `src/`, `scripts/`).
  Digital agents; no robot experiment.
- **kw-bench** (`2610.36405`, cs.AI/cs.SE) — why it qualifies: it tests harnesses on **investigative knowledge work** with
  **deterministic grading and no LLM judge**, on a five-year VAT reconstruction across three SAP generations delivered as
  raw system extracts. Four harnesses on one model (DeepSeek v4.1 Flash, two hours): model-agnostic **OpenCode 0.542** and
  **micro-cc 0.528**, both lab harnesses **0.00 each** away from their home model — cause diagnosed as **inertia** — and
  the same task collapsing to **0.9986 / 1.000** for micro-cc and Claude Code on Claude Opus 5.5 in **22 and 25 minutes**.
  The clean statement that *below the frontier the harness decides how far a model gets*. **Verified open (2026-10-28):**
  MIT `GSequist/kw-bench` (6.1 MB, created 2026-10-26; `datasets/`, `agents/`, `vat_chain/`, `forensic/`, `scripts/`,
  NOTICE) plus the MIT `GSequist/microcc` harness. Digital agents; no robot experiment; author-reported.
- **BiPA** (`2610.37201`, cs.AI) — why it qualifies: it is the **negative efficacy result** for skill optimization, and its
  control is the design — replaying each optimizer's acceptance rule on candidates **identical to its seed** to measure
  what selection alone reports. Across 3 optimizers × 3 agentic benchmarks × 2 models the learned skills match their
  hand-written seed (**+0.1 points**), dev gains vanish on test, a best-of-several rule over **≤30 tasks** reproduces the
  same gains **with no change at all**, and cost rises **~16%** in input tokens because added rules become extra turns
  that re-send the conversation. **Verified public (2026-10-28):** `valleysprings/BiPA` (2.8 MB, created 2026-10-15,
  pushed 2026-10-28; `optimizers/`, `skills/`, `src/`, `tests/`) — real code, but the repository **declares no licence**,
  so it is source-available rather than open. Digital agents; no robot experiment.
- **Monitor Accuracy Isn't the Answer** (`2610.36544`, cs.AI/cs.IR; 29 pages) — why it qualifies: it replaces accuracy
  with a **decision-value** criterion for the monitors that decide when to restart an agent, showing analytically that a
  fixed **90%**-accuracy monitor can swing from **−5 to +35 points** purely on the replacement attempt's residual-budget
  success probability. Contributions: a **decision-sufficiency** criterion (binary-action Blackwell) for whether a progress
  representation preserves what the choice needs, and a **model-free audit** over existing traces with simultaneous
  binomial intervals that may **abstain**. Empirical half: **194** executions on 19 SWE-bench Verified SymPy issues,
  130-trace retrospective audit, a **2.29-point** apparent restart gain traced to one surviving failed attempt. Digital
  agent on `mini-swe-agent`; **no artifact of the study (2026-10-28)**.
- **When to Intervene?** (`2610.36471`, cs.AI) — why it qualifies: it makes **self-recovery** the quantity that gates
  intervention, which is the missing half of every recovery runtime on this list. **ReInBench**: 1,259 traces and 1,403
  error events over computer-use and web tasks; **725** errors self-recover, usually within a few steps, varying by
  execution model, and recovery predicts better handling of later same-type errors and better completion. **ReCoF**
  predicts self-recovery and reports **+9.02%** relative Macro-F1 over the strongest baseline. Digital agents; **no
  first-party artifact (2026-10-28)** — only third-party OSWorld trace dumps are linked.
- **Concordat** (`2610.37461`, cs.AI/cs.MA) — why it qualifies: it makes the permission layer an **enforced type** rather
  than prompt text — a finite-state protocol over tools, out-of-state calls rejected **before side effects**, human
  decisions only *requestable*, budgets split across handoffs, and subclasses **rejected** if they could bypass the parent
  protocol (behavioral Liskov). Evaluated on tau-bench's 50 airline tasks with a simulated customer who pressures the agent
  to skip confirmation: **13/50** (gpt-4.1) and **29/128** task-runs (Claude Haiku 5.5) violated confirmation for the
  standard agent, **0** for Concordat, at no significant success cost — while **tau-bench's reward missed most violations**.
  **Verified open (2026-10-28):** Apache-2.0 `miladsikaroudi/concordat` (357 KB; `src/`, `tests/`, `bench/`, `docs/`,
  `examples/`, `paper/`). Digital agents; no robot experiment; author-reported.

### Agentic Robot and VLA Harnesses (3)

- **When Black-Box Robot Policies Fail** (`2610.36852`, cs.RO, CoRL 2026 SPAIS workshop) — why it qualifies: it is a
  **runtime monitor and input-repair contract around a frozen policy**, which is the deployment shape this list keeps
  finding. A structured surrogate separates **periodic gait** from **sensor-dependent corrections**, so the residual is
  attributable to a channel and a repair target exists. Simulated Go2, frozen locomotion policy: offline diagnosis
  **259/640 → 429/640**, online repair **382/640**, **no triggers on 64 held-out fault-free episodes**; the matched
  boosted-tree surrogate that predicts clean actions equally well produces **no repair improvement**, so the gain is the
  decomposition, not the predictor. Separate fits also help Go1 and ANYmal C. Authors' own limits: **one policy, partly
  screened simulated faults, no RFM- or hardware-level validation** — runtime contract, not field capability.
  **Simulation only**; **no artifact located (2026-10-28)**.
- **Which Hand Was Late?** (`2610.37533`, cs.RO/cs.AI) — why it qualifies: it answers the channel question every
  failure-recovery runtime assumes — **what each sensing channel actually lets a diagnoser identify**. On a simulated
  bimanual tool-use task: the slip shows on the tool hand; **which hand was late needs both arms**; one hand reads its own
  delay and at most part of the other's; and a weak push combined with a late tool hand is **not identifiable from passive
  sensing**. Design consequence: within two attempts, a repair policy that **escalates when the cause is unidentifiable**
  repairs as many failures as one told the true cause. Also: a VLM's hidden state carries the late hand almost perfectly
  but along a direction no closer to its answer's than random, and verbalization depends on **question wording**.
  **Simulation only**; **no artifact located (2026-10-28)**.
- **RoboAuto** (`2610.36533`, cs.RO/cs.AI/cs.CL/cs.CV/cs.LG) — why it qualifies: it attacks the weak premise of
  code-as-policy — that execution success alone is enough feedback — with a hierarchical knowledge base, scene grounding
  of action-critical quantities, and **multi-stage checking with self-designed experiments**. Reported: LIBERO-PRO **89.1%
  vs 71.7%** (ASPIRE), long-horizon **73.4% vs 30.8%**, LIBERO-Plus **90.3% vs 44.7%**, Robosuite **90.6% vs 81.0%**.
  **Artifact status literal (verified 2026-10-28): project page only.** `roboauto-ai.github.io` is live with a real-robot
  section, but its Code control is a disabled `href="#"` reading **"Code coming soon"**; no repository, weights or dataset
  is linked. Simulation evidence for the numbers above; author-reported.

### Safety (1)

- **Program-Level Adversarial Co-Adaptive Self-Play for Robotic Security** (`2610.36143`, cs.RO; 8 pages) — why it
  qualifies: it makes robot security an **adversarial program co-adaptation** problem with executable evidence on both
  sides, which no safety entry here does. Two coding agents revise attack and defense programs from tested opponent code
  plus mission feedback, no fine-tuning, over an autonomous-driving stack and an SDK-level humanoid locomotion stack.
  Two **eight-generation** case studies: a driving defender adds **direct-actuation recovery**, a humanoid defender revises
  **incoming-state validation** — and the honest result is that these are **local task recoveries, not sustained
  robustness**. **No artifact located (2026-10-28)**; software/simulation-level study, author-reported.

## Revision handling (1 changed entry)

- **Defense-as-Skill** (`2609.01487`) — **artifact release, entry updated.** The README entry recorded "no official
  artifacts located as of 2026-09-02". The paper body of **v3 and v4** (v4 updated **2026-10-27**, found through this
  run's version sweep of all 541 listed IDs) links `github.com/yxf203/defense-as-skill`, which the GitHub API confirms is
  the **official implementation, MIT, created 2026-10-09, pushed 2026-10-21, 575 KB**, with `config/`, `eval/`,
  `evolution/`, `run_mcts.sh`, `run_smoke_loop.sh` and `tests/`, and a README that names the NeurIPS 2026 paper. The link
  is absent from the abstract page, which is why earlier runs read the artifact as missing. Code, no weights or dataset.
- **ZAP** (`2610.35817`, v2 on 2026-10-27) — **no entry change**: the v2 adds a citation to the third-party
  `robot-agent-exploration/rax` harness (Apache-2.0, "RAX harness: coding agents that learn robot tasks by online
  exploration"), not a ZAP release, so "no artifact located" remains correct. Recorded here so a later run does not read
  the new URL as a release.

## Rejected, deferred and watch list

- **Deferred as on-scope but not yet qualifying** (each was read to abstract level; none needed an artifact check):
  **Where Should the Evaluation Budget Go in LLM-Driven Agent Scaffold Evolution?** (`2610.37276`, cs.LG/cs.AI) —
  overlaps the evaluation-budget line already covered by ActiveSaddler (`2610.00906`) and Task-CoEvolve (`2608.20169`);
  **A Harness Reducing over-Defensive Writing** (`2610.36717`, cs.CL) — harness that isolates reviewer/filter/writer, but
  the evidence is 25 business drafts with no artifact and a writing-domain claim; **Learning to Code the World**
  (`2610.37046`, cs.CV, SceneEvolve) — harness evolution with a fixed model, but the domain is compositional 3D
  reconstruction rather than an agent or robot runtime; live project page, no repository located; **Evolving Agent Teams**
  (`2610.36848`, cs.AI) — multi-agent restructuring, and the repository named in the text (`caoshiyi/EAT`) **returns 404**;
  **Skill Dreaming** (`2610.36188`, cs.AI) — skill libraries from PR streams, digital SWE only, no artifact;
  **GEPA-Curator** (`2610.37651`, cs.CL) — experience curation for prompt optimizers, no artifact;
  **Scaling Laws for Tool-Use Agents** (`2610.37648`), **EnvSched** (`2610.37609`), **Spine** (`2610.37224`), **Stratum**
  (`2610.36153`), **TrawMem** (`2610.36918`), **Heads of Security** (`2610.37657`, a model-level attention-gate defense,
  not a harness) — infrastructure or technique papers whose runtime contract is not the object of study.
- **Deferred as model/pipeline contributions on the robot side:** **Best-of-N for World Action Models** (`2610.37196`) —
  a test-time-selection measurement protocol with 100k+ branched rollouts, interesting but a WAM evaluation result;
  **Retrieve to Act** (`2610.37215`), **Kinematic Constraint-Referenced Evaluation** (`2610.37550`),
  **Rollback** (`2610.36068`), **Just Action Models** (`2610.37219`), **E0.5** (`2610.36011`), **RynnWorld-Latent**
  (`2610.36791`), **TAPESTRY** (`2610.36823`), **Trace-WAM** (`2610.36840`), **GAWM** (`2610.37091`), **World Dreamer
  v5-lite** (`2610.37518`), **SWIM** (`2610.36662`), **OmniBrain-0** (`2610.36013`), **ASPEN** (`2610.36285`),
  **HIDRA** (`2610.36374`), **SO-101 Bench** (`2610.36420`), **OpenJaws** (`2610.36598`), **VLA-Stem** (`2610.37378`),
  **On Shaky Ground** (`2610.36725`), **Remember Where You Walk** (`2610.37203`), **When, Where, and What to Correct**
  (`2610.37547`) — the VLA/WAM/hardware line. Each is a policy, model or benchmark contribution whose runtime interface is
  not the contribution; none was artifact-checked.
- **Design paper, excluded for the policy on unevaluated proposals:** **Making Self-Driving Laboratories Auditable**
  (`2610.36761`, cs.RO/cs.AI) — proposes LAM + OpenSDL + Safe-SDL + L3-Bench, and a full-text read found **no experimental
  results section**; the Safe-SDL runtime gate and the L3-Bench protocol are the reusable part if a release or evaluation
  appears.
- **Read and excluded as off-scope (representative, out of 589 titles):** autonomous-driving control and traffic
  forecasting (`2610.36079`, `2610.37011`, `2610.37310`, `2610.37403`, `2610.37470`), medical imaging, clinical agents and health text
  (`2610.36298`, `2610.36513`, `2610.36517`, `2610.37081`, `2610.37459`), video/image generation, editing and
  reconstruction (`2610.36371`, `2610.36729`, `2610.36896`, `2610.36978`), perception, SLAM and calibration
  (`2610.36312`, `2610.36507`, `2610.36853`, `2610.37543`), humanoid/quadruped hardware and control design
  (`2610.36789`, `2610.36996`, `2610.37452`, `2610.37582`), bio/chemistry/materials ML, and the large LLM remainder
  (grokking, quantization, KV-cache engineering, RLVR dynamics, RAG retrieval, ASR/TTS). Excluded per the standing rule:
  a paper enters only if it contributes a reusable agent/VLA harness, recovery, safety or evaluation contract.

## Validation performed

- **Markdown structure.** All 13 new entries are single-line list items under an existing `###` heading in the established
  `[Title](primary link) ([code](…), [project](…)) — …` shape; blank lines separate every block from the next heading; no
  heading level was added or removed. One existing entry (`Defense-as-Skill`, line 1165) gained a link and a rewritten
  artifact sentence. The Contents list is unchanged because no section was added; the Contents blurb was rewritten for
  this run instead of left describing the 2026-10-27 pass.
- **`git diff --check`.** Clean — no whitespace errors, no conflict markers, no trailing whitespace introduced.
- **Links.** Every added URL was fetched during verification: arXiv abs pages (13) and full-text HTML (26), GitHub
  repositories and top-level trees (6), one project page with its raw HTML inspected (`roboauto-ai.github.io`), and one
  Zenodo DOI resolved twice (resolver + API). **Two links were deliberately not added** because they do not resolve or are
  not first-party: `github.com/caoshiyi/EAT` (404) and the `robot-agent-exploration/rax` harness (a cited third-party
  dependency, not a release of the paper citing it).
- **Dates and categories.** Every entry states its arXiv ID, primary category and date; all 13 are 2026-10-26 or
  2026-10-27, inside the owed window. Artifact checks are stamped **(2026-10-28)**. Section assignment: 9 general-harness,
  3 robot-harness, 1 safety.
- **Cross-file consistency.** The README badge and `Last verified:` now read **2026-10-28**, matching this record's
  filename date. No duplicate: all 13 IDs and titles are absent from `README.md`, `docs/`, `sources/` and `editorial/`.
- **Artifact-status discipline.** Four statuses are recorded literally and distinctly: **verified open with a licence**
  (kw-bench MIT, HRP MIT, Concordat Apache-2.0, Defense-as-Skill MIT), **public without a licence** (BiPA), **anonymised
  replication package** (Context Is a Pipeline → Zenodo, ICSE 2027 double-anonymous), and **project page only / no artifact
  located** (RoboAuto "Code coming soon"; CoHARL, RuHarness, Monitor Accuracy, When to Intervene, the two robot-diagnosis
  papers and the robotic-security paper have nothing). Simulation-only or digital-only scope is stated per entry, and every
  numerical result is labelled author-reported.

## Operational notes and blockers

- **`lastUpdatedDate` does not detect revisions — use `<updated>` from harvested records, or an `id_list` version sweep.**
  This run found the previous record's revision argument unsound and replaced it with a version sweep of all 541 listed
  IDs, which is also what surfaced the Defense-as-Skill release. Keep that sweep: it is ~6 requests.
- **A day's count is not final until the frontier passes it.** 2026-10-26 was screened as 474 records and later grew to
  603; 109 of the difference carry a publication stamp after 18:00Z the same day. Re-derive the screened set from the
  previous run's recorded frontier ID where possible, and state which criterion (ID or timestamp) was used.
- **arXiv IDs and `<published>` disagree at batch edges.** Late-announced records keep IDs above the old frontier while
  carrying earlier stamps; do not use ID order and timestamp order interchangeably when defining what is new.
- **`.scratch/` is the durable workspace; `/tmp` is not.** All XML, HTML, JSON and API responses were written to the
  untracked `.scratch/arxiv-2026-10-28/` and never committed (40 MB of HTML, including one wrong-version fetch).
- **OpenSSH workaround still required.** `git fetch`/`ls-remote`/`push` fail with
  `Bad owner or permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf`; `GIT_SSH_COMMAND='ssh -F /dev/null'`
  succeeds and is what this run used for `ls-remote` and `push`.
- **Rate limiting was not an issue.** ~26 export-API requests at 5–6 s spacing, no 429; neither the GitHub nor the Zenodo
  API rate-limited.
- **No blockers.** No network, authentication, conflict or artifact-verification problem prevented a step.
