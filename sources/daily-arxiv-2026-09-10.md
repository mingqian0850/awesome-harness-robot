# Daily arXiv scan — 2026-09-10

## Scope

- Interval: since the 2026-09-09 run's cutoff. That run's newest visible stamp
  was **2026-09-08T17:59:55Z** (max ID `2609.09158`, TANGO), and it recorded
  that the 09-09 block had not yet landed ("the 09-09 block will land ~18:00
  UTC today, next run's scope"). The 09-09 block **did** land: the API index now
  runs to ID `2609.10540` with newest stamp **2026-09-09T17:59:32Z**, the normal
  end-of-day boundary. The 09-10 block has not landed yet (this run started
  ~06:30 UTC 09-10), so it is next run's scope.
- Dates in scope: 2026-09-08 (the post-17:59:55Z tail) and 2026-09-09, plus
  late-arriving members of the 09-08 block that no prior run could see.
- **New content screened: 395 entries** — every entry with ID above the previous
  run's max visible ID (`2609.09158`), diffed **by ID** rather than by published
  stamp, per the late-arrival methodology the 09-08 and 09-09 records
  established. Of these, **380 carry published stamps after the previous
  cutoff** (09-08 ×70, 09-09 ×310) and **15 are late-arriving members of the
  09-08 block whose published stamps (09-08T06:21Z–17:59Z) fall before it**;
  their abs pages were 404 during the previous run, so a published-stamp filter
  alone would have silently dropped them (see the Notable late arrivals note
  below — one of them is a README inclusion this run).
- Coverage of the 380 post-cutoff entries by primary category: cs.RO 37, cs.AI
  48, cs.CL 62, cs.CV 74, cs.LG 72, with 87 whose primary category lies outside
  the five but which appear in the five categories' cross-lists. Counting all
  entries that *carry* each target category (including cross-lists): cs.RO 51,
  cs.AI 128, cs.CL 86, cs.CV 93, cs.LG 139. All 395 titles were scanned;
  keyword-hit abstracts (~180) were read; every cs.RO-carrying entry was
  individually accounted for. Candidates were checked against the arXiv record,
  official project pages, and official code/model/data repositories with live
  HTTP / GitHub API / Hugging Face API checks on 2026-09-10.
- **Revisions.** The five categories were probed with a broad submitted-date
  window sorted by last-updated descending, so that replacement versions of
  papers submitted *before* this interval would surface (a submittedDate-range
  query cannot see them). **196 records with `updated` in the window belong to
  papers published before the cutoff**; all 196 were title-screened and the 52
  scope-relevant ones were checked against the README and the watch list.
  Verdict: **no README entry needs a revision edit.** The two README entries
  among them — *Beyond Prompts* `2609.05736` v2 and *Bit-Flip Attacks on
  Vision-Language-Action Models* `2608.15475` v3 — are metadata-identical to
  their prior versions (same comment string, same abstract length and head), so
  they are cosmetic. Watch-list revisions are likewise substance-unchanged:
  *Monkey See, Can Monkey Do? / RoboReel* `2609.08209` v2 (project page still
  carries no code or dataset link), *Harbor Adapters* `2609.04298` v2 and
  *VLA-Precision* `2609.04355` v2 (both byte-comparable abstracts). A further
  **13 records returned by the submitted-date query carry an in-window
  replacement stamp**; all are v2s of 09-07/09-08 papers, none scope-relevant
  beyond the RoboReel/PGMT/Hi-FLoop items already covered.
- **Unchanged-batch re-screen requirement.** The newest visible batch *is*
  changed relative to the previous run (IDs advanced `09158`→`10540`, newest
  stamp `09-08T17:59:55Z`→`09-09T17:59:32Z`), so the full screening above is the
  required re-screen; no additional unchanged-batch pass was needed.
- All performance results below are author-reported unless stated otherwise.

## Included (README updates)

### Show-Harness: Just a VLM Agent Can Play Robots

- Paper: https://arxiv.org/abs/2609.10522 (cs.RO, cs.AI, cs.CV, cs.MM; NUS Show
  Lab; first exposure to the curation pipeline — no prior record)
