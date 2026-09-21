# Daily arXiv scan — 2026-09-21

## Scope

- **Interval:** everything announced after the 2026-09-20 run's cutoff. That run
  (started ~16:00 UTC on Sunday 2026-09-20) screened the announcement block whose
  OAI datestamp is **2026-09-18** — 907 unique base IDs, newest base ID
  `2609.20822`. This run started ~06:36 UTC on **Monday 2026-09-21** and covers
  everything announced since.
- **A new announcement block landed.** After two consecutive runs in which the
  OAI harvest returned `noRecordsMatch` for the newer window, the harvest for
  `from=2026-09-19&until=2026-09-21` returned a real block: **935 memberships =
  708 unique base IDs, every one carrying datestamp 2026-09-21**. This is the
  Sunday-night (US Eastern) announcement the 2026-09-20 record predicted, and it
  ends the weekend gap.
- **Wider window, for the previous block and the delta:** `from=2026-09-18&until=2026-09-21`
  returns **2,122 memberships = 1,578 unique base IDs**, split as **870 carrying
  datestamp 2026-09-18** and **708 carrying 2026-09-21** (no other datestamp is
  present in the window).
- **New-block composition (708 unique IDs), by listed category:** cs.LG 251,
  cs.AI 223, **cs.RO 170**, cs.CV 150, cs.CL 141, plus cross-listed stat.ML 32,
  cs.CR 24, cs.SY 21, eess.SY 21, cs.SD 18, cs.HC 18, eess.SP 13, eess.AS 13,
  cs.SE 12, math.OC 11, cs.CY 10, cs.IR 10 among others.
- **Composition by the submission date of the announced version** (OAI
  `<created>`): **518 carry 2026-09-18**, 127 carry 2026-09-17, 7 carry
  2026-09-16, 4 carry 2026-09-15, and the remainder reach back through August and
  July to 2024 — the tail is re-announced replacement versions.
  **195 records carry pre-`2609.` IDs**, the signature of replacements.
- **Delta against the previous run's screened block (exact).** Previous block 907
  unique IDs → this window 1,578 unique IDs. **671 IDs appeared, 0 disappeared**,
  and 95 records changed in at least one parsed field (datestamp, title, abstract,
  comments, categories, created date). Of the 37 IDs that are in both this
  window's 2026-09-21 block and the previous run's 2026-09-18 block, all 37 were
  **re-datestamped** from 2026-09-18 to 2026-09-21 rather than re-announced with
  edited content; that move accounts exactly for the previous block shrinking from
  907 to 870 within the same window (907 − 37 = 870). The remaining 671 block IDs
  are genuinely new to screening: **155 carry cs.RO**, 185 carry pre-`2609.` IDs,
  and 485 carry a 2026-09-18 submission date.
