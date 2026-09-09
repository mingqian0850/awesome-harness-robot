# Daily arXiv scan — 2026-09-09

## Scope

- Interval: since the 2026-09-08 run's cutoff. The 09-08 record's scope ended
  at 2026-09-08 ~06:45 UTC with **zero newly indexed content** (the
  Sat/Sun/Mon/Tue submission backlog was still invisible; newest stamp then
  was 2026-09-04T17:59:35Z, max ID `2609.05416`). The missing backlog —
  the 09-08 record predicted it for the "next announcement (~00:00 UTC
  09-09)" — **landed between the 09-08 run's probes (which ended
  2026-09-08T17:51 UTC) and this run (~06:00 UTC 09-09)**: abs pages beyond
  `2609.05416` now resolve (probed `05417`–`09158`, all HTTP 200), and the
  API index is populated through ID `2609.09158`, newest stamp
  **2026-09-08T17:59:55Z**. Dates in scope: 2026-09-05 (Sat), 09-06 (Sun),
  09-07 (Mon), 09-08 (Tue), plus late-arriving members of the batch carrying
  earlier published stamps (09-01..09-04). Nothing dated 09-09 is indexed
  yet (the 09-09 block will land ~18:00 UTC today, next run's scope).
- **Methodology note on coverage.** The batch contains 16 entries with IDs
  `2609.05569`–`2609.05594` whose *published* stamps (2026-09-04T00:29Z ..
  17:45Z) fall **before** the previous run's newest visible stamp
  (09-04T17:59:35Z); a published-stamp filter alone would have wrongly
  classified them as covered. They were never visible to any prior run
  (abs pages returned 404 then). Per the 09-08 record's late-arrival
  methodology, membership was therefore diffed by **ID** against record
  coverage: everything with suffix > `05416` (the previous max visible ID)
  is new to the pipeline, regardless of its published stamp.
- **New content screened: 1229 unique entries** (IDs `2609.05417`–
  `2609.09158`) across the five target categories and their cross-lists,
  every one invisible to prior runs. Primary-category split of the five:
  cs.RO 125, cs.AI 183, cs.CL 162, cs.CV 296, cs.LG 251 (remainder are
  cross-listed entries whose primary category lies outside the five).
  Published-stamp spread: 09-01 ×7, 09-02 ×10, 09-03 ×6, 09-04 ×62
  (backlog tail), 09-05 ×203, 09-06 ×189, 09-07 ×403, 09-08 ×349. All
  1,229 titles were scanned; keyword-hit abstracts (~450) were read;
  **every cs.RO-carrying entry was individually accounted for** (the
  125 cs.RO-primary entries are listed exhaustively in the screening
  notes; candidates checked against the arXiv record, official project
  pages, and official code/model/data repositories with live HTTP / GitHub
  API / HF API checks on 2026-09-09). The `/list/{cat}/new` HTML pages were
  still serving the stale Monday block during this run (header and max ID
  `2609.05416` unchanged) while the API index and abs pages had advanced —
  another instance of the announcement/list pipeline lag documented in
  prior records; the API index is the authoritative surface.
- **Revisions.** **51 revision records with `updated` > 2026-09-04T17:59:35Z
  (the previous runs' newest visible stamp) became newly visible with this
  batch** — the replacement content of the Sat–Tue backlog that the 09-07/
  09-08 runs could not see (their replacement sweeps stopped at
  ~09-04T17:45Z). All 51 were title-screened; scope-relevant ones were
  abstract-checked against the README, watch list, and prior records. Only
  one touches a README entry: HookPry `2609.03884` v2 (updated
  2026-09-08T12:59:09Z) — a minor revision (identical title and abstract
  head, same 18-page comment, delta ~1 KB) that needs no README edit.
  Watch-list items among the set are substance-unchanged: HINT `2609.02653`
  v2 (project page folded into the abstract; page live; still no code
  located), StudyBench `2609.00787` v2 (same comment), Spectral-Target
  JEPA structuring `2609.04264` v2 ("minor typo fixes"). The remaining
  revision targets (e.g., ZETA `2609.02546`, TourPhysics `2609.04911`,
  WorldSculpt `2609.05416`, MINT `2609.04958`, Air-Ground VLN `2609.03483`,
  EraseSAE `2609.03629`, MedQA-MM `2609.03261`) are not README entries;
  they are cross-embodiment/memory, world-model, VLN, perception, or
  medical/domain content without a harness contract, or were already
  excluded at v1. All performance results are author-reported unless
  stated otherwise.
- **Unchanged-batch re-screen requirement.** The newest visible batch *is*
  changed relative to the previous run (IDs advanced `05416`→`09158`), so
  the full screening above is the required re-screen of the new content;
  no additional unchanged-batch pass was needed.

## Included (README updates)

### GE-Act 2.0: Pretraining and Scaling a World-Action Model for Robotic Manipulation

- Paper: https://arxiv.org/abs/2609.05588 (AgiBot Research Team technical
  report; first exposure to the curation pipeline — no prior GE-Act record
  exists in README or sources)
- Project: https://ge-act-v2.github.io/ (live HTTP 200, 2026-09-09;
  JS-rendered; no GitHub/ModelScope/HF release links found in the page
  HTML — related pages link AgiBot World, Genie Sim 2.0, GenieReasoner,
  Act2Goal only)
