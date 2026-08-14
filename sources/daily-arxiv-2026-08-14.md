# Daily arXiv scan — 2026-08-14

## Scope

- Gap interval: after the 2026-08-13 scan cutoff at 10:48 UTC through
  2026-08-14 08:49 UTC. The target-category new listings had advanced to the
  Friday, 14 August 2026 announcement batch.
- The arXiv API returned 324 records submitted on 2026-08-13 and 2026-08-14
  across `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, and `cs.LG`. Screening covered
  agent harnesses, self-improvement, robot agents, VLAs, WAMs, skill evolution,
  memory, recovery, runtime monitoring, evaluation, and safety.
- Verification used official arXiv pages and official project/code repositories.
  Existing README, landscape, and source records were checked for duplicates.
  Performance numbers remain author-reported.

## Included

### ContactGuard: Pre-Contact Execution Monitoring with Action-Conditioned Latent World Models

- Paper: https://arxiv.org/abs/2608.13438
- Submitted: 2026-08-13.
- Classification: Agentic Robot/VLA Harness; Evaluation/Safety; Runtime.
- Why included: predicts the short-horizon latent consequence of a policy's own
  planned action chunk and aborts before imminent contact when the predicted
  post-contact state indicates likely failure. The action-conditioned latent
  world model uses unlabeled multi-view trajectories; a lightweight failure
  probe needs only a small labeled pre-contact set.
- Evidence boundary: the authors report real-world contact-rich manipulation and
  live-robot pre-contact abort experiments without modifying the underlying
  policy. No official code or project link was located as of 2026-08-14.

### Decoding Task Progress from VLA Representations

- Paper: https://arxiv.org/abs/2608.13474
- Submitted: 2026-08-13.
- Classification: VLA; Evaluation/Safety; Runtime.
- Why included: finds that normalized task time remaining is linearly readable
  from π0.5 residual activations, generalizes a simple probe to unseen tasks, and
  uses stalled predicted progress as a label-free deployment monitor. The probe
  is observational rather than a steering mechanism.
- Evidence boundary: interpretability, counterfactual-language, and OOD-monitor
  results are author-reported. No official implementation or probe weights were
  linked as of 2026-08-14.

### Practice Makes Unsafe: Skill Misevolution in Self-Improving LLM Agents

- Paper: https://arxiv.org/abs/2608.12851
- Code: https://github.com/henrymao2004/misevolve
- Submitted: 2026-08-13.
- Classification: General Harness Methodology; Evaluation/Safety.
- Why included: models the complete persistent-skill lifecycle—authoring,
  retrieval, later execution, and fresh-session carryover—rather than measuring
  only the triggering attack. SkillMisevo-Gym versions skill state across Claude
  Code, Codex, OpenClaw, and Hermes containers; SkillMisevo-Bench uses paired
  malicious/benign tasks and nine lifecycle metrics. SafeEvolve governs both
  writes and later reuse.
- Evidence boundary: results are for digital coding agents, not robots. The
  robotics transfer is architectural: learned skill admission, provenance,
  retrieval, and execution permissions must be governed separately. The public
  repository contains benchmark code, harness containers, configs, scripts, and
  metrics; it does not declare a license as of 2026-08-14.

## Reviewed but not promoted

- **UniTexture** (`2608.13453`) demonstrates a single textured 3D object that
  transfers targeted VLA action deviations across tasks and models, but no
  official attack benchmark or code was verified. It remains a strong safety
  candidate pending artifacts or independent reproduction.
- **HARD / Beyond Handcrafted Security** (`2608.12977`) evolves harness-level
  runtime defenses from failure traces, but overlaps SHE's safety-harness
  evolution contribution and exposes no verified implementation.
- **DreamX-Phi 1.0** (`2608.13489`) is a geometry-aware action-conditioned robot
  video world model with an official MIT-licensed repository, but that repository
  contains only documentation and explicitly postpones weights and inference code
  until after the WorldArena 2.0 IROS Challenge. It is not yet open source in the
  operational sense.
- **AutoDesign** (`2608.13560`) releases a substantial meta-harness optimization
  implementation, but its evaluated domain is paper-to-poster/media design and it
  has no robot experiment; it is too application-specific for this robot-centered
  list.
- **Capability Sheaves** (`2608.13228`) honestly reports no supported real-world
  cohomological advantage on its repository stress test, so it was not promoted.
- **S2-HWM** (`2608.13103`) introduces event-structured hierarchy for simulated
  surgical manipulation, but lacks verified artifacts and real-robot evidence.
- **H2R-Bench** (`2608.13049`) and **HumanoidVLN** (`2608.12860`) are relevant
  benchmarks, but focus respectively on video generation and navigation rather
  than reusable manipulation-harness interfaces.
- Driving, medical, generic video-generation, perception-only, and conventional
  control records were excluded unless they introduced reusable robot-harness,
  safety, recovery, or evaluation contracts.

## Artifact-status checks

- SkillMisevo: official public implementation verified, including four pinned
  agent harness containers, skill injection, isolated execution, configuration,
  lifecycle metrics, and experiment scripts. No repository license was declared
  as of 2026-08-14.
- DreamX-Phi: official repository verified, but it states that weights and
  inference code will be released only after the challenge; currently a
  documentation placeholder rather than a runnable release.
- ContactGuard, the VLA task-progress probe, UniTexture, and HARD: no official
  implementation, benchmark, dataset, or weights repository was linked from the
  arXiv record or located through official repository search as of 2026-08-14.
