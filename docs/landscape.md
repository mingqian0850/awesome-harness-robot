# Landscape: Agent Harnesses, VLAs, and Robot Foundation Models

Last verified: **2026-08-10**.

This note explains the development arc behind the links in the main list. Dates refer to public releases or papers, not necessarily the start of internal development.

## Short Timeline

| Period | Milestone | Why it mattered |
| --- | --- | --- |
| 2022 | SayCan, RT-1, Code as Policies | Language became a practical interface for skill selection, code generation, and large-scale robot policy learning. |
| 2023 | PaLM-E, RT-2, RT-X/Open X-Embodiment | Web-scale multimodal knowledge and cross-robot data were connected to control. |
| 2024 | Octo, OpenVLA, π0, RDT-1B, CogACT | Open generalist policies diversified into token, diffusion, and flow-based action heads. |
| 2025 | OpenVLA-OFT, SmolVLA, π0.5, GR00T N1/N1.5, Gemini Robotics, V-JEPA 2 | Efficient action chunks, open-world adaptation, humanoid policies, on-device deployment, and world-model planning moved to the foreground. |
| 2026 | Self-Harness, Harness-R1, HarnessOpt-Bench, EvoHarness-RL, EnvACE, OpenForgeRL, A²E, ENPIRE, OpenETA, Guava, ASPIRE, GaP, Harness VLA, In-Context VLA, AtlasVLA, Physical Agency, RoboBRIDGE, Embodied Agents Take Control, HERO, CheckVLA, CoWAM, SAFECAST, InternVLA-A1, LingBot-VLA 2.0, Qwen robotics models, TurboVLA, Robot-Factored World Models, World Action Models in Real Time, ω-0, GeniWorld, XEWorld, DynamicWAM, PSG-JEPA, Qwen-RobotWorld, ViTacWorld, World Action Planner, VisualPatchWorld, GAUGE, VLA-Arena, vla-evaluation-harness | The ecosystem began treating orchestration, memory, recovery, skill discovery, physical autoresearch, cross-embodiment schemas, asynchronous serving, runtime verification, planner-facing world models, physical-fidelity diagnosis, evaluation, and the harness itself as optimization targets. |

## Architecture Trends

### 1. From tokenized actions to specialized action experts

OpenVLA and RT-style systems demonstrated that robot actions can be represented as language-like tokens. Newer systems frequently preserve a pretrained VLM for semantics while attaching a dedicated action module:

- diffusion Transformer heads in CogACT and GR00T;
- flow-matching action experts in π0, SmolVLA, MolmoAct 2, LingBot-VLA 2.0, and the reported Qwen robot models;
- continuous parallel regression in OpenVLA-OFT;
- autoregressive learned action tokenization in FAST-style models.

The benefit is a cleaner split between semantic representation and high-dimensional continuous control. The cost is a more complex serving contract: the harness must carry action horizons, normalization, embodiment schemas, control rates, and chunk-fusion policy.

### 2. Action chunks replace single-step blocking inference

Generating one action at a time makes a slow model part of the control deadline. Action chunks amortize inference, while asynchronous or real-time chunking lets execution overlap with the next prediction. This introduces new harness responsibilities:

- reject chunks computed from stale observations;
- blend, truncate, or replace overlapping chunks;
- monitor inference and network deadlines;
- fall back to hold/stop/recovery behavior;
- record the exact executed action, not only the model proposal.

World Action Models in Real Time tests six concrete deployment strategies on a 10 Hz bimanual platform and shows why blending alone cannot repair a misaligned observation–prediction–execution timeline. Its author-reported results favor prefix-conditioned generation as the best overall speed, smoothness, and task-performance trade-off, but the broader systems lesson is independent of the winning model: the harness must timestamp committed actions, align the next conditioning prefix with what actually executed, and define a safe chunk-switch boundary.

### 3. Cross-embodiment learning becomes a schema problem

A generalist checkpoint does not eliminate robot-specific configuration. Current open stacks make the following explicit:

- camera names, order, intrinsics, extrinsics, and image transforms;
- joint versus end-effector state;
- absolute versus delta actions;
- coordinate frames and rotation representation;
- action/state dimensions, padding, masks, and normalization statistics;
- control frequency, horizon, gripper convention, and embodiment tag.

