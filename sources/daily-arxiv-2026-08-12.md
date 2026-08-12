# Daily arXiv scan — 2026-08-12

## Scope

- Gap interval: after the 2026-08-11 scan cutoff at 08:43 UTC through
  2026-08-12 12:48 UTC.
- The arXiv API returned 344 records submitted on 2026-08-11 and 2026-08-12
  across `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, and `cs.LG`. Only records after
  the prior cutoff were treated as new. A title-and-abstract pass covering
  harnesses, agentic robots, VLAs, world/action models, memory, recovery,
  skills, safety, and evaluation produced the candidates below.
- Verification used official arXiv records, official project pages, and official
  repositories. Existing README, landscape, and source entries were checked for
  duplicates. Performance numbers remain author-reported.

## Included

### Flex-π: A Multi-Stream World-Action Model with Compute Flexibility

- Paper: https://arxiv.org/abs/2608.10860
- Project: https://flex-pi.github.io/
- Official repository: https://github.com/geyan21/flex-pi
- Submitted: 2026-08-11.
- Classification: Robot Foundation/World Model; VLA.
- Why included: jointly models actions with RGB, pointmap geometry, and DINO
  semantics in a shared latent space. Per-stream dropout and cross-modality
  forcing let one checkpoint run with different observed/predicted stream sets,
  exposing an explicit compute-versus-foresight contract for a deployment
  harness rather than fixing one inference mode.
- Evidence boundary: the authors report 2–7× gains on dexterous, precise,
  real-world bimanual tasks and faster execution than π0.5. The official
  repository currently contains a release plan only; it says training code,
  inference code, and checkpoints will be pushed after cleanup. Flex-π is
  therefore **not yet an open implementation** as of 2026-08-12.

### REDAgentBench: Executable Red Teaming and Faithful Measurement of LLM Agent Systems

- Paper: https://arxiv.org/abs/2608.10669
- Submitted: 2026-08-11.
- Classification: General Harness Methodology; Evaluation/Safety.
- Why included: evaluates harmful effects from service receipts and final-state
  changes inside isolated sandboxes instead of relying on a single
  model-judged attack-success score. Its 1,661 cases span five service surfaces,
  six models, and three harnesses, and explicitly separate exposure, execution,
  observation, and adjudication.
- Evidence boundary: the reported attack and mitigation results are for digital
  tool-using agents, not robots. Physical transfer would require typed robot
  state receipts, immutable safety controls, and hardware-aware adjudication.
  No official code or benchmark repository was linked as of 2026-08-12.

## Reviewed but not promoted

- **Surgical WAM** (`2608.11204`) uses action-free surgical video pretraining
  before action-labeled fine-tuning and reports improved closed-loop control,
  but evaluation is limited to four simulated surgical tasks and no public
  implementation was linked.
- **VIScore** (`2608.11174`) diagnoses planning-relevant latent-world-model
  quality through Veracity, Influence, and Sobriety and reports strong
  cross-task success correlation. It is a promising metric contribution, but no
  official artifact or robot-specific evaluation was verified.
- **JEPA-WAM: Stage-Level Joint-Embedding Prediction** (`2608.10780`) adds a
  semantic stage future to short-horizon physical prediction and reports gains
  across 50 RoboTwin 2.0 tasks. It is primarily a model contribution, with no
  official code or real-robot evidence linked.
- **GESTO** (`2608.10886`) and **PBD-AG** (`2608.10449`) develop structured
  long-horizon scene memory, but neither exposed a verified reusable robot-agent
  runtime or artifact in this scan.
- **XCoT-VLA** (`2608.10976`) and **DriveVLA-M0** (`2608.10413`) concern
  autonomous driving, which is outside this repository's robot-manipulation
  focus.
- Narrow manipulation policies, perception systems, teleoperation, navigation,
  and generic video world models were excluded unless they introduced a
  reusable state, recovery, serving, safety, or evaluation interface.

## Artifact-status checks

- Flex-π: official project and repository verified. The repository explicitly
  says code and checkpoints are forthcoming; no runnable implementation or
  weights were present as of 2026-08-12.
- REDAgentBench, Surgical WAM, VIScore, and the stage-level JEPA-WAM: no official
  code, dataset, benchmark, or weights repository was linked from the arXiv
  record or found through official repository search as of 2026-08-12.
