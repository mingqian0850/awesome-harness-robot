# Daily arXiv scan — 2026-10-01

## Scope

- **Interval:** everything announced after the 2026-09-30 run's cutoff. That run
  (started ~08:41 local / ~06:41 UTC on **Wednesday 2026-09-30**) screened the
  announcement block whose OAI datestamp is **2026-09-30** — 1,724 unique base IDs,
  newest base ID `2609.38180`. This run started ~08:32 local / ~06:32 UTC on
  **Thursday 2026-10-01**. **There are no missed dates:** the 2026-09-30 18:00
  catch-up slot could not have seen a new block (the next announcement lands at
  02:00 local on 2026-10-01), and no run intervened between the two.
- **A new announcement block landed.** The OAI-PMH harvest for
  `from=2026-10-01&until=2026-10-01` returned records for all five categories:
  **2,141 set memberships = 1,505 unique base IDs**, every one carrying datestamp
  **2026-10-01** (memberships: cs.LG 664, cs.AI 629, cs.CV 365, cs.CL 274,
  **cs.RO 209**). This is the Wednesday-submission block.
- **Block composition.** 1,129 of the 1,505 records carry a 2026-09-30 created
  date; **966 carry `2609.3xxxx` IDs** and **539 predate that range**, of which
  **356 do not start with `2609.`** and **27 were created before 2026-09-01**
  (re-announced replacement versions reaching back to 2023–2025). **The newest base
  ID moved from `2609.38180` to `2609.40362`**, so the ID frontier advanced by
  2,410 and the block is not being held back.
- **Previous-block re-check.** The datestamp-2026-09-30 block now holds **1,656**
  unique IDs against the **1,724** the previous run recorded for it: 68 records
  were re-announced forward into 2026-10-01. The two datestamp windows share **no**
  IDs, so nothing in this block is a duplicate of the previous block's membership.
- **Export-API cross-check (HTTPS, `curl --globoff`, percent-encoded brackets).**
  `submittedDate:[20260930 TO 20260930]` returns HTTP 200 with cs.RO 112, cs.AI 287,
  cs.CL 128, cs.CV 172, cs.LG 331 = **1,030 memberships**; the query for
  `submittedDate:[20261001 TO 20261001]` returns **0 for all five categories**,
  which is the expected indexing lag (2026-10-01 submissions are announced
  2026-10-02). The export-API slice is a **strict subset** of the OAI block:
  **0 API IDs are missing from the harvest**, while 47 cs.RO and 177 cs.AI records
  with `created == 2026-09-30` are present in OAI and not yet in the API slice.
  The **OAI-PMH harvest is the authoritative source** for every number above.
- **36 of the 1,505 new-block IDs already appear somewhere in the repository**
  (README, `docs/`, `sources/`), 12 of them at README level. Because an OAI
  datestamp of 2026-10-01 means "announced in this block" — and because the
  previous run's window shares no IDs with this one — each of the 36 was checked
  individually against its arXiv version history. **Three carried a substantive
  revision**, and all three change a recorded fact (see "Revision edits"): BATON
  (`2608.16889`, v3 — reported gains revised upward and the v2 release promise
  removed), GlanceWAM (`2608.23927`, v2 — real-robot experiments added), and
  Safety Under Scaffolding (`2603.10044`, v4 — headline statistics revised).
- **Withdrawal notices in this block: two, neither repo-relevant.** `2609.34768`
  (privacy-preserving mmWave full-body meshing; authors still finalizing scope and
  release timing) and `2605.15692` (contextual action-set RL regret bounds;
  superseded by `2609.36486`). `2608.25466`, `2609.35674` and `2607.12896` carry
  withdrawal notices under the previous datestamp and were already recorded on
  2026-09-29/2026-09-30. No ID referenced anywhere in this repository was
  withdrawn, so no entry required annotation.
- **Watch-list re-check:** of the IDs carried by the 2026-09-30 record, four
  reappear in this block — **BATON** (`2608.16889`, now v3), **RoboHarn-Evo**
  (`2609.37583`), **SelfSearch** (`2609.37968`) and **RoXDrive** (`2609.36851`,
  driving, out of scope). RoboHarn-Evo and SelfSearch are unchanged (no artifact
  URL of any kind; still on watch). RoXDrive remains out of scope. BATON's v3 is a
  substantive revision and is handled under "Revision edits".

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`) over three windows: `2026-10-01…2026-10-01` (new block),
   `2026-09-30…2026-09-30` (late-batch/boundary re-check), and
   `2026-09-30…2026-10-01` (previous block plus new block). Pages saved to
   `.scratch/arxiv-2026-10-01/oai-{new,prev,full}-*.xml`; cs.AI and cs.LG needed
   two pages in the `full` window, every other category and window returned a
   single complete page with no resumption token.
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`oai-records-{new,prev,full}.json`, `parse_oai.py`), then **delta against the
   previous run's parsed harvest** with field-wise before/after values
   (`delta.json`), including the entity-normalisation control used since
   2026-09-29 so that parser-induced diffs are not reported as record changes.
3. **Export-API range queries** (`https://export.arxiv.org/api/query`, HTTPS;
   `submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]`, percent-encoded and
   `--globoff`) for all five categories on 2026-09-29, 2026-09-30 and 2026-10-01,
   used as an independent count and a boundary check rather than as the primary
   source. Counts are reported in Scope.
4. **Screening** over all 1,505 records against a 3,227-ID corpus extracted from
   `README.md`, `docs/*.md` and `sources/*.md` (`screen.py`, `triage.py`), with
   keyword-family rankings for harness/scaffold/runtime, self-improvement,
   agentic-robot, VLA/action-expert, world/foundation-model, evaluation/safety and
   robot/embodied concepts, plus an explicit uncurated robot listing and an
   artifact-signal listing with extracted URLs.
5. **Second, independent screen** for records touching *both* robot/embodied and
   harness/agent concepts (76 uncurated records) and a cs.RO-only pass ranking the
   209 cs.RO records by harness/agent/evaluation signal, so that candidates that
   the keyword families under-ranked were not missed.
6. **Dossier verification** by seven parallel read-only agents, one per thematic
   group (robot harness architecture; code-as-policy / robot auto-research /
   self-improvement; VLA recovery and safety envelopes; general harness
   composition and discovery; harness value and runtime contracts; runtime safety
   and authorization; robot verification and memory). Each fetched the arXiv abs
   page, the full-text HTML (or recorded that it was absent), and every candidate
   project page / GitHub / Hugging Face / dataset URL, recording HTTP status; each
   was capped at 5 `api.github.com` calls and told to prefer HTML and
   `raw.githubusercontent.com`. Reports are in
   `.scratch/arxiv-2026-10-01/dossiers/out-*.md`.
7. **Duplicate check** by arXiv ID and by distinctive title phrase against
   `README.md`, `docs/` and `sources/` for every shortlisted candidate.
8. **Watch-list re-check** by ID against the new block and the wide window (see
   Scope).