- Project: https://showlab.github.io/Show-Harness/ (live HTTP 200, 2026-09-10)
- Code: https://github.com/showlab/Show-Harness (Apache-2.0, verified live
  2026-09-10 via GitHub API: real package tree — `core/`, `interpreters/`,
  `gumi/`, `plugins/`, `configs/`, `prompts/`, `train/`, `tests/`, `docs/`,
  `pyproject.toml`, ~500 MB; last push 2026-09-10T05:40Z)
- Models: https://huggingface.co/showlab/Show-Harness-VLMs (verified live:
  LoRA/peft adapters `qwen3_5_0_8b`, `qwen3_5_2b`, `qwen3_5_4b`, `qwen3_5_9b`,
  `gemma4_e4b` plus the simulation adapter `qwen3_5_2b_sim`; Apache-2.0)
- Data: https://huggingface.co/datasets/showlab/Show-Harness-Data (verified
  live: real Franka/Piper rollouts plus RoboLab and ManiSkill; Apache-2.0)
- Classification: Agentic Robot/VLA Harness (primary); General Harness
  Methodology (the interface-contract argument).
- Why included: it is the batch's cleanest instance of the thesis this
  repository is organized around — that the harness, not model capacity, is the
  binding constraint. Show-Harness calls itself an *embodied harness* and backs
  the claim with an interface design rather than a new backbone: the VLM sees a
  parameter-free vocabulary of discrete semantic action units and stays
  responsible for fine-grained physical decisions, while embodiment-specific
  interpreters (Franka impedance, AgileX Piper joint streaming, ManiSkill, Isaac
  Lab) deterministically supply metric magnitude and retain raw actuator
  authority. One vocabulary and one prompt set span a 7-DoF Franka, a 6-DoF
  AgileX (single and dual arm), and two simulators. Two control modes share that
  interface: a closed-source frontier VLM drives a real robot zero-shot through
  the full plugin harness, or a small open VLM fine-tuned on demonstrations
  emits one action token per step with no planner. GUMI (GUI Manipulation
  Interface) maps every unit to a browser key, so human demonstration and live
  human takeover during autonomous rollouts use the same channel and every step
  is recorded as a training-ready (observation, action) pair — teleoperation
  hardware is removed from the loop. Plugin isolation is explicit: each plugin
  mounts on one stage of the loop and is byte-identical to no-plugin when
  disabled, which makes ablations meaningful rather than approximate.
- Evidence: the authors report the frontier-VLM band at seconds per step and the
  fine-tuned open backbones holding the top band from 12 Hz up to 33 Hz while
  π0.5 and GR00T sit below at 20 and 24 Hz, with generalization across tasks,
  embodiments, and environments and gains over representative agentic and VLA
  paradigms. Simulated and real-robot evidence, author-reported; no independent
  reproduction.
- Boundary: **artifacts are genuinely open** (Apache-2.0 harness, six adapters,
  demonstration corpus, per-rig runbooks and calibration/preflight tooling —
  verified 2026-09-10), which is rare for this line and is why it is promoted to
  the main list rather than watched.

### No Free Checker: A Survey of Verifiers for Robot Policies

- Paper: https://arxiv.org/abs/2609.09250 (cs.RO, cs.AI, cs.CV, cs.LG, eess.SY;
  survey, 31 pages, 5 figures, 7 tables, 187 references)
- Companion: https://github.com/ZJUSCL/Awesome-Robot-Verifier (MIT; verified
  live 2026-09-10 — searchable reading list, website
  https://zjuscl.github.io/Awesome-Robot-Verifier/, last push 2026-09-10T02:39Z)