The most reusable artifact is often not the neural network—it is the dataset and policy interface that makes these differences visible.

LingBot-VLA 2.0 and Qwen-RobotManip make this schema layer unusually concrete. LingBot-VLA 2.0 maps arms, end effectors, grippers, hands, waist, head, and base signals into a 55-dimensional canonical vector and releases code, pretraining weights, and a RoboTwin post-trained checkpoint. Qwen-RobotManip reports an 80-dimensional masked vector, camera-frame end-effector deltas, camera geometry, and structured embodiment prompts, but its official repository is informational and explicitly says weights are not planned for release. These are author-reported designs and results, not evidence that either schema transfers without embodiment-specific calibration.

### 4. Web video and human video supplement expensive robot data

Robot demonstrations are scarce and fragmented. Current approaches combine:

- web image/text data for semantics;
- egocentric human video for manipulation priors;
- simulation and world-model-generated trajectories;
- multi-institution robot datasets;
- small embodiment-specific post-training sets.

Human video does not contain robot actions, so latent-action learning, retargeting, annotation, or staged pretraining is required.

Robot-Factored World Models makes this translation boundary explicit. Instead of asking a video model to infer how a raw command becomes robot motion—or conditioning on future logged states that leak the interaction outcome—it rolls the command through the known controller and kinematics, renders the nominal trajectory through the URDF, and presents that geometry to the world model. ViTacWorld takes a complementary sensory route by predicting synchronized vision and touch, then using imagined rollouts for contact-policy augmentation and pre-deployment evaluation. In both cases, the harness owns the action interface, geometry, calibration, and rollout protocol around the learned predictor.

Qwen-RobotWorld explores a broader but less reproducible interface: natural language represents actions across manipulation, navigation, driving, and human activity, and a video generator supplies imagined trajectories for data augmentation, evaluation, or planning. This removes platform-specific numeric actions from the world-model input, but shifts precision and executability back to the downstream harness. No official code or weights were verified as of 2026-08-03.

GeniWorld and ω-0 extend this boundary in different directions. GeniWorld converts numeric manipulation actions into URDF-rendered visual actions and couples autoregressive video prediction to high-frequency kinematic control, making the learned environment interactive for policy evaluation and synthetic rollouts. ω-0 avoids future-video reconstruction and instead couples lightweight future-observation embeddings to controller-compatible whole-body action latents for concurrent humanoid locomotion and manipulation. Both results and real-robot claims are author-reported, and neither arXiv/project page exposed a runnable implementation, public dataset, or weights as of 2026-08-07.

### 5. Hierarchies return

End-to-end pixels-to-actions remains attractive, but deployed systems increasingly use multiple rates and levels:

- a slow reasoning/planning model;
- a mid-rate VLA or learned skill policy;
- a high-rate whole-body or servo controller;
- a deterministic safety supervisor outside all learned components.

This is not a retreat from learning. It is a recognition that language reasoning, visuomotor prediction, contact stabilization, and safety have different data, latency, and verification requirements.

Harness VLA makes this boundary explicit at the manipulation-policy level. Its agent keeps semantic re-grounding, non-contact movement, re-staging, retries, and memory outside a frozen VLA; the VLA is invoked as a local contact-rich primitive. The contribution is therefore not another base policy but a method for extending a policy's operating range through orchestration and execution feedback.

OpenETA makes the hierarchy executable as an open runtime. Its planner is permitted one typed, world-changing tool call per turn; a host-owned interface validates and executes that call; and the next decision is conditioned on a fresh world observation. This is a useful physical-agent contract because permissions, timing, verification, logging, and real-versus-sim adapters remain outside the planner. The official repository includes code, tests, replayable logging, skills, memory, simulation adapters, and a UR5e deployment path; its benchmark and hardware results remain author-reported.

Physical Agency names and measures the orchestration gap between frozen motor skills used alone and the same skills inside a closed-loop planner. Its Pigey orchestrator decomposes goals, invokes either VLA policies or parameterized skills, verifies low-level observations, and recovers without policy post-training. FORGE-plus demonstrates a narrower safety-oriented hierarchy: a frozen text LLM may choose a force budget and recovery maneuver, but the deterministic controller owns enforcement and recovery cannot raise the ceiling.

