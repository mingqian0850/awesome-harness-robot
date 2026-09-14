# Daily arXiv scan — 2026-09-14

## Scope

- **Interval:** everything announced or indexed after the 2026-09-13 run's
  cutoff. That run screened the whole 2026-09-11 announcement block (716 unique
  IDs, newest in-category ID `2609.11929`) and explicitly left the 09-11 and
  09-12 submission blocks to this run ("The 09-11 and 09-12 blocks therefore
  remain next run's scope").
- **A new announcement block landed.** This run (started ~07:05 UTC on Monday
  2026-09-14) found a block with OAI **header datestamp 2026-09-14**:
  **935 set memberships = 697 unique base IDs** across the five target
  categories, newest base ID **`2609.13146`** (`2609.13200` already returns
  HTTP 404, so the frontier is at `2609.1314x`).
- **The export API was unavailable for this entire run.** Every request to
  `https://export.arxiv.org/api/query` returned **HTTP 429 "Rate exceeded."**
  (14 bytes), on the opening probe and on three later retries spaced ~20 s
  apart, including a five-record category query. As on 2026-09-13, this is
  recorded as a blocker for the documented primary query path, **not** as
  evidence about the index.
- **Substitute evidence (official arXiv infrastructure):**
  - **OAI-PMH**, `https://oaipmh.arxiv.org/oai`,
    `verb=ListRecords&metadataPrefix=arXiv&set=cs:cs:<CAT>&from=2026-09-11&until=2026-09-14`
    for cs.RO, cs.AI, cs.CL, cs.CV, cs.LG, every page followed to completion
    (no resumption token outstanding): cs.RO **172**, cs.AI **565**,
    cs.CL **276**, cs.CV **314**, cs.LG **555** = **1,882 memberships =
    1,393 unique IDs**. Datestamps: 2026-09-11 (947 memberships / 696 unique)
    and **2026-09-14 (935 memberships / 697 unique)**; zero records with
    datestamp 09-12 or 09-13.
  - **ID-existence bisect** on `arxiv.org/abs/<id>` to confirm the frontier.
  - **Direct record fetches** on `arxiv.org/abs/<id>` and
    `arxiv.org/html/<id>v<n>` for every candidate below, plus live artifact
    checks (GitHub REST API, Hugging Face Hub API, project pages).
- **Composition of the new block (697 unique IDs).**
  - **623 records carry a latest-version date of 2026-09-10 (146) or
    2026-09-11 (477)** — the Thursday and Friday submission blocks that were
    pending after the weekend — including replacement versions.
  - **74 records carry older latest-version dates.** These are the newly
    announced backlog (submissions that had not been announced before, e.g.
    `2609.11987`, v1 2026-09-08) plus cross-lists of older papers
    (`1304.2717`, `2405.09101`, clinical/geospatial/physics cross-lists). They
    were not in the 09-11 block and are screened here for the first time.
  - **84 records carry cs.RO**; 74 records survive the robot × system-concept
    filter (see Method).
- **Continuity check against the previous run.** The 09-11-datestamp group in
  this harvest (696 unique IDs) is a strict subset of the 716 IDs the 09-13 run
  screened; **20 of those 716 IDs were re-announced into the 09-14 block** and
  were re-checked here (`2512.09417`, `2608.10906`, `2608.22230`, `2609.03753`,
  `2609.03846`, `2609.06746`, `2609.08407`, `2609.10629`, `2609.10647`,
  `2609.10706`, `2609.10851`, `2609.10992`, `2609.11030`, `2609.11085`,
  `2609.11318`, `2609.11489`, `2609.11498`, `2609.11650`, `2609.11655`,
  `2609.11766`). No ID in either run's block is unaccounted for
  (`previous − (09-11 ∪ 09-14) = ∅`). Because a genuinely new block was
  present, the unchanged-batch re-screen requirement did not apply; the whole
  697-record block was screened regardless.
- **Revisions in scope.** The block contains replacement versions that the
  previous screens could not see: **EvoHarnessBench v2** (`2609.04280`,
  2026-09-10T18:02:08Z), **Code-to-Harness v2** (`2609.09468`,
  2026-09-10T23:56:08Z), **Retriever v2** (`2607.17213`,
  2026-09-11T09:52:44Z), **Graph-of-Skills v4** (`2604.05333`,
  2026-09-11T09:10:38Z), **GigaBrain-WBC-0.5 v3** (`2608.18234`,
  2026-09-11T05:22:00Z), and others. Each was version-diffed or read before a
  verdict.

### Unchanged-batch re-screen

Not applicable in the required sense — the block is new (697 IDs, none of them
the previous run's 09-11 batch except the 20 re-announcements above). The
previous run's full-block re-screen is therefore not repeated; instead the
*entire* new block was screened from raw harvested XML, including the 74
older-date records that no earlier new-submission window covered.

## Method

- **Concept sweeps over all 697 records** (title + abstract) for twelve
  concept families: harness/scaffold/orchestration/runtime; self-improvement
  and co-evolution; robot agent / embodied agent; VLA; robot foundation and
  world/action models; code-as-policy; skill discovery/selection/generation and
  tool libraries; memory; recovery/rollback/retry/resume; evaluation/benchmark;
  safety/monitoring/verification/permissions/attacks; manipulation.
  Union candidate pool: **553** records; robot × system "strong" pool: **74**.
- **Full title read of all 84 cs.RO-carrying records** and of every
  harness/self-improvement/skill/code-as-policy title hit (71 records).
- **Abstract reads of the strong pool** and of every candidate in the
  evaluation/safety families that names an artifact or a measurable contract.
- **Duplicate check:** all shortlisted names and IDs were grepped across
  `README.md`, `docs/`, and `sources/`. `EvoHarnessBench` and `Code-to-Harness`
  are already curated (their revisions are handled below); every other
  inclusion is a first exposure. `GigaBrain-WBC-0.5` had never been curated —
  the only prior GigaBrain text in the repository is the standing
  `GigaBrain-0.7/WBC` watch-list line, which is a different artifact.
- All performance numbers below are **author-reported** unless stated
  otherwise. Artifact status is literal and dated to this run (2026-09-14).

## Included (README updates)

Twelve new entries and one revision update.

### Retriever: Composing the Perception-Reasoning-Action Loop (v2)

- Paper: https://arxiv.org/abs/2607.17213 (v1 2026-07-19T12:14:28Z; **v2
  2026-09-11T09:52:44Z**, announced in this block; cs.RO; Linfeng Zhao, Haojie
  Mao, Jiayuan Mao, Weiyu Liu, Mykel Kochenderfer, Lawson L. S. Wong). The
  record names `http://retriever.systems` and `http://openretriever.org`.
- Artifacts: **verified open.** `github.com/openretriever/retriever`
  (Apache-2.0, 11 stars, created 2026-06-21, pushed 2026-09-03) and
  `github.com/openretriever/golden-retriever` (Apache-2.0, 10 stars, created
  2025-08-08, pushed 2026-09-03). `openretriever.org` advertises
  `pip install retriever-core`, docs, examples, a "Hub" of ready-made modules,
  and a Discord — both project domains and both repositories answered HTTP 200
  on 2026-09-14.
- Classification: Agentic Robot/VLA Harness.
- Why included: it is a genuine **robot-agent runtime and programming model**,
  not another policy. An agent is a graph of stateful causal stream functions
  executed on explicit *run clocks*, with one asynchronous environment–agent
  loop over continuous-time streams; finite-memory causal policies are shown to
  be representable as compositions of those operators. The compiler targets
  multiple backends and supports **deterministic replay from logged
  asynchronous data**, which is the property most robot-agent stacks lack —
  schedule-dependent behaviour that cannot be reproduced or debugged.
- Evidence: a real-robot case study plus controlled studies of runtime overhead
  and replay determinism. The v2 diff is modest (v1 9,143 KB → v2 9,147 KB;
  the record's link set is unchanged); the entry is justified by the paper and
  its artifacts, not by the revision. No independent reproduction.

### Agent as Policy for Robotic Manipulation (AGP)

- Paper: https://arxiv.org/abs/2609.12541 (v1 2026-09-11T07:51:39Z; cs.CL
  primary; Mengzhao Jia, Yang Lin, Xixin Zhang, Zhihan Zhang, Xiaobai Liu, Meng
  Jiang, and co-authors). No comment, project, or code link on the record.
- Artifacts: **none located — treated literally as not open.** The HTML's only
  repository links are third-party dependencies cited in the references; the
  robot itself is an **I2RT YAM arm** driven through the open-source
  `github.com/i2rt-robotics/i2rt` platform, which is a dependency rather than a
  release of this work.
- Classification: Agentic Robot/VLA Harness (code-as-policy).
- Why included: it pushes the "agent writes the policy" line onto real hardware
  with **no task-specific or environment-specific training**: a general-purpose
  agent interprets visual evidence, writes executable programs, issues motion
  commands, and revises its actions in response to physical outcomes. That is
  the repository's code-as-policy contract (RHO, VLCP, Guava) evaluated on a
  physical robot rather than in simulation.
- Evidence: real-world manipulation across precision manipulation, dynamic
  motion, and deformable objects — assembly from human videos, block
  construction from goal images, die reorientation, targeted throwing, and
  bimanual towel folding. The authors report **100% / 100% / 80%** success on
  three block-construction configurations. No simulation-only caveat is needed,
  but there is no artifact, no independent reproduction, and the task count and
  trial counts are small.

### Harness or Model? Isolating the Harness Effect (contamination-controlled)

- Paper: https://arxiv.org/abs/2609.11987 (v1 2026-09-08T21:24:40Z, announced
  in this block; cs.AI, cs.CL, cs.SE; single author, Mohsen Arjmandi; 25 pages).
