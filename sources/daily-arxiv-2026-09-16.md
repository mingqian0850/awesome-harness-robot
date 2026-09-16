# Daily arXiv scan — 2026-09-16

## Scope

- **Interval:** everything announced after the 2026-09-15 run's cutoff. That run
  (started ~04:00 UTC on Tuesday 2026-09-15) screened the block whose OAI header
  datestamp is **2026-09-15** — 2,123 set memberships / 1,563 unique base IDs,
  newest base ID `2609.15989` — covering the Saturday, Sunday, and Monday
  submission blocks plus newly announced backlog. This run started ~04:00 UTC on
  Wednesday 2026-09-16 and covers everything announced since.
- **A new announcement block landed.** Datestamp **2026-09-16**:
  **1,061 set memberships = 779 unique base IDs** across the five target
  categories. Newest base ID in the block **`2609.17527`**; the ID frontier sits
  at **`2609.17531`** (`2609.17532` returns HTTP 404, while `2609.17528`–
  `2609.17531` resolve but are not in the announced block — ID-existence bisect
  on `arxiv.org/abs/<id>`, 2026-09-16).
- **Composition of the new block (779 unique IDs), by the submission date of the
  announced version** (the OAI metadata `<created>` field, which carries the
  latest announced version's date — semantics re-confirmed in the previous run):
  - **543 records carry 2026-09-15** — the Tuesday submission block, which is
    what arXiv releases on Wednesday.
  - **180 carry 2026-09-11 through 2026-09-14** (147 on 09-14, 23 on 09-13,
    8 on 09-11, 2 on 09-12) — held and cross-listed new submissions whose ID was
    assigned at announcement.
  - **56 carry dates before 2026-09-11**, reaching back to 2020 — replacement
    versions (re-announcements) and long-held submissions.
  - By category: cs.AI 311, cs.LG 292, cs.CV 188, cs.CL 161, **cs.RO 109**.
- **Continuity check against the previous run.** The previous run counted 2,123
  memberships / 1,563 unique IDs for datestamp 2026-09-15; this harvest sees
  2,077 memberships / **1,526** unique IDs for the same datestamp. No base ID
  carries both datestamps (OAI records a single datestamp that follows the latest
  announcement), so the net **−37 IDs** is the same re-announcement churn the
  previous run measured as a 13-ID difference in the other direction: a
  replacement announced on 09-16 moves out of the 09-15 group. Seven such
  re-announcements were identified and read as revisions (below).
- **"Unchanged batch" re-screen:** not applicable in the required sense — a
  genuinely new block landed, and none of its 779 IDs is a repeat of the previous
  run's batch except the re-announced replacements. The **entire** 779-record
  block was screened, including the 236 records with pre-09-15 dates that no
  earlier new-submission window covered.
- **Revisions in scope.** Replacement versions the previous screen could not see
  were version-diffed from the `abs` submission history before a verdict:
  **MPCoT v3** (`2606.06245`, 2026-09-15), **MANGO v3** (`2606.24815`,
  2026-09-15), **Mem-World v3** (`2606.18960`, 2026-09-15), **ANCHOR v3**
  (`2606.06114`, 2026-09-15), **Shared Selective Persistent Memory v2**
  (`2607.09493`, 2026-09-15), **DynSTEER v2** (`2609.14637`, 2026-09-15), and
  **MessyMem v2** (`2609.15976`, 2026-09-15). Older-ID replacements in the block
  (`1304.3111`, `1906.07927`, `2403.00270`, `2404.01673`, `2408.04910`,
  `2501.02858`, `2507.12441`, `2509.18445`, `2510.10059`, `2510.23176`,
  `2605.03788`, `2605.28136`, and the rest of the 56) were screened by title and
  category and none is in the repository's scope.
- **Watch-list continuity:** none of the six items the 2026-09-15 record placed
  on watch (`2609.15213` X-WBC, `2609.13889` PMPA, `2609.13353` SkillAtlas,
  `2609.13679` REAL-I Challenge, `2609.13335` MetaTool-ROS, `2609.13231`
  ShieldVLA, `2609.15098` LG-VLN) re-announced in this block, so none of their
  artifact statuses changed.

## Method

- **Primary path — the arXiv export API over HTTPS
  (`https://export.arxiv.org/api/query`) was available again this run**, after
  two consecutive runs of blanket `HTTP 429 "Rate exceeded."` It was queried with
  `submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` per category for
  20260915 and 20260916 across cs.RO, cs.AI, cs.CL, cs.CV, cs.LG.
  - 20260915 returned **HTTP 200**: cs.RO 58, cs.AI 127, cs.CL 64, cs.CV 97,
    cs.LG 99 entries. cs.RO and cs.AI returned `429` on the first attempt and
    succeeded on a spaced retry — rate limiting is intermittent rather than
    sustained, which is why the retry loop is kept in the pipeline.
  - 20260916 returned HTTP 200 with **0 entries** for all five categories: the
    09-16 submission block has not been announced yet, which is the expected
    result for a run that starts at 04:00 UTC.
- **Block harvest — OAI-PMH** (`https://oaipmh.arxiv.org/oai`,
  `verb=ListRecords&metadataPrefix=arXiv&set=cs:cs:<CAT>&from=2026-09-15&until=2026-09-16`)
  for the five categories; every response was a single complete page with no
  resumption token outstanding. Totals: cs.RO 978 KB, cs.AI 3.16 MB, cs.CL
  1.51 MB, cs.CV 1.81 MB, cs.LG 2.69 MB = **3,138 memberships = 2,305 unique
  base IDs** across both datestamps. This is the path that establishes
  *announcement* membership, which the export API's `submittedDate` cannot: it
  also captures replacement versions whose original submission date is far
  outside the window.
- **Screening:** keyword concepts (harness, scaffold, orchestration, runtime,
  agent loop, tool use, multi-agent, self-improvement, co-evolution, continual
  learning, robot agent, embodied agent, VLA, action expert, world model,
  world-action, latent action, code-as-policy, skill discovery, memory, recovery,
  rollback, evaluation, benchmark, audit, safety, security, attack, verification,
  guardrail, manipulation, teleoperation) plus a full title-level sweep of the
  block. 176 records entered the strong pool; all 109 cs.RO records were read at
  title level and every candidate at abstract level from the harvested metadata.
- **Verification:** direct `arxiv.org/abs/<id>` and `arxiv.org/html/<id>v<n>`
  fetches for abstracts, comments, journal references, and submission histories;
  live artifact checks through the GitHub REST API (repository metadata,
  license, size, contents listings), the Hugging Face Hub, the Zenodo API,
  and the official project pages. **26 candidate pages and 16 artifact
  endpoints were fetched.**

## Included (README updates)

Eight new entries and four Current Landscape bullets. Nothing was updated in
place: the seven revisions in this block either did not qualify or, where they
did (MANGO v3, MessyMem v2), still fail the artifact bar and remain on watch.

### FluxVLA Engine: A One-Stop VLA Engineering Platform for Embodied Intelligence

- **`2609.17210`** (cs.AI/cs.RO, v1, 15 Sep 2026). Category: **Agentic Robot/VLA
  Harness**; also General Harness Methodology.
- The contribution is the stack, not a policy: datasets, VLM and world-model
  backbones, action heads, reward-/advantage-weighted learning, distributed
  training, simulation evaluation, optimized inference, and robot operators are
  composed through standardized, configuration-driven interfaces so one
  data-to-deployment workflow covers offline learning, simulation validation,
  online correction, and real-robot execution. Deployment is explicit — Real-Time
  Chunking with accelerated inference backends, lightweight remote GPU serving,
  configurable trajectory post-processing — and the human-in-the-loop path
  (rollout, takeover, correction collection, reward annotation) is model-decoupled.
- **Artifact status verified open (2026-09-16):** `FluxVLA/FluxVLA` is
  **Apache-2.0**, created 2026-04-01, **pushed 2026-09-16T03:07Z**, 679 stars,
  35 MB, topics include `vision-language-action-model` and `world-action-model`;
  the documentation site `fluxvla.limxdynamics.com` and the Hugging Face
  organization `limxdynamics/FluxVLAEngine` (fine-tuned SmolVLA LIBERO
  checkpoints) both return HTTP 200. The repository README reports an 84.7
  LIBERO-average across the four suites.
- Results are author-reported and simulation-led; the real-robot paths are
  described rather than independently reproduced.

### Agentic Societies Need a Social Harness

- **`2609.17527`** (cs.AI/cs.MA/cs.NI, v1, 15 Sep 2026; Tapan Chugh, Vidushi
  Singh, Krish Jain, Arvind Krishnamurthy, Ratul Mahajan). Category: **General
  Harness Methodology**.
- Names the half of the harness concept that current stacks leave unowned:
  alongside each agent's **personal harness** (private context, communication
  with its principal) there is no harness for inter-agent interaction across
  trust boundaries. Experiments show honest, competent agents already fail to
  reach satisfactory outcomes with existing harnesses and messaging primitives,
  while faulty or malicious agents stall collaboration, steer outcomes, and
  pursue their own goals by exploiting communication ("speech").
