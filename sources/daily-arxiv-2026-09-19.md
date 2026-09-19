# Daily arXiv scan — 2026-09-19

## Scope

- **Interval:** everything announced after the 2026-09-18 run's cutoff. That run
  (started ~07:30 UTC on Friday 2026-09-18) screened the announcement block whose
  OAI datestamp is **2026-09-18** — 905 unique base IDs, newest base ID
  `2609.20822`. This run started ~17:25 UTC on **Saturday 2026-09-19** and covers
  everything announced since.
- **No new announcement block landed.** The OAI harvest for the two-datestamp
  window `from=2026-09-18&until=2026-09-19` (sets `cs:cs:RO|AI|CL|CV|LG`) returned
  **1,235 memberships = 907 unique base IDs, every one carrying datestamp
  2026-09-18 — zero records carry 2026-09-19.** Per-category memberships for the
  block: cs.AI 350, cs.LG 328, **cs.RO 195**, cs.CV 191, cs.CL 171, stat.ML 38,
  cs.CR 37, cs.IR 25, cs.HC 23, cs.SE 22, cs.MA 21, cs.SY 21, eess.SY 21,
  cs.CY 17.
- **This is the required "unchanged batch" case.** The newest visible block is the
  same one the 2026-09-18 run screened, so — as the procedure requires — the
  **entire 907-record block was re-screened once** rather than assumed settled.
  The re-screen produced four README additions that the first pass did not carry
  (below); no other verdict changed.
- **Delta against the previous run's harvest (exact):** 905 → 907 unique IDs;
  **2 IDs appeared** (`2609.16393`, `2609.16597`) and **0 disappeared**; **1
  record's parsed content changed** (`2605.26163`, abstract +1 byte — an
  out-of-scope wireless-communications paper).