- Classification: Evaluation/Safety (primary); General Harness Methodology.
- Why included: it supplies the missing organizing theory for the verification
  layer that this list treats as a harness responsibility. A verifier is
  anything that reads a candidate robot behavior and returns a score — success
  detectors and reward models, runtime monitors, safety filters, temporal-logic
  specifications — and the survey compares roughly 150 of them along two axes
  instead of by method family. *Availability* is how cheap, how early, and how
  repeatably a verdict can be obtained; *credibility* is how much a high score
  tells us about the task, and it falls as the judgment becomes gameable or
  self-serving. Grouping verifiers by who supplies the judgment (human,
  rule-based/formal, learned and pretrained, model-intrinsic) yields the
  central result: **credibility falls as availability rises, and there is no
  free checker**. It then asks what validates a verifier itself — agreement with
  human labels, the performance of the policy it trains, and behavior under
  reward hacking — and closes with nine metrics that make a verifier claim
  checkable, plus coordinates for verifiers still to be built. It also covers
  world-model evaluation and reward hacking, and its availability/credibility
  framing is directly usable when choosing or reporting a harness's verification
  layer.
- Boundary: survey and reading list only, no code or benchmark artifact
  (the MIT repository is a curated bibliography, not an implementation);
  survey conclusions are the authors' synthesis, not independent measurement.
- **Notable late arrival.** This paper was published 2026-09-08T13:52:59Z, i.e.
  *before* the previous run's newest visible stamp, but its abs page was 404
  then; it was recovered this run only because membership was diffed by ID. It
  is the concrete justification for the late-arrival methodology.

### RobustSGPO: Search-Space Control for Agent Harness Evolution

- Paper: https://arxiv.org/abs/2609.09646 (cs.AI)
- Classification: General Harness Methodology (harness self-evolution).
- Why included: harness optimizers usually treat the *edit proposal* as the
  object of study and leave the edit's scope and operation implicit. RobustSGPO
  makes search-space control explicit: it specifies the requested edit,
  constructs and checks the patch, and continues the search either from the
  incumbent or from a retained snapshot — so accepted mutations are executable
  and resumable instead of free-form rewrites. It then measures three
  search-space controls in the AgentX brainstorming workflow over 120 tasks, 95
  runs, and 7,350 candidate attempts: the authors report periodic `1→2→3`
  permission scheduling exceeding a fixed maximum permission by 0.28 test-score
  points, completion on 30 held-out tasks rising from 60.0% to 80.0% while test
  quality rises from 3.77 to 4.14 under a 20-million-token budget, and
  category-based snapshot retention reducing source-task degradation after a
  shift while random retention reaches a higher destination endpoint. The last
  pair is the useful negative-ish result: retention buys stability at a
  measurable overhead, and stability and destination performance trade off.
- Boundary: digital brainstorming agent only, **no robot experiment**; no
  official artifacts located as of 2026-09-10 (abstract and arXiv record carry
  no code/data link, and no matching repository was found); author-reported.

### Building the Harness Automatically: Self-Play in Code Distills a Text Harness for Black-Box Optimization

- Paper: https://arxiv.org/abs/2609.09468 (cs.LG)
- Classification: General Harness Methodology (harness discovery/distillation).
- Why included: it separates harness *discovery* from harness *deployment* in a
  way none of the curated harness-evolution entries do. An agent repeatedly
  writes and evaluates optimizer programs in code, then distills the program and
  its practice record once into a 197-word text harness that is frozen before
  evaluation — the shipped artifact is text, not code. The authors report the
  distilled harness reducing Gemini Flash regret by 48% in an independent N=30
  study (p<.001), entering the GP-BO performance range on the practice family,
  lowering mean regret on all three held-out BBOB landscapes, improving every
  tested Gemini executor, and transferring to Claude Sonnet (43% and 49% regret
  reductions, p≤.005); an independent end-to-end replication produced a
  different program and a different text at the same performance tier, and the
  framework also attains the lowest regret on a sealed production reward-tuning
  benchmark. The transferable claim is that executable practice can discover a
  search policy while language is the portability layer across executors and
  vendors.
- Boundary: digital black-box optimization only, **no robot experiment**; no
  official artifacts located as of 2026-09-10; author-reported.

### FolDeX: A Physical-World Benchmark for Long-Horizon Robotic Manipulation of Deformable Objects

- Paper: https://arxiv.org/abs/2609.10243 (cs.RO)
- Platform: https://ai.midea.com/#/fold-challenge (HTTP 200, Midea AI Research;
  client-rendered SPA — the URL returns the same application shell for every
  route, so platform content could not be verified by static fetch)