## Included (README updates)

### Harnesses and Development Platforms — General Harness Design and Self-Improvement

- **STITCH** (`2609.38912`, cs.AI; v1 2026-09-30) — makes harness *composition* a
  selection-and-compilation problem instead of a generation problem: a library of
  **Harness Primitives** (harness mechanisms mined from failed trajectories, each
  with an application scope and a composition contract) is selected per task and
  compiled at test time, so no mechanism code is written or repaired at inference.
  The primitives include change-surface tracing, compatibility-envelope gating,
  contract-case exploration, execution supervision and state guarding. Reported:
  **80.5% Pass@1 on SWE-bench Verified** (vs 73.0 Mini-SWE-agent seed, 79.0 Codex
  CLI, 76.0 best fixed primitive) and **72.2% on Terminal-Bench 2** (vs 60.0 seed,
  56.7 Codex CLI), with test-time composition overhead of 2.7% of actor execution.
  **No artifact of any kind** (no repository, project page or model link; "release"
  and "open-source" appear zero times); the actor is GPT-5.6-Luna and development
  and evaluation samples are disjoint by the authors' own protocol. Digital agents,
  no robot experiment. Results author-reported; the abstract's "up to 12 points"
  is against the *seed* harness, while the gain over the strongest fixed primitive
  is 4.5 points on SWE-bench (Terminal-Bench: +5.5). Watch for a repository.

- **MILO** (`2609.38349`, cs.LG/cs.AI; v1 2026-09-29) — co-evolves an agent harness
  *and the search strategy that discovers it*: hierarchical lineage memory over
  island trees keeps rejected mutations as negative evidence, per-island mutator
  agents rewrite whole harnesses from global search history and parent-specific
  feedback, and an orchestrator adapts search through lineage grafting, speciation,
  mutator reassignment and curriculum revision. The genome is one Python file, so a
  mutation can rewrite prompts, tools, skills, memory, middleware, verification
  gates and sub-agent topology. Reported with Opus 4.8: **+12.0 / +28.3 / +10.3**
  resolution over its initial harness on Terminal-Bench 2.1 / PaperBench / DeepSWE,
  **86.1 ± 2.0%** on TB2.1 above the official leaderboard top entry, 26% fewer
  tokens, and **2.7× Mini-SWE-Agent on Frontier-Bench with no further search**.
  **Artifact placeholder:** the project page's Code control is a disabled `href="#"`
  reading "Code soon"; the paper prints its harness source in appendices, which is
  not the same as a release, and its licence is CC BY-NC-ND 4.0. Digital agents, no
  robot experiment; results author-reported.

- **Self-Evolving Harness** (`2609.38372`, cs.AI/cs.LG; v1 2026-09-29;
  single-author) — a framework close to recursive self-improvement in which **the
  same frozen model, on the same harness version, first solves tasks as the solver
  and then reads the complete run records and directly edits the harness that runs
  it**, with no separate proposer harness; edit budget is framed as a learning rate
  and evolution notes as optimizer state. This is the strongest held-out protocol
  in the batch: five in-distribution benchmarks with training and held-out tasks
  strictly separated, plus **five out-of-distribution benchmarks never used during
  evolution**. Reported: iteration 5 averages 60.17 in-distribution (**+4.48** over
  the seed, +2.99 over Codex) and **70.45 OOD (+12.64 over seed)**, with
  Terminal-Bench 2.1 66.67→83.91 and SWE-Bench Pro 51.00→61.00. **No artifact**
  (the `qzzqzzb/Self-Harness` repository the paper links belongs to a *different,
  cited* paper); digital agents, no robot experiment; results author-reported.

- **Turbo Harness** (`2609.40330`, cs.AI; v1 2026-09-30) — adapts a globally
  optimized harness to each *instance* at inference: a completed global search's
  byproducts are recycled into a structured playbook, a trained harness editor
  emits an instance-specific patch to the global harness H\*, and the frozen model
  then runs one rollout inside H\_x. It patches rather than rewrites and calls the
  editor exactly once per instance. Reported: ALFWorld 40.7→**70.7**, ScienceWorld
  35.1→42.4, DBBench 65.0→69.2, WebShop 40.0→42.0 (four-benchmark average
  50.2→56.1); SWE-smith-MR resolve 50.7→64.0 (Haiku) and 70.7→88.0 (Gemini);
  Terminal-Bench-2.1 50.5→55.5. **Real MIT-licensed code release:**
  `github.com/Tyrion58/turbo-harness` (2 commits, created 2026-09-29) ships the
  method package, committed train/val/test splits, the global harnesses H\* and
  playbooks, `docs/REPRODUCE.md` and a one-command demo — **but the trained editor
  weights are not released**. Digital agents, no robot experiment; results
  author-reported, and the only paper in its group where independent reproduction
  is possible today.

- **ScholarEvolve** (`2609.40169`, cs.AI; v1 2026-09-30) — evolves the harness from
  the **research literature** rather than from observed failures: it decomposes the
  harness into five functional modules (tool interface, context management, skills,
  memory, workflow/execution), uses topic modelling to derive distinct improvement
  strategies per module, implements and recombines them, and is designed to ingest
  new publications over time for proactive lifelong evolution. Reported on AppWorld
  Challenge with Qwen3.5-27B: task goal completion **49.6±2.89 → 63.6±0.36
  (+14.0)** and scenario goal completion 28.3→44.8; τ²-Bench Telecom with
  GPT-5.4-mini pass@1 72.7→81.9; module composition beats the strongest single
  module by 3.5 TGC. Compared against Meta Harness at the same allocated search
  budget (it leads by 9.0/10.0 TGC on Challenge, while Meta Harness *degrades*
  Mini on Telecom). **Artifact is an empty repository:** the paper says "Code is
  available at `github.com/UCSB-NLP-Chang/ScholarEvolve`", but the repository is
  0 KB, created and pushed one second apart, with no commits, README or licence.
  Digital agents, no robot experiment; results author-reported over three runs.

- **How Much of a Harness Does a Strong Agent Need?** (`2609.40303`, cs.AI;
  v1 2026-09-30) — the batch's **null result for harness engineering**, and the
  reason it belongs on this list: under an equal 24-hour budget and the same
  frontier backbone on MLE-bench and NatureBench, the authors find that
  open-source state-of-the-art MLE harnesses provide **no advantage over a single
  session of a minimal coding-agent baseline** once the harness owns execution
  (read/write/bash). The largest measured effect is the coding-agent environment
  itself; the largest search-method gap (UCB1 over Best-of-N, +2.71 pp) has a 95%
  CI crossing zero, and adding delegation, parallelism or broadcast to the
  authors' own `Malena` harness never significantly helps (base percentile 66.51
  vs 63.62/67.77/58.91). Their summary is that "the effort spent elaborating
  hand-crafted harnesses around strong models yields poor returns for current MLE
  benchmarks". **No artifact** (the only GitHub link is the third-party OpenCode
  dependency); digital agents, no robot experiment; results author-reported, with
  the authors flagging statistical power and possible tuning-effort asymmetry.

