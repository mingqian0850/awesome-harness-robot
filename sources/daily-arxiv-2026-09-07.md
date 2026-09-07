# Daily arXiv scan — 2026-09-07

## Scope

- Interval: since the 2026-09-06 run's cutoff (~2026-09-06 04:05 UTC, state file
  `1788667513`, which found nothing and left the repo unchanged) through
  2026-09-07 ~06:10 UTC. Missed dates covered: **2026-09-05 (Sat)** and
  **2026-09-06 (Sun)** submissions, plus the post-cutoff tail of 09-03 and
  09-04 (Fri) — all of which became visible when the weekend announcement block
  was indexed (the 09-06 run's log documented the lag: newest entry then was
  `2609.04203`, published 2026-09-03T17:59:55Z).
- **API health (important for future runs):** the arXiv export API
  (`https://export.arxiv.org/api/query`, HTTPS) returned persistent HTTP 500
  ("server encountered an internal error") for **every `submittedDate:` /
  `lastUpdatedDate:` field query** during this run — single-day, multi-day,
  with and without category filters — while plain category queries
  (`cat:cs.RO` etc.) returned clean 200s. The 500 body is the generic
  arXiv API error feed, not a rate-limit 429. Workaround used here: enumerate
  the newly indexed content via per-category heads sorted by
  `submittedDate`/`lastUpdatedDate` descending (1000 entries × 5 categories),
  then filter client-side by `published`/`updated > 2026-09-03T17:59:55Z` (the
  previous run's newest stamp). This is complete for the interval (new entries
  sort to the head) and should be reused verbatim if date queries fail again.
  Scratch XML kept under `.scratch/arxiv-2026-09-07/` (removed after the run;
  `/tmp` is ephemeral per shell invocation).
- **Newly indexed content (the weekend block):** the first announcement block
  newer than the 09-04 00:00 UTC block appeared. Global probe per category:
  newest entries now carry IDs `2609.0540x`–`2609.05416` with published stamps
  up to **2026-09-04T17:59:35Z** (Friday ~14:00 ET processing cutoff). No
  Saturday/Sunday (09-05/09-06) submissions are indexed yet — they should land
  with the next block (Monday evening ET), i.e., the next run's scope.
- **New submissions:** **415 unique entries** with `published` >
  2026-09-03T17:59:55Z, all v1, `published == updated` (0 revisions among
  them): 97 with published 09-03 (post-cutoff tail, IDs above `2609.04203`)
  and 318 with published 09-04. Primary-category split of the five target
  categories: cs.RO 31, cs.AI 101, cs.CL 60, cs.CV 77, cs.LG 73 (remainder are
  cross-listed entries whose primary category lies outside the five). All 415
  titles and 289 keyword-hit abstracts were screened (agent harnesses, harness
  engineering, self-improving harnesses, agentic robotics, robot agents,
  hierarchical/agentic VLA, vision-language-action, robot foundation models,
  robot/world-action models, code-as-policy, skill discovery, memory,
  recovery, evaluation, runtime monitoring, safety). Every cs.RO-carrying
  entry was individually accounted for.
- **Revisions:** **228 entries** with `updated` in [2026-09-03T18:05Z,
  2026-09-04T17:45Z] and `updated != published` became visible with this block
  (the 09-06 run's `lastUpdatedDate 09-04..09-06` sweep returned 0, so none
  were screened before). All were title-screened; scope-relevant ones were
  rechecked against the README and watch list (see below). Only one touches a
  README entry: A²E (`2608.07346`) v3 — a minor revision (added
  "Experimental Setup" appendix, author list change); no README change.
- Candidate claims were checked against the arXiv record (abs pages and HTML
  full text where needed), official project pages, and official
  code/model/data repositories (live HTTP, `git ls-remote`, and GitHub API
  checks on 2026-09-07); README, landscape, and prior source records were
  checked for duplicates (no name/ID collisions; prior coverage ended at ID
  `2609.04203`). All performance results remain author-reported unless stated
  otherwise.

## Included (new curated entries)

### LIBERO-RECOVER: Beyond Task Success Towards Failure Recovery in Robotic Manipulation Models

- Paper: https://arxiv.org/abs/2609.05178
- Project: https://liulin815.github.io/LIBERO-Recovery/ (linked from the arXiv
  record); code: https://github.com/liulin815/LIBERO-Recovery (live 2026-09-07,
  HEAD `4027d2b`); data: ModelScope `ataier/LIBERO_Recovery_Assets` and
  `ataier/LIBERO_Recovery_Expert` (both live HTTP 200, 2026-09-07).