- The proposed layered architecture separates three jobs a single guardrail
  cannot cover at once: prevent classes of failures outright, let agents detect
  invalid messages at runtime, and support post-facto investigation and
  consequences. This is the same ownership boundary the list tracks for tools,
  state, timing, permissions, and verification, applied one level up.
- **Artifact status verified (2026-09-16):** the companion repository
  `social-harness/social-harness-paper` ("Companion repository for Agentic
  Societies Need a Social Harness") is live and contains the run records and
  results — `index.html`, `index.json` (391 KB), `results.json` (270 KB),
  `runs/`, `trace-review/`, `assets/` — but **asserts no license**.
- Evidence is from small coordination studies (meeting scheduling with 1–7
  agents plus adversarial and channel-contention variants): failure modes
  demonstrated, not measured at scale. Digital agents, no robot experiment;
  results author-reported.

### The Latent That Never Was: A Forensic Re-run of the CVAE Ablation in ACT

- **`2609.16745`** (cs.LG/cs.RO, v1, 15 Sep 2026). Category:
  **Evaluation/Safety** (verification), placed under Manipulation and VLA.
- Re-measures one of the most repeated numbers in robot imitation learning. The
  original ACT paper reported encoder removal dropping mean success from 35% to
  2%; this re-run executes the ablation in the original code and the published
  drop **does not reappear**, with smaller gains or losses remaining uncertain.
  The diagnostic finding is that varying **training length** and the
  **checkpoint-selection rule** is enough to reverse which policy scores higher,
  so a success-rate comparison that does not fix those protocol choices is not
  isolating an architectural effect. The paper also reports that the sampled
  latent provides little reconstruction benefit at every tested nonzero weight of
  the latent-information penalty — consistent with inference-time ACT setting it
  to zero — and that skipping the encoder raises training throughput in both
  implementations timed.
- **Artifact status is literal and negative:** the paper states it releases code,
  evaluation tools, and results, but `aida-ugent/act-cvae-forensics` (created
  2026-09-14) **is empty** — the GitHub contents API returns "This repository is
  empty" — and `bokang-ugent/nanoACT` (the second link in the HTML) returns
  **404** (verified 2026-09-16). The release is a placeholder, so it is not
  described as open.
- Simulation evidence; the re-run is author-reported with no independent
  reproduction. Included because the audit result is substantial and is a
  measurement contract for the manipulation-evaluation line, not because an
  artifact shipped.

### Coding Agents Have Converged: Why the SWE-bench Leaderboard Can No Longer Order Its Top Entries

- **`2609.17394`** (cs.AI/cs.SE, v1, 15 Sep 2026; ADMA 2026 camera-ready).
  Category: **Evaluation/Safety**. Placed under Agent and Embodied Reasoning.
- Audits 254 published SWE-bench submissions across four splits without running
  a model. The top two Verified entries resolve exactly the same 396/500
  instances; the top ten share 285 successes and 51 failures; frontier solution
  sets have median nesting 0.935 against a score-implied baseline of 0.774. Exact
  paired McNemar tests separate **none** of the 29 adjacent Verified top-thirty
  pairs at α = 0.05 (14 of 23 separate on the larger Test split). The harness term
  is also quantified: within-model scaffold ranges reach 29.8 percentage points
  against the 8.8-point spread of the top thirty, with six of nine cell-mean
  interaction tests surviving Holm correction in a design the authors state is
  observational and does not identify causal scaffold effects.
- Constructive output: a released partition plus a five-step audit protocol
  (profile shared outcomes, test paired differences, report grouping sensitivity,
  estimate the instance budget a claimed gap would need) and the recommendation
  to report comparison-set-specific resolution and model–scaffold provenance.
- **Artifact status verified (2026-09-16):** `Adkid-Zephyr/resolution-audit` is
  live with `README.md`, `analysis/`, `data/`, `release/`, `tools/`, and
  `fetch_data.sh` (created 2026-08-01, pushed 2026-08-13), but **asserts no
  license** and its repository description still reads "Reproduction package for
  an anonymous double-blind submission".
- Digital coding agents, no robot experiment; results author-reported. Included
  because the argument transfers directly to every robot leaderboard this list
  cites.

### RobResilience: Implementing and Evaluating a Resilience Framework for Cyber-Physical Embodied Systems

- **`2609.17349`** (cs.CR/cs.RO/cs.SY/eess.SY, v1, 15 Sep 2026). Category:
  **Evaluation/Safety**; also recovery/runtime. Placed under Safety.
- Converts "the IDS raised an alert" into a runtime decision procedure. Detection
  alone leaves *graceful-failure paralysis*: the system cannot distinguish a safe
  degraded state from a catastrophic hazard. The framework evaluates three
  predicates continuously over the compromised device set derived from IDS
  confidence scores — tolerable disruption δ, tolerable degradation γ, and
  mitigation feasibility μ — and triggers available mitigations when resilience is
  lost. The transferable contract is that decomposition: detection answers *is
  something wrong*, δ/γ answer *are we still inside safe operational bounds*, and
  μ answers *is recovery even possible*.
- Implementation: a Webots PR2 on ROS 2, evaluated through **eight attack
  scenarios constructed to cover all combinations of the three-predicate state
  space** while varying attack targets, degradation rates, and mitigation
  availability. The authors report runtime behaviour consistent with the formal
  definitions rather than a task-performance win.
- **Artifact status verified (2026-09-16):** `mahyamkashani/RobResilience` is live
  (README, `ros2_ws`, `my_webot_project`, `run_scenario.sh`, `assets/`,
  `auto_runs/`; created 2026-04-09, pushed 2026-09-04) but **asserts no license**.
  Peer-reviewed venue from the record's comment field (CPSIoTSec '26, co-located
  with ACM CCS 2026); the DOI printed in the HTML (`10.1145/3847353.3847504`)
  currently returns **404** and is therefore not cited as a link.