- **Mid-Harness** (`2609.39982`, cs.CL/cs.AI/cs.MA; v1 2026-09-30) — spends
  test-time compute exactly at the **model–harness boundary**: the harness samples
  N candidate actions from an unchanged generator, a verifier ranks them
  (listwise/pointwise/pairwise with a first-runnable fallback), and exactly one
  action is forwarded for execution while generator and downstream harness stay
  byte-identical. Reported with a TMAX-9B generator on TerminalBench-Lite: base
  agent Pass@1 50.00%, **64.63% with a GPT-5.6 Sol listwise verifier at N=4 and
  68.03% at N=8**; the negative control is that more action sampling **alone** buys
  almost nothing (49.32→51.02% when N doubles 4→8 under weak verification), and
  the best self-verifier mechanism is pairwise (54.76% Pass@1 at N=8), rising to
  57.14% after distilling the strong verifier's pairwise responses. Transfer holds
  in seven settings (e.g. Terminal-Bench 2.1 21.72→27.34 with TMAX-9B). **Artifact
  placeholder:** the project page's Code link is `href="#"` annotated
  "soon (internal-only)"; no weights or data. Digital agents, no robot experiment;
  results author-reported.

### Harnesses and Development Platforms — Unified Evaluation

- **Evaluating Agents Across Runtime Contracts** (`2603.01209`, cs.AI/cs.LG;
  **v3 2026-09-26**, v1 2026-03-01 under the different title "Agents Learn Their
  Runtime"; CMU / MPI-IS Tübingen and collaborators) — treats a harness-owned
  contract as the independent variable. In CodeAct-style agents, the runtime either
  **preserves** agent-created Python bindings across turns or **clears** them; task
  and tool state survive either way, so only the agent's own bindings are at stake,
  and a fixed-runtime benchmark therefore conflates task skill with compatibility
  with one contract. Reported on 25 paired held-out instances with Qwen3-8B: at a
  slack per-turn tool-call cap (c=80) the mismatched persistent-trained/stateless-
  deployed agent keeps quality (0.61 vs 0.77 matched) but spends **162k
  tokens/episode, 4.4×** the matched condition; at a binding cap (c=25) the
  mismatched agent collapses to **0.07 vs 0.66**, issuing 681 inspect calls to add
  2.1 new items and exhausting the turn limit in 23/25 episodes. The cap-moderation
  effect is +0.31 [+0.04, +0.57] and its sign replicates across three training
  seeds, an independent rollout, and retrained Mistral-7B and Llama-3.1-8B.
  **Verified open:** `github.com/TieuDaoChanNhan/runtime-contract` (Apache-2.0,
  Python, created 2026-09-21) plus **14 released LoRA adapters** and two Apache-2.0
  Hugging Face datasets (`runtime-contracts/evaluation-traces`,
  `runtime-contracts/teacher-traces`, 6,000 rows); the authors state the raw
  teacher responses and decoding seeds are not released, so the released traces —
  not a re-run — are what fixes the reported values. Digital agents, no robot
  experiment; results author-reported. **Note the version history:** v1 is a
  different paper with a different author list and a 3.5× headline; cite v3.

### Robot Agent Systems — Agentic Robot and VLA Harnesses

- **DynaHarness** (`2609.40306`, cs.RO/cs.AI/cs.LG; v1 2026-09-30; NTU and Nanjing
  University) — gives a **physical execution contract** the authority rather than
  the planner: a Qwen3-VL-4B "slow brain" proposes a capability plus symbolic
  arguments on demand while a **2 Hz "fast brain" grounds, monitors, refuses
  unresolved commands, substitutes capabilities and requests replans** over a 20 Hz
  controller driving a **frozen π0.5 VLA**, analytic contact skills and recovery
  skills. The contract bounds every accepted command by budget and lease, writes an
  append-only evidence store, and that evidence drives attribution over 13 ordered
  layers plus paired regression checks that admit or reject capability revisions —
  one rejected round kept a genuine local gain (hinged-door capability, microwave
  cell 0%→25%) while reverting another. Reported: **75.2% on LIBERO-Pro over 800
  newly sampled initial states vs 17.5% for frozen π0.5** (473 vs 11 exclusive
  wins), 74.25% on the 800-episode development block vs 20.3% PhyAgentOS and 6.0%
  Harness-VLA, 77.0% on 261 untouched official states; paired execution ablations
  give full 74.0% vs 63.9% nominal one-step replanning and 16.6% with no analytic
  contact skills. **Real robot:** a UR7e with RealSense D435/D405 and SAM3
  segmentation, four tasks × 10 trials (ring placement, cup stacking, bread
  placement, drawer refusal-before-motion). **Artifact dead:** the project page's
  Code button points to `github.com/Denghaoyuan123/DynaHarness`, which returns
  **HTTP 404** (the author's account exists and hosts only `Dynaharness_page`); no
  weights or data. Results author-reported; 88 of 800 episodes lost to workcell
  connection drops are counted as failures, and the authors' own appendix notes an
  archived 74.25% aggregate against a paired remeasurement of 74.1%.

- **RoboHarness** (`2607.18060`, cs.RO; **v3 2026-09-29**, v1 2026-07-20; **Best
  Paper Award, ECCV 2026 Agent in the World Workshop**) — encapsulates
  *independently developed heterogeneous policies* — a VLA, a world-action model,
  an RL policy and a TAMP planner — as reusable agentic skills behind one
  interface, and owns capability-aware task decomposition, policy routing and
  execution memory. Its distinguishing mechanism, **Memory Bridge**, makes policy
  handoffs reliable without joint retraining by retrieving anchors from the
  incoming policy's episodic memory, fitting a local progress estimator, and
  synthesizing a handoff trajectory that trades predicted progress against motion
  cost. Reported: LIBERO-Plus **91.9%** average (first in 6 of 7 perturbation
  categories), LIBERO-LoHo 98.2% progress / 96.0% success, RMBench 77.0% on M(1)
  and 77.5% on M(n) (vs π0.5 14.4/5.5 and Mem-0 52.8/28.5), BEHAVIOR-1K six-task
  SR 0.433 / PS 0.658, and a 500-instance LIBERO-Complex suite it introduces.
  **Real robot:** a UR5e with two RealSense D435 cameras, **135 trials** across five
  structure-construction task classes and four execution-time disturbance
  settings. **No artifact of any kind** — no project page, repository, weights or
  data anywhere in the paper — and the paper is CC BY-NC-SA 4.0. **Name collision:**
  an unrelated, MIT-licensed `LZY-1021/RoboHarness` (arXiv 2603.24060) exists under
  the same short name; this entry is keyed to `2607.18060`. Results author-reported.

- **NavHarness: Adaptive Goals for Agentic Vision-Language Navigation**
  (`2609.39915`, cs.CV/cs.RO; v1 2026-09-30; Harbin Institute of Technology
  (Shenzhen) and Pengcheng Laboratory) — an agentic VLN harness of four agents: a
  **Goal Agent** sets an adaptive local goal *plus a goal-specific verification
  question*, a **Verify Agent** decides after each step whether the observed
  outcome satisfies it, a **Memory Agent** uses verified goal completion as the
  boundary for progress-aware multimodal compression of the interaction history,
  and a **Visuomotor Agent** executes toward the active goal. This is the batch's
  clearest example of verification *defining* the memory boundary rather than
  running beside it. Reported: with GPT-6-Astra, **79.0% SR on R2R-CE and 82.5% on
  RxR-CE**, +5.0 and +6.2 points over a Codex-CLI harness on matched backbones, with
  mean input images per decision falling 9.42→7.21; real world **83.3% SR / 1.51 m
  NE** over **24 trials on eight ~15–20 m routes with a Unitree Go2**. **Artifact
  absent by construction:** the project page's metadata file carries
  `"codeUrl": ""` and `"paperUrl": ""`, and its own script comment says the buttons
  stay hidden "until the authors supply the release details". **Name collision:** the
  repository already curates a different **NavHarness** (`2609.34276`, "Towards
  Lifelong Embodied Navigation", MIT-licensed); this entry is keyed to `2609.39915`.
  Compression-enabled SR is slightly *lower* than compression-off (77.0 vs 79.0), and
  the real-world baseline figures are borrowed aggregates rather than re-runs.

- **URAI — Make Code as Policy Great Again** (`2609.39018`, cs.RO/cs.AI;
  v1 2026-09-30; Hexafuture, Peking University, BIT, Tsinghua) — revisits
  code-as-policy with a different division of labour: a **programming agent writes
  reusable and task-specific robot tools** from task intent and refines them through
  execution feedback, while an **execution agent selects and parameterizes them from
  current observations**; each tool call runs a complete multi-phase motion locally
  and only then returns control, so model-level decisions still happen between tool
  executions. Validated tool revisions persist across episodes with foundation-model
  weights frozen, and the execution agent may neither read nor modify the tool code.
  Reported: aggregate RoboDojo success **18.0%→53.0%** across five tasks and four
  frozen execution agents (paired McNemar p = 7.9×10⁻⁸), and a decisive control —
  the *same* tools with an advance-written program score 24% (12/50) against 56%
  (28/50) when the agent decides after each call. **Real robot:** an AgileX
  PiPER-X dual-arm system, seven tabletop tasks, 3–10 trials each, 2.4–3.5× faster
  than published references at 4.7× fewer tokens on one task; the authors caution
  that cross-hardware ratios "are not causal estimates". **No artifact** — no
  project page, repository, weights or data; every external URL in the paper is a
  citation. Results author-reported.

- **SimEX** (`2609.38982`, cs.RO/cs.AI/cs.LG; v1 2026-09-30; Amazon FAR, UT
  Austin, CMU) — a simulation-integrated **robot auto-research harness**: it
  constructs a simulation sandbox, runs open-ended probe-and-optimize cycles in
  which the coding agent invents its own tasks and grows a reusable toolbox, then
  logs every physical trial, replays it in the corrected simulator to diagnose the
  failure, screens candidate repairs offline and spends the next physical trial on
  the winner — so a physical trial both repairs the policy and corrects the
  simulator. Privileged simulator state is withheld from the agent and reading or
  writing it is forbidden by the skill text. Reported: **26/30 real-robot
  successful trials (10/10, 8/10, 8/10) where no baseline exceeds 3/30**, using
  **5 physical trials ≈ 10 minutes of robot interaction per task with no
  demonstrations**; sim-to-sim, it matches or exceeds every external baseline on
  **all 21 held-out tasks** and reaches 85% average on the longest-horizon barcode
  family where no baseline exceeds 18%; swapping GPT-5.5→GPT-6 Astra raises it
  67%→82%, and every internal ablation reduces performance. **Artifact: project
  page only** — `robo-simex.github.io` is a static qualitative video gallery with
  no code, GitHub, dataset, paper or BibTeX link and an empty footer; no repository
  exists. Real platform: a dual-arm YAM, three tasks × 10 trials. Results
  author-reported.

- **EmbodiRSI** (`2609.38905`, cs.RO; v1 2026-09-30; ShanghaiTech University) — an
  agentic **recursive self-improvement** loop that builds task-specific simulations
  from the real deployment scene, uses them as a low-cost workspace for warm-up,
  repeatable evaluation, failure diagnosis and targeted data generation, and
  transfers back to hardware. Two mechanisms close the loop: collaborative error
  correction generating agent-assisted corrective trajectories from states the
  policy actually reaches, and adaptive collection directing expert demonstrations
  at the current policy's weaknesses; candidate data must pass a four-way gate
  (swept-mesh collision, visual review, sequential-motion feasibility, execution
  object/gripper verification). Reported: scene-balanced simulation success
  **50.4%→70.7%→83.5%** over two RSI rounds with all 14 subtasks improving, and
  **83.1% real-world** with 400 simulated plus ten real refinement trajectories per
  subtask against **75.0% for real-only adaptation using 200 real demonstrations**
  — the paper's headline data-efficiency claim, i.e. one-twentieth the retained
  physical trajectories. **Real robot:** 14-DoF dual-arm ALOHA, three tabletop
  scenes / 14 subtasks, ten physical trials per cell, with execution-time recovery
  and human takeover deliberately disabled at evaluation. **No artifact:** the
  Reproducibility Statement says the full pipeline code "will be released soon";
  nothing is released, and the paper is CC BY-NC-SA 4.0. Results author-reported.

- **Scale and Selection** (`2609.39304`, cs.RO/cs.AI; v1 2026-09-30) — the batch's
  most explicit evidence on *how to run* harness evolution. A coding agent drives a
  browser-based 3D interface as a robot policy, and a separate optimizer agent
  rewrites that agent's harness (prompts, tools, control rules) one systemic change
  at a time; the paper varies exactly two things: the number of rollouts B the
  optimizer sees per round and the promotion rule. Reported: with unconditional
  acceptance, training success rises then **falls at every batch size** (B=100:
  42%→47% at round 5→34% at round 8); with a Champion–Challenger rule, B=100 rises
  41%→60% by round 29 and held-out success goes **51%→67%**, while B=5 stalls with
  1 promotion and 27 rejections, and small batches overfit (B=10 reaches ~70% on
  training tasks but ~54% held out). The evolved harness transfers to RoboCasa
  (2/100→37/100, rounds 3–4 rejected) and to a never-seen stronger model (37%→83%
  at fixed GPT-6 Astra). **Simulation-only by the authors' own statement** ("All
  experiments are in simulation"), **no artifact of any kind**, and the ablation
  covers two variables rather than harness content. Results author-reported.

- **FailBank** (`2609.39820`, cs.AI/cs.RO; v1 2026-09-30) — converts runtime safety
  feedback into **persistent** policy improvement instead of per-action correction:
  a fixed CBF-based safety module runs as an observe-only teacher producing
  counterfactual corrections while the policy stays in control, outcome-aware
  admission converts useful proposals into corrective targets, and successful
  uncorrected actions are retained as anchors for guarded LoRA updates. This
  addresses the policy–shield mismatch that makes repeated shield interventions
  block task progress. Reported on VLA-Arena across two difficulty levels and two
  VLA backbones: task success **+8.5 and +6.9 points** with policy-induced
  cumulative cost down **35.6% and 23.8%**. **Verified open:**
  `github.com/Mingyuee88/FailBank`, Apache-2.0, created 2026-09-28 and pushed
  2026-10-01; no weights or datasets of its own (the VLA-Arena checkpoints are
  third-party). Simulation-only; results author-reported.

- **ChunkTrust** (`2609.39754`, cs.RO; v1 2026-09-30) — owns exactly one harness
  decision and turns it from a fixed hyperparameter into a latent variable: **how
  many actions to commit before replanning**. Its training-free Action-aware
  Horizon Selector combines intra-chunk spectral stability of the generation traces
  with inter-chunk continuity between executed history and the predicted prefix,
  tracked online by a Beta posterior with kernel forgetting; an optional lightweight
  Query-based Horizon Adapter learns a context-conditioned dense prior fused with
  current evidence and episode-local memory while the base policy stays frozen.
  Reported: RoboTwin 2.0 50-task π0.5 56.70→**63.50%**, an 8-task π0.5 subset
  29.63→**39.06%**, RoboCasa GR1 Tabletop Qwen3GR00T 47.83→**57.50%**, and held-out
  transfer of a QHA trained on six tasks to two unseen tasks (40.00→42.75%).
  **Real robot:** an AgileX COBOT Magic configured as an ALOHA-style bimanual system
  (four 6-DoF Piper arms, three RealSense D435 cameras), four bimanual household
  tasks, 200 teleoperated demos and 15 rollouts per task, equal-task mean normalized
  process score **50.4%→57.5%**. **Verified open (2026-10-01):** the MIT-licensed
  `hf618/ChunkTrust` repository (16 commits, pushed 2026-10-01, `src/`, `configs/`,
  `scripts/`, `tests/`, `docs/`, recorded results) plus **118 GB of checkpoints** at
  `Niugan/ChunkTrust` — with the caveats that the Hugging Face repository declares
  **no licence**, some checkpoint directories were still uploading, and the adapter
  is not a standalone policy. Regressions are reported (π0.5 regresses on Handover
  Block under QHA); results author-reported.

### Benchmarks and Evaluation — Manipulation and VLA

- **LIBERO-Agent** (`2609.39507`, cs.RO; v1 2026-09-30; Fudan University,
  Shanghai Innovation Institute) — evaluates **general-purpose agents driving a
  robot directly** rather than through a task-specific policy: seven model–harness
  pairs (GPT-6 Astra/Codex, GPT-5.6 Sol/Codex, Claude Opus 5 and Fable 5.1/Claude
  Code, DeepSeek-V4.1-Flash, Kimi K3, Qwen3.8-Max) get an episode handle, a
  published initial observation, an `osc_sequence` call executing 1–50 native
  `OSC_POSE` actions, and `finish_episode`, with observation selection, task
  decomposition and action composition left to the agent; a separate evaluation
  process holds private task checkers. A 200-task pool yields a 30-task primary
  suite, three rollouts per task, 1,800 s each, 90 episodes per agent. Reported:
  best result **GPT-6 Astra at 45.0/100**, and a sharp negative finding — judgment
  accuracy of ~90–100% coexists with 0–30% perception-task success for four agents,
  with only Astra non-zero on hard short-horizon (40%) and hard long-horizon (22%).
  Ablations show calibrated depth adding +20 points for three agents and video
  demonstrations raising Astra's stable stage completion 36.0%→66.2%.
  **Simulation-only** (MuJoCo/robosuite, Franka Panda). **Artifact placeholder:**
  `github.com/dzj441/Libero-Agent` contains only a LICENSE and a README reading
  "Code Comming Soon~" (MIT, 2 commits, created 2026-09-30); no task definitions,
  scene assets, evaluator or leaderboard are released. Results author-reported.

- **SafeVLA-Bench** (`2606.00773`, cs.RO; **v2 2026-09-30**, v1 2026-05-30) — a
  **post-hoc verification layer** over existing simulator benchmarks rather than a
  new task suite: it preserves the host benchmark's observations, actions, seeds,
  success predicates and inference wrappers and owns only measurement, encoding
  task-aware safety requirements as **Signal Temporal Logic invariants with
  quantitative robustness semantics** and reporting native success alongside safety
  rate, the success-but-unsafe rate and a bounded worst-violation-depth Violation
  Severity Index. Binary success is an insufficient contract because a policy can
  reach the goal while applying excessive contact, disturbing bystander objects,
  destabilizing the held object, or entering self-contact. The v2 revision grows the
  roster from 9 to **24 distinct policies / 27 policy–benchmark entries** and
  revises the headline rates upward: **18–28% unsafe-episode rates for LIBERO
  policies above 90% SR** (v1 recorded 13–15%) and **38–56% of successful
  RoboCasa-365 rollouts violating at least one active clause**. Reported across
  18,000 episodes: **3,324 unsafe successes are invisible to success-only
  evaluation**, within-suite rank correlation between success and safety is only
  τ=+0.34, and the ordering inverts at the top (Xiaomi-Robotics-1 has the best
  SR 81.9% but ranks sixth on safety). v2 adds a post-training case study
  (Diffusion-DPO on π0.5, safety 85.0→94.5 with unsafe-success 14.5→5.5) and a small
  **SO-101 hardware pilot** in which 4 of 26 successful rollouts violated a scored
  clause. **Artifact: leaderboard only** — the release repository named in the
  leaderboard's own submission instructions (`JFan5/SafeVLA-Bench-release`) returns
  **HTTP 404** and code/data are "will be released" (verified 2026-10-01); the live
  leaderboard reports 22,500 scored episodes across 24 policies. **Name collision:**
  the unrelated "SafeVLA" safety-alignment framework already linked elsewhere in
  this list is a different work, as is `Jatshi/SafeVLA-Bench`. Results
  author-reported.

### Runtime, Safety, and Observability — Safety

- **Approval Laundering** (`2609.38983`, cs.CR/cs.AI/cs.SE; v1 2026-09-30;
  single-author working draft, not yet submitted for review) — systematizes the
  failure of the assumption every approval-gated harness rests on: that the action
  *A* a human approves is the action *A′* the harness executes. It defines six
  substitution classes (Scope, Argument, Temporal, Tool, Delegation, Semantic),
  instruments Claude Code's native **`PreToolUse`** mediation point headlessly, and
  reports a Bound-Gap Rate with Wilson intervals across 138 traces. Reported: Scope
  1.000 [0.839, 1.000] and Temporal 1.000 [0.839, 1.000] (N=20 each), same-name
  `$PATH` Tool substitution 1.000, Delegation 0.947, Argument 0.450 (9/20), κ=1.0
  inter-rater. The proposed keyed **Approval Token** over seven fields removes
  Temporal (1.000→0.000, p=1.9×10⁻⁶) and Delegation (0.947→0.000, p=7.6×10⁻⁶) — and
  the authors state plainly that it **by design leaves Scope laundering unaffected
  and shows no significant Argument reduction (p=1)**, because those classes leave
  every recorded dispatch field unchanged. **No artifact** (the full text contains
  no repository, data or availability statement); N≈20 per class on **one** harness,
  and the author discloses that runs fork the parent environment and are **not
  independent draws**, so point estimates are nominal under the stated sampling
  assumptions. Digital agents, no robot experiment.

- **HARDE** (`2609.38291`, cs.CR/cs.CL; v1 2026-09-29; USTC/NUS/SMU) — optimizes
  the *harness* for runtime risk control rather than the model: the harness is
  structured as **trigger → monitor → feedback**, where the trigger decides when to
  invoke an LLM monitor, the monitor judges the next proposed tool call against the
  visible request and trajectory prefix, and the feedback module applies the
  intervention while limiting damage to benign utility. HARDE is the two-stage
  optimization over that structure — isolated per-module probing to derive an
  optimization guide, then iterative harness revision on safety and utility
  feedback. Reported across SHADE-Arena, AgentDyn and Agent-SafetyBench with a
  DeepSeek-V4-Flash monitor: Safe Success 0.500 / 0.935 / 0.764 against the
  strongest baselines' 0.269 / 0.816 / 0.694, i.e. **+77.9% relative Safe Success
  on SHADE-Arena**, with module ablations collapsing it to 0.083–0.357.
  **Verified open:** `github.com/Liuz233/HARDE` (created 2026-09-01, 3 commits) with
  per-domain baselines, the component probe and the adaptive search — **but the
  repository carries no LICENSE file** (all three raw license paths 404) even
  though the paper is CC BY 4.0. The authors' own limitations record that the
  optimization guide **does not transfer** to a weaker monitor. Digital agents, no
  robot experiment; results author-reported.

- **Trust Is Not a Score / Runtime Assurance Contracts** (`2609.39717`,
  cs.AI/cs.CY/cs.SE; v1 2026-09-30; single-author, three affiliations) — names the
  **assurance-transition gap**: benchmarks, audits and agent protocols describe
  performance, permissions and repair, but not how observed evidence should change
  an agent's *authority* mid-task. A Runtime Assurance Contract binds autonomy
  boundaries, component eligibility, evidence state, a transition policy,
  human-review capacity and a **non-compensatory gate set**, so soft metrics may
  route but a failed or unknown mandatory gate forces retry, switch, escalation,
  deferral or stop. Reported on a 280-case deterministic failure-injection corpus:
  at the published example weights a score-only rule **admits 80 of 100
  block-required injections and all 40 review-required injections**, while the gate
  conjunction admits none, with 0/140 false refusals in all four arms; an analytic
  proposition shows exact agreement holds only when the threshold does not exceed
  the smallest weight, and the published example configuration is **not** among the
  30 of 114 configurations that satisfy it. **Verified open:**
  `github.com/SZabolotnii/TRACE-RAC-code-supplement` (Apache-2.0, created
  2026-09-23) plus arXiv ancillary files including `rac.py`, freeze manifests,
  judge labels and a decision log — but the paper's 280-case corpus and the private
  TRACE-AI gate are **not** released, and the author states the released 24-episode
  holdout gives the baseline **parity, not superiority**. Synthetic-only, no
  deployment claim, digital agents. Results author-reported.

## Revision edits to existing entries

Three already-curated papers carried a substantive revision into this block, and
all three changed a fact the repository records. Each README entry was corrected.

1. **BATON** (`2608.16889`, README line 495; v3 2026-09-30, v2 2026-09-28,
   v1 2026-08-17) — v3 **revises the reported gains upward**: the abstract now
   states task success **+37.7%** and cumulative success **+29.7%** over the
   current SoTA on RoboMemArena, against the **+11.6% / +14.9%** the entry
   recorded. v3 also **removes the v2 release statement** ("The source code will be
   publicly released upon acceptance"): the word "release" no longer appears
   anywhere in the full text, and the only GitHub link in the paper is the
   third-party `Physical-Intelligence/openpi` checkpoint. The entry's numbers and
   its artifact note were both updated; BATON remains **not open source**.
2. **GlanceWAM** (`2608.23927`, README lines 126 and 678; **v2 2026-09-29**,
   v1 2026-08-25) — v2's comment field reads "**Add real-robot experiments**", and
   the abstract now reports "single-arm and bimanual real-robot manipulation"
   results above π0.5 without robot-data pretraining. Both README entries stated
   that "the evidence is simulation-based"; that was true of v1 and is no longer
   true of v2. Both were corrected to record the added real-robot evidence while
   keeping the MIT-licensed code and author-reported status. This revision was
   **not** visible to the previous run: the record was re-announced in this block
   only, and its v3-absent comment field is empty, so a comment-merge parser
   retains the older text.
3. **Safety Under Scaffolding** (`2603.10044`, README lines 242 and 418;
   **v4 2026-09-30**, v3 2026-09-24, v2 2026-06-03, v1 2026-03-08) — v4 "completes
   the revision begun in v3", applying registered exclusion rules and an H3-bias
   analysis. It **changes the headline statistics**: scored evaluations 62,808 →
   **60,112**; benchmark choice explains **15.1%** of outcome variation (recorded:
   19.3%) against **0.5%** for scaffold architecture (recorded: 0.4%), i.e. "about
   33× less" rather than "about 45× less"; composite reliability **G = 0.251,
   95% CI [0.000, 0.879]** (recorded: G = 0.000, CI [0.000, 0.752]); and the
   refusal-classifier sensitivity finding is now "four of five cases" rather than
   five. Both README entries were corrected; the finding that a single composite
   safety score cannot support a deployment decision is unchanged, but its stated
   evidential strength is different.

One further re-announcement changed a record without changing any verdict, and is
noted here so a later run does not re-open it: **`2609.05797`** ("Safety Monitors
Mostly Catch What the Model Already Refuses") posted **v3** with a substantially
rewritten abstract and a new title (v1 appeared as "Recall Is Not Protection"). It
is referenced only inside the 2026-09-09 record's rejection list, not at README
level, and its exclusion reason (digital-only safety-monitor evaluation, not a
harness contract) is unaffected. `2609.28984` (CrossSafe) posted v2 with an
acknowledgements-only change. The remaining 33 already-referenced IDs carry no
field change that alters a recorded status.

**Two catalogue hazards were confirmed and are recorded so later runs do not merge
them:**

- **NavHarness** — `2609.34276` "Towards Lifelong Embodied Navigation" (already
  curated at README line 255/563, MIT-licensed `billzhao1030/NavHarness`) and
  `2609.39915` "Adaptive Goals for Agentic Vision-Language Navigation" (added this
  run) are **different papers by different authors**.
- **RoboHarness** — `2607.18060` (added this run, no artifact) and the unrelated,
  MIT-licensed `LZY-1021/RoboHarness` at arXiv `2603.24060` share a short name.

## Current Landscape additions

Five bullets added to `README.md`:

1. Robot harnesses are being packaged as **fast/slow execution contracts rather
   than planners** — DynaHarness (fast brain holds authority at 2 Hz, refusal and
   substitution are recorded values), RoboHarness (heterogeneous policies behind
   one interface with a Memory Bridge), NavHarness (verification defines the memory
   boundary).
2. **Harness composition is being separated from harness generation** — STITCH
   compiles mined primitives at test time, MILO co-evolves the harness and the
   search strategy that finds it, Turbo Harness patches a global harness per
   instance, ScholarEvolve mutates modules from the literature.
3. **The value of harness machinery is now being measured, including against a
   null** — How Much of a Harness finds elaborate MLE harnesses add nothing over a
   minimal coding-agent session at equal budget; Mid-Harness finds the gain is at
   the model–harness action-admission boundary and requires a capable verifier;
   Runtime Contracts shows a harness-owned persistence contract plus a harness-owned
   tool-call cap can turn a mismatch into near-total failure.
4. **Robot auto-research loops are closing on hardware** — URAI evolves persistent
   robot tools, SimEX repairs a simulator from each physical trial, EmbodiRSI
   targets data at the current policy's weaknesses, and Scale and Selection shows
   selection rules and rollout scale decide whether harness evolution helps at all.
5. **Approval and runtime authority are being audited as harness contracts** —
   Approval Laundering measures approval→execution substitution at a real
   `PreToolUse` hook, HARDE optimizes trigger/monitor/feedback for runtime risk,
   and Runtime Assurance Contracts replace score-based authorization with
   non-compensatory gates.

## Rejected / watch list

### Screened this pass and rejected

- **ASENA: Self-evolving Agents for Embodied Navigation** (`2609.39207`, cs.RO;
  v1 2026-09-30) — project page `asena-bot.github.io` is live but the author's
  GitHub account has **0 repositories**, so nothing is released; evidence is
  simulation plus three **qualitative** Unitree G1 missions. **Watch** for a code
  release.
- **Multi-Link Safety Filtering for VLA Policies Around Moving Hazards**
  (`2609.40007`, cs.CV/cs.RO; v1 2026-09-30) — a genuinely interesting pairing of a
  safety filter with a frozen VLA and real SO-101 evidence (16 episodes per arm),
  but "Code (coming soon)" is **unlinked text** and the GitHub repository is only
  the project-page source. **Watch.**
- **Blackout vs. Freeze: Physical Failure Modes of VLAs under Camera Faults**
  (`2609.39145`, cs.RO/cs.CV; v1 2026-09-30) — sim plus a real WidowX (100 trials)
  and an MIT-licensed artifact, but the artifact is **anonymized and returns
  HTTP 401**, and checkpoints are withheld "upon acceptance". **Watch** for
  de-anonymization.
- **ActionGuard: Tool Call Authorization under Poisoned Skills** (`2609.39450`,
  cs.CR/cs.AI; v1 2026-09-30) — harness-native `before_tool_call` authorization
  with good scale (319 injection–task pairs × 3 repeats × 5 reviewer models), but
  its only artifact link (`anonymous.4open.science/r/ActionGuard-D85C/`) returns
  **HTTP 401 `not_connected`**, it has **no limitations section**, and the abstract
  and body disagree on the relative ASR reduction (35.54–46.11 / 70.44 vs
  35.5–46.1 / 70.2). **Watch** for a public repository.
- **Who Verifies the Graph? Misspecification Attacks on Causal Action Verification**
  (`2609.40027`, cs.AI/cs.LG; v1 2026-09-30; NeurIPS 2026 workshop poster) —
  conceptually sharp (the committed action–state graph is a trusted computing base
  the harness never checks; a single omitted edge takes a verifier from 0% to 15.3%
  false executions while every action still carries a valid certificate), and its
  limitations are unusually explicit. Excluded on the same grounds as BSC-R on
  2026-09-30: **no released artifact**, **single author**, synthetic-only by
  construction, and the red-teamed verifier is the author's own prior work.
  **Watch** for a camera-ready with code.
- **LIBERO-Agent**, **ScholarEvolve**, **MILO** and **Mid-Harness** are included
  above with their placeholder or empty artifacts recorded literally; the README
  notes for those four must be re-checked on later runs.
- **HIDE / SEEK: Benchmarking and Enhancing Skill-Level Memory for Partially
  Observable Robotic Manipulation** (`2609.38886`, cs.RO; v1 2026-09-30) — strong
  and on-theme (memory as harness state under partial observability: 15 tasks whose
  decision points make identical observations require different actions, with
  simulation plus a Franka Panda / Robotiq / D455 real evaluation at 89% vs 47% and
  13%), but **every artifact class is an explicit placeholder**: the project page
  reads "Paper SOON", "Code SOON" and "Data — Coming soon", the header Code button
  is `href="#"`, `huggingface.co/datasets/nanamma/HIDE` returns **HTTP 401**, and
  no repository exists. Parts of its own real-world gallery still read "Empty slots
  await footage". **Watch** for release.
- **Looking Back to Move Forward / TeV** (`2609.39038`, cs.RO; v1 2026-09-30) — a
  deployment-time verification harness that adds a learned temporal token and a
  contrastive energy verifier (<0.15% extra parameters) to a frozen flow-matching
  VLA, deciding which candidate chunk to commit; reported at LIBERO-Plus 73.2→79.8%
  and RoboTwin 2.0 72.3→81.3%, with real-robot evidence over three tasks × 30
  trials. Excluded this pass because **no artifact exists** — the project page has
  no Code link at all (only author homepages), `github.com/hatchetProject/tev`
  returns **HTTP 404**, and no release is promised — and because the page's headline
  "6%–18%" gain range does not reconcile with the paper's baseline-relative numbers
  (+3.8 and +6.4), so any entry would have to cite the tables. The real-robot
  platform is **never named** in the paper. **Watch.**
- **AssemblyWorld** (`2609.40353`, cs.CV/cs.RO), **Tri-Info** (`2606.19998`,
  cs.RO), **AutoDataBench** (`2609.40097`, cs.CL), **EgoTools** (`2609.39378`,
  cs.CV), **RoboAssist** (`2609.39384`, cs.RO), **RoboCoach** (`2609.39685`,
  cs.AI/cs.RO), **Inline Memory / Reusable Skills** (`2609.39794`, cs.CV/cs.RO) —
  screened this pass; all are plausible future entries but none was verified to the
  repository's artifact-and-evidence bar within this run's budget. They are
  **deferred, not rejected**; a later run should verify them before re-screening
  the block from scratch.
- **Not carried forward as candidates:** the remaining uncurated records in the
  block were rejected by the standing scope rules — generic manipulation,
  locomotion, navigation, grasping, world-model, driving, medical, video, and
  perception-only papers without a reusable agent/VLA harness, recovery, runtime
  contract, safety, or evaluation contribution; pure model/scaling papers; and
  papers whose only "artifact" is a project page of figures or videos.

### Carried watch list — unchanged this window

- `2609.24274`, `2609.24350`, `2609.24563`, `2609.22587`, `2609.21223`,
  `2609.21502`, `2609.22218`, `2609.22247`, `2609.26121`, `2609.26520`,
  `2609.25558`, `2609.29091`, `2609.29065` — the thirteen IDs carried by the
  2026-09-30 run. None appears in this block; they are carried forward unchanged.
- Watch entries opened by the 2026-09-29/2026-09-30 runs and re-checked here:
  **RoboHarn-Evo** (`2609.37583`) has no artifact URL of any kind and is unchanged;
  **SelfSearch** (`2609.37968`) is unchanged; **F4R** (`2609.35575`) does not
  reappear this window. The artifact placeholders recorded on 2026-09-25
  (Maithili/GAP's 1 KB repository, HEXIS's HTTP 401 anonymous link, the RoboRecover
  dataset "being prepared for Hugging Face", HarnessPAI's "not yet publicly
  released" code) are unchanged.
- StageWAM (still v3), ReflexVLA/ReflexBench (post-acceptance), DreamX-Phi
  (README-only), UniTexture (no code), PRISM ("Dataset soon"), GigaBrain-0.7/WBC
  (release unconfirmed), ForceU-VLA (README-only), LIBERO-VIFO, Agent Lightning,
  VLCP, Hydra-0 — no change this window.

## Validation performed

- Markdown structure checked: heading hierarchy, list rendering, and table-free
  bullet style match the preceding daily records; every included entry carries an
  arXiv link and a category label.
- `git diff --check` run before staging (no whitespace errors).
- Every added link was fetched during verification on 2026-10-01 and its HTTP
  status recorded in the dossiers; the four "artifact" claims that matter most
  were re-checked by hand — `github.com/Denghaoyuan123/DynaHarness` (404; the
  author account itself 200), `navharness.github.io/data/publication.js`
  (`"codeUrl": ""`), `github.com/dzj441/Libero-Agent` (LICENSE + "Code Comming
  Soon~" only) and `github.com/UCSB-NLP-Chang/ScholarEvolve` (0 KB, empty).
- Dates checked for cross-file consistency: the README badge, the "Last verified"
  line, and this record all carry **2026-10-01**.
- Categories checked against the arXiv record for every included entry, including
  the two collisions (NavHarness `2609.39915` is cs.CV primary / cs.RO; RoboHarness
  `2607.18060` is cs.RO only).
- Cross-file consistency: each of the five Current Landscape bullets added by this
  run refers only to entries now present in the main list, and every arXiv link
  added by this run was fetched and returned HTTP 200 (47 of 47; the one
  `img.shields.io` badge URL excluded). **One pre-existing inconsistency was found
  and is recorded rather than silently fixed:** a Current Landscape bullet that
  predates this run cites the *Counterfactual Memory Audit* (`2609.27247`) while
  its two companions in the same bullet — MemBodied (`2609.28256`) and Uranus
  (`2609.24815`) — do have main-list entries. `git show HEAD:README.md` confirms
  the orphan was already present before this run. It was left untouched because
  neither adding the entry (unverified) nor deleting the reference (curated
  content) is in scope for a maintenance run; a later run should verify
  `2609.27247` and either add it or trim the citation.
- The provenance caveats stated in the README (DynaHarness's 404 code link, RoboHarness's absent artifacts, NavHarness's
  empty `codeUrl`, URAI's and EmbodiRSI's and SimEX's and Scale-and-Selection's
  missing artifacts, LIBERO-Agent's and MILO's and Mid-Harness's placeholders,
  ScholarEvolve's empty repository, Turbo Harness's unreleased editor weights,
  STITCH's and How-Much-of-a-Harness's absent artifacts, HARDE's missing license,
  ChunkTrust's undeclared Hugging Face licence and 118 GB of partially-uploading
  checkpoints, SafeVLA-Bench's 404 release repository and leaderboard-only assets,
  Approval Laundering's and RAC's caveats, BATON's revised numbers and removed
  release promise, GlanceWAM's added real-robot evidence, and Safety Under
  Scaffolding's revised statistics) match this record.
- Duplicate check: every included ID was grepped against `README.md`, `docs/*.md`
  and `sources/*.md` before insertion; none was present.

## Operational notes and blockers

- **`/tmp` is per-command in this sandbox**, so all harvest XML, parsed JSON,
  dossiers, full texts, project pages and logs were kept under
  `.scratch/arxiv-2026-10-01/` (untracked, never staged). The task prompt's
  "save XML to `/tmp`" instruction was satisfied in substance by keeping the XML
  on disk for inspection; the literal `/tmp` path does not survive between
  commands here. This matches the operational note carried by recent runs.
- **Export-API rate limiting:** the second and later range-query batches returned
  **HTTP 429** for cs.CL, cs.CV, cs.LG and all of the 2026-09-29 windows. The two
  queries that succeeded (cs.RO and cs.AI for 2026-09-30) were used for the
  cross-check in Scope. No verdict depends on a rate-limited call; the OAI-PMH
  harvest is the authoritative source and returned complete pages.
- **Seven verification agents ran in parallel** and were each capped at 5
  `api.github.com` calls; the group reports record 0–2 calls each, so the shared
  unauthenticated budget was not exhausted. Everything else was read from GitHub
  HTML tree pages, `raw.githubusercontent.com`, project pages, the arXiv HTML, and
  the Hugging Face Hub. All HTTP statuses are recorded in
  `.scratch/arxiv-2026-10-01/dossiers/out-*.md`.
- **A parser artifact worth remembering:** the OAI comment-merge keeps the last
  non-empty comment across datestamps, so when a replacement version *empties* its
  comment field (BATON v3) the merge hides the change. Version histories must be
  read from the arXiv abs page, not inferred from the merged comment field. This is
  how the BATON revision was caught.
- No conflict occurred; `git fetch origin`/`git pull --ff-only` were run before
  committing and the working branch is `main`.

## Commit

Task-owned files committed on `main`: `README.md`,
`sources/daily-arxiv-2026-10-01.md`. Unrelated user changes
(`docs/reference-architecture.md`, `docs/ring-harness.png`, `handoff.md`,
`.scratch/`) were left untouched and unstaged. No branches, no pull requests, no
force-push.