- Classification: Robot Foundation/World Model (world-action model
  pretraining); VLA-adjacent.
- Why included: a large-scale answer to the WAM-pretraining question the
  curated WAM line keeps hitting — most WAMs inherit pretrained video
  generators, so whether WAM components can be pretrained *from scratch*
  and how they scale was open. GE-Act 2.0 initializes all trainable
  generative and action components from scratch on manipulation data and
  decomposes the WAM into a control-oriented autoencoder (CoAE) that keeps
  action- and instruction-relevant information under aggressive
  compression, a single-step visual planner (SVP) producing a complete
  future state in one differentiable pass, and an inverse dynamics model
  (IDM); visual planning and inverse dynamics are pretrained separately on
  complementary data, then jointly trained with knowledge-aligned
  selective optimization (KASO), which keeps only predicted futures judged
  behaviorally compatible with the recorded action. Evaluated without
  per-task fine-tuning on 100 tasks across 20 manipulation skill groups
  with held-out scenes, backgrounds, lighting, and object instances: the
  authors report scaling co-training data from 300 to 30,000 hours raising
  success from 17.1% to 44.1% (G1-OP) and 13.4% to 31.1% (G2-90D), with
  G2-90D improving 17.7 points while comprising under 2% of co-training
  data (suggestive of cross-embodiment transfer), gains spanning 19/20 and
  18/20 skill groups, and skill-specific coverage strongly correlated with
  zero-shot OOD success (Pearson r=0.80; Spearman rho=0.85). It is the
  scaling/composition study counterpart to OpenWAM's controlled-factors
  program (same run, both included).
- Boundary: project page live but **no code or weights located as of
  2026-09-09 — treated literally as not open**; results author-reported.

### OpenWAM: An Open, Modular Exploration Towards Systematic World-Action Model Pretraining

- Paper: https://arxiv.org/abs/2609.07398 (cs.RO)
- Code: https://github.com/OpenWAM-Official/OpenWAM (Apache-2.0; verified
  live 2026-09-09: real package tree — `openwam/`, configs, benchmarks,
  scripts, tests, CI, pyproject — pushed 2026-09-09T03:15Z)
- Models & data: https://huggingface.co/OpenWAM (verified live 2026-09-09:
  OpenWAM-Alpha-Pretrain-Foundation-Model with `checkpoint_step_154000.
  safetensors`, plus real-robot checkpoints for dexterous hand Wuji,
  RoboDojo ARX-X5/Piper/Piper-X, Franka single arm, and sim checkpoints
  for EBench, LIBERO, RoboCasa-GR1/365)
