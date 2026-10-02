# Daily arXiv scan — 2026-10-02

## Scope

- **Interval:** everything announced after the 2026-10-01 run's cutoff. That run
  (started ~08:46 local / ~06:46 UTC on **Thursday 2026-10-01**) screened the
  announcement block whose OAI datestamp is **2026-10-01** — 1,505 unique base
  IDs, newest base ID `2609.40362`. This run started ~10:25 local / ~08:25 UTC on
  **Friday 2026-10-02**. **There are no missed dates:** the 2026-10-01 18:00
  catch-up slot could not see a new block (the next announcement lands at 02:00
  local on 2026-10-02) and no run intervened between the two. The overnight cron
  at 06:00 local did not execute because the WSL host was off; this run is the
  catch-up and covers the single pending block.
- **A new announcement block landed.** The OAI-PMH harvest for
  `from=2026-10-02&until=2026-10-02` returned records for all five categories:
  **1,955 set memberships = 1,386 unique base IDs**, every one carrying datestamp
  **2026-10-02** (memberships: cs.LG 620, cs.AI 580, cs.CV 309, cs.CL 287,
  **cs.RO 159**). This is the Thursday-submission block.
- **Block composition.** 937 records carry a 2026-10-01 created date (new
  submissions), 294 carry 2026-09-30, and the remainder reach further back.
  **944 carry `2610.` IDs** and **442 predate that range**, of which **306 do not
  start with `2609.`** (replacement versions and re-announcements reaching back to
  2023–2025). **The newest base ID moved from `2609.40362` to `2610.02210`**, so
  the ID frontier advanced by 2,410 and the block is not being held back.
- **Previous-block re-check.** The datestamp-2026-10-01 block now holds **1,451**
  unique IDs against the **1,505** the previous run recorded for it: **55 records
  were re-announced forward** into 2026-10-02, and **one record newly appeared in
  the window** — `2602.22673`, which carries a withdrawal notice. The two
  datestamp windows share **no** IDs, so nothing in this block duplicates the
  previous block's membership.
- **Export-API cross-check (HTTPS, `curl --globoff`).**
  `submittedDate:[20261001 TO 20261001]` returns HTTP 200 with cs.RO 72,
  cs.AI 233, cs.CL 109, cs.CV 152, cs.LG 243 = **809 memberships / 593 unique
  IDs**; the query for `submittedDate:[20261002 TO 20261002]` returns **0 for all
  five categories**, the expected indexing lag (2026-10-02 submissions are
  announced 2026-10-03). **0 API IDs are missing from the OAI harvest**, and 344
  OAI records with `created == 2026-10-01` are not yet in the API slice, so the
  **OAI-PMH harvest is the authoritative source** for every number above.
- **15 of the 1,386 new-block IDs already appear somewhere in the repository**
  (README, `docs/`, `sources/`), two of them at README level. Because an OAI
  datestamp of 2026-10-02 means "announced in this block" — and the previous
  block shares no IDs with this one — each of the 15 was checked individually
  against its arXiv version history and the facts recorded in this repository.
  **Two carry a README-level revision, both metadata-identical to their previous
  version** (Meta-Skills `2609.38143` v2, EmbodiRSI `2609.38905` v2); one
  previously excluded paper published a **real artifact release**
  (Thinkingbox `2608.19741`, promoted below); three exclusions changed their
  artifact or evidence status (Dex-X, One from Infinity, ForeWAM); one withdrawal
  notice became explicit (`2609.35674`); and eight were cosmetic, non-substantive
  or title-only re-announcements.
- **Withdrawal notices in this block: one repo-relevant.** `2609.35674` (the
  oracle-bone-character study already recorded as withdrawn on 2026-09-30) now
  carries the reason: "The previous version did not adequately disclose the
  permissions and usage rights associated with the dataset. We are withdrawing
  the manuscript to address this data authorization and compliance issue."
  `2602.22673` carries a withdrawal notice under the previous datestamp. No ID
  referenced at README level was withdrawn, so no entry required annotation.
- **Watch-list re-check:** of the IDs carried by the 2026-10-01 record,
  **none reappears in this block** — not the thirteen IDs from the 2026-09-30
  run, not RoboHarn-Evo (`2609.37583`), SelfSearch (`2609.37968`), F4R
  (`2609.35575`), ASENA (`2609.39207`), Multi-Link Safety Filtering
  (`2609.40007`), Blackout vs. Freeze (`2609.39145`), ActionGuard
  (`2609.39450`), Who Verifies the Graph (`2609.40027`), HIDE/SEEK
  (`2609.38886`) or TeV (`2609.39038`). They are carried forward unchanged. The
  seven records that were **deferred rather than rejected** on 2026-10-01
  (AssemblyWorld `2609.40353`, Tri-Info `2606.19998`, AutoDataBench
  `2609.40097`, EgoTools `2609.39378`, RoboAssist `2609.39384`, RoboCoach
  `2609.39685` and Inline Memory `2609.39794`) likewise do not reappear in this
  block; they remain deferred for a later run, and none was reachable for
  re-verification today because all seven keep their original IDs and were not
  re-announced.

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`) over three windows: `2026-10-02…2026-10-02` (new
   block), `2026-10-01…2026-10-01` (late-batch/boundary re-check), and
   `2026-10-01…2026-10-02` (previous block plus new block). Pages saved to
   `.scratch/arxiv-2026-10-02/oai-{new,prev,full}-*.xml`; every category and
   window returned a single complete page with no resumption token.
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`oai-records-{new,prev,full}.json`, `parse_oai.py`), then **delta against the
   previous run's parsed harvest** with field-wise before/after values
   (`delta.json`), including the entity-normalisation control carried since
   2026-09-29 so that parser-induced diffs are not reported as record changes.
3. **Export-API range queries** (`https://export.arxiv.org/api/query`, HTTPS;
   `submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]`, percent-encoded and
   `--globoff`) for all five categories on 2026-10-01 and 2026-10-02, used as an
   independent count and boundary check rather than as the primary source.
   Counts are reported in Scope.
4. **Screening** over all 1,386 records against a 3,268-ID corpus extracted from
   `README.md`, `docs/*.md` and `sources/*.md` (`screen.py`), with
   keyword-family rankings for harness/scaffold/runtime, self-improvement,
   agentic-robot, VLA/action-expert, world/foundation-model, evaluation/safety and
   robot/embodied concepts, plus an explicit uncurated robot listing and an
   artifact-signal listing with extracted URLs.
5. **Two independent second screens.** (a) Every new-block record that mentions
   *harness* anywhere in title or abstract was listed and reviewed; (b) a
   robot∩harness pass re-ranked all 1,371 uncurated records by combined
   robot-embodied and harness/evaluation signal. These produced four candidates
   the family ranking had under-weighted (`2610.00487` ScaffoldM3C,
   `2606.21562`, `2610.01863` LiteReality-Agent, `2610.01670`), all of which were
   screened and excluded as out of scope.
