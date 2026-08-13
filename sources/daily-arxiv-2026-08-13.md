# Daily arXiv scan — 2026-08-13

## Scope

- Gap interval: after the 2026-08-12 scan cutoff at 12:48 UTC through
  2026-08-13 10:48 UTC. The target-category new listings had advanced to the
  Thursday, 13 August 2026 announcement batch.
- The arXiv API returned 296 records submitted on 2026-08-12 and 2026-08-13
  across `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, and `cs.LG`. Title-and-abstract
  screening covered harness construction and evaluation, agentic robots, VLAs,
  WAMs, memory, recovery, skills, runtime, and safety.
- Harness-IF was submitted before the prior wall-clock cutoff but first appeared
  in the newly available announcement batch, so it is included as a recovered
  announcement-gap item. Existing repository records were checked for duplicates.
- Verification used official arXiv records and searches for official project or
  code pages. All performance results below are author-reported.

## Included

### AI4AI at Test-Time: Strong-to-Weak Capability Transfer via Harnesses

- Paper: https://arxiv.org/abs/2608.12307
- Submitted: 2026-08-12.
- Classification: General Harness Methodology.
- Why included: a stronger builder model iteratively constructs an inference-time
  harness for a weaker frozen target without parameter updates. The reported
  gains mainly come from deterministic code, benchmark-specific routing, and
  strict output enforcement rather than longer reasoning or more sampling.
- Evidence boundary: four Theory-of-Mind benchmarks are used, with 5% of each
  dataset used to refine the harness and the finalized harness evaluated on the
  full test set. There are no robot experiments, and no official code link was
  located as of 2026-08-13.

### Harness-IF: Evaluating Instruction Following Across Instruction Surfaces in Coding Agents

- Paper: https://arxiv.org/abs/2608.11727
- Submitted: 2026-08-12.
- Classification: General Harness Methodology; Evaluation.
- Why included: evaluates operational rules individually from execution evidence
  across five configurable harness surfaces. Against-Prior Accuracy reruns tasks
  without a rule to distinguish genuine compliance from behavior the model would
  have produced anyway; a conflict pilot also tests precedence between surfaces.
- Evidence boundary: the 60-item, 642-rule-library study covers coding agents,
  not robots. A robotic adaptation would need rules tied to observable state and
  immutable host-side safety enforcement. No official benchmark repository was
  linked as of 2026-08-13.

### StellaVLA: In-Context Structured Demonstration for Generalizable VLAs

- Paper: https://arxiv.org/abs/2608.11671
- Submitted: 2026-08-12.
- Classification: VLA; Agentic Robot/VLA Harness.
- Why included: an offline pipeline converts a retrieved trajectory into a task
  plan, subgoals, and verbalized 3D motion, giving the VLA a single structured
  in-context demonstration. The interface accepts robot, human-hand, and XR
  demonstrations, while dual training removes the language path at deployment
  so high-rate action inference adds no stated latency.
- Evidence boundary: VLA-Arena, LIBERO, LIBERO-Plus, and real-robot results are
  author-reported. No official project, code, or weights link was located as of
  2026-08-13.

### Keep the Future, Drop the Rollout: RIFT for World Action Models

- Paper: https://arxiv.org/abs/2608.11521
- Submitted: 2026-08-12.
- Classification: Robot Foundation/World Model; Runtime.
- Why included: paired interventions first show that WAM action generation uses
  future-cache content but, for tested models, can consume a fixed final-clean
  cache. RIFT then learns anticipation tokens that construct the complete future
  key/value cache in one backbone pass while retaining the existing future-read
  interface, removing iterative video rollout at deployment.
- Evidence boundary: the authors report 68.2–89.1% lower action-chunk latency and
  competitive LIBERO/RoboTwin success. No real-robot result or official code
  link was located as of 2026-08-13.

## Reviewed but not promoted

- **ForeWAM / Foresight Without Seeing** (`2608.11605`) is another efficient
  latent-future WAM, using a Video-DiT prefill and reusable layer-wise K/V state.
  It overlaps RIFT's deployment concern and has no verified artifacts or
  real-robot evaluation; RIFT was selected for its explicit causal cache tests.
- **G0.5** (`2608.11739`) unifies reasoning and action tokens in one
  autoregressive stream, but it is primarily a foundation-model architecture and
  exposes no verified code or weights.
- **DreamFly** (`2608.12308`) combines causal memory with receding-horizon
  diffusion planning for aerial navigation; navigation is outside the list's
  manipulation focus and no artifact was verified.
- **Diagnosis Before Recovery** (`2608.11772`) proposes selective agent repair,
  but it is a generic digital-agent method without robot evidence or a verified
  implementation.
- **Policy-Induced Hand Priors** (`2608.11769`) is a useful VLA diagnostic for
  initial-pose dependence, but it is narrower than the reusable evaluation and
  runtime interfaces prioritized here.
- **D3D-GEN** (`2608.11876`) generates social-robot simulation worlds, but its
  focus is environment generation rather than a robot-agent harness or policy.
- Driving, generic video generation, perception-only, locomotion/control, and
  domain-specific digital-agent benchmarks were excluded unless they introduced
  reusable harness, recovery, safety, or robot evaluation interfaces.

## Artifact-status checks

- AI4AI, Harness-IF, StellaVLA, RIFT, ForeWAM, and G0.5: no official project,
  implementation, benchmark, dataset, or weights repository was linked from the
  arXiv record or located through official repository search as of 2026-08-13.
