# Daily arXiv scan — 2026-08-10 (second scan)

## Scope

- Second scan on 2026-08-10. The arXiv `/list/{cat}/new` listing pages advanced
  to the **"Monday, 10 August 2026"** announcement after the earlier scan that
  day reviewed the "Friday, 7 August 2026" batch. This record covers the Monday
  announcement across `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, and `cs.LG`: 633
  unique records in total, of which 502 have API metadata updated on or after
  2026-08-07 and appear in no prior `README.md`, `docs/landscape.md`, or
  `sources/` entry.
- A title-and-abstract relevance pass produced roughly 20 candidates for manual
  review. Verification used arXiv metadata and full-text pages plus official
  project pages and GitHub repositories; existing entries were checked for
  duplicates before selection.

## Included

### VLA-Arena: An Open-Source Framework for Benchmarking Vision-Language-Action Models

- Paper: https://arxiv.org/abs/2512.22539
- Code: https://github.com/PKU-Alignment/VLA-Arena
- Submitted: 2025-12-27 (revised 2026-08-07); accepted to ICML 2026.
- Classification: Evaluation/Safety; VLA.
- Why included: an open-source benchmark that quantifies VLA capability
  boundaries along three orthogonal difficulty axes (task structure, language
  command, visual observation). It comprises 11 task suites across Safety,
  Distractor, Extrapolation, and Long Horizon dimensions (170 tasks total), with
  fine-tuning restricted to the L0 difficulty level to assess generalization.
- Evidence boundary: the official repository (environments, skills, RLDS
  dataset builder, and test harness) was verified as public on 2026-08-10;
  model results on the benchmark are author-reported.

### AtlasVLA: Persistent World-Ego State Modeling for Vision-Language-Action Models

- Paper: https://arxiv.org/abs/2608.06729
- Submitted: 2026-08-07.
- Classification: Agentic Robot / VLA Harness; VLA.
- Why included: directly attacks the reactive-policy bottleneck in long-horizon
  and partially observable tasks. A 4D Persistent World State Memory lifts
  transient 2D observations into a voxel-hashed spatial state to resolve visual
  blind spots, while an Ego-Working State Memory tracks task progress; a
  diffusion Transformer conditions on the joint world-ego state. This is a
  memory/harness contract rather than a change to the base action head alone.
- Evidence boundary: results are author-reported; no official code, project, or
  weights link was located as of 2026-08-10.

### Is Forward Prediction Enough? Physical State Grounding for JEPA World Models

- Paper: https://arxiv.org/abs/2608.06799
- Submitted: 2026-08-07.
- Classification: Robot Foundation/World Model; Evaluation.
- Why included: PSG-JEPA adds two physical grounding objectives on top of JEPA
  forward prediction—grounding individual latents in robot proprioceptive state
  and latent pairs in multi-horizon joint-angle changes—applied only during
  training so inference cost is unchanged. The paper evaluates at three levels
  (latent identifiability, planning, policy), making it an evaluation of what
  JEPA-style world models actually capture.
- Evidence boundary: an official repository exists but marks code as coming
  soon as of 2026-08-10; reported results are author-reported.

### When Coordination Becomes a Threat: Communication Attacks in LLM-Controlled Multi-Robot Systems

- Paper: https://arxiv.org/abs/2608.06830
- Submitted: 2026-08-07.
- Classification: Evaluation/Safety.
- Why included: extends prompt-injection-style threat analysis from single
  robots to multi-robot coordination. It formulates an External Entry Point
  Attack and a Privileged In-System Attack and evaluates both across DMAS,
  HMAS-1, and HMAS-2 communication architectures, probing how attacker access
  settings shape attack propagation in LLM-planned robot teams.
- Evidence boundary: simulation-based and author-reported; no official code link
  was located as of 2026-08-10.

### AutoIntervene: Calibrated Intervention for Action-Chunking Imitation Learning Policies

- Paper: https://arxiv.org/abs/2608.07065
- Project: https://aus.bot/research/autointervene/
- Submitted: 2026-08-07.
- Classification: Evaluation/Safety; Runtime.
- Why included: an online framework that selectively transfers control between
  an action-chunking policy and an operator during deployment. It evaluates
  proposed chunks against a visual-action support memory built from successful
  executions, uses phase-local support for policy-to-operator transfer and
  global support for return-to-policy, and calibrates separate switching
  thresholds. This is a concrete intervention contract for chunk-based
  policies, complementing CoWAM's gating approach.
- Evidence boundary: the official project page exists; the linked GitHub
  repository is the project page and no implementation or benchmark release was
  verified as of 2026-08-10.

### OpenForgeRL: Train Harness-native Agents in Any Environment

- Paper: https://arxiv.org/abs/2607.21557
- Submitted: 2026-07-23 (revised 2026-08-07).
- Classification: General Harness Methodology.
- Why included: tackles the training side of harness-native agents. A lightweight
  proxy serves the harness's model calls while recording them as training data
  for a standard RL codebase (for example veRL), and a Kubernetes orchestrator
  runs each rollout in its own remote container, enabling end-to-end SFT/RL on
  stateful, multi-process harnesses such as Claude Code, Codex, and OpenClaw in
  arbitrary environments.
- Evidence boundary: the paper describes the framework as open-source, but no
  official repository link was located on the arXiv record as of 2026-08-10.

### A²E: An End-to-End Agent Auditing Engine

- Paper: https://arxiv.org/abs/2608.07346
- Code: https://github.com/datamllab/A2E
- Submitted: 2026-08-07.
- Classification: General Harness Methodology; Evaluation.
- Why included: an end-to-end evaluation engine for agent harnesses. An Agent
  Task Protocol enables rapid integration of evaluation tasks across harnesses,
  an automatically instrumented Monitor captures standardized execution traces,
  and an Evaluation stage scores harness capabilities with multidimensional
  metrics rather than correctness alone.
- Evidence boundary: the official repository (monitor, server, eval, task, and
  UI components) was verified as public on 2026-08-10; evaluated harnesses and
  tasks are digital-agent focused.

## Reviewed but not promoted

- **EchoVLA** (`2511.18112`) adds declarative scene and episodic memory to a VLA
  for mobile manipulation; the memory theme overlaps AtlasVLA and no official
  artifacts were located.
- **ODEWorld** (`2607.27924`) proposes a continuous-time latent world model, but
  the linked project page was not reachable during verification and there is no
  robot evaluation.
- **τ / Touch-Augmented VLA** (`2607.24485`) learns tactile representations from
  future visual supervision; a project page exists but no implementation or
  weights were verified.
- **SkillProx** (`2608.07449`) self-evolves agent skills via proximal textual
  gradient descent; it is a digital-agent method without robot evaluation.
- **TEMPO** (`2608.07314`) and **STRONG-VLA** (`2604.10055`) are VLA
  post-training/robustness methods with no verified artifacts.
- **CrossTracer** (`2608.06688`) is cross-embodiment navigation via VLA
  reasoning; navigation-domain and no artifacts located.
- **PILOT / Decoupling Intention from Trajectory** (`2608.06994`) addresses WAM
  representation entanglement but exposes no verified implementation.
- **Cross-View Action Consistency** (`2608.06965`) regularizes flow-based VLAs
  for camera robustness; no artifacts located.
- **SimWAM** (`2608.07468`) releases code and weights for an autonomous-driving
  world-action model; the driving domain is outside this list's robot
  manipulation focus.
- **How Should I Pick a Foundation Model for My Robot?** (`2608.06898`) is a
  FoRMA workshop position paper on social-robot evaluation; it is a framework
  proposal without released artifacts.
- **A²E was selected over OpenForgeRL-style training frameworks** — both are
  included, but purely method-oriented harness papers (for example
  **MemWM** `2608.07107`, **Agent Memory Distillation** `2608.07169`, and
  **Trajectory-Relative Hindsight Distillation** `2608.07371`) were excluded for
  lacking robot relevance or verified artifacts.
- Narrow teleoperation, conventional control, perception, locomotion,
  navigation, and hardware records were excluded unless they introduced a
  reusable agent, serving, recovery, safety, or evaluation interface.

## Artifact-status checks

- VLA-Arena: official repository verified (github.com/PKU-Alignment/VLA-Arena,
  last pushed 2026-08-09).
- A²E: official repository verified (github.com/datamllab/A2E).
- PSG-JEPA: official repository exists but states "Code coming soon" as of
  2026-08-10.
- AutoIntervene: project page reachable; GitHub repository is the project page
  only.
- OpenForgeRL: no official repository link was located on the arXiv record as
  of 2026-08-10.
- AtlasVLA, When Coordination Becomes a Threat, EchoVLA, ODEWorld, τ VLA,
  SkillProx, TEMPO, STRONG-VLA, CrossTracer, PILOT, and Cross-View Action
  Consistency: no verified code, dataset, or weights links as of 2026-08-10.