- Artifacts: **replication package released, verified reachable.** The paper's
  replication package is an OSF project behind a **view-only link**
  (`https://osf.io/enjha/?view_only=…`), answering HTTP 200 on 2026-09-14, and
  contains the orchestrator, grading oracle, reanalysis code, and derived
  aggregates. **The 256-task suite itself stays private** by design to preserve
  contamination control. There is no public code repository.
- Classification: General Harness Methodology.
- Why included: it is the sharpest measurement yet of the question this
  repository's harness-effect line keeps asking — how much of an agentic coding
  result belongs to the harness rather than the model — and its answer is a
  **null result with a contamination-controlled design**: paired same-model
  contrasts on a private suite of 256 repository and post-cutoff contest tasks,
  with 792 of 800 planned runs graded by an isolated oracle.
- Evidence: neither contrast resolves an average advantage — **−1.25 pp** for
  Opus 4.8 (48.8% vs 50.0%, task-bootstrap 95% CI [−10.0, +7.5]) and
  **+1.25 pp** for GPT-5.5 (55.6% vs 54.4%, CI [−4.4, +6.9]). The Opus average
  hides opposite strata (native harness −9.0 pp on 61 repository tasks, +23.7 pp
  on 19 contest tasks; label-permutation p = 0.003), and the authors state the
  partition was chosen after seeing the data and needs a designed replication.
  Correctness and completion separate: 22 of 81 runs cancelled at the wall-clock
  ceiling had already produced a passing patch. Cost is re-priced from raw
  per-turn usage at frozen list prices — the neutral harness cost **1.3–1.6×**
  per solved task on Opus 4.8 and **1.2×** on GPT-5.5 — but 58 runs on the
  Anthropic account left no usage record, so allocating that spend either way
  moves the Opus ratio between 0.7 and 2.3 and the billed ordering is
  unresolved. The paper also documents that this v1 corrects an August 2026
  manuscript whose cost figures rested on a telemetry defect. Digital agents
  only, no robot experiment.