- Classification: Evaluation/Safety (failure-recovery benchmark for
  manipulation VLA/WAM models, built on LIBERO).
- Why included: recovery is a distinct evaluation contract from ideal-condition
  task completion, and this is the first large-scale benchmark built on *real*
  execution failures of current embodied models: 1,000+ recovery scenarios
  across four recovery levels — Action Retry, Action Adaptation, Object State
  Recovery, Environmental Recovery — scored across four capabilities (spatial
  understanding, object-structure reasoning, interaction understanding,
  topological reasoning). The paper's motivating measurement (SOTA near-100%
  on LIBERO yet unable to recognize/recover from failed grasps, collisions, or
  unintended object movement) is exactly the benchmark-vs-reality gap this
  list tracks, and it extends the LIBERO family already curated (LIBERO-VIFO
  cue safety; FailBench failure detection as the detection half). Real
  artifacts: repository live with benchmark/dataset assets (project page
  reports 2,178 scenarios, 3,184 human recovery demos, 16 evaluation
  dimensions). Page notes the paper is under review; results author-reported.
- Boundary: recovery-level taxonomy and 16-dimension scoring are the reusable
  contract; base-task completion is not re-benchmarked.

### ROBORMBENCH: Same Trajectory, Contradictory Rewards — Paraphrase Fragility in Vision Language Reward Models

- Paper: https://arxiv.org/abs/2609.05401
- Classification: Evaluation/Safety (paraphrase-invariance benchmark for
  VLM-based robot reward models).
- Why included: VLM reward models are increasingly used to score robot
  trajectories for learning, which presupposes paraphrase invariance — the same
  trajectory must get the same reward under semantically equivalent goal
  descriptions. The benchmark measures exactly this: 2,390 real-robot
  trajectories with ground-truth progress labels and 21,673 verified
  paraphrases (lexical, syntactic, action-goal rewrites). The authors report
  that paraphrasing the instruction alone can flip identical behavior between
  failure and success; instability is widespread across proprietary and open
  VLMs, grows under more divergent rewrites, is not reliably reduced by scale
  or explicit reasoning, and is substantially lower in trajectory-grounded
  reward models. Direct sibling of the included FailBench (VLM *success
  judging*) and of TrAct's VLAC reward selection — the judge/reward-reliability
  line. FailBench disambiguation note does not apply (no name overlap).
- Boundary: no artifacts located on the arXiv record as of 2026-09-07 (no
  comment, no data/code link; web search found none) — benchmark contract and
  author-reported measurements only, treated literally.

### TacPAC: Tactile Prediction and Real-Time Action Correction in World-Action Models for Contact-Rich Manipulation

- Paper: https://arxiv.org/abs/2609.05266
- Code: https://github.com/LogosRoboticsGroup/TacPAC (live 2026-09-07, HEAD
  `73565cc`, pushed 2026-09-07; MIT license file verified).
- Classification: Robot Foundation/World Model (tactile world-action model
  with real-time action correction during chunk execution).
- Why included: a concrete fix for the WAM timing mismatch this list's
  prediction–execution line (HarnessWAM, TempoWAM, GlanceWAM) is organized
  around — predictions precede execution while tactile feedback arrives during
  it. TacPAC caches the predicted contact a planned chunk was conditioned on
  and corrects not-yet-executed actions against each new tactile image (one
  cache pass, 20.7× cheaper than chunk regeneration). Real-robot evidence:
  five tasks (precision insertion, fragile-object handling, reorientation,
  long-horizon), average success 22% (vision-only base) → 64% (author-
  reported). MIT-licensed code live on a documented Flexiv Rizon 4 platform.
- Boundary: correction scope is the planned chunk; the tactile expert is a
  separate component trained for the contact regime.

### HackProbe: Harness-Agnostic Detection and Immunization of Reward Hacking in Self-Evolving Language Models

- Paper: https://arxiv.org/abs/2609.04665
- Classification: General Harness Methodology; Evaluation/Safety
  (harness-agnostic reward-hacking monitor + immunization for self-evolving
  loops).
