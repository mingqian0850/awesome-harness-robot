# Daily arXiv scan — 2026-10-30 (2026-10-28 and 2026-10-29 batches plus the 2026-10-27 late tail: 949 previously unscreened records, and one correction from the 2026-10-27 at-frontier window)

## Scope

- **Interval owed.** The previous successful run started **2026-10-28 10:20 UTC** and committed at
  **2026-10-28 10:33 UTC** (11:33 Europe/Berlin) on `main` at `8b41135` with record
  `sources/daily-arxiv-2026-10-28.md`. It screened the 2026-10-27 batch plus the 2026-10-26 late tail
  (589 records) and closed 2026-10-27 at frontier ID `2610.37668` (`2026-10-27T17:59:59Z`). This run started
  **2026-10-30 10:52 UTC** on `main` at `8b41135`, so the interval it owes is **submission dates
  2026-10-28 → 2026-10-30**, plus **whatever of 2026-10-27 appeared after the previous cutoff**.
- **The previous run's "closed" day was not closed.** 2026-10-27 now returns **590** records against the
  **456** it screened — **134 new IDs** (`2610.37712`–`2610.38084`), published across the whole day
  (`2026-10-27T01:47:28Z` → `23:57:25Z`), not just late in the evening. The previous record already warned
  that a live day's count is a lower bound; this is the largest instance of that effect yet (+29.4%).
- **Day counts and the frontier.**

  | query (one OR-query over cs.RO/cs.AI/cs.CL/cs.CV/cs.LG per day) | 2026-10-27 | 2026-10-28 | 2026-10-29 | 2026-10-30 |
  | --- | ---: | ---: | ---: | ---: |
  | `submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` | **590** | **317** | **498** | **0** |
  | recorded by the previous run | 456 | — | — | — |
  | screened by this run | **134** (tail) | **317** | **498** | — |

  An unrestricted descending `cs.RO` query returns **`2610.40646`** (`2026-10-29T17:59:20Z`) as the newest
  record, against `2610.37668` (`2026-10-27T17:59:59Z`) at the previous run — the frontier moved 2,978 IDs.
  2026-10-29 is therefore **closed** (last record at 17:59:20Z) and **2026-10-30 is not yet open** (0 in
  `submittedDate` at 11:03 UTC), which is the normal mid-day state, not a zero-submission day.
- **The batch screened.** **949 unique records** never screened by any run: **134** late-tail 2026-10-27 IDs
  (`2610.37712`–`2610.38084`), all **317** of 2026-10-28 (`2610.38086`–`2610.39065`) and all **498** of
  2026-10-29 (`2610.39070`–`2610.40655`). The three ID ranges are contiguous with no gap, and `published`
  spans **2026-10-27T01:47:28Z → 2026-10-29T17:59:53Z**. The 456 records of 2026-10-27 at or below the old
  frontier belong to the previous run and were not re-screened as part of the owed interval. Because the
  previous batch was only screened once, this run **re-checked that window once** with a keyword filter: 74
  of its records carry an agent/robot/harness keyword and are not named anywhere in the 2026-10-28 record.
  Reading the closest of them found **one genuine miss — CASEL (`2610.36570`)** — which is included below as
  a correction. The other candidates are recorded in the deferral list so a later run can revisit them
  instead of treating them as screened.
- **Coverage.** All 949 carry at least one of the five target categories. Primary category counts:
  `cs.LG` **211**, `cs.CV` **197**, `cs.AI` **158**, `cs.RO` **109**, `cs.CL` **96**, `cs.CR` 26,
  `stat.ML` 24, `cs.HC` 11, `cs.SE` 10, `cs.IR` 9, `math.OC` 6, `cs.DC` 6, with the remainder single
  papers in `eess`, `cs.GT`, `physics`, `q-bio`, `quant-ph` and `math`. `cs.RO` appears as primary or
  cross-list on **128** records.