- **ID frontier unchanged.** Newest base ID in the block is still **`2609.20822`**.
  A direct existence probe on `arxiv.org/abs/<id>` confirms `2609.20823` exists
  (it is `quant-ph`, "Environment Alignment and Redundant Record Formation in
  Imperfect-CNOT Quantum Darwinism", outside the five target categories) while
  `2609.20824`, `2609.20825`, `2609.20826`, `2609.20830`, `2609.20900`, and
  `2609.21000` all return **HTTP 404**. No IDs have been assigned past the
  frontier, so nothing is being held back by the harvest.
- **Composition of the re-screened block (907 unique IDs), by the submission date
  of the announced version** (OAI `<created>`): **615 carry 2026-09-17**, **156
  carry 2026-09-16**, **9 carry 2026-09-15**, and the remainder reach back to July
  2026; **254 records carry pre-`2609.` IDs**, the signature of re-announced
  replacement versions. (The 09-15 group gained one record versus the previous
  run: new ID `2609.16597`, created 2026-09-15.)
- **Export-API cross-check.** The `submittedDate:[20260917000000 TO
  20260917235959]` window across the five categories returns **541 raw entries**
  (cs.RO 98, cs.AI 153, cs.CL 71, cs.CV 94, cs.LG 125) — identical to the 541 the
  previous run measured, and a subset of the 907-ID block. The
  `submittedDate:[20260918000000 TO 20260918235959]` window returns
  `opensearch:totalResults` **0 for all five categories**: Friday's submissions
  (2026-09-18) have not been announced yet, which is the expected weekday lag and
  confirms the block boundary rather than a truncation.
- **Weekend lag, consistent with the repository's own history.** 2026-09-19 is a
  Saturday; the 2026-09-12 record documents the same pattern (a Saturday run with
  an index that had not advanced), and the 2026-08-30 and 2026-09-07 records
  document the matching Friday/weekend behaviour. The next genuinely new block is
  expected with the next weekday announcement.
- **Late-batch re-check:** not applicable in the usual sense — the previous day's
  range was re-harvested inside the same OAI window (the whole 907-ID 09-18 group
  is the current block), and the frontier probe above shows no newer IDs exist.
- **Withdrawals in this block:** five records carry a withdrawal notice
  (`2608.04765`, `2604.25323`, `2606.01670`, `2609.19422`, `2609.20217`) — exactly
  the set the 2026-09-18 run recorded, two of them in scope and already filed.
  **No curated README entry is affected by a withdrawal.**

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`, `from=2026-09-18&until=2026-09-19`), one request per
   category with resumption-token exhaustion (each category returned a single
   page), saved to `.scratch/arxiv-2026-09-19/oai-*.xml` (1,235 memberships).
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`oai-records.json`).
3. **Delta against the previous run's parsed harvest** (`.scratch/arxiv-2026-09-18/oai-records.json`):
   set difference for appeared/disappeared IDs and field-wise comparison for
   content changes, so the two new records and the one changed record were read
   individually.
4. **Export-API cross-check** over the 09-17, 09-18, and 09-19 `submittedDate`
   windows per category, used strictly as a subset/boundary check (the API cannot
   see held, cross-listed, or replacement records).
5. **Full re-screen of the whole 907-record block** (`.scratch/arxiv-2026-09-19/rescreen.py`):
   keyword-breadth ranking over title + abstract for harness, agentic,
   self-evolution, runtime, tools, skills, memory, recovery, safety, evaluation,
   robot, VLA, world-model, and policy signals; a dedicated scan of every block
   title containing *harness* (8 records); and a novelty pass that cross-references
   every block ID against the 2,630 arXiv IDs already mentioned anywhere in
   `README.md`, `docs/*.md`, or `sources/*.md`. **816 block IDs have never been
   mentioned in the repository; 347 of those carry core-signal hits**, and each of
   those 347 was read at title/abstract level.
6. **Dossiers and primary-source verification** for every shortlisted record: full
   title/abstract/comments, then the arXiv abstract page, the arXiv full-text HTML
   (artifact links extracted), and the official GitHub / Hugging Face APIs (license
   field, size, creation and push dates, default branch, top-level tree) on
   2026-09-19.
7. **Duplicate screening** of every candidate against `README.md`, `docs/*.md`, and
   `sources/*.md` by arXiv ID before writing.

## Included (README updates)

Four entries added, plus three "Current Landscape" bullets. Artifact status is
reported literally; all four artifacts were verified live on 2026-09-19.

### General Harness Design and Self-Improvement

- **Chronicle: Cut-Point Replay for Regression Testing of LLM Agents**
  (`2609.20625`, cs.AI/cs.CL) — a *verification* primitive for agent harnesses:
  record a run at its non-deterministic boundaries (model calls, tool calls,
  routing) as immutable envelopes, then replay with a chosen subset of boundaries
  served from the record while the complementary subset executes live with new
  code, turning a production incident into a regression test that runs in CI with
  zero model calls. Per-name call-count guards detect contract drift and flag a
  fixture for re-recording. The authors report 23 µs recording overhead per
  crossing, bit-stable full replay across 20 repetitions, 6/6 fault detection
  (unguarded code fails; guarded and benign rewordings pass) versus a
  stub-every-boundary baseline that passes everything, and 51/192 tool mutants
  killed versus 0/192. Artifact: the MIT-licensed
  [`theagentplane/chronicle`](https://github.com/theagentplane/chronicle)
  (created 2026-06-17, pushed 2026-09-19; `chronicle/`, `docs/`, `examples/`,
  `fixtures/`, `tests/`, PyPI package `agent-chronicle`, CI) plus the released
  six-incident benchmark. Evidence is digital-agent only, the model boundaries in
  the released benchmark are simulated, and the authors state the guarantee is
  conditional on boundary-annotation coverage.
- **SkillAA: Attribution-Guided Skill-Graph Updating with Targeted Validation and
  Rollback** (`2609.20455`, cs.AI) — makes the *edit surface* of a skill library
  explicit: one graph serves activation, execution, composition, failure
  attribution, and validation; a diagnosed root cause maps to the only graph
  object authorized to change; candidate patches stay local; and every patch must
  pass a Local Gate (graph-scoped retest) and a Big Gate (epoch-level commit rule)
  before commitment, with rollback of rejected patches. The authors report the
  highest observed mean in every main model–benchmark setting (81.5% SearchQA,
  66.7% LiveMath, 91.2% DocVQA with `gpt-5.6-sol`) and 1.3–20.7 pp gains over the
  strongest alternative in each row, with progressive ablations. Artifact: the
  official [`Ziqiao-Shang/SkillAA`](https://github.com/Ziqiao-Shang/SkillAA)
  repository (created and pushed 2026-09-17, 2.6 MB; `graphopt/`, `configs/`,
  `data/` with split metadata, `scripts/` including `verify_release.py`, `tests/`)
  — **no license is asserted** as of 2026-09-19. Digital QA benchmarks only.

### Benchmarks and Evaluation

- **VA-Bench: Measuring Embodied Spatial Intelligence through Visual
  Demonstrations, Active Perception, and Metric Control** (`2609.19554`,
  cs.CV/cs.RO) — an evaluation harness for the full observe–reason–act–revise
  loop: a general-purpose MLLM reads an RGB-only demonstration, chooses camera
  viewpoints to acquire missing evidence, and emits metric Cartesian
  end-effector commands that a *fixed model-agnostic controller* executes, with no
  privileged object poses, oracle trajectories, or learned action heads. 14 task
  families (11 single-arm, three dual-arm), 20 physically verified seeds each,
  seven held-out geometry/layout variants, a five-object long-horizon composition
  track, nine behavioral diagnostics, and subtask-progress checkpoints. Reported
  best results: Qwen3.8-max 65.61% single-arm, GPT-5.6-sol 22.22% dual-arm, best
  overall macro-average 53.93 ± 3.17% over three runs, with the top three means
  within 2.38 pp and overlapping ranges (no reliable ordering claimed). Artifact:
  MIT-licensed
  [`zhangzhongbo2213/VABench`](https://github.com/zhangzhongbo2213/VABench)
  (44 MB, created 2026-09-16, pushed 2026-09-19; task configs, environments,
  `agent/`, `configs/`, `docs/`, `demos/`, `RELEASE.json`, `SHA256SUMS`).
  **Physics-simulated only** (no physical robot), and success is decided by the
  environment checker rather than a judge.

### Observability and Replay

- **AgentPProf: Semantic Profiler for Long Horizon AI Agents** (`2609.20301`,
  cs.AI) — argues agent observability needs *profiling*, not only per-run
  debugging, and supplies the attribution layer: uniform operations plus a
  semantic operation stack replace the runtime call stack, recursive operation
  segmentation splits a trajectory at task boundaries to produce the stable
  identifiers aggregation requires, and unresolved parent links are left
  unassigned (fail closed) rather than inferred. Output is pprof-compatible, so
  profiles can be re-weighted by tokens, duration, files, or network and rendered
  as flame graphs, including one task folded across sessions. Reported: 0.764 B³
  F1 against independent human stage annotations on all 405 CodeTraceBench
  trajectories (0.663 / 0.541 for two baselines), MAP gains of 0.031/0.107/0.117
  on three localization workloads, and a profile-only repair case in ToolSandbox.
  Artifact: the MIT-licensed
  [`eunomia-bpf/agentsight`](https://github.com/eunomia-bpf/agentsight)
  repository carries the profiler under `ext/pprof` (Rust Cargo project plus
  `docs/agentpprof.md`), verified live on 2026-09-19; the wider repository
  (created 2025-07-07, pushed 2026-09-13) is the AgentSight observability project.
  Digital coding and web agents only.

### Current Landscape additions

Three bullets, one per theme: **verification as a replayable artifact**
(Chronicle — a recorded incident committed as a model-call-free CI regression
test); **observability moving from per-run tracing to cross-run profiling**
(AgentPProf — pprof-compatible semantic flame graphs over trajectories); and
**embodied evaluation packaged as a model-agnostic loop** (VA-Bench — fixed
controller and checker, MLLM-side perception and metric control, dual-arm
coordination far below single-arm success).

## Rejected / watch list

### Re-screen of the unchanged block

- **Harness-titled records (8):** `2609.15188` (MUSE, theory-harnessed story
  engine — a narrative-generation "harness", out of scope) and `2609.18620`
  (DeformSmith — "harness" here is a physics solver; already excluded 2026-09-17)
  are the only two not already curated; `2609.19413`, `2609.19974`, `2609.20474`,
  `2609.20519`, `2609.20804`, and `2609.20822` are existing README entries from
  the 2026-09-18 run. No harness-titled record was missed.
- **Two IDs new to the block:** `2609.16393` (ParsHate — Persian hate-speech
  benchmark, cs.CL) and `2609.16597` (brain-tumour vision-language foundation
  model, cs.AI/cs.CV). Both out of scope; no robot, agent-harness, or evaluation
  contract.
- **One content-changed record:** `2605.26163` (adversarial water-filling for
  wireless foundation models) — abstract grew by one byte; out of scope.
- **Everything else in the block:** the 2026-09-18 record's per-item rejected and
  watch lists remain the authoritative verdicts for the 905 IDs it screened (28
  revisions re-read, 12 inclusions, the rejected/watch entries listed there), and
  the re-screen of the full 907-record block changed none of them.

### Candidates newly surfaced by the re-screen, verified and rejected

Each was read in full and checked against the arXiv record, full-text HTML, and
(where a link exists) the GitHub/Hugging Face API on 2026-09-19.

- **EconSkills** (`2609.19523`, cs.AI/cs.CL): a skill library and evaluation
  framework for web agents on live economic data — 50 procedures, each recording
  scope, navigation procedure, site-specific guidance, **verification checks, and
  recovery steps**, with transfer and library-selection measured separately.
  Artifact is real: Apache-2.0 Hugging Face dataset
  `EconWebArena/EconSkills` (last modified 2026-09-18). Excluded this window as a
  domain skill-library dataset for digital web agents; the skill-library line is
  carried by SkillAA, SkillGate, and COBRA-Skills in this update. **Watch** if a
  robot or general-harness instantiation appears.
- **DeliveryGym** (`2609.19801`, cs.LG): 3D RL environment for long-horizon
  embodied courier planning with persistent world dynamics, trajectory rewards
  from simulator events, and an adaptive training curriculum (RL improves
  Qwen3-VL-4B net income by 54.3%; adapted training +16.5% over uniform sampling).
  An environment/benchmark contribution with real potential, but **no repository,
  dataset, or environment release could be located** in the record or full text.
  Treated literally as not open; **watch for the environment release**.
- **SPAR: A Simulation Platform for AUV Fault Recovery** (`2609.20620`,
  cs.AI/cs.RO, AUV 2026): a closed-loop architecture in which deterministic layered
  control runs the vehicle while an invokable LLM acts as diagnostic and recovery
  planner, wrapped in an evaluation harness that couples real-time C vehicle
  software to fault injection, structured prompting, mission-file generation,
  validation, execution, and LLM-judge scoring across 480 mass-shift trials. The
  harness shape and the "ensemble testing rather than individual demonstrations"
  argument fit this list's recovery line, but **no artifact was located** and the
  evidence is simulation-only with a small frontier-vs-local model comparison.
  **Watch.**
- **Diagnose, Recover, Certify: Task Readiness under Hidden Dynamics Changes**
  (`2609.20304`, cs.AI, created 2026-07-30): dormant-dynamics-drift readiness with
  evidence-gated matched-pulse transport, task-conditioned policy recovery, and a
  calibrated readiness lower bound with **abstention to a safe fallback** —
  conceptually close to a recovery/certification contract, evaluated on
  dormant-actuator MuJoCo benchmarks. The paper states the code and data "will" be
  released upon acceptance and are withheld during review, so the artifact status
  is literally **not open**. Simulation-only. **Watch for the release.**
- **When AI Agents Commit: Cognitive Serializability Across Data, Evidence,
  Policy, and Authority** (`2609.20261`, cs.AI/cs.DC, created 2026-07-28): typed
  dependency tokens, sealed envelopes, guard-first commit transactions, and
  effect-compatible admission for agentic transactions. A substantial conceptual
  extension of the transactional-agent line, but a 22-page theory/architecture
  paper with **no artifact or implementation endpoint**, digital-only; the line is
  already carried by Agentic Transaction in the README. Excluded.
- **Reach or Solve? Attributing Agentic RL Gains with Checkpoint Handoffs**
  (`2609.19636`, cs.AI): an evaluation protocol that clones a state one released
  checkpoint reached and hands it to another, splitting an endpoint gain into
  REACH and SOLVE; reports that restricting comparison to states both policies
  reach flips the sign of the effect. Useful measurement discipline and a good
  candidate for the evaluation section, but **no artifacts were located** and the
  evidence is digital-agent only. **Watch.**
- **Refuse, Decompose, Refresh: A Claim-Safe Protocol for Closed-Loop AI
  Evaluation** (`2609.20538`, cs.AI): abstain when a clean reference stream or
  matched runtime comparison is unsupported, decompose protocol execution from
  operational false admission and structural hypotheses, and treat distribution
  shift as a request to recompute the reference map. Close to this list's
  evaluation-contract line, but the "reproducibility artifact" is an
  **anonymous-review URL**, which is not an official release, and the evidence is
  an aggregate-only simulator. Excluded; **watch for a stable artifact**.
- **TraceFlow** (`2609.20646`, cs.RO): turns retrieved successful and failed
  rollouts into a bounded guidance field for a **frozen** flow-matching VLA using
  only a terminal outcome bit per rollout (real-robot packing order completion
  21/50 → 39/50, and 47/50 after one stacking round). A test-time guidance method
  rather than a runtime/interface contribution, with **no artifact located**;
  excluded under the standing VLA-model policy. **Watch for the TraceBank
  release.**
- **RAFT** (`2609.20754`, cs.AI, EMNLP 2026 Industry): stateful
  retrieval-augmented troubleshooting for enterprise support; entry-level
  retrieval over case timelines. Out of scope (no robot, no harness contract).
- **Screened by title and category, set aside by the standing policy:** the
  remaining records the novelty pass surfaced — conventional manipulation,
  locomotion, navigation, SLAM, control, gripper/actuator design, medical, remote
  sensing, autonomous-driving, and domain-agent papers — including
  `2609.19803` (HEROIC cross-robot identification), `2609.20694` (HOPHY off-road
  mission planning), `2609.20388` (Navi-Agent monocular navigation), `2609.20191`
  (VLN on the Fly onboard aerial stack), `2609.19656` (Self-Evolving Search
  Index), `2609.19526` (SIFT — self-improvement tree search, no artifacts
  located), `2609.19690` (UniExo), `2609.19961`/`2609.19538` (UAV networking), and
  `2609.19236`/`2609.19617` (domain agent frameworks). None introduces a reusable
  agent/VLA harness, recovery, safety, or evaluation contract beyond what the list
  already carries.

### Watch-list re-checks (no change)

- **FIERCE** (`github.com/ar-mine/FIERCE`) still ships only `README.md`;
  **ContrAgent** (`github.com/yfxiao16/ContrAgent`) still returns 404. Neither
  released an artifact in this window.

## Operational notes

- **Export-API rate limiting appeared late in this run.** The first pass at the
  start of the run returned clean results (541 entries for the 09-17 window, 0 for
  the 09-18 window). After roughly 40 API/OAI requests, `export.arxiv.org`
  answered **HTTP 429 with an empty body** for the whole 09-18 and 09-19 window
  re-check, including with 4-second spacing; the 09-19 window therefore has **no
  API-derived count** in this record, and the conclusion that no 09-19 block exists
  rests on the OAI harvest (zero 2026-09-19 datestamps), the frontier probe
  (no IDs above `2609.20823`), and the 0-entry 09-18 window measured before the
  limit. A 429 is not evidence of "no submissions", and it is recorded here so a
  later run does not read it as such.
- `curl -g` remains required for `submittedDate:[…]` queries (square-bracket
  globbing); a silent empty response is otherwise indistinguishable from zero
  results.
- The OAI route remains the block definition (1,235 memberships → 907 IDs); the
  export API sees only the 541-entry 09-17 window and is used strictly as a subset
  and boundary check.
- GitHub REST verification succeeded throughout (repos, contents, and tree APIs);
  no 403 rate limiting occurred. The `git ls-remote` fallback was not needed.
- No network, authentication, conflict, or artifact-verification blocker remained
  at the end of the run.

## Validation performed

- **Markdown structure:** `##`/`###` heading order in `README.md` unchanged; every
  heading is preceded by a blank line and no heading deeper than `###` was added;
  the new `sources/daily-arxiv-2026-09-19.md` follows the established record
  structure (Scope / Method / Included / Rejected / Operational notes /
  Validation).
- **`git diff --check`:** clean (no whitespace errors, no conflict markers).
- **Added links:** every URL added to `README.md` was requested during this run —
  the four arXiv abstract pages and `github.com/theagentplane/chronicle`,
  `github.com/Ziqiao-Shang/SkillAA`, `github.com/zhangzhongbo2213/VABench`, and
  `github.com/eunomia-bpf/agentsight` — all returned **HTTP 200**; the
  AgentPProf component path `ext/pprof` and the SkillAA/VA-Bench/Chronicle
  repository contents were read from the GitHub API rather than assumed.
- **Dates:** README badge and "Last verified" both set to **2026-09-19**, equal to
  this record's date; entry dates quoted in prose are the arXiv `<created>` dates
  from the OAI harvest.
- **Categories:** each entry's section placement follows this list's taxonomy
  (General Harness Design and Self-Improvement; Benchmarks and Evaluation →
  Agent and Embodied Reasoning; Runtime, Safety, and Observability →
  Observability and Replay) and matches the classified arXiv categories recorded
  here.
- **Cross-file consistency:** no arXiv ID added to `README.md` duplicates an ID
  already present in `README.md`, `docs/landscape.md`,
  `docs/reference-architecture.md`, or any other `sources/*.md` (checked by ID
  before writing); the README "Last verified" date equals the newest record's
  date.
- **Artifact claims:** each open/closed statement was verified against the GitHub
  or Hugging Face API on 2026-09-19 (license field, size, creation and push dates,
  default branch, and repository contents) and is described literally — including
  SkillAA's **absent license**, VA-Bench's simulation-only evidence, and the
  withheld/withheld-pending-acceptance artifacts recorded under Rejected.