- Project: https://openwam-official.github.io/ (live HTTP 200)
- Classification: Robot Foundation/World Model (world-action model
  pretraining stack + study).
- Why included (artifact release): the first open, modular substrate for
  world-action pretraining as a controlled experimental program.
  OpenWAM-Infra factorizes the WAM design space into composable modules
  with unified training, inference, deployment, and evaluation;
  OpenWAM-Study then runs controlled experiments over three questions —
  what to inherit, how world and action learning interact, and how their
  synergy scales — and distills three principles: upstream knowledge
  transfers through a sufficiently capable generative backbone plus a
  compact, information-rich latent space; world-action synergy requires
  dedicated action capacity, explicit world-to-action information flow,
  and synchronized joint denoising; and embodied pretraining principally
  improves out-of-domain generalization, with one-stage co-training over
  egocentric and robot data integrating world coverage and action
  grounding. Composing these, OpenWAM-α is pretrained on ~6,400 hours of
  egocentric human and robot data and evaluated across eight simulation
  benchmarks and real-robot experiments spanning single-arm, bimanual,
  and dexterous-hand embodiments. Infrastructure, evaluation protocols,
  pretrained models, and data recipes are all released — a genuinely open
  WAM line that most prior WAM entries lack.
- Boundary: results author-reported; real-robot evidence is the authors'
  own evaluation, not independent verification.

### NeoHorse-1: Towards Recursive Self-Improvement via Agentic Post-Training with Routing Harness

- Paper: https://arxiv.org/abs/2609.08183 (cs.CL; first exposure)
- Code: https://github.com/TokenRhythm/NeoHorse (Apache-2.0; verified live
  2026-09-09: technical report PDF, examples, assets; pushed
  2026-09-09T02:39Z)
- Models: https://hf.co/collections/TokenRhythm/neohorse-1 (NeoHorse-1-4B
  and NeoHorse-1-9B plus GGUF quantizations; verified live 2026-09-09)
- Classification: General Harness Methodology (self-improving harness /
  recursive self-improvement); digital agents, no robot experiment.
- Why included: a concrete RSI mechanism with a real routing harness and
  released artifacts. NeoHorse-1 combines a heterogeneous model pool with
  intelligent routing and records, per user turn, the predicted capability
  demand, the selected service tier, and the subsequent interaction; those
  records become training examples preserving interleaved reasoning, tool
  calls, and harness context, admitted through structural validation,
  six-dimensional semantic evaluation, and subscene-level labeling.
  Routing signals organize SFT into a three-stage curriculum and extend to
  routing-guided on-policy distillation (a teacher supervises
  student-generated responses under the same progression); capability-
  guided allocation then converts evaluation feedback into the next
  training mixture, closing an evaluation-selection-update loop. Across
  eleven benchmarks covering harness-based agents, tool use, coding, and
  instruction following, the authors report post-training raising the
  macro-average from 58.94 to 64.87 at 4B and from 65.60 to 69.04 at 9B,
  substantially narrowing the gap between the post-trained 4B model and
  the 9B base. Artifact-backed and squarely in the harness-mediated RSI
  line (Harness-R1, MetaRSI-family, SKILL.state watch items).
- Boundary: digital agents only; results author-reported.

### Co-Evolving Harnesses and Models: On-Policy Correction Helps Weaker Models Catch Up Where Imitation Fails

- Paper: https://arxiv.org/abs/2609.09134 (cs.AI; first exposure)
- Classification: General Harness Methodology (model–harness co-evolution);
  digital agents, no robot experiment.