6. **Dossier verification** by eight parallel read-only agents (~63 candidates,
   each a thematic group: harness optimization; runtime contracts and auditing;
   robot recovery/memory/safety; robot benchmarks and self-improvement;
   VLA/world-action models; safety and security; revision and artifact
   re-checks; secondary harness candidates and robot infrastructure). Each fetched
   the arXiv abs page (version history), the full-text HTML (or recorded that it
   was absent), and **every** candidate project page, GitHub repository, Hugging
   Face model/dataset and DOI, recording HTTP status; each was capped at 5
   `api.github.com` calls and told to prefer GitHub HTML and
   `raw.githubusercontent.com`. Group reports are in
   `.scratch/arxiv-2026-10-02/dossiers/out-{A…I}.md`.
7. **Duplicate check** by arXiv ID and by distinctive title phrase against
   `README.md`, `docs/` and `sources/` for every shortlisted candidate; every
   included ID was grepped again after insertion.
8. **Watch-list re-check** by ID against the new block and the wide window (see
   Scope).

## Included (README updates)

23 entries were added, grouped as they appear in the README.

### Harnesses and Development Platforms — General Harness Design and Self-Improvement

- **ActiveSaddler** (`2610.00906`, cs.AI/cs.CL/cs.LG/cs.MA/cs.SE; v1 2026-10-01)
  — makes the *curriculum* the missing dimension of harness optimization:
  recurring failures become reusable failure-pattern arms in a non-stationary
  bandit, learning progress per arm is estimated, and exploration of unseen
  scenarios is balanced against revisiting known weaknesses so curriculum and
  harness co-evolve. Reported: GAIA2 Pass@1 55.4→59.8 (+4.4) and Terminal-Bench
  2.0 72.5→80.0 (+7.5) against the same optimizer with a frozen scenario order,
  with the cost of reaching a target dev score falling 4.6× ($1,360→$298) and
  1.7× ($220→$128). **Artifact:** MIT code live as the V2 `activesaddler`
  task-selection policy on the `microsoft/AutoSaddler` feature branch (parent
  repo 232★, 26 commits on the branch) — not a standalone release or tag, and the
  paper's "will be available" hedge is stale. Digital agents only; no robot
  experiment.

- **Harness Annealing (HAT)** (`2610.01235`, cs.CL; v1 2026-10-01) — asks how much
  of the harness the *model* can absorb. HAT pairs explicit control supervision
  with a curriculum of teacher trajectories collected under a four-level removal
  ladder (file access, state, workflow, answer checking) and evaluates every
  checkpoint under four deployment harnesses, so *harness internalization* — not
  task score alone — is the measured object. With Qwen3.5-9B and 35B-A3B on
  SWE-QA/SWE-QA-Pro, selected annealed checkpoints running with tools alone come
  close to their starting checkpoints under the full harness. The authors report
  honestly that the benefit varies with model scale and configuration and that
  further annealing is not uniformly better. **No first-party artifact of any
  kind** (the substrate is the third-party MIT `Darwin-Agent/HarnessX`, 483★), so
  this is listed as a conceptual contribution with a bounded negative result.

- **GUI-HARVEST** (`2610.00948`, cs.LG/cs.AI; v1 2026-10-01) — evidence-driven
  harness evolution for GUI agents with frozen backbones: diagnoses are tied to
  before/after screenshot transitions, repeated runs of one task are treated as a
  joint evidence unit, and verified findings become recurring failure patterns
  mapped to bounded source-code edits with predictions recorded before
  evaluation and per-edit promotion/rollback gates. Reported: +12.33 points for
  Qwen3-VL-32B-Instruct on OSWorld-Verified and frozen-harness transfer of
  +13.87 points to GPT-5 on WindowsAgentArena at 50 steps, ahead of Self-Harness
  and Meta-Harness at matched backbone and initial harness. **Artifact:** the
  Apache-2.0 `GaryYang12345/GUI-HARVEST` repository ships the optimizer, config
  schema, `docs/METHOD.md` promotion contract, skills and tests (last commit
  2026-09-29, 0★) but deliberately **no benchmark and no seed harness**, so
  reproduction requires installing OSWorld and Agent S3 independently at
  unspecified revisions. Digital only.

- **Finding the Right Fit** (`2610.00917`, cs.AI/cs.SE; v1 2026-10-01) — 66
  configurations (four configurable harnesses × five models plus native
  Codex-GPT and Claude Code-Claude pairings) under one runner, one scoring rule
  and one cost ledger across TUA-Bench, ALE-CLI and Terminal-Bench 4. Model
  rankings reverse across harnesses (Claude +7.94 over GPT under OpenHands,
  −30.16 under PI on TB4); for four of five models the best harness changes
  between benchmarks; vendor harnesses are not reliably best and cost does not
  reliably buy score. **Artifacts:** Apache-2.0 adapters and 6,204 scored
  trajectories with the code pushed 2026-10-02 — but the dataset is
  **CC-BY-NC-4.0**, a split licence that blocks commercial reuse. Digital only.

- **VeriHarness** (`2610.00972`, cs.AI/cs.MA; v1 2026-10-01) — a verification
  harness that turns the same base model that produced the rollouts into an
  agentic verifier with a workspace, evidence tools and reusable verification
  skills; a disagreement resolver checks competing claims against environmental
  evidence and a consensus challenger tests shared claims and hunts omitted
  requirements. The premise is a negative finding — disagreement often exposes
  correct alternatives while consensus conceals errors — and the claimed gain
  comes from rollout-pool structure plus actively acquired evidence rather than a
  stronger judge. Reported: best primary selection in 10/10 model–benchmark
  settings, +6.2 points (Gemini 3.5 Flash) and +6.4 points (Claude Opus 4.8) from
  evidence-backed revision, with ~26,000 released rollouts produced at a reported
  cost above $100,000. **Artifacts:** Apache-2.0
  `google-research/veriharness` (harness, benchmarks, skills, docs; pushed
  2026-10-02) and an ungated HF dataset whose licence is
  `mixed-upstream-see-readme`, so redistribution terms for the five upstream
  benchmarks are not resolvable from the Hub page. Digital only.

- **Mingbird** (`2610.02001`, cs.AI/cs.CL; v1 2026-10-01) — a shipped local-first
  harness whose thesis is that much small-model failure is harness failure. Ten
  named mechanisms map to failure forms, with a byte-level net-zero prefill
  budget enforced in CI, signature-level loop detection escalating nudge → hard
  reset → graceful exit, an executable completion gate that re-reads the task and
  verifies its own work, test-execution feedback, auto-backup/rollback, a crawl
  guard and a five-ring safety model. Reported: self-built LRAB (4 harnesses × 4
  open models 2B–35B × 18 real tasks, **all 288 cells published**) gives 0.886 vs
  0.631/0.479/0.405; τ²-bench 0.856 vs 0.791/0.737; the same 2B model spans
  0.017→0.821 across four harnesses. The leave-one-mechanism-out ablation is
  labelled **directional only** (same-night replication noise 0.069 ≥ every
  nominal single-trial delta) and only the batch-matched completion-guard
  comparison is well powered (+0.10 paired). **Artifact:** Apache-2.0 v1.9.2, 58
  commits, benchmark protocol and scoring code, `LICENSE-DATA`; 4★. Digital only,
  single machine, single-trial scoring.