### Guardrailed Meta-Agent Loops (GuardrailLoop)

- Paper: https://arxiv.org/abs/2609.12216 (v1 2026-09-10T21:22:08Z; **cs.RO
  primary**; Qinzhen Ma, Jialin Wu; 10 pages).
- Artifacts: **none located** — no repository, project page, or dataset link on
  the record or in the HTML (verified 2026-09-14).
- Classification: General Harness Methodology; Evaluation/Safety.
- Why included: it turns three operational contracts of a self-improving agent
  loop into **jointly testable** properties — preservation of human-defined
  policy, compute accounting at every recorded execution prefix, and recovery of
  a specified scientific state after crashes — and its headline finding is
  exactly the kind of guarantee the repository's recovery line has been missing:
  **successful outcome recovery is not evidence of exactly-once execution.**
  A hash-pinned policy fixes goals, scope, evaluation identity, budget, and
  release conditions, while machine-directed evolution is restricted to a
  code-owned feature catalog and bounded knobs.
- Evidence: a paired 50-seed 2 × 2 study in which round-stage growth changes
  target attainment by **+1.00** and restricted mean compute-to-target by
  **−56.97 simulated GPU-hours** (95% paired-bootstrap interval
  [−58.91, −54.70]), while idle growth has zero measured utility effect. Across
  **240 enumerated crash injections all runs recover the defined outcome, but
  only 210 preserve the normalized trace** — 30 pre-commit crashes repeat a
  planner call. Resource-drift, kill-switch, integrity, and output-guard
  matrices satisfy their specified checks. The authors scope this as
  **conformance within one calibrated deterministic testbed**, not general
  safety or real-world self-improvement. Digital simulation, no robot
  experiment; no independent reproduction.

### Online Video Agent Harness for Long Video Understanding (VideoXAgent)

- Paper: https://arxiv.org/abs/2609.12818 (v1 2026-09-11T13:13:03Z; cs.CV,
  cs.AI; Sen Yang and co-authors; 35 pages). Project page:
  https://go-agent-x.github.io/video_agent_harness/ (live, 2026-09-14).
- Artifacts: **project page only — not open.** The site is a paper brief with
  no code, weights, or data link; treated literally as no released artifacts.
