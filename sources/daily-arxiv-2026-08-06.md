# Daily arXiv scan — 2026-08-06

## Scope

- Incremental interval: after the 2026-08-03 05:26 UTC repository update
  through 2026-08-06 09:39 UTC. The query deliberately included all of
  2026-08-03 so that announcement and repository timing could not create a
  partial-day gap.
- Official arXiv API sweep: 1,410 records submitted from 2026-08-03 through
  query time across `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, and `cs.LG`, followed
  by a relevance filter for harness engineering, agentic robotics, VLA,
  robot/world foundation models, skills, memory, recovery, evaluation, and
  safety.
- Important-revision sweep: the same categories sorted by arXiv
  `lastUpdatedDate`, paginated from query time past the previous commit cutoff;
  the final fetched page reached 2026-08-02 04:03 UTC, fully covering the gap.
- Verification used only arXiv records, author/project pages, and official
  repositories or model collections. Existing `README.md`,
  `docs/landscape.md`, and `sources/` entries were checked for duplicates.

The configured research-search backend was unavailable, so this run used the
official arXiv API and first-party artifact pages directly.

## Included

### ETA: A New Agentic Paradigm for Embodied Tasks / OpenETA

- Paper: https://arxiv.org/abs/2608.03924
- Project: https://openmoss.ai/OpenETA/
- Code: https://github.com/OpenMOSS/OpenETA
- Submitted: 2026-08-04.
- Classification: Agentic Robot/VLA Harness.
- Why included: this is a runnable physical-agent system boundary rather than
  another planner diagram. A planner emits one typed world-changing Tool call,
  a host-owned Interface validates and executes it, and the World returns the
  result and a fresh observation. The Apache-2.0 repository contains planner,
  tool, skill, memory, logging, replay, simulation, test, and real-robot paths,
  including UR5e integration guidance.
- Evidence boundary: benchmark and hardware results are author-reported. The
  repository makes the interface inspectable, but does not independently
  validate general physical robustness or safety.

### Harness-R1: Learning to Edit Executable Runtime Harnesses from Agent Failure Trajectories

- Paper: https://arxiv.org/abs/2608.02276
- Code: https://github.com/DeepExperience/Harness-R1
- Models: https://huggingface.co/ShaoShuai0605/Harness-R1
- Submitted: 2026-08-03.
- Classification: General Harness Methodology.
- Why included: trains a separate 9B harness engineer, initialized with
  supervised fine-tuning and improved with online group-relative policy
  optimization, to convert failure batches into validated executable
  lifecycle-hook patches while the target remains frozen. The official
  Apache-2.0 repository includes code, configs, demos, documentation, and
  tests, and the model weights are public.
- Evidence boundary: the authors report an average increase from 44.3% to
  53.6% across WebShop, ALFWorld, and DBBench before target fine-tuning. There
  are no robot experiments. Same-batch reruns supply the training reward, so a
  robotics transfer would still require untouched final evaluation, replay and
  simulation gates, immutable safety boundaries, approval, and rollback.

### World Action Models in Real Time: An Empirical Study of Smooth Execution via Asynchronous Deployment

- Paper: https://arxiv.org/abs/2608.01880
- Submitted: 2026-08-03.
- Classification: Robot Foundation/World Model; deployment harness.
- Why included: compares six synchronous and asynchronous action-chunk serving
  strategies using offline trajectory analysis and online experiments on a
  10 Hz bimanual robot. It isolates temporal alignment between observations,
  predictions, and committed commands as a core harness obligation rather than
  treating real-time execution as a model-only property.
- Evidence boundary: the authors report prefix-conditioned generation as the
  best overall balance of task performance, speed, and smoothness. No official
  code or project link was present on the arXiv record as of 2026-08-06.

### CoWAM: Coordination Contracts for Selective Policy Intervention with WAMs

- Paper: https://arxiv.org/abs/2608.02578
- Submitted: 2026-08-03.
- Classification: Agentic Robot/VLA Harness; Evaluation/Safety.
- Why included: turns synchronization, role compatibility, and collision
  convergence into typed admissibility checks, event-conditioned verification,
  calibrated intervention gates, and a predefined abstention fallback. This is
  a concrete contract for deciding when world-action evidence may override a
  nominal bimanual policy.
- Evidence boundary: across eight simulated bimanual tasks, the authors report
  a 9.6 percentage-point closed-loop gain over the strongest selective baseline
  and harmful interventions below 1%. There is no real-robot evidence or
  official code link on the arXiv record as of 2026-08-06.

### SAFECAST: Robust Failure Detection for VLA Policies with Contrast-Set Training and Calibration

- Paper: https://arxiv.org/abs/2608.04246
- Submitted: 2026-08-04.
- Classification: Evaluation/Safety.
- Why included: uses visual and language contrast-set perturbations to improve
  hidden-state failure-probe training and calibration under deployment shift.
  This directly addresses how a harness decides when a VLA rollout is becoming
  unsafe or invalid.
- Evidence boundary: the authors report statistically significant ROC-AUC
  improvements over their baseline on real-world DROID rollout data and LIBERO
  simulation across multiple VLM backbones. That is not an independent safety
  certification, and no official code link was present as of 2026-08-06.

### Weights or Skills? A Survey of Robot-Learning Techniques

- Paper: https://arxiv.org/abs/2608.01851
- Submitted: 2026-08-03.
- Classification: Survey; VLA and code-as-policy/self-improving harnesses.
- Why included: organizes 77 representative systems around competence stored
  in action-predicting weights versus executable skills, then distinguishes
  degrees of code-skill self-improvement, persistent memory, evolutionary
  search, portability, provenance, and verification. Its operational taxonomy
  is directly aligned with this repository's model–harness distinction.
- Evidence boundary: this is a focused author synthesis, not a benchmark or an
  independent validation of the systems it surveys.

## Reviewed but not promoted

- **HarnessCompass** (`2608.01918`) proposes constrained, task-agnostic harness
  evolution with agent feedback and component-wise optimization. It has no
  robotics evaluation or official implementation link, and overlaps the more
  reproducible Self-Harness/Harness-R1 entries.
- **EvolveNet** (`2608.04968`) evolves data-local multi-agent harnesses, but has
  no robot evidence and did not add enough implementation value beyond the
  selected general harness entries.
- **LongHorizon-Harness**, **OneDayAgent**, and **Argus** are digital-agent
  harness or evaluation systems without a demonstrated robotics boundary; they
  remain useful transfer references but would dilute the curated robot list.
- **ChainVLA** (`2608.02326`) and other new VLA architecture papers are
  model-centric and do not expose a substantially new runtime, recovery,
  evaluation, or physical-agent interface.
- **GUARD** (`2608.04510`) is adjacent runtime failure-monitoring work. SAFECAST
  was selected as the stronger single representative in this batch because its
  reported study spans real-world DROID data, LIBERO simulation, calibration
  shift, and multiple backbones.
- **Mimir** (`2608.04933`) proposes embodied neuro-symbolic memory, but its code
  is described as forthcoming; it is not represented as open source.
- **ProtoAct** targets wet-lab protocol translation and execution rather than a
  broadly reusable robot harness interface.
- Revision candidates including the *Foundation Models in Robotics*
  comprehensive review, Jetson-PI, ActionCache, QuASH, Mamba Policy, ActQuant,
  VLAFlow, DeepThinkVLA, SimFoundry, WCM, and GEM-4D were checked. No revision
  in the covered interval changed an artifact boundary or harness conclusion
  enough to justify a curated update.

## Artifact-status checks

- Guava's official page still marks code as coming soon.
- ASPIRE's official page still marks code as forthcoming.
- Robot-Factored World Models still links an official repository whose README
  says code is coming soon and does not expose the implementation.
- No checked project changed from forthcoming/placeholder status to a verified
  runnable release during this interval.