- **Beyond Memory / PoS** (`2610.01415`, cs.AI; v1 2026-10-01) — an inference-time
  harness whose context is an explicit *belief state*: Entity/State/Relation world
  estimate plus epistemic gaps and achievement gaps, a Belief Sentinel
  consistency validator, trapping-aware recovery (Static/Cycle/Drift) and
  detection of *Belief Trapping*. Reported: best overall metric in all 12
  benchmark–backbone settings, up to +22.68% (ALFWorld) and +37.89% (RCA-100) over
  the strongest same-backbone baseline, with the cost disclosed — 5.06× total
  tokens versus raw trajectories on one setting, and "existing context management
  does not consistently improve upon raw trajectories". **Artifact:** MIT
  `luoyu100/PoS` (10 modules, commits through 2026-10-02, 13★) plus a project
  page. Digital only.

- **VISTA** (`2610.02200`, cs.AI/cs.CV; v1 2026-10-01) — a visual harness: the
  model perceives the environment visually, keeps a lossless memory of final and
  animation frames, and can actively retrieve and reorganize its visual input
  while reasoning, with an explicit untrained mechanism. Reported: Claude Opus
  5.0 rises from 40.68 to 100.00 RHAE on ARC-AGI-3 across all 25 public games
  with 57.4% fewer actions than first-time human participants, plus gains on
  three further visual benchmarks. **Artifact:** MIT `joshhhhhan/VISTA` (118★,
  runners and Dockerfiles) and a live blog — but the repository is a single
  squashed commit dated about a month before the preprint, and contamination is
  explicitly unresolved. Digital only.

### Harnesses and Development Platforms — Unified Evaluation