- Classification: General Harness Methodology.
- Why included: it is an explicitly **online** harness with the controls this
  repository looks for in an agent runtime — plan/decompose, on-demand expert
  tool invocation, multimodal evidence aggregation with conflict resolution,
  objective-evidence prompting to curb hallucination, and **budget-aware control
  to prevent non-termination** — over a data-driven taxonomy of atomic
  capabilities (scripts, VLMs, and domain models such as detection, OCR, ASR,
  face recognition). Its most transferable result is about harness design rather
  than model strength: the harness **remains effective with a visually weak or
  even text-only orchestrator**, so long-video competence can come from
  progressive evidence seeking instead of packing the video into one context.
- Evidence: competitive with frontier LMMs on Video-MME-Long,
  LongVideoBench-Long, LVBench, and MINERVA at about **50k tokens of agent
  context per sample** — on MINERVA, roughly **15%** of the context a
  1,024-frame dense-packing baseline needs. Non-robotic (video understanding),
  digital agents only; no artifacts; results author-reported.

### Look Before You Leap: Pre-Action Verification for LLM Agents

- Paper: https://arxiv.org/abs/2609.11957 (v1 2026-08-09, announced in this
  block; cs.LG, cs.MA; 8 pages).
- Artifacts: **claim and status disagree — treated literally as not open.** The
  abstract says "We release both benchmarks, the verifiers, and the guards," but
  the paper's own artifact table reads **"Code — Will-be-released. Datasets —
  Will-be-released."** No repository link exists on the record or in the HTML
  (verified 2026-09-14).
- Classification: General Harness Methodology; Evaluation/Safety.
- Why included: it makes **silent failure** — an action that produces a
  plausible but wrong effect and raises no error — the object of a deterministic
  check placed *before* the action takes effect, which is a distinct point in
  the harness's verification stack from the post-hoc monitors (CURA, OBPE) and
  the outcome verifiers already listed here. The mechanism is also a design
  contract: the verifier fixes the action's correct effect by construction and
  may **abstain rather than guess**, with a refuse-when-unsure policy converting
  silent failures into recoverable ones at a tunable cost in applicability.
- Evidence: across two action modalities in one framework — shell commands and
  code edits. A static verifier over **9,930 commands and 482 tools** catches
  **95.8%** of invalid commands at a 10.0% false-positive rate, and its syntax
  and binary checks are oracle-exact (zero false positives, half of all errors
  caught) while every false positive comes from the flag check, which is bounded
  by help-text coverage. For edits, a **640-edit / 224-file** benchmark
  isolating the apply step exposes a sharp split: content-anchored formats
  (search/replace, diff) fail cleanly, whereas location-anchored formats fail
  silently — line numbers corrupt **99.1%** of files under a one-line shift and
  function-name edits hit the wrong function **12.7%** of the time. Selective
  grounding reaches 0.958 recall at 7.0% false positives, and an
  anchor-and-verify applier records **one silent misapplication in 8,320 trials
  (0.01%)**. Digital agents, no robot experiment; results author-reported; code
  and data not yet released.

### ParaRecover: Process-Level Recovery Benchmark

- Paper: https://arxiv.org/abs/2609.12345 (v1 2026-09-11T02:09:33Z; cs.LG,
  cs.SE; **EMNLP 2026 Main Conference**; Bowen Guan, Zhentao Yin, Yanming Shen).
  The record names `https://github.com/gbw206/ParaRecover`.
- Artifacts: **verified open.** `github.com/gbw206/ParaRecover` (MIT, created
  2026-05-24, pushed 2026-08-29) contains `README.md`, `requirements.txt`,
  `.env.example`, and `analysis/`, `data/`, `environment/`, `scripts/` trees
  (GitHub contents API, 2026-09-14). This is code, not weights.
- Classification: Evaluation/Safety (recovery).
- Why included: recovery is a first-class harness primitive in this repository,
  but the existing recovery entries measure it inside a single system or a
  single runtime (REVISE, AgentRewind, the rollback-security study). ParaRecover
  makes it a **benchmark with a process-level rubric** rather than a final-score
  comparison, which is what the line needs to compare recovery behaviour across
  agents.
- Evidence: **10,626 instances** over two difficulty levels, built on a
  fine-grained taxonomy of **14 error types** covering planning dependencies,
  tool selection, and argument matching, targeted at multi-turn parallel
  tool-use where errors propagate across dependent branches. The proposed SDE
  rubric scores structural integrity, diagnostic reasoning, and evolutionary
  strategy during execution; across more than ten mainstream LLMs the authors
  report that even state-of-the-art models struggle with multi-turn error
  propagation, implicit tool-use failures, and precise replanning, and that SDE
  provides usable supervision for improving reflective recovery. Digital agents,
  no robot experiment; results author-reported.

### Graph-of-Skills: Dependency-Aware Structural Retrieval for Massive Agent Skills (v4)