- Why included: a clean, surprising negative result that constrains the
  co-evolution recipes the curated line has been accumulating (HELIX,
  WHALE, SafeEvolve, HCL). Across seven enterprise agent tasks the authors
  evolve a harness with a weaker model, then observe that a stronger
  expert often uses the evolved harness more effectively — suggesting
  expert supervision could close the gap — but training the weaker model
  on the expert's *complete* trajectories under the evolved harness
  **backfires: performance regresses on all seven tasks by 4 to 30 points**
  (Qwen3-Coder, Gemma 4), even though the identical procedure helps under
  the unevolved harness. The analysis attributes this to disrupted
  model–harness fit: imitation transfers knowledge and increases scaffold
  usage, but the weaker model adopts the expert's planning strategy without
  the competence to execute it and no longer matches a harness evolved
  around its native planning style. The fix — an on-policy
  expert-correction pipeline automated by a meta-level MLE agent that
  localizes the failing turn in the weaker model's own rollout and has the
  expert rewrite only that turn — preserves planning style and combines
  harness-evolution and weight-adaptation gains. A compatibility-preserving
  recipe for the model+harness optimization literature.
- Boundary: digital agents only; no official artifacts located on the arXiv
  record as of 2026-09-09; results author-reported.

### Beyond Prompts: Measuring and Optimizing LLM Tool-Agent Harnesses

- Paper: https://arxiv.org/abs/2609.05736 (EMNLP 2026; cs.AI; first
  exposure)
- Classification: General Harness Methodology (harness measurement and
  optimization protocol); Evaluation.
- Why included: the missing measurement contract for the fixed-model
  harness-optimization line (Same Model Different Harness, EnvHarness,
  HarnessOpt-Bench). The paper studies resource-bounded harness selection
  for fixed-model multi-turn tool agents with the search surface scoped to
  prompts and tool-boundary middleware (guarded intercepts, not arbitrary
  rewriting of execution logic), and defines an optimizer-agnostic protocol
  reporting mean held-out lift, worst-condition lift, repeatability, logged
  cost diagnostics, and RelLift95(B) — a conservative estimate of the
  held-out gain of the harness selected under budget B. It instantiates the
  protocol with prompt-only and prompt-plus-middleware optimizers,
  including PRISM, which clusters failures and routes repairs to prompt,
  tool-boundary middleware, or joint edit surfaces within a Pareto search.
  On BFCL multi-round, tau2-Retail, and tau2-Telecom, PRISM obtains mean
  held-out lifts of 14.2, 14.9, and 10.1 percentage points with positive
  empirical RelLift95 on all three benchmarks; an ablation attributes the
  margin chiefly to failure-surface routing and the edit-pattern
  constraint. The authors also show some search procedures can find large
  gains yet choose brittle updates — motivating reporting reliability of
  the chosen harness alongside average held-out lift.
- Boundary: digital tool agents, no robot experiment; no official code link
  on the arXiv record as of 2026-09-09; results author-reported.

### SimpleMemVLA: A Simple but Effective Native-Video Memory for VLA Models

- Paper: https://arxiv.org/abs/2609.05533 (cs.CV; first exposure)
- Code: https://github.com/wadeKeith/SimpleMemVLA (MIT; verified live
  2026-09-09: full training + closed-loop evaluation code for LIBERO,
  Mikasa, RMBench, RoboMemArena, RoboMME, lerobot integration, configs,
  scripts; pushed 2026-09-09T00:44Z)
- Checkpoints/data: HF repos simplememvla_{libero,mikasa,rmbench,
  robomemarena,robomme} (verified live 2026-09-09)