- Classification: Evaluation (real-robot benchmark); recovery/intervention data.
- Why included: it targets exactly the gap the curated benchmark list has —
  real-robot, long-horizon, deformable-object manipulation, where the abstract
  notes that simulation success degrades on hardware. More interestingly, it is
  organized as four **data-reuse axes** rather than a task list: reusing human
  intervention and recovery data collected during deployment; transferring data
  across tasks (including across garment categories and rigid-to-deformable);
  reusing data across scenes with lighting, background, and layout changes; and
  transferring across embodiments. The authors report 2,000+ hours of
  real-robot data spanning 20+ tasks and 10+ embodiments, plus a standardized
  external-policy submission platform with held-out physical objects, controlled
  initializations, and a unified execution protocol. Treating deployment-time
  recovery and intervention data as a first-class benchmark asset instead of
  discarding it is the transferable idea, and it complements the recovery-
  oriented benchmarks already on the list.
- Boundary: the platform is announced but **no dataset or code release was
  located as of 2026-09-10** (no repository or Hub link on the arXiv record or
  in the abstract), so the reported data assets are not yet open; the four-axis
  and 2,000-hour figures are author-reported and were not independently
  verified.

### JEPA Policy: Diffusion-Free Imitation Learning via Paired Action and Future Representation Prediction

- Paper: https://arxiv.org/abs/2609.09630 (cs.RO)
- Code: https://github.com/jiejie567/JEPA-Policy (MIT, verified live
  2026-09-10 via GitHub API: real repository, last push 2026-09-10T01:22Z;
  README documents robomimic, LIBERO, and MimicGen task support, Hydra configs,
  and ARX5 real-robot inference for JEPA Policy, MIP, and Diffusion Policy)
- Project: https://jiejie567.github.io/JEPA-Policy/ (live HTTP 200)
- Classification: VLA (open and reproducible); imitation-learning method.
- Why included: it is an open, artifact-backed answer to a specific design
  question in the VLA/imitation line — whether the future observation's
  representation should be a *paired training target* for the action chunk
  rather than an auxiliary head. Action and future-representation tokens
  interact in one shared Transformer and are refined through two forward passes,
  so future prediction shapes the representation used to generate actions,
  with dual-branch and gradient-routing controls attributing the gain to the
  shared topology. The authors report improved mean success over the action-only
  MIP baseline and over Diffusion Policy across nine simulated tasks at +0.29 ms
  model latency, a five-task 630-episode physical-robot study producing the same
  pooled ranking, no complete representation collapse under action supervision,
  and a task-conditioned failure-ranking signal in future-prediction error.
- Boundary: simulation and real-robot results are author-reported; the MIT
  release covers training/evaluation code and the real-robot inference path, and
  no independently reproduced numbers were located.

### GTA-2: A Multi-VLM Framework for Synthesizing Robot Manipulation Skills via Grounded Task Axes

- Paper: https://arxiv.org/abs/2609.09808 (cs.RO)
- Project: https://gta2-project.github.io/ (live HTTP 200)
- Classification: Planning/Tool Use/Skill Composition (code-as-policy family).
- Why included: it pushes the code-as-policy decomposition one level below fixed
  task-level primitives. A skill becomes an explicit *task-axis* structure —
  task-relevant keypoints and axes, controller compositions, and scene-dependent
  parameters — assembled by four specialized VLM agents that separately
  decompose the instruction, construct the abstract skill, assign controller
  parameters, and ground the visual features from RGB-D. Because the
  abstraction-to-grounding factorization keeps intermediate decisions
  inspectable, targeted human feedback repairs one wrong stage while correct
  components survive: correction stays local instead of triggering a full
  re-plan, which is the harness-relevant property. The authors report 73.9%
  average zero-shot success on 14 real-robot tasks with no task-specific
  demonstrations, policy training, or fine-tuning, versus two Code-as-Policies
  baselines and π0.5 (31.4 percentage points above the strongest baseline),
  rising to 90.7% with targeted refinement.
