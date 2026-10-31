# Daily arXiv scan — 2026-10-31 (no new announcement batch; full re-screen of the unchanged 2026-10-28 and 2026-10-29 batches: 815 records, ten previously unexamined inclusions)

## Scope

- **Interval owed.** The previous successful run started **2026-10-30 10:52 UTC**, committed at **2026-10-30 11:06 UTC** (12:06 Europe/Berlin) on `main` at `b8f5d80` with record
  `sources/daily-arxiv-2026-10-30.md`, screened **949** records (the 134-record late tail of 2026-10-27, all 317 of
  2026-10-28, all 498 of 2026-10-29) and closed 2026-10-29 at frontier ID `2610.40646` (`2026-10-29T17:59:20Z`).
  This run started **2026-10-31 09:30 UTC** on `main` at `b8f5d80`, so the interval it owes is **submission dates
  2026-10-30 → 2026-10-31**.
- **The interval turned out to be empty, and the frontier proves it.** An unrestricted descending `cs.RO` query
  returns **`2610.40646`** (`2026-10-29T17:59:20Z`) as the newest record — **byte-identical to the frontier the
  previous run recorded**. The frontier did not move at all in the ~23 h between the two runs.

  | query (one OR-query over cs.RO/cs.AI/cs.CL/cs.CV/cs.LG per day) | 2026-10-29 | 2026-10-30 | 2026-10-31 |
  | --- | ---: | ---: | ---: |
  | `submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` | **498** | **0** | **0** |
  | recorded by the previous run | 498 (unchanged) | — | — |

  2026-10-30's **0 is not a zero-submission day**; it had simply not been announced when this run probed at
  09:35 UTC on a **Saturday**. arXiv announces on a weekday schedule, so Friday submissions reach the export API
  with the next working announcement, and 2026-10-29 remains the newest closed day. **An unmoved frontier means
  nothing was announced, not that nothing was submitted.** The remaining 10-30 and 10-31 records stay owed to the
  next run, which must query both days again rather than assume they were screened here.
- **The batch re-screened.** Because the newest visible batch was **unchanged from the previous run**, the
  standing instruction is to re-screen that whole batch once before concluding that nothing qualifies. This run
  re-screened **815 records**: all **317** of 2026-10-28 (`2610.38086`–`2610.39065`, `2026-10-28T00:00:17Z` →
  `23:51:50Z`) and all **498** of 2026-10-29 (`2610.39070`–`2610.40655`, `2026-10-29T00:00:46Z` → `17:59:53Z`).
  2026-10-28 was included because that day, too, had been screened exactly once.
- **The re-screen changed the outcome completely.** Ten on-scope papers were **neither included nor deferred nor
  excluded** by the previous run's record: they are absent from its 95 named IDs and from the full repository.
  That is not a judgment disagreement about borderline cases — several are among the strongest harness-contract
  papers of the month. **A single screening pass over ~500 mixed-domain records is not sufficient**, and this run
  produces its entire output from the second pass.
- **Coverage.** All 815 carry at least one of the five target categories. Primary category counts — 2026-10-28:
  `cs.LG` **72**, `cs.AI` **57**, `cs.CV` **55**, `cs.CL` **41**, `cs.RO` **34**, `cs.CR` 9, `stat.ML` 6, `cs.HC` 5;
  2026-10-29: `cs.CV` **126**, `cs.LG` **103**, `cs.AI` **77**, `cs.RO` **62**, `cs.CL` **43**, `stat.ML` 12,
  `cs.CR` 10, `eess.SY` 5. `cs.RO` appears as primary or cross-list on **112** records (**37** + **75**).
