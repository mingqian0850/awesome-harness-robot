# Daily arXiv scan — 2026-09-03

## Scope

- Interval: since the 2026-09-02 run's cutoff (~2026-09-02 04:00 UTC) through
  2026-09-03 ~06:45 UTC (commit `92064ec`, "Curate September 2 robot harness
  research"). No missed dates.
- The arXiv export API (`https://export.arxiv.org/api/query`, HTTPS) was
  healthy but heavily rate-limited throughout the run: persistent 429
  ("Rate exceeded.", ~14-byte bodies) and intermittent 503s, resolved with
  60 s retry backoff and 30–45 s spacing between queries. XML saved under the
  workspace scratch dir (`.scratch/arxiv-2026-09-03/`, removed after the run).
- **Operational discovery (important for future runs):** a combined query of
  the form `cat:cs.RO OR cat:cs.AI OR … OR cat:cs.LG AND submittedDate:[…]`
  does **not** constrain the date for the OR'd categories — arXiv binds `AND`
  tighter than `OR`, so only the last term receives the date filter and the
  response caps at 3000 mixed-date entries. Day-scoped growth rechecks must
  parenthesize: `(cat:cs.RO OR cat:cs.AI OR cat:cs.CL OR cat:cs.CV OR
  cat:cs.LG) AND submittedDate:[…]` (verified: parenthesized `[20260901]`
  returns exactly the day's 543 entries). Earlier records' per-day numbers
  from unparenthesized queries should be read as client-side day counts of a
  desc-sorted capped response (complete for recent days).
- Main interval query, per category over
  `submittedDate:[20260902000000 TO 20260903235959]` (cs.RO, cs.AI, cs.CL,
  cs.CV, cs.LG): cs.RO 32, cs.AI 118, cs.CL 74, cs.CV 103, cs.LG 111 →
  **323 unique entries, all v1, all submitted 2026-09-02, 0 revisions**
  (`published == updated`). `[20260902]` is exposed for the first time (this
  run; announced 2026-09-03 00:00 UTC); no 09-03 submissions visible yet
  (normal announce lag). Fully screened here.
- Growth rechecks (parenthesized combined five-category OR per day, per the
  09-02 record's instruction to re-check `[20260902]` plus growth in
  `[20260901]`/`[20260831]`/`[20260830]`):
  - `[20260901]` **grew from 447 to 543 unique (+96)**. The 09-02 record's
    last screened ID for the day was `2609.01603`; the not-previously-screened
    high-ID tail (`2609.01604`–`2609.01952` + the `2609.02654` outlier, 98
    entries) was fully screened here. Low-ID remainder was fully screened on
    09-02. No qualify for inclusion; watch items below.
  - `[20260831]` **grew from 567 to 574 unique (+7)** — the late
    `2609.01649`–`2609.01663` block announced today; all seven screened
    (PACT `2609.01662` included; How Fast Do Agents Rot? `2609.01660`
    watch-listed; the rest domain-excluded).
  - `[20260830]` **grew from 260 to 261 unique (+1)** — `2609.01649`
    (live-streaming moderation; domain-excluded; nothing to screen).
- Revisions in the rechecked days: `[20260901]` 17 v2, `[20260831]` 18 v2
  (12 known at the 09-02 run), `[20260830]` 10 v2 (7 known) — all minor v2
  updates dated 2026-09-01/09-02 (mostly camera-ready/author fixes); none
  dated 09-03. Watch items re-flagged by revisions, unchanged in curation
  status: SAGE `2609.01567` v2, When History Is Multimodal `2608.29897` v2,
  Will the User Ever Know `2608.30362` v2, Lazy Grounding `2608.30303` v2,
  Arkios `2608.30092` v2.
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action models,
  code-as-policy, skill discovery, memory, recovery, evaluation, runtime
  monitoring, and safety. Candidate claims were checked against the arXiv
  record, official project pages, and official code/model/data repositories
  (live HTTP, GitHub `ls-remote`/shallow clones, and Hugging Face checks on
  2026-09-03); README, landscape, and source records were checked for
  duplicates (no name/ID collisions; see the SafeEvolve disambiguation
  below). All performance results remain author-reported unless stated
  otherwise.

## Included (new curated entries)

### SafeEvolve: Harness-Policy Co-Evolution from Agent Experience for Safety Alignment

- Paper: https://arxiv.org/abs/2609.02786
- Code: https://github.com/MaoPopovich/SafeEvolve (live 2026-09-03, HEAD
  `94dc369`, MIT)
- Classification: General Harness Methodology / Evaluation-Safety
  (safety-directed harness–policy co-evolution).
- Why included: closes the co-evolution loop on the safety axis. On-policy
  trajectory evidence grouped by risk and task metadata is distilled into
  bounded, component-level harness artifacts — a global safety prompt and a
  hierarchical SkillBank — admitted only after safety and utility gates
  (auditable and reversible); the policy side then runs harness-use SFT
  followed by harness-augmented GRPO with verifier-decomposed rewards, so the
  policy actively leverages the evolved harness and its use generates the
  next round's evidence. On agentic safety benchmarks the authors report a
  3× attack-success-rate reduction for Qwen3.5-4B on AgentDojo while benign
  utility rises from 59.79% to 61.86% — a co-evolution result that keeps
  utility in the objective, unlike pure harness-side safety hardening (SHE)
  or pure policy-side alignment. A real artifact release (MIT) in the
  model–harness co-evolution line (WHALE, HELIX, Task-CoEvolve) crossed with
  the safety-harness line (SHE, HarnessRisk).
- Boundary: digital agents, no robot experiment; results author-reported
  (code verified live; contents beyond HEAD not audited in detail).
- **Name disambiguation:** the README's SkillMisevo entry (2608.12851,
  MisEvolve project, different author team) also calls its write-time
  governance defense "SafeEvolve"; that wrapper audits/repairs candidate
  skills at admission and governs reuse, and is unrelated to this framework.
  The two are distinguished in the README text.

