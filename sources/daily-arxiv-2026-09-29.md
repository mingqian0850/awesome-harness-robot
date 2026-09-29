# Daily arXiv scan — 2026-09-29

## Scope

- **Interval:** everything announced after the 2026-09-28 run's cutoff. That run
  (started ~08:20 UTC on **Monday 2026-09-28**, committed 10:42 local) screened
  the announcement block whose OAI datestamp is **2026-09-28** — 886 unique base
  IDs, newest base ID `2609.31620`. This run started ~08:16 UTC on **Tuesday
  2026-09-29**. **There are no missed dates:** the intervening 2026-09-28 18:00
  catch-up slot could not have seen a new block (the next announcement lands at
  02:00 local on 2026-09-29), and the 2026-09-28 window re-harvested here returns
  866 unique IDs, i.e. the same block, 20 records lighter because 20 of its
  records were re-announced into the new block.
- **A new announcement block landed.** The OAI-PMH harvest for
  `from=2026-09-29&until=2026-09-29` returned records for all five categories:
  **4,867 memberships = 3,447 unique base IDs**, every one carrying datestamp
  **2026-09-29** (memberships: cs.AI 1,478, cs.LG 1,563, cs.CV 831, cs.CL 633,
  **cs.RO 362**). This is the Monday-submission batch, ~3.9× the size of the
  2026-09-28 block, and it is the largest single block screened by this
  repository to date.
- **Window and delta.** The wide window `from=2026-09-28&until=2026-09-29`
  returns **5,991 memberships = 4,313 unique base IDs** across two datestamps:
  **866** at 2026-09-28 and **3,447** at 2026-09-29. Against the previous run's
  parsed window (1,762 unique IDs, 2026-09-25…2026-09-28) this is a window
  shift rather than a pure addition — the 2026-09-25 datestamp has left the
  window — so the meaningful delta is over the **916 IDs common to both
  windows** (4,313 − 3,397 appeared; arithmetic closes exactly against the 846
  that disappeared): **51 of those carry field changes** after the
  entity-normalisation control (`&#34;`/`&quot;` versus `"`) applied to titles,
  abstracts, and comments, and none of the rest changed silently under
  field-wise comparison. **The newest base ID moved
  from `2609.31620` to `2609.35770`**, so the ID frontier advanced by 4,150 and
  the block is not being held back.
- **Block composition.** 3,380 of the 3,447 records carry a September 2026
  created date; the tail runs back through June (15), May (8), January (7),
  July (7), March (4), April (4), September 2025 (4), and then sparsely through
  2018–2025 as re-announced replacement versions. **954 records carry
  pre-`2609.` IDs** and **2,493 carry `2609.3xxxx` IDs**. **362 records carry
  cs.RO** (337 of them not previously curated).
- **Withdrawal notices in this block: eight, plus one admin note.**
  `2608.02617` (posted without final co-author approval), `2609.06914`
  (administrative compliance oversight regarding a Data Use Agreement; also the
  record carrying the admin note), `2609.23688` (substantially revised version
  to follow), `2509.23373` (reported results incorrect), `2506.18831` (superseded
  by a more rigorous study), `2512.15138` (substantive issues requiring
  correction), `2607.12896` (publisher-specific template used before acceptance),
  and `2506.18078` (identifiability result incorrect in Sections 3.3–3.4). Two
  further records (`2602.22673`, `2606.30566`) announce corrections rather than
  withdrawals. None of these IDs is referenced anywhere in this repository, so
  no entry required annotation.
- **Export-API boundary check is inconclusive for the new block, by design of
  the endpoint.** With `submittedDate` percent-encoded and `curl --globoff`, the
  HTTPS range query for **cs.RO on 2026-09-29 returns HTTP 200 with a
  well-formed empty Atom response (0 entries, `totalResults` 0)** at ~10:05 UTC
  — the same announcement-versus-indexing lag recorded by the 2026-09-27 and
  2026-09-28 runs. Further date-window queries (including the intended positive
  control on an older date and the 2026-09-28 window) then returned **HTTP
  429**: the export API rate-limited this IP after the parallel verification
  pass, so the boundary check could not be repeated later in the run. The
  **OAI-PMH harvest is the authoritative source** for every number above, and no
  result in this record depends on the export API.
- **62 of the 3,447 new-block IDs already appear somewhere in the repository**
  (README, `docs/`, `sources/`); 10 of those are README-level entries. Because
  an OAI datestamp of 2026-09-29 means "announced in this block", these are
  re-announcements of records curated earlier. Each was checked for a revision
  that changes its recorded status (see "Revision edits"): **exactly one did**
  (`2609.30123`, title change), and one previously recorded withdrawal reason
  changed (`2608.04408`).