- Classification: VLA (robot memory); Agentic Robot/VLA memory line.
- Why included: a parsimonious, artifact-backed answer to VLA memory
  design. SimpleMemVLA deliberately has **no dedicated memory module**:
  it keeps the sampled history intact and feeds it to the backbone in the
  timestamped video format the backbone was pretrained on, and the hidden
  states of a generated sub-task form the only channel from history to a
  standard flow-matching action head. Because consecutive decisions share
  most of their history, prefilling the shared prefix during action
  execution keeps latency close to a single-frame VLA. The authors report
  a new state of the art on four memory benchmarks (LIBERO-class
  long-horizon settings) without cost on general-purpose control, beating
  retrieval, compression, and recurrent-state mechanisms by a wide margin
  under a fixed backbone and training setup, and causal interventions
  confirm the policy genuinely reads its history. Complements MEMOBench
  (same run): MEMOBench measures where memory fails process-level;
  SimpleMemVLA is an open implementation of the "context is enough"
  alternative.
- Boundary: results author-reported; evidence is simulation-benchmark
  based as located on the arXiv record.

### MEMOBench: A Process-Level Memory Benchmark for Robotic Manipulation

- Paper: https://arxiv.org/abs/2609.07047 (EMNLP 2026; cs.RO; first
  exposure)
- Code: https://github.com/Collab-Gen/MEMOBench (verified live 2026-09-09:
  LIBERO-vendored evaluation harness with openpi websocket policy client,
  task configs, scripts; repository LICENSE is CC BY-NC 4.0 —
  non-commercial research only, with third-party LIBERO code under its own
  license — so it is open for research but not fully open-source)
- Data: https://huggingface.co/datasets/SunSeaLucky/MEMOBench (verified
  live 2026-09-09: MEMOBench.zip task data + assets.zip)
- Classification: Evaluation (robot manipulation memory benchmark).
- Why included: the process-level memory evaluation contract the VLA
  memory line needs. Robotic manipulation often requires acting on
  information no longer visible, yet VLAs are usually evaluated when the
  current observation largely determines the next action; existing memory
  benchmarks rely mainly on final task success, conflating forgetting with
  manipulation failure. MEMOBench instead labels one memory operation —
  Storage, Update, or Compression — per checkpoint: 30 history-dependent
  tasks, 1,500 expert demonstrations, and 4,200 executable checkpoint
  instances from 84 templates, each pairing coarse-to-fine language with a
  simulator predicate, and defines Memory Storage Rate, Memory Update
  Rate, and Memory Compression Rate measured alongside task success.
  Across standard and memory-augmented VLA policies the strongest memory
  module baseline reaches only 31.9% average success, and high storage
  often coexists with weak update and compression; checkpoint language
  also supervises semantic, contrastive, and framewise memory-alignment
  objectives with modest gains. It is the benchmark counterpart to
  SimpleMemVLA's architecture (both included this run).
- Boundary: benchmark assets public; results author-reported (EMNLP 2026).

## Watch list (new candidates, rechecks)

- **Where Success Breaks (DLS)** (`2609.06114`, cs.RO) — reframes robust
  VLA adaptation as *Failure-Boundary Learning* (Discover, Localize,
  Shape) with a real-grounded behavioral prior plus simulated co-training
  and semantic progress localization from privileged states. Directly
  relevant to the failure/recovery line (FailBench, LIBERO-RECOVER,
  VLA-Corrector); no artifacts located on the arXiv record as of
  2026-09-09; watch for a release.
- **VLA-Corrector** (`2609.06508`, cs.RO) — stage-aware observable-state
  verification + prompt-based closed-loop recovery for a *fixed* VLA (no
  parameter updates, no privileged simulator state): a learned verifier
  jointly estimates progress and execution risk over multi-view history,
  and failures are corrected through prompt recovery against semantic
  progress stages. Recovery-line complement to LIBERO-RECOVER; no
  artifacts located as of 2026-09-09; watch.
- **Proxy Policy Steering** (`2609.09148`, cs.RO) — inference-time
  specialization of a frozen generalist diffusion policy: two lightweight
  proxy policies whose calibrated velocity-space difference steers the
  frozen base sampler, with conditions under which the residual isolates
  task-induced change. Robot adaptation method; no artifacts located as of
  2026-09-09; watch.
