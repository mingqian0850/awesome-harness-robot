# Daily arXiv scan — 2026-09-30

## Scope

- **Interval:** everything announced after the 2026-09-29 run's cutoff. That run
  (started ~08:16 UTC on **Tuesday 2026-09-29**, committed 10:4x local) screened
  the announcement block whose OAI datestamp is **2026-09-29** — 3,447 unique base
  IDs, newest base ID `2609.35770`. This run started ~08:41 local / ~06:41 UTC on
  **Wednesday 2026-09-30**. **There are no missed dates:** the 2026-09-29 18:00
  catch-up slot could not have seen a new block (the next announcement lands at
  02:00 local on 2026-09-30), and no run intervened between the two.
- **A new announcement block landed.** The OAI-PMH harvest for
  `from=2026-09-30&until=2026-09-30` returned records for all five categories:
  **2,425 memberships = 1,724 unique base IDs**, every one carrying datestamp
  **2026-09-30** (memberships: cs.AI 749, cs.LG 724, cs.CV 412, cs.CL 337,
  **cs.RO 203**). This is the Tuesday-submission batch, about half the size of the
  Monday block that preceded it and roughly twice the 2026-09-28 block.
- **Window and delta.** The wide window `from=2026-09-29&until=2026-09-30` returns
  **7,099 memberships = 5,040 unique base IDs** across two datestamps: **3,316** at
  2026-09-29 and **1,724** at 2026-09-30. The 2026-09-29 datestamp is now 3,316,
  i.e. **131 fewer than the 3,447 the previous run recorded for that block** —
  re-announcement moves a record's datestamp forward, so those 131 now appear under
  2026-09-30 without being new work. Against the previous run's parsed window
  (4,313 unique IDs, 2026-09-28…2026-09-29) the window shifted rather than grew:
  **1,579 IDs appeared and 852 disappeared**, and the 145 records with field changes
  after the entity-normalisation control are dominated by `created`/datestamp churn
  from re-announcement. **The newest base ID moved from `2609.35770` to
  `2609.38180`**, so the ID frontier advanced by 2,410 and the block is not being
  held back.
- **Block composition.** 1,281 of the 1,724 records carry a 2026-09-29 created
  date; the remaining 443 run back through September (400), August, July, June,
  May, March, January, 2026, and sparsely to 2018 as re-announced replacement
  versions. **377 records carry pre-`2609.` IDs** and **1,266 carry `2609.3xxxx`
  IDs**; **203 records carry cs.RO**.
- **Withdrawal notices in this block: three, none repo-relevant.**
  `2607.12896` (UniMedSeg; publisher-specific template used before acceptance —
  already recorded in the 2026-09-29 record), `2608.25466` (Homo-RAG; incorrect
  version uploaded), and `2609.35674` (Oracle-bone-character study posted as an
  incomplete draft). No ID referenced anywhere in this repository was withdrawn,
  so no entry required annotation.
- **Export-API boundary check confirms the indexing lag, not a missing block.**
  With `submittedDate` percent-encoded and `curl --globoff`, the HTTPS range query
  for **cs.RO on 2026-09-30 returns HTTP 200 with a well-formed empty Atom response
  (0 entries, `totalResults` 0)** at ~06:50 UTC, as does cs.AI for the same date.
  The positive control on **2026-09-29 cs.RO returns HTTP 200 with 111 entries**,
  and a no-date-filter liveness control returns cs.RO `totalResults` **59,445**, so
  the endpoint is healthy and the new block is simply not yet indexed. The
  **OAI-PMH harvest is the authoritative source** for every number above.
- **28 of the 1,724 new-block IDs already appear somewhere in the repository**
  (README, `docs/`, `sources/`), 7 of them at README level. Because an OAI
  datestamp of 2026-09-30 means "announced in this block", these are
  re-announcements of records curated earlier; each was checked for a revision that
  changes its recorded status (see "Revision edits"): **three did** — Guava
  (`2606.18363`, retitled and code released), Agent as Policy (`2609.12541`,
  artifacts released), and HarnessVLN (`2609.15195`, v3 with an anonymized code
  repository and revised numbers).