RoboBRIDGE makes the deployment boundary more explicit through five replaceable modules: Monitor, Perceptor, Planner, Controller, and Robot Interface. Unlike wrappers centered on one recovery technique, it treats asynchronous perception, hierarchical recovery, embodiment adaptation, and policy selection as coordinated runtime services. Embodied Agents Take Control probes an even thinner interface: general software-agent harnesses receive a camera and discrete actions, revealing that model capability, optional waypoint tools, context growth, and latency can matter as much as bespoke embodied workflow code.

Qwen-RobotNav presents a complementary model–harness contract for navigation. An upper-level agent selects a task mode and controls visual token budget, temporal decay, camera weights, and frame sampling, while the base model emits waypoint chunks. This makes observation scheduling a typed inference-time interface rather than a fixed preprocessing choice; the current evidence and deployment demonstrations are author-reported, and the official repository states that weights are not planned for release.

The surrounding 2026 systems explore different harness boundaries. Guava isolates three reusable ingredients—iterative perception–reasoning–action, semantic action abstractions, and multimodal observations—and tests whether that interface transfers across reasoning-model scales. ASPIRE grows a code-as-policy skill library from validated repairs. GaP represents policies as directed graphs assembled by multiple coding agents, then searches graph structures and parameters in generated simulation. InternVLA-A1 takes the complementary model-centric route: it internalizes scene understanding, visual foresight, and flow-matching action generation in one Mixture-of-Transformers rather than placing those capabilities in an external agent harness. In-Context VLA sharpens the reasoning/control boundary at the model level: its authors report that free-form textual chain-of-thought degrades low-level control and that a VLA should consume grounded language (structured perceptual evidence plus agentic tool queries) rather than generate ungrounded narrative; its simulation and real-robot results are author-reported and no code was linked as of 2026-08-10.

### 6. Memory moves from prompt history to execution knowledge

Harness VLA separates two forms of operational memory:

- task-specific successful command traces used as few-shot examples for a target task;
- global success rules and failure models reused across tasks.

This is a useful harness pattern because it stores executable experience and failure boundaries rather than relying only on an ever-growing conversation transcript. The paper is a July 2026 preprint; its reported results should be treated under the authors' protocols, and no public code repository was linked as of 2026-07-25.

ASPIRE and GaP broaden this idea from runtime memory to system improvement. ASPIRE admits validated repairs into a transferable skill library; GaP uses simulated rehearsals to refine a persistent graph policy before real execution. Guava demonstrates a lighter-weight alternative in which recovery emerges from an iterative observation/reasoning/tool loop. These mechanisms should not be treated as interchangeable: trace retrieval, reusable skill accumulation, graph search, and in-context replanning have different compute costs and different failure modes.

HERO adds a consolidation path: heuristic reasoning and exemplar reuse bootstrap behavior from autonomous interaction, while recurring experience is converted into faster closed-loop visuomotor policies. This couples data collection, capability scheduling, and policy learning inside one improvement loop, so its safety case depends on which environments may be explored, how exemplars are admitted, and how consolidated policies are regression-tested.

AtlasVLA pushes the same memory idea into the policy's own state. Instead of retrieving past context, it maintains a 4D persistent world-state memory that lifts transient 2D observations into a voxel-hashed spatial state, plus an ego-working memory that tracks task progress, and conditions a diffusion transformer on the joint world-ego state. This targets the perception-forgetting and task-progress-forgetting failures that arise when a reactive VLA sees only a wrist camera. Its results are author-reported and no code was linked as of 2026-08-10.

### 7. Harness optimization becomes regression-gated

Self-Harness provides a general coding-agent example of optimizing the non-parametric system around a fixed model. It clusters verifier-grounded failure traces, asks the same target model for bounded and auditable harness edits, and promotes a candidate only if it improves one evaluation split without degrading the other. The accepted changes can affect instructions, tools, memory, runtime controls, verification, skills, or subagent structure.

For robotics, this pattern belongs in an offline improvement plane rather than the live safety-critical control path. Candidate changes should run through simulation, recorded-trajectory replay, shadow evaluation, independent approval, versioned deployment, and rollback. Safety limits, permission boundaries, emergency stops, and verified low-level controllers must remain outside the editable surface.

The evidence is preliminary: Self-Harness evaluates terminal agents, not robots, and uses its so-called held-out split when deciding which candidate edits to promote. Those traces are hidden from the proposer, but repeated score-based selection can still adapt to the split, so a separate untouched final test set remains necessary.