- **WHIRL** (`2609.06009`, cs.RO, CoRL 2026) — intervention-aware world
  models with *real-world* RL for dexterous manipulation: binary human
  interventions are turned into a safety signal rather than discarded
  (project page https://whirl-dexterous.github.io/ live). Watch for code.
- **Monkey See, Can Monkey Do? / RoboReel** (`2609.08209`, cs.RO, CoRL
  2026) — unified benchmark for learning policies from human videos
  (RoboReel: bundled real-world human videos, simulated robot
  trajectories, evaluation environments on ten manipulation tasks). Site
  https://roboreel.github.io/ is live; no code/dataset repository located
  on GitHub as of 2026-09-09; watch for release.
- **Dex-X** (`2609.07747`, cs.RO) — visual-tactile dexterous manipulation
  learned from human videos with simulation as a tactile completion engine
  (project https://dexx-code.github.io/dexx-code/ live); watch for code.
- **TANGO** (`2609.09158`, cs.RO) — whole-body 29-DoF VLA navigation for
  humanoids in clutter, trained entirely in simulation; simulation-only,
  no artifacts located; watch-lite.
- **DeCAL** (`2609.09119`, cs.RO) — physically grounded dexterous VLA with
  contact-aware latent co-imagination (project page live); watch-lite.
- **WM-Craftnet** (`2609.07002`, cs.RO, CoRL 2026) — world-synesthesia
  model for dexterous in-hand manipulation; watch-lite.
- **SkillAdam** (`2609.08944`, cs.AI) — Adam-inspired skill-document
  optimization for stable skill self-evolution; repo live but no license
  asserted as of 2026-09-09; watch-lite.
- **MetaRSI / RSI2** (`2609.06396`, cs.LG) — meta-level recursive
  self-improvement with typed Data-RSI / Harness-RSI / Model-RSI
  operators across scientific domains; no artifacts; watch-lite.
- **Procedural Graphs** (`2609.09153`, cs.AI) — self-evolving execution
  structures (procedural-knowledge graphs) guiding LLM agents at decision
  time; digital, no artifacts; watch-lite.
- **Safe Harness Self-Evolution (theory)** (`2609.08175`, cs.AI) —
  theoretical feasibility/limits of safe harness self-evolution with
  finite-data certification bounds; theory-only, no artifacts; watch-lite.
- **SkillX** (`2609.06718`, cs.RO) — unified multi-skill policy for
  humanoid soccer; watch-lite.
- **EvoNav-Bench** (`2609.08292`, cs.RO) — lifelong navigation benchmark
  with evolving environments; watch-lite.
- **OVMAN** (`2609.06424`, cs.RO) — open-vocabulary motion-aware
  navigation task/benchmark (two-visit episodes); watch-lite.
- **NutriBench-Kitchen** (`2609.07135`, cs.CV, ECCV) — embodied nutrition
  management benchmark; watch-lite.
- **RoboCousin** (`2609.08339`, cs.RO) — bimanual simulation playground
  pipeline for new objects; watch-lite.
- **CR-VLA-Force / GloVLA / ICI-VLA / ContextFlow / 3DWay / LayerRoute /
  MobileVLA-R1 2.0** (`2609.05832/06256/07581/06852/08224/06079/06251`) —
  VLA/method papers; MobileVLA-R1 2.0's Apache-2.0 repo is live but its HF
  model card is empty (no weights) as of 2026-09-09; watch-lite.
- **Digital-agent eval/safety watch-lite:** Recall Is Not Protection
  (`2609.05797`, safety-monitor eval against elicitable prompts), The
  Oversight Gap (`2609.07162`, monitor 2-safety detectability), MOLE
  (`2609.06966`, insider-threat benchmark for AI agents — abstract says
  "open benchmark" but no link located on the record as of 2026-09-09),
  AgentDrift (`2609.06972`, step-labeled injection-hijack corpus, CC-BY-4.0
  dataset live), AURA-Eval (`2609.06783`, risk-awareness trajectory eval),
  HAE-GEO (`2609.06027`, deep-search poisoning benchmark — GitHub repo
  returns 404 as of 2026-09-09, not open), SchemeArena (`2609.08126`),
  Structurally Close, Temporally Distant (`2609.05911`), ResidualAuth
  (`2609.08062`), Counter-Swarm Doctrine (`2609.06140`), Seeing is Not
  Believing (`2609.08280`, ROS2 telemetry trust-boundary attack — robotics
  security, no artifacts located), How Long Until Your Robot Ignores You
  (`2609.07288`, CBS 2026 workshop safety benchmark for LLM orchestrators
  in human-humanoid collaboration), Rethinking Safety for Generalist
  Robots (`2609.06326`, embodied-AI-safety position paper), SkillSpec /
  SkillAlign / SE-GoS / Code2Skill / Who Maintains Agent Skills
  (`2609.06052/07255/08228/05571/05677`, skill-contract line), Experience
  Funnel (`2609.08919`), Environments as Scaffold (`2609.08404`),
  Elastic Horizon (`2609.07247`), Skynet (`2609.06835`), ExecCritic
  (`2609.09133`), AgentGrad (`2609.08572`), MemForest / CreaMem / MEMO /
  EdgeMem (`2609.08273/08550/07471/05553`), Revoked but Still Authoritative
  (`2609.08258`, code live), What Eviction Destroys (`2609.08279`),
  Unreliable Progress Bar (`2609.08589`), Do Reasoning Representations
  Help Humans Evaluate (`2609.09038`).
- **RoboSPA watch status update** (`2609.05324`, EMNLP 2026): the 09-08
  record listed RoboSPA as not open ("repo README still 'being prepared'").
  The official repository (github.com/fanzhenxuan/RoboSPA) **went live
  2026-09-08T08:47Z** (after that run's morning check) with full code
  (task envs, data-collection and policy-evaluation scripts, policy
  baselines, MIT license, project page live) — but the Hugging Face
  dataset `zxfan/RoboSPA` is still **card-only** (no data files) as of
  2026-09-09, so the benchmark assets are not yet open. Status: code open,
  dataset pending — retained on the watch list until the data lands.
- **Interface-Induced Trajectory Censoring** (`2609.03966`) — unchanged
  since 09-04 (no revision in this batch); watch status retained.
- Rechecked and unchanged from the 09-08 record (no revision in this
  batch, no new located release as of 2026-09-09): FWBC-VLA, RedVLA (v2
  only), VLA-Precision, Task-CoEvolve, TrapVLA, HINT, WISE, XR-2,
  StageWAM, ReflexVLA weights, DreamX-Phi, UniTexture, PRISM,
  GigaBrain-0.7/WBC, ForceU-VLA, LIBERO-VIFO, Agent Lightning, VLCP,
  Hydra-0, BATON, Q-Planning, AutoSaddler, JIT-Agent, UCAG-P,
  TemporalFlow-VLA, PredVLA, FlashVLA, WikiSkill, RedEvoAgent,
  SKILL.state, Agent Mesh, WALL-SS, R2M-Bench, INTENT-AS-A-TOOL,
  BTS-AgentBench, TraceBench, GraphMemix, UrbanGround, LM-X, Zero-WAM,
  VLAct, Code as Worlds, Aero Hand Open, CAITLYN, Dogwood, LongGuard,
  WebWorld, ASPIRE, S3Gym, StudyBench/S3Gym-class, WorldReward,
  Principia, Statebench, Puffin-World, Civilization Framework-class,
  FailureSpot, FailSAE, Harbor Adapters, Spectral-Target JEPA
  structuring (all unchanged from the 09-08 record's wording).

## Exclusions in the screened ranges (per policy)

- The five target categories' cross-lists include many entries whose
  primary category lies outside the five (cs.CR, cs.SE, cs.IR, cs.HC,
  math/stat/physics, eess.*, etc.); every such title was scanned and
  scope-relevant abstracts read. Domain-excluded: medical/surgical
  (OCTN `2609.06810`, ophthalmic haptics `2609.07857`, MedQA-MM
  `2609.03261v2`, radiographic world models `2609.07719`, RevalExo
  `2609.08090`, EmoMed `2609.07194`, organoid agents `2609.08696`),
  driving/ADS (LANTERN `2609.06368`, PlannerForge `2609.08965`,
  DriftParking `2609.06923`, DriveMotion `2609.08117`, PV-WM
  `2609.07328`, Hi-FLoop `2609.08796`), aerial/UGV ops and vehicle
  fleets (RoboSense `2609.06813`, D3ARC `2609.07350`, AGOS-Bench
  `2609.08402`, AirAnchor `2609.08442`, AeroBelief `2609.08164`),
  perception/estimation/SLAM/planning-without-agent-contract
  (OcclusionCBF, TASG-Explore, PGMT, Visible-Reachable Workspace,
  EquiGQNet, LightSplat, MFVINS, DCLP++, CAST, tensegrity MPC, mjorbit
  sim framework, OcclusionCBF, Geometric Distributional Control,
  anti-gravity MPC), teleop/HRI studies without reusable harness
  contract (SPOT, M3-Tele, CALM, HiBRIDGE, agency-perception study),
  tactile sensing hardware (TacClip `2609.08214`, BIFTA), bio/chem/astro
  and domain-science agents (WolfSociety finance agents, agriculture
  Monte Carlo papers, drug-response benchmarks, ferroelectric
  human-agent discovery), video/audio generation and editing (AVENUE,
  Encore, GEPARD TTS, AnomalyCraft, GAN-Blot, SceneMosaic, LoGAN,
  BinauralVAE, ActionSplice, Mask Forcing), watermarking/attribution,
  RAG/retrieval serving systems (RAGMark, Q2D-Web, Noësis, RepoNav,
  ProtoRAG, FRAME), code/algorithmic methods without harness contract
  (AttnCompress, SQLMorph, SWE-Test, RepoNav, CASD's distillation
  machinery treated as method-only, Flow3D-OPD, TV-Regulated OPD,
  RouteOPD, OracleZoom, BrachistoneLR), and the many cs.CL/cs.LG
  language-model and optimization papers that touch none of the curated
  concepts. Driving, medical, video-generation, perception-only, and
  conventional-control exclusions follow the standing policy.
- The 09-07 record's "unchanged batch" re-screen items and the 09-08
  record's exclusions were re-verified where they reappear in this
  batch's cross-lists; nothing previously excluded now qualifies.

## Operational notes

- SSH: `git fetch` failed initially with the known OpenSSH
  `/etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf` ownership error;
  `GIT_SSH_COMMAND='ssh -F /dev/null'` works (user key and known_hosts
  unaffected).
- arXiv API called over HTTPS; all queries HTTP 200. `/list` HTML pages
  stale (still the Monday block) while API index and abs pages advanced —
  listed under Scope; the API index was treated as authoritative.
- Working tree before the run: `docs/reference-architecture.md` modified,
  `docs/ring-harness.png` and `handoff.md` untracked — preserved untouched
  (never staged or committed). Scratch data under
  `.scratch/arxiv-2026-09-09/` removed after the run.
- Consistency: README header badge and Current Landscape "Last verified"
  both updated to 2026-09-09; entries added to General Harness Design
  (NeoHorse-1, Co-Evolving Harnesses and Models, Beyond Prompts), World
  and Physical-Reasoning Models (GE-Act 2.0, OpenWAM), VLA/Open and
  Reproducible table (SimpleMemVLA), and Benchmarks/Manipulation and VLA
  (MEMOBench); `git diff --check` clean; cross-file consistency with this
  record verified.