- **Revisions.** No in-batch `<updated>` > `<published>` exists in either day (both days are single-vintage v1
  blocks), so revisions were again hunted with the **`id_list` version sweep over all 560 arXiv IDs listed in
  `README.md`**. **13** records carry `<updated>` inside the run window, and **every one of them is a v1 whose
  `<updated>` equals its own same-day publication timestamp** — no v2 or later appeared, so no entry needed a
  revision update. The three revisions the previous run handled (`2609.19906v2`, `2609.38905v4`, `2610.37255v2`)
  are unchanged.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`; artefacts saved under
  `.scratch/arxiv-2026-10-31/` (untracked, never committed). One frontier probe, three day counts, nine `abs`
  pages, ten full-text HTML pages, seven GitHub REST calls plus one top-level tree listing, six `id_list` batches
  (~40 requests at 3–5 s spacing, **zero HTTP 429**).
- **No re-download needed for the batch.** The previous run's harvested XML for both days was still present and
  its record counts still matched the live day counts exactly (317 / 498), so the re-screen used the same bytes
  the first pass read. This is what makes a full second pass cheap: re-screening costs parsing, not requests.
- **A parser bug had to be fixed first, and it matters for future runs.** The first parse produced **empty
  abstracts for all 815 records** because the extractor asked for an `<abstract>` element; the arXiv Atom feed
  carries the abstract in **`<summary>`**. With blank abstracts the keyword screen flagged **712 of 815** records
  and 646 appeared "not previously named" — a filter with no discriminative power. After the tag fix the same
  screen selected **121** records (112 with `cs.RO`, plus robot-anchored harness terms), which is a screenable
  shortlist. Any agent reusing this code should assert non-empty abstracts before trusting a shortlist.
- **Screening.** (a) Every one of the 815 titles was read in ascending ID order in two passes — first all
  `cs.RO`/`robotics` records, then non-robot records carrying a harness, self-evolution, skill, memory, monitoring,
  safety or verification term. (b) **24** abstracts were read in full. (c) Full-text HTML of **10** records was
  read for release statements, harness internals and result tables. (d) Artifact verification against live
  sources: the GitHub REST API for **7** repositories (licence, size, creation/push timestamps, stars) plus one
  top-level tree listing, **2** project pages fetched for content, **1** Hugging Face dataset endpoint, and **1**
  anonymized-code endpoint probed for status.
- **Deduplication.** All 10 included IDs and titles were grepped across `README.md`, `docs/` and `sources/`;
  all are absent. **13** candidate IDs were checked this way before any abstract was read, and all 13 came back
  absent, so no candidate was a duplicate of an existing entry.

## Included (10)

### General Harness Design and Self-Improvement (3)

- **PCM: A Proof-Carrying Message Protocol for Verifiable Multi-Agent Communication** (`2610.39109`, cs.CE) —
  why it qualifies: it designs the **trust boundary between agents**, the layer authentication protocols leave
  open. A2A and MCP authenticate the channel while the *sender* is the party in doubt, so a prompt-injected agent
  can launder fabricated facts and borrowed authority through a multi-agent system. Every claim now carries
  evidence the sender cannot write — records from an append-only Merkle log with inclusion proofs, signed by an
  attesting signer the agent cannot control — which the receiver checks offline into a **three-valued verdict**
  (proven / unproven / failed), where a claim resting only on the agent's own report is *never* proven. Authority
  travels as grant chains rooted in recorded human approval and can only narrow; the specification covers four
  claim types, a typed caveat algebra, replay rules that survive restarts, and strict readings under which
  independent verifiers agree, and proves the security properties. Reported: on **926 messages from real runs,
  712 of them lies in 16 classes**, PCM accepts **10**, all stale evidence (**1.4%**, 95% CI 0.8–2.6%), against
  plain A2A accepting all 712 and models of six deployed protocols (signed Agent Cards, uAgents, RFC 9421, SCITT,
  AP2, Verifiable Intent) accepting 504–712; two independent verifiers agree on all 926 verdicts at ~1 ms.
  **Verified open (2026-10-31): MIT `Cygnux-Labs/PCM-protocol`** (4.3 MB, created 2026-10-28, pushed 2026-10-29).
  Digital agents; no robot experiment; author-reported.
- **Kairos: Semantics-Aware Orchestration of Planning Engines across Classical, Numeric, Temporal and Hybrid PDDL**
  (`2610.39454`, cs.AI + cs.RO) — why it qualifies: it is a **runtime harness around planning engines**, which is
  what online robot planning needs. It profiles each task to choose among five engine classes (ground heuristic
  search, compressed and snap-action temporal search, time steps for hybrid dynamics, lifted search), schedules
  them under **one shared time-and-memory budget**, and states what each answer guarantees. The harness, not the
  planners, carries the discipline: every candidate plan is checked by a **validator independent of the planner's
  transformations**, and *unsolvable* is reported only when a search **whose exhaustion is a proof** exhausts its
  space. Reported: on **1,756 tasks from nine comparative suites** it solves more tasks at full limits than the
  strongest evaluated planner of each suite (largest margins on numeric, temporal, PDDL3 and PDDL+), also solves
  the most at 100 ms and 1 s, and resamples above zero gain in six of nine suites. **Validity is PDDL-model
  validity; the authors state it does not imply executability by a physical system** — there is no robot
  experiment, only planner benchmarks. Real infrastructure: ROS 2, conda, Python and planutils packages, and the
  default planner of PlanSys2. **Verified open (2026-10-31): Apache-2.0 `PlanSys2/kairos`** (2.2 MB, created
  2026-10-10, pushed 2026-10-30) and **Apache-2.0 `PlanSys2/kairos-benchmarks`** (4.7 MB).
- **Before the Next Rollout: Diagnosing Skill Utility Shifts from Policy Updates** (`2610.39358`, cs.AI) — why it
  qualifies: it names the **staleness bug in skill evolution**. Skills are revised from execution trajectories, but
  a policy update can change an unchanged skill's behavioral effect and marginal utility, so earlier-trajectory
  attribution no longer reflects what the skill contributes now, and failures arise from several factors at once
  so single-skill contributions are hard to isolate. The diagnostic is cheap by construction: at fixed contexts
  from **pre-update** rollouts it measures how policy updates change the **contrast in token preferences between
  skill and control conditions**, and combines those interaction shifts with advantage estimates into
  reward-directed readouts — reusing rollouts that already exist and needing only **forward passes**, with no
  post-update rollout evaluation. Reported: policy updates do alter skill marginal utility, the readouts identify
  skills with declining utility, and readout-guided prioritization in skill evolution improves downstream
  performance while **reducing policy and editor token consumption**. Digital agents; author-reported.
  **Artifact status literal (2026-10-31): the only link is an anonymized `anonymous.4open.science/r/SkillSense/`
  that returns 401** — not accessible; no repository, weights or data located.

### Agentic Robot and VLA Harnesses (1)

- **Gym-Anything for Robotics: Scaling Physical Simulations with Digital Agents** (`2610.40565`, cs.RO) — why it
  qualifies: it reframes the simulation bottleneck as a **harness problem, not a physics-engine problem**. What
  limits robot simulation is expert authoring effort, so if simulation creation is a coding-agent task then better
  coding agents buy richer and more faithful simulations directly. Task selection is driven by **U.S. GDP data** to
  cover economically valuable physical work, "what makes a good simulation and a good task" is written down as an
  explicit definition set, and an **adversarial creation-audit loop** checks every generated simulation and task
  against those definitions before admission — the audit is what stops the loop grading its own output. Result:
  **RoboWorld, 25 simulations with 25 test and 175 training RL environments**, spanning manufacturing,
  construction, food service, households, laboratories, healthcare, logistics, sports and education, with liquids,
  chemistry, cloth, molten metal, granular soil, pastes, soft tissue and acoustics, and completion-checking
  verifiers (a barcode counts as applied only if it decodes from a rendered image). Reported: **the best frontier
  models fail every test task, averaging reward 0.06**, and in-context learning across attempts, a privileged
  teacher, self-assisted learning from privileged experience, and code policies generating VLA demonstrations each
  improve performance substantially. **Verified open (2026-10-31): MIT `gym-anything/robotics`** (185 MB, created
  and pushed 2026-10-30; `agents/`, `benchmarks/`, `src/`, `tests/`, `docker/`), project page live with leaderboard
  and task browser. Simulation-only; author-reported.

### Benchmarks and Evaluation (2)

- **BURGER: Benchmarking and Understanding Robotic Generalization via Embodied Drawing** (`2610.40532`, cs.RO) —
  why it qualifies: it picks a task family that makes language-conditioned generalisation *legible* rather than
  statistically convenient. Every episode starts from the **same blank canvas**, so the instruction is the only
  difference; an instruction admits an **open-ended set of valid drawings and stroke orders**, so multimodal
  behaviour is observable instead of penalised; trajectories are continuous, so scale is cheap. Reported:
  **28,200 simulated demonstrations over 282 drawing tasks**, five representative VLA models evaluated across data
  scale, language grounding and multimodal behaviour, with failures separating language grounding from spatial
  composition from action generation; some VLAs benefit from greater task diversity and scale and generalise to
  unseen compositions, and a policy deployed on a **Franka Panda** draws reactively under human intervention.
  **Verified open (2026-10-31): MIT `Qxjyqzw/BURGER`** (23.8 MB, created and pushed 2026-10-30), the **BURGER
  Hugging Face dataset** live, and a live project page. Author-reported.
- **CuraEval: Evaluating Demonstration Curation on Mixed-Skill Robot Data** (`2610.38221`, cs.RO) — why it
  qualifies: an **evaluation contract for data curation**, which is where robot-learning selection studies stop
  being science, since differing protocols make it impossible to tell whether a scored selector beats a trivial
  baseline. CuraEval pairs mathematical analysis with **controlled comparison against fixed-size matched random
  subsets and against full-data training** on mixed-skill robomimic data with Diffusion Policy and BC-RNN.
  Reported: the evaluated **PSD score depends only on episode length and squared position magnitudes** and nearly
  reproduces episode-length ranking on *square* (**ρ = 0.999**); a sophisticated selector can lose to chance — an
  adapted geometric-entropy leave-one-out selector cuts success at 25% retention from **94.7% to 74.0%** on *can*
  and reaches **41.7%** on *square*, below the mean of all three random subsets (59.8–62.5%); length-based
  selection at 50% does beat random on *square* (**70.7% → 76.5%**) though Holm-corrected intervals permit only
  modest gains. CoRL 2026 Workshop "Oops, I Erred"; simulation-only. **Verified open (2026-10-31): MIT
  `jiuyixu25/CuraEval`** (3.4 MB, created and pushed 2026-10-28).

### Runtime and Safety (4)

- **VulnBench 2.0: How Deterministic SAST and AI Security Agents Divide the Work** (`2610.38617`, cs.CR) — why it
  qualifies: the clearest direct evidence on this list that **a security agent's misses belong to its harness as
  much as to its model**. **108 hand-curated, attacker-reachable vulnerabilities in 20 open-source web
  applications** (Go, JavaScript/TypeScript, Python, Java; **62 multi-file**), credited only if a finding locates
  both the **attacker-controlled source and the dangerous sink, each within two lines** — an evidence-routing
  rule, not a plausibility judgment. Nine productized configurations under one blind protocol. Reported: Snyk Code
  reaches **82.4% macro recall (89/108)** and Claude Code `/security-review` with Opus 5 at xhigh effort **79.3%
  (85/108)**; they disagree on 28 vulnerabilities (16–12, too few to separate them) and together reach **101 of
  108**, the ceiling of all seven comparable configurations, while the AI configurations report at least seven real
  vulnerabilities absent from the reference set. The attribution is the reusable part: **without Claude Code's
  built-in review policy the same model matched 16 instead of 9 of the 20 vulnerabilities** in the classes that
  policy excludes or restricts; Codex Security located 80 vulnerabilities at file level but **34** at line level;
  DeepSec's model-free file selection kept a file from only **56 of 108** flows; coverage and cost were only
  loosely related. **Verified open (2026-10-31): Apache-2.0 `snyk-labs/snyk-vulnbench`** (75 MB, created
  2026-05-06, pushed 2026-10-28) with the harness, scorer (`src/scorer.ts`), the 20 fixtures and reference sets.
  Digital targets; no robot experiment; author-reported.
- **PASA-KV: Attacking Action-Facing Visual Routes in World Action Models** (`2610.39816`, cs.RO) — why it
  qualifies: it makes the **world-to-action pathway** of a world action model a stated attack surface and shows
  the intuitive metric is the wrong one: **representation perturbability and closed-loop vulnerability disagree**,
  because attack effectiveness varies sharply across visual K/V routes while larger representation displacement
  does not reliably imply stronger control failure. It attributes the mismatch to the **action-conditioned
  readout** — keys decide where action queries read, values what content they receive — and derives an
  *action-facing* view in which consequence depends on how a perturbation is **routed into action generation**
  rather than on its representation-space magnitude. **Tail-AV** is a short-probe signal identifying vulnerable
  routes; **PASA-KV** uses it to select action-facing K/V targets before training a fresh task-shared attack.
  Reported: across multiple WAM systems and **a physical robot**, readout-informed targeting is validated, attacks
  are strong in closed loop, and resistance differs substantially across systems. A safety boundary for the
  world-action-model line beside Bit-Flip Attacks on VLAs and TrapVLA; author-reported. **No code, weights, data
  or project page located (2026-10-31).**
- **Soei: Structured Observability of Evidence and Intent for Multi-Agent AI Software Engineering** (`2610.39468`,
  cs.SE) — why it qualifies: it puts a number on what a **recording contract** is for. First it **verifies an
  existing open-source observability tool (Agenttrail) against its source at commit `e4ba2da`**: nine documented
  constants and mechanisms hold at file-and-line level, but two event pipelines are never unified and its Map
  service exposes an **unauthenticated local mutation surface** — an audit of the auditor. Second it measures
  **45 real coding-agent sessions — 102,233 records, 20,577 tool executions, 2,336 human-to-agent episodes**:
  **23.4%** (95% CI 17.1–35.7) of code-editing episodes end with **no independent check after the last edit**,
  **57.8%** of to-dos marked complete after editing had no proof in between, a 10-minute timing heuristic for
  session hand-offs recovers only **6 of 24** pairs that content lineage links, **42%** of sessions exceed 24 h,
  **48.1%** of transcript bytes lie past a 2 MiB replay bound, and 1,326 records in 16 sessions contain
  credential-like strings. Third it proposes the Soei **acknowledgement-aware event schema** with evidence and
  intent structure, so the record distinguishes a check that happened from a claim that it did. Digital coding
  agents; no robot experiment; author-reported. **No first-party artifact located (2026-10-31)** — no repository,
  schema release or project page, and the arXiv record has no HTML full text (v1.1 preprint, 27 pages).
- **Assuring Learning-Enabled Cyber-Physical Systems with Foundation Models: A Neuro-Symbolic Agenda for Testing,
  Verification, and Runtime Assurance** (`2610.39547`, cs.RO) — why it qualifies: it is the only entry that
  allocates responsibility across a whole assurance stack instead of measuring one monitor. The **Foundation Model
  Assurance Stack** joins (i) neuro-symbolic architectures constraining the model through explicit symbolic world
  models, reasoning layers and interfaces; (ii) semantically grounded, conformance-based **testing** combining
  formal specifications and symbolic world models with foundation-model-assisted specification mining, scenario
  generation and test synthesis; and (iii) **compositional verification and runtime assurance** over formally
  tractable components and interfaces — modular contracts, equivalence checking, verified synthesis,
  uncertainty-aware reasoning, and runtime monitoring and enforcement. The load-bearing claim is that **symbolic
  abstractions are the assurance boundary**, separating stochastic learning from formal reasoning and connecting
  testing, verification and runtime evidence across the lifecycle, with the foundation model admitted as a
  constrained assistant whose outputs are independently checked wherever correctness can be formalized. It names
  the open problems (specification engineering, abstraction fidelity, evidence composition, lifecycle
  certification) and a staged agenda. **An agenda, not an evaluated system**: no experiments, no benchmark, **no
  artifact located (2026-10-31)**. Embodied CPS including autonomous vehicles and UAVs.

## Revision handling (no entry changed)

- **`id_list` sweep.** All **560** distinct arXiv IDs in `README.md` were resolved in six requests; **13** carry
  `<updated>` inside the run window. All 13 are **v1 records whose `<updated>` is their own same-day publication
  timestamp** from 2026-10-29, i.e. same-vintage duplicates, not revisions. **No v2 or later exists anywhere in the
  sweep**, so **no README entry required a revision update and none was made.** The three revisions recorded by the
  previous run (`2609.19906v2`, `2609.38905v4`, `2610.37255v2`) are unchanged.
- **In-batch check.** Neither re-screened day contains an `<updated>` > `<published>` record, consistent with both
  being single-vintage announcement blocks.

## Rejected, deferred and watch list

- **Read at abstract level and deferred as on-scope but not qualifying:**
  **What Should a Navigator Remember?** (`2610.38318`, cs.RO — task/temporal/spatial memory for zero-shot VLN, with
  useful ablations: removing temporal memory raises average path length 16.9 → 39.1 m; a memory-organisation study
  rather than a harness contract, and the memory line is already dense). **EgoContact** (`2610.39167`, cs.RO —
  22.5K verified egocentric interaction events for contact-point prediction, improving an affordance-transfer
  policy 14.2 → 31.2% in simulation and 16 → 58% on a real robot; a dataset/evaluation contribution but not a
  harness, and it needed more artifact work than the window allowed — **revisit next run**).
  **SpaceAct** (`2610.38405`, cs.RO — closed-loop 3D spatiotemporal benchmark with 108 verified instances, active
  perception and rollback feedback; strong, but the benchmark space is covered this run by BURGER and CuraEval and
  SpaceAct was not artifact-checked). **VeriCUA** (`2610.40578`, cs.AI — verification as agentic exploration for
  computer-using agents, rubric then GUI/code checks under explicit evidence rules; on-scope, no release statement
  or repository found, so deferred rather than included). **A-RSI** (`2610.38568`, cs.AI — adaptive task refresh
  gated on training-log reward variation, with a specified task-generation contract; promising self-improvement
  harness work, artifact unverified). **Efficient Scalable Oversight via Rewinding** (`2610.40535`, cs.AI —
  rewindable interactive proofs exploiting model-state snapshots; elegant, but oversight theory rather than a
  harness or interface). **Agent Transfer Protocol** (`2610.38398`, cs.AI — domain-mediated cross-organisation
  agent messaging; overlaps PCM's problem with a weaker verification story, so PCM carries the theme).
  **Auditable Belief Updates as a Missing Safety Layer** (`2610.38869`, cs.AI), **MAS-Loop** (`2610.38946`,
  cs.MA), **AgentDecept** (`2610.39377`, cs.AI), **ATLAS-Bench** (`2610.39438`, cs.AI), **rTaste** (`2610.40488`,
  cs.CL), **AI Cosmos** (`2610.40031`, cs.CL), **Practical Assessment of Agentic Systems in Acoustics**
  (`2610.39685`, cs.SD) — adjacent agent-evaluation work, deferred for scope density.
- **Deferred as model or pipeline contributions on the robot side:** **Cross-Controller Transfer via In-Context
  Behavior Adaptation** (`2610.39280` — controller-semantics adaptation, +33 points over naive pooling; a policy
  result), **Utility-WAM** (`2610.39367` — a genuinely useful audit that future-conditioned paths add +0.8/−0.8
  points on average while costing 1.7–1.9× latency and losing 54.0 points on *Press Stapler*, but the deliverable
  is a residual adapter), **Improving Fine-Grained Manipulation through Demonstration Expansion** (`2610.40430`),
  **RoboAtom** (`2610.40207`), **AcrossDEX 1.0** (`2610.40210`), **ScheduleGen** (`2610.39429`),
  **JEDI** (`2610.40356`), **TouchBridge** (`2610.40349`), **Gaussian Distillation of Flow Policies** (`2610.40271`),
  **Learning Safe Flow Matching Robot Policies** (`2610.40075`), **SymAnt** (`2610.40632`), **LLM-guided Planar
  Loco-Manipulation** (`2610.40248`), **OpenFlyV2** (`2610.39287`), **MATCH** (`2610.38248`),
  **CSO** (`2610.38357`) — policies, world models or data methods whose runtime interface is not the contribution.
- **Read and excluded as off-scope (representative, out of 815 titles):** SLAM, localization, mapping and
  calibration (`2610.38141`, `2610.38657`, `2610.39005`, `2610.39283`, `2610.39708`, `2610.39789`, `2610.40429`,
  `2610.40431`); robot hardware, mechanisms and actuation (`2610.38502`, `2610.39055`, `2610.39119`, `2610.39279`,
  `2610.39560`, `2610.39582`, `2610.40285`, `2610.40502`, `2610.40600`); conventional control, reachability and
  motion planning (`2610.38125`, `2610.38731`, `2610.38930`, `2610.38967`, `2610.39126`, `2610.39879`,
  `2610.40050`, `2610.40427`, `2610.40473`); perception-only, including the run-time-monitor paper
  **TRIM** (`2610.38613`, whose monitor is a real runtime-monitoring contribution but whose domain is 3D detection
  for autonomous driving, so it falls under the standing perception exclusion); autonomous-driving planning
  (`2610.40257`); **Securing the Robotics Supply Chain** (`2610.40385`, a federal-investment agenda with no
  technical artefact); video/game generation, including **Wizard** (`2610.40339`) and **RFW-A** (`2610.40333`);
  pure theory with no system (**From Grind to Grasp**, `2610.38728`); position pieces
  (**Agentic process evidence**, `2610.40071`); and the large LLM remainder (quantization, KV-cache, speculative
  decoding, serving, RLVR dynamics, RAG, ASR/TTS).
- **Watch list carried forward, re-checked only at link level this run:** **RoboSeed** weights ("Pending release"
  for all nine checkpoints), **ReSTEP** dataset and code, **EvoLife** ("Code is coming soon"), **Sandman**,
  **LOGIV** and **AgentHeal** (public, unlicensed), **METATRON** and **ELEVATE** (project page and demo only),
  **Scan Before You Install** ("upon acceptance"), and nothing-located **KARMA**, **VEvo-Harness**,
  **Masked Erosion**, **Prefix Executability**, **REALM** and the
  oversight-directives study. Newly added: **SkillSense** (`anonymous.4open` returns 401), **PASA-KV**, **Soei**
  and the **Assurance Stack** agenda (nothing located).
- **Corrected prior-pass status.** The ten inclusions above were **not** in the previous record's deferral or
  exclusion lists — they were never examined. For future runs the reliable check is not "is it in the deferral
  list" but "does the record name it at all"; an unmentioned ID inside a screened range is an unscreened record.

## Validation performed

- **Markdown structure.** 10 new entries are single-line list items appended under existing `###` headings —
  three to *General Harness Design and Self-Improvement*, one to *Agentic Robot and VLA Harnesses*, two to
  *Manipulation and VLA*, three to *Safety*, one to *Observability and Replay*. No heading was added, removed or
  reordered; every insertion used a unique anchor and the anchor count was asserted to be exactly 1 before each
  write, so no entry landed twice or in the wrong section. Blank-line separation between entries is preserved.