- Simulation-only, no robot experiment, no independent reproduction.

### BLINDSPOT: A Benchmark for Safety and Refusal Calibration in Long-Horizon Tool-Using Agents

- **`2609.16305`** (cs.AI/cs.CE/cs.CL/cs.LG/cs.MA, v1, 14 Sep 2026). Category:
  **Evaluation/Safety**. Placed under Safety.
- Moves agent-safety evaluation from "did it refuse this request" to "did it hold
  the correct boundary across the whole interaction": the unit under test is the
  complete user–agent–environment trajectory with stateful tool execution,
  persistent state, evolving authorization, and adversarial content arriving
  through retrieved artifacts. Every trajectory gets one of five outcomes — Safe
  Completion, Correct Refusal, Unsafe Completion, Over-Refusal, Indeterminate —
  so over-refusal and unresolved runs are scored instead of collapsing into a
  binary.
- The abstract reports 22 attack families and 35 scenarios across seven domains,
  more than 2,500 trajectories at 14.7 turns average, 13 evaluated proprietary
  and open-weight models, and eight metrics covering unsafe completion,
  appropriate refusal, benign utility, over-refusal, repeated-run robustness, and
  post-refusal failure; failures emerge only after several initially safe turns.
  The framework is a live-simulation benchmark rather than a fixed attack set —
  attacks, scenarios, tools, policies, domains, and agent configurations can be
  added without redesigning the pipeline.