- **Revisions.** Within the 949 screened records **none** carries `<updated>` > `<published>`: the three
  days contained no in-batch v2. Revisions of already-listed papers are not discoverable through
  `submittedDate` or `lastUpdatedDate` (the 2026-10-28 record established that `lastUpdatedDate` tracks the
  *original* submission date), so this run repeated the **`id_list` version sweep of all 541 arXiv IDs
  listed in `README.md`** (6 requests). Exactly **three** have `<updated>` in the window:
  `2609.19906v2` (Closed-Loop Robot Software, 2026-10-29) — **entry updated**, see below;
  `2609.38905v4` (EmbodiRSI, 2026-10-29) — **no entry change**; `2610.37255v2` (Your Policy Is Secretly a
  Harness, 2026-10-28) — **no entry change**.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`, saved under
  `.scratch/arxiv-2026-10-30/` (untracked, never committed). One OR-query per day across all five
  categories, paginated at 200, sorted by `submittedDate` ascending: 2026-10-27 took three pages
  (200+200+190), 2026-10-28 two (200+117), 2026-10-29 three (200+200+98), 2026-10-30 zero.
  **19 export-API requests** (1 frontier probe + 4 day counts + 8 harvest pages + 6 `id_list` batches) at
  4–6 s spacing, **zero HTTP 429**.
- **Screening.** (a) Every one of the 949 titles was read in ascending ID order, then re-swept with a
  keyword filter. (b) Abstracts of **56** shortlisted records (52 from the owed interval, 4 from the
  prior-window re-check). (c) Full-text HTML of **19** records for release statements, repository URLs
  and result tables — this is what separated "claims an artifact" from "ships one" and caught the
  ReSTEP abstract/body contradiction. (d) Artifact verification against live sources: the GitHub REST API
  for **9** repositories (licence, size, creation/push timestamps, stars, top-level tree), **5** project
  pages fetched for HTTP status and content (plus one cited third-party page fetched to confirm it was
  *not* the paper's artifact), and **4** raw `README`/`LICENSE` files read to resolve licence and release
  status.
- **Deduplication.** Every shortlisted ID and title was grepped across `README.md`, `docs/`, `sources/`
  and `editorial/`; all 18 included IDs and titles are absent. Name collisions checked explicitly:
  **REALM** (a different EMNLP 2026 hosted library in `sources/daily-arxiv-2026-09-15.md`), **Preflight**
  (only the phrase "preflight passed" in an architecture diagram), **KARMA** and **HEMO** (no conflict),
  and **Recursive Self-Improvement in Physical AI** vs the listed **Toward Recursive Self-Improvement in
  Physical AI** (`2610.29677`) — different papers, explicit cross-reference added.

## Included (19)

### General Harness Design and Self-Improvement (5)

- **VEvo-Harness** (`2610.39158`, cs.CV) — why it qualifies: it makes the harness the evolving object and
  puts a **causal test in front of every update** — a visual traceback densifies coarse VLM judgments into
  root-cause attributions over a hierarchical verification tree, governance admits a skill patch only when
  counterfactual dual-arm replay shows that patch is responsible for the repair, a working/experience/pattern
  state distills root-cause-anchored episodes into a routable library, and a heterogeneous sandbox routes
  cost-aware repair primitives by cause. Reported: beats agentic generation baselines and rivals proprietary
  generators (GPT-Image, Nano-Pro). **No code, weights or data located (2026-10-30)**. Visual generation
  domain; no robot experiment; author-reported.
- **KARMA** (`2610.39884`, cs.AI) — why it qualifies: cleanest statement of **what should stay frozen** —
  model weights *and* executable harness code are fixed while six typed knowledge modules evolve, so every
  edit is inspectable and reversible. A deterministic Trace Verifier maps errors to modules, a Critic
  contrasts successful and failed trajectories, and a fixed gate keeps an atomic edit only when its measured
  gain clears a threshold, else restoring the previous state. Reported: hold-out **30.19 → 39.15**
  (+8.96 points, +29.7%) across 18 domain–model settings over three LLM families and ~500k real operation
  logs; transfer **+8.54**; ablations show removing either success or failure trajectories more than halves
  the gain. **No artifact located (2026-10-30)**. Digital agents; author-reported.
- **Masked Erosion** (`2610.38392`, cs.AI; 25 pages) — why it qualifies: it is the **failure mode of the
  self-improving-harness line**, measured. A gate scores task success, not what survives one safeguard
  failing, so deleting a redundant safeguard is invisible. Every subset of four written safeguards across
  two environments, **411 loops under ten gate rules** plus three GEPA runs: a masked deletion adds almost
  no observable harm; a paired significance test blocks chance-passing deletions but **not** deletions that
  genuinely raise success; **20-task batches kept every harness protected while 40-task batches lost all
  six** at the same cost; large harm audits prevent exposure but not lost depth, which surfaces under a
  weaker model (GEPA kept every safety rule verbatim yet two of three final harnesses misused credentials).
  A pre-registered statistical model predicts where each gate breaks; freezing safety text or one-layer
  ablation audits preserves depth. **No public repository located (2026-10-30)** — the paper says analysis
  code, predictions and run records are released with timestamps, but no link resolves. Digital agents.
- **Appreciation Teaches Unauthorized Execution** (`2610.39122`, cs.CR) — why it qualifies: the adversarial
  counterweight. A two-phase design in which the user asks the agent to read external content and complete
  the described task, the agent learns from feedback, and then a third party plants a similar task that the
  test request does not authorize. Self-evolution **raises** average attack success by **14.0–48.8 points**
  under skill evolution and **3.0–39.3 points** under reinforcement learning versus the non-evolving base.
  **Verified open (2026-10-30):** MIT `SaFo-Lab/Evolve-IPI` (1.1 MB, created 2026-10-24, pushed
  2026-10-28). Digital personal agents; no robot experiment; author-reported.
- **When Skills Tell Agents Not to Ask** (`2610.37734`, cs.CR; 2026-10-27 tail) — why it qualifies: it makes
  the **skill package itself the permission surface**. First large-scale measurement of oversight-loosening
  directives over **1.53 million English skills from GitSkills**: about **one in seventeen (~88,000)**
  contains one, **70.1%** apply unconditionally rather than only in specific contexts such as CI, and
  skills that run shell code, install packages or use `sudo` carry **1.7–1.8× the odds** without
  compensating tightening directives. 2,097 units hand-labelled for polarity, mechanism and scope.
  **No first-party code, model or data repository located (2026-10-30)**. Digital agents; author-reported.

### Agentic Robot and VLA Harnesses (4)

- **Ask to Evolve / EvoLife** (`2610.39446`, cs.RO) — why it qualifies: a **navigation harness that owns
  three decisions waypoint policies leave implicit** — how to move (hybrid action space of waypoint, local
  discrete, pixel-based and object-directed navigation), when to ask (active clarification/assistance), and
  what experience to carry forward (persistent spatial memory binding observations and dialogue to places).
  Eight benchmarks across four task families, **1.87 active interactions per episode**; authors report
  **100% first-visit success** on four 100-episode sets, complete-sequence **98%** on 50 LHPR-VLN and
  **94%** on 50 GOAT-Bench sequences, R2R-revisit SPL **82.23% → 93.31%** with retained experience, and a
  multi-day physical navigation deployment. **Artifact status literal (verified 2026-10-30): project page
  only** — the Code control is a disabled `href="#"`-class button reading "Code is coming soon"; no
  repository, weights or dataset. Simulation benchmarks plus a physical demo; author-reported.
- **LOGIV** (`2610.40016`, cs.RO) — why it qualifies: it answers the **persistence question** a successful
  action does not — a completed step does not ensure its effects outlive the rest of the plan. It compiles a
  validated plan into condition-support and resource-ordering edges, checks physical validity during
  execution, traces dependencies to decide what must be restored, constrains repair by retained unfinished
  actions, and symbolically re-validates the combined continuation before resuming. Runs above VLA and
  world-action-model policies with **no trainable components**. Reported: **+10.67** (RoboTwin V2) and
  **+15.33** (BiCoord) mean task success across three backbones without injected disturbances, better than
  three adapted recovery baselines under perturbations for every backbone on both, and real-robot success
  **12/30 → 17/30** on three tasks. **Verified public (2026-10-30):** `Enqiao-Lu/LOGIV`, a real 82.9 MB
  repository created and pushed 2026-10-29 (`logiv/`, `LOGIV_Demos/`, `assets/`), which declares **no
  licence** — source-available rather than open. Project page live.
- **METATRON** (`2610.38027`, cs.RO; NVIDIA) — why it qualifies: it makes the **execution layer programmable
  while the policy stays frozen**. A generative base policy receives partial goals (timed body-part targets,
  velocity commands, style cues); a skill layer expresses task intent as ordinary code issued as prompts at
  each control step, refined by coding agents that fit compact rules from motion data without task-specific
  policy training. One frozen checkpoint on a physical Unitree G1 covers walking, recovery, teleoperated
  pickup and sitting on furniture; authors report higher reaching success and push robustness than the tested
  reference-tracking pipelines. **Artifact status literal (verified 2026-10-30): project page and an
  interactive ProtoMotions demo only** — no repository, weights or dataset. Real-robot results
  author-reported; skill refinement demonstrated, not released.
- **ELEVATE** (`2610.38388`, cs.RO; 23 pages) — why it qualifies: it closes the loop between a planning agent
  and the controller library that limits it. **Persistent failures during task-program refinement are read as
  evidence of a missing physical capability**; a coding agent formulates and refines an RL problem (rewards,
  curricula, termination), and validated controllers join the capability library and are composed to retry.
  From one pretrained loco-manipulation controller it acquires wall-supported elevation, supported
  manipulation and wall descent over seven simulation tasks, reaching **98.4%–100% mean success over five
  runs** on the five tasks outside the initial workspace where fixed-controller coding-agent baselines fail
  outright; the programs and controllers transfer to a **physical quadrupedal manipulator** with no
  real-robot training or fine-tuning (**18/20** rack retrieval, **16/20** floor-to-rack placement).
  **No repository, weights or dataset located (2026-10-30)**; project page and videos only. Author-reported.

### Survey (1)

- **Recursive Self-Improvement in Physical AI: Mechanisms, Evidence, and Evaluation** (`2610.38266`,
  cs.RO) — why it qualifies: it turns the RSI question into a **measurement instrument**. It separates
  learner updates from *improvers*, compares policy learning, imagined experience, memory/skill accumulation
  and executable harnesses by what each revises, and proposes an **L0–L3 rubric** (task-local adaptation,
  persistent capability gain, inherited process revision, verified improver gain) plus common-data and
  interactive comparisons that evaluate a reference and a revised improver from **shared learner starts**,
  separating the *use* of experience from its *acquisition*. Contact validity, recovery, retention and
  resource cost gate whether gains accumulate. It is a **different paper** from the listed
  **Toward Recursive Self-Improvement in Physical AI** (`2610.29677`, 2026-10-24), which states four
  carry-forward conditions and reports the measurement gap; the cross-reference is in the README.
  **Verified public (2026-10-30):** `nkd-lkz/physical-ai-rsi-survey` (524 KB, pushed 2026-10-14, **no
  licence**). Survey; no experiments of its own.

### Benchmarks and Evaluation (5)

- **RoboSeed** (`2610.40026`, cs.RO) — why it qualifies: it replaces "highest success rate" with **five
  dimensions** — capability, downstream training affordability, deployment affordability, locally verified
  research value, reproduction evidence — mapped to 0–100 profiles by fixed anchors under balanced,
  training-constrained and deployment-constrained preferences. The discipline is in the refusals: interface
  and protocol checks decide when a comparison is appropriate, **missing measurements stay explicit**,
  incomplete aggregates are withheld, and weighted coverage names the gaps. Applied to VLA and WAM models
  through local training, released-weight evaluation, task-level audits and an evaluation-system study;
  reports budget-dependent backbone choices, sensitivity to adaptation recipes, and language-vs-visual task
  interface differences, and declines overall rankings. **Artifact status literal (verified 2026-10-30):**
  `PrimitiveX/RoboSeed` (9.9 MB, created 2026-09-28, pushed 2026-10-29) ships `configs/`, `data/`, `docs/`
  and `scripts/` with **no licence**, and its checkpoint registry marks all nine trained checkpoints
  **"Pending release"** — configuration and size records ship, weights do not.
- **Prefix Executability** (`2610.40444`, cs.AI, NeurIPS 2026) — why it qualifies: it makes the **tool-call
  prefix, not the terminal outcome, the evaluation unit**. The protocol requires every prefix of a predicted
  call sequence to remain executable under tool schemas, state preconditions and data dependencies, with
  Prefix Executability Curves, First-Error Depth, Trajectory Executability Rate, horizon-normalized AUC-PEC
  and Recovery Rate. On BFCL-v3 multi-turn, ToolSandbox and the complete τ³-bench Airline/Retail splits,
  **outcome-only and prefix-level rankings diverge**: plan-conditioned agents can post higher point-estimate
  final success while substantially lowering TER and AUC-PEC. Pre-execution plan checking is shown to
  improve executable-prefix survival at competitive terminal success. **No first-party artifact located
  (2026-10-30)**. Digital tool-use agents; author-reported.
- **Preflight-Bench** (`2610.39282`, cs.AI) — why it qualifies: it locates a **timing hazard the harness owns
  and usually leaves to a fixed sleep**. On 31 public pages, **0.4%** of clicks planned on a loaded page went
  stale within 30 s, against **17%** planned in the first second after page open; a ~1 s network-idle wait
  removes most risk, yet the wait rules of **8 of 10** widely used open-source agents still leave >1% stale.
  Commercial pages keep loading (15% of 65 booking/buying home pages reached network idle within 3 s of
  DOM-ready vs 63% of 3,028 `.gov` pages; 52% shifted layout within 6.5 s of a capped wait). A pixel check
  verifies target position as accurately as a frontier VLM at **1/8 the latency**, and a 4B model matches
  the frontier model at judging whether the previous action succeeded. Ships a benchmark generator, a
  break-even formula for when a check pays, and three rules. **Verified open (2026-10-30):** MIT code +
  CC BY 4.0 data in `multimodal-ai-engineering/preflight-bench` (28.6 MB, created 2026-10-27, pushed
  2026-10-29). Digital computer-use agents; author-reported.
- **Repairs for Whom?** (`2610.38610`, cs.SE) — why it qualifies: it asks the question every repair runtime
  skips — **does the evaluator pick repairs that help the agent that will execute them?** Holding failed
  trajectories and candidate single-step repairs fixed, it labels every candidate with independent rollouts
  of the deployed policy and measures the deployment value each evaluator loses. Greedy replay loses
  **0.08–0.13** expected net success in ALFWorld and **0.06–0.13** net reward in WebShop versus the deployed
  agent's own rollouts; at equal budget, selecting for Qwen2.5-7B with 14B rollouts loses **0.25** and
  **0.07**, twenty 14B rollouts are worth no more than two 7B ones, and fresh rollouts show opposite
  preferences. Cross-family same-size losses run **0.07–0.17** in both directions, and the scoring executor
  changes repair-location labels and method rankings. **Verified open (2026-10-30):** Apache-2.0
  `lyj20071013/repairs-for-whom` (15.3 MB, created and pushed 2026-10-27). Digital agents; author-reported.
- **Tracking Task Progress Through Errors and Recovery (ReSTEP / REMORA)** (`2610.39125`, cs.RO) — why it
  qualifies: it supplies the supervision progress monitors lack. **ReSTEP**: **3,156 episodes /
  1,692,390 synchronized frames (~15.7 h)** on an AgiBot G02 across two manipulation tasks, with semantic
  stage labels and **frame-level error annotations**, separating mistake-free episodes from mistake episodes
  whose errors are followed by recovery (both ending in success), plus head and bilateral-wrist RGB,
  proprioception and actions at 30 Hz. **REMORA** combines recurrent visual memory, distributional prediction
  over alternative hypotheses and causal peak selection; authors report beating SARM on progress accuracy
  and temporal consistency on both tasks. **Artifact status literal (verified 2026-10-30): not available** —
  the abstract says the dataset is open-sourced but the body says "we will publicly release ReSTEP", and the
  code "will be publicly released upon acceptance"; no download link or DOI was located. Real-robot data.

### Runtime and Safety Infrastructure (4)

- **CASEL** (`2610.36570`, cs.AI, CBASE 2026; 2026-10-27 at-frontier window — **correction to the
  2026-10-28 record**) — why it qualifies: it treats **where the runtime spends scrutiny** as an allocation
  problem rather than a threshold. For each proposed tool call it chooses among direct execution,
  verification, sandboxing, clarification, approval and abstention by minimizing a per-mode cost over
  calibrated failure probability, state-dependent consequence, reversibility, ambiguity and evidence
  freshness, formalizing direct execution as selective prediction with a finite-sample validity condition
  and a trajectory risk ledger. In a controlled simulation (20 seeds × 120,000 test actions, identical
  intervention menu and compute), harmful-side-effect trajectories fall from **14.75%** for an
  uncertainty-ranked gate to **7.02%** and mean normalized harm **0.184 → 0.123**; ranking by
  failure-probability × consequence reaches similar harm (**5.82%**, 0.120) but sends **45% more** actions to
  human approval; removing reversibility-aware routing raises the rate to **15.53%**; and a trajectory-level
  risk budget is *worse* than the baseline at equal cost (**11.82%**). Authors' own limit: **intervention
  effectiveness is assumed, not measured**, so this ranks allocation rules under the simulator's assumptions.
  **No first-party artifact located (2026-10-30)** — the linked project page (`agentabstain.github.io`) and
  cited repository belong to other papers. Digital tool-use agents; author-reported.
- **Tracekit Box** (`2610.38458`, cs.CE; 20 pages) — why it qualifies: it makes the **boundary itself the
  recorded object**, which sandboxes and tracers each do only half of. Each job runs in a Firecracker
  micro-VM with no network card; the only exits are host doors over vsock (commands, outbound connections,
  secrets, model calls, workspace patch). A signer the agent cannot reach decides at each door; names resolve
  on the host; private and cloud-metadata addresses are refused; secrets never enter the VM (placeholders,
  swapped only in one header for bound hosts); changes return as a per-file patch after each file is decided.
  Fails closed; each job ends as an offline-verifiable `.tkb` bundle. Reported: **13/13** end-to-end checks
  on a real KVM runner (28.5 s including image pull and boot), **15 fuzz targets** on the trust boundary
  yielding nine findings all fixed, and 102 signed records in the demo verifying. **Verified open
  (2026-10-30):** MIT `Cygnux-Labs/Tracekit-box` (10.8 MB, created 2026-10-25, pushed 2026-10-27). Authors'
  limits: no Firecracker jailer yet, needs a co-located signer, in-VM processes recorded only where they
  cross a door.
- **Sandman** (`2610.39711`, cs.DC; 42 pages) — why it qualifies: the **operational counterweight** to the
  single-agent sandbox. An Amazon service that creates, prepares, reclaims and tears down task environments
  for agentic RL at burst scale on shared Kubernetes; each sandbox is a pod with a small daemon mounted into
  an unmodified task image, files and processes persist across calls until release, and commands plus file
  operations are the interface. Reported: **11,858 concurrent tiny sandboxes** in a scale test and
  **3.1 million sandboxes** created by four production SWE-bench Verified training campaigns in October 2026,
  with design trade-offs for creation bursts, abandoned-sandbox reclamation and controlled evaluation.
  **Artifact status literal (verified 2026-10-30): not released** — the paper says the daemon, provider and
  pool "will" be open-sourced. Production digital-agent evidence; author-reported.
- **REALM** (`2610.39155`, cs.CR) — why it qualifies: it applies **least privilege to the agent's own task**
  rather than to the tool list, including assets whose identity emerges only during execution. Required
  assets become Dependency Graph nodes with resolved prerequisites; newly discovered assets are admitted
  through source-scoped selection; discovering an asset is separated from authorizing its use; the REALM
  Builder maintains grants and projects Asset Files into a Task Container, and a Partial API Manager exposes
  resource-bound APIs. Reported on **60 clean tasks and 560 injected cases** in AgentDyn with
  qwen3.5-flash: completion **40.0% / 36.4%** and attack success **16.6% → 0.2%**. **No first-party
  repository, model or data located (2026-10-30)**. Digital agents; author-reported.

## Revision handling (1 changed entry, 2 recorded without change)

- **Learning and Transferring Closed-Loop Robot Software** (`2609.19906`) — **entry updated.** v2
  (2026-10-29) adds a **physical Unitree G1 validation** absent from v1: 29 actuated body joints with fixed
  five-finger hands, generated policy code at **50 Hz above SONIC**, Claude Opus 5.5 generating and revising
  code for up to eight generations, best train candidate deployed after small task-specific runtime-parameter
  adjustments — closing a drawer **4/5**, a refrigerator door **3/5**, navigating to a refrigerator or
  cabinet **4/5**. Archive-based cross-task **transfer remains simulation-only**, and there is still no
  artifact. The README sentence "Simulation only (RoboCasa), no robot experiment" was therefore wrong as of
  v1 and has been replaced; the record's provenance for the change is the v1→v2 body text, not the abstract
  (the abstract changed too, but only to add one sentence).
- **EmbodiRSI** (`2609.38905`, v4 on 2026-10-29) — **no entry change.** The v3 and v4 abstracts are
  byte-identical (1,677 characters), the reported numbers are unchanged, and the Reproducibility Statement
  still says the full pipeline code "will be released soon". No artifact.
- **Your Policy Is Secretly a Harness** (`2610.37255`, v2 on 2026-10-28) — **no entry change.** Same-batch
  revision; the v2 abstract carries the same MiniWoB++ 85.0% → 19.2% observation-field reversal and the same
  claims, and its MIT `2020-qqtcg/HRP` repository remains live (pushed 2026-10-28, 21.8 MB).

## Rejected, deferred and watch list

- **Deferred as on-scope but not yet qualifying** (abstract read; none needed an artifact check unless noted):
  **HEMO** (`2610.40023`, cs.NE; 48 pages) — makes the automatic-operator-design harness a self-evolving
  target, a clean instance of the idea, but the domain is multi-objective combinatorial optimization rather
  than agents or robots and there is no artifact; the harness-as-target line is already carried by
  VEvo-Harness, KARMA and the listed ClawGym II. **Compile Away Uncertainty / SkillCompiler** (`2610.39508`,
  cs.AI) — compiles self-evolving skills into deterministic programs; interesting but a skill-runtime
  technique without a runtime contract of its own. **Video Generation Meets Agent Harness** (`2610.40191`,
  cs.CV) — a conceptual report on agentic video generation, agentic world modeling and recursive
  self-improvement with a live awesome-list repository, but no experiments of its own. **Teaching a Tree to
  Recover** (`2610.39775`, cs.RO) — online distillation of an LLM supervisor into a decision tree for
  replan/wait/help decisions; simulation-only, no artifact. **π0.5 inference settings on BEHAVIOR-1K**
  (`2610.37917`, cs.RO) — paired evaluation of inference and data-collection strategies across all 100 tasks;
  useful configuration evidence, project page only. **AtomTree** (`2610.38744`, cs.RO, IROS 2026) —
  hierarchical state-aware planner with backtracking replanning; project page, no artifact. **AgentHeal**
  (`2610.39234`, cs.CR) — goal-preserving resource recovery under access barriers, with public code and
  dataset but **no licence**; recovery evaluation is covered this run by ReSTEP and Repairs for Whom.
  **SkillCT** (`2610.38147`, cs.AI) — multidimensional skill-quality benchmark; no artifact located.
  **EnvOS** (`2610.40246`, cs.AI) — certifying environments that change mid-task for computer-use agents;
  on-scope but narrow next to Preflight-Bench. **Clean Sheets and Checkpoints** (`2610.39124`, cs.LG) —
  certification theory for sequential agent risk; no artifact. **VerifiedLab** (`2610.37923`, cs.AI) —
  multi-agent research-verification harness; different domain, no artifact check. **FilingLoop**
  (`2610.37905`, cs.AI) — interactive benchmark plus self-evolving agent for corporate disclosure review;
  benchmark and framework both new, deferred for scope. **Vendster** (`2610.38292`, cs.GT) — evaluation arena
  for how forecasting-tool quality changes agent negotiation outcomes; interesting, but a tool-quality
  measurement rather than a harness contract. **MUSE** (`2610.38332`), **CALM** (`2610.38573`), **MemSteer**
  (`2610.39276`), **Long-Term Agent Memory** (`2610.39790`) — memory frameworks; the memory line is already
  dense. **Skeleton Coding** (`2610.38214`) and **Precedent or Intent?** (`2610.38244`) — coding-agent
  behaviour studies, no harness contract. **Where You Inject Matters** (`2610.38867`) — injection-placement
  ("trust region") study; safety-relevant but no artifact located and adjacent to the included IPI paper.
- **Deferred as model/pipeline contributions on the robot side:** **Same Label, Different Interface**
  (`2610.38104`, an excellent interface-attribution study but a robot-learning methodology result),
  **SaiVLA-JEPA** (`2610.38508`), **How Much Should a VLA Trust Its Reasoning?** (`2610.40002`),
  **Consequence-Grounded Alignment for VLA** (`2610.39543`), **HiFOR** (`2610.40373`), **Continuous-Action
  World Models** (`2610.40401`), **PRIME W0** (`2610.39528`), **Seeing and Imagining as One** (`2610.39232`),
  **WorldProbe** (`2610.39721`), **SwapBot** (`2610.40646`), **Humanoid Dodgeball** (`2610.40003`),
  **EmbodiExp** (`2610.39180`), **ProLoop** (`2610.39319`), **Event2Reward** (`2610.39212`), **RoboAuto /
  Robotic Data Inoculation** (`2610.39054`) — policies, world models or data methods whose runtime interface
  is not the contribution.
- **Prior-window re-check (2026-10-27 at-frontier, 456 records).** CASEL was included as a correction. The
  remaining unmentioned keyword hits were read to title/abstract level and deferred rather than excluded:
  **Handoff State Continuity / HSC-Bench** (`2610.36523`, cs.CL — 1,039 tasks on recovering and continuing
  from inherited agent state; a real handoff contract, but no artifact and the closest thing to it this run
  already covers with Prefix Executability's trajectory protocol), **Blindfold** (`2610.37055`, cs.CR —
  audit-coin noninterference, showing that a silent audit leaks through tool-call latency and invalidates
  the certificate; strong, but a certification-theory result whose monitor is not released),
  **Constraining the Commit Boundary** (`2610.37407`, cs.AI — an authorization boundary between evidence
  for reasoning and evidence eligible to commit a persistent mutation), **Gym-Anything-RT** (`2610.37567`,
  cs.LG — real-time environments for vision-language agents), **From 12 to 200 Demonstrations**
  (`2610.37170`, cs.RO — ACT/Diffusion Policy/SmolVLA/π0.5 scaling study), **Proactive Memory for
  Event-Driven LLM Agents** (`2610.36967`), **WorkFactory** (`2610.37161`), **Follow the Gradient**
  (`2610.37481`, safety degradation traced to training data), **RubricAtlas** (`2610.36878`), **InfraDev**
  (`2610.36589`) and **From Interaction to Capability** (`2610.37482`). None was artifact-checked beyond a
  link scan; they are recorded here so a later run can revisit them.
- **Read and excluded as off-scope (representative, out of 949 titles):** autonomous-driving control,
  traffic forecasting and UAV/UGV teaming (`2610.38125`, `2610.38731`, `2610.38930`, `2610.39320`,
  `2610.39379`), medical imaging, clinical agents and health text (`2610.38289`, `2610.38389`,
  `2610.39681`, `2610.40318`, `2610.40379`), video/image generation and 3D reconstruction
  (`2610.38153`, `2610.38278`, `2610.39232`, `2610.40453`), perception, SLAM and calibration
  (`2610.38141`, `2610.38587`, `2610.38993`, `2610.39689`), humanoid/quadruped hardware and whole-body
  control (`2610.37974`, `2610.38502`, `2610.39327`, `2610.40003`), bio/chemistry/materials ML, and the
  large LLM remainder (on-policy distillation, quantization, KV-cache engineering, RLVR dynamics, RAG,
  ASR/TTS). Excluded per the standing rule: a paper enters only if it contributes a reusable agent/VLA
  harness, recovery, safety or evaluation contract.
- **Watch list for artifact releases** (verified 2026-10-30): **RoboSeed** weights ("Pending release" for
  all nine checkpoints); **ReSTEP** dataset and code ("will be released"/"upon acceptance"); **EvoLife**
  ("Code is coming soon"); **Sandman** ("will open-source"); **LOGIV** (public but unlicensed);
  **METATRON** and **ELEVATE** (project page and demo only); **AgentHeal** and **RoboSeed** (public but
  unlicensed); **Scan Before You Install** (`2610.37901`, the scanner, rule catalog and findings are
  "upon acceptance" — explicitly **not** counted as open); **KARMA**, **VEvo-Harness**, **Masked Erosion**,
  **Prefix Executability**, **REALM** and the **oversight-directives** study (nothing located).

## Validation performed

- **Markdown structure.** 19 new entries are single-line list items under existing `###`/`##` headings in
  the established `[Title](primary link) ([code](…), [project](…)) — …` shape, separated by blank lines; no
  heading level was added, removed or reordered (verified by diffing the heading list: all 39 headings and
  their order are unchanged). The Contents blurb was rewritten for this run rather than left describing the
  2026-10-28 pass, and two bullets were prepended to `Current Landscape` for this batch's themes.
