# Daily arXiv scan — 2026-08-10

## Scope

- Gap interval: covers the announcement data available after the 2026-08-07
  scan (which reviewed the newest arXiv API records, updated through
  2026-08-06 17:59 UTC) through the current scan on 2026-08-10. The arXiv
  `/list/{cat}/new` listing pages for `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, and
  `cs.LG` expose the newest announcement batch ("Friday, 7 August 2026" per the
  listing header): 725 unique records in total.
- Cross-check: 716 of those 725 records appear in no prior `README.md`,
  `docs/landscape.md`, or `sources/` entry and were treated as new candidates.
  A title-and-abstract relevance pass produced 22 candidates for manual review.
- Verification used arXiv metadata and full-text pages plus official project
  pages and GitHub repositories. Existing `README.md`, `docs/landscape.md`, and
  `sources/` entries were checked for duplicates before selection.
- The arXiv API metadata for this batch timestamps records on 2026-08-05 and
  2026-08-06; no records with submitted or updated dates after 2026-08-06
  17:59 UTC were present in the API snapshot, so the listing batch is the
  newest announcement data available.

## Included

### EvoHarness-RL: Learning Self-Evolving Runtime Harness for Long-Horizon LLM Agents

- Paper: https://arxiv.org/abs/2608.05446
- Submitted: 2026-08-05; accepted to LLA@COLM 2026.
- Classification: General Harness Methodology.
- Why included: treats the harness itself as learned policy rather than
  hand-written prompts and conventions. Belief, Progress, and Experience (BPE)
  become policy-facing harness state; supervised fine-tuning teaches the action
  space and cost-aware GRPO learns when to read, update, and consolidate state.
  The reported dynamics (harness annealing, harness evolution) directly extend
  the Self-Harness / Harness-R1 / HarnessOpt-Bench thread.
- Evidence boundary: instantiated on ALFWorld with Qwen3-8B, reporting 96.9%
  success. These are digital-agent results, not robot experiments. No official
  code link was located as of 2026-08-10.

### EnvACE: Internalizing Environment Dynamics via World Rehearsal for Agentic RL

- Paper: https://arxiv.org/abs/2608.06197
- Code: https://github.com/Within-yao/EnvACE
- Submitted: 2026-08-06.
- Classification: General Harness Methodology.
- Why included: removes external environment interaction during agent training
  by having the policy alternate between acting and rehearsing the environment
  response, producing an internalized agent world model with private rehearsal
  at test time. This is a training-time harness/world-model technique with a
  released implementation (agent system plus simulator core).
- Evidence boundary: reported gains are on digital tool-use benchmarks
  (BFCL-v4, tau^2-Bench, VitaBench, FinMCP-Bench) rather than robots; robotics
  transfer remains unvalidated.

### In-Context VLA: Endowing Vision-Language-Action Models with Language via In-Context Post-Training and Agentic Tool Use

- Paper: https://arxiv.org/abs/2608.05738
- Submitted: 2026-08-06.
- Classification: Agentic Robot / VLA Harness.
- Why included: argues empirically and analytically that free-form textual
  chain-of-thought degrades low-level VLA control (ungrounded reasoning,
  latency, conflicting reasoning-versus-action objectives) and instead endows a
  VLA with language competence through in-context post-training on structured
  perceptual evidence plus an agentic tool-use interface (open-vocabulary
  detectors, monocular depth, VLM queries). This directly informs the
  reasoning/control boundary a robot harness must enforce.
- Evidence boundary: author-reported SOTA performance and efficiency versus
  CoT-based methods under matched configurations on RoboCasa-GR1, SimplerEnv,
  LIBERO, and eight real-robot manipulation tasks. No official code link was
  located as of 2026-08-10.

### XEWorld: Can Action-Conditioned World Models Generalize to Unseen Robot Embodiments?

- Paper: https://arxiv.org/abs/2608.05799
- Submitted: 2026-08-06.
- Classification: Robot Foundation/World Model; Evaluation.
- Why included: a controlled cross-embodiment testbed that isolates embodiments
  by evaluating held-out robots in physically identical scenes. The finding that
  current world models act as 2D visual pattern matchers (generalization tracks
  visual rather than kinematic similarity, zero-shot transfer requires
  pixel-space actions and spatial-temporal alignment, and few-shot adaptation
  causes catastrophic forgetting) is an evaluation contribution, not just a new
  model.
- Evidence boundary: the negative findings are author-reported diagnostics over
  existing world models; no official code or benchmark release was linked as of
  2026-08-10.

### DynamicWAM: Dual-Path Motion Conditioning for World-Action Models in Dynamic Manipulation

- Paper: https://arxiv.org/abs/2608.00793
- Project: https://dynamicwam.github.io/
- Code: https://github.com/Autumn1337/DynamicWAM
- Submitted: 2026-08-01 (revised 2026-08-06).
- Classification: Robot Foundation/World Model.
- Why included: a compact WAM for dynamic manipulation with dual-path motion
  conditioning (history-flow optical-flow frames through a frozen video VAE
  plus kinematic descriptors) fused through joint world-action attention, with
  a distilled backbone and real-time chunking for asynchronous execution. It
  directly addresses the motion-awareness and serving gaps documented for WAMs.
- Evidence boundary: the authors report a 38.2% success rate on DOMINO and a
  46.7% average success rate across 12 real-world tasks (22.9 points over the
  strongest baseline); results are author-reported. Official code and project
  page are public and were verified on 2026-08-10.

### Hijacking Robots with a Piece of Paper: Physical Prompt Injection in VLM-Controlled Robots

- Paper: https://arxiv.org/abs/2608.05715
- Submitted: 2026-08-06.
- Classification: Evaluation/Safety.
- Why included: the first systematic taxonomy (indirect signage, task
  redefinition, authority impersonation, conflict injection) and benchmark for
  physical prompt injection against VLM-controlled manipulation, with a
  20-prompt test set across three scene layouts and three command formulations.
  It makes perception-pipeline integrity an explicit harness safety boundary.
- Evidence boundary: across 5,670 trials on GPT-4o, Gemini 2.5 Flash, and
  Qwen3-VL-32B, attacks succeeded at 27.0%, 29.4%, and 5.0%. Simple mitigations
  (prompt-based defense, two-stage verification, text masking) were reported to
  reduce risk substantially but may impair tasks that require reading in-scene
  labels. No official code link was located as of 2026-08-10.

### Failing Gracefully: Mitigating Impact of Inevitable Robot Failures

- Paper: https://arxiv.org/abs/2608.05313
- Submitted: 2026-08-05; accepted to ICRA 2026.
- Classification: Evaluation/Safety.
- Why included: shifts failure analysis from probability alone to the impact of
  failures, scoring both the likelihood of impactful interactions between a
  failing robot and surrounding entities and the severity of outcomes, then
  feeding that into planning. FailBench adds a MuJoCo-based simulation
  framework spanning sensing and actuator failure modes.
- Evidence boundary: the safety formulation and FailBench are author-reported
  and simulation-based; no official code link was located as of 2026-08-10.

## Reviewed but not promoted

- **Adaptive-WAM** (`2608.06008`) is a quality-guided early-exit planner for
  autonomous driving rather than robot manipulation; it was not added because
  the list is robot-centric and no code was linked.
- **Tactile-WAM** (`2606.26663`) adds tactile asymmetric attention to WAMs but
  is an RSS workshop submission with no released artifacts located.
- **Enfold** (`2607.26657`) folds world-model computation into a current-only
  predictive representation; a code repository exists, but the contribution is
  representation-focused rather than a harness or evaluation interface.
- **VLAff** (`2608.05215`) is an affordance VLA with a large claimed
  EgoAffordance dataset (204K episodes), but its GitHub repository is only the
  project page; no implementation or dataset release was verified as of
  2026-08-10.
- **JoyAI-RA 0.5** (`2608.05674`) is a generalist VLWA scaling report with a
  project page but no code repository located.
- **World-to-Wrist / W2-VLA** (`2608.05369`) models future wrist latents for
  fine-grained manipulation, but no official artifacts were linked.
- **MMaDA-VLA** (`2603.25406`) is a discrete diffusion VLA accepted at ACM MM
  2026; it overlaps the existing diffusion-VLA entries and exposes no verified
  code.
- **Visual Grounding in Zero-Shot Vision-Language Control** (`2608.06154`)
  reports a valuable negative result (many zero-shot VL controllers are
  image-invariant under input ablation) but ships no benchmark artifacts.
- **NavTrust** (`2603.19229`) is a trustworthiness benchmark for embodied
  navigation; the navigation domain is outside this list's manipulation focus
  and the linked GitHub repository was not reachable (HTTP 404).
- **MERIT** (`2608.05906`) applies causal episodic memory to Text-to-SQL agent
  repair; the digital, single-domain evaluation was judged too narrow.
- **SkillCorpus** (`2607.15557`) and **Comparative Agent Retrieval**
  (`2608.06196`) consolidate and retrieve digital-agent skills; they are
  relevant to skill registries but contain no robot evaluation.
- **CS-JEPA / One Future, Every Robot** (`2607.28443`) is a decentralized
  collective-state prediction method for swarms; it is an ICRA 2027 submission
  with no released artifacts.
- **TRACT** (`2607.29285`) is phase-structured action chunking for
  contact-rich control; it is a narrow policy contribution without harness or
  evaluation scope.
- **Observation-Grounded Self-Predictive RL** (`2608.05989`) improves visual-RL
  representation learning but is a method contribution, not a systems or
  evaluation interface.
- Records previously reviewed on 2026-08-07 — SkillMemo (`2608.05970`), HiRoC
  (`2608.05999`), Robust-WAM (`2608.05903`), MobileWAM (`2608.04657v2`),
  DyPES-VLA (`2608.06374`), Reinforcing Action Policies by Prophesying
  (`2511.20633v2`), and IcFuzz (`2608.06088`) — showed no artifact-status or
  version changes in this batch.
- Narrow teleoperation, conventional control, perception, locomotion,
  navigation, and hardware records were excluded unless they introduced a
  reusable agent, serving, recovery, safety, or evaluation interface.

## Artifact-status checks

- DynamicWAM: official implementation repository verified
  (github.com/Autumn1337/DynamicWAM, last pushed 2026-08-04).
- EnvACE: official code repository verified
  (github.com/Within-yao/EnvACE, last pushed 2026-08-08).
- VLAff: GitHub repository is a static project page; no implementation,
  dataset, or weights verified as of 2026-08-10.
- EvoHarness-RL, In-Context VLA, XEWorld, Adaptive-WAM, Tactile-WAM, Enfold,
  Hijacking Robots with a Piece of Paper, Failing Gracefully, JoyAI-RA,
  World-to-Wrist, and MMaDA-VLA: no official code, dataset, or weights links
  were located on arXiv pages or through repository search as of 2026-08-10.
