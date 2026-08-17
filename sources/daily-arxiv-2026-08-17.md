# Daily arXiv scan — 2026-08-17

## Scope

- Interval: after the 2026-08-16 verification through 2026-08-17 06:48 UTC.
- The official arXiv API exposed the 2026-08-14 late submission batch, which had
  not been visible at the prior weekend check. Screening covered `cs.RO`,
  `cs.AI`, `cs.CL`, `cs.CV`, and `cs.LG` across harness engineering, recovery,
  agentic robotics, VLA, world models, skill discovery, evaluation, and safety.
- Metadata and claims were checked against official arXiv pages, author/project
  pages, and official repositories. Existing repository entries were checked for
  duplicates. All performance results below remain author-reported.

## Included

### AgentRewind: Recoverable Execution for Long-Horizon LLM Agents

- Paper: https://arxiv.org/abs/2608.14380
- Classification: General Harness Methodology; Recovery.
- Contribution: aligned checkpoints of agent context and controlled environment
  state permit rewind and informed resumption after errors propagate.
- Boundary: evaluated on long-horizon engineering tasks, not robots. Physical
  migration requires safe reset or compensating actions; no official code or
  MettleBench release was linked as of 2026-08-17.

### HELIX: Model-Harness Co-evolution for Recursive Self-Improvement

- Paper: https://arxiv.org/abs/2608.13951
- Classification: General Harness Methodology; Self-improvement.
- Contribution: source-traceable typed harness artifacts preserve intervention
  identity and provenance while verified sibling trajectories supply subsequent
  model-training data.
- Boundary: one code-repair evolution round, with no robot experiment. The arXiv
  record claims code at https://github.com/HKUDS/HELIX, but the URL returned 404;
  it is therefore not treated as an available implementation.

### Evolve Vision-Language-Action Model into an Agent with On-the-fly Tool-use

- Paper: https://arxiv.org/abs/2608.14047
- Classification: Agentic Robot/VLA Harness; VLA.
- Contribution: ART injects low-level vision, affordance, and embodiment tools
  into VLA trajectories and trains long-horizon tool use with 30K trajectories.
- Boundary: the CVPR 2026 Findings paper reports simulation and real-world gains,
  but no official project, code, dataset, or model release was linked.

### Reflex: Enabling Fast and Predictive Vision-Language-Action Models

- Paper: https://arxiv.org/abs/2608.14379
- Project: https://reflexvla.github.io/
- Classification: VLA; Evaluation/Safety; Runtime.
- Contribution: ReflexBench advances simulation independently of policy inference
  and supports configurable latency and synchronous/asynchronous control;
  ReflexVLA combines future prediction, temporal fusion, and serving acceleration.
- Boundary: simulation and three real-world dynamic-task results are
  author-reported. The project explicitly says code will be released after
  acceptance, so code, model, and benchmark are not currently available.

## Important revision

- **StageWAM** (`2608.10780v3`) renames the prior JEPA-WAM paper and clarifies the
  Stage-JEPA framing. The reported 50-task RoboTwin 2.0 evidence and lack of an
  official implementation are unchanged, so it was recorded but not promoted to
  the curated main list.

## Reviewed but not promoted

- **PRM-as-a-Judge 1.5** (`2608.14284`) evaluates robot process assessment, but
  does not introduce a reusable harness contract or verified public artifact.
- **BICPO-VLA** (`2608.13924`) addresses asynchronous handoff smoothness, but
  overlaps existing real-time chunk-serving coverage and has no verified code or
  real-robot evidence.
- **MedClaw** (`2608.14015`) is a heuristic harness for surgical-video reasoning,
  not robot execution.
- **Ensuring Safe Physical AI in Urban Mobility** (`2608.14481`) and the
  articulated-vehicle safety-certificate paper (`2608.14531`) focus on classical
  safety envelopes rather than learned robot-agent or VLA harnesses.
