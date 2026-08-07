# Daily arXiv scan — 2026-08-07

## Scope

- Incremental interval: after the 2026-08-06 09:39 UTC scan through
  2026-08-07 08:39 UTC.
- Official arXiv API query: `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, and `cs.LG`,
  sorted by `lastUpdatedDate`. One 500-record page reached
  2026-08-06 02:59 UTC, safely past the previous cutoff.
- Records inside the exact interval: 299 total, comprising 203 version-one
  submissions and 96 revisions. A title-and-abstract relevance pass produced
  29 candidates for manual review.
- Themes reviewed: harness optimization, agentic robotics, hierarchical VLA,
  robot skill memory, world-action/foundation models, interactive world models,
  physical-fidelity evaluation, recovery, and runtime safety.
- Verification used arXiv metadata and full-text HTML plus official project
  pages. Existing `README.md`, `docs/landscape.md`, and `sources/` entries were
  checked for duplicates.

`parallel-cli` was installed but had no configured Parallel or OpenRouter
credential, so the search used the official arXiv API and first-party pages.

## Included

### HarnessOpt-Bench: Evaluating LLMs at Harness Optimization

- Paper: https://arxiv.org/abs/2608.06301
- Submitted: 2026-08-06.
- Classification: General Harness Methodology; Evaluation.
- Why included: defines an end-to-end protocol for harness optimization under
  expensive and stochastic evaluation. The optimizer receives a seed harness,
  graded feedback, and a fixed target-evaluation budget, while a trusted
  execution environment hides the held-out test partition, meters target-agent
  resources, and preserves candidate versions for audit. This directly
  complements Self-Harness and Harness-R1 with an evaluation boundary.
- Evidence boundary: the authors report 111 scored runs using five frontier
  optimizer models, shared and native coding harnesses, and four downstream
  tasks. Those are digital-agent tasks rather than robot experiments. No
  official code or benchmark artifact was linked from the paper as of
  2026-08-07, so robotics transfer remains unvalidated.

### ω-0: A Latent Predictive World Action Model for Concurrent Humanoid Loco-Manipulation

- Paper: https://arxiv.org/abs/2608.06375
- Submitted: 2026-08-06.
- Classification: VLA; Robot Foundation/World Model.
- Why included: directly couples a predictive future-observation latent
  objective to diffusion-based, controller-compatible whole-body actions for
  concurrent locomotion and manipulation instead of decomposing the two. It
  also defines the author-reported ω-HOME dataset with more than 40 hours
  of synchronized multiview observations, whole-body motion, robot state, and
  action latents.
- Evidence boundary: the authors report real-world evaluation on 11 household
  tasks and improvements over selected imitation, VLA, humanoid, and WAM
  baselines. No official project, code, public dataset, or weights link was
  present in the arXiv record or full text as of 2026-08-07.

### GeniWorld: A Generalizable Interactive World Model for Robotic Manipulation via Visual Actions

- Paper: https://arxiv.org/abs/2608.06332
- Official project: https://chenghaogu.github.io/GeniWorld/
- Submitted: 2026-08-06.
- Classification: Robot Foundation/World Model; Evaluation.
- Why included: transforms numeric actions into URDF-rendered visual actions,
  decouples embodiment kinematics from environmental dynamics, and connects
  autoregressive video prediction with high-frequency robot kinematic control.
  This makes the world model an interactive interface for policy evaluation,
  human teleoperation, and synthetic rollout generation rather than only a
  passive video predictor.
- Evidence boundary: zero-shot environment generalization, evaluator
  reliability, and policy-improvement results are author-reported. The official
  project page exposed method material and demos but no code, data, or model
  weights as of 2026-08-07.

### GAUGE: A Measurement-Grounded Benchmark for Physical Fidelity in Simulation Engines and Video World Models

- Paper: https://arxiv.org/abs/2608.05948
- Submitted: 2026-08-06.
- Classification: Evaluation/Safety; Robot Foundation/World Model.
- Why included: evaluates numerical simulators and generative video world
  models against real trajectories, calibrated physical metadata, uncertainty
  annotations, and task-specific observables. Its 22 controlled task families
  cover rigid bodies, cables, textiles, volumetric deformables, collision,
  friction, momentum transfer, oscillation, self-contact, and deformation.
- Evidence boundary: the authors compare Isaac Sim, Genesis, and Newton on 14
  task families and six image-to-video models on five rigid-body families. The
  reported failures are diagnostic findings, not an independent certification
  of any engine. No official code or dataset link was present as of
  2026-08-07.

## Reviewed but not promoted

- **SkillMemo** (`2608.05970`) learns latent atomic skills and retrieves them
  from an episodic memory inside DP/VLA policies. Its official project page
  still labels code and data as coming soon, and the mechanism is model-internal
  rather than an inspectable agent-harness memory interface.
- **Beyond Flat Policies / HiRoC** (`2608.05999`) hierarchically post-trains a
  planner and low-level executor, but is primarily a policy-training method and
  did not expose official implementation artifacts.
- **GeniWorld was selected over Robust-WAM** (`2608.05903`) because it exposes
  a clearer interactive evaluation interface; Robust-WAM is a model-centric
  semantic-foresight objective without a verified public artifact.
- **MobileWAM** (`2608.04657v2`) extends WAMs to mobile manipulation, but its
  updated abstract still says code will be released soon. It is not represented
  as open source.
- **DyPES-VLA** (`2608.06374`) is a cross-embodiment dynamics/control
  architecture rather than a new harness contract or released development
  stack.
- **Reinforcing Action Policies by Prophesying** (`2511.20633v2`) combines a
  learned robot world model with RL post-training. Its project endpoint could
  not be reached during verification, so no artifact-status change was claimed.
- **IcFuzz** (`2608.06088`) fuzzes Isaac Sim stages and is useful simulator
  testing work, but it does not directly evaluate the model–harness boundary
  targeted by this list.
- Narrow teleoperation, conventional control, perception, locomotion, and
  navigation records were excluded unless they introduced a reusable agent,
  model-serving, recovery, safety, or evaluation interface.

## Artifact-status checks

- GeniWorld's official page exposed no implementation, dataset, or weights.
- SkillMemo's official page explicitly marked code and data as coming soon.
- MobileWAM still described code as forthcoming in its updated arXiv abstract.
- No reviewed revision changed a previously listed project from placeholder or
  forthcoming status to a verified runnable release.
