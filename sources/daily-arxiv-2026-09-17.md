# Daily arXiv scan — 2026-09-17

## Scope

- **Interval:** everything announced after the 2026-09-16 run's cutoff. That run
  (started ~04:00 UTC on Wednesday 2026-09-16) screened the block whose OAI
  header datestamp is **2026-09-16** — 1,061 set memberships / 779 unique base
  IDs, newest base ID `2609.17527` — covering the Tuesday submission block plus
  newly announced backlog. This run started ~07:47 UTC on **Thursday
  2026-09-17** and covers everything announced since.
- **A new announcement block landed.** Datestamp **2026-09-17**:
  **2,238 set memberships = 1,653 unique base IDs** across the five target
  categories for the two-datestamp harvest window, of which
  **898 unique base IDs carry the 2026-09-17 datestamp** — the block this run
  screened. Per-category memberships for the 09-17 datestamp: cs.RO 173,
  cs.AI 319, cs.CL 178, cs.CV 200, cs.LG 338. The newest base ID in the block is
  **`2609.19145`**, and the ID frontier is the same: `2609.19146` returns HTTP
  404 (ID-existence bisect on `arxiv.org/abs/<id>`, 2026-09-17).
- **Composition of the 09-17 block (898 unique IDs), by the submission date of
  the announced version** (the OAI metadata `<created>` field, which carries the
  latest announced version's date):
  - **615 records carry 2026-09-16** — the Wednesday submission block, which is
    what arXiv releases on Thursday.
  - **164 carry 2026-09-15** — held and cross-listed new submissions whose ID
    was assigned at announcement.
  - **119 carry earlier dates**, reaching back to 2006 (replacement versions and
    long-held submissions); **248 records in the block carry pre-`2609.` IDs**,
    the signature of re-announced replacements.
  - By category (all listed categories, not just primary): cs.LG 338, cs.AI 319,
    cs.CV 200, cs.CL 178, **cs.RO 173**, stat.ML 40, cs.CY 27, cs.SY 26,
    eess.SY 26, math.OC 22, cs.CR 22, cs.HC 21.
- **Continuity check against the previous run.** The previous run counted 1,061
  memberships / 779 unique IDs for datestamp 2026-09-16; this harvest sees 755
  unique IDs for the same datestamp. No base ID carries both datestamps (OAI
  records a single datestamp that follows the latest announcement), so the net
  **−24 IDs** is the same re-announcement churn the previous runs measured: a
  replacement announced on 09-17 leaves the 09-16 group. The 09-17 block
  contains 164 records created 2026-09-15 plus 119 older ones, which is the
  destination of that churn.
- **Export-API cross-check.** All **380** unique base IDs returned by the
  `submittedDate:[20260916000000 TO 20260916235959]` window across the five
  categories are inside the 898-ID block, and the block contains **518 further
  IDs** (held, cross-listed, and replacement records) that the export API cannot
  see. The API window is therefore a subset check, not the block definition.
- **"Unchanged batch" re-screen:** not applicable in the required sense — a
  genuinely new block landed, and the ID frontier moved from `2609.17527`
  (`2609.17531` existing) to `2609.19145`. The **entire** 898-record block was
  screened, including the 283 records whose announced version predates
  2026-09-16 and that no earlier new-submission window covered.
- **Revisions in scope.** Six replacement versions were version-diffed from the
  `abs` submission history and the v1↔latest HTML before a verdict:
  **LoopHarness v5** (`2608.27141`, 16 Sep), **EvoUndo v2** (`2608.28363`,
  16 Sep), **PACT v2** (`2609.01662`, 16 Sep), **The Latent That Never Was v2**
  (`2609.16745`, 16 Sep), **HarnessVLN v2** (`2609.15195`, 16 Sep), and
  **After the Party v2** (`2609.17274`, 16 Sep). MANGO (`2606.24815`) reached v3
  on 2026-09-15 — before this block — and was already read by the previous run.
  The other 242 pre-`2609.` records in the block were screened by title and
  category; none is in the repository's scope.
- **Withdrawals in this block:** none affecting a curated entry. Two records in
  the robot/VLA-adjacent neighborhood were withdrawn by their authors and are
  recorded so later agents do not re-screen them: **`2608.04765`** (Explicit
  Language Memory for Long-Horizon Planning in VLA Models — "unresolved
  differences among the authors") and **`2608.21388`** (Gimbal-Based Human
  Tracking for Companion Robots Using Continual Learning — "an incomplete draft
  was submitted prematurely"). Neither had ever been curated; `2608.21388` was
  already an exclusion note in `sources/daily-arxiv-2026-08-25.md`.
- **Watch-list continuity:** of the previous record's watch list, only
  **After the Party** (`2609.17274`) re-announced in this block (v2, retitled,
  replication DOI still unregistered). `2609.15213` X-WBC, `2609.13889` PMPA,
  `2609.13353` SkillAtlas, `2609.13679` REAL-I Challenge, `2609.13335`
  MetaTool-ROS, `2609.13231` ShieldVLA, `2609.15098` LG-VLN, `2606.24815` MANGO,
  `2609.15976` MessyMem, `2609.16705` Robot Data Factory, `2609.16635` EchoPath,
  `2609.17372` XPACE, `2609.17524` ModAR, and `2609.16644` WholeBodyWAM are
  **absent from the block**, so none of their artifact statuses changed.

## Method

- **Primary path — the arXiv export API over HTTPS
  (`https://export.arxiv.org/api/query`)** was available this run and was
  queried with `submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` per category
  for 20260916 and 20260917 across cs.RO, cs.AI, cs.CL, cs.CV, cs.LG.
  - 20260916 returned HTTP 200 after one `429 "Rate exceeded."` on the first
    cs.RO attempt (succeeded on a 20-second-spaced retry):
    cs.RO 88, cs.AI 127, cs.CL 78, cs.CV 83, cs.LG 128 = **504 entries →
    380 unique base IDs**, all at v1 (this is the Wednesday submission block
    that was announced on 09-17).
  - 20260917 returned HTTP 200 with **0 entries** for all five categories: the
    09-17 submission block has not been announced yet, the expected result for a
    run that starts before the 09-18 announcement.
- **Block harvest — OAI-PMH** (`https://oaipmh.arxiv.org/oai`,
  `verb=ListRecords&metadataPrefix=arXiv&set=cs:cs:<CAT>&from=2026-09-16&until=2026-09-17`)
  for the five categories; every response was a single complete page with no
  resumption token outstanding. Totals: cs.RO 277, cs.AI 622, cs.CL 336,
  cs.CV 381, cs.LG 622 = **2,238 memberships / 1,653 unique base IDs** across
  both datestamps, **898** of them carrying the 09-17 datestamp. This is the
  path that establishes *announcement* membership, which the export API's
  `submittedDate` cannot: it also captures replacement versions whose original
  submission date is far outside the window.
- **Screening:** keyword concepts (harness, scaffold, orchestration, runtime,
  agent loop, tool use, multi-agent, self-improvement, co-evolution, continual
  learning, robot agent, embodied agent, VLA, action expert, world model,
  world-action, code-as-policy, skill discovery, memory, recovery, rollback,
  evaluation, benchmark, audit, safety, security, attack, verification,
  guardrail, manipulation, teleoperation) plus a full title-level sweep of the
  block. All 173 cs.RO records were read at title level and every candidate at
  abstract level from the harvested metadata; 40 candidate abstracts were
  dumped in full.
- **Verification:** direct `arxiv.org/abs/<id>` and `arxiv.org/html/<id>v<n>`
  fetches for abstracts, comments, journal references, submission histories, and
  artifact links; live artifact checks through the GitHub REST API (repository
  metadata, license, size, contents listings), the Zenodo record API, the
  Hugging Face Hub, and the official project pages. **12 GitHub repositories,
  2 Zenodo records, 9 project pages, and 1 Hugging Face collection were fetched
  live on 2026-09-17**, and 5 v1↔latest HTML text diffs were run for the
  revisions.

## Included (README updates)

Seven new entries, two in-place revision updates, and four Current Landscape
bullets.

### WetRobo: A Reproducible Robot Kit for Coding Agents in Biological Laboratories

- **`2609.18435`** (cs.AI/cs.RO, v1, 15–16 Sep 2026). Category: **Agentic
  Robot/VLA Harness**; also a runnable agent-to-robot bridge. Placed under
  Agentic Robot and VLA Harnesses.
- The contribution is the *environment as the distributable artifact*, with the
  coding agent as the runtime: one AgileX Piper arm with a string-driven
  Dynamixel gripper, a fixed head RGB-D camera (iPhone via Record3D) plus a
  wrist camera, an incubator and bench equipment, fifteen teleoperated
  demonstrations per task, the base control stack (one-demo replay, live bias,
  safety layer, camera streaming, arm RPC server), and an `AGENTS.md` skill file
  carrying the operating rules and starting workflow. The task is deliberately
  not part of the environment; a biological experimentalist gives a
  natural-language task at run time and the agent observes the lab, writes and
  executes programs, and uses external tools to adapt — no local teleoperation
  data collection and no neural-network training. The repository also keeps a
  `wetrobo+petri+cap+door` branch as an evolved-code exemplar.
- The transfer claim is the reason to include it: with OpenAI Codex
  (gpt-5.6-sol) the agent performed three real tasks (Petri dish lid, bottle
  cap, incubator door), and the authors report that it achieved the cap task in
  **both Lab X and Lab Y**, whereas a VLA fine-tuned on Lab X demonstrations
  succeeded there but failed to transfer to Lab Y.
- **Artifact status verified (2026-09-17):** `tsudalab/WetRobo` is live
  (201 MB, created 2026-09-15, pushed 2026-09-15) with `AGENTS.md`, `demo/`
  (`petri_lid`, `bottle_cap`, `incubator_door`, one HDF5 trajectory plus a
  head-camera preview per episode), `robot/`, `rollout/`, `src/`, and teleop
  scripts — but **asserts no license**, so it is described as source-available,
  not open source.
- Real-laboratory evidence, three tasks, author-reported; no independent
  reproduction, and no failure statistics beyond the reported task outcomes.

### GPT-Policy: In-Context Robot Learning with VLM Agents

- **`2609.19138`** (cs.CV/cs.RO, v1, 16 Sep 2026). Category: **Agentic
  Robot/VLA Harness**. Placed under Agentic Robot and VLA Harnesses.
- Tests whether a commercial VLM's agentic competence can be spent on robot
  control without gradient updates: a **context compiler** keeps task-relevant
  visual transitions (human video, robot video with or without time-aligned
  end-effector poses/gripper states/commands, goal images, interaction history,
  online human help); the VLM proposes robot-tool actions through a structured
  interface; a **constrained controller** interpolates poses, applies inverse
  kinematics, verifies each request, executes it, and returns the outcome before
  the next decision.
- Reported real-robot results (ARX X5 and I2RT/YAM arms) separate what each
  context source buys instead of reporting one aggregate: one human
  demonstration video lifts "Pick Red Towel" and "Pick Up Notebook" from 0/3 to
  2/3 completion with fewer decisions (96.3→76.7 and 94.0→66.7) and less
  wall-clock time; adding action references to robot video takes bottle-cap
  opening from 0/3 (none) and 2/3 (video) to 3/3 and plug reinsertion from 0/3
  and 0/3 to 2/3. Matched model comparisons and context ablations document where
  the approach remains unreliable.
- **Artifact status verified (2026-09-17):** the paper links the project page
  `cheng-haha.github.io/GPT-Policy/` and `cheng-haha/GPT-Policy`; the repository
  is live and active (234 MB, created 2026-09-10, **pushed 2026-09-17T07:32Z**,
  173 stars) with `configs/`, `docs/`, `scripts/`, `src/`, `tests/`, and
  hardware adapters for both arms. Its `LICENSE` file states that **"no license
  has been selected for redistribution or commercial use yet"**, so the release
  is source-available for review/evaluation, **not open source**. The
  `GPT-Policy-Eval` repository named in the README now redirects to
  `GPT-Policy`.
- Trial counts are small (three per condition); results are author-reported with
  no independent reproduction.

### AeroWeaver: An Embodied-Agent Harness for Weaving Aerial Skills into Distributed, Adaptive Swarm Execution

- **`2609.18520`** (cs.AI, v1, 16 Sep 2026). Category: **Agentic Robot/VLA
  Harness**. Placed under Agentic Robot and VLA Harnesses.
- Extends the harness contract from one robot to a swarm: every skill declares
  semantic purpose, invocation interface, and operating conditions, and is
  routed to the vehicle that owns its execution interface; a Commander supplies
  task context and monitors mission progress but holds **no flight authority**;
  role-conditioned local agents act body-locally instead of a central agent
  emitting joint actions from global context; and role-indexed
  state–action–reward records refine skill selection online while model
  parameters, skill definitions, and flight executors stay frozen.
- Reported evaluation: nine MPE-inspired closed-loop tasks in an AirSim runtime,
  54 task–condition pairs × 10 episode-return observations, two LLM baselines
  (Centralized, HMAS-2) and three component controls, with higher mean episodic
  returns than both baselines at fewer tokens, plus ablations for
  experience-guided adaptation, backbone sensitivity, and mission-wording
  perturbation.
- **Artifact status verified open (2026-09-17):** `Admire-ljb/AeroWeaver` is
  **MIT-licensed** (5 MB, created 2026-07-26, pushed 2026-09-15, 3 stars,
  topics include `airsim`, `drone-swarm`, `llm-agents`) with `backend/`,
  `frontend/`, `deploy/`, `docs/`, `experiments/`, `tests/`, and `paper-assets/`.
- Evidence is **simulation-only** — the paper states physical UAV validation as
  future work — and author-reported. Included despite the aerial domain because
  the governed-skill-plus-role-local-agent contract is reusable.

### Bad Genius: Counterfactual-Guided Harness Evolution Beyond Task-Specific Shortcuts

- **`2609.18366`** (cs.AI/cs.LG/stat.ML, v1, 16 Sep 2026). Category: **General
  Harness Methodology**; also Evaluation/Safety. Placed under General Harness
  Design and Self-Improvement.
- Names the failure mode automatic harness optimization invites: task holdout
  varies semantic tasks but leaves the **benchmark protocol** fixed, so a
  Proposer editing prompts, memory, retrieval, tools, and control code can
  produce a cheating harness whose released-benchmark gain depends on a
  protocol-wide shortcut rather than capability.
- **CHASE (Counterfactual Harness Search and Evolution)** casts harness
  evolution as constraint generation over validity-preserving benchmark
  counterfactuals: after each Proposer update a Challenger searches for an
  executable protocol transformation with large gain destruction; a validity
  firewall checks that task semantics are preserved; a confirmation set decides
  whether the counterfactual enters a finite archive. The paper formalizes an
  exact shortcut-neutralized benchmark and links finite counterfactual archives
  to it with statistical guarantees, then evaluates on a synthetic benchmark and
  OfficeQA, where CHASE retains released-benchmark gains while substantially
  reducing gain destruction under valid protocol changes.
- **Artifact status is literal and negative:** no repository is linked on the
  record or in the HTML — the only GitHub links are to the third-party OfficeQA
  benchmark (`databricks/officeqa`) and its Hugging Face datasets (verified
  2026-09-17). Treated as not open.
- Digital agents, no robot experiment; results author-reported.

### EvoSkill-GUI: Reflect, Revise, Reuse — Training-Free Skill Evolution for GUI Agents

- **`2609.17653`** (cs.AI/cs.LG, v1, 15 Sep 2026, held into this block).
  Category: **General Harness Methodology** (skill discovery/evolution). Placed
  under General Harness Design and Self-Improvement.
- Each skill is a structured multi-file package — retrieval metadata, executable
  plans, backup localization, failure-recovery rules, accessibility utilities,
  and recorded failure cases — so a skill is an editable artifact rather than a
  prompt fragment. The **reflect–revise–reuse** loop runs at three levels:
  instant in-rollout revisions by the executor, an isolated critic that
  diagnoses failed trajectories under strict information isolation, and
  restricted file-level edits applied through a constrained tool interface.
- The authors report maximum gains of **+16.2%** on MobileWorld, **+6.0%** on
  AndroidWorld, and **+10.5%** on OSWorld across multiple base models with no
  training, with evolved libraries continuing to benefit related tasks.
- **Artifact status verified open (2026-09-17):** `ZJU-REAL/EvoSkill-GUI` is
  **Apache-2.0** (82 MB, created 2026-08-12, pushed 2026-08-29) with `src/`,
  `docker/`, `docs/`, `resources/`, `scripts/`, `site/`, `tests/`, and `tools/`;
  the project page `zju-real.github.io/EvoSkill-GUI/` is live.
- Digital GUI agents, no robot experiment; results author-reported.

### ScienceIDE: Turning the World's Scientific Codebase into Agent-Learnable Environments

- **`2609.19134`** (cs.CL/cs.CY, v1, 16 Sep 2026). Category: **General Harness
  Methodology** (environment construction for training and evaluation). Placed
  under General Harness Design and Self-Improvement.
- Attacks the *scientific experience bottleneck*: decades of executable
  knowledge sit in scientific repositories behind fragmented toolchains,
  implicit domain conventions, and specialized correctness criteria. Guided by
  expert-defined scientific cases and acceptance criteria, agents transform
  repositories into executable environments supporting task generation,
  execution, and **scientific verification**, providing one shared substrate for
  supervised fine-tuning, reinforcement learning, and evaluation.
- The authors train PhAI-IDE-72B/9B/4B on verified interaction trajectories and
  report gains in held-out scientific-code repair plus improvements on selected
  general code, reasoning, and knowledge benchmarks, which they read as positive
  transfer from scientific experience.
- **Artifact status verified (2026-09-17):** `aitofound/ScienceIDE` is live
  (280 MB, created 2026-09-11, pushed 2026-09-17, 12 stars) with `RL/`, `SFT/`,
  `docs/`, `environments/`, and `hard85/`, and it **asserts no license**; the
  project page `aitonomy.org/projects/scienceide` is live; the model series is
  published as the Hugging Face collection `AItonomy/scienceide-model-series`
  (all HTTP 200).
- One caution recorded rather than cited: the Zenodo DOI
  `10.5281/zenodo.21262220` printed in the paper's reference list resolves to an
  unrelated record (`AxFoundation/strax: v2.2.3`). It is a **reference
  citation**, not a data-availability statement for ScienceIDE, so no release
  claim is affected — but it is another reminder that a DOI in a paper's HTML is
  not necessarily that paper's artifact.
- Software/science environments only, no robot experiment; results
  author-reported.

### AutoTuneBench: Trustworthy Measurement for Agent Auto-Tuning of LLM Serving Engines

- **`2609.18123`** (cs.AI, v1, 16 Sep 2026). Category: **Evaluation/Safety**;
  also General Harness Methodology. Placed under Benchmarks and Evaluation →
  Agent and Embodied Reasoning.
- Makes the *measurement* the artifact when an agent runs a propose–measure–keep
  loop over GPU kernels and serving engines. A four-day pilot corpus of 619
  model calls characterizes four failure modes — strawman baselines manufacture
  speedups, absolute times do not transfer across machines, saturated tasks
  nullify comparisons, and infrastructure defects impersonate science.
- The protocol makes trust architectural: frozen as code with test-enforced
  provenance, database-level rejection of out-of-protocol results on ingest,
  anti-cheat checks **outside the agent's modification surface**, pre-registered
  readouts, measurements anchored to externally published results, and
  paired-seed statistics under a 5% cross-run coefficient-of-variation cap.
- Honest measurement rewrites the headlines: best kernel **10.6×** against a
  naive baseline but **2.03×** against an honest one; one configuration delivers
  1.174× on one machine and 1.0049× on another; a pre-registered on/off
  comparison nulls at a shared wall (2.4840 vs 2.4957 ms); KernelBench Level-1
  admits 51% of tasks at a median 1.0001× over PyTorch eager.
- **Artifact status verified open (2026-09-17):** `li-ch/autotunebench` is
  **MIT-licensed** (3.4 MB, created and pushed 2026-09-16) with a claim-by-claim
  `ARTIFACT.md`, `config/protocol.yaml`, `gates/`, `pdb/` ingest validators,
  `agents/`, `orchestrator/`, `sandbox/`, `observability/`, `engines/`,
  `experiments/`, and 329 tests; the `v1.0.0` release tag resolves.
- Digital infrastructure agents, no robot experiment; results author-reported.

### Revision updates (in place)

- **LoopHarness v5** (`2608.27141`) — v1 27 Aug → v2 28 Aug → v3 2 Sep →
  v4 3 Sep → **v5 16 Sep 2026** (226 KB, down from 347 KB; announced in this
  block). The v5 adds the evaluation the earlier versions deferred: on native
  Agent-SafetyBench tasks, overall outer-state attack success is reported at
  **88.4–97.6% for the evaluated baselines versus 0.1% for LoopHarness** at
  **96.9% clean target completion**, with the bound stated as
  `B+m−1+m/δ_M` under mediated commits and an arbiter detection floor, plus
  per-module ablations, a controlled retention study, and an adaptive white-box
  red team. The v5 also carries new affiliations (JD.com, Beihang University)
  and a restructured theory section ("observation-interface separation"). The
  README's Safety entry and the Current Landscape bullet were updated with the
  numbers; no code or other artifacts are linked.
- **The Latent That Never Was** (`2609.16745`) — the artifact release landed
  between runs. The v2 re-announcement (16 Sep) is a metadata/primary-category
  update whose HTML text is otherwise **identical to v1**, but
  `aida-ugent/act-cvae-forensics` is now **Apache-2.0 and populated** (24.7 MB,
  pushed 2026-09-16T05:23Z) with `ARTIFACTS.md`, `REPRODUCING.md`,
  `VERIFICATION.md`, `artifacts/`, `evidence/`, `experiments/`, `numbers/`,
  `tests/`, and `tools/`, and `bokang-ugent/nanoACT` is now **Apache-2.0** (54 KB,
  pushed 2026-09-16T05:29Z) with the `nanoact/` package, tests, and `uv.lock`.
  The previous run verified both at ~04:00 UTC on 09-16, hours before they were
  populated; the README's benchmark entry and the Current Landscape bullet now
  record the release instead of the placeholder.

### Current Landscape additions (4 bullets)

- The coding agent becoming the deployed robot harness, with the kit as the
  distributable artifact (WetRobo + GPT-Policy).
- Embodied-agent harnesses extending past single arms to distributed platforms
  (AeroWeaver).
- Harness optimization being audited against the benchmark it optimizes (Bad
  Genius/CHASE + AutoTuneBench).
- Skill libraries and executable environments becoming the harness artifacts
  (EvoSkill-GUI + ScienceIDE).

## Rejected / watch list

### Revisions read in this block, not qualifying

- **EvoUndo v2** (`2608.28363`, v1 28 Aug → **v2 16 Sep**, cs.AI): the v2
  abstract keeps the numbers the README already carries from v1 (197
  capability-improving mutations failing recoverability verification;
  conventional repair 0/197; 48/197 under the original recovery language L0;
  191/197 with the extended calculus; the protocol-locked 2×2 intervention
  lifting 0/48→38/48 and reaching 142/143 in the oracle-defined stratum;
  133/143 on the primary gpt-oss-120b backbone; the Qwen3.8-27B replication not
  reproducing the negative interaction). The diff is framing ("single-sample
  zero-shot generation", "state-grounded diagnostic feedback" for "exact
  state-address grounding"), added appendices, and the replication note — no new
  results and no artifacts. **No README change needed**; the curated entry
  (2026-08-31) is already accurate.
- **PACT v2** (`2609.01662`, v1 31 Aug → **v2 16 Sep**, cs.AI/cs.RO): retitled
  "…Provenance-Conserving Fusion and Typed Action Admission", with clarified
  theoretical assumptions and restructured evaluation; the README's description
  (refuses to count repeated inference over one observation as corroboration
  before robot actions are admitted) remains accurate, and the MIT-licensed
  repository is unchanged (pushed 2026-09-03). No update.
- **HarnessVLN v2** (`2609.15195`, v1 14 Sep → **v2 16 Sep**, cs.RO): abstract
  rewritten, affiliations added (Nanjing University, AGIBOT, Tsinghua), and a
  project page added at `agibot-harnessvln.netlify.app` (live); still **no
  code, weights, or data**, so the README's "treated literally as not open"
  statement stands. No update.
- **After the Party v2** (`2609.17274`, v1 15 Sep → **v2 16 Sep**,
  cs.SE/cs.AI/cs.CY/cs.SE): retitled "Growth, Governance, and Security Scanning
  in the OpenClaw Agent Skill Ecosystem" with revised text. The replication DOI
  `10.5281/zenodo.21469516` printed in the v2 HTML **still returns
  `{"status": 404, "message": "The persistent identifier is not registered."}`**
  from the Zenodo record API (verified 2026-09-17), and `zenodo.org/records/21469516`
  returns 404. The registry study remains a strong watch item, but its data and
  scripts are still not accessible.

### Candidates verified and rejected

- **FIERCE** (`2609.18651`, cs.RO): generalist-initialized RL refining compact
  specialists through a task-adaptive progress–failure evaluator, with the
  abstract claiming "Code, model weights, and data-restoration tools are
  released at `github.com/ar-mine/FIERCE`". The repository exists (created
  2026-09-15) but is **0 KB and contains only `README.md`** (GitHub contents
  API, 2026-09-17). A placeholder, not open. **Watch** for the real release.
- **ContrAgent / Symbolic Temporal Supervision of LLM Agents Using Contracts**
  (`2609.18128`, cs.AI/cs.LO): LTLf assume–guarantee contracts compiled to
  deterministic finite automata that serve both as an online action gate and an
  offline trace evaluator, with a contract library maintained independently of
  the agent — genuinely harness-shaped and the closest digital-agent miss of the
  window. The repository named in the HTML, `github.com/yfxiao16/ContrAgent`,
  returns **404** (verified 2026-09-17), so it is treated literally as not open.
  **Watch for the implementation.**
- **Causal-History Test-Time Scaling for Failure Recovery in Autoregressive
  World-Action Models** (`2609.18016`, cs.RO): training-free recovery as
  test-time scaling over causal histories — a progress-aware recovery trigger, a
  history-prefix recovery that rebuilds the causal KV state from a retained
  prefix anchored to the current physical state, and hypothesis verification
  across complete-history, recovered-prefix, and full-reset continuations.
  A clean recovery contract, but **no artifact links at all** on the record or
  in the HTML (verified 2026-09-17). **Watch.**
- **RAFAIL** (`2609.18324`, cs.RO): relationship-aware failure detection that
  focuses OOD detection on task-relevant entity relationships, learns point-cloud
  representations from VLM-annotated successful demonstrations without failure
  data, and reports 73.4% balanced accuracy across three real manipulation tasks.
  The only repository link in the HTML is a third-party UR impedance driver
  (`edgarwelteKIT/ur_impedance_driver_ros2`), not this paper's code. Excluded;
  the runtime failure-monitor line is already carried by FARM, ContactGuard,
  SAFECAST, and FailBench. **Watch.**
- **KINO** (`2609.18869`, cs.RO): keyframe interface between VLM planning and
  RL whole-body control, with a saliency-based keyframe sampling strategy
  lifting end-to-end success from 44% to 92% and evaluation on a Unitree G1.
  An interface contribution without an external runtime contract, and no
  artifacts located. Excluded.
- **Traverse / Scout** (`2609.17930`, cs.LG): 2,518 agent trajectories across
  software engineering, computer use, and science, 6,967 mistakes classified
  into 78 failure types, human-verified first-mistake localization, evidence
  that an outcome cannot reveal where a run went wrong (even the strongest of
  six frontier judges locates the first mistake in under a third of runs), and a
  trained 4B verifier that transfers across domains and improves test-time
  selection. The paper states "we release the benchmark, the training data and
  recipe, and the verifier", but **no repository or dataset URL appears in the
  paper or the HTML** (every outbound link is a reference or a LaTeX artifact,
  verified 2026-09-17). Would be a strong include with an endpoint. **Watch.**
- **PointZero** (`2609.19142`, cs.CV/cs.RO): 3D point-track completion as a
  robot-action-label-free pre-training objective, a 2.9M-frame synthetic dataset,
  and released checkpoints claimed in the abstract. The project page
  `pointzero-wm.github.io` links only author homepages — no code, dataset, or
  checkpoint endpoint (verified 2026-09-17). **Watch.**
- **Predictive Varanus** (`2609.17625`, cs.RO): a two-stage runtime-verification
  pipeline combining CSP conformance gating with predictive LTL, evaluated on a
  nuclear-store inspection rover; relevant to the Runtime/Safety line. The
  linked repository `AngeloFerrando/PredictiveVaranus` (171 MB, created
  2026-03-25, pushed 2026-04-16) **predates the paper by six months and asserts
  no license**, so its relationship to this paper's artifact is unverified.
  Excluded this window; **watch**.
- **RoboVAD** (`2609.17843`, cs.AI/cs.CV/cs.LG/cs.RO): large cross-domain
  benchmark for anomaly detection in robotic-arm manipulation videos, with a
  real release — Zenodo record `22754659` ("RoboVAD dataset", **CC-BY-4.0**,
  `RoboVAD.zip`, created 2026-09-14) verified live. Excluded under the standing
  perception/dataset policy: it is a video-anomaly-detection resource rather
  than a harness, recovery, safety, or agent-evaluation contract, and all
  methods remain below 70% micro-averaged frame-level AUC in the hardest setup.
  Recorded because the artifact is genuine and a future recovery-trigger entry
  may want it.
- **WholeBodyWAM** (`2609.18197`, cs.RO): a **name collision, not a
  duplicate ID** — `2609.16644` ("WholeBodyWAM: Generalizing Pre-trained
  World-Action Priors to Humanoid Loco-Manipulation via WBC-Grounded
  Coordination", Zhuo Li et al., v1 15 Sep) was excluded by the previous run,
  while `2609.18197` ("WholeBodyWAM: Learning Whole-Body World Action Models
  with Scalable Motion Priors", Zhang Bowei et al., v1 16 Sep) is a different
  author list and paper with a project page only. Excluded as a model paper.
- **Reflections on Trusting Trust, Revisited** (`2609.17817`, cs.AI/cs.CR):
  instantiates Thompson's compiler-poisoning argument against three
  self-modifying coding agents (Darwin Gödel Machine with experimental
  modifications, the Self-Improving Coding Agent, and Hyperagents), showing that
  poisoned benchmarks can drive self-evolved instructions that disable HTTPS
  certificate validation on neutral tasks and that contamination often persists
  through evolution against clean benchmarks. A substantial safety result for
  the self-improvement line, but there are no artifacts and the contribution is
  an attack demonstration rather than a harness or evaluation contract already
  covered by Auditing Harness Tampering and HookPry. Excluded this window;
  **watch** for a defense or release.
- **Real-Time EXPO-FT** (`2609.18207`, cs.LG/cs.RO): decouples slow action
  generation from fast reactive edits and RL-finetunes a real-time VLA policy
  (42%→97% average on four dynamic real-world tasks with ≤10 minutes of online
  data). A model/RL result with a website only; no code, weights, or data
  located. Excluded under the standing rule.
- **Project Kitchen / Game2Policy** (`2609.18650`, cs.RO): VR gamified
  robot-free data collection with embodiment-invariant affordance cues
  (+10.0 simulation / +18.3 real-robot points in the few-shot setting); the
  platform and code "will be released upon acceptance", so it is treated
  literally as not open. **Watch.**
- **Not All Layers Need Tuning** (`2609.18084`, cs.CV/cs.LG/cs.RO): diagnoses
  per-region VLA adaptation cost and allocates variable-rank LoRA under a
  parameter budget (median Spearman 0.91; matches full fine-tuning with 0.04% of
  trainable parameters on a physical xArm-7). An adaptation-allocation pipeline,
  not a harness or evaluation contract, and no artifacts located. Excluded.
- **VLA-ULAP** (`2609.18663`, cs.LG/cs.RO) and **rMuscle** (`2609.19104`,
  cs.AI/cs.RO): inference/serving optimizations for VLA policies (cloud/edge
  interleaving with a 7.4M-parameter local predictor; cross-execution
  muscle-memory caches). Both are policy-serving results with no artifacts
  located; the serving-contract line is already carried by Robion and
  Latency-Tolerant Cloud-Edge VLA. Excluded.
- **DeformSmith** (`2609.18620`, cs.RO): "physics harness"-guided generation of
  deformable assets for manipulation — the word *harness* here refers to a
  physics solver harness, not an agent runtime; project page only. Excluded.
- **Digital-agent safety, evaluation, and memory papers screened and set aside**
  as too far from a robot or general harness contract to justify an entry this
  window: `2609.18272` (independence-graded audit protocol), `2609.18820`
  (compositional policy violations — a good taxonomy of step-level compliance
  failing to compose, no artifacts), `2609.18304` (rollback-induced reflection,
  no artifacts), `2609.18849` (tool-progress signals to the serving system, no
  own artifacts), `2609.18298` (fuzz-tested reliability of agentic-generated
  programs), `2609.18864` (ASLEval privacy-exposure displacement), `2609.19101`
  (reward-hacking probes from internal representations), `2609.17865` (safety
  evidence seeking), `2609.17745` (REVERSAL-BENCH), `2609.17695` (GraphEcho),
  `2609.17688` (CapMem), `2609.17848` (SFT vs RL for tool calling),
  `2609.18445` (M-SQE multilingual skill quality), `2609.18460` (collective loss
  of control), `2609.19099` (Andromeda 2 autonomous formulation — domain),
  `2609.18540` (SVMemAgent video memory), `2609.17885` (ERPBench),
  `2609.17590` (evolutionary ensemble search), `2609.17842` (Lexara-RF),
  `2609.17631` (independently challengeable claims), `2609.17921` (collaborative
  memory for multi-agent VLM systems), `2606.11897` (Notes2Skills),
  `2609.18182` (WFM), `2609.17984` (TuiML), `2609.19144` (zeroth-order
  preference alignment), `2609.18769`, `2609.18805` (ProgramDistill),
  `2609.18909` (dual-view relational agent benchmarking), `2609.17613`,
  `2609.17601`, `2609.17619`.
- **Robot policy, world-model, and VLA model papers screened and excluded**
  under the standing rule that a model paper is not a harness paper without an
  external runtime, interface, or released artifact: `2609.18259` (M²Tok),
  `2609.18487` (ActionPiece), `2609.18374` (Decoupled Embodiment Model),
  `2609.18242` (ForceDelta-VLA), `2609.18243` (Acting in Meters), `2609.18232`
  (UMI-Bridge), `2609.18174` (TacBPM), `2609.18497` (TAO-Force), `2609.18164`
  (energy-regularized IL), `2609.18108` (drifting action heads for GR00T N1.7),
  `2609.18197`, `2609.18016`, `2609.18004` (AALT active imitation learning),
  `2609.18293` (function-preserving real-to-sim-to-real), `2609.18514`
  (ActiveScale), `2609.18930` (whole-body loco-manipulation), `2609.18763`
  (whole-body teleoperation), `2609.18869` (KINO), `2609.19142` (PointZero),
  `2609.18549` (DynoFluxBench), `2609.18651` (FIERCE), `2609.19137` (audio-force
  generation), `2609.18100` (sonar GAN evaluation), `2609.18628` (VIO
  benchmarking), plus the 242 older-ID replacement records not in scope.
- **Domain papers** excluded under the standing policy: driving, racing, and
  aerial (`2609.18623` FIVE-VLA, `2609.18442` risk-aware world modeling for
  automated driving, `2609.18451` VLM-MPPI aerial navigation, `2609.18326` UAV
  embodied intelligence, `2609.17728` RAF-VLA, `2609.18542` 4D radar
  preprocessing, `2609.18562` sim-to-real traffic scenes); medical, surgical, and
  clinical (`2609.18667`, `2609.18753`, `2609.18546`,
  `2609.18578`, `2609.19093`); perception, SLAM, odometry, mapping, and sensor
  datasets (`2609.18034`, `2609.18056`, `2609.18088`, `2609.18139`, `2609.18493`,
  `2609.18511`, `2609.18634`, `2609.18716`, `2609.18819`, `2609.18893`,
  `2609.18413`, `2609.18490`, `2609.18092`, `2609.18073`); and
  conventional control, estimation, planning, gait, and mechanism design
  (`2609.17946`, `2609.18050`, `2609.18070`, `2609.18167`, `2609.18169`,
  `2609.18191`, `2609.18359`, `2609.18395`, `2609.18482`, `2609.18487`,
  `2609.18700`, `2609.18813`, `2609.18881`, `2609.18910`, `2609.19080`,
  `2609.18117`, `2609.18119`, `2609.18153`, `2609.18193`, `2609.18358`,
  `2609.18504`, `2609.18685`, `2609.18732`, `2609.18789`, `2609.18581`,
  `2609.18738`, `2609.18752`, `2609.18776`, `2609.18245`, `2609.18051`).

## Operational notes

- **The export API was available but still intermittently rate-limited.**
  cs.RO returned `429 "Rate exceeded."` on the first attempt for the 20260916
  window and succeeded on a 20-second-spaced retry; every other request returned
  HTTP 200. The retry loop stays in the pipeline, and OAI-PMH remains the
  authoritative path for announcement membership (518 of this block's 898 IDs
  are invisible to the `submittedDate` window).
- **OAI set names are `cs:cs:<CAT>` with the bare category code** (`cs:cs:RO`,
  `cs:cs:AI`, …). Building the set name from the dotted category (`cs:cs:cs.RO`)
  returns `badArgument: Set does not exist` for every request; that mistake cost
  one harvest cycle this run and is recorded so later agents do not repeat it.
  With the corrected names, each category returned a single complete page
  (no resumption tokens).
- **The block was unusually large** (898 unique IDs versus 779 the previous day)
  because it absorbed 164 held/cross-listed submissions from 09-15 and 119 older
  records, including 248 pre-`2609.` IDs. This is why the export-API subset
  check (380 IDs) understates the block by more than half.
- **GitHub REST API rate limits did not bind this run** (12 repository lookups
  plus 10 contents listings all returned HTTP 200); `git ls-remote` was not
  needed as a fallback.
- **`git push` over SSH failed with the documented OpenSSH error**
  (`Bad owner or permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf`)
  on the first `git fetch origin`; every subsequent network Git operation used
  `GIT_SSH_COMMAND='ssh -F /dev/null'` successfully.
- **`.scratch/arxiv-2026-09-17/`** (untracked, deliberately not staged) holds the
  harvest XML, parsed `oai-records.json`/`records-20260916.json`, the export-API
  responses, screening/dossier/diff scripts, the fetched `abs`/HTML pages,
  version histories, and the artifact-check output. `/tmp` is **not** persistent
  between shell invocations on this host, so all state lives under `.scratch/`.
- The working branch was `main` throughout. Only task-owned files (`README.md`
  and this record) were staged; the unrelated user changes
  (`docs/reference-architecture.md` modified; `docs/ring-harness.png` and
  `handoff.md` untracked) were left untouched.

## Validation performed

- `git diff --check` clean.
- Markdown structure re-validated by heading enumeration after editing: all 39
  `##`/`###` headings are present and in the original order, and the seven new
  entries were confirmed by script to sit under `### Agentic Robot and VLA
  Harnesses` (×3), `### General Harness Design and Self-Improvement` (×3), and
  `### Agent and Embodied Reasoning` (×1).
- Every newly added URL was fetched: **17 distinct links** (7 arXiv `abs` pages,
  6 GitHub repositories, 3 project sites, 1 Hugging Face collection) returned
  **HTTP 200** on 2026-09-17. The links that do **not** resolve are documented as
  such rather than presented as live: `yfxiao16/ContrAgent` (404),
  `ar-mine/FIERCE` (repository exists but contains only a README),
  `zenodo.org/api/records/21469516` (404, unregistered), and
  `zenodo.org/records/21469516` (404).
- README internal consistency: the `last verified` badge and the body
  `Last verified:` line were both moved to **2026-09-17**, matching this
  record's date. Per-entry "verified" dates elsewhere were left at their
  original values, since they record when that entry was last checked rather
  than the list's verification date.
- Cross-file consistency: all names and arXiv IDs in the README changes were
  grepped against `README.md`, `docs/`, and `sources/` before editing. No
  duplicates: WetRobo, GPT-Policy, AeroWeaver, CHASE/Bad Genius, EvoSkill-GUI,
  ScienceIDE, and AutoTuneBench had zero prior mentions. The revision checks
  confirmed the existing LoopHarness, EvoUndo, PACT, HarnessVLN, and ACT-forensic
  entries are the only places those IDs appear, and each was updated or left
  unchanged deliberately, as recorded above.
- Categories and artifact statuses in every new entry were taken from primary
  sources (arXiv record, `abs` submission history, `html/<id>v<n>`, official
  project page, GitHub REST API, Zenodo record API, Hugging Face collection) on
  2026-09-17, and each entry states the author-reported versus verified
  distinction, the simulation-only status where applicable, and the literal
  artifact status (including the two source-available-but-unlicensed code
  releases and the "no license asserted" repositories).