### Not All Agreement Counts as Corroboration: Provenance-Conserving Multi-View Fusion for Typed Action Admission in Human-Robot Collaboration

- Paper: https://arxiv.org/abs/2609.01662
- Code: https://github.com/ZekaiJ/PACT (live 2026-09-03, HEAD `310422a`,
  MIT software / CC BY 4.0 research materials, tests included)
- Classification: Evaluation/Safety (provenance-aware typed action
  admission).
- Why included: for embodied systems, predictive agreement alone does not
  determine whether evidence warrants action — repeated inference over one
  observation multiplies agreement without adding evidence. PACT treats
  evidence countability as a relational variable: a supplied provenance
  partition defines countable units, coordinatewise support is retained
  within each unit and accumulated only across units, and unmet release
  conditions map to hold, confirm, or fallback. Under the stated assumptions
  source-local values cannot identify countability, and the coordinatewise
  meet is the greatest budget satisfying singleton fidelity and insertion
  non-amplification (with coarsening monotonicity and fixed-partition
  stability). Across 31,200 evaluations in 48 scene clusters PACT attains a
  common-support normalized risk-coverage area (ncsAURC) of 0.0861 and,
  excluding the constructed adversarial-consensus arm, provenance-partition
  aggregation cuts ncsAURC by 0.0557 relative to singleton aggregation; in
  offline human-robot collaboration, eightfold within-camera duplication
  leaves 720 typed responses per checkpoint unchanged while camera-grouped
  PACT admits 47 of 57 Qwen3-VL-32B reference-consistent candidates with no
  observed reference-inconsistent admission across 60 episodes. A robot-side
  instantiation of the typed-admission/evidence-ownership contract (One Gate
  Is Not Enough, SARA, Bounded Agents, The Verification Gap); real artifact
  release with tests.
- Boundary: evidence is recorded human-robot-collaboration episodes and
  synthetic scene evaluations (no live closed-loop robot deployment
  reported); results author-reported.

### AnyWorld: Factorized Egocentric World Models for Cross-Embodiment Generalization (promoted from watch list)

- Paper: https://arxiv.org/abs/2608.29242
- Project page: https://xpeng-robotics.github.io/anyworld (live 2026-09-03;
  404 on 2026-09-02)
