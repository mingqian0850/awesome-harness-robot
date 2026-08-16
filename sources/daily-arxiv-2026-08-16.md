# Daily arXiv scan — 2026-08-16

## Scope

- Gap interval: after the 2026-08-14 scan cutoff through 2026-08-16 08:08 UTC,
  covering the missed 2026-08-15 run and the current run.
- The official arXiv API showed no target-topic submission or revision newer than
  2026-08-13 17:59 UTC; the weekend therefore produced no new announcement batch.
- Queries covered robot/robotics, agent harnesses, harness engineering,
  self-improvement, robot agents, VLA, world-action models, code-as-policy,
  memory, recovery, evaluation, and safety. Existing README, landscape, and
  source records were checked for duplicates.

## Curated backfill

### Harness Engineering for Physical AI: Robot Middleware Is the Harness Layer

- Paper: https://arxiv.org/abs/2606.09416
- Submitted: 2026-06-08.
- Classification: Agentic Robot/VLA Harness; Evaluation/Safety; Middleware.
- Why included: explicitly defines robot middleware as the Physical AI harness
  layer and proposes Projection, Isolation, and Transfer as composable
  enforcement functions across control, compute, and communication. The proposed
  ROS 2 Harness Profile is directly aligned with this repository's systems scope.
- Evidence boundary: this is a position and architecture paper, not an implemented
  profile or experimental robot evaluation. No official code was linked as of
  2026-08-16.

### RHO: Your Coding Agent is Secretly a Roboticist

- Paper: https://arxiv.org/abs/2606.16458
- Submitted: 2026-06-15.
- Classification: Agentic Robot/VLA Harness; Code-as-Policy; Harness Optimization.
- Why included: Robotics Harness Optimization searches over interpretable
  multi-file repositories-as-policies using a bounded coding agent, reflective
  execution feedback, and Pareto-frontier selection during training. The selected
  artifact runs without deployment-time code generation.
- Evidence boundary: Robosuite, LIBERO-PRO, and O3DE results are author-reported
  and simulation-only. No official code repository was linked as of 2026-08-16.

## Reviewed but not promoted

- No new or newly revised target-topic arXiv records appeared after the previous
  scan cutoff. The latest matching records remained the 2026-08-13 batch already
  screened on 2026-08-14.
- AutoDesign remains outside the robot-centered scope because its evaluated domain
  is paper-to-poster generation, despite its meta-harness contribution.
- DreamX-Phi remains a documentation-only release: its repository still promises
  weights and inference code after the WorldArena 2.0 challenge rather than
  providing a runnable release.
- UniTexture remains a strong VLA-safety candidate, but no official code or
  benchmark artifacts were verified.

## Verification notes

- Metadata, method descriptions, experimental scope, and artifact status were
  checked against official arXiv records and official links exposed there.
- Performance claims are attributed to the authors; no independent replication
  was identified.