- Paper: https://arxiv.org/abs/2604.05333 (v1 2026-04-07; **v4
  2026-09-11T09:10:38Z**, announced in this block; cs.AI, cs.IR; **EMNLP 2026
  Main Conference**; Dawei Liu, Zongxia Li, and co-authors). The record names
  `https://github.com/davidliuk/graph-of-skills`.
- Artifacts: **verified open.** `github.com/davidliuk/graph-of-skills` (MIT,
  **208 stars**, created 2026-04-03, pushed 2026-08-30), described as
  "[EMNLP '26] Dependency-Aware Structural Retrieval for Massive Agent Skills"
  (GitHub API, 2026-09-14).
- Classification: General Harness Methodology (skill discovery / retrieval).
- Why included: the repository tracks skill *selection* (SkillGate), skill
  *generation* (Skill-α), and skill *optimization* (COBRA-Skills), and GitSkills
  showed the corpus is millions of copied `SKILL.md` files; what was missing is
  the retrieval structure that decides which subset enters context. Graph-of-
  Skills builds an executable skill graph offline and retrieves a **bounded,
  dependency-aware bundle** — hybrid semantic-lexical seeding, reverse-aware
  Personalized PageRank, and context-budgeted hydration — targeting the
  *prerequisite gap* that plain semantic retrieval leaves behind.
- Evidence: across SkillsBench and ALFWorld with Claude Sonnet 4.5,
  MiniMax M2.7, and GPT-5.2 Codex, the authors report the highest average reward
  in all six model–benchmark blocks at a fraction of full-library token cost;
  on SkillsBench with GPT-5.2 Codex, **+7.0 absolute reward (25.6% relative)**
  over full skill loading while cutting total tokens by **56.7%**. Ablations
  localize the mechanism: replacing reverse traversal with forward propagation
  costs 9.1 reward points — more than removing the graph entirely — so the gain
  comes from traversing dependencies backwards, not from diffusion as such. A
  budget-matched study reproduces the ordering with dependency-pair co-recovery
  falling from 0.654 to 0.362. Digital agents, no robot experiment; no
  independent reproduction.

### Breaking the Vision-Action Shortcut: Latent Interface Training (LIT)

- Paper: https://arxiv.org/abs/2609.12641 (v1 2026-09-11T09:42:44Z; cs.RO;
  Jianman Lin, Shailesh Shailesh, Zhongyi Luo, Jiafei Duan). Project page:
  https://magiclab-nus.github.io/LIT/ (live).
- Artifacts: **verified open.** `github.com/MAGICLAB-NUS/LIT` (Apache-2.0,
  25 stars, created 2026-09-09, pushed 2026-09-13; `README.md`, `docs/`,
  `scripts/`, `results/`), Hugging Face **real-robot checkpoint**
  `shailes-h/Molmoact2-LIT` (16 files including `model.safetensors`, created
  2026-09-11), and Hugging Face **real-robot dataset**
  `shailes-h/yam_bimanual_manipulation` (32 files, 201 downloads) — all
  verified through the Hub API on 2026-09-14. Authors' affiliations as stated
  in the repository: South China University of Technology, National University
  of Singapore, Nanyang Technological University.
- Classification: VLA (robot foundation model).
- Why included: it names and attacks a concrete failure mode of the current
  generation — models that generate actions from pretrained visual
  representations can exploit task-irrelevant visual cues that correlate with
  demonstrated actions in-distribution (the *vision-action shortcut*), which
  then breaks under visual shift. The fix is an interface change rather than a
  new backbone: Stage 1 learns a spatial-goal-conditioned action prior **with no
  images at all**, and Stage 2 makes a pose-supervised latent interface the
  action expert's **only** visual pathway. Because it is framework-agnostic and
  the recipe is identical across architectures, it is a reusable training-layer
  contract rather than a single model release.
- Evidence: applied to four architectures — **Pi0.5, MolmoAct2, FAST-WAM, and
  ImageWAM** — LIT improves overall LIBERO-Plus success by **3.87–10.70
  percentage points** while preserving or improving average LIBERO success, and
  real-world evaluation shows **13.30–16.70 pp** gains aggregated across three
  tasks under unseen camera configurations, lighting variations, and
  distractors. Simulation and real-robot results are author-reported; code,
  one real-robot checkpoint, and the real-robot dataset are public.

### DropVLA: An Action-Level Backdoor Attack on Vision-Language-Action Models

- Paper: https://arxiv.org/abs/2510.10932 (**accepted at IROS 2026**; cs.CR,
  cs.AI, cs.RO; Zonghuan Xu, Jiayu Li, Yunhan Zhao, Xiang Zheng, Xingjun Ma,
  and co-authors; 8 pages; announced in this block).