ENPIRE moves this improvement loop onto physical hardware. Its Environment module exposes bounded reset, safety, observation, and automated verification; Policy Improvement edits heuristics, behavior-cloning or RL infrastructure; Rollout preserves auditable trials; and Evolution lets multiple coding agents compare branches and reuse successful recipes across a robot fleet. This is closer to an automated robotics laboratory than runtime task planning. The distinction in reported metrics matters: the project page's 99% pass@8 permits up to eight failure-conditioned retries per subtask, so single-attempt reliability, intervention rate, robot utilization, and token cost remain separate quantities.

Harness-R1 takes a different route from Self-Harness: it post-trains a separate 9B harness engineer with supervised cold start and online reinforcement learning, using fresh reruns of a frozen target agent to reward executable lifecycle-hook patches. The official repository and model weights are released, but the reported WebShop, ALFWorld, and DBBench results contain no robot experiments. In robotics, same-batch repair rewards would need stronger temporal separation, untouched evaluation sets, simulation and replay gates, and immutable host safety boundaries before any candidate patch reached hardware.

HarnessOpt-Bench turns this improvement loop into a measured capability. An optimizer receives a seed harness, graded feedback, and a fixed target-evaluation budget; a trusted execution environment hides the held-out test partition, meters resource use, and preserves every candidate version. This is a stronger search/evaluation separation than repeatedly selecting on a visible regression set, but the current four downstream tasks are digital-agent tasks rather than robotics. A robot adaptation would additionally need simulator and replay budgets, hardware-use accounting, immutable safety tests, and a final multi-site physical evaluation.

EvoHarness-RL moves from proposing harness edits to learning the harness policy itself. It exposes Belief, Progress, and Experience as policy-facing harness state, teaches the base agent the harness action space by supervised fine-tuning, and uses cost-aware GRPO to learn when to read, update, and consolidate external state during long-horizon interaction. Its reported 96.9% ALFWorld success with Qwen3-8B is a digital-agent result, but the reported dynamics—harness annealing, where the model internalizes recurring harness use and calls the workspace less often, and harness evolution, where progress and experience consolidate into a compact task-adaptive substrate—are directly relevant to how a physical-agent harness should decide which state lives in the model versus in the external workspace. No code was linked as of 2026-08-10.

EnvACE offers a complementary training-time route: the agent alternates between acting and rehearsing the environment response, jointly optimizing both roles with task-success rewards so the environment dynamics are internalized as an agent world model. This replaces external environment interaction during training and enables private rehearsal before committed execution at test time. The released repository is a genuine implementation, but the reported gains are on digital tool-use benchmarks rather than robots, so physical-agent transfer remains unvalidated.

Two infrastructure pieces complete the loop. OpenForgeRL makes harness-native agents trainable: a proxy records the harness's model calls as training data for a standard RL codebase while a Kubernetes orchestrator runs each rollout in an isolated container, so stateful multi-process harnesses can be fine-tuned end-to-end in arbitrary environments. A²E makes harness evaluation systematic in the other direction: an Agent Task Protocol integrates evaluation tasks across harnesses, an instrumented Monitor captures standardized execution traces, and multidimensional metrics replace correctness-only scoring. Both are described or released as open source, but neither evaluates robots.

### 8. Evaluation becomes a harness, not a notebook

VLA results were historically difficult to compare because each model carried its own environment fork, dependency set, observation adapter, action scaling, and rollout script. The emerging pattern is:

- benchmark environments in pinned containers;
- models behind versioned servers;
- declarative observation/action adapters;
- batch rollout scheduling;
- full trajectory recording;
- schema-validated metrics and reproducible configs.

The remaining gap is standardized, multi-site real-robot evaluation. Simulation success is useful for regression testing but is not a substitute for physical robustness.

CheckVLA illustrates how evaluation logic can also become an online runtime service. It compares observed evolution with an action-conditioned world-model reference, calibrates when to intervene, and rewrites only the suffix that remains executable after inference latency. Its current RoboCasa365 evidence is simulation-only, but the separation between action proposal, execution evidence, verifier, and repair is a reusable harness pattern.