- **ID frontier moved and is not truncated.** Newest base ID in the block is
  **`2609.22086`** (the previous run's frontier was `2609.20822`). Direct existence
  probes: `2609.22087`, `2609.22100`, `2609.22150`, and `2609.22200` all return
  **HTTP 404**, so no IDs have been assigned past the frontier and nothing is
  being held back by the harvest.
- **Export-API cross-check.** `submittedDate` windows for **2026-09-19, 2026-09-20,
  and 2026-09-21** return `opensearch:totalResults` **0 for all five target
  categories** (cs.RO, cs.AI, cs.CL, cs.CV, cs.LG): the weekend's submissions are
  announced but not yet indexed by the export API. The **2026-09-18** window does
  return results (cs.RO 84, cs.AI 98, cs.CL 49, cs.CV 84, cs.LG 102), which is the
  same frontier seen from the other direction. The export API is therefore used
  here strictly as a boundary check; the OAI harvest is the authoritative source,
  as in previous runs.
- **Withdrawals in this block: one** — `2309.13476` (a speech-level transformer for
  bi-modal depression detection) carries a withdrawal comment. It appears
  **nowhere in `README.md`, `docs/`, or `sources/`** (checked 2026-09-21), so **no
  curated entry is affected**.
- **41 of the 708 block IDs were already curated** somewhere in `README.md`,
  `docs/*.md`, or `sources/*.md`; every one of them was re-checked and none
  required a change in this window.

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`) for two windows: `from=2026-09-19&until=2026-09-21`
   (the new block) and `from=2026-09-18&until=2026-09-21` (previous block plus new
   block). Pages saved to `.scratch/arxiv-2026-09-21/oai-new-*.xml` and
   `oai-full-*.xml`.
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`.scratch/arxiv-2026-09-21/oai-records-{new,full}.json`, `parse_oai.py`).
3. **Delta against the previous run's parsed harvest**
   (`.scratch/arxiv-2026-09-20/oai-records.json`): set difference for
   appeared/disappeared IDs plus field-wise comparison for content change
   (`.scratch/arxiv-2026-09-21/delta.json`).
4. **Export-API cross-check** over the 2026-09-18 … 2026-09-21 `submittedDate`
   windows per category, used only as a subset/boundary check.
5. **Two full passes over the 708-record block with different ranking emphases.**
   Pass 1 (`analyze.py`) ranked by artifact-signal language (GitHub / Hugging
   Face / GitLab / project page / "we release" / "code available") and by concept
   breadth (harness, VLA, robot foundation, world model, code-as-policy, skill,
   memory, recovery, evaluation, safety, agentic), and additionally listed **every
   record in the block mentioning "harness"** — **10 records**: four were already
   curated (`2607.26148`, `2609.18366`, `2609.19759`, `2609.20116`) and six are out
   of scope (`2604.18519` internal-representation harmful-content detection,
   `2604.23478` LLM-as-judge prompt sensitivity, `2609.13294` "VectorHarness" for
   scientific graphics, `2609.21293` a game-code benchmark, `2609.21879` face-
   recognition explainability, and `2609.22000` RecreationWorld, listed below) —
   and **all 170 cs.RO records**.
   Pass 2 (`rescreen.py`) re-ranked the whole block by lifecycle and contract
   vocabulary instead — release lifecycle (version/rollback/promote/gate: 33
   records), runtime monitor/intervention (54), verification/audit/provenance
   (102), failure diagnosis/recovery (39), evaluation protocol/benchmark contract
   (52), skill library/discovery/evolution (2), robot deployment/integration (15)
   — plus a pass over every robot/agent record carrying artifact language (153
   records carry artifact language overall). The two passes agreed on the
   shortlist; pass 2 is what surfaced `2604.07799` as the standout.
6. **Dossiers and primary-source verification** for every shortlisted record:
   full title/abstract/comments, the arXiv abstract page (version history and
   submission dates), the arXiv full-text HTML (artifact links and release
   sentences extracted), project pages fetched and inspected directly, and the
   official GitHub/GitLab/Hugging Face APIs (license field, size, creation and
   push dates, default branch, top-level tree, commit log) — all on **2026-09-21**.
7. **Duplicate screening** of every candidate against `README.md`, `docs/*.md`,
   and `sources/*.md` by arXiv ID, project name, and repository URL before
   writing. Only one shortlisted ID had a prior verdict: `2609.20648` (SkipVLA),
   rejected on 2026-09-18 and re-checked below.

## Included (README updates)

Eight entries added, plus four "Current Landscape" bullets. Artifact status is
reported literally; every repository and dataset claim below was verified live on
2026-09-21.

### Agentic Robot and VLA Harnesses

- **Evolving Skill Modules under a Fixed Planner: Versioning, Rollback, and
  Runtime Governance for Long-Lived Robot Systems** (`2604.07799`, cs.AI/cs.RO;
  **v3 announced 2026-09-18**, v1 was 2026-04-09; 66 pages, 6 figures, 12 tables,
  submitted to the Journal of Systems and Software) — the strongest fit in this
  block, because it studies the **release machinery** of a long-lived robot rather
  than the policy. A fixed decision layer dispatches versioned skill modules
  (ECMs) that are trained, promoted through a held-out gate, and rolled back when
  a promotion regresses, and every action crosses a runtime governance layer at
  the execution boundary. Its three negative results are the transferable part.
  (i) Peak task success is **unstable across random seeds** — within one method it
  spans **23.3 to 73.3%** — so single-run peaks cannot rank these methods.
  (ii) The rotating four-policy configuration the system's documentation describes
  as a phase decomposition is **refuted by a budget-matched single-policy
  control**: one such policy holds the geometry at the final step in **0.734** of
  episodes reaching it, averaged over seeds, against **0.023** for the rotation,
  with no seed overlap at four seeds per arm (exact p = 0.029), and an intervention
  shows that restoring the termination condition the clock replaced raises
  retention on every seed. (iii) The runtime safety shield cut flagged violations
  **98–100% on five single-arm tasks (34.9% on the sixth) while leaving task
  success at zero** — its acceptance criterion omitted completions, so a shield
  that stopped the robot scored perfectly. What survives is measured lifecycle
  behaviour: the promotion gate kept **12 of 12 injected regressions** out of
  deployment at a **22.5% clean-candidate rejection cost** (the injections were
  calibrated above the gate threshold, so 12 of 12 is close to what that
  calibration guarantees and the rejection cost is the measurement), and the
  post-deployment dip detector caught **9 of 12**, missing all three on one seed,
  with a severity sweep placing the trigger boundary above σ = 0.2. **Artifact
  verified open**: [`s20sc/capability-evolution`](https://github.com/s20sc/capability-evolution)
  (Apache-2.0, 12,862 KB, created 2026-04-12, **pushed 2026-09-18**; `agent/`
  ECM data model with version management, `runtime/` governance predicates,
  `baselines/`, `configs/`, seven evaluation drivers, `analyze_results.py`,
  `make_paper_figures.py`, `tests/`, and `EXPERIMENT_NOTES.md`), whose README
  carries the same negative results verbatim. Evidence is **robosuite simulation
  only** (six tasks); v3 is an important revision and the repository push is a
  real artifact release, so this entry would qualify on either ground; results
  author-reported, no independent reproduction.

### Robot Middleware and Execution

- **OpenRoIS: A Community-Driven Open-Source Middleware Implementing the Robotic
  Interaction Service (RoIS) Framework** (`2609.21178`, cs.HC/cs.RO; submitted to
  the 2027 IEEE/SICE International Symposium on System Integration) — treats a
  **standard as the interface and the implementation as the missing artifact**.
  OMG's RoIS Framework 2.0 defines a platform-independent model in which service
  applications address HRI engines through standardized interfaces and
  hardware-independent symbolic messages, but a specification supplies no
  maintained implementation, SDKs, or adapters. OpenRoIS supplies them: a
  recursive engine architecture in which a single engine class realizes both the
  main and sub HRI Engine roles, an internal five-method component contract
  distinct from the five external RoIS interfaces, a mapping of those interfaces
  onto **JSON-RPC 2.0 over WebSocket**, a single-source-of-truth type pipeline
  generating three consistent language stacks, TypeScript and C# client SDKs with
  web and Unity support, and a Python adapter SDK with ROS 2 support — so one
  service application can address physical robots and virtual agents over the
  internet. **Artifacts verified open**: the Apache-2.0
  [`openrois/openrois`](https://github.com/openrois/openrois) middleware
  (6,517 KB, pushed 2026-09-18), [`openrois/openrois-docs`](https://github.com/openrois/openrois-docs),
  and a Pollen Robotics Reachy Mini reference adapter
  ([`openrois/openrois-adapter-reachy-mini`](https://github.com/openrois/openrois-adapter-reachy-mini)),
  with the organization created 2026-06-02 and
  [openrois.org](https://openrois.org/) live. The contribution is the
  middleware/harness layer rather than a policy result; no robot task numbers are
  reported.

### End-to-End Robot Learning

- **A Sim-to-Real Integration Pipeline for Training and Deployment of Chunk-Based
  VLA Manipulation Policies** (`2609.21817`, cs.RO) — attacks the data-collection
  bottleneck by making **collection and evaluation share one deployment stack**
  instead of two. Expert trajectories generated in simulation are replayed
  **open-loop on a real Franka FR3**, where the corresponding real visual and
  proprioceptive observations are recorded and converted into a VLA-training
  format; the same ROS 2 stack is then reused **closed-loop** to evaluate a
  trained policy on that hardware, so a policy is measured on the configuration
  its data came from. Because every real recording is paired with the simulated
  trajectory that produced it, the protocol also turns the real robot into a
  measurement instrument: the deviation between the paired trajectories is a
  **policy-independent estimate of the sim-to-real gap**, rather than a difference
  in task success inferred end to end. **Artifacts verified open**: the ISIR
  (Sorbonne Université) GitLab project
  [`kappel/sim2real_public_chunk_control`](https://gitlab.isir.upmc.fr/kappel/sim2real_public_chunk_control/)
  (live, 12 validations, created 2026-09-08; ROS 2 packages for real-robot policy
  inference, expert-replay data collection, and ROS-bag debug/replay, with an
  explicit real-time-PC/GPU-PC hardware topology) and both datasets on Hugging
  Face — [`clemgris/RealRobot_push-blocks`](https://huggingface.co/datasets/clemgris/RealRobot_push-blocks)
  and [`clemgris/Simulation_push-blocks`](https://huggingface.co/datasets/clemgris/Simulation_push-blocks)
  (200 paired cube-pushing trajectories; both HTTP 200). Real-hardware evidence on
  one Franka FR3 cell, author-reported; the release is an experimental protocol
  and stack rather than a new model.

### Vision-Language-Action Models — Open and Reproducible

- **VLA-Feedback / "Catch Me If You Can: Real-Time Feedback Denoising for
  Responsive VLAs"** (`2609.21022`, cs.RO; **CoRL 2026**) — added as a row in the
  open-VLA table. Diffusion action generators model temporally coherent action
  chunks but those chunks are normally executed **open-loop** after inference,
  which limits responsiveness when objects move, contacts change, or the scene
  evolves. VLA-Feedback is a two-timescale architecture: instead of fully
  denoising a chunk before execution it **retains the final denoising step as a
  lightweight feedback interface**, so each action is corrected from the latest
  observation before it is executed without rerunning the full vision-language
  diffusion model. The authors report matching GR00T on static LIBERO tasks while
  improving average success on dynamic simulation tasks from **27.5% to 85.0%**
  and, on real-robot tasks, from **51% to 73%**. **Artifacts verified open**:
  [`jidaxian010/VLA-Feedback-release`](https://github.com/jidaxian010/VLA-Feedback-release)
  (Apache-2.0, 92,953 KB, created 2026-09-10) and the simulation checkpoint
  [`jidaxian010/vla-feedback-sim`](https://huggingface.co/jidaxian010/vla-feedback-sim)
  (HTTP 200) on Hugging Face, both linked from the live project page
  [vla-feedback.github.io](https://vla-feedback.github.io/). Real-robot and
  simulation evidence, author-reported, no independent reproduction.

### Robot Foundation and World Models — World and Physical-Reasoning Models

- **SeeQ: Training Generalist Value Functions for Long-Horizon Robotic
  Manipulation** (`2609.22085`, cs.RO; Carnegie Mellon University) — supplies the
  ranking signal a best-of-N harness needs without asking a sparse task-level
  reward to carry the whole horizon. **SeeQ (Subtask-elicited Q-functions)** learns
  Q-values for the **currently active subtask** rather than the complete task,
  which shortens the value-prediction horizon enough for temporal-difference
  backups to become effective; the subtask-level annotations already present in
  offline robot data supply the decomposition during training, and the
  architecture **autoregressively predicts the active subtask in natural language
  before estimating its value**, so no human annotation or separate subtask
  predictor is needed at test time. Built on a PaliGemma backbone, pretrained on
  diverse open-source manipulation data and fine-tuned per task, it steers a base
  policy by scoring candidate action chunks; the authors report substantially
  improved best-of-N steering across **four real-world manipulation tasks on two
  bimanual robot platforms**. **Artifacts verified open**: the paper's code
  released **the same morning as this run** — initial public release 2026-09-21
  05:29 UTC — at
  [`saksham002/generalist-value-functions`](https://github.com/saksham002/generalist-value-functions)
  (Apache-2.0, a fork of Physical Intelligence's `openpi` extended with
  `scripts/train_value_function.py`, `compute_counterfactual_actions.py`,
  `evaluate_value_function.py`, and `serve_policy.py`, plus `packages/`, `src/`,
  and `uv.lock` for `uv sync`), together with the
  [`CMU-AIRe/SeeQ-3B`](https://huggingface.co/CMU-AIRe/SeeQ-3B) checkpoint on
  Hugging Face (created 2026-09-18, last modified 2026-09-21, `license: gemma`).
  **Verification note:** the project page's own Code button points at
  `saksham002/generalist_value_functions` (underscores), which **404s**; the live
  repository uses hyphens and was found by GitHub repository search. The stale
  link is recorded here because a future run checking only the project page's own
  link would wrongly conclude the code is missing. Real-robot evidence,
  author-reported.
- **AtomEgo: Exploring Ego-Robot Integration for Embodied Foundation Model
  Pretraining** (`2609.21461`, cs.AI/cs.RO) — asks not *whether* egocentric human
  video helps an embodied foundation model but **how it has to be aligned to
  help**, and answers with a controlled comparison at scale: a curated corpus of
  roughly **2,659 hours** plus a scalable processing pipeline, evaluated across
  vision–language–action and world–action architectures under three paradigms —
  joint co-training with domain-specific action heads, progressive ego-to-robot
  transfer through embodiment alignment, and joint video–action modeling. The
  stated principle is multiplicative rather than additive (**data scale ×
  alignment quality → capability gain**), and the useful part is the negative:
  direct ego–robot world–action co-training **underperforms** the robot-only
  world–action baseline, while progressive alignment gives the strongest
  real-robot results and both VLA routes improve out-of-distribution performance
  over the robot-only VLA baseline. **Artifact verified open**:
  [`Agentic-Intelligence-Lab/Atom-0`](https://github.com/Agentic-Intelligence-Lab/Atom-0)
  (Apache-2.0, 109,060 KB, created 2026-07-13, pushed 2026-09-07), whose README is
  titled with the paper's own subtitle and ships the Atom-DH, Atom-CL, and
  Atom-WAM routes plus robot-only baselines. It also carries a **paper-alignment
  audit** naming the remaining corpus and reproducibility gaps, including that the
  WAM code was adapted to the manuscript's interface but **not retrained** — the
  kind of first-party disclosure this repository treats as part of the artifact
  description. Evaluation is **real-robot** (seven tasks, 10 in-distribution and
  10 out-of-distribution trials per task per checkpoint, 140 trials per
  checkpoint); results author-reported. The repository predates the paper's v1, so
  it is the project's own release rather than a post-hoc link.

### Runtime, Safety, and Observability — Safety

- **RAYA: Learning Where and When to Intervene for Robot Recovery** (`2609.21690`,
  cs.RO/cs.SY/eess.SY; Carnegie Mellon University and Dartmouth College) —
  relocates recovery from a post-hoc veto to a term inside the controller. The
  observation is that a robot can predict failure and still be unable to prevent
  it, because by the time a safety mechanism reacts the nominal plan may already
  have spent the control authority that recovery requires — and fixed task
  priorities can block whatever response remains. RAYA therefore places a learned
  finite-horizon recoverability margin **inside an optimal controller with hard
  constraints**, so recoverability informs actions while they are being chosen
  rather than being checked afterwards, and pairs it with a bounded learned
  scheduler that shifts task weights as recoverability shrinks. Across **7,200
  simulation episodes per controller** spanning quadrotor and autonomous-vehicle
  benchmarks the authors report improved survival rates, with the learned
  components transferring zero-shot to unseen trajectories, disturbances, plant
  shifts, and friction layouts; an embedded realization was deployed on a **35 g
  Crazyflie**, where across 40 combined hardware flights under wind with either
  aerodynamic mismatch or an unmodeled 40% motor-command loss each of three
  baselines failed in all trials while RAYA completed 10/10 six-cycle missions.
  **Artifact status is not open** (verified 2026-09-21): the paper states "We
  release our project open-source" and the project page
  [raya-control.github.io](https://raya-control.github.io/) renders Paper, Code,
  and Video buttons, but all three are `data-placeholder-link aria-disabled="true"`
  anchors with `title="Link coming soon"` and empty `href`. Treated literally as
  **not open**; included as a substantial paper because the in-controller
  recoverability contract is a distinct addition to this list's recovery line
  (AgentRewind, REVISE, Recoverability as a System Primitive), and because the
  hardware evidence is real rather than simulated. Real-hardware evidence,
  author-reported, no independent reproduction.

### Runtime, Safety, and Observability — Observability and Replay

- **ProTracer: Proprioception-Guided Failure Diagnosis in Robot Manipulation**
  (`2609.21369`, cs.CV/cs.RO; Australian Institute for Machine Learning, Adelaide
  University) — adds the label execution monitoring actually needs: not only
  *whether* a rollout failed, but **when it first stopped following a valid
  trajectory to completion**. The paper defines **failure onset localization** as
  identifying the earliest moment at which an execution deviates from a valid
  task-completion trajectory and is ultimately followed by task failure, and
  treats it alongside binary failure detection, failure categorization, and
  explanation generation — the missing axis, since a failure score with no onset
  cannot tell a monitor where to cut or a recovery policy where to resume.
  ProTracer is deliberately **training-free**: proprioceptive dynamics identify
  temporally informative action boundaries, richer robot-state signals are
  converted into structured natural-language descriptions, and a vision-language
  model jointly analyzes those descriptions with visual observations, so the
  temporal precision of proprioception is combined with multimodal reasoning
  without training a new model. **Artifacts verified open**: the live project page
  [protracer-failure.github.io](https://protracer-failure.github.io/) links Code →
  [`Chang-AIML/ProTracer`](https://github.com/Chang-AIML/ProTracer) (22,307 KB,
  created 2026-09-16, pushed 2026-09-20) and Data →
  [`ChangUoA/FailTime`](https://huggingface.co/datasets/ChangUoA/FailTime)
  (CC-BY-4.0, 227 files, created 2026-09-20, **22 downloads**, with synchronized
  visual and proprioceptive observations and failure-onset annotations). The code
  repository **asserts no license**, so it is source-available rather than open
  source. The released benchmark is the transferable artifact: onset-annotated,
  proprioception-synchronized failure data for whatever monitor or recovery policy
  a harness runs. Results author-reported.

### Current Landscape additions

Four bullets: **the release machinery as the studied object** (Evolving Skill
Modules — with the sharpest lesson stated as "an acceptance criterion that does
not include task completion will certify a robot that does nothing"); **recovery
moving from post-hoc veto to a controller-internal term** (RAYA — with the
disabled-placeholder artifact status recorded inline); **failure monitoring
acquiring an onset label while value functions supply the harness's ranking
signal** (ProTracer + SeeQ); and **one deployment stack shared between data
collection and evaluation** (the sim-to-real pipeline, where the sim-to-real gap
becomes a policy-independent measurement).

## Rejected / watch list

Everything below was read against the arXiv record, the full-text HTML, and (where
a link existed) the live project page or repository API on 2026-09-21.

### Verified and rejected — artifact is a placeholder, an empty repository, or 404

- **SafeStage: Evaluating Safety Before, During, and After
  Vision-Language-Conditioned Robot Manipulation** (`2609.21223`, cs.RO): the
  most tempting near-miss of the block. It is a **lifecycle-structured** safety
  benchmark — 97 purpose-built risk scenarios split into Initial-State Hazards,
  Execution-Time Safety, and Final-State Hazards — that reports native task success
  **independently** from stage-specific safety outcomes and localizes *when* a
  violation occurs, which is exactly the before/during/after contract this list's
  safety section keeps asking for. The full text advertises
  `https://github.com/JinzhuLuo/SafeStage`, and the repository exists — but the
  GitHub API reports **size 0 KB, no license, created 2026-09-17T23:44:29Z and
  pushed 2026-09-17T23:44:30Z** (a one-second create/push, i.e. an empty
  placeholder). Treated literally as **not open**. **Watch for the release**; the
  three-stage separation of task success from safety outcome is the strongest
  benchmark idea in this block and would be an immediate inclusion once the
  scenarios and checks are actually published.
- **Adaptive World Memory 3D Foundation Model** (`2609.21502`, cs.CV/cs.RO): the
  full text links `https://github.com/dtc111111/AWM-3DFM` and states "The dataset
  and code will be made publicly available at …". The repository returns **HTTP
  404 via the GitHub API**, so the future tense is load-bearing and the artifact is
  **not open**. Excluded; **watch**.
- **RAYA** (`2609.21690`): the project page's Paper, Code, and Video links are
  `aria-disabled` "Link coming soon" placeholders (see above). **Included as a
  paper, excluded as an artifact**; re-check on the next run that runs near this
  paper's artifact release.
- **RecreationWorld: Scalable and Verifiable Environments for Hybrid Computer-Use
  Agents** (`2609.22000`, cs.CL/cs.SE): the appendix is titled "Release Artifacts
  and Reproducibility" and promises the resources needed to inspect, run, and
  reproduce the benchmark, and the artifact-language screen flags it as
  open-source with "we release". **No first-party release URL could be located** —
  the only repository links in the full text are third-party dependencies
  (`QwenLM/qwen-code` pinned paths and `microsoft/playwright-mcp`). Digital
  computer-use agents, no robot component, and the release is not reachable.
  Excluded; **watch**.

### Verified and rejected — no artifact, out of scope, or both

- **CommitFlow: Semantic Commitment Verification and Local Correction for
  Long-Horizon Robot Manipulation VLA Execution** (`2609.21908`, cs.RO; ICRA 2027
  submission): the closest thing in this block to a *harness* contribution among
  the VLA papers — a Semantic Commitment Monitor holds back dependent actions when
  a stage's required physical condition is unmet, BoundaryFlow generates a local
  correction with the base policy frozen, and Relation and Gain Calibration picks
  the smallest correction strength that satisfies the constraints (mean 75.9% on
  ten RoboTwin 2.0 tasks, +22.7 points over π0.5). No project page, no repository,
  and no artifact link anywhere in the record or full text. Excluded; **watch**
  for artifacts, since the commitment-monitoring contract belongs in the
  CheckVLA / CoWAM / HarnessWAM verification line.
- **When Should a Failing Robot Ask? Initiating Corrective Human-Robot Dialogue
  from Audited Sensor Evidence** (`2609.21942`, cs.AI/cs.HC/cs.RO; IROS 2026
  workshop): makes the *first* corrective-dialogue decision explicit — act,
  consult another onboard sensor, or interrupt a person — and measures what each
  sensor actually reveals (some failures diagnosable from force data at 0.99 while
  no image method exceeds 0.55), finding that six open VLMs track prompt surface
  rather than evidence (moving the refusal option from last to first collapses
  refusal rates from 78–100% to 0–6% in three of six pairs) and that stated
  confidence carries no information about correctness. The decision should be tied
  to measured accuracy and stated costs, not to model confidence — a genuinely
  useful escalation contract. **No artifact located**; simulated benchmark.
  Excluded; **watch**.
- **VLA-Scope: Shift-Aware Failure Prediction for Vision-Language-Action Models**
  (`2609.21246`, cs.AI/cs.CV/cs.RO/cs.SY/eess.SY): two-stage OOD-shift
  characterization plus a shared logistic failure-risk model over action-prefix
  and execution-progress features (ROC-AUC 0.8497 after 60 actions vs. 0.7906
  without progress features, OpenVLA on ten LIBERO-Spatial tasks). Runtime
  monitoring is in scope, but **no artifact was located**, the domain is already
  covered by FARM, SAFECAST, CheckVLA, and ActProbe inside this list, and the
  evidence is LIBERO simulation only. Excluded.
- **ASGARD: Action-Space Guard for UAV Resilience via Reinforcement Learning**
  (`2609.20982`, cs.CR/cs.LG/cs.RO): targets the window this list cares about —
  actions overwritten **after** the policy emits them and **before** the actuators
  execute them — with a teacher–student monitor that emits corrected commands
  from physical-state history alone, generalizing to unseen and stealthy attacks.
  A real runtime-monitor contract, but **no artifact located**, no hardware
  validation reported, and the line is already carried by OBPE (out-of-band policy
  enforcement), CURA (certified runtime alarms), and the action-space attack
  entries. Excluded; **watch**.
- **Same World, Different Knowledge: When Isolated Audits Misjudge World-Model
  Repairs** (`2609.21155`, cs.RO): distinguishes **fidelity gaps** (exact inputs
  become estimates) from **availability gaps** (inputs are missing) and shows that
  a repair favoured under an isolated input fault can be inferior once deployed
  modules share the faulty information — coupled opposite-sign 10% mass/thrust
  calibration errors cut a physics-anchored model's success from 69% to 8%, and
  repair ranking flips between the isolated and shared-corruption regimes. The
  audit framing is a good evaluation contract, but it is a single-author
  simulation study on quadrotor MPC with **no artifact located**, and the
  world-model-repair subject matter is narrower than this list's evaluation
  entries. Excluded; **watch** if the information-interface audit is generalized
  beyond one controller stack.
- **FAN: Foresight Action Normalization for Continual Adaptation of
  Vision-Language-Action Models** (`2609.21358`, cs.RO): identifies action
  normalization as the neglected mechanism behind continual VLA adaptation, shows
  existing protocols induce inter-task coordinate drift, limited motion coverage,
  or train-test coordinate mismatch, and proposes consistency/coverage/causality
  (3C) design principles with statistics frozen from a small task-independent
  calibration set — evaluated across four **real-world** task streams including
  bimanual manipulation. A representation/normalization contribution rather than
  an external runtime, and **no artifact located**. Excluded under the standing
  model-paper policy; **watch** for code.
- **SkipVLA: Skipping VLA Steps with Classical Planning for Fast Robot
  Manipulation** (`2609.20648`, cs.RO; **v2 2026-09-18**): re-checked because this
  window carries its v2. The 2026-09-18 run rejected it as a model/adaptation
  contribution without an external runtime or a released artifact; the v2 revision
  changes nothing about artifact status (no project page, no repository, and the
  only code-adjacent links in the full text are third-party — Octo, OpenVLA,
  LIBERO), and the hybrid frozen-VLA-plus-motion-planner idea is already
  represented by VLCP and SkipVLA's own neighbours. **Prior verdict stands.**
- **Benchmarking World Models for Continual Learning on Compositional Tasks**
  (`2609.22055`, cs.LG/cs.RO): a good measurement idea — a compositional
  continual-learning curriculum for robot-manipulation world models, factorised
  along action and perception axes to separate knowledge *reuse* from raw learning
  speed, finding that modularity balances reuse against forgetting better than
  conventional methods but that none solve it. **Project page only**
  ([object814.github.io/Compositional-Continual-Learning](https://object814.github.io/Compositional-Continual-Learning/),
  HTTP 200, appendix material and per-task numbers, no code link). Excluded;
  **watch for code**.
- **CARF: Contrastive Attraction-Repulsion of Failure-Guided Flow Matching**
  (`2609.21982`, cs.RO): treats failed trajectories as asymmetric supervision —
  attract toward progressive segments, repel from failure-critical ones, exclude
  ambiguous ones — with a progress scorer trained only on successful
  demonstrations and their perturbations, evaluated in simulation and the real
  world. The project page ([zhao-sq.github.io/carf](https://zhao-sq.github.io/carf/))
  carries only a **Paper** link (a local PDF); no code, weights, or data.
  Excluded; **watch**.
- **Backup-Based Safety Filters: A Comparative Review of Backup CBF, Model
  Predictive Shielding, and gatekeeper** (`2604.02401`, cs.RO/cs.SY/eess.SY;
  **CDC 2026**): a compact tutorial/review that unifies three backup-based safety
  filters under one abstraction and compares them through their filter-inactive
  sets, proving MPS is a special case of gatekeeper and locating a shared source of
  conservatism (safety evaluated through backup-feasibility rather than the nominal
  policy's continued safe execution). Directly relevant to the 2026-09-20 run's
  Runtime Safety Filtering entry, and the unified abstraction is reusable — but it
  is a control-theory review with no implementation, no experiment, and **no
  artifact**, and the standing policy excludes conventional control work that does
  not introduce an agent/VLA harness contract. Excluded.
- **MOSCOPT: Mixture-of-Skills Collective Optimization for LLM Agents**
  (`2609.14399`, cs.AI/cs.CL; **v2 2026-09-18**): a revision, not a new paper. The
  only artifact link is a subdirectory of the large MIT-licensed parent project
  `zhangzhenyu13/SummerClaw` (64.6 MB, pushed 2026-05-30, unchanged in this
  window), so nothing new was released with the revision; the skill-optimization
  line is already carried by SkillOpt, COBRA-Skills, and GraphSkillEvo-adjacent
  entries in General Harness Design. Excluded.
- **GraphSkillEvo: Evolutionary Optimization of Graph-Structured Agent Skills**
  (`2609.21749`, cs.LG): a real and active repository
  ([`ruisun7/GraphSkillEvo`](https://github.com/ruisun7/GraphSkillEvo), 236 KB,
  created 2026-09-09, **pushed 2026-09-21**, no license asserted, README states
  "Our code is available at …"), and skill-graph evolution is in scope for the
  general-harness section. Excluded from this run because it is a digital-agent
  skill-optimization entry in an already very deep section (the 2026-09-18 run
  added eleven harness-optimization entries) and because it asserts no license;
  **watch** — promote if a license is added or if a robot-side instantiation
  appears.
- **Everything else in the block:** the 907 IDs the 2026-09-18 and 2026-09-20
  records screened remain governed by their per-item verdicts, and the 37
  re-datestamped IDs changed no verdict (all 37 were re-read in their 2026-09-21
  form). The remaining records the two novelty passes surfaced — conventional
  manipulation, locomotion, navigation, SLAM, visual odometry, control-barrier
  synthesis, gripper and actuator design, aerial and marine platforms, medical,
  remote-sensing, autonomous-driving, and domain-agent papers, plus the digital
  benchmark and safety clusters (`2609.21259` CogGym, `2609.21527` OpenMAS-GCom,
  `2609.21562` GameLogicBench, `2609.21284` Authorization Revocation alongside the
  already-curated `2609.14744` Runtime Authorization, `2609.22048`
  Available Guardrails, `2609.22043` MDL memory controller, `2609.22068` CodeMidas,
  `2609.21863` AutoRecLab) — introduce no reusable agent/VLA harness, recovery,
  safety, or evaluation contract beyond what the list already carries, or lack a
  reachable artifact. Two specific near-misses are worth recording: **PARTS**
  (`2609.21788`, cs.LG/cs.RO) is a real-world subtask-RL framework whose frozen
  policy is paired with agent-generated selectors and success verifiers, lifting
  complete-task success from 32% to 61% (bimanual YAM) and 50% to 95% (Franka) on
  tens of minutes of real rollouts, but its project page
  ([destiny000621.github.io/PARTS](https://destiny000621.github.io/PARTS/)) ships
  no code and the paper states RLT is reproduced from the paper "as no official
  code is available" — **watch**; and **DEXTERA** (`2609.21045`, cs.RO) automates
  image-to-digital-twin construction with a shared multimodal policy interface
  across 13 task–embodiment pairs, 2 platforms, and 6 policy architectures, with
  **no artifact located** — **watch**.

## Operational notes

- **The OAI set name is `cs:cs:<SHORT>` with the bare suffix, not the full
  category.** The first harvest attempt passed `cs.RO` into a helper that already
  prefixed `cs:cs:`, producing `set=cs:cs:cs.RO`; arXiv answered with a 616-byte
  `badArgument` document reading "Set does not exist" for every category. The
  failure is silent if only the byte count is checked, because a `noRecordsMatch`
  document is also ~616 bytes. `ListSets` (183 sets, 42 under `cs`) confirmed the
  correct spellings. Future runs should assert on the `<error>` element, not on
  response size, and should use the short category names.
- **A `noRecordsMatch` window and a real block can be adjacent.** On 2026-09-20 the
  `from=2026-09-19&until=2026-09-20` window returned `noRecordsMatch` for all five
  categories; one day later `from=2026-09-19&until=2026-09-21` returned 708
  records. Widening `until` by one day is what distinguishes "no announcement yet"
  from "announcement arrived", which is why this run harvested both a narrow and a
  wide window.
- **Re-datestamping, not re-announcement, explains an apparent shrink.** The
  previous block's ID count fell from 907 to 870 inside the same `from=2026-09-18`
  window, which would look like records disappearing. Field-wise comparison across
  the two runs shows the 37 missing IDs did not vanish — they now carry datestamp
  2026-09-21. The set-difference check ("0 disappeared") and the datestamp check
  together are what make this unambiguous; either alone would mislead.
- **Distinguish a paper's own repository from its parent project's, and from its
  project *website* repository.** Three distinct traps appeared in this block.
  AtomEgo's artifact is `Agentic-Intelligence-Lab/Atom-0`, a project repository
  that predates the paper and whose README carries the paper's subtitle — a
  legitimate first-party release, but only recognizable by reading the README
  rather than trusting the name. SeeQ's project page links a repository name that
  404s while the real code lives under a hyphenated name published the same
  morning. And `saksham002/seeq` — which looks like the code repository and which
  GitHub happily serves — is the **project website source** (`index.html`,
  `static/`, `history/`, with commit messages about favicons and chart labels).
  Checking the repository tree, not just its existence, is what separated these.
- **A one-second create/push interval is the signature of a placeholder
  repository.** SafeStage's repository was created and pushed within one second
  (`23:44:29Z` → `23:44:30Z`) and reports size 0 KB; no content was ever added,
  even though the paper advertises it. This is faster to detect than fetching the
  tree.
- **The export API lags the announcement by at least a day.** The `submittedDate`
  windows for 2026-09-19, 2026-09-20, and 2026-09-21 all return zero results while
  the OAI harvest already serves 708 announced records. Consistent with the
  repository's standing note that the export API is a boundary check, not the
  authoritative source.
- **GitHub API rate limits did not appear in this run.** Every repository,
  organization, dataset, and model check completed cleanly (roughly 20 repository
  calls, 3 Hugging Face dataset calls, 2 organization calls, 1 repository search).
- **`git ls-remote` and `git push` require the OpenSSH workaround on this host** —
  `GIT_SSH_COMMAND='ssh -F /dev/null'` — because
  `/etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf` is owned by `nobody`. The
  workaround was needed for the pre-commit sync in this run.

## Validation performed

- Markdown structure checked for all eight insertions: contiguous list items with
  no stray blank lines inside a list, the VLA table's new row carrying exactly the
  five columns of its header, and no heading hierarchy changes.
- `git diff --check` clean; no trailing whitespace and no conflict markers.
- Every added link resolved live on 2026-09-21: `arxiv.org/abs/2604.07799`,
  `2609.21178`, `2609.21817`, `2609.21022`, `2609.22085`, `2609.21461`,
  `2609.21690`, `2609.21369`; `github.com/s20sc/capability-evolution`,
  `github.com/openrois/openrois`, `github.com/openrois/openrois-docs`,
  `github.com/Agentic-Intelligence-Lab/Atom-0`,
  `github.com/saksham002/generalist-value-functions`,
  `github.com/jidaxian010/VLA-Feedback-release`, `github.com/Chang-AIML/ProTracer`;
  `gitlab.isir.upmc.fr/kappel/sim2real_public_chunk_control/`;
  `huggingface.co/CMU-AIRe/SeeQ-3B`, `huggingface.co/jidaxian010/vla-feedback-sim`,
  `huggingface.co/datasets/ChangUoA/FailTime`,
  `huggingface.co/datasets/clemgris/RealRobot_push-blocks`,
  `huggingface.co/datasets/clemgris/Simulation_push-blocks`; and the project pages
  `openrois.org`, `saksham002.github.io/seeq/`, `vla-feedback.github.io`,
  `protracer-failure.github.io`, `raya-control.github.io`.
- Dates and cross-file consistency: the README badge and the "Last verified" line
  now read **2026-09-21**, matching this record's filename; the `RAYA` entry
  records artifact status "not open as of 2026-09-21" rather than implying a
  release; the `SeeQ` entry records the stale project-page link explicitly; no
  curated entry references a withdrawn record (`2309.13476` is the block's only
  withdrawal and appears nowhere in the repository); and each new arXiv ID was
  checked against `README.md`, `docs/*.md`, and `sources/*.md` for prior inclusion
  before writing, with none duplicated.
- Categories recorded per entry match the arXiv listings (cs.AI/cs.RO; cs.HC/cs.RO;
  cs.RO; cs.RO; cs.RO; cs.AI/cs.RO; cs.RO/cs.SY/eess.SY; cs.CV/cs.RO).
- Every "verified open" claim distinguishes code, checkpoint, and dataset, and
  every repository that asserts no license (ProTracer) or is a placeholder
  (SafeStage, AWM-3DFM, RAYA) is described literally rather than as open source.