- Artifacts: **verified open.** `github.com/megaknight114/DropVLA` (MIT,
  12 stars, created 2025-08-31, pushed 2026-07-25) — the repository the paper
  links as `TabVLA` now resolves here; it contains backdoor training and
  evaluation scripts plus `openvla/`, `prismatic/`, `scripts/`, and
  `experiments/` trees for LIBERO (GitHub API + contents, 2026-09-14).
- Classification: Evaluation/Safety (VLA security).
- Why included: it moves the VLA attack surface from task-level hijacking to
  **individual safety-critical action primitives** — forcing a reusable
  primitive such as `open_gripper` at attacker-chosen decision points — under a
  realistic pipeline-black-box setting with limited data-poisoning access. That
  is the exact granularity at which a harness's admission checks would have to
  catch an attack, and it complements the physical red-teaming and physical
  prompt-injection entries already listed.
- Evidence: on OpenVLA-7B with LIBERO, **vision-only poisoning reaches
  98.67–99.83% attack success with only 0.31% poisoned episodes** while
  preserving 98.50–99.17% clean-task retention, triggering the targeted action
  within 25 control steps at 500 Hz (0.05 s). Text-only triggers are unstable at
  low poisoning budgets (0.72% cross-suite transfer) and add nothing when
  combined with vision; the backdoor transfers across evaluation suites
  (96.27%, 99.09%) and is robust to moderate trigger variation. Physical
  feasibility is validated on a **7-DoF Franka arm with π0-fast**. Digital and
  physical evidence is author-reported; no independent reproduction.

### Robion: Efficient VLA Management and Serving for Robot Factories

- Paper: https://arxiv.org/abs/2609.12075 (latest version dated 2026-09-10,
  announced in this block; cs.DC primary with cs.AR, cs.LG, cs.PF, cs.RO;
  Dionysios Adamopoulos, Nattapol Chanpaisit, Basel Fakhri, Christina Giannoula,
  and co-authors).
- Artifacts: **none located.** The HTML links only the compared systems
  (`vllm-project/vllm-omni`, `sgl-project/sglang-omni`); no implementation
  release (verified 2026-09-14).
- Classification: Runtime/Safety/Observability (VLA serving).
- Why included: the two-stage VLA stack (VLM planner + action diffusion
  transformer) is usually assumed to run on the robot, but weight, cost, and
  power constraints push it to edge servers that must serve many robots under
  **latency SLOs** — a harness concern the repository has not covered. Robion
  disaggregates the two stages *within* one GPU using two streams, dynamically
  restricts SMs on the VLM stream so the ADiT stage always finds resources
  alongside it, co-locates multiple models over those streams, prioritizes
  requests by least remaining SLO time, and adds placement plus traffic control
  that maximizes per-model batching while bounding each GPU's load.
- Evidence: the authors report serving on average **6.7×** (vs vLLM-Omni) and
  **1.5×** (vs a monolithic single-pipeline baseline) higher robot load within
  98% SLO attainment for individual models, and up to **64 robots served within
  98% SLO attainment** in a large-scale experiment with 8 different models on a
  4-GPU server. Systems measurements, author-reported, no artifact and no
  independent reproduction; no robot experiment beyond the serving workload.

### GigaBrain-WBC-0.5: A Behavior World Model for Whole-Body Control (v3)

- Paper: https://arxiv.org/abs/2608.18234 (v1 2026-08-18T18:21:36Z; v2
  2026-08-23; **v3 2026-09-11T05:22:00Z**, announced in this block; cs.RO,
  cs.AI, cs.LG; Ziyang Cheng, Tianshu Tang, Jinxin Lan, and 17 co-authors;
  20-page technical report). Project page:
  https://shepherd1226.github.io/gigabrain-wbc-0.5/ (live).
- Artifacts: **not open — stated literally by the project page itself.** It
  reads "**Code coming soon**" and "Real-robot footage **forthcoming**"; there
  is no code, weights, or data link on the page or the record (verified
  2026-09-14). The paper is therefore curated for its system contribution only,
  and the hardware evidence rests on written claims rather than viewable
  footage.
- Classification: Robot Foundation/World Model.
- Why included: it makes a whole-body tracker into a **behavior world model**
  rather than a purely reactive policy — a causal Transformer jointly predicts
  its next action, next state, *and* the distribution over its next latent
  behavior command, so the network that acts also models how the environment
  reshapes what it can do next. The predicted distribution is then reused at
  deployment to detect implausible commands online and retract them onto learned
  behaviours, which is a test-time use of the world model as part of the runtime
  contract rather than as an offline evaluator. It also removes the usual
  flat-ground assumption: an automatic terrain-annotation pipeline recovers full
  3D contact geometry from retargeted motion at existing dataset scale.