- Why included: self-evolving LMs that keep whatever raises a visible proxy
  score are structurally exposed to reward hacking, and this is a monitor
  contract that attaches to *an arbitrary* self-evolving loop through two
  black-box hooks (no weights/activations access): a secret, distribution-
  fixed comparison core (frozen distribution keeps the capability proxy
  comparable across generations) plus a rotated fresh layer hardening the
  probe bank against co-adaptation; four proxy tests (level gap, scale-aligned
  divergence with online change-point detection, capability stagnation,
  conditional confidently-wrong rate) combined under a Šidák-corrected
  family-wise p-value; a risk-aware immunization layer reselects an honest
  candidate from the proposal pool (≤ log₂ Pᵢ bits/generation disclosure); and
  a detectability bound converting a target error rate into a probe-size
  budget. Distinct artifact/contract within the self-improvement audit line
  (Phantom Gains measures null baselines; Auditing Harness Tampering
  categorizes harness tampering; BAITBENCH measures hacking rates) — HackProbe
  is the deployable monitor + reselection layer.
- Boundary: digital evidence only (controlled prompt-level host with four
  injected hacking channels and ground-truth labels); no robot experiment; no
  official artifacts located as of 2026-09-07.

### What Does Multi-Harness RL Learn? Credit Assignment and Portability in Coding Agents

- Paper: https://arxiv.org/abs/2609.04518
- Classification: General Harness Methodology (harness-effects measurement for
  agent RL).
- Why included: the cleanest quantitative statement yet that in harness-native
  agent RL the *evaluation harness* is the dominant variable: from one
  Qwen3-8B warm start, frozen records from Aider, OpenHands, Qwen Code, and
  SWE-agent replayed under GRPO Within vs Cross grouping, scored with a sealed
  SWE-bench Verified oracle — across 24,000 sealed evaluations the harness
  moves mean solve rate 2.14%→9.27% (4.3×) while the training recipe moves it
  1.16, and the grouping rule (credit-assignment choice) is null (Cross−Within
  +0.25 pp, 95% CI [−0.48, +1.02]), with seed range exceeding the between-rule
  difference. RL sequel to the harness-effects measurement line (Same Model,
  Different Harness; EnvHarness): results reported through one harness are
  harness-conditioned.
- Boundary: single model family and task domain (repository-level coding);
  digital; no robot experiment; no official artifacts located as of
  2026-09-07.

### RedVLA: Physical Red Teaming for Vision-Language-Action Models

- Paper: https://arxiv.org/abs/2604.22591 (v1 2026-04-24 → **v2 2026-09-04,
  major expansion** ~5.6 MB → ~13.8 MB; first exposure of this paper to the
  curation pipeline, which began after its v1)
- Project: https://redvla.github.io (live 200; JS-redirect page carries no
  code/data links)
- Classification: Evaluation/Safety (pre-deployment physical red teaming of
  VLA models).
- Why included (revision-driven): v2 is a substantive revision of an
  in-scope paper that was never previously screened, and the topic fills a gap
  in the curated VLA-safety line: the proactive *elicitation* counterpart to
  TrapVLA (backdoor configured failures), Bit-Flip Attacks (weight integrity),
  and MANIGUARD (specification-grounded safety eval). RedVLA's two-stage
  pipeline — Risk Scenario Synthesis (identify critical interaction regions in
  benign trajectories, position the risk factor to entangle with execution)
  and Risk Amplification (gradient-free refinement of the risk-factor state
  for stable elicitation across models) — is a reusable red-teaming protocol;
  across six VLA models the authors report attack success up to 95.5% within
  ten optimization iterations, plus SimpleVLA-Guard, a lightweight guard
  trained on RedVLA-generated data.
- Boundary: evidence primarily simulation-based with a real-world validation
  study (v2 adds extensive appendices: risk suites, annotation, guard
  training); the abstract claims data/assets/code are available but neither
  the arXiv record nor the project page exposes a link as of 2026-09-07 —
  treated literally as not open; results author-reported.

### From Language Models to World-Acting Systems: Progress and Limits of Agentic AI across Digital, Social, Virtual, and Physical Environments

- Paper: https://arxiv.org/abs/2609.04894
- Classification: Survey (agentic-AI systems map; model/harness/environment
  separation).
- Why included: a 29-page critical review (cutoff 2026-08-31) that organizes
  the agentic-AI evidence along delegated authority, temporal persistence, and
  environmental coupling while *separating model, harness, and environment* —
  the same separation this repository's curation policy applies. Its
  evidenced asymmetry (action-interface expansion documented more convincingly
  than robust completion, recovery, authorization, or independent
  verification; MCP/Agent2Agent improve interoperability without establishing
  trustworthy delegation; robotics and self-driving labs show bounded
  feasibility) makes it a useful systems-map anchor in the Surveys section
  alongside The Embodiment Gap and Toward Unified Robot Learning.
