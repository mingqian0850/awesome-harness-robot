# Daily arXiv scan — 2026-07-31

## Scope

- Catch-up interval: **2026-07-28 00:00 UTC through 2026-07-31**.
- Latest relevant arXiv metadata available during the scan: **2026-07-30**.
- Primary source: arXiv API, latest 200 `cs.RO` submissions sorted by
  `submittedDate`, followed by checks of arXiv abstract pages and official
  project or code pages.
- Themes: agent harnesses, agentic robotics, hierarchical and self-improving
  agents, VLA deployment and recovery, runtime verification, robot foundation
  models, world models, code-as-policy, and evaluation.
- Existing `README.md`, `docs/landscape.md`, and `sources/` entries were checked
  for duplicates before selection.

The optional `research-lookup` backends were unavailable because `parallel-cli`
and its API credentials were not configured. The scan therefore used first-party
arXiv metadata and official artifact pages directly.

## Included

### RoboBRIDGE: A Modular Framework for Bridging Policies to Robust Real-World Robotic Agents

- Paper: https://arxiv.org/abs/2607.27881
- DOI: https://doi.org/10.48550/arXiv.2607.27881
- Submitted: 2026-07-30; accepted to IROS 2026.
- Classification: Agentic Robot / VLA Harness.
- Why included: directly defines a modular orchestration layer around pretrained
  policies with Monitor, Perceptor, Planner, Controller, and Robot Interface
  components. It covers failure detection, hierarchical recovery, asynchronous
  perception, replanning, and embodiment abstraction.
- Evidence boundary: authors report LIBERO, RoboCasa, and multi-platform
  real-robot case studies. No official public code repository was located.

### Embodied Agents Take Control: Minimal-Interface Zero-Shot Agents Rival Industrial-Scale Policies in Vision-and-Language Navigation

- Paper: https://arxiv.org/abs/2607.26148
- DOI: https://doi.org/10.48550/arXiv.2607.26148
- Submitted: 2026-07-28.
- Classification: General Harness Methodology / Agentic Robot Evaluation.
- Why included: directly evaluates three general software-agent harnesses holding
  a long perceive–act–verify–correct loop through a minimal camera/action
  interface. It isolates model choice, tool-interface effects, latency, context
  growth, and long-horizon degradation.
- Evidence boundary: zero-shot navigation experiments; no official code link was
  present on the arXiv page.

### Practice Makes Policies: Bootstrapping and Consolidating Robotic Capabilities from Zero Human Demonstrations

- Paper: https://arxiv.org/abs/2607.26809
- DOI: https://doi.org/10.48550/arXiv.2607.26809
- Submitted: 2026-07-29.
- Classification: Self-Improving Agentic Robot Harness.
- Why included: HERO combines heuristic reasoning, exemplar reuse, autonomous
  experience collection, capability scheduling, and consolidation into
  closed-loop visuomotor policies.
- Evidence boundary: author-reported manipulation experiments and reduced human
  intervention; no official code link was present on the arXiv page.

### CheckVLA: Execution-Time Verification with Action-Conditioned World Model for Long-Horizon Mobile Manipulation

- Paper: https://arxiv.org/abs/2607.26789
- DOI: https://doi.org/10.48550/arXiv.2607.26789
- Submitted: 2026-07-29.
- Classification: VLA Harness / Runtime Verification.
- Why included: adds a separately trained frozen world-model verifier, conformal
  intervention threshold, latency-aware suffix replacement, and keyframe memory
  around open-loop VLA action chunks.
- Evidence boundary: results are simulation-only on RoboCasa365; no official
  code link was present on the arXiv page.

### World Action Planner: Generalizable Decision-Making with Action-Conditioned World Models

- Paper: https://arxiv.org/abs/2607.27599
- DOI: https://doi.org/10.48550/arXiv.2607.27599
- Project: https://worldactionplanner.github.io/
- Submitted: 2026-07-30.
- Classification: Robot Foundation / World Model.
- Why included: combines VLM plan proposals with optimization and search over
  action-conditioned visual rollouts, exposing an explicit planner/world-model
  interface rather than using prediction only as an auxiliary loss.
- Evidence boundary: the abstract reports compositional, layout, and zero-shot
  generalization; no public code repository was verified.

### VisualPatchWorld: Code World Models as Latent Structured Representations for Planning

- Paper: https://arxiv.org/abs/2607.25236
- DOI: https://doi.org/10.48550/arXiv.2607.25236
- Code: https://github.com/HKBU-KnowComp/VisualPatchWorld
- Submitted: 2026-07-28.
- Classification: Robot World Model / Code-as-Policy.
- Why included: induces inspectable executable dynamics from probes and
  state–action traces, then uses the resulting programs inside model-predictive
  control. The MIT repository includes source, induced models, reports, and
  reproduction scripts.
- Evidence boundary: author-reported results cover four simulated control
  environments, with a remaining gap on contact-rich pushing.

### TurboVLA: Real-Time Vision-Language-Action Model at 32 Hz on an RTX 4090 with <1 GB VRAM

- Paper: https://arxiv.org/abs/2607.27205
- DOI: https://doi.org/10.48550/arXiv.2607.27205
- Code: https://github.com/H-EmbodVis/TurboVLA
- Submitted: 2026-07-29.
- Classification: VLA / Deployment-Efficient Foundation Model.
- Why included: replaces the recurrent LLM-centric `V -> L -> A` path with a
  compact direct vision/language-to-action design and releases official training
  and evaluation code.
- Evidence boundary: the authors report 97.7% LIBERO success, 31.2 ms latency,
  and 0.9 GB inference VRAM on an RTX 4090. These are upstream results under the
  stated benchmark and hardware protocol, not independent verification.

## Reviewed but not added to the main list

- **RedFlow** (`2607.27782`): useful offline-RL treatment of failed VLA rollout
  data, but primarily a policy-training method rather than a reusable harness.
- **RL2-VLA** (`2607.26991`): modular failure-triggered latent steering is
  relevant to deployment, but its main contribution is VLA inference-time policy
  adaptation; retain as a future model/post-training candidate.
- **DR-LfD** (`2607.25397`): strong TAMP-gated skill composition, but overlaps
  with already represented planner-plus-skill systems and is less directly about
  foundation-model harnessing.
- **DC-WAM** (`2607.25918`): improves future-video supervision for policy
  learning, but is a world-action-model architecture contribution rather than a
  distinct runtime interface.
- **Causality-aware IDR** (`2607.25516`): training-free VLA modality refinement,
  but currently fits better as an adjacent test-time adaptation method.
- **Temporal-Distance JEPA** (`2607.25337`) and **QQWorld** (`2607.28415`):
  relevant latent-world-model improvements, but narrower than the selected
  planner-facing world-model artifacts.
- Navigation-only, autonomous-driving, conventional control, SLAM, sensing,
  hardware, and domain-specific papers were excluded unless they introduced a
  reusable harness, foundation-model interface, or broadly useful evaluation
  pattern.