- **Artifact status verified open (2026-09-16):** `sadia-sigma-lab/BLINDSPOT` is
  **MIT-licensed** (5 MB, created and pushed 2026-09-14) and ships the dataset,
  scenario definitions, schemas, prompts, src, tests, and generators. Its own
  README reports a **larger frozen instantiation than the abstract**: 3,024
  trajectories, 65 scenarios, 22 attack families, splits 2,122/451/451. The
  discrepancy is recorded rather than reconciled — the paper's numbers are cited
  as the paper's, the repository's as the repository's.
- The Zenodo DOI that appears in the paper's related-work HTML
  (`10.5281/zenodo.19908321`) is **not** a BLINDSPOT dataset: the Zenodo API
  resolves it to an unrelated multi-agent-security literature review
  (2026-04-30, CC-BY-4.0). No separate BLINDSPOT dataset DOI was claimed.
- Digital agents, no robot experiment; results author-reported.

### World-Action Models for Robot Learning and Control: A Survey

- **`2609.16074`** (cs.CV/cs.RO, v1, 13 Sep 2026). Category: **Robot
  Foundation/World Model** (survey). Placed under Surveys and Reading Lists.
- Robotics-oriented survey of models coupling future world prediction with
  executable action generation, and the reference that separates WAMs from
  conventional world models, model-based RL, action-conditioned video generation,
  and reactive VLA policies. Taxonomy along representations, transition
  modelling, action interfaces, architectures, training pipelines, data
  modalities, and scaling strategies; applications across manipulation,
  navigation, and driving; a dedicated summary of datasets, benchmarks, metrics,
  and protocols; and an open-problem list (action alignment, world–action
  factorization, spatial and multi-view consistency, long-horizon memory, neural
  simulation for closed-loop policy learning, efficient inference).
- **Project page verified live (2026-09-16):**
  `rcl-robotics.github.io/Awesome-World-Action-Models` (MBZUAI-led author list,
  19 pages) hosts a paper library, research map, and reading reports. No code or
  dataset is released, as expected for a survey.

### World Models for Embodied Intelligence: From Plausible to Controllable to Actionable

- **`2609.16697`** (cs.AI/cs.RO, v1, 15 Sep 2026). Category: **Robot
  Foundation/World Model** (survey). Placed under Surveys and Reading Lists.
- A decision-centered survey answering a different question from the WAM survey:
  not which architectures exist, but **which predictive capabilities improve
  behaviour**. Three capability levels — *Plausible* (preserves task-relevant
  temporal, geometric, or physical structure), *Controllable* (additionally
  predicts how interventions change it), *Actionable* (translates predictions
  into measurable gains in planning, action, learning, evaluation, verification,
  recovery, or data selection) — crossed with geometry, physics, and action
  grounding and the four improvement loops (data, rewards, policies, the model).
  Reframing the evaluation target from visual plausibility to behaviour change is
  the same move as the verifier-credibility and success-judging entries already
  curated.
- **Project page verified live (2026-09-16):**
  `3dagentworld.github.io/EmbodiedWM/` publishes the framework, the 3 × 4 matrix,
  a landscape, a paper library, and the challenge list. No code or dataset is
  released.

### Current Landscape additions (4 bullets)

- VLA engineering consolidating into platforms (FluxVLA Engine).
- The harness idea extending from one agent to the interaction between agents
  (Social Harness).
- Runtime safety acquiring predicate-checked contracts on physical systems
  (RobResilience + BLINDSPOT).