- **Watch-list re-check:** **none** of the thirteen carried watch IDs
  (`2609.24274`, `2609.24350`, `2609.24563`, `2609.22587`, `2609.21223`,
  `2609.21502`, `2609.22218`, `2609.22247`, `2609.26121`, `2609.26520`,
  `2609.25558`, `2609.29091`, `2609.29065`) appears in the 2026-09-29 block;
  checked by ID against both the new block and the wide window. All thirteen are
  carried forward unchanged, and the artifact placeholders recorded on 2026-09-25
  (Maithili/GAP's 1 KB repository, HEXIS's HTTP 401 anonymous link, the
  RoboRecover dataset "being prepared for Hugging Face", and HarnessPAI's "not
  yet publicly released" code) are unchanged.

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`) over three windows: `2026-09-29…2026-09-29` (new
   block), `2026-09-28…2026-09-28` (late-batch/boundary re-check), and
   `2026-09-28…2026-09-29` (previous block plus new block). Pages saved to
   `.scratch/arxiv-2026-09-29/oai-{new,prev,full}-*.xml`; the `new` window
   needed two pages for cs.AI and cs.LG, every other category and window
   returned a single complete page with no resumption token.
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`oai-records-{new,full}.json`, `parse_oai.py`), then **delta against the
   previous run's parsed harvest** with field-wise before/after values
   (`delta.json`) and the entity-normalisation control.
3. **Export-API boundary cross-check** over the 2026-09-26, 2026-09-27,
   2026-09-28, and 2026-09-29 `submittedDate` windows, over **HTTPS**, with a
   no-date-filter liveness control (cs.RO `totalResults` 59,311; cs.AI 202,786)
   and percent-encoded range syntax. Result and limitation are recorded under
   Scope and Operational notes.
4. **Screening passes with five distinct emphases** over the 3,447 new records:
   (a) a **harness-layer** sweep (harness, scaffold, agent loop, runtime,
   middleware, tool call, skill discovery, code-as-policy, execution trace,
   replay, rollback; 246 uncurated records scored ≥2 on harness or
   self-improvement terms); (b) a **robot/embodied** sweep (263 uncurated
   records); (c) a **VLA / robot foundation / world-action** sweep (214
   uncurated records); (d) an **eval/safety** ranking (863 uncurated records);
   and (e) an **artifact-signal** listing of every uncurated record naming a
   concrete release object, repository URL, "we release" sentence, or project
   page (662 records). `screen.py`; outputs in
   `sweep-{harness,robot,vla,eval,artifacts}.txt` and compact
   `triage-{harness,robot,vla}.txt`.
5. **Dossiers and primary-source verification** for 27 shortlisted records by
   five parallel verification agents: arXiv export-API version history and
   dates, the arXiv abstract page and full-text HTML (or the PDF when HTML is
   unavailable, as for `2609.33822`), official project pages, the GitHub API
   (license, size, creation and push dates, default branch, stars, top-level
   tree) and the Hugging Face Hub API (gating, license, last modified,
   downloads) — all on **2026-09-29**, with raw captures under
   `.scratch/arxiv-2026-09-29/{xml,abs,html,pdf,pages,gh,ghpages,hf}/` and
   per-batch dossiers `dossier-{A,B,C,D,E}.md`. Where a paper's arXiv e-print
   was needed to test an "attached code" claim, the source tarball was
   downloaded and listed. Nothing was taken from a secondary source.
6. **Duplicate screening** of every candidate against `README.md`, `docs/*.md`,
   and `sources/*.md` by arXiv ID, project name, and repository URL before
   writing. No included ID was already present in `README.md`.
7. **Revision screening of the 62 already-referenced IDs** by diffing the
   harvest fields and, for the two that mattered, fetching the abstract page
   submission history and the latest full text to test whether an artifact
   status or evidence class changed.
8. **Watch-list re-check** by ID against the new block and the wide window (see
   Scope).

## Included (README updates)

Thirteen new entries in six README sections, plus two revision edits. Artifact
status is reported literally; every repository, license, dataset, date, project
page, and tree claim below was verified live on **2026-09-29**.

### Harnesses and Development Platforms — General Harness Design and Self-Improvement

- **Compositional Safety Failures in Harness Evolution** (`2609.33123`, cs.AI;
  v1 2026-09-27): makes cross-component **interaction** the unit of safety
  analysis for evolving harnesses, where memory, prompt, skill, and tool updates
  are each safe and utility-preserving but compose into unsafe behavior. Reports
  **43 pairwise and 18 irreducible three-way** failures across three safety
  benchmarks, then replaces combinatorial validation with a **typed hypergraph**
  (component states as nodes, safety-relevant higher-order interactions as
  hyperedges) whose update touches only the changed states' interaction
  neighborhood, driving a runtime monitor. **No artifact located:** the full
  text links no repository, project page, dataset, or model, and makes no
  release claim in any tense. Digital-agent evidence only.
- **LiteEvo** (`2609.33146`, cs.AI; v1 2026-09-27, ICLR-2027 style): harness
  evolution from a **versioned component library** rather than whole-program
  search — tool-free meta-agents mine trajectories for reusable components,
  curate them, and compose each round's harness from the library starting from
  one neutral harness that never names the benchmark. Reported with a frozen
  Qwen3.5-9B on ALFWorld, WebShop, AppWorld, GAIA, and SWE-bench Verified:
  pass@2 lifts of **10.5–67.7 points**, mean pass@2 **71.0 vs 67.3** against a
  HarnessX reproduction at **13.0× lower mean API cost**, retention on unseen
  test tasks of four benchmarks, and a 1.2–71.4 point lift for Claude Code with
  Sonnet 4.6. **Artifact status literal:** the paper states it "attaches a
  minimal set of our code" and "will release the full code on GitHub upon
  acceptance"; the arXiv record has **no ancillary files** and the downloaded
  e-print source (58 files) contains **zero code files**, so the availability
  statement is not substantiated. CC BY-NC-ND 4.0.
- **Harness Learning** (`2609.35738`, cs.CL/cs.LG; v1 2026-09-28): trains a
  **proposer** to revise a solver's harness from execution feedback, formulating
  the process as meta-learning over executable programs in which harness
  revisions play the role of weight updates and the reward is the revised
  harness's task performance. The reported properties are that revision quality
  improves, that test-time adaptation transfers to unseen tasks, and that a
  proposer trained on individual revisions keeps improving harnesses over
  multiple rounds — all with **no parameter-space update** at test time.
  Reasoning and multi-hop QA only. **No artifact announced** in any tense.
- **Audit the Scaffold, Not the Checkpoint** (`2609.34924`, cs.LG/cs.AI/cs.SE;
  v1 2026-09-28, 9 pages main / 46 total): the **stationarity dichotomy** — an
  iterative self-modifying agent hits strict diminishing returns while its
  reachable set of edits is fixed and can escape only when that set expands, so
  scaffold rewriting raises reach without touching a weight and frozen weights
  guarantee a ceiling but not stationarity. Separates fixed-class search,
  test-time training, and scaffold rewriting; argues best-of-k realizes the best
  worker's ceiling exactly while a weighted vote needs diversity same-family
  workers lack (majority fails 23 of 55 tasks). Evidence is the measured decay
  of per-round improvement on SWE-bench plus geometric churn decay across 401
  production sessions against a pre-AI human baseline. **No locatable
  artifact:** the text claims a "supplementary repository" but gives no URL,
  DOI, or location.
- **Hearsay** (`2609.32495`, cs.CR/cs.AI/cs.CE/cs.SE; v1 2026-09-26, 48 pages):
  evaluates the harness's **own run record** and defines a record as
  *evidentiary* when a reader who was not there can check it without trusting
  the writer. Across sixteen deployed frameworks none writes one in full; in
  140 runs over five harnesses, three blinded LLM examiners and a human panel
  named the right fault in **74–91%** of cases but could prove it only from
  files the benchmark added, **fewer than one citation in ten** landed on
  anything the harness did not write, and the lowest-false-alarm examiner caught
  only half of the deleted, rewritten, or fabricated entries. The remedy is a
  **second author**: an append-only ledger of harness↔model traffic kept outside
  the harness reported all **28** seeded omissions and fabrications that a hash
  chain over the harness's own record reported none of. **Artifact status
  literal:** the only released object is an **anonymized review repository**
  (`anonymous.4open.science`, code MIT, data CC BY-NC 4.0, ~8.7 MB); the
  de-anonymized archive is promised only on acceptance. Digital agents only.

### Harnesses and Development Platforms — Unified Evaluation

- **Claw-SWE-Bench** (`2606.12344`, cs.LG/cs.CL; **v2** announced in this block,
  v1 2026-06-10): makes the **harness the controlled variable** for software
  engineering with 350 real GitHub issue-resolution instances over eight
  languages and 43 repositories plus an 80-instance calibrated "Lite" subset,
  behind a **shared adapter protocol** that aligns inputs, outputs, and
  execution environments across harnesses at a fixed model. **Verified open:**
  the canonical MIT-licensed repository is `TokenRhythm/claw-swe-bench` (143 KB,
  110 stars, created 2026-06-10, pushed 2026-09-28) — the abstract's
  `opensquilla/claw-swe-bench` URL now 301-redirects there — with adapters for
  seven harnesses, inference and evaluation entry points, configs, prompts, and
  tests; the ungated MIT-licensed Hugging Face dataset (2,837 downloads,
  last modified 2026-09-28) holds full and lite parquet splits. A live
  leaderboard is linked from the repository. Digital coding agents.

### Robot Agent Systems — Agentic Robot and VLA Harnesses

- **Recursive Harness Distillation across Agents for Robot Manipulation**
  (`2609.33378`, cs.AI/cs.CV/cs.LG/cs.RO; v1 2026-09-27): distills a strong
  agent's intervention experience into a written **playbook**, executes with a
  light agent, and recursively refines the playbook from that agent's rollouts —
  accumulated guidance rather than weight updates. The interface is explicit:
  three intervention sites on the frozen VLA's computation (instruction;
  attention over features with learned parameters preserved; output), with
  admissible transformations fixed by the interface, candidate approval, prefix
  execution within declared limits, and a decision that need not advance the
  environment. Reported: real-world manipulation **37.3% → 64.0%**, SimplerEnv
  Bridge **66.7%** for the light agent with the playbook vs **41.7%** for the
  GR00T-only baseline, and **79.2%** for the strong agent with the same
  playbook. **No artifact located:** no code, playbook, prompts, or data link
  anywhere in the paper. Simulation plus a three-task real-world evaluation.
- **RoboFoundry** (`2609.32862`, cs.RO/cs.AI; v1 2026-09-26): makes the
  supporting system itself the policy — a frozen foundation model inspects and
  edits a filesystem-resident system with `cat`/`grep` and
  `add`/`modify`/`delete`, over a context system (`SAVE`/`RETRIEVE`/`UTILIZE`
  against persistent memory), a hierarchical skill system, and a **recovery
  tree** that replans from a valid state instead of repeating the failed action,
  with a semantic binding layer separating embodiment-invariant from
  embodiment-specific execution. Reported: EmbodiedBench state of the art and
  **+27.8%** over GPT-5.5 (Qwen3.7-Plus 70.3% vs GPT-5.5 72.7%), **≥39.0%**
  over all baselines on RoboMemArena, large LIBERO-PRO perturbation gains, and
  real-robot deployment on an AgileX platform and a Unitree G1. **Artifact
  status literal:** the project page is live and links code, but
  `robofoundry2026/RoboFoundry` holds a README, one PNG, four unchecked
  "Release …" TODO items, and **no LICENSE file at all** — promised, not
  released.
- **What Stops Recursive Self-Improvement in Robotics?** (`2609.31760`,
  cs.RO/cs.AI/cs.LG; v1 2026-09-23, technical report, single author): 123
  improvement rounds in which an agentic loop watches failures, diagnoses
  missing capabilities, writes or installs skills, and tests each change in
  simulation — with the target task (condiments on a fridge's top shelf)
  **never succeeding** while individual changes kept passing their tests. Three
  diagnosed causes sit around the agent rather than in it: chained perception
  modules do not understand relations, skill chains lock learning onto the first
  step, and **the harness decides what is learned** because the agent optimizes
  exactly what the evaluator measures. The harness — evaluators, tests, memory,
  and the actuator description — is the part only humans may change. **No
  artifact** of any kind (code, harness, logs, prompts, or data); simulation-only
  (RoboCasa), so the value is the failure taxonomy and its falsifiable
  recommendations.
- **NavHarness** (`2609.34276`, cs.RO; v1 2026-09-28): a training-free embodied
  harness that makes memory processing part of the navigation loop — each task
  or recovery attempt is a fresh multi-round agentic session consulting an
  evolving map, earlier task records, and house knowledge, checked against
  observations and corrected in place, with outcome verification and run-end
  consolidation deciding what later sessions inherit. Reported: **+18.6 points
  s-SR** with Astra and **+22.6** with Opus 5 over context-only independent
  sessions on GOAT-Bench, **83.7 s-SR / 36.9 e-SR** with SLAM-estimated poses,
  and **structured recovery handovers beating length-matched summaries** by 8.3
  s-SR. **Verified open:** MIT-licensed `billzhao1030/NavHarness` (15.5 MB,
  created and pushed 2026-09-28, single push burst, no releases) with the
  harness and runner; no weights or datasets. Simulation-only.

### Benchmarks and Evaluation — Manipulation and VLA

- **CodeActionBench** (`2609.33807`, cs.RO/cs.AI; v1 2026-09-27): evaluates
  agentic code-as-policy as a controlled contract — 25 tasks behind a **shared
  robot API** (RGB observations, calibrated geometric operations, robot
  feedback, bounded motion) with fixed instances, resource budgets, and a
  **hidden physical-outcome verifier**, and no task-specific fine-tuning,
  demonstrations, specialist perception or grasp modules, privileged state, or
  predefined policies. Reported across nine model/harness configurations and 675
  attempts: success from **2.7% to 73.3%**, with GPT-6 Astra driven by Codex CLI
  solving 22 of 25 tasks at least once in three attempts. **Verified open:** the
  MIT-licensed `lyhkk/CodeActionBench` repository (1.7 MB, created and pushed
  2026-09-27) contains all 25 task directories and the `codeaction` package.
  **Artifact status literal:** the repository README states all 675 attempts are
  on Hugging Face, but `Yiheng-Lyu/CodeActionBench` returns **401/404**, and the
  project site still marks code and full results "coming soon". Simulation-only
  (RoboTwin 2.0); the paper states transfer to physical robots is untested.

### Benchmarks and Evaluation — Agent and Embodied Reasoning

- **RLE-Bench** (`2609.34210`, cs.RO; v1 2026-09-28): examines coding agents as
  **robot-learning engineers** — nine task families across interactive control,
  policy learning, perception and estimation, and mechanical design, requiring
  the agent to write code, train policies, build its own harness and skills,
  design mechanisms, and reason from multimodal feedback, scored by
  benchmark-owned private verifiers under hidden scenes, dynamics, embodiments,
  and seeds and aggregated into an RLE Index plus workflow-specific capability
  profiles. **Verified open:** the MIT-licensed `RLE-Bench/RLE-Bench` repository
  (17.1 MB, 63 stars, created 2026-09-14, pushed 2026-09-28) ships a
  `rlebench list|doctor|prepare|run|summarize|view` CLI, containerized task
  workspaces, pinned simulators, shared robot descriptions, Harbor-based
  evaluation, and tests; the leaderboard site is live. **Simulation-only** —
  the paper states its evaluations are exclusively in simulation and frames the
  public release as planned while the repository is already public; reproduction
  requires Docker and, for most families, an NVIDIA GPU.

### Runtime, Safety, and Observability — Runtime Building Blocks

- **Planarian** (`2609.35366`, cs.OS/cs.AI/cs.CR; v1 2026-09-28): introduces
  **agent statepoints** — consistent, restorable point-in-time versions of
  environment state spanning local files and processes *and* remote services —
  exposed as **snapshot** (incremental CRIU process plus zero-copy ZFS
  file-system snapshots, with transparently recorded compensating actions that
  undo remote changes without remote checkpoint support), **rollback** (restore
  a prior local checkpoint and replay the compensating actions), and **fork**
  (isolated branches for parallel exploration). Reported: task-quality gains up
  to **15×** from undoing mistakes and exploring alternatives, and about **3%
  overhead** for user-initiated recovery. **No artifact announced** in any
  tense, and the implementation depends on ZFS, CRIU, and a custom remote proxy.
  Digital agents only.

## Revision edits to existing entries

Two line-level edits, each driven by a metadata change detected in this block.

1. **HEXIS** (`2609.30123`) — **v2 retitled.** The block's record carries the
   title "HEXIS: Compiling **Agent** Skills into Extended Finite State Machines"
   (the entry was written as "Compiling Skills into Extended Finite State
   Machines"), and the comment field changed from "32 pages, 13 figures, 8
   tables" to "34 pages, 13 figures, 8 tables". The README entry title was
   updated to the current title. **No other recorded status changed:** the
   paper's sole artifact link is still the anonymized `anonymous.4open.science`
   repository, which still returns **HTTP 401** on 2026-09-29, and the release
   sentence is still future tense — the watch-list note is unchanged.
2. **`2608.04408`** — **recorded withdrawal reason is now wrong.** This ID
   appears in the 2026-09-28 record's withdrawal list as "withdrawn by the
   authors 'due to issues in experimental validation'". In this block the
   comment field was replaced by **"false information"** (latest version v3,
   2026-09-28, 1 KB; the paper remains withdrawn). The repository records that
   ID only inside the 2026-09-28 source file, so no README entry was touched;
   the correction is recorded here for traceability, and the earlier record's
   wording should be read as superseded.

Two further re-announcements changed titles without changing any recorded
verdict and required no edit: `2609.28919` ("Control the Harness, Control the
Cost" → "Harness Tokenomics: A Router for the Enterprise Agentic Context") and
`2609.29773` ("Breaking the Environment Wall: Evolving LLM Agent Environments…"
→ "…A Unified Framework for Preparing…"), both screened and rejected on
2026-09-25. `2609.31620` carries a minor abstract revision; it is referenced
only as the previous block's newest base ID. Four further re-announcements
(`2609.28766`, `2609.28767`, `2609.28973`, `2609.29340`) changed only page
counts or the arXiv `<created>` date, and the other 53 of the 62
already-referenced IDs carry no field changes at all.

## Current Landscape additions

Four bullets added to `README.md`:

1. Harness evolution is now a safety surface and an audit object — compositional
   failure across individually safe component updates (Compositional Safety
   Failures), and the harness's own record not being evidence without a second
   author outside it (Hearsay).
2. Harness improvement is being decomposed into reusable pieces and audited for
   what actually improves — LiteEvo's versioned component library, Harness
   Learning's learned proposer, and the stationarity dichotomy's "audit the
   scaffold, not the checkpoint".
3. Robot self-improvement loops are being reported honestly, including where
   they stall — Recursive Harness Distillation's playbook transfer, RoboFoundry's
   filesystem-editable system-as-policy, and a 123-round negative result whose
   causes were the evaluator, the skill chain, and the harness.
4. Harness-level evaluation is being packaged as released contracts —
   Claw-SWE-Bench's adapter protocol, CodeActionBench's shared robot API with a
   hidden verifier, RLE-Bench's robot-engineering workflows, and NavHarness's
   memory-in-the-loop navigation harness.

## Rejected / watch list

### Screened this pass and rejected

- **MetaBench-Harness** (`2609.33411`, cs.AI/cs.CL/cs.SE; v1 2026-09-27, "Work in
  progress") — dual-loop search that optimizes the **benchmark-generation**
  workflow itself, applied to CodeContests and AIME-2024. Excluded on category:
  it manufactures harder datasets rather than contributing an agent-execution
  harness, and it overlaps the evaluation-construction line already carried by
  HarnessOpt-Bench. **Artifact status literal:** "all seed datasets, prompt
  templates, and the complete codebase will be publicly released upon paper
  acceptance" — promised, not released.
- **Raven: The Harness of Harnesses** (`2609.33439`, cs.AI/cs.CL/cs.GT/cs.MA/cs.NE;
  v1 2026-09-27) — the only shortlisted paper with a substantive artifact:
  Apache-2.0 `EverMind-AI/Raven` (252 MB, created 2026-05-21, pushed 2026-09-29,
  4,454 stars, LICENSE on disk) plus the Apache-2.0 `EverMind-AI/EverOS`
  companion (13,270 stars), a live project page, and a DAG-based Host Agent
  orchestration contract with session-unique node IDs, dependency sets, cost
  accounting, and `SKILL.md` procedures. **Excluded anyway:** it is authored by
  the vendor collective "EverMind AI" with no named individuals, the repository
  predates the paper by about four months and is a shipped product whose release
  PDF is the same document as the arXiv paper, and the headline claims
  ("All-Domain", "significantly outperforms the state of the art") are not
  falsifiable from the artifact. Recorded here as **verified open** so a later
  run can revisit it without re-verifying the links.
- **PluginRSI** (`2609.32423`, cs.AI; v1 2026-09-26) — atomizes a harness into
  plugins behind standardized interfaces, improves them independently, and
  recombines them from a shared library; a genuinely relevant mechanism, but the
  **advertised repository `syr-cn/PluginRSI` returns 404** (the account exists
  with 30 public repositories) and Appendix A claims an anonymous repository
  "linked in the abstract" that the abstract does not contain. Excluded as a
  concept with no retrievable artifact; **watch** for the repository appearing.
- **Vestrum** (`2609.33822`, cs.AI; v1 2026-09-27, "Under review") — turns
  execution-trace failures into scope-screened harness changes across
  verification, retrieval, decomposition, and knowledge synthesis, with a
  persistent lessons file and reported held-out gains (UltraHorizon 47.6 → 59.8,
  Terminal-Bench 4 Hard 63.7% → 70.3% of checks at 1.03× cost, cell-type
  annotation agreement 67.5% → 77.8%). Excluded as a trace-failure-to-harness-edit
  loop that overlaps the curated Self-Harness and Evo-Harness line, with **no
  artifact**, no HTML full text, and no robot evidence. **Watch** for a release.
- **Where Memory Belongs: Ledger** (`2609.34554`, cs.RO; v1 2026-09-28) — splits
  memory by type, keeping short-term perceptual memory in the policy and
  long-term object memory outside it as an explicit ledger read by an LLM
  planner, reporting a 64.3% four-suite RoboMME average against 45.9% for the
  strongest prior method. A harness-shaped contribution, but **no artifact** and
  simulation-only. **Watch.**
- **ActionGround** (`2609.33256`, cs.RO; v1 2026-09-27) — a neuro-symbolic,
  training-free runtime layer that wraps a frozen VLA with a phase-aware finite
  state machine and an inertia-weighted Euler–Lagrange term at under 1 ms per
  control step, reporting up to +6 points success and +19.3 points stability
  across four VLA backbones on LIBERO-Spatial. **No artifact** anywhere, and the
  physical-hardware result is presented as a qualitative deployment
  demonstration. **Watch.**
- **Find Something You Can't Do (FIND)** (`2609.32069`, cs.RO; v1 2026-09-25) —
  agentic real-world RL that uses the post-rollout scene to choose what to
  practice next, with a vision-language agent selecting feasible tasks by recent
  success rate and evaluating outcomes from paired pre/post observations;
  reported human-assessed success **55% → 71.9%** over eight real-world tasks,
  456 autonomous episodes in six hours with no human reward labels. Excluded
  because **no first-party repository exists**: the abstract's `FIND.github.io`
  resolves to an unrelated site, and the real page's "Code" button links to
  itself. **Watch.**
- **ARS: Agentic Reward System for Robot Learning** (`2609.34484`, cs.LG/cs.RO;
  v1 2026-09-28) — inference-time progress-reward modeling with a proposing
  subagent and a verifying primary agent, including auditing of external reward
  models, evaluated on a semantic-mismatch benchmark, simulation policy
  learning, and real-robot multi-screw fastening. The paper states "Code is at
  https://github.com/midea-ai/ars", which returns **404** — promised, not
  released. **Watch.**
- **DexAgent** (`2609.35318`, cs.RO; v1 2026-09-28) — agentic
  Human2Sim2Robot pipeline with a self-evolving tool library and property-specific
  verifiers; the project site is live but states "Code (Coming Soon!)".
  Promised, not released. **Watch.**
- **F4R** (`2609.35575`, cs.AI/cs.RO; v1 2026-09-28) — failure-driven
  real-to-sim-to-real loop reporting 93.75% in-distribution and 90.0%
  out-of-distribution real-world success without new corrective demonstrations;
  the release statement is "Code will be released soon". **Watch.**
- **RE-0** (`2609.32416`, cs.AI/cs.LG/cs.RO; v1 2026-09-26) — verified recursive
  improvement of embodied code-as-policy agents, where only counterfactually
  verified teacher interventions supply distillation supervision. "We will
  release the complete pipeline and all artifacts" — future tense. **Watch.**
- **SEES** (`2609.32698`, cs.RO; v1 2026-09-26) — self-evolving embodied system
  that monitors atomic-task outcomes, restores simulator states, and runs online
  RL on the most frequently failing family adapter. A policy-training method
  rather than a runtime or interface contribution, **no artifact**, and the
  real-world evaluation is explicitly future work (simulation-only). Excluded.
- **MetaBench / benchmark-harness family, remaining 2026-09-29 records** — the
  benchmark-construction and dataset-generation papers in the block
  (`2609.34428` AgentHop, `2609.34710` FromPitch2Board, `2609.34683`
  AgentPerfBench, `2609.34790` CoSec, `2609.35026` WebPageBench, `2609.35639`
  GPUPhysBench, `2609.34214` GlyphBench, `2609.34492` PowerBench, and the
  remainder of the 863-record eval sweep) are evaluation resources for digital
  or non-robotic domains, or benchmarks without a harness/runtime contract;
  excluded under the standing scope rule.
- **VLA, world-action, and robot-method papers screened and rejected this pass** —
  `2609.31904` GT-VLA, `2609.35709` discrete-VLA humanoid loco-manipulation,
  `2609.34792` D²-VLA, `2609.33575` SLIP-VLA, `2609.32253` DS-VLA,
  `2609.34982` ActionUNet, `2609.34724` DexWeave, `2609.34199` WB-WAM,
  `2609.35450` Uni-VLaT, `2609.33148` DroneWAM, `2609.33299` AquaWAM,
  `2609.33177` DeltaWAM, `2609.10040` Efficient-WAM, `2609.33748` AnyStep-WAM,
  `2609.33269` Q-WAM, `2609.34250` WAM-OPD, `2609.35311` RoGSW4RLD,
  `2609.34414` From World Models to World Action Models, `2609.34362`
  FutureDuet, `2609.34687` VCN-Bench, `2609.34256` UMR, `2609.34286` Dexterous
  Tactile World Model, `2609.33765` principal steering subspaces,
  `2609.34893` ECHO, `2609.35200` ReCAT, `2609.32453` DRAM, `2609.32155`
  RecastVLA, `2609.23580` TaskAnchor, and the remainder of the 263-record robot
  sweep — model, representation, or policy-training contributions without an
  external runtime, interface, recovery, safety, or evaluation contract.
- **Genuine-world / domain papers excluded under the standing scope rule** —
  surgical robotics (`2609.34823`, `2609.33237`, `2609.25642`), aerial and
  planetary autonomy, autonomous driving, tactile-sensor and mechanism design,
  motion planning, and the whole-body control papers in the robot sweep. They are
  legacy or domain contributions rather than harness contributions, notwithstanding
  individual technical merit.
- **Harness-adjacent digital-agent papers screened and rejected** — `2609.33260`
  CORTEX (verified-experience layer; contract without an artifact), `2609.34649`
  ContextEvo (context policy learning; overlaps the harness-evolution line),
  `2609.33180` REUSE (risk-controlled RSI evaluation; no link, and the list's
  measurement line already carries Phantom Gains and HarnessOpt-Bench),
  `2606.25447` The Interplay of Harness Design and Post-Training (v2; harness as
  a controllable dimension in ALFWorld, but no artifact and no robotics),
  `2609.34215` recovery-evaluation set-agreement critique (a measurement
  contribution already covered by the RecoveryBench line),
  `2609.32528` RADAR/"Fail Loudly", `2609.32391` SCLATE, `2609.32091` memory
  middleware, `2609.32691` SilentCall-adjacent tool-call mediation harness, and
  `2609.34366` When Harness Beats Scale (workshop system description).

### Carried watch list — unchanged this window

Carried forward without status change (none of the thirteen IDs appears in this
block; checked by ID against both the new block and the wide window): vla.simd
(`2609.24274`), LIBERO-VPro (`2609.24350`), ARSTAG (`2609.24563`), React When
You Need To (`2609.22587`), SafeStage (`2609.21223`), AWM-3DFM (`2609.21502`),
Toollery (`2609.22218`), CHART (`2609.22247`), DTOC (`2609.26121`), MATE
(`2609.26520`), HABILIS Brain 0 (`2609.25558`), Passive→Active Exploration
(`2609.29091`), and DA-GRD (`2609.29065`). New watch entries from this pass are
recorded inline under "Screened this pass and rejected" (PluginRSI, Vestrum,
Ledger, ActionGround, FIND, ARS, DexAgent, F4R, RE-0). The artifact placeholders
recorded on 2026-09-25 (Maithili/GAP's 1 KB repository, HEXIS's HTTP 401
anonymous link, the RoboRecover dataset "being prepared for Hugging Face", and
HarnessPAI's "not yet publicly released" code) are unchanged.

## Validation performed

- `git diff --check` clean (no whitespace errors, no conflict markers); zero
  conflict-marker lines in `README.md`.
- Markdown structure re-checked after every insertion: all top-level `- [`
  bullets have balanced parentheses and brackets with zero imbalance, every
  bullet has an even number of `**` and backtick markers, and the section
  headings and the Contents list are unchanged.
- **Every link added or rewritten this run was fetched on 2026-09-29 and returns
  HTTP 200**: 13 arXiv abstract pages, the `TokenRhythm/claw-swe-bench`
  repository and the `TokenRhythm/Claw-SWE-Bench` dataset (ungated, MIT,
  2,837 downloads), the `lyhkk/CodeActionBench` repository, the
  `RLE-Bench/RLE-Bench` repository and its leaderboard, the
  `billzhao1030/NavHarness` repository, and the RoboFoundry project page.
  Placeholder and 404 links deliberately recorded as such were verified to fail:
  `robofoundry2026/RoboFoundry` content, `syr-cn/PluginRSI`,
  `midea-ai/ars`, and the CodeActionBench trajectories dataset.
- Repository licenses, sizes, creation and push dates, default branches, stars,
  and top-level trees were read from the GitHub API; Hugging Face gating,
  license, downloads, and last-modified from the Hub API; project pages and full
  texts were fetched directly; the LiteEvo availability claim was tested against
  the downloaded arXiv e-print source.
- Dates checked for cross-file consistency: the README badge, the "Last
  verified" line, and this record all carry **2026-09-29**.
- Categories checked against the arXiv record for every included entry.
- Cross-file consistency: each of the four Current Landscape bullets refers only
  to entries now present in the main list, and the provenance caveats stated in
  the README (LiteEvo's unsubstantiated availability claim, Hearsay's anonymized
  artifact, RoboFoundry's placeholder repository, CodeActionBench's missing
  dataset, RLE-Bench's simulation-only scope, and the several entries with no
  artifact at all) match this record.

## Operational notes and blockers

- `/tmp` is per-command in this sandbox, so harvest XML, parsed JSON, dossiers,
  full texts, project pages, and logs were kept under
  `.scratch/arxiv-2026-09-29/` (untracked, never staged).
- The export API's `submittedDate` range syntax required **both** percent-encoded
  brackets/spaces **and** `curl --globoff`; without them curl rejects the URL and
  the call silently yields nothing. With both, the endpoint returned HTTP 200 for
  the 2026-09-29 cs.RO window but **zero entries**, then began returning **HTTP
  429** for every window, including the positive control on an older date, after
  the five parallel verification agents used the same endpoint. The OAI-PMH
  harvest is authoritative and unaffected.
- Verification was split across five parallel agents; the unauthenticated GitHub
  API limit (60 requests/hour) was exhausted by one of them, so that agent read
  the remaining repository trees from GitHub HTML tree pages after capturing all
  required API fields live. No verdict depends on the fallback.
- `2609.33822` has no arXiv full-text HTML (HTTP 404); it was verified from the
  PDF instead, which contains no URLs at all.
- No conflict occurred; `git fetch origin`/`git pull --ff-only` were run before
  committing and the working branch is `main`.

## Commit

Task-owned files committed on `main`: `README.md`,
`sources/daily-arxiv-2026-09-29.md`. Unrelated user changes
(`docs/reference-architecture.md`, `docs/ring-harness.png`, `handoff.md`,
`.scratch/`) were left untouched and unstaged. No branches, no pull requests, no
force-push.
