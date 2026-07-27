# Daily arXiv Scan — 2026-07-27

Search performed on 2026-07-27 for agent harnesses, agentic robotics, hierarchical
and memory-augmented VLAs, robot foundation/world models, recovery, safety, and
evaluation. The keyword query was cross-checked against the latest 100 `cs.RO`
submissions sorted by submission date.

The newest `cs.RO` records available through the arXiv API were submitted on
2026-07-24. This is consistent with the first scan after the weekend.

## Included

### Physical Agency / Pigey

- Paper: https://arxiv.org/abs/2607.21725
- DOI: https://doi.org/10.48550/arXiv.2607.21725
- Submitted: 2026-07-23
- Decision: include under Agentic Robot and VLA Harnesses.
- Reason: directly studies the orchestration gap around frozen VLA policies and
  parameterized skills, with closed-loop planning, subgoal decomposition,
  verification, recovery, simulation, and real-robot experiments.
- Availability: no official project or code link was found on the arXiv record
  or through a title search when checked.

### Robot-Factored World Models via Robot Rendering

- Project: https://bjkim95.github.io/rofacto/
- Paper: https://arxiv.org/abs/2607.22535
- DOI: https://doi.org/10.48550/arXiv.2607.22535
- Submitted: 2026-07-24
- Decision: include under Robot Foundation and World Models.
- Reason: factors action realization and embodiment geometry out of the learned
  predictor through controller rollout and camera-aligned URDF rendering,
  creating an explicit cross-embodiment world-model interface.
- Availability: the project page exposed a code link, but the linked GitHub URL
  returned 404 when checked; it is not described as released code.

### ViTacWorld

- Project: https://vitacworld.github.io/
- Paper: https://arxiv.org/abs/2607.22530
- DOI: https://doi.org/10.48550/arXiv.2607.22530
- Submitted: 2026-07-24
- Decision: include under Robot Foundation and World Models.
- Reason: predicts synchronized visual and tactile outcomes and uses imagined
  action-conditioned rollouts for contact-policy augmentation and evaluation.
- Availability: project page marked GitHub as "Coming Soon".

### FORGE-plus

- Paper: https://arxiv.org/abs/2607.21227
- DOI: https://doi.org/10.48550/arXiv.2607.21227
- Submitted: 2026-07-23
- Decision: include under Runtime, Safety, and Observability.
- Reason: demonstrates a clear authority boundary in which an LLM may select a
  force ceiling and bounded recovery action but cannot override low-level force
  enforcement.
- Limitation: all reported experiments are rigid-body simulation and the paper
  explicitly makes no sim-to-real claim.

## Reviewed but not included

- **MemoHarness** (`2607.14159`): highly relevant general harness research, but
  outside this daily announcement window and not newly revised. Retain as a
  backlog candidate for a dedicated general-harness sweep.
- **Zero-Shot Mission-Level Evaluation for Aerial MLLM Agents** (`2607.22014`):
  relevant evaluation research, but narrower than the current curated benchmark
  set and without a verified reusable artifact in this scan.
- **One Hand Watches The Other** (`2607.22119`): relevant bimanual multi-agent
  policy learning, but primarily a task-policy contribution rather than a
  foundation-model or harness system.
- Autonomous-driving VLAs, conventional control, SLAM, sensing, hardware, and
  biomedical robotics papers were excluded unless they contributed a reusable
  harness, foundation-model interface, or general evaluation pattern.