- **`git diff --check`.** Clean — no whitespace errors, no conflict markers, no trailing whitespace.
- **Links.** Every added URL was fetched during verification: 10 arXiv `abs` pages plus 10 full-text HTML pages,
  7 GitHub repositories through the REST API (plus one tree listing), 2 project pages (`gym-anything.github.io`
  returning HTTP 200, `siyingzhao.github.io/BURGER` returning HTTP 200 with content), the BURGER Hugging Face
  dataset endpoint (HTTP 200), and the `anonymous.4open.science` link recorded as **failing** (302 → 401) rather
  than being described as code. No third-party or self-referential link was added.
- **Dates and categories.** Every entry states its arXiv ID, primary category and date; all 10 are 2026-10-28
  (3) or 2026-10-29 (7), inside the re-screened range. Artifact checks are stamped **(2026-10-31)**.
  Section assignment: 3 general-harness, 1 robot-harness, 2 benchmark/evaluation, 4 runtime/safety.
- **Cross-file consistency.** The README badge and `Last verified:` now read **2026-10-31**, matching this
  record's filename date. All 10 IDs and titles are absent from `README.md`, `docs/` and `sources/`, so no
  duplicate was introduced; the topical near-neighbours were cross-referenced in prose instead of duplicated —
  PASA-KV against Bit-Flip Attacks and TrapVLA, Kairos against the planner-orchestration line, and VulnBench 2.0
  against the security-agent entries.