- Code: https://github.com/xpeng-robotics/AnyWorld (live 2026-09-03, HEAD
  `2ed9d9c`, Apache-2.0)
- Classification: Robot Foundation/World Model (factorized cross-embodiment
  egocentric world model).
- Why included: expands a single human interaction into diverse robot-native
  rollouts without paired human–robot clips: factorized controls
  independently steer action, camera, embodiment, and scene/object context,
  so interaction structure is preserved while its realization is recomposed
  across robot bodies, viewpoints, lighting, and environments. The same
  generator serves broad experience scaling and targeted intervention —
  constructing missing robot-native states plus matched counterfactual
  instruction–action pairs for policy correction — and an embodiment editor
  grounds human egocentric video to target embodiments via reverse
  pseudo-pair construction. The code release covers embodiment-editor
  training/inference and factorized world-model inference with scripts and
  geometry/weight-layout docs; behavior gains on a physical IRON robot are
  reported on the project page. The cross-embodiment world-model line
  (CLAP, XEWorld, ZimaBlue) gains a real code release; watch item promoted.
- Boundary: adapted weights are intentionally not bundled in the repository
  — the expected layout over Qwen-Image-Edit-2511 and Wan2.1 bases is
  documented (`docs/MODEL_WEIGHTS.md`) and no public weight-download link was
  verified on 2026-09-03; results author-reported (code/page verified live).

### World-Coherent Decoding: Self-Verifying Test-Time Planning for World Action Models

- Paper: https://arxiv.org/abs/2609.02159
- Classification: Agentic Robot/VLA Harness (external self-verification
  runtime over frozen WAMs).
- Why included: empirical observation that WAM control results depend
  strongly on which stochastically generated visual future is selected is
  turned into a harness contract: WCD treats WAM rollouts as falsifiable
  future–action hypotheses, samples multiple candidates from the frozen WAM
  at each decision step, ranks them with internal generative signals
  (flow-based video surprisal for visual plausibility, action-path effort
  for action-generation stability), and after execution audits the selected
  imagination against the realized observation — the imagination–reality
  mismatch trains a lightweight online predictor that converts delayed
  self-verification into pre-execution reliability estimation without
  updating the backbone. On RoboTwin 2.0 the authors report Hard success
  rising from 55.80% to 60.90% under limited randomized-scene supervision,
  with a +16.43-point gain on Horizon-3 tasks. An external-runtime
  contribution to the frozen-WAM test-time-imagination line (GlanceWAM,
  RIFT, CheckVLA, SCOPE).
- Boundary: evidence is RoboTwin 2.0 simulation only; results
  author-reported; no official artifacts located as of 2026-09-03 (no code,
  project page, or weight link on the arXiv record).

### WHALE (artifact-status update to existing entry)

- Paper: https://arxiv.org/abs/2609.00196 (curated 2026-09-02)
- Code: https://github.com/krafton-ai/WHALE — **now live** (HEAD `1b29341`,
  Apache-2.0, reproduction code for search QA / math reasoning / chess
  domains, docs, `run/` launchers; README matches the paper). Returned 404 on
  2026-09-02 and was recorded as "code not open"; per policy the literal
  404 status is now reversed by a verified live release. README entry updated
  accordingly (no re-classification of the paper itself).

## Reviewed but not promoted (watch list)

From `[20260902]` (first exposure):

- **SA-WAM** (`2609.02531`) — Spatially Aware WAM repurposing a pretrained
  video model for joint action/RGB/depth prediction via a nonlinear encoding
  of unbounded depth into the frozen VAE's input domain (no 3D fine-tuning);
  SOTA on RoboCasa and LIBERO-Plus, real-UR5 gains in randomized
  environments, plus an analysis of world-model prediction quality vs
  rollout success. WAM model-architecture contribution without a reusable
  external contract and without official artifacts; watch for release.
- **ZETA** (`2609.02546`) — controlled zero-shot cross-embodiment VLA
  transfer study: strict vs pretrain-exposed transfer definitions, a
  14-embodiment benchmark (sim + real validation), and factor analysis
  (EEF representations +15, source embodiment diversity +18, co-training +7,
  5% target exposure +13.4 points). Useful evaluation contract; no artifacts
  located; also a near-name collision with the curated Zetta ζ entry —
  watch, flag in README if promoted later.