CoWAM and SAFECAST sharpen two adjacent contracts. CoWAM allows a bimanual policy intervention only when typed coordination obligations and calibrated gates admit it, otherwise retaining the nominal action or abstaining; its current evidence is eight simulated tasks. SAFECAST augments hidden-state failure-probe training and calibration with visual and language contrast sets, with author-reported results on real-world DROID rollout data and LIBERO simulation. Neither arXiv page linked official code as of 2026-08-06, and neither result is an independent safety certification.

GAUGE adds a complementary environment-level diagnosis. Rather than judging a simulator or video world model only by pixels or human preference, it pairs real trajectories and calibrated metadata with observables for collision, friction, momentum transfer, oscillation, self-contact, and deformation. The authors report that no tested physics engine is uniformly faithful and that video models may reproduce the expected equation form while inferring incorrect physical parameters. Its 22-task design is valuable for harness regression, but no official code or dataset release was linked as of 2026-08-07.

XEWorld extends the same evaluative stance to embodiment generalization. By holding out entire robots inside physically identical scenes, it tests whether an action-conditioned world model renders embodiments it has never seen. The authors report that current models behave as 2D visual pattern matchers—generalization follows visual similarity rather than kinematic similarity, zero-shot rendering of unseen embodiments requires heavily grounded pixel-space actions and explicit spatial-temporal alignment, and few-shot adaptation triggers catastrophic forgetting of seen embodiments. These are author-reported diagnostics over existing models, not an independent certification, but they define the right question for a world-model harness: does the model know physics, or only pixels? No code or benchmark release was linked as of 2026-08-10.

On the deployment side, DynamicWAM shows one concrete serving answer for dynamic manipulation: a distilled world-action model conditioned on history-flow optical flow and kinematic descriptors, executed through real-time chunking so motion awareness and asynchronous execution are coupled rather than traded off. Its DOMINO and real-robot results are author-reported, and the official implementation repository was verified as public as of 2026-08-10.

PSG-JEPA questions the JEPA assumption itself: is forward prediction in latent space enough for control-relevant representations? It adds grounding objectives—latents matched to robot proprioceptive state and latent pairs matched to multi-horizon joint-angle changes—applied only during training, and evaluates identifiability, planning, and policy performance separately. This is a useful reminder that a world model's latent space must be tied to quantities the controller can use, not only to future observations. An official repository exists but marks code as coming soon as of 2026-08-10.

On the benchmark side, VLA-Arena standardizes how capability boundaries are measured: 11 task suites across Safety, Distractor, Extrapolation, and Long Horizon (170 tasks), with difficulty varied along task structure, language command, and visual observation axes and fine-tuning restricted to the easiest level. The official repository is public, and model results are author-reported.

## Implementation Families

