# Daily arXiv scan — 2026-08-03

## Scope

- Incremental interval: after the 2026-08-02 11:22 UTC successful scan through
  2026-08-03.
- Official arXiv API queries:
  - latest 200 `cs.RO` records sorted by `lastUpdatedDate`;
  - latest 100 records matching agent harness, robot agent,
    vision-language-action, robot foundation model, robot world model, or
    agentic robot, also sorted by `lastUpdatedDate`.
- Broader first-party sweep: arXiv pages, official project pages, official
  GitHub repositories, and official checkpoint collections for high-value VLA
  and robot-foundation-model omissions.
- Existing `README.md`, `docs/landscape.md`, and `sources/` entries were checked
  for duplicates before selection.

`parallel-cli` was installed but unauthenticated, so the scan used arXiv's
official API and first-party artifact pages directly.

## Incremental arXiv result

No records in either official API query had an arXiv `updated` timestamp after
the previous successful scan. At query time, the Monday announcement batch had
not produced an in-scope strict-incremental candidate. No paper is represented
here as a new 2026-08-03 submission.

## Included backlog corrections

### LingBot-VLA 2.0: From Foundation to Application

- Paper: https://arxiv.org/abs/2607.06403
- Official project: https://technology.robbyant.com/lingbot-vla-v2
- Code: https://github.com/Robbyant/lingbot-vla-v2
- Checkpoints: https://huggingface.co/collections/robbyant/lingbot-vla-v2
- Submitted: 2026-07-07.
- Classification: VLA / Robot Foundation Model / Open and Reproducible.
- Why included: exposes a concrete cross-embodiment implementation boundary—a
  55-dimensional canonical state/action schema, sparse MoE action expert, and
  predictive-dynamics distillation—across bimanual and mobile-manipulation
  settings. The repository includes training, evaluation, and custom-data
  tooling, pretraining weights, and a RoboTwin post-trained checkpoint.
- Evidence boundary: all benchmark and real-robot numbers are author-reported.
  The paper evaluates two long-horizon tasks on two embodiments with 15 trials
  per setting; its out-of-distribution success rates remain substantially below
  in-domain rates, so it should not be described as solved general deployment.

### Qwen robotics foundation-model family

- Qwen-VLA paper: https://arxiv.org/abs/2605.30280
- Qwen-VLA official repository: https://github.com/QwenLM/Qwen-VLA
- Qwen-RobotManip paper: https://arxiv.org/abs/2606.17846
- Qwen-RobotManip official repository:
  https://github.com/QwenLM/Qwen-RobotManip
- Qwen-RobotNav paper: https://arxiv.org/abs/2606.18112
- Qwen-RobotNav official repository: https://github.com/QwenLM/Qwen-RobotNav
- Submitted: 2026-05-28 and 2026-06-16.
- Classification: VLA / Robot Foundation Model / Agentic Navigation / Partial
  or Closed.
- Why included as one family: Qwen-VLA defines a shared manipulation,
  navigation, and trajectory-prediction model; RobotManip focuses on aligning
  heterogeneous embodiments, motions, cameras, and behavior at scale; RobotNav
  exposes task mode and observation-budget controls to an upper-level agent.
  Grouping them avoids duplicating a tightly related official model family.
- Evidence boundary: simulation, benchmark, and real-robot results are reported
  by the authors. The official repositories contain papers, descriptions, and
  demos rather than training/inference implementations or usable checkpoints.
  RobotManip and RobotNav explicitly state that weights are not planned for
  release; Qwen-VLA had no official code or checkpoint release as of 2026-08-03.

### Qwen-RobotWorld

- Paper: https://arxiv.org/abs/2606.17030
- Official blog: https://qwen.ai/blog?id=qwen-robotworld
- Submitted: 2026-06-15.
- Classification: Robot Foundation / World Model / Partial or Closed.
- Why included: treats natural language as a common action-conditioning
  interface across manipulation, navigation, autonomous driving, and
  human-to-robot transfer, and positions predicted video for synthetic data,
  policy evaluation, and downstream planning.
- Evidence boundary: benchmark rankings and generalization claims are
  author-reported. No official implementation or model weights were located.

## Reviewed but not separately listed

- **LingBot-VLA 1.0** (`2601.18692`) has open code and weights, but version 2.0
  is the more current, broader, and directly reproducible family representative.
- **Qwen-RobotManip and Qwen-RobotNav** are not separate README bullets because
  their shared Qwen foundation-model lineage and identical artifact boundary are
  clearer as one curated family entry.
- **Qwen-AgentWorld** targets software-agent environment simulation rather than
  robotics; it was excluded from the robot world-model section despite the
  adjacent naming.
- Navigation-only, autonomous-driving, conventional control, sensing, and
  hardware results were excluded unless they contributed a reusable agent
  interface, foundation-model contract, or evaluation pattern.