- Published results being re-measured before they are extended (ACT forensic
  re-run + SWE-bench resolution audit).

## Rejected / watch list

### Revisions read in this block, none qualifying

- **MANGO v3** (`2606.24815`, v1 2026-06-23 → v2 2026-09-14 → **v3 2026-09-15**,
  cs.RO/cs.SE): multi-agent automatic test-oracle generation for VLA models
  (LIBERO_10, RoboCasa Humanoid Tabletop). Previously screened and excluded. The
  v3 HTML still says the replication package "will be made available upon
  acceptance", and no repository appears on the record or in the HTML (verified
  2026-09-16). Treated literally as not open. **Watch**: would qualify if the
  generated oracle libraries and implementation are released.
- **MessyMem v2** (`2609.15976`, v1 2026-09-14 → **v2 2026-09-15**, cs.RO, CoRL
  2026): persistent 3D-scene-graph memory for mobile manipulation, evaluated in a
  continuous 25-task simulation (80.0% task progress, +14.8 over the strongest
  ablation) and on a real mobile manipulator. The previous run excluded it for
  "project page but no code link located"; the v2 was re-checked and the project
  page `messymem.github.io` still shows **"Code soon" and "Video soon"** with
  placeholder `TODO-your-org` links in the page source (verified 2026-09-16).
  **Watch** for the code release.
- **Mem-World v3** (`2606.18960`, v1 2026-06-17 → **v3 2026-09-15**, cs.CV/cs.RO,
  CoRL 2026): memory-augmented multi-view action-conditioned world model with a
  4D wrist-view-centered surfel memory; reported +14.5% Pearson correlation with
  real-world policy performance over Ctrl-World and 58%→72% success on
  long-horizon tasks when used for synthetic data generation. A world-model paper
  without an external runtime or interface; no code, weights, or project link
  located on the record or in the HTML. Excluded under the standing rule.
- **MPCoT v3** (`2606.06245`, v1 2026-06-04 → v2 2026-08-20 → **v3 2026-09-15**,
  cs.AI/cs.RO): reward-guided multi-path latent reasoning for test-time-scalable
  VLA (LIBERO, CALVIN; 71.3%→82.0% on five real ALOHA Mini tasks over
  OpenVLA-OFT). The code repository `EDGSCOUT/MPCoT` is live, but the
  contribution is a policy architecture — it preserves the action interface
  rather than contributing an external runtime, interface, or evaluation
  contract. Excluded under the standing rule that a model paper is not a harness
  paper.
- **ANCHOR v3** (`2606.06114`, v1 2026-06-04 → **v3 2026-09-15**, cs.AI):
  external LLM-driven supervisory module retrofitted into two open-source
  self-evolving agent frameworks, improving safety while holding core capability.
  Relevant to the self-evolution-supervision line, but no artifacts located.
  Excluded this window (the line is already carried by Self-Harness, Harness
  Continual Learning, EvoUndo, and Auditing Harness Tampering).
- **Shared Selective Persistent Memory v2** (`2607.09493`, v1 2026-07-10 →
  **v2 2026-09-15**, cs.AI/cs.MA/cs.SE): retains task specs, data schemas, tool
  configurations, and output constraints across sessions while discarding
  session-specific traces, with a controlled replication (0/12 → 12/12 trials at
  essentially equal token cost versus 8/12 for full history, Bonferroni-corrected
  McNemar). A well-measured digital memory architecture with no artifacts
  located; the memory line is already carried by Recuris, DreamBench-SWE, and
  the harness-memory entries. Excluded for this window.