- **Zero2Repo** (`2609.38269`, cs.SE/cs.AI; v1 2026-09-29 → **v2 2026-10-01**) —
  from-scratch repository construction as an execution-verified contract: the
  agent gets a PRD, an interface contract and an empty workspace, builds inside a
  real CLI harness, then the workspace is frozen and a hidden deterministic
  acceptance suite runs in a fresh container with no LLM judge. **Artifact:** 17
  released cases across Python, TypeScript, Go, C++, C and Zig, the `cbrun`
  runner, Docker recipe lock and judge entrypoint, plus a live leaderboard — but
  **no LICENSE file** (raw `LICENSE` 404), so reuse terms are unspecified.
  Reported: the strongest agent solves 10 of 11 evaluation tasks; every failing
  submission passes 90–99% of hidden tests; 67–100% of a strong agent's failed
  tests trace to one omission or a low-frequency spec rule. Neutralization does
  not prevent recall of the upstream project (authors' own caveat). Digital only.

- **Kepler** (`2610.00834`, cs.AI/cs.MA; v1 2026-09-30) — an auditable harness that
  represents hypotheses as executable world models validated by retrospective
  transition checks and conditional prediction checks through five tools.
  **Artifact:** MIT `Cveinnt/kepler` with `DESIGN.md`, `INTEGRITY.md`,
  `RESULTS.md`, `ABLATION.md`, a reproduction guide, a **v1.0.0 release**, an
  `incidents/` directory documenting its own evaluation failures (source-read
  leakage, control agents reconstructing a removed harness, repair masking a
  broken planner) and an MIT public trace corpus. Reported: one frozen Claude
  Opus 5 configuration reaches server-verified 100.00 RHAE on all 25 public games
  with 8,256 environment actions and $777.72 of API spend; the author states that
  whether the harness or the models cause the scores is unmeasured until a
  boundary-correct ablation is run. NeurIPS 2026 non-archival workshop; digital
  only.

### Robot Middleware and Execution

- **multipanda_ros2** (`2602.02269`, cs.RO/cs.AI/cs.SE/eess.SY; v1 2026-02-02 →
  **v2 2026-10-01**, ICRA 2026, DOI 10.1109/ICRA57385.2026.11696149) — a runtime
  contract rather than a policy result: native ROS 2 torque/position/velocity/
  Cartesian interfaces for any number of Franka arms from one process on
  `ros2_control`, 1 kHz control, runtime controller switching with ≤2 ms delay,
  `ControlException` recovery through a service that re-executes the previous
  loop without reloading, and MuJoCo co-simulation with quantitative
  kinematic/dynamic comparison plus real inertial identification. **Artifact:**
  Apache-2.0 repository, 88★/42 forks, 15+ `franka_*` packages and a Docker
  installer, default branch `humble`; last commit 2026-08-25 **predates** the v2
  paper, and it depends on a fork of `mujoco_ros_pkgs`. Real Franka Panda
  hardware (single, dual, GARMI) plus simulation; author-reported.

### Robot Agent Systems — Agentic Robot and VLA Harnesses

- **Recova** (`2610.01178`, cs.RO; v1 2026-10-01) — the batch's strongest
  recovery-harness story: an agent diagnoses failures in a reconstructed digital
  twin and writes/tests recovery programs, while the deploy-time harness monitors
  progress at 30 Hz, decides when the scene is not intact, dispatches a learned
  recovery policy or a programmatic recovery, **verifies scene restoration with a
  monitor model**, resumes the task policy, requests a human demonstration only
  when no recovery exists, and routes that episode into DAgger. It also owns
  multi-station orchestration (one operator, four workstations). Reported:
  LIBERO-Pro 78.8% vs 71.7% (ASPIRE) and MolmoSpaces 64.9% vs 38.0%; on four
  bimanual YAM workstations over 20 trials per task per configuration 23.8% base →
  77.5% +DAgger → 87.5% +recovery skills, with human intervention across four
  collection rounds falling **87.5% → 0%**. Baselines are imported published rows,
  not re-runs. **No artifact:** project page and demo MP4s only; the `recova-bot`
  GitHub account has **0 public repositories**. Real robot plus simulation,
  author-reported.

- **Rewind-IL** (`2604.16683`, cs.RO/cs.AI/cs.CV; v1 2026-04-17 → **v2 2026-09-30**,
  IEEE RA-L 2026, DOI 10.1109/LRA.2026.3734897) — a training-free online safeguard
  around a frozen action-chunked policy that separates *when* from *where*:
  detection by TIDE, the mean-squared discrepancy between the current and previous
  action-chunk predictions on the overlapping window, thresholded by split
  conformal prediction calibrated on successful episodes; respawning to an
  offline, VLM-built database of semantically verified checkpoints matched by
  cosine similarity, after which the runtime **clears the temporal-ensembler
  buffer and action queue** and restarts inference clean. Reported: detection
  balanced accuracy 0.95 vs 0.78 for the strongest conformal-calibrated baseline;
  20 real rollouts per task on two AgileX Piper arms recover perturbed ACT from
  15–25% to 80–85% on five of six tasks at 22.0 ± 7.2 ms per step; the matched
  ablation shows swapping the recovery axis collapses success to the no-recovery
  floor while swapping the detector costs little. v2 adds a Lipschitz analysis of
  TIDE and the detection/respawn disentanglement. **No artifact:** project page
  says "Code (Coming Soon)".

### Vision-Language-Action Models — Open and Reproducible

- **Dex-X** (`2609.07747`, cs.RO; v1 2026-09-07 → v2 2026-09-09 → **v3 2026-10-01**,
  CoRL 2026 per project page) — visual-tactile dexterous manipulation learned from
  monocular human video with simulation as a tactile completion engine: MANO
  trajectories retarget to a Franka FR3 + Sharpa Wave hand and replay in Isaac
  Lab for contact supervision, and a privileged PPO teacher is distilled into a
  30 Hz point-cloud + proprioception + tactile student. Reported: 65.9% teacher
  success across six simulated task categories, 93% real cube picking, 53% table
  cleaning, zero-shot generalization to unseen geometries. **Artifact release
  verified (promoted from watch):** MIT `gcfy63821/dexx_lab` (3 commits,
  2026-09-29/30) contains the training/inference source, **three checkpoints
  verified as genuine PyTorch archives**, retargeted data and a ten-lesson
  rebuild tutorial with runnable checks — but only **four sample demonstrations**,
  not the full dataset. The v3 arXiv posting is **content-identical to v2** (9
  diff spans, all arXiv stamp and LaTeXML affiliation footnotes); the substantive
  event is the code/weight release, not the revision.

### Robot Foundation and World Models — World and Physical-Reasoning Models

- **One from Infinity / RoboActualizer** (`2609.36413`, cs.RO; v1 2026-09-29 →
  **v2 2026-10-01**) — formalizes *actualization*: keep the pretrained video world
  model frozen and learn a tiny actualizer that selects the task-conditioned
  future and reads out the actions realizing it; a frozen V-JEPA 2.1 ViT-L/16
  encoder plus as few as **60M trainable parameters** (two DiT experts in a
  Mixture-of-Transformers trained with flow matching), one 32 GB GPU, ~15 hours,
  no embodied pretraining. Reported: LIBERO 98.0%, LIBERO-Plus 63.1/65.0%,
  RoboTwin 2.0 58.84%, 39 ms inference, real deployment on a G1 and a Fanuc
  CRX-10iA; removing the latent-prediction target collapses LIBERO-Plus
  60.0→17.9%. **Artifact release verified:** MIT code (9 commits) and MIT weights
  (three checkpoints, 127–492 MB, published SHA256SUMS) went live within hours of
  v2 — from a personal GitHub account and an unlabelled HF account, with a
  leftover "will release when accepted" sentence still in the paper.

- **UniWAM** (`2610.02054`, cs.RO; v1 2026-10-01) — a Mixture-of-Transformers over
  a physical reasoner, world generator and action predictor, whose reusable
  contributions are an *action-space interface* (low-level robot actions written
  in natural language so a pretrained VLM adapts to embodiment without losing
  language ability) and a documented data cleaning/annotation pipeline assigning
  VQA, egocentric and robot supervision to the right expert. Reported: LIBERO
  ≈99%, RoboTwin 2.0 ≈75% (50 tasks), LIBERO-Plus ≈92% across seven perturbation
  dimensions, real AgileX Piper ablation 51.74% → ≈71% from the pretraining
  recipe. **Artifacts:** Apache-2.0 `UniWAM/UniWAM` (32 commits, 4★) and public
  ModelScope checkpoints — but the release explicitly excludes Bridge, DROID,
  Fractal and real-world inference, so the real-robot numbers are not
  reproducible from it; the code is a renamed derivative of `thu-ml/Motus`
  ("OLA-SEM").

### Datasets and Data Infrastructure — Multi-Robot and Foundation-Model Data

- **MIKASA-Robo-VLA** (`2610.00604`, cs.LG/cs.AI/cs.RO; v1 2026-09-30) — turns
  "does this policy have memory?" into an environment-specified measurement: 90
  language-conditioned ManiSkill 3 tasks, all but 10 hiding the cue the action
  depends on (those 10 are reactive controls), three horizon splits, and, for 70
  tasks, an *information gap* — the provably cue-absent interval — 28 of which
  exceed the 16-frame window of the widest fixed-context VLA surveyed. 22,500
  oracle trajectories, 10 memory types, 6M+ transitions, RLDS 287.4 GiB plus a
  LeRobotDataset v3 release (the RLDS repository's Hugging Face viewer does not
  index its TFRecord shards, so the LeRobot release or the docs site is the
  primary download path; the README's ICLR-2026 badge refers to the original
  32-task MIKASA-Robo, and this 90-task extension states no venue), with the
  evaluation protocol in code. The π0.5
  reference baseline (14 tasks, no history, no memory module) reaches
  0.211 ± 0.044 mean success, and the authors flag the Long split as confounded
  by open-loop chunking. **Artifact:** MIT repo (139★) plus two ungated MIT HF
  datasets (785 and 3,667 files) and a docs site — released **before** the paper
  and unchanged since 2026-06-30. Simulation only.

### Benchmarks and Evaluation — Manipulation and VLA

- **HumanoidToolBench** (`2610.02089`, cs.RO/cs.AI; v1 2026-10-01) — tool use as a
  staged contract: 18 tasks over three scenarios × three execution levels
  (selection → stationary use → mobile use) × two tool-set modes (standard/decoy)
  with 55 tool assets, recording interaction events so selection is scored apart
  from execution, plus ToolBook (3.1k demonstrations) and an `htb-eval` CLI that
  writes machine-readable results. The headline is negative: policies frequently
  pick the wrong tool and fail execution, decoys expose selection failures, and
  GR00T N1.7 probes lose selection accuracy on unseen tools and continue under
  unrelated instructions. **Artifacts:** MIT toolkit (14 commits), ungated HF
  teleop dataset (CC BY-NC 4.0), two simulation checkpoints. Real evidence is 91
  Unitree G1 trajectories across three policies — author-reported, small N.

- **DexHoldem** (`2605.18727`, cs.RO/cs.AI; v1 2026-05-18 → v2 2026-09-30 →
  **v3 2026-10-01**) — a real-hardware benchmark coupling dexterous execution,
  agentic perception and embodied decision routing on one physical tabletop: 14
  manipulation primitives with primitive-specific success checks, a 36-problem
  deterministic perception bench with published normalization rules, and a
  system-level loop with explicit endpoint criteria. Reported: π0.5 leads
  primitive completion at 61.2% with π0.5/π0 tied on scene-preserving success at
  47.5%; Opus 5.5 leads perception at 49.1% strict / 80.6% field-wise (the gap is
  the finding); in the v3 system study only **12.1% of 33 closed-loop hands
  complete**, retries recover 12 of 34 dispatches, and exactly one hand finishes
  with neither a retry nor human help. The 1,470 teleoperated ShadowHand
  demonstrations are CC BY 4.0, but both code repositories carry **no LICENSE**
  and were last pushed May 2026, so the v3 numbers are not reproducible from them.

### Benchmarks and Evaluation — Agent and Embodied Reasoning

- **Thinkingbox** (`2608.19741`, cs.CL/cs.DB; v1 2026-08-20 → v4 2026-10-01) —
  **promoted from the 2026-08-21 watch note after an artifact release.** A sandbox
  and benchmark whose contract is evaluated over terminal backend state rather
  than responses: isolated MCP-compatible tool sessions, complete traces, a
  simulated LLM user, and task-specific executable checks that reject wrong,
  missing or extra effects. 507 policy-conditioned workflows across five business
  domains; Claude Opus 5 falls from 66.50% pass@1 to **47.53% pass^20** and
  Kimi-K3 from 57.37% to **17.60%**, and many failed trials terminate cleanly
  after valid state-changing actions. **Artifacts verified:** MIT
  `microsoft/thinkingbox` (Python framework, session proxy, `tb` CLI, tests,
  15 commits, 84★) and `microsoft/thinkingbox-data` (executable benchmark, MCP
  tool servers, `thinkingbox-bench-v1.0` tag; code MIT, data
  CDLA-Permissive-2.0), with an HF `tasks` split containing exactly the paper's
  507 cases. The v4 revision is an **abstract/metadata change — the PDF is
  byte-identical to v3** — and `microsoft/thinkingbox-training`, cited in the v4
  footnote, returns 404. Digital only.

- **Embodied Agent Arena** (`2610.00854`, cs.RO; v1 2026-10-01) — 1,000 cases from
  32 sources plus GeoProbe, a new 168-case geometric-estimation benchmark over
  Blender renders and real-scene images, evaluating VLM agents across geometry,
  spatial reasoning, affordance, task planning and manipulation. The contribution
  is the evaluation contract: a minimal harness preserves each source's own
  observations and semantics while separating metric precision, functional
  grounding and native goal completion. Reported: Astra's affordance strict-box
  success 44% vs Qwen-Max 96% (box IoU 0.694, mask IoU 0.361) and 37 trajectory
  failures (27 initiation, 24 arrival, 10 collision check). **Artifact:** MIT
  repository (40 commits) pushed the day of the paper; physical-robot evaluation
  is listed as future work, so all evidence is digital/simulation.

### Runtime, Safety, and Observability — Safety

- **False Prophets** (`2607.23147`, cs.CR/cs.AI; v1 2026-07-25 → **v2 2026-10-01**,
  AISec '26) — attacks the harness's own verification artifact: a text-based world
  model used as a pre-execution checker can be induced to predict a benign
  outcome for a destructive command (`$GITHUB_WORKSPACE` unset ⇒ checker predicts
  artifact cleanup while the real system deletes `/bin`). It classifies
  world-model vulnerabilities as mitigable or intrinsic, releases a benchmark
  corpus of terminal scripts, and gives countermeasures; the authors report
  induced mispredictions in **up to 95% of cases** for some categories. Evidence
  is digital-agent only; the `mlsec-group/worldmodel-security` repository is a
  **single-commit, unlicensed** drop. The v2 announced here is a
  licence/venue/copy-edit revision with no new experiments (v2 switches the paper
  to CC BY 4.0).

- **POEF / intent–behavior gap** (`2412.16633`, cs.RO/cs.AI/cs.CY;
  v1 2024-12-21 → **v5 2026-10-01**, NDSS 2027, DOI 10.14722/ndss.2027.230579) —
  measures how often jailbreak *intent* fails to become physically executable
  behavior (hallucinated control APIs, logic errors, kinematic and hardware
  violations), so intent-level attack success overstates physical risk. Contributes
  the Harmful-Behavior dataset (150 harmful + 150 harmless RLBench tasks with
  reference policies, scenes and success criteria; ISO 10218 risk taxonomy), POEF
  (hidden-layer-gradient prompt optimizer plus a four-agent
  acceptance/harmfulness/logic/physical-executability evaluator, with a word-level
  constraint so the suffix survives speech recognition), and two harness-side
  defenses. Reported: average 45% intent–behavior gap; 80% behavior jailbreak
  success in CoppeliaSim; black-box transfer to a Franka Panda and a Unitree G1
  (10 trials per task, closed-source planners); GPT-4o task success drops 82.67
  points under the SafeRobot prompt. Real robot plus simulation. Two caveats
  recorded: **neither repository carries a LICENSE**, and v5 silently corrected
  three table numbers (85.33→55.33, 80.00→74.00, 100.00→99.33) without
  explanation.

## Revision edits to existing entries

- **Meta-Skills** (`2609.38143`, README line 442; v1 2026-09-29 → **v2
  2026-10-01**): the v2 announcement carries **no metadata change at all** —
  title, abstract, comment and announced file size (1,237 KB) are identical to v1
  — so the README entry stands unchanged. Recorded here so a later run does not
  re-open it.
- **EmbodiRSI** (`2609.38905`, README line 606; v1 2026-09-30 → **v2 2026-10-01**):
  same result — identical title, abstract, comment and announced size (11,385 KB).
  No edit; the entry's facts remain current.
- **Thinkingbox** (`2608.19741`): the previously recorded exclusion note in
  `sources/daily-arxiv-2026-08-21.md` ("no verified release; watch") is now
  **stale**; the paper is included in the main list (above) and the release is
  verified. The stale note is left in place as a historical record, and this entry
  supersedes it.
- **Dex-X** (`2609.07747`): `sources/daily-arxiv-2026-09-09.md` recorded "watch
  for code"; the trigger has fired. Promoted to the main list, with the note that
  the arXiv v3 is content-identical to v2.
- **One from Infinity** (`2609.36413`): `sources/daily-arxiv-2026-09-30.md`
  carried it in a bulk exclusion sweep for model/policy papers without a runtime
  or artifact; the MIT code and checksummed weights released on 2026-10-01 change
  that assessment, and it is now included under Robot Foundation and World
  Models.
- **ForeWAM** (`2608.11605`; v1 2026-08-12 → v2 2026-10-01): the v2 revision is
  **substantive** — +5 pages, abstract rewritten around new results (RoboCasa
  59.2% vs 49.5% Fast-WAM at one-sixth the demonstrations, LIBERO-Plus 61.6% →
  77.6%, 88.7 ms and 6.27× speedup) and a **new real-world manipulation section**,
  so the 2026-08-13 rationale ("no real-robot evaluation") is no longer valid.
  It stays on **watch** rather than in the list because the project page's "Code
  Coming soon" is plain text with no URL and no repository, weights or data exist;
  the stale figure (LIBERO-Plus 61.6%) must not be carried forward.
- **ACE** (`2607.04162`; new version 2026-10-01): the revision changes the
  in-document title and raises reported figures (50/70 → 70/80) but has **no
  artifact of any kind**; remains excluded.
- **WholeBodyWAM** (`2609.18197`) and **PARTS** (`2609.21788`): new versions add
  a project page and appendices respectively. WholeBodyWAM's v2 is
  content-cosmetic (+53 characters; abstract and headings identical) and its page
  uses a disabled `href="#"` for code (not a release). PARTS' v2 is substantive
  in text (+10,050 characters of appendices, including one on *Human Contracts
  and Executable Scaffolding*, which is why it stays on watch rather than being
  dropped) but its code is a plain-text promise with no link. Both remain
  watch-list items.
- **Rethinking World Models for Safety-Critical Embodied Systems (RIWM)**
  (`2609.03774`, cs.AI/cs.RO; v1 2026-09-03 → **v2 2026-10-01**) — carried as
  "watch (perspective)" since 2026-09-04. The v2 is marginal: the abstract is
  unchanged and exactly one section (§3.5, illustrative applications) was added,
  so the 6-page perspective contributes no artifact and no measurement.
  **Watch-lite**; do not re-open on the strength of the revision alone.
- **ACE** (`2607.04162`, cs.RO; v1 2026-07-05 → **v2 2026-09-30**) — never
  previously recorded in this repository. The v2 is the batch's largest claim
  change: the in-document title becomes "Semantic Task Composition for Tabletop
  Manipulation via Tracked Pick-and-Place Masks" while arXiv metadata still serves
  the old title, the text now calls the method an *agentic manipulation harness*,
  and reported figures rise from 50%/70% to 70%/80% (55%/70% without persistent
  context). **No artifact of any kind** exists — no page, code, weights or data —
  so it is recorded here as a watch item rather than an entry.
- **SyzHarness** (`2609.23889`): the new version is a **typography-only** change
  (hyphenation in "LLM-only", "bug-critical"). Non-substantive.
- **2609.35674**: the withdrawal notice now states the reason (undisclosed
  dataset permissions and usage rights). It was already recorded as withdrawn on
  2026-09-30; no README impact.
- **MIKASA name collision re-checked:** the repo already references a *different*
  MIKASA (the original 32-task RL suite and `simplememvla_mikasa` checkpoints in
  `sources/daily-arxiv-2026-09-09.md`); the new entry is keyed to `2610.00604`
  (the 90-task VLA extension) and must not be merged with them.

## Current Landscape additions

Ten bullets were appended to `## Current Landscape`, covering: harness
optimization moving into the curriculum and up to the harness/model boundary
(ActiveSaddler, Harness Annealing); harness choice as a controlled variable
(Finding the Right Fit, VeriHarness); small-model competence re-attributed to
harness engineering (Mingbird); context as an explicit auditable state object
(PoS, VISTA); executable end-state contracts as the benchmark spine (Thinkingbox,
Zero2Repo); auditable harness artifacts shipping their own failure record
(Kepler); recovery split into detection and restoration (Recova, Rewind-IL);
robot benchmarks separating selection from execution and scoring reliability
(HumanoidToolBench, DexHoldem, MIKASA-Robo-VLA); verification artifacts as attack
surfaces (False Prophets, POEF); and frozen-backbone efficiency contracts with
real releases (RoboActualizer, Dex-X). The `last verified` badge and line were
updated to 2026-10-02.

## Rejected / watch list

### Screened this pass and rejected

- **Harness Annealing** is included above; the note that its **artifact is
  absent** is the literal status (third-party MIT HarnessX is not the authors'
  contribution).
- **ReLiveGym: Evaluating Long-Lived Agents over Weeks of Replayed Reality**
  (`2610.00710`, cs.AI) — an interesting evaluation design (agents replayed
  against multi-week reality), but the released repository has **no LICENSE**,
  only 2 commits, and the 33 GB of replayed-reality data is gated and
  undocumented. **Watch.**
- **Praxa: an Evidence-Bound Harness for Governed AI Agent Execution**
  (`2610.00015`, cs.AI) — an evidence-bound governed-execution harness with a live
  Apache-2.0 npm SDK/CLI/MCP, but the `preprint-v1.4.0` release contains only
  paper assets (PDF/DOCX/source zip/manifest), the benchmark repository is
  paper-only and unlicensed, `praxa-harness` returns 404, the governed runtime
  sits behind a closed hosted gateway, and the evidence is single-author with
  neutral benchmark results. **Watch.**
- **When Harnesses Lose the Signal: Causal Evaluation of Recovery in LLM Agents**
  (`2610.00372`, cs.AI), **Auditing Action Settlement in LLM Agent Environments**
  (`2610.01138`, cs.AI), **Authorization for Self-Modifying AI Agent Populations**
  (`2610.00347`, cs.AI/cs.CR), **Actions with Receipts** (`2610.00327`,
  cs.AI/cs.CL/cs.CR/cs.LG), **Continuous Process-Level Evaluation for Evolving
  Enterprise AI Agent Skills** (`2610.01833`, cs.AI/cs.SE) and **Measuring the
  Microtask Eligibility Gap** (`2610.00025`, cs.AI/cs.LG) — each touches a
  harness contract the repository curates (recovery, action settlement,
  authorization under forking/rollback, replayable receipts, skill-lifecycle
  evaluation, model/harness right-sizing) but **none releases an artifact** — the
  authors state results "remain local, not a public release", withhold
  executables, or use a self-authored benchmark. **Watch.**
- **Empty Commitments: When Agents Promise What Their Runtime Cannot Deliver**
  (`2610.01045`, cs.AI/cs.CL) — a four-page position paper on runtime promises
  with **no experiments**; excluded on the same grounds as other conceptual notes
  that ship no measurement.
- **SE-GoS: Self-Evolving Graph-of-Skills for Skill Library at Scale**
  (`2609.08228`, cs.AI/cs.CL; v1 2026-09-08 → v2 2026-10-01) — a skill-library
  evolution method relevant to skill discovery, but **no URL appears anywhere**
  and the in-text "released code" is unlinked. **Watch.**
- **VideoEvolve** (`2610.01766`, cs.AI/cs.CV) — evolving harnesses for video
  temporal grounding; the linked GitHub repository is **empty** ("This repository
  is empty"; raw README/LICENSE 404). Excluded literally.
- **EvoGen-Harness** (`2610.00383`, cs.LG) — harness evolution for image
  generation: non-agent, non-robot, **no artifact and no availability statement**.
  Out of scope.
- **It Takes Workflows to Evolve Better Workflows** (system name **FloWright**;
  `2610.01026`, cs.AI/cs.CL; v1 2026-10-01) — in scope on mechanism and reported
  result (+7.41%), but the repository is a **scaffold** (README, LICENSE and
  config only; the `harness/` and `flow/` paths 404), so it fails the literal
  artifact bar. **Watch.**
- **LabBook** (`2610.00675`, cs.AI/cs.LG) — repository README reads "Coming
  Soon…" with LICENSE only. **Watch.**
- **Agent Error Dataset** (`2609.40111`, cs.AI/cs.CL) — 50,000 error–diagnosis
  pairs, but the authors "plan to open-source following acceptance". **Watch.**
- **Keyword Harnesses Fail Open** (`2610.02142`, cs.CL) — the diagnostic ladder
  itself is not published; only an unlicensed HF checkpoint mirror exists.
  **Watch.**
- **Refusal Localizes, the Damage Relocates** (`2610.00320`, cs.CL/cs.CR/cs.LG) —
  good repo and MIT licence, but generic LLM fine-tuning safety with no harness,
  agent or robot contract. Out of scope.
- **Managing Context and Communication in Distributed Agentic UAV Swarms**
  (`2610.01569`, cs.AI/cs.LG/cs.MA/cs.NI/cs.RO) — **no artifact**; simulation
  (SITL) only. **Watch.**
- **PROMO** (`2610.01260`, cs.AI/cs.HC/cs.LG/cs.RO/cs.SY/eess.SY) — the advertised
  open-source URL `amrmousa.com/promo/` returns **404** and the author's sitemap
  contains no such page. Excluded.
- **WBAG** (`2610.01083`, cs.RO) — a whole-body/attached-geometry safety framework
  for VLA manipulation, but every method is handed simulator ground-truth object
  identities (including the baseline), there is **no artifact URL at all**, and
  the evidence is simulation-only; the safety margin therefore overstates what a
  deployable perception stack would achieve. **Watch.**
- **ECoMEM** (`2610.00801`, cs.RO) — explicit concept memory with real-robot
  evidence (7-DoF YAM, 68/79 vs 5/58) but "Code · Coming soon". **Watch.**
- **Divide-and-Remember** (`2610.00982`, cs.AI/cs.RO) — the abstract promises
  "Code, checkpoints and more results" at a page that is an **anonymised
  placeholder** saying both paper and code are "coming soon" with zero `href`s.
  The artifact claim is false as written. **Watch.**
- **TRUST / When Reasoning Helps Action** (`2610.00601`, cs.AI/cs.RO) — CoT
  monitoring and steering for VLA policies is on-theme, and its reasoning-quality
  metric rises 75.9% → 90.0%, but **closed-loop manipulation task success is
  essentially unchanged**, the correctness labels come from VLM judges (digital
  even though the underlying policy is a VLA), and the project page says "Code
  Coming Soon". **Watch** on evidence quality rather than topic.
- **Is Success All You Need?** (`2610.01351`, cs.RO) — the footnote claims "Full
  code … is made available" while the linked repository is an **empty 1 KB
  placeholder** with no licence and `main` returning 404. Excluded literally.
- **FineART** (`2609.36416`, cs.AI/cs.CV/cs.LG/cs.RO) — the abstract claims "We
  open-source the full dataset, model weights, and training code", but the record
  and full text contain **zero artifact URLs** (the only link is the generic
  `huggingface/lerobot` organization) and both HF and GitHub searches return
  nothing. Excluded literally; the announced v2 was metadata-only.
- **Reconstruct, Practice, Go Real** (`2610.02204`, cs.AI/cs.RO/cs.SY/eess.SY) —
  literal "Code coming soon" placeholder. **Watch.**
- **InterEvolve** (`2610.02196`, cs.CV/cs.GR/cs.RO) and **EIDA** (`2610.01219`,
  cs.RO) — no artifacts at all. Deferred rather than rejected; both are test-time
  evolution / execution-interface adaptation designs a later run may revisit.
- **Tool-Policy Co-Design for Powder Weighing** (`2609.39797`, cs.RO) — the only
  artifact is a YouTube video. Excluded.
- **DuoMind** (`2610.02161`, cs.AI/cs.RO) — repository says "code coming soon"
  and the dataset link 404s; simulation-only. **Watch.**
- **Bounded-Fidelity Sim-as-Demo-Stage** (`2610.00008`, cs.AI/cs.RO) — an
  Apache-2.0 reference implementation and replication data are genuinely live
  (1 commit, 2026-06-26, 0★) and the work is honestly labelled digital-only, but
  the contribution (mocap handoff for governance benchmarks) is narrow enough
  that it is recorded as **deferred**, not included, in this pass.
  **arXiv ID/date anomaly to remember:** this record carries an October `2610.`
  ID but `published`/`updated` of **2026-07-09** (single version, confirmed
  present in the cs.RO 2026-10 listing), so ID-prefix heuristics must not be used
  to infer announcement date.
- **Ego2Act** (`2610.01092`, cs.CV/cs.RO) — a fully released evaluation harness
  (MIT code, two ungated CC-BY-4.0 datasets, live page) but its subject is
  egocentric *video generation*, not robot execution; excluded by the standing
  scope rule on video-generation work with no robot/VLA harness contract.
- **Secondary robot-infrastructure screen (nine further records reviewed in
  detail; all recorded here so a later run does not re-litigate them):**
  **ALFRED** (`2610.01477`, cs.AR/cs.CV/cs.RO) is the batch's sharpest integrity
  finding — it uses the strongest "open source / released in full" language while
  its cited `CiaranJohnson/ALFRED` URL returns 404 (HTML and API), its
  `ForestYear3D` repository also 404s with code described as kept private, and
  its dataset is "coming soon"; it must not be recorded as open source.
  **STARS** (`2609.40245`, cs.LG/cs.RO) has an **unfilled project-page
  template** (literal `github.com/ORG/STARS`, `arxiv.org/abs/XXXX.XXXXX`) and its
  arXiv abstract is the *wrong paper's* (SocialNav-SUB's) — do not curate; note
  that the neighbouring SocialNav-SUB (`2509.08757`, CoRL 2025) is genuinely open
  but belongs to a different paper and batch. **EvolvingNav** (`2609.39166`,
  cs.AI) has real code committed 2026-10-01 and the batch's strongest
  state/timing/verification runtime design (predictive 4D belief, arrival-time
  forecasting, evidence de-duplication), but EvoWorld-Bench (54 scenes, 803,680
  tasks) is **not released** and the repository carries **no LICENSE** — top
  watch item for a code/data release. **TacDyn-WAM** (`2610.00638`, cs.RO) —
  project page live but Code is a disabled "Code is coming soon" span. **ReCo**
  (`2610.01612`, cs.RO), **FlashNav** (`2606.15846`, cs.RO) and **HAMA**
  (`2610.00897`, cs.RO) — no artifact of any kind. **Dyna3** (`2610.01286`) and
  **CLoSeR** (`2610.01927`) — cs.CV reconstruction work whose claimed repository
  (`MoyangLi00/CLoSeR`) returns 404. **ID flag:** `2609.36081`, which the
  keyword screen surfaced as a robot-adjacent candidate, resolves to an unrelated
  continual-learning paper ("Early Learning Shapes Later Directions of
  Representation Change in Continual Learning"); no robot paper exists at that ID,
  so a later run should not chase it.
- **ActiveWAM** (`2610.01698`, cs.RO) — the model's own repository is an
  unchecked TODO stub with no licence and the project page says "Coming soon";
  the reusable asset is **RoboTwin-AV** (MIT, 237 commits, 50-task active-vision
  extension with an evaluation protocol and data-collection harness), which is
  recorded here as a **deferred benchmark candidate** for a later run rather than
  added under a paper whose own artifact is a placeholder.
- **DynamicVLA** (`2601.22153`, cs.CV/cs.RO), **Fewer Tokens, Better Action**
  (`2610.01939`, cs.CV), **Kinematic MeanFlow** (`2610.00864`; repository 404),
  **Completion Aware Guidance** (`2610.01559`; no URL), **Supervise What Decides
  Success** (`2610.01224`; no URL), **FutureWorlds** (`2610.01019`; weights
  unpublished, no licence, digital-only metrics), **Measuring Asset and Scene
  Reconstruction Effects** (`2610.00731`; the cited release commit 404s) and
  **VAPS** (`2610.01397`; "Code — on publication") — conventional model or
  policy papers whose artifact status does not clear the bar this pass. Deferred
  individually, not rejected as a class.
- **Benchmarking the Safety of LLMs for Robotic Health Attendant Control**
  (`2604.26577`, cs.AI/cs.CY/cs.RO; R. Soc. Open Sci. 13(9):261022, online
  2026-09-30) — the 2026-10-02 OAI entry is a **metadata-only re-announcement**
  (latest version remains v2, 2026-07-24, with `created` = 2026-07-24 and absent
  from the 2026-10-01 harvest), so nothing new was published today. The content is
  a substantial simulation-only safety benchmark (270 instructions across 72 LLMs;
  mean violation rate **54.4%**, proprietary 23.7% vs open-weight 72.8%) whose
  repository `kztakemoto/RHASafety` is real (5 commits, pushed 2026-09-30) but
  whose README **claims MIT while `LICENSE` returns 404**; scoring is
  response-level with an LLM judge spot-checked on 45 responses, not physical
  execution. **Watch** — a promotion candidate if the licence is fixed and the
  contract is extended past response-level violation.
- **The Alignment Flywheel** (`2603.02259`, cs.LG/cs.MA/cs.RO; v3 2026-10-01) —
  the strongest *reusable governance contract* in the safety group: a real
  MIT-licensed repository (`decide-ugent/Alignment-Flywheel`, 18 commits, 27.9 MB,
  API-confirmed licence) providing an Oracle interface, enforcement with
  fail-closed behaviour, signed batches, staged rollout and rollback. Deferred
  rather than rejected only because the evidence is digital demos on an IIRL grid
  and a clinical proxy, and the authors state there is no production or clinical
  validity claim; a later run can promote it as a runtime-governance entry on the
  strength of the contract alone.
- **Out of scope by standing rule:** the remaining uncurated records in the block
  — generic manipulation, locomotion, navigation, grasping, world-model,
  driving, medical, video-generation and perception-only papers without a
  reusable agent/VLA harness, recovery, runtime contract, safety or evaluation
  contribution; pure model/scaling papers; and papers whose only "artifact" is a
  project page of figures or videos.

### Carried watch list — unchanged this window

- The thirteen IDs carried by the 2026-09-30 run (`2609.24274`, `2609.24350`,
  `2609.24563`, `2609.22587`, `2609.21223`, `2609.21502`, `2609.22218`,
  `2609.22247`, `2609.26121`, `2609.26520`, `2609.25558`, `2609.29091`,
  `2609.29065`), plus RoboHarn-Evo (`2609.37583`), SelfSearch (`2609.37968`),
  F4R (`2609.35575`), ASENA (`2609.39207`), Multi-Link Safety Filtering
  (`2609.40007`), Blackout vs. Freeze (`2609.39145`), ActionGuard
  (`2609.39450`), Who Verifies the Graph (`2609.40027`), HIDE/SEEK
  (`2609.38886`) and TeV (`2609.39038`) — **none appears in this block**; all are
  carried forward unchanged with their previously recorded artifact status.
- Artifact placeholders recorded on 2026-09-25 remain unchanged: Maithili/GAP's
  1 KB repository, HEXIS's HTTP 401 anonymous link, the RoboRecover dataset
  ("being prepared for Hugging Face") and HarnessPAI's "not yet publicly
  released" code.
- StageWAM (still v3), ReflexVLA/ReflexBench (post-acceptance), DreamX-Phi
  (README-only), UniTexture (no code), PRISM ("Dataset soon"), GigaBrain-0.7/WBC
  (release unconfirmed), ForceU-VLA (README-only), LIBERO-VIFO, Agent Lightning,
  VLCP, Hydra-0, BATON — no change this window.

## Validation performed

- Markdown structure checked: heading hierarchy, list rendering, absence of
  malformed bullets and unbalanced link parentheses across all 23 new entries and
  10 new landscape bullets (`README.md` now 1,120 lines, 40 headings).
- `git diff --check` reports no whitespace errors.
- Every included ID was grepped after insertion: each appears once in the main
  list and, where applicable, once in the Current Landscape; no duplicate entry
  was created, and none of the 23 IDs or their titles was present in `README.md`,
  `docs/*.md` or `sources/*.md` before this run.
- Dates and categories cross-checked against the arXiv records: all 23 entries
  carry their true version history (v1 date and latest version date), and the two
  README-level revisions (Meta-Skills v2, EmbodiRSI v2) were verified to be
  metadata-identical to their prior versions before concluding no entry edit was
  needed.
- Cross-file consistency: the `last verified` badge and the `Last verified:`
  line both read 2026-10-02, matching this record's date; the Current Landscape
  bullets reference only entries now present in the list.
- Artifact statements in the README were taken from the dossiers' literal HTTP
  checks (codes recorded in `.scratch/arxiv-2026-10-02/dossiers/out-*.md`), and
  every "verified open" claim in the new entries is tied to a checked repository,
  dataset or release tag; no unchecked URL was described as released.

## Operational notes and blockers

- **`/tmp` is per-command in this sandbox**, so all harvest XML, parsed JSON,
  dossiers and logs were kept under `.scratch/arxiv-2026-10-02/` (untracked,
  never staged). The task prompt's "save XML to `/tmp`" instruction was satisfied
  in substance by keeping the XML on disk for inspection; the literal `/tmp` path
  does not survive between commands here, matching the operational note carried
  by recent runs.
- **Export-API indexing lag reproduced:** the query for 2026-10-02 submissions
  returned 0 entries for all five categories while the 2026-10-01 query returned
  593 unique IDs, all of which are present in the OAI block. The OAI-PMH harvest
  is again the only source that sees the whole announcement.
- **Eight verification agents ran in parallel** and were each capped at 5
  `api.github.com` calls; the group reports record few or no API calls, so the
  shared unauthenticated budget was not exhausted. Everything else was read from
  GitHub HTML tree pages, `raw.githubusercontent.com`, project pages, the arXiv
  HTML, and the Hugging Face Hub. All HTTP statuses are recorded per candidate in
  `.scratch/arxiv-2026-10-02/dossiers/out-*.md`.
- **A parser artifact worth remembering:** the OAI harvest returns each record
  with the datestamp of its *latest* modification, so a paper whose previous
  version was announced before the previous run's window appears as an
  "appeared" ID with no local before/after diff. Version histories must be read
  from the arXiv abs page. This is how the Meta-Skills, EmbodiRSI, ForeWAM and
  One from Infinity revisions were classified.
- **Two "false as written" artifact claims were caught and recorded literally:**
  Divide-and-Remember's abstract points at an anonymised "coming soon" page, and
  the VLA-perturbation paper's footnote claims available code that is an empty
  1 KB placeholder. Neither is described as open source anywhere in the README.
- No conflict occurred; `git fetch origin`/`git pull --ff-only` were run before
  committing and the working branch is `main`.

## Commit

Task-owned files committed on `main`: `README.md`,
`sources/daily-arxiv-2026-10-02.md`. Unrelated user changes
(`docs/reference-architecture.md`, `docs/ring-harness.png`, `handoff.md`,
`.scratch/`) were left untouched and unstaged. No branches, no pull requests, no
force-push.

- **Curation commit:** `47a17b1` — "Curate October 2 robot harness research"
  (`README.md` + this record; 23 entries added, 10 Current Landscape bullets, the
  `last verified` date advanced to 2026-10-02).
- **Sync:** the working branch was confirmed as `main`; `git fetch origin` and
  `git pull --ff-only` were run before committing (already up to date at
  `f7560fe`).
- **Push:** `git push origin HEAD:main` advanced `f7560fe..47a17b1`, and
  `git ls-remote origin refs/heads/main` returned the same SHA as local `HEAD`.
- **SSH workaround used:** the documented `Bad owner or permissions on
  /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf` failure occurred on the first
  `git fetch`; retrying with `GIT_SSH_COMMAND='ssh -F /dev/null'` succeeded for
  fetch, pull, push, and `ls-remote`. The user's key and `known_hosts` were
  unaffected.
- **Unrelated user changes verified untouched after the push:**
  `docs/reference-architecture.md` (modified), `docs/ring-harness.png` and
  `handoff.md` (untracked), and `.scratch/` (untracked scratch tree) all remain
  unstaged and uncommitted.
- **Record-only follow-up commits:** `109b3cb` (SSH workaround and this SHA
  block) and `0c04164` (secondary robot-infrastructure screen and the
  `2609.36081` ID flag) touched only this file. Each was pushed to `origin/main`,
  and `git ls-remote origin refs/heads/main` confirmed the remote tip equalled
  local `HEAD` immediately after every push; the tip of `main` after the last
  record commit is this run's final state.