- **HINT** (`2609.02653`) — agentic framework invoking semantic reasoning
  only at manipulation-pattern transitions and communicating tracked intent
  to frozen foundation action policies through image-space semantic
  highlighting and attention-prior injection (no new trainable parameters in
  the policy); three long-horizon tasks + OOD variants across two foundation
  policies. Project page live (robot-hint.github.io) but no code release;
  watch.
- **Do Better Imagined Rollouts Mean Better Robot Control?** (`2609.02811`)
  — controlled study of world-model evaluation under feedback: trajectory
  replay correlates more strongly with closed-loop cross-track error than
  20-step rollouts (ρ=0.923 vs 0.774) and picks a different estimator in
  5/24 vs 18/24 conditions; horizon/update-grid analysis shows long rollouts
  without correction mis-rank closed-loop behavior. Evaluation-methodology
  line for WAMs; differential-drive domain, no artifacts; watch.
- **Modeling What Changes** (`2609.02046`) — sparse residual world models
  with per-object change gates (MuJoCo tabletop pushing: 2.5–4.6× more
  accurate next-state prediction at 8.6–11.1× fewer parameters, zero-retrain
  object-count transfer, planner-useful featurization). Simulation-only WM
  training method; watch.
- **Safe-Stop** (`2609.02358`) — emergency-stop as reach-avoid with learned
  stoppability estimators and a fall-policy handoff agreement check;
  humanoid control-level safety, no artifacts; watch.
- **A Physics-Consistent Benchmark for Contact-Rich HRI in Assistive Care**
  (`2609.02402`) — deformable responsive human, physics-aware scores,
  frozen vision-only/scorer-only protocol (ROBIO 2026); bathing domain;
  results: LLM state machine 72.9%→56.4% after safety screening, VoxPoser
  27.9%, zero-shot pi0.5 0.7%; watch.
- **LAVLA** (`2609.02634`) — latent cluster analysis of GR00T N1.5 action
  decoding with cross-attention embedding weighting; interpretability
  framework; watch.
- **Discriminative World Models for Web Agents** (`2609.02885`) —
  predicted-state matching objective for web-agent world models; digital;
  watch.
- **LLM-as-a-Judge Is Not an Oracle** (`2609.02246`) — position backed by
  months of production prompt-optimization loops: eleven ways the evaluation
  signal fails (judge bias, harness/metric failures, ground-truth errors,
  reward hacking — cached answer keys read from the environment gave 100%
  pass rate over 68% true capability); argues deterministic verification
  layers the LLM judge cannot override. Digital; watch (position-with-field-
  evidence; no artifacts).
- **Improving Evaluation Realism with Inference-Time Compute and Deployment
  Scaffolds** (`2609.02302`) — critique refinement + DISH
  (Deployment-Imitating SWE-Agent Harness) to make alignment evaluations
  harder to distinguish from deployment; code live (`meridianlabs-ai/
  petri_dish`, `AxelAhlqvist1995/petri-bon`); digital eval realism; watch.
- **SkillGLoW** (`2609.02217`) — procedural-family skill consolidation
  (global priors + per-task detail regeneration, commit gate on real
  execution; +17.2 points across math/terminal/software-repair/embodied
  control); watch.
- **MASkills** (`2609.02094`, code `DaRL-GenAI/MASkills` live) — continual
  skills optimization for multi-agent LLM systems; watch.
- **CHIME** (`2609.02074`) — credit-aware hierarchical memory evolution
  (plan vs execution banks, attribute-before-memorize); watch.
- **Monitoring Web Agents Without Internal Signals** (`2609.02057`) —
  prefix-level risk prediction from observable trajectories with key-step
  supervision; watch.
- **ClaimReceipt** (`2609.01992`) — claim-relative receipt specification +
  selective verifier for agent evaluations (signed manifests, PASS/INVALID/
  INCONCLUSIVE); digital audit; watch.