| Family | Examples | Strengths | Common harness concern |
| --- | --- | --- | --- |
| Autoregressive action tokens | OpenVLA, RT-1/2, π0-FAST | Reuses language-model training and decoding machinery. | Decode latency, token/action calibration, compounding errors. |
| Continuous regression | OpenVLA-OFT | Simple, fast, deterministic action chunks. | Median-mode behavior and limited multimodality. |
| Diffusion / flow matching | π0/π0.5, CogACT, GR00T, SmolVLA, MolmoAct 2, LingBot-VLA 2.0, Qwen-VLA/RobotManip | Expressive continuous action distributions. | Denoising latency, stochasticity, chunk consistency, and artifact availability. |
| World-model planning and interaction | V-JEPA 2-AC, DreamerV3, Robot-Factored World Models, Qwen-RobotWorld, ViTacWorld, GeniWorld, World Action Planner, VisualPatchWorld, DynamicWAM | Predicts consequences and supports explicit planning, interactive rollouts, augmentation, policy evaluation, or inspectable executable dynamics. | Model bias, conditioning leakage, geometry/calibration drift, code validity, and planning budget. |
| Whole-body world-action models | ω-0 | Couples predictive latent objectives to controller-compatible locomotion and manipulation actions. | Dataset access, whole-body safety, controller dependence, artifact availability, and independent real-robot replication. |
| Planner plus skills | OpenETA, RoboBRIDGE, Physical Agency, Harness VLA, Guava, ASPIRE, HERO, SayCan, ROSA, Code as Policies | Interpretable task decomposition, typed tool calls, host-owned execution, retry, memory, capability consolidation, and reuse of verified or learned primitives. | Tool permissions, grounding, recovery, context drift, and unsafe experience admission. |
| Asynchronous action-chunk serving | World Action Models in Real Time | Overlaps inference with execution and makes observation–prediction–command alignment an explicit runtime contract. | Stale observations, chunk-boundary discontinuities, unsafe switching, and hardware-specific timing. |
| Runtime execution verification | CheckVLA, CoWAM, SAFECAST | Uses predicted futures, coordination contracts, calibrated thresholds, or contrast-set probes to detect and constrain deployment-time failures. | Model misspecification, false interventions, repair latency, calibration shift, and simulation-to-real transfer. |
| Graph-as-policy search | GaP | Interpretable perception/planning/control graphs refined through simulated rehearsal. | Simulator fidelity, search cost, graph validation, transfer to hardware. |
| Safety-bounded supervisor | FORGE-plus | Keeps semantic force-budget and recovery selection above immutable low-level enforcement. | Simulation-only evidence, force-model mismatch, and unsafe editable limits. |
| Harness optimization | Self-Harness, Harness-R1, HarnessOpt-Bench, EvoHarness-RL, EnvACE, OpenForgeRL, A²E | Trace-grounded, model-specific changes proposed or learned around a fixed target, with budgeted evaluation and auditable candidate selection; training-time world rehearsal; or training and auditing infrastructure for harness-native agents. | Evaluation leakage, same-batch reward overfitting, search cost, unsafe editable surfaces, and rollback. |
| Perception-threat audit | Hijacking Robots with a Piece of Paper | Evaluates physical prompt injection against VLM-controlled robots and tests simple defenses. | Attack-transfer assumptions, defense trade-offs (for example masking in-scene labels), and no independent benchmark replication yet. |
| Robot-team communication security | When Coordination Becomes a Threat | Studies communication attacks in LLM-planned multi-robot systems across DMAS and HMAS architectures. | Simulation-only evidence, architecture coverage, and no released benchmark artifacts. |
| Calibrated operator handoff | AutoIntervene | Scores action chunks against a visual-action support memory and transfers control to an operator with calibrated thresholds. | Project page only; no verified implementation or real-robot replication yet. |
| VLA benchmarking | VLA-Arena | Structured difficulty axes and 170 tasks to measure VLA capability frontiers with restricted fine-tuning. | Model results are author-reported; leaderboard coverage and hardware portability are open. |
| Physical autoresearch harness | ENPIRE | Automates reset, verification, policy experiments, rollout auditing, and multi-agent evolution on real robot fleets. | Hardware wear, unsafe experiment generation, verifier gaming, retry-sensitive metrics, robot utilization, and token cost. |
| Unified foresight VLA | InternVLA-A1 | Joint scene understanding, future visual prediction, and continuous action generation. | Prediction error propagation, compute cost, and separating model versus harness failures. |
| Hierarchical whole-body | Helix 02 and humanoid stacks | Matches semantic, visuomotor, and stabilization time scales. | Cross-layer contracts, failure propagation, end-to-end tracing. |

## What Is Mature Enough to Use?

### Reasonably mature

- ROS 2, ros2_control, MoveIt 2, Nav2, BehaviorTree.CPP;
- MuJoCo, Isaac Lab, robosuite, ManiSkill, Habitat;
- robomimic and standard imitation-learning baselines;
- LeRobot data capture and supported hardware paths;
- LIBERO/CALVIN-style simulation regression testing.

### Fast-moving but usable

- OpenVLA-OFT, openpi, SmolVLA, GR00T, MolmoAct 2;
- StarVLA and vla-evaluation-harness;
- LeRobot integrations for large VLAs and world models;
- world-model-based robot planning.

Pin versions and reproduce one upstream result before changing the model or embodiment.

### Research frontier

- reliable zero-shot transfer to unseen real robots;
- long-horizon autonomous recovery;
- unified navigation, manipulation, and whole-body control;
- calibrated uncertainty and runtime failure prediction;
- learning safely from online experience;
- independent, cross-lab real-robot leaderboards.

## Practical Takeaway

Model choice matters, but the harness determines whether the model can be compared, adapted, stopped, debugged, and trusted. Build the observation/action contract, recorder, replay path, safety supervisor, and deterministic baseline before optimizing the foundation model.