- **`git diff --check`.** Clean — no whitespace errors, no conflict markers, no trailing whitespace.
- **Links.** Every added URL was fetched during verification: 19 arXiv abs pages/IDs, 9 GitHub repositories
  through the REST API plus 4 raw `README`/`LICENSE` files, and 5 project pages returning HTTP 200
  (`EvoLife-Web`, `logiv-project`, NVIDIA `metatron`, `behavior2026`, `atomtree-website`), plus one cited
  third-party page fetched only to confirm it was not CASEL's artifact. Three links were deliberately
  **not** added because they are third-party or self-referential rather than the paper's artifact: the
  cited `containers/bubblewrap` project in REALM, the `agentic-videogen` awesome list (which belongs to the
  deferred report), and `agentabstain.github.io` (a different, cited paper).
- **Dates and categories.** Every entry states its arXiv ID, primary category and date; 18 are 2026-10-27
  (tail) / 2026-10-28 / 2026-10-29, inside the owed window, and one is the 2026-10-27 at-frontier
  correction, labelled as such. Artifact checks are stamped **(2026-10-30)**.
  Section assignment: 5 general-harness, 4 robot-harness, 1 survey, 5 benchmark/evaluation, 4 runtime/safety.
- **Cross-file consistency.** The README badge and `Last verified:` now read **2026-10-30**, matching this
  record's filename date. No duplicate: all 19 IDs and titles are absent from `README.md`, `docs/`,
  `sources/` and `editorial/`, and the one topical near-duplicate (the two physical-AI RSI surveys) is
  cross-referenced in both directions in the README entry.