- **EarlyEval** (`2609.02783`, code `inphotoo/earlyeval` live) — early
  outcome prediction to halt agent runs early (LightGBM success/failure
  classifiers); watch.
- **SolarWM** (`2609.02886`) — fully open video-world-model stack (1.43M
  canonical clips from 10 datasets, 4 models 5B–33B on Wan2.2/LTX-2.5/
  MiniMax-H3); video-domain world models, no robot experiment; watch.
- **FUSE** (`2609.02168`) — modular dangerous-capability evaluation
  framework (Knowledge/Defense/Harm pipelines, 12 commercial LLMs); watch.
- **READY or Not** (`2609.02095`) — reliability qualification framework for
  enterprise agent deployment ("open testbed" claimed, no link verified);
  watch.
- **AGENTSCOPE** (`2609.02371`) — neuro-symbolic agent failure diagnosis via
  behavioral abstractions and neural invariants; watch.
- **Repo-To-Skill / DisCo** (`2609.02749`) — operational-knowledge
  distillation into verified skills (AREX-Skill Library, 5,000+ skills);
  watch.
- **When Agents Implement Systems** (`2609.01985`) — coding-agent systems
  case study with defect catalog; watch (case-study scope).
- **Coverage, Not Targeting** (`2609.02417`) — structural regime in
  multi-turn agent credit assignment; watch.
- **Act More, Decide Less** (`2609.02042`) — skill-guided adaptive action
  chunking for long-horizon LLM agents; watch (digital analogue of the
  Knowing-When-to-Stop VLA item).
- **Cliff** (`2609.02817`) — process rewards from the first mistake; RLVR;
  watch.
- **CAPTURE** (`2609.02265`) — disentangling preference drift from memory
  poisoning in personalized agents; watch.
- **PGPO / APEx / Codebook Agent / Beyond Outcome Gaps (EMNLP) /
  Measurement-Driven Sub-Network Selection / ExecRetrieval / UTP-Bench /
  CivBench-class game-domain items** — digital/game/enterprise agents
  without reusable contracts; watch or domain-excluded as above.

From the `[20260901]` growth tail (+96, IDs `2609.01604`–`2609.01952`):

- **HEART / Tool Primitives** (`2609.01736`) — harness engineering for tool
  use: natural-language tool interface replacing rigid API schemas, ToolFace
  repository of 25,519 wrapped functions with dynamic retrieval, Planner/
  Router/Verifier orchestration (+10% over SFT models, −85% API cost on five
  benchmarks). Digital; tool-layer harness; no artifacts located; watch.
- **Endogenous Authorization Laundering / EAL-Bench** (`2609.01836`) —
  persistent memory as an authorization surface: spurious permissions
  written into memory let agents act without the history ever granting them
  (false authority on up to 50.2% of unauthorized requests under incremental
  updates; executors act on it 98.6% of the time); source-event backing and
  bounded event sourcing reduce but do not eliminate laundering (safety–
  utility tradeoff). Memory-authorization line; watch for release.
- **The Memory Trust Gap** (`2609.01852`) — stale stored facts overriding
  current evidence is capability-gated across a Qwen3 model-size series
  (Safety-suite harm collapses in larger models once a stale note looks
  current); mitigation is scale-dependent. Memory-evaluation line; watch.
- **Belief-Calibrated Optimization** (`2609.01861`) — writes the coding
  optimizer's implicit belief about environment responses into a persistent
  in-context world-model document, revised as candidates are evaluated;
  consistent held-out gains on five benchmarks and after target-model swaps.
  Scaffold-optimization line; no artifacts; watch.
- **DemoMimic** (`2609.01938`) — dexterous manipulation from human
  demonstrations via contact-local geometry rewards (71% success across 16
  objects, four tasks, two hand embodiments, smallest sim-to-real drop);
  policy method, no artifacts; watch.
- **Zeta-Lite** (`2609.01818`) — WebAssembly build of the Zeta SQL engine
  (2.87 MB) with snapshot-isolated concurrent transactions and copy-on-write
  branching for in-browser agentic memory; database-systems artifact, agent-
  memory framing thin; watch.