- Boundary: real-robot evidence, author-reported. The project page lists both
  code and an extended report as **"Coming Soon"** as of 2026-09-10 — treated
  literally as **not open**; no repository was located.

### HaWMPO: Hallucination-Aware World Model-based Policy Optimization for Generalist Robot Policy

- Paper: https://arxiv.org/abs/2609.09941 (cs.RO)
- Classification: Robot Foundation/World Model (world-model-based policy
  post-training); VLA-adjacent.
- Why included: it names and attacks the failure mode that limits world-model
  post-training of VLA policies — over long horizons imagined rollouts drift into
  prediction hallucinations, and the biased state transitions then mislead the
  policy update. HaWMPO adds an action-conditioned hallucination-aware model
  that estimates the reliability of generated image sequences and folds those
  scores into group-relative policy optimization through a Reward-Soft
  mechanism, suppressing unreliable action chunks during training instead of
  trusting every imagined rollout equally. The transferable part is the
  interface: an explicit "is this imagined future trustworthy?" signal placed
  *inside* the training loop rather than applied as an offline filter. The
  authors report the best average LIBERO success rate (+15.0 points over the
  base policy, +2.8 over the strongest baseline) and, on a G1 humanoid, average
  real-robot success on two manipulation tasks rising from 67.5% to 80.0%. It
  sits next to the curated world-model-reliability line (CaliBench's calibration
  scoring, Onto-EV-WM's verification-gated correction) as the in-loop training
  counterpart.
- Boundary: simulation plus limited real-robot evidence (two tasks, one
  platform), author-reported; **no official artifacts located as of
  2026-09-10**.

## Watch list (new candidates, rechecks)

- **Programmable World Model** (`2609.10540`, cs.CV) — decouples explicit
  persistent world state from generative rendering: a coding agent writes
  entity/transition programs, a state executor maintains canonical state
  including off-screen entities, and state-augmented 3D OBBs are deterministically
  compiled into pixel-aligned conditioning for a pretrained video model; a
  CombatStateBench accompanies it (94% count / 98% state accuracy,
  author-reported). **Treated literally as not open**: the GitHub repository
  (AlayaLab/PWM, 72 stars) contains only `README.md` and `assets/`, with no
  branches, tags, or releases, and its roadmap shows "Inference code" and
  "Pretrained weights" unchecked; the repo's commit history is README/badge
  edits only. Non-robot (playable-game) domain. Retained on the watch list —
  the state/rendering split is a genuinely reusable interface, so promote if
  code/weights land.
- **DUET-DINO** (`2609.10506`, cs.RO) — simultaneous cross-view latent world
  model (side + wrist camera) for full 7-DoF end-effector latent planning;
  trained from scratch on DROID and RoboArena, 92%/72.5%/60.0% on
  reach/angled-reach/lift, with a V-JEPA 2 vs DINOv3 wrist-prediction
  comparison. Project page live; paper says code and checkpoints "will be
  open-sourced" — **not open as of 2026-09-10**.
- **GALATEA** (`2609.10050`, cs.RO) — grounds generated hand-object-interaction
  video plans in simulation to train dexterous controllers (1,500+ videos
  grounded; real-world functional grasps and non-prehensile manipulation).
  The linked repository (boyuan-an/GALATEA) resolves to a `gh-pages` **project
  page** (figures, static assets, videos, `index.html`) with no license and no
  policy code; "Videos and code are available" therefore does not mean released
  code. Watch.
- **RoboDrop** (`2609.10021`, cs.RO) — training-trajectory-aware supervision
  auditing for VLA post-training: per-sample gradient compatibility against
  task-semantic and visually matched validation samples, aggregated to episode
  level, with real-robot rollout success rising from 35.0% to 67.5%. No
  artifacts located; fits no existing section cleanly (it is a curation method,
  not a dataset or format) — watch-lite, revisit if code or a curated-data
  release appears.
- **AXON** (`2609.10024`, cs.RO) — alternative ROS 2 RMW implementation with
  POSIX shared-memory rings on-host, QUIC off-host, and two fail-closed TLS 1.3
  key-establishment configurations (hybrid X25519MLKEM768, or a QKD-derived
  256-bit external PSK with per-message AES-256-GCM rotation). Distinctive for
  the curated Robot Middleware and Execution line, but it is a transport-layer
  contribution with an explicitly delimited validation scope and no located
  release — watch-lite.
- **VLA architecture modules without artifacts** — **Time-Frequency Geometric
  Cross-Attention** (`2609.09925`, cs.AI/cs.RO; drop-in chunked-VLA module,
  +6.3 LIBERO-Plus OOD and +11.67 points on three real AgiBot A2 tasks) and
  **FreqFM** (`2609.10405`, cs.RO; frequency-conditioned flow matching, +9.3 on
  LIBERO-Plus and six real-robot tasks). Both author-reported, no code located;
  watch-lite.
- **AgentAudit** (`2609.09875`, cs.AI) — attach-to-the-agent trust evaluation
  over recorded execution traces across ten capability/grounding/security/
  behavioural dimensions with failure attribution to a specific stage; reads
  only the trace, so it constrains no internal implementation. Abstract calls it
  "open, extensible" but **no repository or artifact link was located on the
  record as of 2026-09-10** — treated as not open. Watch-lite.
- **Digital-agent evaluation/measurement watch-lite:** **IBIB** (`2609.10494`,
  enterprise AI measurement by serving route rather than model identifier; the
  procedure is the artifact and the corpus stays sealed), **KVShareArena**
  (`2609.10266`, KV-cache reuse across contexts and checkpoints — multi-agent
  coordinator reports and RAG chunks are the motivating workloads),
  **MetroLLM-Bench** (`2609.10016`, kiosk-runtime tool-calling benchmark with
  code and data at a GitHub link), **VidHalLoc** (`2609.09895`, "Harness
  Engineering-informed" multi-agent data-construction workflow for video
  hallucination detection; HF dataset live), **Do Agents Know When They
  Succeed?** (`2609.09448`, zero-overhead reliability monitor from internal
  representations), **Black-Box Red Teaming of Agentic AI** (`2609.09647`),
  **MMPIBench** (`2609.09404`, multimodal prompt-injection benchmark across six
  agentic frameworks), **The Menu Is an Execution Prior** (`2609.09395`,
  state-path tool menus).
- **Digital-agent memory/recovery/verification watch-lite:** **RD-Forget /
  What Should an Agent Forget?** (`2609.10263`, separates what is stored from
  what is used, with a rate-distortion view of the answer-time memory view),
  **Procedural Memory Under Change** (`2609.09774`, mostly negative results on
  routine reuse under interface change), **Proof-Carrying Cognition**
  (`2609.09776`, verifier soundness as the exchange rate between test-time
  compute and capability; program-synthesis testbeds, not robots),
  **Belief-State Engine** (`2609.10036`, Bayesian posterior module outside the
  LLM with a sound-Markov-policy proof), **A-JIT** (`2609.10248`, runtime
  harness + embedded agent for self-evolving software; position/technical
  report).
- **Robot-side watch-lite:** **CT-SAFR** (`2609.09692`, multi-layered
  Chain-of-Thought safety/faithfulness verification for robots; 94.2%
  hallucination detection and 87% unsafe-output reduction in a warehouse case
  study, but a 7-page conference paper), **ViBe** (`2609.09918`, perceptive
  humanoid whole-body control via low-rank adaptation of a motion tracker),
  **InstantMimic** (`2609.09821`, GPU-native physics-skill training in seconds
  with LLM-agent hyperparameter search), **Compact Visuotactile World Models**
  (`2609.09597`, explicitly simulation-only with no demonstrated sim-to-real
  transfer — its own boundary statement), **Semigroup-JEPA** (`2609.10464`,
  latent dynamics consistency for zero-shot physics generalization),
  **Seven Sources of Physical AI Capability Formation** (`2609.09627`,
  capability-formation taxonomy; 49 evidence records, no artifacts),
  **TRACE** (`2609.10297`, irreversible visual-token admission for GUI agents).
- **TANGO** (`2609.09158`) — boundary entry already assessed by the 09-09 run
  (whole-body 29-DoF VLA navigation, simulation-trained, deployed zero-shot on
  a Unitree G1 but with no artifacts located). No revision in this batch; watch
  status retained.
- **Rechecked and unchanged** (revision present but metadata/claims unchanged,
  or no new release located as of 2026-09-10): RoboReel / Monkey See, Can
  Monkey Do? (`2609.08209` v2 — project page still exposes no code or dataset
  repository), Harbor Adapters (`2609.04298` v2), VLA-Precision (`2609.04355`
  v2), Beyond Prompts (`2609.05736` v2), Bit-Flip Attacks on VLA (`2608.15475`
  v3), MetaRSI/RSI2 (`2609.06396` v2), PGMT (`2609.08511` v2), Dex-X
  (`2609.07747` v2), EMERGE-Policy (`2608.29896` v2), LightNav-0
  (`2608.30935` v2), Sumo (`2604.08508` v3), RubricRefine (`2605.09730` v5),
  StateVLM (`2605.03927` v3), MOSAIC (`2603.01260` v2), Spec-Harness
  (`2604.00280` v2), SpecBench (`2605.21384` v2), Running the Gauntlet
  (`2606.14397` v3), FrogNano (`2609.07925` v2), FiberTune (`2606.08653` v2).
  The standing watch list from the 09-09 record (StageWAM, ReflexVLA,
  DreamX-Phi, UniTexture, PRISM, GigaBrain-0.7/WBC, ForceU-VLA, LIBERO-VIFO,
  Agent Lightning, VLCP, Hydra-0, BATON, Q-Planning, AutoSaddler, JIT-Agent,
  UCAG-P, TemporalFlow-VLA, PredVLA, FlashVLA, WikiSkill, RedEvoAgent,
  SKILL.state, Agent Mesh, WALL-SS, R2M-Bench, INTENT-AS-A-TOOL, BTS-AgentBench,
  TraceBench, GraphMemix, UrbanGround, LM-X, Zero-WAM, VLAct, Code as Worlds,
  Aero Hand Open, CAITLYN, Dogwood, LongGuard, WebWorld, ASPIRE, S3Gym,
  StudyBench, WorldReward, Principia, Statebench, Puffin-World, FailureSpot,
  FailSAE, Spectral-Target JEPA) is unchanged — none of its members received a
  scope-relevant revision in this window.

## Exclusions in the screened ranges (per policy)

- The five target categories' cross-lists include many entries whose primary
  category lies outside the five (cs.CR, cs.SE, cs.NI, cs.HC, eess.*, math/stat/
  physics, q-bio, astro-ph, quant-ph, etc.); every such title was scanned and
  scope-relevant abstracts read. Domain-excluded this window: medical/clinical
  (Myocardial Strain Drift Correction `2609.09577`, LightMedSeg-ISLES
  `2609.09634`, MedDeID `2609.10049`, OmniMed-FL `2609.10364`, polyp
  segmentation reliability `2609.10495`, laminographic X-ray nanoimaging
  `2609.10456`, brain-taskonomy fMRI `2609.10518`, SA-Profile `2609.10125`,
  morphological malocclusion classification `2609.09801`, CT synthesis
  `2609.09920`, EEG/epileptiform binding `2609.09728`, gait and keypoint
  interpolation `2609.09670`, UPDRS-gait challenge solution `2609.10187`),
  driving/ADS and vehicle fleets (risk fields for end-to-end driving
  `2609.10377`, odometer-agnostic drift correction `2609.10336`, IMU moving
  horizon estimation `2609.10202`, driver gaze `2609.10139`), aerial/UAV/UGV and
  maritime ops (UAV target following `2609.10166`, UAV wildfire MARL
  `2609.10433`, maritime trajectory prediction `2609.09840`, AGV routing
  `2609.09752`, traffic management `2609.10400`), perception/estimation/SLAM/
  planning without an agent contract (camera-pose view-graph learning
  `2609.09491`, RoMa-Ω `2609.09507`, camera intrinsic calibration `2609.10082`,
  LiDAR diffusion `2609.10322`, remote-sensing detection and segmentation
  `2609.09626`/`2609.10156`/`2609.10371`, hyperspectral DR `2609.10334`,
  PccDiffuser continuum-robot planning `2609.09745`, symmetry in learned motion
  planners `2609.10033`, tactile/vision hardware `2609.09612` MuJoCable and
  `2609.10230` CougarTail), teleop/HRI shared control without a reusable harness
  contract (`2609.10215`, `2609.10339`), graphics/simulation systems without a
  robot-agent interface (InstantMimic `2609.09821` and RealSimLoop `2609.09828`
  are graphics-venue systems; kept as watch-lite rather than included), and the
  many cs.CL/cs.LG language-model, optimization, speech, retrieval, graph,
  quantum, physics, and domain-science papers that touch none of the curated
  concepts. Driving, medical, video-generation, perception-only, and
  conventional-control exclusions follow the standing policy.
- **Late arrivals (09-08 block, invisible to every prior run, screened this
  run):** of the 15, thirteen are plainly out of scope — post-training
  ternarisation scaling (`2609.09240`), distribution-consistent SMoE inference
  (`2609.09241`), code-comment effects on generation (`2609.09242`), RAG
  document poisoning (`2609.09243`), critical-initialization theory
  (`2609.09244`), what fixed-rollout pass@k identifies (`2609.09245`),
  geophysical drill targeting (`2609.09246`), temperature downscaling
  (`2609.09247`), FPGA LUT training (`2609.09254`), approximate Cholesky
  (`2609.09255`), sensor-AI evaluation under distribution shift in underground
  mines (`2609.09257`), tensor-network moral-graph recovery (`2609.09258`),
  speech gender attribution (`2609.09263`), and a Lean stochastic-processes
  benchmark (`2609.09264`). One, **No Free Checker** (`2609.09250`), is a
  README inclusion above.
- The 09-07/09-08/09-09 records' "unchanged batch" re-screen items and
  exclusions were re-verified where they reappear in this batch's cross-lists;
  nothing previously excluded now qualifies.

## Operational notes

- SSH: `git ls-remote` failed initially with the known OpenSSH
  `/etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf` ownership error;
  `GIT_SSH_COMMAND='ssh -F /dev/null'` works (user key and known_hosts
  unaffected).
- arXiv API called over HTTPS; all queries HTTP 200. `/tmp` is **not persistent
  across tool invocations in this environment**, so scratch XML lives under
  `.scratch/arxiv-2026-09-10/` (removed before staging). The API index is the
  authoritative surface; the max visible ID advanced
  `2609.09158`→`2609.10540` and the newest stamp to `2026-09-09T17:59:32Z`.
- A plain `submittedDate`-range query cannot see replacement versions of papers
  submitted before the range, and the API's `lastUpdatedDate` filter behaved
  like `submittedDate` when used as a range (a 2026-09-01 range returned entries
  whose `<updated>` was 2026-09-09). Revisions were therefore captured by
  querying a broad submitted-date window sorted by `lastUpdatedDate` descending
  and keeping the in-window head; returned sets were verified strictly
  descending with no in-window entry cut off by the result cap.
- Working tree before the run: `docs/reference-architecture.md` modified,
  `docs/ring-harness.png` and `handoff.md` untracked — preserved untouched
  (never staged or committed). Scratch data under `.scratch/arxiv-2026-09-10/`
  removed after the run.
- Consistency: README header badge and Current Landscape "Last verified" both
  updated to 2026-09-10; entries added to General Harness Design and
  Self-Improvement (RobustSGPO, Building the Harness Automatically), Agentic
  Robot and VLA Harnesses (Show-Harness), Planning/Tool Use/Skill Composition
  (GTA-2), VLA Open and Reproducible table (JEPA Policy), World and
  Physical-Reasoning Models (HaWMPO), Benchmarks/Manipulation and VLA (FolDeX),
  and Surveys and Reading Lists (No Free Checker); five Current Landscape
  bullets added; `git diff --check` clean; VLA table rows all carry the same
  seven fields; every added link returns HTTP 200.