- **Artifact-status discipline.** Four distinct statuses are recorded literally: **verified open with a licence**
  (PCM MIT; Kairos and its benchmarks Apache-2.0; Gym-Anything MIT; BURGER MIT plus an HF dataset; CuraEval MIT;
  VulnBench 2.0 Apache-2.0), **link exists but does not resolve** (SkillSense, 401), **nothing located**
  (PASA-KV, Soei, the Assurance Stack agenda), and **agenda rather than system** (Assurance Stack). Digital-only,
  simulation-only and physical-robot scope is stated per entry, and every numerical result is labelled
  author-reported.

## Operational notes and blockers

- **An unmoved frontier is the correct signal for "nothing new", and it must not produce an empty commit.** The
  owed interval legitimately contained zero records because arXiv had not announced a 2026-10-30 batch at 09:35 UTC
  on a Saturday. The run therefore did **not** create an empty commit and did **not** push on that basis; the
  entire change set comes from the mandated re-screen of the newest visible batch, which is what the procedure
  requires precisely for this situation.
- **Second passes over ~500 records are where the recall is.** The 2026-10-30 run screened 949 records, read 56
  abstracts and 19 full texts, and still left PCM, Kairos, Gym-Anything, VulnBench 2.0, Soei, BURGER, CuraEval,
  PASA-KV, the Assurance Stack and the skill-utility paper unexamined — ten on-scope papers, several of them the
  strongest harness-contract work of the week. Whatever the first pass's coverage claim, a keyword screen tuned to
  `cs.RO` plus robot-anchored harness terms over the same bytes recovers them at the cost of parsing only.