- **A Survey on Self-Improving Test-Time Intelligence** (`2609.01679`,
  accepted at Machine Intelligence Research) — unified TTI view across
  test-time adaptation/learning/scaling; general survey, robotics only one
  of several domains; watch (not added to Surveys — the section favors
  robot/embodied-focused surveys).
- **Retrieved but not ranked** (`2609.01556`, code `nabirarashid/
  structural-retrieval` live) — surface-form bias in structural retrieval
  from competition math to embodied-agent trajectories (below chance when
  gold differs in object/receptacle); retrieval-eval line; watch.
- **EDGE** (`2609.01360`) — error-dependency graphs for multi-error
  attribution in multi-agent systems; watch.

From the `[20260831]` growth (+7):

- **How Fast Do Agents Rot?** (`2609.01660`) — long-horizon degradation in
  LLM agents for production decision-making: success follows a geometric law
  governed by per-step reliability (saturating below 1 even for strong
  models); agentic tool-use task collapses within ~16 steps; degradation
  driven by step count, not context length (bounding context steepens decay);
  quantifies benchmark-vs-production gap (0.42 GAIA-length → 0.24 at
  hundred-step horizons). Horizon-aware evaluation argument for the
  reliability line; 10,664 analyzed trajectories, nine models; no artifacts;
  watch.
- **PACT** (`2609.01662`) — see Included.
- Remaining block entries (`2609.01655`, `2609.01657`, `2609.01658`,
  `2609.01659`, `2609.01663` and the `2609.0049x` stragglers) are
  recommendation/encoder/RLVR/driving-survey/security items; domain-excluded.

## Watch-list status (rechecks)

- **New artifact releases (verified 2026-09-03, promoted above):** WHALE
  (repo `krafton-ai/WHALE` live @ `1b29341`, Apache-2.0 — previously 404),
  AnyWorld (project page live; repo `xpeng-robotics/AnyWorld` live @
  `2ed9d9c`, Apache-2.0 — previously 404 page/no repo), SafeEvolve (repo
  `MaoPopovich/SafeEvolve` live @ `94dc369`, MIT), PACT (repo `ZekaiJ/PACT`
  live @ `310422a`, MIT/CC BY-4.0), EarlyEval (`inphotoo/earlyeval` live),
  MASkills (`DaRL-GenAI/MASkills` live), petri_dish/petri-bon (live),
  structural-retrieval (live).
- **Still absent (treated literally, not open):** Peg-in-Bench
  (`aistairc/peg-in-bench` still 404), CANOPY (`AlibabaResearch/
  SignalCoverageRL` still empty — no HEAD), HINT (page live, no code),
  World-Coherent Decoding (no artifacts), StageWAM, ReflexVLA/ReflexBench,
  DreamX-Phi, UniTexture, PRISM, GigaBrain-0.7/WBC, ForceU-VLA, LIBERO-VIFO,
  Agent Lightning, VLCP, Hydra-0, BATON, Q-Planning, AutoSaddler, JIT-Agent,
  Task-CoEvolve code, UCAG-P, TrapVLA code/benchmarks, TemporalFlow-VLA,
  PredVLA, FlashVLA, WikiSkill, RedEvoAgent, SKILL.state, Agent Mesh,
  WALL-SS, R2M-Bench, INTENT-AS-A-TOOL, BTS-AgentBench, TraceBench,
  GraphMemix, UrbanGround, LM-X, Zero-WAM, VLAct, Code as Worlds, Aero Hand
  Open, CAITLYN, Dogwood, LongGuard, WebWorld, ASPIRE, S3Gym, StudyBench/
  S3Gym-class — unchanged (spot-checked via `ls-remote`/HTTP on 2026-09-03
  where links exist; no arXiv revisions signal change).

## Domain exclusions in the screened ranges (per policy)

- Driving/CAV: CrashDiffuser (`2609.02270`), DiffuSearch (`2609.02252`),
  Proxy-to-Decision driving planners (`2609.02688`), transitional-vehicle
  lane-change (`2609.02575`), Zero-Shot Transfer Across Embodiments for
  Driving VLAs (`2609.02341`), Beyond Textual CoT driving survey
  (`2609.01659`), VIPS cooperative planning (`2609.02462`), Sim2Signal
  traffic control (`2609.01676`).