- **DynSTEER v2** (`2609.14637`, v1 2026-09-13 → **v2 2026-09-15**, cs.AI):
  stage-wise dynamic trajectory evaluation with a path-tolerant milestone graph
  (85.2% discriminability improvement, 45.41% saved execution steps on failed
  rollouts). No HTML version and no artifacts located (the v1 HTML returns 404).
  Excluded as incremental against the trajectory-evaluation line already curated
  (DynSTEER's granularity argument is close to CatchBench and ParaRecover).

### Candidates verified and rejected

- **The Robot Data Factory** (`2609.16705`, cs.RO, v1): mission-driven
  infrastructure and methodology for continuously generating, validating, and
  reusing robot experience, with a
  mission–task–skill–episode–dataset–benchmark–capability hierarchy and scaling
  laws connecting fleet size, sensor rates, storage, tokenization, training
  compute, and latency. Substantial as a position/infrastructure paper, but it
  fails every clause of this repository's scope policy: **no code, no dataset, no
  benchmark, and no public artifact**. The paper itself states that "internal
  operational readiness, public access to services, and versioned dataset or code
  releases are distinct milestones", and the announced project site
  `agentic-robotics-lab.github.io/robot-data-factory` redirects to a GitHub
  sign-in page (verified 2026-09-16) rather than serving content. Excluded;
  **watch** the first public mission-based dataset/benchmark release.
- **Intrinsic Robot Rewarding** (`2609.17115`, cs.LG/cs.RO): position paper
  reusing a frozen VLA's visual encoder and demonstration endpoints as a reward
  and outcome-evaluation mechanism. TRL-4 COMAU Racer 3 demonstrator, no
  artifacts located, explicitly "the next research step". Excluded (position, no
  artifact, no evaluation contract).
- **EchoPath** (`2609.16635`, cs.AI): model-agnostic harness converting
  artifact-validated GUI trajectories into parameter-controlled callable
  memories with image-based target re-aiming (>90% median token reduction, ~60%
  median time reduction) — genuinely harness-shaped and the closest
  digital-agent miss of the window. The repository named in the paper's HTML,
  `github.com/JackZhao1998/EchoPath`, returns **404** (verified 2026-09-16), so
  it is treated literally as not open. **Watch for the implementation.**
- **After the Party: Governing What a Viral Agent-Skill Ecosystem Left Behind**
  (`2609.17274`, cs.AI/cs.CY/cs.SE, APSEC 2026): measures the OpenClaw/ClawHub
  skill-registry wave — the observable stock nearly doubled in 91 days, the top
  10% of skills took 46.93% of downloads, 77.86% have zero stars and zero
  comments, 85.06% of readable skills carry privilege evidence, and three
  security scanners disagreed on 23,702 of 61,990 commonly covered skills with
  post-adjudication weighted sensitivity of 21.67%–61.06%. Directly relevant to
  the skill-safety line (MaliciousSkillBench, SkillMisevo, GitSkills,
  Defense-as-Skill), and the paper states an anonymized replication package of
  all scripts and data. **However, the cited DOI `10.5281/zenodo.21469516`
  returns HTTP 404 from the Zenodo API — "the persistent identifier is not
  registered" — and from `doi.org`** (verified 2026-09-16), so the data and
  scripts are not accessible. **Watch for the replication package to be
  registered**; it would be a strong include.
- **XPACE** (`2609.17372`, cs.RO): joint world-and-action model trained on
  heterogeneous human/robot experience that uses its own simulator to synthesize
  deviation-recovery trajectories (XPENG IRON humanoid). Recovery-augmentation is
  a good idea, but no artifacts were located and the contribution is a model
  rather than a runtime contract. Excluded; **watch**.
- **ModAR: Modality-Autoregressive World-Action Models** (`2609.17524`, cs.RO):
  systematically studies which future modalities a WAM should predict (point
  tracks, DINO features, and depth help; extra RGB does not) and reports 75% vs
  72% over a video-model-initialized baseline at ~20× fewer training FLOPs.
  Useful negative/ablation results, but a model paper with a project page only.
  Excluded.
- **WholeBodyWAM** (`2609.16644`, cs.RO): generalizes pre-trained world-action
  priors to humanoid loco-manipulation (91.9% simulation success, +0.23
  real-world OOD task progress, 70% variance reduction across whole-body
  controllers). Project page only; model paper. Excluded.
- **Residual Fault Adaptation** (`2609.17404`, cs.RO): teacher-anchored residual
  policy for hidden command-channel joint faults with fault-injection domain
  randomization and zero-shot real-robot deployment. Controller-level fault
  adaptation with no artifacts located; recovery is a harness concern here only
  if it exposes a runtime contract. Excluded.
- **Learning Options for Compositional Motor Control with Adapter Banks**
  (`2609.17042`, cs.LG/cs.RO/q-bio.NC): low-rank residual adapters over a shared
  recurrent core with a frozen high-level option policy, in closed-loop
  biomechanical control. Skill-discovery framing but a control architecture with
  no artifacts. Excluded.
- **ManiSkillFormer** (`2609.16331`, cs.RO): demonstration-free compositional
  manipulation via LLM-generated task-conditioned geometric contracts and a
  reusable motion-template library (88.24% pick-and-place over 8 categories).
  Interestingly skill-library-shaped, but no artifacts located. Excluded.
- **DriveMCP** (`2609.17247`, cs.RO): agentic ADAS framework with a stateful
  orchestration layer exposing Rules, Weather, and MCP-CAN servers behind an
  RSS-inspired speak-versus-act guardrail — a clean MCP-to-physical-agent
  harness pattern, but the standing policy excludes driving-domain work unless it
  introduces a reusable contract; the MCP-expert-plus-arbiter pattern is already
  carried by DroneServer and Agentic Harnesses. Excluded as domain + incremental.
- **Auto-HSI** (`2609.16346`, cs.AI/cs.HC/cs.MA/cs.RO): LLM-generated
  personalized human-swarm interfaces from natural language and gesture
  demonstrations, tested with real operators over 50 simulated robots and
  demonstrated on real robots. Code-generation-as-policy for a swarm; no
  artifacts located and the domain (human-swarm interaction interfaces) sits
  outside the curated harness scope. Excluded.
- **VLM-as-Probabilistic-Grounder** (`2609.16884`, cs.AI/cs.RO, NeuS 2026):
  represents VLM predicate groundings as a distribution over symbolic states to
  plan in belief space. Simulated household settings; no artifacts located.
  Excluded.
- **LiDAR simulation-fidelity diagnostics** (`2609.16378`, cs.CV/cs.RO): a
  graph-spectral metric `r_λ` for structural fidelity of simulated LiDAR,
  evaluated on 50 paired real/CARLA scans. A digital-twin diagnostic, but for
  ADAS/autonomous driving — excluded under the standing domain policy.
- **Accepted-but-not-artifact-bearing safety and evaluation work screened and
  excluded:** `2609.16461` (protocol-preserving context trimming — study, no
  artifacts), `2608.24794` (CAFE co-evolving feedback — no artifacts),
  `2609.16098` (universal defenses for tool-integrated agents — code live but
  incremental against StepGuard, ClawSentry, SARA, and OBPE), `2609.16268`
  (spurious tool use in RL — a good measurement result on shortcut tool
  selection, but no artifacts and the tool-reward line is already dense),
  `2609.16302` (assurance envelopes for coding agents — digital, no artifacts),
  `2609.16730` (LSREP + ICE v2 — live repository and tagged release at
  `Deepnar/ice`, but a conversational-memory evaluation whose reported negative
  result is close to DreamBench-SWE and the memory-hygiene entries),
  `2609.17300` (Machine Zygote — artificial-life framing), `2609.17414`
  (SlotDiT), `2609.16056` (action preconditions in neuro-symbolic RL),
  `2609.16057` (OmniHarness — visual generation despite the name),
  `2609.16075` (AssemblyGrid v1 — multi-robot production benchmark whose own
  artifacts are promised "when the paper is online"), `2609.16487` (skill-based
  agentic evaluation for data-science tasks), `2609.16313` (cognitive admission
  control), `2609.16936` (RepoAtlas), `2609.17296` (conformal policy learning —
  a statistics/policy-learning result, not a machine-learning-systems harness).
- **Robot policy, world-model, and VLA model papers screened and excluded** under
  the standing rule that a model paper is not a harness paper without an external
  runtime, interface, or released artifact: `2510.23176` (TARC), `2512.15020`
  (ISS Policy), `2602.08776` (I/O design for contact-rich visuomotor learning),
  `2603.07875` (foundation/small model coordination), `2603.23983` (SafeFlow),
  `2604.22551` (QDTraj), `2609.15570` (DIDO),
  `2609.16503` (dense-to-MoE VLA), `2609.16504` (UniDex-ViTac), `2609.16586`
  (ProxiDex), `2609.16641` (SAVLA), `2609.16683` (Weave), `2609.16696` (IL-ACT),
  `2609.16815` (visual embodiment dependence), `2609.16864` (TEMPO),
  `2609.17021` (sensVLA), `2609.17035` (SWIM), `2609.17099` (GeoLAM),
  `2609.17115`, `2609.17172` (fingers as legs), `2609.17187` (Fleet-To-Lab),
  `2609.17372`, `2609.17414`, `2609.17484` (motion-prior regularization),
  `2609.17524`, plus the 56 older-ID replacement records.
- **Domain papers** excluded under the standing policy: driving, racing, and
  aerial (`2501.02858`, `2605.03788`, `2605.28136`, `2607.00736`,
  `2608.20948`, `2609.12609`, `2609.16629`, `2609.16724`,
  `2609.17147`, `2609.17198`, `2609.17247`, `2609.17265`, `2609.17292`,
  `2609.16378`); medical, surgical, and clinical (`2604.23696`, `2609.16186`,
  `2609.16031`, `2609.16035`, `2609.16036`); perception, SLAM, odometry, and
  sensor datasets (`2603.26740`, `2609.09012`, `2609.16686`, `2609.17145`,
  `2609.17168`, `2609.17302`); conventional control, estimation,
  gait, locomotion, and mechanism design (`2512.23650`, `2601.19499`,
  `2603.23983`, `2604.11768`, `2605.13748`, `2609.10286`, `2609.16256`,
  `2609.16319`, `2609.16405`, `2609.16437`, `2609.16786`, `2609.16810`,
  `2609.16958`, `2609.17249`, `2609.17240`, `2609.17405`, `2609.17430`);
  multi-robot and swarm coordination (`2607.20992`, `2609.16075`,
  `2609.16346`, `2609.16852`, `2609.17384`); and agricultural,
  construction, marine, space, exoskeleton, HRI, and service-domain work
  (`2609.14710`, `2609.15232`, `2609.16089`, `2609.16369`, `2609.16413`,
  `2609.16996`, `2609.17124`, `2609.17240`, `2609.16880`).
- **Digital-agent papers screened and set aside** as too far from a robot or
  general harness contract to justify an entry this window: `2609.15983`
  (Stellar Colosseum — excluded by the previous run as well), `2609.16625`
  (AURA recommender diagnosis), `2609.16680` (little m), `2609.16760`
  (turn-level density ratio estimation), `2609.16936`, `2609.17010` (ThinkFlow),
  `2609.17088`, `2609.17107`, `2609.17391` (FlashVector), `2609.17416`
  (Never Stop Thinking), `2609.17439`, `2609.17523` (ScienceBuddy),
  `2609.12394` (BlueLM-GUI), `2609.16251` (CADWorld), `2609.16541`, `2609.17306`.
- **Withdrawals and administrative records in this block:** none affecting the
  robot/harness scope, and no previously curated entry was withdrawn.

## Operational notes

- **The export API recovered.** After two consecutive runs of blanket
  `HTTP 429 "Rate exceeded."`, `https://export.arxiv.org/api/query` answered
  HTTP 200 for every category this run. cs.RO and cs.AI still returned `429` on
  the first attempt and succeeded on a 20-second-spaced retry, so intermittent
  rate limiting persists — the retry loop and the OAI-PMH fallback both stay in
  the pipeline.
- **OAI-PMH remains the authoritative path for announcement membership.** The
  export API's `submittedDate` filter cannot see a replacement version whose
  original submission date is outside the window, and this block's seven
  revisions plus 56 older-ID records are exactly that case. Block counts
  (1,061 memberships / 779 unique IDs for 2026-09-16) come from OAI-PMH; the
  export API is used as the documented primary path for the new-submission window
  and as a cross-check.
- **The frontier moved from `2609.15989` to `2609.17527`** in the announced
  block, while the ID frontier is `2609.17531` (`2609.17532` → 404). IDs
  `2609.17528`–`2609.17531` resolve but are not in this block, so ID existence
  alone overstates the announced frontier by four.
- **The Zenodo API is the correct verification path, and it can return a
  definitive negative.** The previous run found that `doi.org` landing pages
  return 403 to non-browser clients and recommended the API. This run added the
  complementary case: `https://zenodo.org/api/records/<id>` returned
  `{"status": 404, "message": "The persistent identifier is not registered."}`
  for a DOI printed in a paper's own data-availability statement — a stronger
  and more reliable "not open" signal than an HTTP redirect. Two other DOI-shaped
  links failed the same way this run: the ACM DOI in RobResilience's HTML
  (`10.1145/3847353.3847504` → 404) and the Zenodo DOI in BLINDSPOT's related-work
  HTML, which resolved to an unrelated record — a reminder that a DOI appearing
  in a paper's HTML is not necessarily that paper's artifact.
- **`.scratch/arxiv-2026-09-16/`** (untracked, deliberately not staged) holds the
  harvest XML, parsed `records.json`/`newblock.json`, the export-API responses,
  screening and link-extraction scripts, and the fetched `abs`/HTML pages,
  version histories, and artifact-check output. `/tmp` is **not** persistent
  between shell invocations on this host, so `/tmp/ax0916` created in the first
  probe was already gone by the next call; all state lives under `.scratch/`.
- The working branch was `main` throughout. Only task-owned files (`README.md`
  and this record) were staged; the unrelated user changes
  (`docs/reference-architecture.md` modified; `docs/ring-harness.png` and
  `handoff.md` untracked) were left untouched.

## Validation performed

- `git diff --check` clean.
- Markdown structure re-validated by heading enumeration after editing: every
  `##`/`###` heading is present and in the original order, with the eight new
  entries confirmed by script to sit under `### General Harness Design and
  Self-Improvement`, `### Agentic Robot and VLA Harnesses`, `### Manipulation and
  VLA`, `### Agent and Embodied Reasoning`, `### Safety` (×2), and
  `## Surveys and Reading Lists` (×2) respectively.
- Every newly added URL was fetched: **17 distinct links** (8 arXiv `abs` pages,
  5 GitHub repositories, 2 project sites, 1 documentation site, 1 Hugging Face
  organization) returned **HTTP 200**. The three artifact links that do **not**
  resolve are documented as such rather than presented as live:
  `aida-ugent/act-cvae-forensics` (empty repository), `bokang-ugent/nanoACT`
  (404), and `zenodo.org/api/records/21469516` (404, unregistered).
- README internal consistency: the `last verified` badge and the body
  `Last verified:` line were both moved to **2026-09-16**, matching this record's
  date. Per-entry "verified" dates elsewhere in the README were left at their
  original values, since they record when that entry was last checked rather than
  the list's verification date.
- Cross-file consistency: all names and arXiv IDs in the README changes were
  grepped against `README.md`, `docs/`, and `sources/` before editing. No
  duplicates: FluxVLA, RobResilience, Social Harness, the ACT forensic re-run,
  the SWE-bench resolution audit, BLINDSPOT, and both surveys had zero prior
  mentions; the "World-Action Models" and "World Models for Embodied
  Intelligence" title greps matched only the distinct GlanceWAM and TacPAC
  entries already curated, and "MessyMem"/"MANGO" matched only the previous
  record's exclusion notes, which this record supersedes.
- Categories and artifact statuses in every new entry were taken from primary
  sources (arXiv record, `abs` submission history, official project page, GitHub
  REST API, Hugging Face Hub, Zenodo API) on 2026-09-16, and each entry states
  the author-reported versus verified distinction, the simulation-only status
  where applicable, and the literal artifact status.
