# Landscape: Agent Harnesses, VLAs, and Robot Foundation Models

Last verified: **2026-08-03**.

This note explains the development arc behind the links in the main list. Dates refer to public releases or papers, not necessarily the start of internal development.

## Short Timeline

| Period | Milestone | Why it mattered |
| --- | --- | --- |
| 2022 | SayCan, RT-1, Code as Policies | Language became a practical interface for skill selection, code generation, and large-scale robot policy learning. |
| 2023 | PaLM-E, RT-2, RT-X/Open X-Embodiment | Web-scale multimodal knowledge and cross-robot data were connected to control. |
| 2024 | Octo, OpenVLA, π0, RDT-1B, CogACT | Open generalist policies diversified into token, diffusion, and flow-based action heads. |
| 2025 | OpenVLA-OFT, SmolVLA, π0.5, GR00T N1/N1.5, Gemini Robotics, V-JEPA 2 | Efficient action chunks, open-world adaptation, humanoid policies, on-device deployment, and world-model planning moved to the foreground. |
| 2026 | Self-Harness, ENPIRE, Guava, ASPIRE, GaP, Harness VLA, Physical Agency, RoboBRIDGE, Embodied Agents Take Control, HERO, CheckVLA, InternVLA-A1, LingBot-VLA 2.0, Qwen robotics models, TurboVLA, Robot-Factored World Models, Qwen-RobotWorld, ViTacWorld, World Action Planner, VisualPatchWorld, vla-evaluation-harness | The ecosystem began treating orchestration, memory, recovery, skill discovery, physical autoresearch, cross-embodiment schemas, runtime verification, planner-facing world models, efficient deployment, evaluation, and the harness itself as optimization targets. |

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

### 5. Hierarchies return

End-to-end pixels-to-actions remains attractive, but deployed systems increasingly use multiple rates and levels:

- a slow reasoning/planning model;
- a mid-rate VLA or learned skill policy;
- a high-rate whole-body or servo controller;
- a deterministic safety supervisor outside all learned components.

This is not a retreat from learning. It is a recognition that language reasoning, visuomotor prediction, contact stabilization, and safety have different data, latency, and verification requirements.

Harness VLA makes this boundary explicit at the manipulation-policy level. Its agent keeps semantic re-grounding, non-contact movement, re-staging, retries, and memory outside a frozen VLA; the VLA is invoked as a local contact-rich primitive. The contribution is therefore not another base policy but a method for extending a policy's operating range through orchestration and execution feedback.

Physical Agency names and measures the orchestration gap between frozen motor skills used alone and the same skills inside a closed-loop planner. Its Pigey orchestrator decomposes goals, invokes either VLA policies or parameterized skills, verifies low-level observations, and recovers without policy post-training. FORGE-plus demonstrates a narrower safety-oriented hierarchy: a frozen text LLM may choose a force budget and recovery maneuver, but the deterministic controller owns enforcement and recovery cannot raise the ceiling.

RoboBRIDGE makes the deployment boundary more explicit through five replaceable modules: Monitor, Perceptor, Planner, Controller, and Robot Interface. Unlike wrappers centered on one recovery technique, it treats asynchronous perception, hierarchical recovery, embodiment adaptation, and policy selection as coordinated runtime services. Embodied Agents Take Control probes an even thinner interface: general software-agent harnesses receive a camera and discrete actions, revealing that model capability, optional waypoint tools, context growth, and latency can matter as much as bespoke embodied workflow code.

Qwen-RobotNav presents a complementary model–harness contract for navigation. An upper-level agent selects a task mode and controls visual token budget, temporal decay, camera weights, and frame sampling, while the base model emits waypoint chunks. This makes observation scheduling a typed inference-time interface rather than a fixed preprocessing choice; the current evidence and deployment demonstrations are author-reported, and the official repository states that weights are not planned for release.

The surrounding 2026 systems explore different harness boundaries. Guava isolates three reusable ingredients—iterative perception–reasoning–action, semantic action abstractions, and multimodal observations—and tests whether that interface transfers across reasoning-model scales. ASPIRE grows a code-as-policy skill library from validated repairs. GaP represents policies as directed graphs assembled by multiple coding agents, then searches graph structures and parameters in generated simulation. InternVLA-A1 takes the complementary model-centric route: it internalizes scene understanding, visual foresight, and flow-matching action generation in one Mixture-of-Transformers rather than placing those capabilities in an external agent harness.

### 6. Memory moves from prompt history to execution knowledge

Harness VLA separates two forms of operational memory:

- task-specific successful command traces used as few-shot examples for a target task;
- global success rules and failure models reused across tasks.