- Evidence: the authors report the highest success rate across all four regimes
  among three large-scale tracker baselines — **81.3%** on terrain interaction
  (4.3× the strongest baseline), **83.1%** under implausible commands, and
  **99.3%** fall recovery (16.8× the strongest baseline) — with hardware trials
  under missing supports and disturbances, and a Unitree G1 checkpoint
  transferring to a Maker L01 robot with simple fine-tuning. Hardware results
  are author-reported and **no footage is public yet**; no independent
  reproduction.

### EvoHarnessBench v2 — revision of an existing entry

- Paper: https://arxiv.org/abs/2609.04280 (**v2 2026-09-10T18:02:08Z**; v1 was
  curated by the 2026-09-08 run).
- **The revision is substantive.** A v1/v2 HTML diff shows the abstract and
  introduction rewritten and, more importantly, **quantitative headline results
  added** to claims that v1 stated only qualitatively: harness-induced
  forgetting from expansion alone is now given as performance drops of
  **12.1% (tools), 13.8% (skills), and 46.4% (agents)**, and self-evolving
  adaptation under an evolving harness is quantified as **−3.3% (skills), −2.2%
  (tools), −1.2% (agents)** relative to task-specific reference harnesses. The
  appendix is renamed ("Additional" → "Extended" Related Work) and the HTML
  grows from 34 to 36 figures; the benchmark's structure and counts (17 streams,
  802 tasks, 520 tools, 42 skills, 62 agents) are unchanged.
- README action: the existing EvoHarnessBench entry was **updated in place**
  with the v2 numbers and the revision date, rather than duplicated. No new
  artifact was released with v2 — the project page remains the only public
  asset and no code repository is linked, so the "not open" status stands.

## Rejected / watch list

- **Code-to-Harness v2** (`2609.09468`, v2 2026-09-10T23:56:08Z): already
  curated on 2026-09-10; the v2 is **cosmetic** — same file size (221 KB), a
  two-sentence appendix-label difference, no new claims, and still no official
  artifacts. No README change.