- Boundary: review article, no new experiments; "justified delegation" is
  proposed as an analytical heuristic, not a certified score.

## Watch list (new candidates, artifact rechecks)

New-submission watch items (checked 2026-09-07; arXiv record + project pages +
official repos):

- **RoboSPA** (`2609.05324`, EMNLP 2026 main) — large VLA spatial–procedural
  diagnostic benchmark (527K trajectories, 280 task variants, five difficulty
  levels, diagnostic metrics beyond success). The arXiv abstract states data
  and code are available at `github.com/fanzhenxuan/RoboSPA`, but the
  repository README says "code and dataset are currently being prepared and
  will be released soon" — treated literally: **not open yet**. Watch for
  release.
- **VLA-Precision** (`2609.04355`) — real-world online RL post-training for
  VLA (ACoB asymmetric co-bootstrapping across timescales; ACoB-Stream
  closed-loop experience–policy architecture, up to 10.9× throughput; nine
  high-precision tasks). Project page live; the Apache-2.0 repository
  `scy-v/VLA-Precision` (HEAD `7915295`) is live with real content
  (teleop/data/training pipeline). Held on the watch list: no clearly fitting
  README section for a robot RL training framework; promote when the entry
  structure supports it or on further release.
- **One Word, Different Action** (`2609.05260`) — real-robot benchmark pairing
  task-preserving/task-changing instructions to test Decision Invariance and
  Decision Sensitivity; models near saturation on single-constraint changes
  but degrade on multi-constraint composition. No artifacts; watch.
- **Latent Semantic Scaffolding (LSS)** (`2609.04893`) — training-time-only
  reasoning alignment for VLA (action-token ↔ physical-reasoning-rationale
  alignment via projection head, dropped at inference; dense phase-local
  alignment transfers better than pooled). No artifacts; watch.
- **CFAMs** (`2609.04552`) — continual field-adaptive models for
  post-deployment physical AI (frozen slow component + gradient-free Capsule
  Field; five embodiments incl. pi0/CogACT/SpatialVLA comparisons). Strong
  claims; no artifacts located; watch.
- **NavArena** (`2609.04602`) — automated construction of goal-oriented
  navigation benchmarks from 3DGS reconstructions (22.2M expert trajectories);
  assets "will be released publicly" — not open; watch.
- **What Matters, When?** (`2609.05376`) — ACT conditional-visual-grounding
  diagnosis (distractor similarity × manipulation stage; interventions help in
  sim and on a UR3e). 4-page non-archival DexHAND workshop abstract; watch.
- **Neuro-symbolic procedural reasoning for long-horizon VLA** (`2609.05369`)
  — task graphs + multimodal procedural memory over VLA control; 6-page
  non-archival X-Reason workshop abstract; watch-lite.
- **TourPhysics** (`2609.04911`) — physics-grounded interactive world model
  for exploration/manipulation from a single image (extends PhysOmni);
  video-world-model domain; watch-lite.
- **Online skill evolution for computer-use agents (Skill-Evo4GUI)**
  (`2609.04869`, code `LongtaoHu/Skill-Evo4GUI` live) — persistent versioned
  skill library from interaction traces with empty-library control across four
  OSWorld domains; digital skill-evolution line (ASPIRE, RedEvoAgent-class);
  watch.
- **TROVE** (`2609.05019`) — trace-grounded route validation and editing for
  skill orchestration; digital; watch-lite.
- **Trace2Tower** (`2609.05261`) — transition-aware skill-hierarchy induction
  from traces (ALFWorld/WebShop); digital; watch-lite.
- **Memory portability study** (`2609.05339`) — controlled study of agent
  memory across model upgrades (LC-RAW/RAG/NOTES/KG under identical history);
  digital memory line; watch.
- **τ^τ-Bench** (`2609.04611`) — agent-construction-as-task benchmark (53
  tasks, deploy-and-score protocol; best config 23.9% vs 82.2% expert
  reference); digital; S3Gym/StudyBench-class agent-construction env; watch.
- **SiLR** (`2609.04629`) — structure-preserving admission (product-order
  gate, proof that no scalar surrogate is sound) + process reward for tool
  agents; digital runtime-gate line (One Gate Is Not Enough); watch.
