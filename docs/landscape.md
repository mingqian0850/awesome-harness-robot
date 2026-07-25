# Landscape: Agent Harnesses, VLAs, and Robot Foundation Models

Last verified: **2026-07-25**.

This note explains the development arc behind the links in the main list. Dates refer to public releases or papers, not necessarily the start of internal development.

## Short Timeline

| Period | Milestone | Why it mattered |
| --- | --- | --- |
| 2022 | SayCan, RT-1, Code as Policies | Language became a practical interface for skill selection, code generation, and large-scale robot policy learning. |
| 2023 | PaLM-E, RT-2, RT-X/Open X-Embodiment | Web-scale multimodal knowledge and cross-robot data were connected to control. |
| 2024 | Octo, OpenVLA, π0, RDT-1B, CogACT | Open generalist policies diversified into token, diffusion, and flow-based action heads. |
| 2025 | OpenVLA-OFT, SmolVLA, π0.5, GR00T N1/N1.5, Gemini Robotics, V-JEPA 2 | Efficient action chunks, open-world adaptation, humanoid policies, on-device deployment, and world-model planning moved to the foreground. |
| 2026 | Guava, ASPIRE, GaP, Harness VLA, InternVLA-A1, vla-evaluation-harness, GR00T N1.7, MolmoAct 2, Helix 02, Gemini Robotics-ER 1.6 | The ecosystem began treating orchestration, memory, recovery, skill discovery, graph-structured policies, evaluation, world-model foresight, and reasoning/action separation as shared infrastructure. |

## Architecture Trends

### 1. From tokenized actions to specialized action experts

OpenVLA and RT-style systems demonstrated that robot actions can be represented as language-like tokens. Newer systems frequently preserve a pretrained VLM for semantics while attaching a dedicated action module:

- diffusion Transformer heads in CogACT and GR00T;
- flow-matching action experts in π0, SmolVLA, and MolmoAct 2;
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

### 4. Web video and human video supplement expensive robot data

Robot demonstrations are scarce and fragmented. Current approaches combine:

- web image/text data for semantics;
- egocentric human video for manipulation priors;
- simulation and world-model-generated trajectories;
- multi-institution robot datasets;
- small embodiment-specific post-training sets.

Human video does not contain robot actions, so latent-action learning, retargeting, annotation, or staged pretraining is required.

### 5. Hierarchies return

End-to-end pixels-to-actions remains attractive, but deployed systems increasingly use multiple rates and levels:

- a slow reasoning/planning model;
- a mid-rate VLA or learned skill policy;
- a high-rate whole-body or servo controller;
- a deterministic safety supervisor outside all learned components.

This is not a retreat from learning. It is a recognition that language reasoning, visuomotor prediction, contact stabilization, and safety have different data, latency, and verification requirements.

Harness VLA makes this boundary explicit at the manipulation-policy level. Its agent keeps semantic re-grounding, non-contact movement, re-staging, retries, and memory outside a frozen VLA; the VLA is invoked as a local contact-rich primitive. The contribution is therefore not another base policy but a method for extending a policy's operating range through orchestration and execution feedback.

The surrounding 2026 systems explore different harness boundaries. Guava isolates three reusable ingredients—iterative perception–reasoning–action, semantic action abstractions, and multimodal observations—and tests whether that interface transfers across reasoning-model scales. ASPIRE grows a code-as-policy skill library from validated repairs. GaP represents policies as directed graphs assembled by multiple coding agents, then searches graph structures and parameters in generated simulation. InternVLA-A1 takes the complementary model-centric route: it internalizes scene understanding, visual foresight, and flow-matching action generation in one Mixture-of-Transformers rather than placing those capabilities in an external agent harness.

### 6. Memory moves from prompt history to execution knowledge

Harness VLA separates two forms of operational memory:

- task-specific successful command traces used as few-shot examples for a target task;
- global success rules and failure models reused across tasks.

This is a useful harness pattern because it stores executable experience and failure boundaries rather than relying only on an ever-growing conversation transcript. The paper is a July 2026 preprint; its reported results should be treated under the authors' protocols, and no public code repository was linked as of 2026-07-25.

ASPIRE and GaP broaden this idea from runtime memory to system improvement. ASPIRE admits validated repairs into a transferable skill library; GaP uses simulated rehearsals to refine a persistent graph policy before real execution. Guava demonstrates a lighter-weight alternative in which recovery emerges from an iterative observation/reasoning/tool loop. These mechanisms should not be treated as interchangeable: trace retrieval, reusable skill accumulation, graph search, and in-context replanning have different compute costs and different failure modes.

### 7. Evaluation becomes a harness, not a notebook

VLA results were historically difficult to compare because each model carried its own environment fork, dependency set, observation adapter, action scaling, and rollout script. The emerging pattern is:

- benchmark environments in pinned containers;
- models behind versioned servers;
- declarative observation/action adapters;
- batch rollout scheduling;
- full trajectory recording;
- schema-validated metrics and reproducible configs.

The remaining gap is standardized, multi-site real-robot evaluation. Simulation success is useful for regression testing but is not a substitute for physical robustness.

## Implementation Families

| Family | Examples | Strengths | Common harness concern |
| --- | --- | --- | --- |
| Autoregressive action tokens | OpenVLA, RT-1/2, π0-FAST | Reuses language-model training and decoding machinery. | Decode latency, token/action calibration, compounding errors. |
| Continuous regression | OpenVLA-OFT | Simple, fast, deterministic action chunks. | Median-mode behavior and limited multimodality. |
| Diffusion / flow matching | π0/π0.5, CogACT, GR00T, SmolVLA, MolmoAct 2 | Expressive continuous action distributions. | Denoising latency, stochasticity, chunk consistency. |
| World-model planning | V-JEPA 2-AC, DreamerV3 | Predicts consequences and supports explicit planning. | Model bias, planning budget, reward/goal specification. |
| Planner plus skills | Harness VLA, Guava, ASPIRE, SayCan, ROSA, Code as Policies | Interpretable task decomposition, retry, memory, and reuse of verified or learned primitives. | Tool permissions, grounding, recovery, context drift. |
| Graph-as-policy search | GaP | Interpretable perception/planning/control graphs refined through simulated rehearsal. | Simulator fidelity, search cost, graph validation, transfer to hardware. |
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