- Locomotion/control/perception/hardware: DemoMimic (watch), World-Model-
  Augmented Visual Locomotion (`2609.02542`), Safe-Stop (watch), MPPI
  torque-sampling (`2609.02020`), Koopman MPC (`2609.02079`), MS-MEM mapping
  (`2609.02493`), FOCUS odometry (`2609.02222`), lower-limb calibration
  (`2609.02306`), fisheye UAV platform (`2609.02319`), Mini-Girona I-AUV
  (`2609.02605`), space LiDAR segmentation (`2609.02830`), lunar instance
  segmentation (`2609.02219`), WildFab printing (`2609.02413`), knee
  prosthesis (`2609.02003`), reservoir computing (`2609.02157`), greenhouse
  navigation (`2609.02487`), MV-dVRK surgical perception (`2609.02717`),
  MACAW surgical (`2609.01961`), TriSAR aerial teams (`2609.01731`), MRJEPA/
  motion-retargeting (`2609.02134`).
- Medical/clinical/finance/energy: the usual cs.CV/cs.CL/cs.LG out-of-scope
  clusters in the screened ranges (pathology, radiology, EEG, speech/audio,
  retail/e-commerce, markets, power/outage forecasting, satellite/remote
  sensing, live-streaming moderation `2609.01649`, agriculture federated
  learning `2609.01667`).
- General LLM/agent papers without harness scope: CivBench-class game agents
  (`2609.02459`), UTP-Bench travel (`2609.02421`), OmegaUse-SOP (`2609.02149`),
  Efficient GUI Agents survey (`2609.02309`), PaperCompiler (`2609.02272`),
  Rendering-in-the-Loop web agents (`2609.02088`), most cs.CL/cs.CV items in
  range; retrieval/memory/quantization/efficiency items without agent-harness
  contracts.
- Image/video generation and perception: SolarWM (watch), most cs.CV
  entries in range (restoration, super-resolution, 3DGS, forgery/forensics,
  tracking, segmentation).

## Operational notes

- Rate limiting was severe at the start of the run (429/503 on first
  attempts); 60 s retry backoff and 30–45 s inter-query spacing completed 8
  parenthesized/category queries cleanly. The combined-OR precedence issue
  (see Scope) cost three 3000-entry capped responses before detection — the
  g2 parenthesized re-queries are authoritative for day counts.
- Watch-item artifact checks on 2026-09-03 (live HTTP/`ls-remote`/shallow
  clones): krafton-ai/WHALE (live @ `1b29341`, Apache-2.0), xpeng-robotics/
  AnyWorld (live @ `2ed9d9c`, Apache-2.0, weights external per
  `docs/MODEL_WEIGHTS.md`), MaoPopovich/SafeEvolve (live @ `94dc369`, MIT),
  ZekaiJ/PACT (live @ `310422a`, MIT/CC BY 4.0, tests), inphotoo/earlyeval
  (live), DaRL-GenAI/MASkills (live), meridianlabs-ai/petri_dish +
  AxelAhlqvist1995/petri-bon (live), nabirarashid/structural-retrieval
  (live), aistairc/peg-in-bench (404), AlibabaResearch/SignalCoverageRL
  (empty), xpeng-robotics.github.io/anyworld (200), robot-hint.github.io
  (200), junchao-cs.github.io/SolarWM-Web (200). SkillMisevo's "SafeEvolve"
  vs the new SafeEvolve paper were confirmed distinct (different author
  teams and system scope: MisEvolve governance wrapper vs harness–policy
  co-evolution framework).
- Scratch XML and cloned repos kept under `.scratch/arxiv-2026-09-03/`
  during the run and removed afterward (not committed).
- Working tree before the run: `docs/reference-architecture.md` modified,
  `docs/ring-harness.png` and `handoff.md` untracked — preserved untouched.
- Consistency: README header badge and Current Landscape "Last verified"
  both updated to 2026-09-03 (both were 2026-09-02 after the previous run).