- **CONTINUITY** (`2609.05269`, code `zast-ai/continuity` live) —
  security-context contracts (assume-guarantee + authenticated context) for
  composable agent controls; digital; watch.
- **Speculative Uncertainty / draft-model gate** (`2609.05274`, EMNLP 2026
  industry) — pre-execution veto gate from draft-model cross-likelihoods
  (cuts execution error 6–8 pp); digital gating line; watch-lite.
- **KVMem** (`2609.04852`) — KV-context virtualization for million-token agent
  workspaces; digital infra; watch-lite.
- **CUA-Universe** (`2609.05374`) — scalable hybrid GUI+CLI agent environment
  pipeline; digital; watch-lite.
- **Substrate-aware agents** (`2609.05232`, code live) — execution context as
  first-class input (memory/time/compute contract disclosure changes generated
  code); digital; watch-lite.
- **ICM-Bench** (`2609.04438`, code `Shidu-Ren/ICM-Bench` live) —
  identity-centric long-term-memory benchmark for multimodal agents; digital;
  watch-lite.
- **HarvestBench** (`2609.04444`) — priced side-effect benchmark (kill rates
  0.4–98.8% across nine models); value-alignment eval; digital; watch-lite.
- **DCFA** (`2609.04749`) — causal failure attribution in LLM multi-agent
  systems; digital observability; watch-lite.
- **Cross-domain tracker adaptation via VLM agents** (`2609.05239`) — VLM as
  diagnostic agent tuning a detect-to-track pipeline; vision domain with an
  agent-in-the-loop; watch-lite.
- **MARL change-point detection (PPR)** (`2609.05298`, PAAMS 2026) —
  reward-based drift detection for cooperative MARL; monitoring; sim-only;
  watch-lite.
- **Risk-aware optimal control with rulebooks** (`2609.05199`) —
  lexicographic risk-rulebook control with anytime certified gaps; control
  theory, highway-merge sim; domain-lite; watch-lite.
- **RefactorPlatform** (`2609.04898`, EMNLP 2026 System Demonstrations) —
  open-source evaluation harness for repository-scale refactoring agents
  (isolated workspaces, AST verification, telemetry). Abstract claims
  open-source, but no repository link on the arXiv record and no search hit
  located as of 2026-09-07 — watch until the repo is found/verified.
- **La Agente Óptima** (`2609.04564`) — agentic self-driving-laboratory
  framework (LLM reasoning separated from executed Bayesian-optimization
  campaigns; physical contact-angle platform); science-automation domain;
  watch-lite.
- **Behavior trees adaptation study** (`2609.05331`) — literature-driven
  classification of robotic adaptation needs vs BT sufficiency; position-y;
  watch-lite.
- **Coupled control + wireless world models** (`2609.04851`) — JEPA world
  models of robot + channel for predictive communication scheduling; IoT-J
  submission; watch-lite.
- **Schema-bounded LM policy refinement** (`2609.05133`) — NetLogo–Python
  multi-robot study (90 records, 30 rounds); descriptive config-level evidence;
  watch-lite.
- **Why Better Models Can Create Riskier Systems** (`2609.04373`) — correlated
  LLM-trader behavior and non-diversifiable risk; finance domain; watch-lite.
- **A²E v3** (`2608.07346`, README entry) — minor revision (added
  "Experimental Setup" appendix, author change); no README update warranted.
- **FWBC-VLA v2** (`2609.03889`, watch-listed 09-04) — same-day v2, same file
  size, no comment; watch status unchanged (no artifacts).
- **RedVLA** — see Included (its SimpleVLA-Guard release is also tracked
  here: guard code/weights not located).
- Watch items from the 09-04 record rechecked and **unchanged** (no revision,
  no located release): Task-CoEvolve, TrapVLA, HINT, WISE, XR-2, StageWAM,
  ReflexVLA weights, DreamX-Phi, UniTexture, PRISM, GigaBrain-0.7/WBC,
  ForceU-VLA, LIBERO-VIFO, Agent Lightning, VLCP, Hydra-0, BATON, Q-Planning,
  AutoSaddler, JIT-Agent, UCAG-P, TemporalFlow-VLA, PredVLA, FlashVLA,
  WikiSkill, RedEvoAgent, SKILL.state, Agent Mesh, WALL-SS, R2M-Bench,
  INTENT-AS-A-TOOL, BTS-AgentBench, TraceBench, GraphMemix, UrbanGround, LM-X,
  Zero-WAM, VLAct, Code as Worlds, Aero Hand Open, CAITLYN, Dogwood, LongGuard,
  WebWorld, ASPIRE, S3Gym, StudyBench/S3Gym-class, WorldReward, Principia,
  Statebench, Puffin-World, Civilization Framework-class.