This is a useful harness pattern because it stores executable experience and failure boundaries rather than relying only on an ever-growing conversation transcript. The paper is a July 2026 preprint; its reported results should be treated under the authors' protocols, and no public code repository was linked as of 2026-07-25.

ASPIRE and GaP broaden this idea from runtime memory to system improvement. ASPIRE admits validated repairs into a transferable skill library; GaP uses simulated rehearsals to refine a persistent graph policy before real execution. Guava demonstrates a lighter-weight alternative in which recovery emerges from an iterative observation/reasoning/tool loop. These mechanisms should not be treated as interchangeable: trace retrieval, reusable skill accumulation, graph search, and in-context replanning have different compute costs and different failure modes.

HERO adds a consolidation path: heuristic reasoning and exemplar reuse bootstrap behavior from autonomous interaction, while recurring experience is converted into faster closed-loop visuomotor policies. This couples data collection, capability scheduling, and policy learning inside one improvement loop, so its safety case depends on which environments may be explored, how exemplars are admitted, and how consolidated policies are regression-tested.

### 7. Harness optimization becomes regression-gated

Self-Harness provides a general coding-agent example of optimizing the non-parametric system around a fixed model. It clusters verifier-grounded failure traces, asks the same target model for bounded and auditable harness edits, and promotes a candidate only if it improves one evaluation split without degrading the other. The accepted changes can affect instructions, tools, memory, runtime controls, verification, skills, or subagent structure.

For robotics, this pattern belongs in an offline improvement plane rather than the live safety-critical control path. Candidate changes should run through simulation, recorded-trajectory replay, shadow evaluation, independent approval, versioned deployment, and rollback. Safety limits, permission boundaries, emergency stops, and verified low-level controllers must remain outside the editable surface.

The evidence is preliminary: Self-Harness evaluates terminal agents, not robots, and uses its so-called held-out split when deciding which candidate edits to promote. Those traces are hidden from the proposer, but repeated score-based selection can still adapt to the split, so a separate untouched final test set remains necessary.

ENPIRE moves this improvement loop onto physical hardware. Its Environment module exposes bounded reset, safety, observation, and automated verification; Policy Improvement edits heuristics, behavior-cloning or RL infrastructure; Rollout preserves auditable trials; and Evolution lets multiple coding agents compare branches and reuse successful recipes across a robot fleet. This is closer to an automated robotics laboratory than runtime task planning. The distinction in reported metrics matters: the project page's 99% pass@8 permits up to eight failure-conditioned retries per subtask, so single-attempt reliability, intervention rate, robot utilization, and token cost remain separate quantities.

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

## Implementation Families

| Family | Examples | Strengths | Common harness concern |
| --- | --- | --- | --- |
| Autoregressive action tokens | OpenVLA, RT-1/2, π0-FAST | Reuses language-model training and decoding machinery. | Decode latency, token/action calibration, compounding errors. |
| Continuous regression | OpenVLA-OFT | Simple, fast, deterministic action chunks. | Median-mode behavior and limited multimodality. |
| Diffusion / flow matching | π0/π0.5, CogACT, GR00T, SmolVLA, MolmoAct 2, LingBot-VLA 2.0, Qwen-VLA/RobotManip | Expressive continuous action distributions. | Denoising latency, stochasticity, chunk consistency, and artifact availability. |
| World-model planning | V-JEPA 2-AC, DreamerV3, Robot-Factored World Models, Qwen-RobotWorld, ViTacWorld, World Action Planner, VisualPatchWorld | Predicts consequences and supports explicit planning, augmentation, policy evaluation, or inspectable executable dynamics. | Model bias, conditioning leakage, geometry/calibration drift, code validity, and planning budget. |
| Planner plus skills | RoboBRIDGE, Physical Agency, Harness VLA, Guava, ASPIRE, HERO, SayCan, ROSA, Code as Policies | Interpretable task decomposition, retry, memory, capability consolidation, and reuse of verified or learned primitives. | Tool permissions, grounding, recovery, context drift, and unsafe experience admission. |
| Runtime execution verification | CheckVLA | Uses action-conditioned predictions and calibrated thresholds to interrupt or repair action chunks after deployment-time deviations. | World-model misspecification, false interventions, repair latency, and simulation-to-real transfer. |
| Graph-as-policy search | GaP | Interpretable perception/planning/control graphs refined through simulated rehearsal. | Simulator fidelity, search cost, graph validation, transfer to hardware. |
| Safety-bounded supervisor | FORGE-plus | Keeps semantic force-budget and recovery selection above immutable low-level enforcement. | Simulation-only evidence, force-model mismatch, and unsafe editable limits. |
| Self-improving harness | Self-Harness | Trace-grounded, model-specific changes promoted through regression tests while weights remain fixed. | Evaluation leakage, search cost, unsafe editable surfaces, and rollback. |
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