- **CoSkill** (`2609.04865`): record carries an explicit author withdrawal
  ("Withdrawn pending internal content review and approval by the authors'
  institution"); screened and recorded, not curated. This is the second
  withdrawal on record after HODAgent (`2608.17584`).
- **Pelican-Sim 1.0** (`2609.12036`, cs.RO/cs.AI): substantial world-model
  simulator report (≈1M real and simulated trajectories; 500 generated
  trajectories per task raising policy success 70%→93% on RoboTwin). But
  `github.com/ZouShilong1024/Pelican-Sim1.0` contains only a **108-byte
  README** and a LICENSE, and the project page says "Models · Coming soon" —
  a placeholder is not an open release. **Watch for code and weights.**
- **DATAFARM** (`2609.12316`, cs.RO): TAMP data generation for VLA fine-tuning
  with reported 56.7% vs 8.3% (raw TAMP) success. The named project page
  (`prpl-group.com/datafarm/`) is an explicit **"project page is coming soon"**
  placeholder with no code — treated literally as not open. **Watch.**
- **VRL-Bench** (`2609.12404`): harness for fair evaluation of trial-and-error
  learning under finite trial budgets (MiniWoB, WebShop) plus the VEX²
  scheduler. Relevant to the harness/evaluation line but incremental against the
  existing verbal-RL and budgeted-evaluation entries, and no artifacts were
  located. **Watch.**
- **Embodied-BenchForge** (`2609.13082`): closed-loop agentic workflow for
  embodied benchmark construction with artifact dependency graphs,
  artifact-specific verification, and local re-execution/upstream rollback.
  Interesting construction harness; no artifacts located and the evidence is
  author-reported benchmark-quality assessment. **Watch.**
- **IMPLY** (`2609.12441`, cs.RO): argues that the self-consistency checks used
  to vet world-action models are physics-blind and replaces them with
  evidence-anchored disagreement (AUROC 1.00 vs 0.70 for self-consistency on the
  controlled case; 73% vs 52% correct-evidence preference on a real V-JEPA 2-AC
  model). A genuinely relevant **evaluation** idea, but a 7-page paper with no
  artifacts. **Watch.**
- **PhysCodeBench + SMRF** (`2604.23580`): benchmark and multi-agent refinement
  for physics-aware symbolic simulation code. Screened; the object under test is
  simulation-code correctness rather than a robot-agent harness contract — out
  of scope.
- **SimSkill** (`2609.03753`): self-evolving agent with episodic/procedural/
  semantic memory and a public Apache-2.0 repository, but the domain is traffic
  simulation (SUMO); domain-specific, no robot or general harness contract.
- **BlueLM-GUI** (`2609.12394`): 35B mobile GUI agent with a real-device
  flywheel and a quota-driven evolving benchmark. A model/technical report;
  the benchmark-iteration idea is close to the harness line but the contribution
  is a model release, not an external runtime or evaluation contract.
- **NS-VLA** (`2603.09542`): neuro-symbolic VLA with plan-constrained primitive
  inference and a public project page. A model-architecture paper; per the
  standing policy a model paper is not a harness paper unless it contributes an
  external runtime or interface — no such artifact was located here.
- **ARC: Autonomous Robotics Compliance** (`2609.12932`): proposed three-layer
  governance architecture; abstract-only (14 pages), no artifacts, no
  experimental evidence. Screened and set aside pending substance.
- **Robot policy/model papers screened and excluded** for supplying no external
  runtime, interface, or evaluation contract: `2609.13053` (Dynin-Robotics),
  `2609.12549` (STAR), `2608.27406` (CLAP), `2607.10625` (DASL),
  `2609.12498` (ArtManip), `2609.12103` (RodForesight), `2609.12245` (DIA),
  `2608.29208` (AdaVLA), `2609.10706` (HuRo), `2609.06718` (SkillX),
  `2609.12316` (DATAFARM, also watch-listed), `2603.09542` (NS-VLA, above),
  `2609.12721`, `2609.12677`, `2609.12634`, `2609.12737`, `2609.12894`,
  `2609.12927`.
- **Security/evaluation-adjacent papers screened, no harness contract or
  artifact:** `2609.12413` (SoK: jailbreaking agentic AI), `2602.17990`
  (WorkflowPerturb), `2606.10388` (Right Family, Wrong Skill), `2609.12459`
  (EvoRS), `2609.12265` (GTA), `2609.12863` (GenOR-Twin), and `2609.10630`
  (AI Safety: Not Optional, Not Later — a position piece).
- **Domain papers** excluded under the standing policy: driving, racing, and
  aerial (`2607.13410`, `2609.12371`, `2609.13011`, `2609.12456`, `2609.12795`,
  `2609.12660`, `2609.12292`, `2607.00141`, `2603.00338`, `2609.13015`),
  medical/bioprinting/surgical (`2609.12159`, `2609.12206`, `2609.12530`),
  perception, SLAM, odometry, and sensor datasets (`2609.12221`, `2609.12837`,
  `2609.12871`), multi-robot and swarm exploration (`2609.12502`,
  `2609.12959`), conventional control, gait, and energy-tuning work
  (`2609.12400` hexapod gait evolution, `2609.12971` ROS 2 costmap energy
  tuning), soft robotics and actuators (`2609.12258`, `2511.23372`), and the
  geospatial, clinical, physical-sciences, and materials cross-lists in the
  older-date group (`2609.12900` transient ice-flow solvers and the rest).
- **Withdrawn/withdrawal risk:** only `2609.04865` (CoSkill) in this block; it
  is recorded above and was not curated.

## Operational notes

- **The arXiv export API was rate-limited for the entire run** — `HTTP 429`,
  body `Rate exceeded.` (14 bytes), on the opening probe and on three later
  retries including a five-record query. Recorded as a blocker for the primary
  query path, not as evidence about the index. The substitute (OAI-PMH block
  harvest + direct `abs`/`html` fetches + ID-existence bisect) is official arXiv
  infrastructure and is stronger for the "did a block land?" question, because
  OAI datestamps records by announcement while the export API's date filters do
  not expose that axis.
- **OAI-PMH metadata semantics confirmed again this run:** the header
  `<datestamp>` is the announcement date, and the metadata `<created>` field
  carries the **latest announced version's** submission date. That is how the
  new block separates fresh work (623 records dated 09-10/09-11) from the
  newly-announced backlog and cross-lists (74 older-date records), and how the
  revisions above were identified (e.g. `2609.04280` carries created
  2026-09-10, its v2 date, although v1 is dated 2026-09-03).
- **GitHub REST API rate limits (HTTP 403)** appeared late in the run for the
  *search* endpoints; repository-lookup endpoints and direct `raw`/contents
  fetches continued to work. Artifact statuses below therefore rest on
  repository lookups, contents listings, project pages, and the Hugging Face Hub
  API rather than on search results.
- `/tmp` is not persistent between shell invocations on this host, so the
  harvest XML, parsed JSON, screening scripts, version diffs, and fetched pages
  were staged under **`.scratch/arxiv-2026-09-14/`** (untracked, deliberately
  not staged).
- The working branch was `main` throughout. Only task-owned files
  (`README.md` and this record) were staged; the unrelated user changes
  (`docs/reference-architecture.md` modified; `docs/ring-harness.png` and
  `handoff.md` untracked) were left untouched.