## Exclusions in the screened ranges (per policy)

- Driving/CAV (all domain-excluded unless harness/eval contract):
  `2609.04364` edge-assisted CAV fusion, `2609.04807` CoLMIN cooperative
  driving negotiation, `2609.04921` diffusion planner + safety-critical
  scenario generation (nuPlan closed-loop), `2609.05397` CrossDepth surround
  depth.
- Locomotion/perception/hardware/control: `2609.04411` AquaBEV underwater
  occupancy, `2609.04464` near-optimality theory, `2609.04545` SocioGesture,
  `2609.04607` open-set 3D scene graphs, `2609.04620` triadic packing HRI,
  `2609.04759` dressing diffusion policy (single-task), `2609.04770`
  continuous cognitive coverage, `2609.04799` HaptiNet rehab teleoperation,
  `2609.04817` mechanics theory, `2609.05084` ToPos topography, `2609.05161`
  APEX-RBD hardware, `2609.05206` hand morphology analysis, `2609.05282`
  tactile handover (task-specific), `2609.05300` H2INT crowd navigation,
  `2609.05325` FIRE-LIVWO odometry, `2609.05361` humanoid HRI prototype
  hardware, `2609.04339` quadrotor RNN, `2609.05199`-class rulebook control
  (watch-lite above), `2609.04851`-class remote-control world models
  (watch-lite above).
- Digital/enterprise/game/medical/finance/audio/vision-generation items in the
  screened ranges (speech, medical imaging, OCR, graphs, weather, e-commerce,
  GUI/CLI benchmarks without robot/harness contract, TTS, legal, biology,
  security ops, video/3D generation, tracking, super-resolution, deepfake,
  forensics, recommendation, quantum, spiking, federated learning, MoE
  efficiency, KV/serving) are domain-excluded per policy unless they carry an
  agent-harness/evaluation/safety contract (those are watch-listed above).
  Representative named items: `2609.05093` Discovery Loop (math-optimization
  program evolution), `2609.04444` HarvestBench (watch-lite), `2609.04697`
  SQL-Zero, `2609.04711` research-software catalog, `2609.05171` WeAgent
  image-gen agents, `2609.04850` ElderBench, `2609.05258` OR-Clarify,
  `2609.05157` AxQM, `2609.04913` ARIA infotainment testing, `2609.05239`
  tracker agents (watch-lite), `2609.04611`-class agent-construction benches
  (watch), `2609.04681` agentic-SDLC research agenda (no new measurements),
  `2609.04518`-siblings, `2609.05370` Decompile-Diverge, `2609.05296`
  LexFlip, `2609.04579` identity-handoff audit (GroundLM workshop; digital),
  `2609.04894`'s SDL/robotics coverage is via the survey entry itself.
- Survey/position items without new systematic scope (`2609.05257`
  commonsense-CV survey, `2609.04779` graph survey, `2609.05314` HVAC review)
  excluded.

## Operational notes

- SSH: `git ls-remote`/`git fetch` failed initially with the known OpenSSH
  `/etc/ssh/ssh_config.d` ownership error; `GIT_SSH_COMMAND='ssh -F /dev/null'`
  worked (user key and known_hosts unaffected). Remote `main` = `f7896d3` =
  local HEAD before this run; no divergence.
- The arXiv export API 500ed on all date-field queries this run (see Scope);
  per-category head enumeration was the complete, verified workaround. 10/10
  category fetches clean; artifact checks used live HTTP / `git ls-remote` /
  GitHub API (no 403s encountered).
- Working tree before the run: `docs/reference-architecture.md` modified,
  `docs/ring-harness.png` and `handoff.md` untracked — preserved untouched
  (never staged or committed). Scratch XML removed (`.scratch/` deleted after
  the run).
- Consistency: README header badge and Current Landscape "Last verified" both
  updated to 2026-09-07; entries added to Benchmarks/Manipulation-and-VLA
  (LIBERO-RECOVER, ROBORMBENCH), Robot Foundation and World Models
  (TacPAC), Harnesses/General Harness Design (HackProbe, Multi-Harness RL),
  Runtime-Safety/Safety (RedVLA), and Surveys (World-Acting Systems review);
  `git diff --check` clean; cross-file consistency with this record verified.