- **Watch-list re-check:** **none** of the thirteen carried watch IDs
  (`2609.24274`, `2609.24350`, `2609.24563`, `2609.22587`, `2609.21223`,
  `2609.21502`, `2609.22218`, `2609.22247`, `2609.26121`, `2609.26520`,
  `2609.25558`, `2609.29091`, `2609.29065`) appears in the 2026-09-30 block; they
  are carried forward unchanged. Of the nine watch entries opened by the 2026-09-29
  run, only **F4R** (`2609.35575`) reappears, as **v2 (2026-09-29)**; its project
  page is live but still carries **no code link**, so it stays on watch. The
  artifact placeholders recorded on 2026-09-25 (Maithili/GAP's 1 KB repository,
  HEXIS's HTTP 401 anonymous link, the RoboRecover dataset "being prepared for
  Hugging Face", and HarnessPAI's "not yet publicly released" code) are unchanged.

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`) over three windows: `2026-09-30…2026-09-30` (new
   block), `2026-09-29…2026-09-29` (late-batch/boundary re-check), and
   `2026-09-29…2026-09-30` (previous block plus new block). Pages saved to
   `.scratch/arxiv-2026-09-30/oai-{new,prev,full}-*.xml`; cs.AI and cs.LG needed
   two pages in the `prev` and `full` windows, every other category and window
   returned a single complete page with no resumption token.
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`oai-records-{new,full}.json`, `parse_oai.py`), then **delta against the
   previous run's parsed harvest** with field-wise before/after values
   (`delta.json`) and the entity-normalisation control.
3. **Export-API boundary cross-check** over the 2026-09-29 and 2026-09-30
   `submittedDate` windows, over **HTTPS**, with a no-date-filter liveness control
   and percent-encoded range syntax. Result recorded under Scope.
4. **Screening passes with five distinct emphases** over the 1,724 new records:
   (a) a **harness-layer** sweep (harness, scaffold, agent loop, runtime,
   middleware, tool call, skill discovery, code-as-policy, execution trace, replay,
   rollback; 130 records scored ≥2 on harness or self-improvement terms);
   (b) a **robot/embodied** sweep (172 records); (c) a **VLA / robot foundation /
   world-action** sweep (128 records); (d) an **eval/safety** ranking (305 records
   with ≥5 hits); and (e) an **artifact-signal** listing of every uncurated record
   naming a concrete release object, repository URL, "we release" sentence, or
   project page (368 records). `screen.py` / `triage.py`; outputs in
   `sweep-*.txt` and `triage-*.txt`.
5. **Dossiers and primary-source verification** for 30 shortlisted records by
   **six parallel verification agents**: arXiv export-API version history and
   dates, the arXiv abstract page and full-text HTML (or the PDF where HTML is
   unavailable, as for `2608.28795`), official project pages, the GitHub API
   (license, size, creation and push dates, default branch, stars, top-level tree)
   and the Hugging Face Hub API (gating, license, last modified, downloads) — all
   on **2026-09-30**, with raw captures under
   `.scratch/arxiv-2026-09-30/{xml,abs,html,pages,gh,ghpages,hf,external}/` and
   per-batch dossiers `dossier-{A…F}.md`. Nothing was taken from a secondary
   source.
6. **Duplicate screening** of every candidate against `README.md`, `docs/*.md`,
   and `sources/*.md` by arXiv ID, project name, and repository URL before
   writing. Three name collisions were checked and cleared: "VACE" and "ProAct"
   match only as substrings elsewhere, and the earlier "Encore" (`2609.04249`,
   audio-video generation) is a different paper from `2609.37359`.
7. **Revision screening of the 28 already-referenced IDs**, extended by hand for
   the seven that are README-level entries: each was re-opened on arXiv and its
   artifact links re-fetched (see "Revision edits").
8. **Watch-list re-check** by ID against the new block and the wide window (see
   Scope).

## Included (README updates)

Nineteen new entries in seven README sections, plus three revision edits. Artifact
status is reported literally; every repository, license, dataset, date, project
page, and tree claim below was verified live on **2026-09-30**.

### Harnesses and Development Platforms — General Harness Design and Self-Improvement

- **Harness Evolution as Learning** (`2609.36892`, cs.AI/cs.CL; v1 2026-09-29;
  Renmin University of China): formalizes harness evolution as a **learning
  problem** with the model frozen, decomposing its limits into approximation
  (context-reachable policies stay in the convex hull of the pretrained prior,
  control-reachable ones need not), generalization (memory capacity under finite
  interaction evidence) and optimization (biased update dynamics) errors, and adds
  **AppWorld-P**, a preference layer whose personas carry executable compliance
  rules. Reported: in-support preferences drop from 1.00 to 0.07/0.00 violations
  while out-of-support ones stay at 0.44–0.78 unless the harness computes or
  rejects the check; pooled violation rate falls from ≈0.77 at memory length 0 to
  ≈0.20 at length 10 and then **rises to ≈0.25–0.27 by length 20–150**; ACE/TEPA
  end near 0.48 against an oracle 0.071 while Reflexion stays at the no-memory
  0.881. Simulation-only (AppWorld-P), one foundation model, small held-out sets.
  **Verified open:** `ZyGan1999/self-evolving-harness-as-learning` (315 KB, created
  2026-09-02, pushed 2026-09-25) with runnable multi-seed scripts and the persona
  rule pools; **no license**. Its reproducibility statement still claims an
  "anonymous repository listed at the end of the abstract" that does not exist — a
  stale double-blind artifact, recorded as a documentation defect.
- **VACE** (`2609.37105`, cs.LG/cs.AI/cs.CL; v1 2026-09-29; Huawei ICT): alternates
  agentic RL with trajectory-driven harness refinement and accepts a candidate
  harness **only if it improves validation performance with the updated model held
  fixed**. Reported on Qwen3.5-9B: **45.26% OfficeQA / 75.19% AutomationBench**
  against 38.83/66.10 for weight-only RL and 40.67/68.25 for an ungated
  alternation baseline; the gate ledger shows **25 accepted, 17 rejected, 2 tied**
  out of 44 proposals, including a candidate falling 60.37 → 49.06 at the same
  checkpoint. Digital-agent only. **No artifact released or promised**; the authors
  flag adaptive-selection risk from repeated validation use and non-monotonic
  progress.
- **Mixture of Self-Improving Branches** (`2609.37834`, cs.AI; v1 2026-09-29; Meta,
  Duke, UC Davis): makes the **search process** the optimized object — branches
  carry evolving development subsets and branch-local proposal guidance, and a
  router selects a development-selected head per input before execution. Reported
  relative gains over Meta-Harness of **34.8%** (Math–Gemini 3 Flash), **11.6%**
  (Terminal-Bench 2.0), **3.8%** (SWE-bench Lite); held-out branch heads 56.0/58.0
  and 44.8/48.3. Digital-agent only; the authors concede unequal token usage,
  limited repeated evaluations, and routing losing to a single head in two of four
  settings. **No artifact claimed or released.**
- **SafeCoEvo** (`2609.36580`, cs.AI; v1 2026-09-29; ECNU, Shanghai Innovation
  Institute, Huawei Noah's Ark, UCL): co-evolves the safety layer at two timescales
  — S-Harness externalizes runtime feedback into safety prompt, validated memory,
  safety skills, permission policy and guard policy, while GuardVPO internalizes
  event-level experience into a parametric guard with an **asymmetric reward** that
  penalizes unsafe-as-safe far more than over-conservatism. Reported held-out
  **UOR 7.67% / TSR 81.33% / SUCR 78.33%** against 29.19/54.36/41.95 for the
  strongest evolution baseline (−10.05% unsafe outcomes, +12.15% task success over
  the strongest baseline), with the task model never retrained. Digital-agent only
  (Agent-SafetyBench + ASB). **Verified open:** MIT-licensed
  `SII-YUCHENG2002/SafeCoEvo` (4.6 MB, created 2026-09-28, 6 stars) with runtime,
  evolution graph, guard/permission artifacts and tests; the link appears only in
  §1 of the full text, not on the abstract page.
- **Meta-Skills** (`2609.38143`, cs.AI/cs.CL/cs.LG; v1 2026-09-29; Apodex + UIUC):
  freezes both Builder and Target weights and learns `when`/`provide`/`use`
  meta-skills for **constructing harnesses** out of seven components (instruction,
  memory, tools, context, controller, verification, workspace) inside a constrained
  runtime that interprets policies **without Python `exec`/`eval`** and touches the
  environment only through a three-method adapter. Reported macro-average **65.31**
  across Harness-Bench and NewtonBench versus 56.36 for a Builder without skills,
  53.29 for delivering the same bank directly to the Target, and 51.40 for the
  native environment. Digital-only. **Partial release:**
  `qiancheng-apodex/MetaSkill-AI4AI` (466 KB, real runtime, interpreter, schema and
  tests) explicitly **omits the experiment scheduler and BM25 retrieval pipeline**
  and declares **no license**.

### Harnesses and Development Platforms — Unified Evaluation

- **LongHarness Bench** (`2609.38137`, cs.CL; v1 2026-09-29; University of Alberta
  / Amii): makes the **harness the measured variable for long-context work**,
  scoring accuracy and cost across four retrieval-saturating suites (constraint
  solving, equivalent-program pairs, program execution tracing, outlier memo
  detection), 200 instances at ~128K tokens under a 3M-token budget per instance
  with exact instance accuracy. Reported across 25 model–harness combinations:
  macro-average spanning **0–68%**, best **68% for GPT-5.6-sol + mini-swe-agent at
  $0.816/instance** versus 53.5% for RLM at $4.30, no configuration leading on
  every task. Digital-agent only. **Verified open:** ungated Hugging Face dataset
  with all 200 instances, SHA-256 manifests and a verification tool, plus a GitHub
  repository with scoring and release tooling — **neither declares a license**.
- **Do Agent Benchmarks Do What They Say?** (`2609.37315`, cs.SE/cs.AI/cs.LG;
  v1 2026-09-29; Florida International University): treats a tool's advertised
  surfaces as an **executable contract** (one YAML file per tool with
  `pre.`/`eff.`/`frame.`/`arg.` clauses, each citing its provenance), checks the
  implementation with a static AST half plus a dynamic adapter half, and traces a
  score **backwards** to the verdicts that derive from state a defective tool should
  have written. Reported: **34 audited mutating tools across four benchmarks, seven
  tool defects and one evaluator property confirmed at pinned commits**, and on
  1,120 constructed paths the tau2-bench evaluator rewards refuelling a suspended
  line. The checker is mostly confirmatory — no false positive in 25 flags, most
  injected defects missed, static half alone flagging 14 of 17 confirmed sites; the
  README states seven of eight findings were surfaced by hand. Digital-agent only.
  **Verified open:** MIT-licensed `rohithreddybc/tool-contract-conformance`
  (3,495 KB, 179 commits) plus Zenodo software archive `10.5281/zenodo.22182792`.
- **More Programs or More Rolls?** (`2609.35873`, cs.AI/cs.LG/cs.SE; v1
  2026-09-26; CUHK, CASIA, others): separates **answer coverage, stable
  complementarity and usable selection**, with a same-code control of nine
  **byte-identical baseline copies** that makes the repeat-noise floor measurable.
  Reported on 386 MATH-500 tasks: **2.16 points of apparent oracle headroom among
  identical programs**, losses persisting across all three repeats on 100 tasks
  against a persistent win on one, a **frozen selector gaining 0.00 points**, and
  98.70% oracle coverage at 27 executions for both populations. Digital-agent only;
  a rigorous negative result for harness diversity at this scale. **Verified open
  but thin:** MIT-licensed `StatXzy7/harness-eval` is real Python but a
  single-commit snapshot that excludes datasets, model responses and runtime
  outputs, so its numbers cannot be re-derived from the artifact.

### Robot Agent Systems — Agentic Robot and VLA Harnesses

- **RoboSkill / Explore, Execute, Evolve** (`2609.37810`, cs.RO/cs.AI; v1
  2026-09-29): owns the **execution and session lifecycle around a coding agent
  that drives a robot** — a loopback LIBERO gateway/SDK proxy, one agent per lease
  for native Codex/Claude CLI agents, a batch runner with concurrency limits and
  retry of structural failures, a four-hour deadline with same-thread session
  resumption, deterministic process-group teardown, and a **same-session
  post-success reviewer that must emit a validated action ledger and byte-identical
  executed code**; skills are text-plus-code packages. Reported LIBERO-10 results
  across four agents plus a real Piper-arm evaluation under per-cell time budgets;
  real-robot success is human-judged and allowed to accrue across episodes, and the
  authors report first-episode success separately. **Verified open:** MIT-licensed
  `SII-dannyXSC/RoboSkill` with `libero_gateway`, tests, docs and
  `make verify-publication`, but a one-shot snapshot that excludes the evolved skill
  packages. Institutional affiliation is not confirmable from primary sources.
- **Encore** (`2609.37359`, cs.RO/cs.AI/cs.CV/cs.LG; v1 2026-09-29; MIT, 2077AI,
  Texas A&M, UW–Madison): treats demonstrations as **evidence rather than training
  data** — a deterministic distiller builds packs (multi-view keyframes, gripper
  events, frame strips, trajectory), a coding agent writes a policy program against
  a fixed API and refines it over 15 development states, then the program is
  **frozen before a sealed evaluation that never reveals the success signal**, and
  the deployed policy makes no model calls. Reported: **2,889/3,000 (96.3%)** on
  LIBERO-PRO at K=3 versus 94.9% at K=0, 89.3% on a rerun of ASPIRE (published
  71.7%), **95/450 on RoboDojo with demos versus 0 without**, 9/10 on each of two
  real bimanual Trossen WidowX tasks from five demos, and **zero on six
  contact-precise tasks in every arm**. Simulation plus real-robot evidence,
  author-reported. **Verified open:** `YIFANK/encore` ships the search harness,
  sealed-evaluation programs and per-episode results; **no license**. The paper
  discloses that a shared-note protocol leak was found and does not report the
  demo-vs-no-demo gap as an effect.
- **Skill-Space Shooting** (`2609.38178`, cs.RO/cs.AI/cs.LG; v1 2026-09-29;
  Tsinghua, UC Berkeley, Shanghai Qi Zhi): a **real-world recovery-and-improvement
  runtime** — a learned value model triggers on suspected failure, a skill is
  selected and run as a physical trial, a judge verifies the repair before judging
  task completion (≤3 retries), and only accepted repair segments are aggregated by
  DAgger, with skills shared across tasks. Reported: Drawer **0/20 → 7/20** after
  one update (DSRL ≤2/20), +55 progress points on Coffee and +63.8 on Sweeping over
  DSRL, trigger accuracy 71–83% with repair verification 70–77%, and skill sharing
  lifting a 10-demo grasp from 5% to 70%. Real-world only, few tasks, human scene
  resets. **No public artifact:** the project page is a video supplement headed
  "Anonymous ICLR 2027 Submission"; no code, data or weights.
- **ProAct-VLM** (`2609.37681`, cs.RO; v1 2026-09-29; IROS 2026): a **pre-failure
  replanning runtime** — persistent object identities from Grounded-SAM/DeAOT with
  6-DoF FoundationPose estimates maintain structured environment state, a
  change-detection threshold fires on object addition, removal or goal change, and
  the plan monitor replans **before** execution fails, with tracking near 30 Hz and
  full updates near 5 Hz. Reported on a real Franka with YCB objects: **89% success
  / 93% efficiency** for GPT-4o against 83% for OWG and 61.4% for LLM3+Image across
  seven scenarios × 10 trials, Gemini 2.5 Pro at 100% — small and author-judged.
  **Artifact status literal:** `moured/ProAct-VLM` is a README-and-images
  placeholder whose badges say "Coming Soon"; no code, weights or data.
- **Taming VLAs under Robot Execution Errors** (`2609.37334`, cs.RO; v1
  2026-09-29; POSTECH + MBZUAI): puts a **deployment-time adaptation loop and a
  fault benchmark** around a frozen VLA — the residual between commanded action and
  proprioceptively executed motion drives an online LoRA update of the action
  expert with no rewards or labels, while **RoboStress** injects Stribeck friction,
  gravity-compensation error, backlash and compliance at the correct control stage
  across seven scenarios. Reported: LIBERO averages **49.1% → 56.6%** (π0.5) and
  41.7% → 44.2% (π0), **over 30 points** average real-robot gain on each of two
  AgileX Piper arms (new versus one-year-old), 64% versus 16% on out-of-distribution
  objects. Simulation plus real robots; the authors state compensation uses
  proprioception only and is **not** a safety guarantee. **No artifact:** code and
  RoboStress are future-tense promises with no URL.

### Benchmarks and Evaluation — Manipulation and VLA

- **LIBERO-MAX** (`2609.36518`, cs.RO/cs.AI; v1 2026-09-29): makes environmental
  change a **paired evaluation contract** — 8,000 Base/Dynamic cases over eight
  online event types (geometry, observation, appearance, clutter, path) where each
  pair shares task, reset, instruction, policy seed and the **executed action
  prefix**, derived from LIBERO-Plus and LIBERO-PRO, plus an 800-pair Lite subset
  and source-stratified bootstrap intervals. Reported across 14 policies × 16,000
  rollouts: every policy losing **11.0–25.7 points** to the event, **20.8–56.1%**
  of Base successes failing afterwards, X-VLA camera Base/Reset/Mid-task
  68.2/1.3/12.4%, and Fast-WAM 43.4% → 0.3% under sensor noise. Simulation only,
  one persistent event per pair. **Verified open:** live project page and
  `liberomax/LIBERO-MAX` with the benchmark package, runtime-integration and spec
  docs, tests and a 16.4 MB case dataset; **no license file**.
- **RoboChrono** (`2609.36605`, cs.RO; v1 2026-09-29; 25 authors across Tianji
  Technology, Shenzhen University, GIM, Alibaba and others): imposes a
  **causal/streaming observation boundary** (only frames up to time *t*) on real
  robot data — 39 scenarios, 34,713 instances, 1,816 episodes, ~25 hours — with
  **seven separately scored dimensions** (current/next/goal-conditioned action,
  frame match, view match, frame order, action-time tIoU@0.5) and matched-pair
  ablations. Eighteen VLMs are evaluated zero-shot; reported GPT-6-Astra 80.5%
  choice average versus 62.8% for the best open model, 98.3% versus 68.3% for frame
  matching versus ordering, and −22.1 points from removing vision on current action
  but only −0.7 on next action. Real robot and human data, models rather than
  policies. **Verified open:** Apache-2.0 `mfan-res/ROBOCHRONO` plus ungated
  CC-BY-4.0 datasets `gimai/RC-Tianji` (31.5 GB) and `gimai/RC-GIM` (42.5 GB);
  178/242 downloads.
- **Faster and Better?** (`2609.37771`, cs.RO; v1 2026-09-29; Shanghai Jiao Tong
  University): an **evaluation-defect audit** of VLA acceleration — 22 bugs across
  RoboTwin, LIBERO, LIBERO-PRO, LIBERO-Plus, RoboCasa365, VLABench and RoboDojo in
  three classes (task consistency, initialization, reproducibility) plus four
  design limitations, with a reusable workflow of anomaly → matched-settings check
  → trajectory-versus-acceptance-region plots → code and expert confirmation.
  Demonstrated rank reversals include a LIBERO-PRO Spatial-Task baseline moving
  **0.4% → 53.2%** (last to first), a RoboTwin baseline going from 21 points behind
  to 5 ahead after mass correction, and motion-aware scoring flipping Scan Object by
  16.6 points. Simulation only. **Artifact status literal:** fixes are described as
  released but only upstream commit SHAs are given — there is no artifact URL.

### Benchmarks and Evaluation — What to Measure

- **The reach of a verification tool decides its value** (`2608.28795`,
  cs.SE/cs.AI; **v2** announced in this block, v1 2026-08-28; single author,
  affiliation not stated): the cleanest **harness-as-single-controlled-variable**
  study of verification tooling — a purpose-built minimal coding agent with a fixed
  system prompt in which only the tool list changes, over **1,116 web applications**
  built by six frontier models under eight configurations, every artifact graded
  blind against a frozen rubric with a pre-specified Holm-corrected analysis plan.
  Reported: without tools about **one build in seven fails to launch**; a single
  boot probe removes nearly all of those failures at ~35% of a shell's token cost
  while the shell multiplies cost by 2.35; screenshots help where mistakes are
  visible but **the gain does not survive correction for multiple comparisons** and
  adds nothing where failures must be measured rather than seen. Digital-only,
  single grader (98.8% regrade stability). **Verified open:** 3.3 GB Zenodo dataset
  (MIT code / CC BY 4.0 data) with per-run logs, human scores and sealed snapshots;
  the paper's own GitHub URL returns **404**.

### Runtime, Safety, and Observability — Runtime Building Blocks

- **Governed Capability Evolution** (`2604.08059`, cs.RO/cs.AI; **v6** announced in
  this block, v1 2026-04-09; Harbin Institute of Technology, Heriot-Watt Malaysia,
  Soochow University, Fraunhofer FIT): brings **lifecycle governance to harness and
  component upgrades** with four compatibility dimensions (interface, policy,
  behavioral, recovery) and a seven-stage pipeline (validation → sandbox → shadow →
  gated activation → monitoring → rollback → audit), plus a seeded fault-operator
  generator with deliberate out-of-taxonomy probes. Reported in a 5-strategy ×
  5-round × 15-seed simulation of an embodied manipulation stack: naive upgrades
  activate unsafe versions in **74.7%** of rounds and canary/blue-green in
  61.3%/64.0% despite comparable success, while governed upgrades admit **no
  in-taxonomy fault and one out-of-taxonomy probe in 75 rounds** at a 4.6-point
  success cost, with rollback restoring verified operation in **72.4%** of drift
  scenarios (Holm p ≤ 0.024 against all three baselines). Simulation-only
  (behavioral plus a PyBullet backend); adversarial rollback is explicitly out of
  scope. **Verified open:** Apache-2.0 `s20sc/governed-capability-evolution`
  (54 files, 37 Python, full pipeline plus shipped result data) and a live project
  page; the journal version is Information and Software Technology 200 (2026)
  108337, DOI `10.1016/j.infsof.2026.108337`.

### Surveys and Reading Lists

- **Distilling Agentic Systems** (`2609.36630`, cs.AI; v1 2026-09-29; Central South
  University, BAAI, HK PolyU, BUPT, UIC): a **survey/roadmap** whose organizing
  device is an agent decomposed into a parametric model, independently addressable
  **artifacts** (memories, skills, tools) and the **harness** that organizes
  execution (routing, verification, recovery, orchestration), with the deliberately
  sharp test that a stored checklist is an artifact while the runtime that
  retrieves it, binds its variables, advances its steps and invokes recovery is the
  harness. It formalizes a harness-distillation objective with cost, latency and
  risk penalties, develops four levels of harness control expressiveness, gives a
  compile/crystallize/internalize/externalize matrix, and argues that benchmark
  availability does not imply a benchmark can identify whether a gain came from the
  model, an artifact or the harness. Literature synthesis only: **no experiments, no
  artifact, no venue**, and the record still carries an unfinalized ACM template
  (placeholder DOI/ISBN), so the taxonomy is a vocabulary rather than validated
  evidence.

## Revision edits to existing entries

Three README entries were updated, each driven by a change detected in this block.

1. **Guava** (`2606.18363`) — **v3 (2026-09-29), retitled and code released.** The
   title is now "Guava: Distilling Frontier VLMs into a Compact Agent through a
   Robotic Manipulation Harness" (was "…Distilling Frontier VLM Agents into a
   Compact Model with a Manipulation Harness"). The v3 full text adds
   `Code: https://github.com/hdacnw/guava-release`, so the earlier "code was marked
   coming soon as of 2026-07-25" note is superseded. **Verified live:** the
   repository (13,966 KB, created 2026-09-20, pushed 2026-09-29, 3 stars) contains
   the `guava` package, configs and prompts, setup/training scripts, `pyproject`
   and `uv.lock`, and it links the ungated `AIcell/guava-v13b-qwen3.5-4b` checkpoint
   (41 downloads, last modified 2026-09-12). **No LICENSE file exists at the
   repository root** (the GitHub API reports no license; `LICENSE`,
   `LICENSE.md` and `LICENSE.txt` all return 404), so the code is public but not
   formally licensed. The entry's title, links and artifact sentence were updated.
2. **Agent as Policy** (`2609.12541`) — **v3 (2026-09-28), artifacts released.** The
   entry previously ended "No artifacts located … treated literally as not open
   (verified 2026-09-14)". v3 links the Apache-2.0 repository
   `agent-as-policy-2026/agent-as-policy` (28,246 KB, created 2026-09-15, pushed
   2026-09-21, 17 stars) containing `agp/`, a `hardware-bridge/`, calibration, run
   snapshots and dataset documentation, plus an ungated CC BY 4.0 trials dataset
   `Agent-as-Policy/agent-as-policy` (1,500 downloads, last modified 2026-09-16) and
   a raw-capture companion `Agent-as-Policy/yam-agent-as-policy`; the project page
   `agent-as-policy-2026.github.io` is live. The entry's links and artifact sentence
   were updated; the small task/trial count and absence of independent reproduction
   stand.
3. **HarnessVLN** (`2609.15195`) — **v3 (2026-09-29), anonymized code linked and
   headline numbers revised.** The v3 full text reports **59.6% (R2R) and 51.4%
   (RxR)**, down from the 60.8%/53.9% recorded from v1–v2, with HM3D-v2 76.0% and
   HM3D-OVON 59.3% unchanged; the entry's numbers were corrected. The project page
   now links `anonymous.4open.science/r/harnessvln`, whose file API returns a
   substantial repository (LICENSE 20,850 B, `MANIFEST.sha256`, `data/`,
   `deployment/`, `eval_scripts/`, `habitat_extensions/`, `navdp/`, `patches/`,
   `tests/`, `tools/`, `vlnce_baselines/`, a 28 KB `run_mp.py`) whose README
   declares **CC BY-NC-SA 4.0** and describes evaluation on VLN-CE R2R/RxR,
   HM3D-v2, HM3D-OVON and MP3D ObjectNav. It remains anonymized for double-blind
   review, so the entry now records it as inspectable but neither attributable nor
   citable, replacing the earlier "no code, weight, or data release" statement.

One further re-announcement changed a field without changing any verdict: BATON
(`2608.16889`) posted **v2 (2026-09-28)**, whose only release statement is "The
source code will be publicly released upon acceptance" — still a promise, so the
README's "no official code link was located" note stands. `2607.12896` remains the
withdrawn medical-segmentation record already noted on 2026-09-29. The remaining
24 already-referenced IDs carry no field change that alters a recorded status.

## Current Landscape additions

Five bullets added to `README.md`:

1. Harness evolution is being formalized, gated, and searched — Harness Evolution
   as Learning, VACE, Mixture of Self-Improving Branches, Meta-Skills.
2. Harness evaluation is being audited rather than trusted — LongHarness Bench,
   More Programs or More Rolls?, Do Agent Benchmarks Do What They Say?, and the
   verification-surface study.
3. Robot harnesses are being packaged as runtimes with explicit lifecycles —
   RoboSkill, Encore, ProAct-VLM, Skill-Space Shooting, Taming VLAs under Robot
   Execution Errors.
4. Robot benchmarks are adding paired, streaming and self-auditing contracts —
   LIBERO-MAX, RoboChrono, Faster and Better?
5. Safety and upgrades are being governed as harness objects — SafeCoEvo and
   Governed Capability Evolution.

## Rejected / watch list

### Screened this pass and rejected

- **AgentBug-Smith** (`2609.37864`, cs.SE/cs.AI; v1 2026-09-29) — automated
  reproduction of real-world **harness bugs** plus Live-Harness-Bench with 200
  executable bugs; reported 20.00–28.00% reproduction across three backbones against
  ≤13.78% for general bug-reproduction techniques, and +6.32% from distilling repair
  skills. Conceptually a strong fit, but **the artifacts are the contribution and
  neither resolves**: `github.com/EaminC/AgentBug-Smith` returns **404** on page,
  raw and codeload paths, and `huggingface.co/buckets/EaminChan/live-harness-bench`
  returns **401** (the HF user has no public repositories). Recorded as **watch**
  with the links to re-test; include when the benchmark is actually retrievable.
- **Counterfactual Rollout Replay** (`2609.33875`, cs.SE/cs.AI/cs.LG; v2
  2026-09-29, NeurIPS 2026) — forkable environments as process rewards: snapshot,
  restore at a decision point, sample an alternative action and roll forward, with a
  deterministic package mirror, HTTP replay layer and flaky-test filter; reported
  +5.0 points on SWE-bench Verified (41.7% vs 36.7%) at equal wall-clock with fork
  overhead included. **No artifact:** the checklist answers "[No]" to open access
  and every code, configuration and dataset item is a future-tense promise tied to
  the camera-ready version. **Watch** for that release.
- **Auditable Long-Term Memory** (`2609.38021`, cs.CL/cs.AI/cs.IR; v1 2026-09-29) —
  a deterministic retrieval chain with a readable evidence repository (reader
  outputs, judge verdicts, control records), re-derived to 479/475 of 500 on
  LongMemEval-S. Excluded as too narrow: the method sources are held, there is no
  held-out split, and 72 rows used an unmeasured modified prompt. **Watch.**
- **Evaluating Bounded Autonomy in Regulated Agentic AI** (`2609.37501`, cs.AI;
  v1 2026-09-28) — a diagnostic harness with constitutional rewards, escalation
  labels and runtime governance, but evidence is n=12/n=8 smoke scale, the
  "ancillary artefacts" are schemas, pseudocode and ten sample tasks with **no
  runnable code**, and the implementation is promised "upon acceptance". **Watch.**
- **RobotValues** (`2606.03312`, cs.RO/cs.AI; **v2** re-announced 2026-09-30,
  v1 2026-06-02; Seoul National University) — a values-conflict evaluation contract
  for household robots (8,707 image-grounded instances, 59,612 candidate actions,
  default-choice and value-conditioned tasks, Bradley–Terry scoring, random
  baselines), reporting that conflicting-condition steering succeeds only
  13.4–24.6% and that fine-tuning Qwen3-VL-2B lifts 31.73% → 77.97%. Substantively
  qualifying, but **no dataset, code or project page exists**, and the scale claim
  was revised downward from 10K to 8K. **Watch** for the first public release.
- **BAVO-Bench / Recovering the View** (`2609.37292`, cs.RO; v1 2026-09-29; SYSU,
  UESTC, Pengcheng Lab, X-Era AI Lab) — a controlled-occlusion active-vision
  benchmark (Clean/Stage/Random-time, SR/TPS/CVG metrics with a privileged
  counterfactual; A-FAR 51.3/63.7/18.7 SR versus ManiFlow 45.7/60.7/15.0, and π0.5
  collapsing to 4.0% under random-time occlusion despite the highest CVG). The
  advertised code link is the **project-page repository only** (README, index.html,
  static assets; no license, no benchmark code or data), and real-robot evidence is
  qualitative. **Watch.**
- **MotorMind** (`2609.38078`, cs.RO; v1 2026-09-29) — scaffolding general VLMs for
  zero-shot manipulation. The code link is a **README-only placeholder** promising
  a release "before October 15". Promised, not released. **Watch.**
- **RoboHarn-Evo** (`2609.37583`, cs.RO; v1 2026-09-29) — evolving hierarchical
  physical knowledge for self-improving manipulation (3 real tasks × 8 scenes per
  arm). **No artifact URL of any kind** in the record or full text. **Watch.**
- **SimpleARM** (`2609.36595`, cs.RO/cs.AI; v1 2026-09-29; CMU, Tulane, NYU,
  Princeton, Columbia) — a training-free memory layer between a frozen subgoal
  predictor and a frozen VLA, reporting 67.17% on RoboMME against 44.51% for the
  strongest non-oracle baseline. The project page is live but the code repository is
  a **README-only placeholder**. **Watch.**
- **Video2Skill** (`2609.36691`, cs.CL only; v1 2026-09-29) — streaming experience
  to reusable embodied skills. The advertised "Code" repository is just the
  project-page source (no README), so nothing runnable is released. **Watch.**
- **SkillWeaver** (`2609.36171`, cs.RO; v1 2026-09-28; CoRL 2026) — a robot
  data-generation runtime (four PPO-trained neural interaction skills behind a tool
  interface, verifier-guided tree search, cross-episode memory) with simulation and
  real Franka evidence and large reported data yields. **Artifacts dead:** the
  project page's "Code" is `href="#"`, "Dataset" is commented out, and
  `katefgroup/skillweaver` is a website-only repository with no README. **Watch.**
- **RASO / Retrieval-Augmented Skill Optimization** (`2609.38024`, cs.AI; v1
  2026-09-29) — models the harness (tools, file access, observation, budget) as the
  adaptation target across harnesses. Digital-only, **no artifact**, and the paper
  has **no limitations section at all**. **Watch.**
- **T²Mem** (`2609.36720`, cs.RO; v1 2026-09-29; Stanford/NVIDIA/UMich) — test-time
  memory as fast weights between a π0.5 VLM and its action expert, reporting
  17.93% → 56.83% on RoboMME with a ≥3× speedup. The claimed project page returns
  **404** and the repository is website source only; the contract is internal to one
  policy and every artifact is missing. **Watch.**
- **BSC-R / Boundary-State Control** (`2609.37475`, cs.AI; v1 2026-09-26) — a
  deterministic commit-time effect-boundary kernel binding a single-use commit
  authorization to the exact action plus a semantic projection of the justifying
  state, with paired invalid-commit/valid-retention metrics and an unusually honest
  negative result (25% invalid-effect rate across 2,560 broad-suite attacks, and a
  restrictive comparator reaching 0.105% ASR at 11.2 points of utility cost).
  Excluded on artifact and evidence grounds: **no released artifact** (the "audit
  artifact" has no URL), single author with an obscure affiliation, digital-only.
  **Watch.**
- **Altered Thoughts, Altered Actions (v2)** (`2603.12717`, cs.RO/cs.AI/cs.LG; v2
  2026-09-29, v1 2026-03-13; University of Melbourne) — a pre-registered
  oversight/evaluation protocol over four text-channel conditions on DeepThinkVLA,
  reporting a 47.8-point recovery from injecting a correct chain against a corrupted
  instruction but a **descriptive D−B contrast at power 0.39 with no p-value**.
  Simulation-only, LIBERO-Goal only, and "code and data will be released with the
  archival version". **Watch.**
- **Beyond Token Importance / GeoScaffold** (`2609.36967`, cs.RO; v1 2026-09-29) —
  training-free VLA token pruning with a spatial-coverage-radius diagnostic
  (20% retention, 93.2% LIBERO average, 1.78× prefill). **Excluded:** a
  model/inference-efficiency improvement with an internal metric, no external
  runtime, interface, recovery, safety or evaluation contract, no artifact, and
  simulation only.
- **VLA, world-action, and robot-method papers screened and rejected this pass** —
  `2609.36915` AeroManip-VLA, `2609.37150` CoRe-VLA, `2609.37307` action-history
  memory VLA, `2609.29382` decoupled early exits, `2609.36588` cooperative
  multi-agent VLA, `2609.36540` reactive flow policies, `2609.36413` One from
  Infinity, `2609.37793` MVG-WAM, `2609.38163` ReWAM, `2609.38057` EVO-WAM,
  `2609.37721` CogWAM, `2609.36471` Staircase Policy, `2609.37250` V-JEPA Policy,
  `2609.36182` actuator-degradation TTA, `2609.37681`-adjacent navigation and
  mobile-manipulation work, and the remainder of the 172-record robot sweep —
  model, representation, or policy-training contributions without an external
  runtime, interface, recovery, safety, or evaluation contract.
- **Genuine-world / domain papers excluded under the standing scope rule** —
  surgical and medical robotics (`2609.34823`, `2609.37455`, `2609.37456`),
  aerial/planetary/marine autonomy, autonomous driving (`2609.34387`, `2609.36851`,
  `2609.37970`), tactile-sensor and mechanism design, motion planning, and
  whole-body/legged control (`2609.37070`). Domain or legacy contributions rather
  than harness contributions, notwithstanding individual technical merit.
- **Harness-adjacent digital-agent papers screened and rejected** — `2609.36049`
  co-trained monitors, `2609.36490` evading latent monitors, `2609.36201` SCOUT,
  `2609.34113` GUITAR, `2609.13463` root-cause attribution, `2609.36086` PADMÉ,
  `2609.37923` EpiCon, `2609.37968` SelfSearch, `2609.37633` RLTL;DR,
  `2609.38070` divergent-token uncertainty, `2609.35793` pass@k vs pass@1,
  `2609.35912` MMSkillRisk, `2609.37196` ToolFence, `2609.38021`-adjacent memory
  work, and the remainder of the 305-record eval sweep — measurement,
  middleware or guardrail papers that overlap lines the list already carries
  (Phantom Gains, HarnessOpt-Bench, RecoveryBench, MaliciousSkillBench) without a
  new harness contract or a retrievable artifact.

### Carried watch list — unchanged this window

Carried forward without status change (none of the thirteen IDs appears in this
block; checked by ID against both the new block and the wide window): vla.simd
(`2609.24274`), LIBERO-VPro (`2609.24350`), ARSTAG (`2609.24563`), React When
You Need To (`2609.22587`), SafeStage (`2609.21223`), AWM-3DFM (`2609.21502`),
Toollery (`2609.22218`), CHART (`2609.22247`), DTOC (`2609.26121`), MATE
(`2609.26520`), HABILIS Brain 0 (`2609.25558`), Passive→Active Exploration
(`2609.29091`), and DA-GRD (`2609.29065`). New watch entries from this pass are
recorded inline above (AgentBug-Smith, Counterfactual Rollout Replay, Auditable
Long-Term Memory, Bounded Autonomy, RobotValues, BAVO-Bench, MotorMind,
RoboHarn-Evo, SimpleARM, Video2Skill, SkillWeaver, RASO, T²Mem, BSC-R, Altered
Thoughts v2). F4R (`2609.35575`) was re-checked after its v2 re-announcement and
still has no code link. The artifact placeholders recorded on 2026-09-25
(Maithili/GAP's 1 KB repository, HEXIS's HTTP 401 anonymous link, the RoboRecover
dataset "being prepared for Hugging Face", and HarnessPAI's "not yet publicly
released" code) are unchanged.

## Validation performed

- `git diff --check` clean (no whitespace errors, no conflict markers); zero
  conflict-marker lines in `README.md`.
- Markdown structure re-checked after every insertion: all 513 top-level `- [`
  bullets have balanced parentheses and brackets, every bullet has an even number
  of bold and backtick markers, and the section headings and Contents list are
  unchanged (39 headings).
- **Every link added or rewritten this run was fetched on 2026-09-30 and returns
  HTTP 200**: 19 arXiv abstract pages, the 3 revision links (Guava code and model,
  Agent as Policy code/data/site, HarnessVLN anonymized code), and 21 artifact
  links across repositories, project pages, Hugging Face datasets and Zenodo DOIs
  (40 in total, listed in `.scratch/arxiv-2026-09-30/revisions/` captures and the
  per-batch dossiers).
- Placeholder, dead and broken links were verified in the same pass and are
  recorded literally: `EaminC/AgentBug-Smith` 404 and the HF bucket 401
  (AgentBug-Smith), `moured/ProAct-VLM` README-only, `liberomax/LIBERO-MAX` and
  `YIFANK/encore` and `stringnlplab/longharness` and `qiancheng-apodex/MetaSkill-AI4AI`
  without licenses, the paper's own `achintmehta/coding-eval-agent-runs` URL 404,
  and `yzliu84.github.io/T2MEM-project/` 404.
- Repository licenses, sizes, creation and push dates, default branches, stars and
  top-level trees were read from the GitHub API (budget shared across six agents,
  ≤6 calls each); Hugging Face gating, license, downloads and last-modified from
  the Hub API; project pages, full texts and PDFs fetched directly.
- Dates checked for cross-file consistency: the README badge, the "Last verified"
  line, and this record all carry **2026-09-30**.
- Categories checked against the arXiv record for every included entry.
- Cross-file consistency: each of the five Current Landscape bullets refers only to
  entries now present in the main list, and the provenance caveats stated in the
  README (VACE's and Mixture of Self-Improving Branches' missing artifacts,
  Meta-Skills' partial unlicensed release, RoboSkill's one-shot snapshot, Encore's
  absent license, ProAct-VLM's placeholder repository, LIBERO-MAX's missing
  license, Faster and Better?'s unaddressable fixes, and the verification study's
  broken GitHub link) match this record.

## Operational notes and blockers

- `/tmp` is per-command in this sandbox, so harvest XML, parsed JSON, dossiers,
  full texts, project pages and logs were kept under
  `.scratch/arxiv-2026-09-30/` (untracked, never staged).
- The export API's `submittedDate` range syntax required **both** percent-encoded
  brackets/spaces **and** `curl --globoff`; with both, the 2026-09-30 windows
  returned HTTP 200 with zero entries while the 2026-09-29 positive control and the
  no-filter liveness control returned normally. This is the announcement-versus-
  indexing lag recorded by the 2026-09-27, 2026-09-28 and 2026-09-29 runs; the
  OAI-PMH harvest is authoritative and unaffected.
- Six verification agents ran in parallel and shared one unauthenticated GitHub API
  budget (60 requests/hour). Each was capped at 5–6 `api.github.com` calls and read
  everything else from GitHub HTML tree pages and `raw.githubusercontent.com`; all
  HTTP statuses are recorded in the dossiers. No verdict depends on a rate-limited
  call.
- `2608.28795` has **no arXiv full-text HTML** (404 for the bare id and for v1/v2);
  it was verified from the v2 PDF instead.
- The HarnessVLN code link remains **anonymized** (`anonymous.4open.science`). As
  with Hearsay on 2026-09-29, the rendered page redirects to a single-page app while
  the file API returns the real tree; a later run seeing only the redirect should
  re-check the API endpoint rather than conclude the artifact has disappeared.
- No conflict occurred; `git fetch origin`/`git pull --ff-only` were run before
  committing and the working branch is `main`.

## Commit

Task-owned files committed on `main`: `README.md`,
`sources/daily-arxiv-2026-09-30.md`. Unrelated user changes
(`docs/reference-architecture.md`, `docs/ring-harness.png`, `handoff.md`,
`.scratch/`) were left untouched and unstaged. No branches, no pull requests, no
force-push.
