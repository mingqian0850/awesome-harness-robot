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

---

# Second pass — 2026-08-17 ~08:10 UTC

## Scope

- Interval: after the first-pass cutoff at 2026-08-17 06:48 UTC through
  ~08:20 UTC.
- The arXiv API still exposes only the 2026-08-14 announcement batch (247
  records, max ID `2608.14546`). A fresh `submittedDate:[202608150000 TO
  202608172359]` query returns zero records, so no 08-15/08-16/08-17 batch has
  appeared; no revisions were indexed in the `lastUpdatedDate` window (the API
  normalizes that field to `submittedDate`).
- Because the visible batch was unchanged, this pass re-screened all 247
  records of the 08-14 batch for candidates the first pass did not record, and
  re-verified every watch-list item against official pages and repositories.
  All performance results remain author-reported.

## Included (additions to the curated list)

### Twin: Playing an Unknown Game with a Test-Time Digital Twin

- Paper: https://arxiv.org/abs/2608.14490
- Code: https://github.com/Alexyskoutnev/TWIN-ARC-AGI-3 (MIT, verified, Python)
- Replays: https://arc-agi-3-twin.vercel.app/
- Classification: General Harness Methodology; World Model; Recovery.
- Why included: verification-gated test-time world-model harness. Scored
  actions are withheld until an executable model reproduces every observed
  transition; each prediction mismatch becomes a counterexample that repairs
  the model. The authors report 97.8% of ARC-AGI-3 levels cleared vs 7.8% for
  the base model played directly. A real, runnable MIT artifact was verified.
- Boundary: game-domain (ARC-AGI-3), not robotics; robot transfer would need
  observation and physics grounding.

### Engineering Reliable Coding Agents: Evaluating and Operating the System Around the Model

- Paper: https://arxiv.org/abs/2608.13867
- Companion: https://github.com/sjarmak/engineering-reliable-coding-agents
  (Apache-2.0, verified; TeX source, protocols, catalog, skills)
- Classification: General Harness Methodology; Evaluation/Safety.
- Why included: a 314-page engineering monograph plus runnable evaluation and
  operation protocols, a machine-readable catalog of 206 reliability records,
  an evidence ledger, and five reusable agent skills that separate model
  capability from harness, state, retrieval, permissions, and observability
  effects.
- Boundary: digital coding agents; no robot experiment.

### Agentic Transaction: Towards ACID-Compliant Agent Systems

- Paper: https://arxiv.org/abs/2608.13900
- Classification: General Harness Methodology; Recovery.
- Why included: first-class semantic ACID (atomicity, consistency, isolation,
  durability) for long-horizon agent execution, instantiated in a data agent
  with exploration–execution–validation cycles; authors report a 10.6%
  improvement over prior agents including Claude Code.
- Boundary: no official code or repository link was located as of 2026-08-17;
  digital-agent benchmarks.

### Onto-EV-WM: Ontology-Grounded World Models for Failure Diagnosis and Closed-Loop Repair in Physical AI Systems

- Paper: https://arxiv.org/abs/2608.13901
- Classification: Robot Foundation/World Model; Evaluation/Safety.
- Why included: turns world-model quality signals into typed task predicates,
  correction-route labels, and verification-gated acceptance, giving physical-AI
  agents a diagnosis-and-repair interface rather than only scores.
- Boundary: simulation-only (PointMaze, LIBERO; 85.00% success over the
  10,030-task LIBERO-Plus registry, author-reported); real-robot recovery is
  not evaluated; no official code was linked as of 2026-08-17.

### ScienceFlow: A long-horizon agent for ML research, scientific discovery and beyond

- Paper: https://arxiv.org/abs/2608.14354
- Classification: General Harness Methodology; Self-improvement.
- Why included: autoresearch harness with recoverable executable research
  state, ESTRA re-anchoring between live and archived states, and
  evidence-gated compute allocation; authors report a 70.22% Any-Medal score on
  MLE-bench within a 24-hour budget, 4.92 points above prior reported results.
  Complements the existing ENPIRE physical-autoresearch entry.
- Boundary: ML/scientific research, not robotics; no official code or
  repository link was located as of 2026-08-17.

## Reviewed but not promoted (second pass)

- **FlatLab** (`2608.14049`) — ICML 2026 flat-object manipulation framework and
  simulation benchmark, but the cited project page is a placeholder with no
  code or benchmark release and no repository was found. Watch for a real
  release.
- **AdvDex** (`2608.14028`) — dexterous-manipulation VLA with the OmniShare
  dataset; no official project, code, or data link was located.
- **Mandato** (`2608.14074`) — protocol-level enforcement of signed mandates on
  MCP agent actions with hash-chained audit trails; the reference implementation
  is described but not released.
- **MemoryLake on MemoryArena** (`2608.13883`) — matched agent memory-backend
  study; no official code, and most observed differences fall within
  overlapping confidence intervals.
- **OpenBelief-Nav** (`2608.13923`) — evidence-preserving object memory for
  language-guided navigation; code is promised only upon acceptance.
- **SSP** (`2608.14024`) — event-matched Syn2Sim2Phy evaluation framework for
  autonomous-driving VLAs; no artifacts, driving-domain.
- **Graph-RL drift recovery** (`2608.14109`) — plug-and-play RL recovery module
  for autonomous LLM agents; no official code, digital-agent scope.
- **Traj-LeWM** (`2608.14125`) — path-aware world-model planning; incremental
  over LeWM, no code.
- **Envs-FORGE** (`2608.14312`) — reward-grounded environment synthesis for
  agent RL; code sits in a general-purpose synthetic-data toolkit and the
  contribution is tangential to robot harnesses.
- **Marionette** (`2608.14530`) — explicit-state game world model; game-domain,
  no robot interface.
- **Handover of In-Context Learning State** (`2608.14528`) — theory of session
  handover; no harness artifact.

## Watch-list status (rechecks)

- **DreamX-Phi 1.0** (`2608.13489`) — official repo still documentation-only
  (4 KB) and still postpones weights/inference code until after the WorldArena
  2.0 IROS Challenge. Unchanged.
- **UniTexture** (`2608.13453`) — still no official attack code or benchmark
  artifacts. Unchanged.
- **StageWAM** (`2608.10780`) — still v3 (last updated 2026-08-14); no new
  version, code, or weights. Unchanged.
- **HELIX** (`2608.13951`) — `HKUDS/HELIX` still returns 404. Unchanged.
- **ReflexVLA / ReflexBench** (`2608.14379`) — still pre-release ("after
  acceptance"). Unchanged.