- **Artifact-status discipline.** Five distinct statuses are recorded literally: **verified open with a
  licence** (Preflight-Bench MIT + CC BY data; Tracekit Box MIT; Evolve-IPI MIT; Repairs for Whom
  Apache-2.0), **public without a licence** (LOGIV, RoboSeed, the RSI survey list), **public with weights
  explicitly pending** (RoboSeed), **announced but not available** (ReSTEP dataset and code, EvoLife
  "coming soon", Sandman, METATRON, ELEVATE), and **nothing located** (VEvo-Harness, KARMA, Masked Erosion,
  Prefix Executability, REALM, CASEL, the oversight-directives study). Simulation-only, real-robot and
  digital-only scope is stated per entry, and every numerical result is labelled author-reported.

## Operational notes and blockers

- **A "closed" day can still grow by 29%.** 2026-10-27 was screened as 456 records at 2026-10-28 10:20 UTC
  and now holds 590, with the 134 additions spread across the whole submission day rather than a late tail.
  Closure must be judged by the frontier probe and by re-querying the day at the *next* run, not by the
  count at the time. Re-screening the additions is cheap (one day query); skipping them is not recoverable
  from a count.
- **The `id_list` version sweep is now the primary revision detector and it paid off twice in a row.** Six
  requests over all 541 listed IDs found the Closed-Loop v2 hardware validation; `submittedDate` and
  `lastUpdatedDate` would both have missed it. Keep the sweep.
- **Abstracts and bodies can disagree on artifact status.** ReSTEP's abstract says the dataset is
  "open-sourced" while the body says "we will publicly release"; only the full-text read caught it. Treat
  the body's tense as authoritative and record the conflict.
- **`/tmp` is not durable in this environment.** A split-file written to `/tmp` in one tool call did not
  exist in the next; `.scratch/arxiv-2026-10-30/` (untracked, never committed) was used for all XML, HTML
  and API responses, consistent with the previous record's note.
- **OpenSSH workaround still required.** `git ls-remote`/`fetch`/`push` fail with
  `Bad owner or permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf`;
  `GIT_SSH_COMMAND='ssh -F /dev/null'` succeeds and was used for sync and push.
- **Rate limiting was not an issue.** 19 export-API requests at 4–6 s spacing, no 429; the GitHub API was
  not rate-limited.
- **No blockers.** No network, authentication, conflict or artifact-verification problem prevented a step.