- **Fix the Atom tag before trusting a screen.** The abstract lives in `<summary>`, not `<abstract>`; a extractor
  that asks for the wrong tag yields 815 blank abstracts, a screen that selects 87% of the batch, and a
  wildly inflated "not previously named" count. Assert non-empty abstracts and a shortlist of plausible size
  (here 121/815) before reading anything into the gap list.
- **A "closed" day is closed only after two independent looks.** 2026-10-29 held exactly 498 records on both
  passes (no growth), which is what makes the re-screen cheap: the previous run's XML could be re-parsed rather
  than re-downloaded. That check — live day count equal to the count already harvested — is the cheap gate for
  reusing stored bytes.
- **`/tmp` is still not durable across tool calls in this environment.** All XML, HTML and API responses were kept
  in `.scratch/arxiv-2026-10-31/` (untracked, never committed), consistent with the previous record's note.
- **OpenSSH workaround still required.** `git ls-remote`/`fetch`/`push` fail with
  `Bad owner or permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf`; `GIT_SSH_COMMAND='ssh -F /dev/null'`
  works and the user's key and known_hosts are unaffected.
- **Nothing left owed but the two unannounced days.** The next run must query `submittedDate` for **2026-10-30 and
  2026-10-31 again** (plus a frontier probe): both were 0 here only because they had not been announced, so they
  are owed, not empty. Expect a large combined batch.
