# Landscape: Agent Harnesses, VLAs, and Robot Foundation Models

Last verified: **2026-08-19**.

This note explains the development arc behind the links in the main list. Dates refer to public releases or papers, not necessarily the start of internal development.

## Short Timeline

| Period | Milestone | Why it mattered |
| --- | --- | --- |
| 2022 | SayCan, RT-1, Code as Policies | Language became a practical interface for skill selection, code generation, and large-scale robot policy learning. |
| 2023 | PaLM-E, RT-2, RT-X/Open X-Embodiment | Web-scale multimodal knowledge and cross-robot data were connected to control. |
| 2024 | Octo, OpenVLA, π0, RDT-1B, CogACT | Open generalist policies diversified into token, diffusion, and flow-based action heads. |
| 2025 | OpenVLA-OFT, SmolVLA, π0.5, GR00T N1/N1.5, Gemini Robotics, V-JEPA 2 | Efficient action chunks, open-world adaptation, humanoid policies, on-device deployment, and world-model planning moved to the foreground. |
| 2026 | Self-Harness, Harness-R1, HarnessOpt-Bench, EvoHarness-RL, HELIX, AgentRewind, AI4AI, SHE, Evo-Bench, Harness-IF, REDAgentBench, SkillMisevo, EnvACE, OpenForgeRL, A²E, Harness Engineering for Physical AI, RHO, ART, ENPIRE, OpenETA, XPolicyLab, Guava, ASPIRE, GaP, Harness VLA, HarnessWAM, HyMeS, In-Context VLA, StellaVLA, AtlasVLA, Physical Agency, RoboBRIDGE, Embodied Agents Take Control, HERO, CheckVLA, ContactGuard, CoWAM, TempoWAM, SAFECAST, ReflexVLA/ReflexBench, VLA task-progress probes, InternVLA-A1, LingBot-VLA 2.0, Qwen robotics models, TurboVLA, Robot-Factored World Models, World Action Models in Real Time, ω-0, GeniWorld, XEWorld, DynamicWAM, Flex-π, RIFT, PSG-JEPA, Qwen-RobotWorld, ViTacWorld, World Action Planner, VisualPatchWorld, GAUGE, WorldSimProbe, VLA-Arena, RoboGraph, vla-evaluation-harness, StateM, Evo-Harness, ClawGym II, Bounded Agents, SCOPE, KV-cache rollback audits, Zetta, BATON, τ0-VLA, DeepInsight II, CaliBench, RigidBench, HarnessEval-W, bit-flip VLA attacks, LEGO-RL, Agent Lightning v1.0, HarnessRisk, MANIGUARD, LIBERO-VIFO, HODAgent, VLCP, Hydra-0 | The ecosystem began treating middleware enforcement, orchestration, memory, recovery, skill discovery, physical autoresearch, repository-level policy search, model–harness co-evolution, latency-aware dynamic evaluation, cross-embodiment schemas, asynchronous serving, runtime verification, planner-facing world models, physical-fidelity diagnosis, evaluation, and the harness itself as optimization targets. Newer work adds harness-native RL frameworks, lifecycle-oriented harness-safety benchmarks, specification-grounded manipulation-safety evaluation, unauthorized visual-cue-following benchmarks, System-2 humanoid service agents, in-episode closed-loop code replanning, and pixel-motion (action-flow) world-model interfaces. |

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

ReflexBench makes that temporal contract an evaluation variable. Its six dynamic manipulation tasks keep the simulator advancing while the policy computes, compare synchronous with asynchronous inference, and inject configurable latency. ReflexVLA combines latent future prediction and multi-frame fusion with batched visual encoding and CUDA Graph replay, with author-reported simulation and real-robot results. The project page says code will arrive only after acceptance, so the benchmark and model are not yet independently runnable.

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

Hydra-0 (2026-08-18) generalizes the action interface itself: robot actions are represented as pixel motion ("action flow"), so one world model learns action consequences across embodiments, tasks, environments, and video-generation backbones, and an emergent inverse mode predicts compatible robot motion from desired object flow transferred from a human demonstration, mapping the resulting latents to executable actions without task-specific expert demonstrations. Reported error reductions over an action-conditioned baseline (90.4% robot-motion, 60.2% object-motion) and r=0.96 replay/reference correlation on RoboLab are all open-loop, and the project page marks code "coming soon" as of 2026-08-19; the contribution is the shared visual interface contract rather than a released runtime.

Flex-π makes the number of active world-model streams a deployment choice. RGB appearance, pointmap geometry, DINO semantics, and actions share a denoising backbone; stream dropout during training allows the same checkpoint to run in action-only mode or generate selected futures. That is a useful harness contract for adapting compute to task difficulty, but the reported real-robot gains remain author-reported and the official repository contains only a forthcoming-release notice as of 2026-08-12.

RIFT separates production of a future representation from its consumption by the action model. Cache interventions show that tested WAMs use future K/V values, while fixed final-clean caches nearly preserve execution for selected models. Learned anticipation tokens then construct that cache in one pass, retaining the future-read interface while dropping iterative video rollout. The reported latency and benchmark gains are simulation-only, and no official implementation was linked as of 2026-08-13.

### 5. Hierarchies return

End-to-end pixels-to-actions remains attractive, but deployed systems increasingly use multiple rates and levels:

- a slow reasoning/planning model;
- a mid-rate VLA or learned skill policy;
- a high-rate whole-body or servo controller;
- a deterministic safety supervisor outside all learned components.

This is not a retreat from learning. It is a recognition that language reasoning, visuomotor prediction, contact stabilization, and safety have different data, latency, and verification requirements.

ART inserts callable modules inside the VLA trajectory rather than placing all tool use in a separate high-level planner. Low-level vision, affordance, and embodiment tools narrow the continuous action solution space, while a 30K-trajectory training set teaches long-horizon invocation. The authors report both simulated and real-world manipulation gains, but no official code, data, or model artifacts were linked as of 2026-08-17.

Harness VLA makes this boundary explicit at the manipulation-policy level. Its agent keeps semantic re-grounding, non-contact movement, re-staging, retries, and memory outside a frozen VLA; the VLA is invoked as a local contact-rich primitive. The contribution is therefore not another base policy but a method for extending a policy's operating range through orchestration and execution feedback.

OpenETA makes the hierarchy executable as an open runtime. Its planner is permitted one typed, world-changing tool call per turn; a host-owned interface validates and executes that call; and the next decision is conditioned on a fresh world observation. This is a useful physical-agent contract because permissions, timing, verification, logging, and real-versus-sim adapters remain outside the planner. The official repository includes code, tests, replayable logging, skills, memory, simulation adapters, and a UR5e deployment path; its benchmark and hardware results remain author-reported.

Physical Agency names and measures the orchestration gap between frozen motor skills used alone and the same skills inside a closed-loop planner. Its Pigey orchestrator decomposes goals, invokes either VLA policies or parameterized skills, verifies low-level observations, and recovers without policy post-training. FORGE-plus demonstrates a narrower safety-oriented hierarchy: a frozen text LLM may choose a force budget and recovery maneuver, but the deterministic controller owns enforcement and recovery cannot raise the ceiling.

RoboBRIDGE makes the deployment boundary more explicit through five replaceable modules: Monitor, Perceptor, Planner, Controller, and Robot Interface. Unlike wrappers centered on one recovery technique, it treats asynchronous perception, hierarchical recovery, embodiment adaptation, and policy selection as coordinated runtime services. Embodied Agents Take Control probes an even thinner interface: general software-agent harnesses receive a camera and discrete actions, revealing that model capability, optional waypoint tools, context growth, and latency can matter as much as bespoke embodied workflow code.

Qwen-RobotNav presents a complementary model–harness contract for navigation. An upper-level agent selects a task mode and controls visual token budget, temporal decay, camera weights, and frame sampling, while the base model emits waypoint chunks. This makes observation scheduling a typed inference-time interface rather than a fixed preprocessing choice; the current evidence and deployment demonstrations are author-reported, and the official repository states that weights are not planned for release.

The surrounding 2026 systems explore different harness boundaries. Guava isolates three reusable ingredients—iterative perception–reasoning–action, semantic action abstractions, and multimodal observations—and tests whether that interface transfers across reasoning-model scales. ASPIRE grows a code-as-policy skill library from validated repairs. GaP represents policies as directed graphs assembled by multiple coding agents, then searches graph structures and parameters in generated simulation. InternVLA-A1 takes the complementary model-centric route: it internalizes scene understanding, visual foresight, and flow-matching action generation in one Mixture-of-Transformers rather than placing those capabilities in an external agent harness. In-Context VLA sharpens the reasoning/control boundary at the model level: its authors report that free-form textual chain-of-thought degrades low-level control and that a VLA should consume grounded language (structured perceptual evidence plus agentic tool queries) rather than generate ungrounded narrative; its simulation and real-robot results are author-reported and no code was linked as of 2026-08-10.

StellaVLA applies the same principle to demonstrations. An offline pipeline converts a retrieved trajectory into a task plan, subgoals, and verbalized 3D motion, so robot, human-hand, and XR demonstrations become a common context interface. The language path is used during dual training but removed for high-rate action inference. This is author-reported across VLA-Arena, LIBERO, and real-robot tasks, with no verified public artifacts as of 2026-08-13.

HarnessWAM and HyMeS make the high/low-rate split operational. HarnessWAM keeps scene belief, a task graph, capability checks, milestone deliberation, and failure recovery outside the WAM's local action generator. HyMeS keeps motor skills in VLA weights but lets a coding agent evolve executable memory heuristics from rollout feedback, with multimodal stage verification controlling updates. Agentic Harnesses adds a semantic permission layer between planning and the robot-facing MCP server. All three are author-reported 2026 preprints without verified public implementations as of 2026-08-11.

τ0-VLA is the strongest open example in this batch of hierarchy plus deliberation: high-level subtask generation is formulated as compute-scalable inference in which a world model guides test-time search over candidate subtasks before commitment, while a low-level policy executes across embodiments. Trained on 40,115 hours of heterogeneous real-world data with multimodal co-training, it reports that additional test-time computation improves next-subtask accuracy and closed-loop long-horizon success. The Apache-2.0 code and released weights make the compute-vs-reliability trade-off directly tunable, though the reported gains remain author-reported.

Zetta applies the same closed-loop idea to the harness rather than the policy. Its three timescale-separated loops give action-frequency governance, rollout-level critic–recovery proposals, and validation-gated skill updates, so code-based runtime critics and recovery skills evolve online while the base policy stays frozen; Z-Infra decouples agent logic from heterogeneous execution resources. The authors report 90.8% on LIBERO-Pro and 93.6% on RoboCasa with an 11.1× inference speedup and zero-shot skill transfer. The repository is public (2026-08-18) but asserts no license.

BATON shows what a transition-aware memory adds to frozen-VLA agents: it makes the subtask the unit of exploration so long-horizon failure cost becomes additive (T·K) instead of multiplicative (T^K), and it equips memory with verifier-governed VLA invocation, entry-state-restoring handoffs, and lookahead strategy selection, with no parameter updates. Reported gains on RoboMemArena are +11.6% task success and +14.9% cumulative success over the SoTA; no official code was linked as of 2026-08-18.

VLCP closes the loop one level below subtask selection. Instead of retrying a fixed policy or choosing a different subtask, the harness rewrites the control code that failed: a frozen VLM writes the policy as a short Python control function, then re-observes the scene every K steps and regenerates the function from the current state delta, so a failure is caught before it compounds within a single episode. On a 57-task MuJoCo/RoboVerse sweep the authors report 35.1% pooled success versus 3.5% for the identical system queried once per episode, with a 27.3% within-episode recovery rate on failed grasps. This is a clean separation of model (frozen) and editable surface (code), but the evidence is simulation-only and no official code was linked as of 2026-08-19.

HODAgent applies the same separation to humanoid service interaction at the agent level: a semi-duplex System-2 stack (Env-Interactor, Planner, Executor, hierarchical Memory) keeps planning, execution, and interaction state coherent while the robot is mid-motion, and a shared simulation/physical interface isolates platform-specific control for the Unitree G1. The authors report 84.8%/91.5% joint success across 164 interactive simulation cases and 92%/72%/63.3% physical pass rates for atomic/composite/complete tasks. It is a useful reference architecture for situated, revisable humanoid service tasks, though results are author-reported and no official code or project link was located as of 2026-08-19.

### 6. Memory moves from prompt history to execution knowledge

Harness VLA separates two forms of operational memory:

- task-specific successful command traces used as few-shot examples for a target task;
- global success rules and failure models reused across tasks.

This is a useful harness pattern because it stores executable experience and failure boundaries rather than relying only on an ever-growing conversation transcript. The paper is a July 2026 preprint; its reported results should be treated under the authors' protocols, and no public code repository was linked as of 2026-07-25.

ASPIRE and GaP broaden this idea from runtime memory to system improvement. ASPIRE admits validated repairs into a transferable skill library; GaP uses simulated rehearsals to refine a persistent graph policy before real execution. Guava demonstrates a lighter-weight alternative in which recovery emerges from an iterative observation/reasoning/tool loop. These mechanisms should not be treated as interchangeable: trace retrieval, reusable skill accumulation, graph search, and in-context replanning have different compute costs and different failure modes.

HERO adds a consolidation path: heuristic reasoning and exemplar reuse bootstrap behavior from autonomous interaction, while recurring experience is converted into faster closed-loop visuomotor policies. This couples data collection, capability scheduling, and policy learning inside one improvement loop, so its safety case depends on which environments may be explored, how exemplars are admitted, and how consolidated policies are regression-tested.

SkillMisevo demonstrates why admission alone is not enough. It follows a skill from malicious exposure through authoring, later retrieval, benign-task contamination, and fresh-session carryover across four real coding-agent harnesses. Its SafeEvolve wrapper governs both what the persistent store may write and what a later executor may reuse. The released benchmark is digital-agent infrastructure rather than robot validation, but the lifecycle maps directly to robot skill libraries, where provenance and execution permissions must survive beyond the triggering rollout.

AtlasVLA pushes the same memory idea into the policy's own state. Instead of retrieving past context, it maintains a 4D persistent world-state memory that lifts transient 2D observations into a voxel-hashed spatial state, plus an ego-working memory that tracks task progress, and conditions a diffusion transformer on the joint world-ego state. This targets the perception-forgetting and task-progress-forgetting failures that arise when a reactive VLA sees only a wrist camera. Its results are author-reported and no code was linked as of 2026-08-10.

HyMeS chooses the opposite placement boundary: reusable motor skills remain in a Markovian VLA, while non-Markovian state is explicit executable code evolved by a coding agent. This improves inspectability and compositional reuse, but makes heuristic admission, rollback, verifier quality, and code permissions first-class harness concerns.

AgentRewind extends durable state from memory into recovery. It snapshots aligned agent context and controlled environment state, rewinds both after an early mistake, and resumes with information retained from the failed attempt. Its MettleBench evidence is for digital engineering agents; physical transfer requires compensating actions, reset guarantees, and safety checks because real robot and human environments cannot generally be restored by loading a checkpoint.

The KV-cache rollback audit adds a cross-layer requirement to that recovery model: a logical abort must restore what the model attends, not only what the application transcript shows. Retained key/value state can keep an agent attending to content the application believes it discarded—a same-token/different-cache audit flipped a typed protected effect in 25 of 63 audited cells across seven open-weight families, including under LangGraph time-travel—while transaction-local cache restoration closes the channel without a global flush. For robots, the analogue is that replaying or reverting the semantic plan is insufficient unless perception buffers, world state, and served model context are rewound together.

### 7. Harness optimization becomes regression-gated

Harness Engineering for Physical AI identifies a lower systems boundary than most agent-harness work: robot middleware already mediates control commands, compute schedules, and communication, so it is the natural place to compose learned-model enforcement. Its proposed ROS 2 Harness Profile carries declared output regions, inference budgets, and operating regimes, with Projection, Isolation, and Transfer enforcing them. This is a position and architecture paper rather than a released profile or robot evaluation, but it makes an important distinction: a physical harness must govern continuous control and timing, not only discrete tool calls.

RHO treats the executable robot policy repository itself as the artifact to optimize. A bounded coding agent mutates multi-file neurosymbolic policies during training, receives reflective feedback from environment execution, and retains candidates through Pareto-frontier selection; the final repository executes without deployment-time code generation. The reported Robosuite, LIBERO-PRO, and O3DE gains show a path from agentic search to inspectable deployment artifacts, but they remain author-reported simulation results and no official implementation repository was linked as of 2026-08-16.

HELIX couples harness evolution to the model-update loop. Typed ports, atoms, recipes, product shells, and runtime policies keep interventions identifiable, while verified successes, regressions, near misses, and alternative solutions become matched training records. The paper reports one code-repair evolution round rather than robotics; as of 2026-08-18 its official repository is live (MIT, TypeScript), decomposing four product harnesses (OpenCode, Pi Mono, Nanobot, Hermes) into 1,332 atoms across eight dimensions with 96 standard swap ports, and reporting that composed harnesses outperformed upstream harnesses on LiveCodeBench and SWE-bench subsets.

ClawGym II makes the harness itself the target of reinforcement learning. Rather than scripting how an agent should use a harness, it runs RL *through* the harness as a black box: sandboxed rollouts isolate environments and harnesses, a serving proxy captures model calls at the model boundary, multi-turn trajectories are reconstructed as prefix trees, and PPO or GRPO optimize over the recovered tree while training–inference consistency is preserved. Mix-harness training lets one model be jointly optimized by heterogeneous harnesses. The reported Pass@1 gains (Qwen3-30A3B, +9.98 and +14.81 points through OpenClaw and Claude Code, stable over 200–400 steps) are digital-agent results with no robot experiment and no official code link as of 2026-08-18.

Two entries reviewed in the 2026-08-19 scan make harness-native RL a documented framework line. Agent Lightning v1.0 names the paradigm—*harnessed agentic RL*—in which the deploy-time harness owns the environment loop and the trainer observes only LLM request–response pairs through an endpoint proxy; its ~3,500-line framework targets retokenization, sample merging, advantage calculation, loss normalization, and backend scheduling as first-class engineering problems, and reports improving Qwen3.5-9B on SWE-bench Verified with 6K examples. LEGO-RL attacks the same misalignment from the execution side: in-process LLM proxying preserves token-level alignment even when the harness compacts or re-serializes generations, sandbox orchestration with stage-wise defenses mitigates reward hacking, and an observability plugin keeps training inspectable; the authors report SWE-bench Verified gains across three native harnesses (OpenHands SDK, Claude Code, OpenCode) while maintaining a rollout–training probability correlation above 0.99. HarnessRisk adds a lifecycle-oriented safety benchmark for exactly these systems: 128 sandboxed cases across six harness phases (configuration, capability extension, runtime, state persistence, action control, incident recovery) show attack success ranging 12.6–80.9% with Harness Configuration the most vulnerable phase, and its MIT-licensed code and public dataset make the cases reproducible. All three are digital-agent results without robot validation.

StateM and Evo-Harness turn harness scaling and harness learning into released, reproducible artifacts. StateM is a runtime that organizes execution around durable states, phase-local context, checked transitions, recoverable runbooks, and versioned procedural practices; its Apache-2.0 repository is public, and the authors report 95.3% raw accuracy on Terminal-Bench 2.1 with GPT-5.6 Sol xhigh at ~$15 of final-score API usage, with runbooks transferring across models. Evo-Harness formalizes online harness learning—a frozen solver improves by compiling noisy, single-shot executions into a structured, inspectable skill harness—and releases an MIT-licensed implementation with five benchmark pipelines. Both are digital-agent results without robot validation.

SCOPE applies the regression-gated pattern to frozen video world models at inference time: external controls become a typed state updated only through bounded, evidence-supported changes, candidates are filtered score-blind, selection uses conformal risk control, and the resulting policy is frozen before held-out evaluation. The authors report +14.24 (95% CI [+8.10, +21.23]) on Physics-IQ over the exact frozen base, while finding that gains do not transfer uniformly across backbones. The released code asserts no license as of 2026-08-18.

Bounded Agents extends the editable surface from capability to delegation. The Agentic Principal Chain enforces six conjunctive checks—identity, scope and composition, context, approval, evidence, intent—over accumulated session state, narrows delegated scope monotonically across hops, and evaluates composition restrictions over session history rather than per action. Its Apache-2.0 reference implementation (215 tests, zero runtime dependencies) and evaluation data are public; reported AgentDojo exfiltration drops from 75–100% to 0% at a utility cost of 8.6–13.9 percentage points. This is digital multi-agent infrastructure, but the same enforcement pattern maps to robot tool permissions and skill delegation.

Bit-flip attacks add a new physical-security boundary for embodied foundation models: quantized VLA weights are vulnerable to Rowhammer-style faults, and the flip budget depends sharply on the action head (1–5 flips for direct regression or token policies, ~100–300 for the evaluated flow-matching policies), with damage concentrating in a few action-generating layers. Task-calibrated emulated flips produced 0/20 real-robot successes versus 14/20 clean, making weight integrity a deployment concern that mirrors the existing perception-level injection literature (physical signage, communication attacks) at the model-parameter level. Code is released as arXiv ancillary material.

Self-Harness provides a general coding-agent example of optimizing the non-parametric system around a fixed model. It clusters verifier-grounded failure traces, asks the same target model for bounded and auditable harness edits, and promotes a candidate only if it improves one evaluation split without degrading the other. The accepted changes can affect instructions, tools, memory, runtime controls, verification, skills, or subagent structure.

For robotics, this pattern belongs in an offline improvement plane rather than the live safety-critical control path. Candidate changes should run through simulation, recorded-trajectory replay, shadow evaluation, independent approval, versioned deployment, and rollback. Safety limits, permission boundaries, emergency stops, and verified low-level controllers must remain outside the editable surface.

The evidence is preliminary: Self-Harness evaluates terminal agents, not robots, and uses its so-called held-out split when deciding which candidate edits to promote. Those traces are hidden from the proposer, but repeated score-based selection can still adapt to the split, so a separate untouched final test set remains necessary.

ENPIRE moves this improvement loop onto physical hardware. Its Environment module exposes bounded reset, safety, observation, and automated verification; Policy Improvement edits heuristics, behavior-cloning or RL infrastructure; Rollout preserves auditable trials; and Evolution lets multiple coding agents compare branches and reuse successful recipes across a robot fleet. This is closer to an automated robotics laboratory than runtime task planning. The distinction in reported metrics matters: the project page's 99% pass@8 permits up to eight failure-conditioned retries per subtask, so single-attempt reliability, intervention rate, robot utilization, and token cost remain separate quantities.

Harness-R1 takes a different route from Self-Harness: it post-trains a separate 9B harness engineer with supervised cold start and online reinforcement learning, using fresh reruns of a frozen target agent to reward executable lifecycle-hook patches. The official repository and model weights are released, but the reported WebShop, ALFWorld, and DBBench results contain no robot experiments. In robotics, same-batch repair rewards would need stronger temporal separation, untouched evaluation sets, simulation and replay gates, and immutable host safety boundaries before any candidate patch reached hardware.

HarnessOpt-Bench turns this improvement loop into a measured capability. An optimizer receives a seed harness, graded feedback, and a fixed target-evaluation budget; a trusted execution environment hides the held-out test partition, meters resource use, and preserves every candidate version. This is a stronger search/evaluation separation than repeatedly selecting on a visible regression set, but the current four downstream tasks are digital-agent tasks rather than robotics. A robot adaptation would additionally need simulator and replay budgets, hardware-use accounting, immutable safety tests, and a final multi-site physical evaluation.

EvoHarness-RL moves from proposing harness edits to learning the harness policy itself. It exposes Belief, Progress, and Experience as policy-facing harness state, teaches the base agent the harness action space by supervised fine-tuning, and uses cost-aware GRPO to learn when to read, update, and consolidate external state during long-horizon interaction. Its reported 96.9% ALFWorld success with Qwen3-8B is a digital-agent result, but the reported dynamics—harness annealing, where the model internalizes recurring harness use and calls the workspace less often, and harness evolution, where progress and experience consolidate into a compact task-adaptive substrate—are directly relevant to how a physical-agent harness should decide which state lives in the model versus in the external workspace. No code was linked as of 2026-08-10.

EnvACE offers a complementary training-time route: the agent alternates between acting and rehearsing the environment response, jointly optimizing both roles with task-success rewards so the environment dynamics are internalized as an agent world model. This replaces external environment interaction during training and enables private rehearsal before committed execution at test time. The released repository is a genuine implementation, but the reported gains are on digital tool-use benchmarks rather than robots, so physical-agent transfer remains unvalidated.

Two infrastructure pieces complete the loop. OpenForgeRL makes harness-native agents trainable: a proxy records the harness's model calls as training data for a standard RL codebase while a Kubernetes orchestrator runs each rollout in an isolated container, so stateful multi-process harnesses can be fine-tuned end-to-end in arbitrary environments. A²E makes harness evaluation systematic in the other direction: an Agent Task Protocol integrates evaluation tasks across harnesses, an instrumented Monitor captures standardized execution traces, and multidimensional metrics replace correctness-only scoring. Both are described or released as open source, but neither evaluates robots.

SHE and Evo-Bench add missing controls around evolution. SHE assigns safety responsibility to four artifacts and converts failed trajectories into localized, safety–utility-tested changes; Evo-Bench uses harness-sensitive task construction and stratified transfer splits to ask whether a model improved the harness rather than memorized a workflow. Their evidence is entirely digital-agent based, so a robotics version still needs immutable hardware limits, replay and simulation gates, physical-use budgets, and a truly untouched final test.

AI4AI and Harness-IF expose two more evaluation boundaries. AI4AI asks whether a strong builder can transfer capability into deterministic code, routing, and formatting around a weaker frozen target. Harness-IF asks whether rules placed on five harness surfaces changed behavior at all, using rule-withheld probes to separate compliance from coincidence and conflict tests to inspect precedence. Both are digital/coding-agent studies; neither validates physical transfer.

REDAgentBench adds a complementary measurement boundary: attacks execute inside isolated service sandboxes, while receipts and final-state changes establish whether a harmful effect actually occurred. Separating exposure, execution, observation, and adjudication reveals when an apparent safety improvement is only an evidence-visibility artifact. Its current cases are digital service agents; a robot adaptation would need signed state/action receipts, physical consequence models, immutable controllers, and hardware-aware adjudication.

### 8. Evaluation becomes a harness, not a notebook

VLA results were historically difficult to compare because each model carried its own environment fork, dependency set, observation adapter, action scaling, and rollout script. The emerging pattern is:

- benchmark environments in pinned containers;
- models behind versioned servers;
- declarative observation/action adapters;
- batch rollout scheduling;
- full trajectory recording;
- schema-validated metrics and reproducible configs.

The remaining gap is standardized, multi-site real-robot evaluation. Simulation success is useful for regression testing but is not a substitute for physical robustness.

XPolicyLab is the clearest attempt in this batch to standardize the wiring itself. Its public repository defines observation, action, trajectory, lifecycle, and reset contracts, isolates policy dependencies from simulator or robot clients, and supplies adapters for 42 policies across RoboTwin, RoboDojo, and real-robot paths. This changes integration complexity from bespoke policy–environment pairs toward reusable policy and environment adapters; its time-saved measurements remain author-reported.

CheckVLA illustrates how evaluation logic can also become an online runtime service. It compares observed evolution with an action-conditioned world-model reference, calibrates when to intervene, and rewrites only the suffix that remains executable after inference latency. Its current RoboCasa365 evidence is simulation-only, but the separation between action proposal, execution evidence, verifier, and repair is a reusable harness pattern.

CoWAM and SAFECAST sharpen two adjacent contracts. CoWAM allows a bimanual policy intervention only when typed coordination obligations and calibrated gates admit it, otherwise retaining the nominal action or abstaining; its current evidence is eight simulated tasks. SAFECAST augments hidden-state failure-probe training and calibration with visual and language contrast sets, with author-reported results on real-world DROID rollout data and LIBERO simulation. Neither arXiv page linked official code as of 2026-08-06, and neither result is an independent safety certification.

ContactGuard and the π0.5 task-progress probe move monitoring to two earlier boundaries. ContactGuard rolls a latent world model forward under a planned chunk and may abort before contact; the progress probe reads a semantic progress signal from internal activations and flags stalls without labels. The former has author-reported live-robot abort experiments and the latter is observational rather than a steering mechanism. Neither exposed an official implementation as of 2026-08-14.

GAUGE adds a complementary environment-level diagnosis. Rather than judging a simulator or video world model only by pixels or human preference, it pairs real trajectories and calibrated metadata with observables for collision, friction, momentum transfer, oscillation, self-contact, and deformation. The authors report that no tested physics engine is uniformly faithful and that video models may reproduce the expected equation form while inferring incorrect physical parameters. Its 22-task design is valuable for harness regression, but no official code or dataset release was linked as of 2026-08-07.

XEWorld extends the same evaluative stance to embodiment generalization. By holding out entire robots inside physically identical scenes, it tests whether an action-conditioned world model renders embodiments it has never seen. The authors report that current models behave as 2D visual pattern matchers—generalization follows visual similarity rather than kinematic similarity, zero-shot rendering of unseen embodiments requires heavily grounded pixel-space actions and explicit spatial-temporal alignment, and few-shot adaptation triggers catastrophic forgetting of seen embodiments. These are author-reported diagnostics over existing models, not an independent certification, but they define the right question for a world-model harness: does the model know physics, or only pixels? No code or benchmark release was linked as of 2026-08-10.

On the deployment side, DynamicWAM shows one concrete serving answer for dynamic manipulation: a distilled world-action model conditioned on history-flow optical flow and kinematic descriptors, executed through real-time chunking so motion awareness and asynchronous execution are coupled rather than traded off. Its DOMINO and real-robot results are author-reported, and the official implementation repository was verified as public as of 2026-08-10.

TempoWAM and WorldSimProbe examine the two sides of action-conditioned execution. TempoWAM monitors observed progress to decide whether to keep executing a predicted chunk or replan, replacing a fixed prefix with a calibrated runtime policy. WorldSimProbe tests whether an action-conditioned world model satisfies the more basic simulator contract that actions cause the intended motion and interactions are grounded in that realized motion. RoboGraph complements both by varying the horizon over which an agent must preserve task-relevant state, including failures and interventions. None had a verified public implementation or benchmark repository as of 2026-08-11.

PSG-JEPA questions the JEPA assumption itself: is forward prediction in latent space enough for control-relevant representations? It adds grounding objectives—latents matched to robot proprioceptive state and latent pairs matched to multi-horizon joint-angle changes—applied only during training, and evaluates identifiability, planning, and policy performance separately. This is a useful reminder that a world model's latent space must be tied to quantities the controller can use, not only to future observations. An official repository exists but marks code as coming soon as of 2026-08-10.

On the benchmark side, VLA-Arena standardizes how capability boundaries are measured: 11 task suites across Safety, Distractor, Extrapolation, and Long Horizon (170 tasks), with difficulty varied along task structure, language command, and visual observation axes and fine-tuning restricted to the easiest level. The official repository is public, and model results are author-reported.

DeepInsight II treats the physical half of the evaluation stack as the object of study. It reproduces released-checkpoint references on two navigation and four manipulation benchmarks under native protocols, places four released whole-body controllers under one workload and metric contract (MotionBench), and carries a qualified cohort from parallel simulation to matched real-robot trials in which simulated and physical rollouts share a parent trace identity—making the sim-to-real gap a native reduction rather than a cross-toolchain reconciliation. A composed System 2–1–0 study maps trace localization to five evidence-grounded handoff labels, each tied to a concrete repair action with a measured repairability criterion, and tests the same attribution under hardware-observable state. The contribution is evaluation continuity and repair-oriented diagnosis rather than a new evaluation architecture or artifact release.

World-model evaluation is also becoming distribution-aware. CaliBench scores stochastic video-world-model generations in physically interpretable discrete outcome spaces (Galton boards, Bernoulli forks, dice, cards, roulette) whose reference distributions are known in closed form, decomposing performance into scorability and calibration and finding pervasive miscalibration and outcome collapse across six image-to-video models. RigidBench instead separates motion, geometry, identity, background stability, and visual similarity in generated continuations against simulator reference rollouts, releasing code and a 5,000-video dataset with exact simulator state; its rankings depend strongly on the measurement (higher SSIM accompanies larger 3D trajectory error, r = 0.89). HarnessEval-W agentifies the judging itself: specialized sub-agents reason over measurable subproblems and a parent agent validates evidence into a verdict, turning every evaluation into a transparent evidence tree, with the pipeline released as a live benchmark. All three are video-world-model evaluations rather than robot experiments; GAUGE remains the closest robot-facing counterpart.

Robot-manipulation safety evaluation is gaining the same specification rigor. MANIGUARD (2026-08-18) defines safety independently of task success and enforces it with LTL_f-grounded automaton monitors over physics-grounded predicates across 200 locked base tasks × five perturbation axes (1,000 scenarios), releasing 8,000 safety-annotated demonstrations; across 23,000+ rollouts, 6–21% of *successful* rollouts violate the specification, and fine-tuning on the suite raises safe completion from near zero to 7.5–29.8%. Its datasets are public (CC-BY-4.0), while the code repository returned 404 at verification time. LIBERO-VIFO adds a capability-and-safety benchmark for visual cue following: eight cue families across authorized-following and unauthorized-following protocols show current VLAs will execute cue-indicated tasks without language instruction, an emerging risk corroborated by real-robot deployment. HarnessRisk brings the lifecycle view to digital-agent harnesses (six operational phases, 128 sandboxed cases, MIT code and public dataset), finding Harness Configuration the most vulnerable phase and explicit risk recognition an unreliable predictor of safe action.

## Implementation Families

| Family | Examples | Strengths | Common harness concern |
| --- | --- | --- | --- |
| Autoregressive action tokens | OpenVLA, RT-1/2, π0-FAST | Reuses language-model training and decoding machinery. | Decode latency, token/action calibration, compounding errors. |
| Continuous regression | OpenVLA-OFT | Simple, fast, deterministic action chunks. | Median-mode behavior and limited multimodality. |
| Diffusion / flow matching | π0/π0.5, CogACT, GR00T, SmolVLA, MolmoAct 2, LingBot-VLA 2.0, Qwen-VLA/RobotManip | Expressive continuous action distributions. | Denoising latency, stochasticity, chunk consistency, and artifact availability. |
| World-model planning and interaction | V-JEPA 2-AC, DreamerV3, Robot-Factored World Models, Qwen-RobotWorld, ViTacWorld, GeniWorld, World Action Planner, VisualPatchWorld, DynamicWAM, Flex-π, RIFT, HarnessWAM | Predicts consequences and supports explicit planning, interactive rollouts, augmentation, policy evaluation, deliberation, recovery, configurable or rollout-free future interfaces, and inspectable executable dynamics. | Model bias, conditioning leakage, geometry/calibration drift, code validity, artifact availability, and planning budget. |
| Whole-body world-action models | ω-0 | Couples predictive latent objectives to controller-compatible locomotion and manipulation actions. | Dataset access, whole-body safety, controller dependence, artifact availability, and independent real-robot replication. |
| Planner plus skills | OpenETA, RoboBRIDGE, Physical Agency, Harness VLA, RHO, Guava, ASPIRE, HERO, SayCan, ROSA, Code as Policies | Interpretable task decomposition, typed tool calls, host-owned execution, retry, memory, capability consolidation, repository-level policy search, and reuse of verified or learned primitives. | Tool permissions, grounding, recovery, context drift, policy-repository validation, and unsafe experience admission. |
| Asynchronous action-chunk serving | World Action Models in Real Time, TempoWAM | Overlaps inference with execution and makes observation–prediction–command alignment and adaptive chunk commitment explicit runtime contracts. | Stale observations, chunk-boundary discontinuities, unsafe switching, monitor error, and hardware-specific timing. |
| Runtime execution verification | CheckVLA, ContactGuard, CoWAM, TempoWAM, SAFECAST, VLA task-progress probes | Uses predicted futures, pre-contact latent consequences, progress signals, coordination contracts, or calibrated probes to detect and constrain deployment-time failures. | Model misspecification, false interventions, repair latency, hidden-state drift, calibration shift, and simulation-to-real transfer. |
| Graph-as-policy search | GaP | Interpretable perception/planning/control graphs refined through simulated rehearsal. | Simulator fidelity, search cost, graph validation, transfer to hardware. |
| Safety-bounded supervisor | FORGE-plus | Keeps semantic force-budget and recovery selection above immutable low-level enforcement. | Simulation-only evidence, force-model mismatch, and unsafe editable limits. |
| Harness optimization and transfer | Self-Harness, Harness-R1, HarnessOpt-Bench, EvoHarness-RL, AI4AI, SHE, Evo-Bench, Harness-IF, EnvACE, OpenForgeRL, A²E | Proposes, learns, transfers, or evaluates non-parametric structures around fixed targets, with budgeted evaluation, execution-grounded rule checks, safety-attributed evolution, or training/audit infrastructure. | Evaluation leakage, same-batch reward overfitting, instruction precedence, search cost, unsafe editable surfaces, and rollback. |
| Executable, persistent-skill, and perception-threat audit | REDAgentBench, SkillMisevo, Hijacking Robots with a Piece of Paper | Verifies digital-agent effects from service state, tracks unsafe skills across authoring/retrieval/execution, and evaluates physical prompt injection against VLM-controlled robots. | Digital-to-physical transfer, persistent-store provenance, attack-transfer assumptions, defense trade-offs, artifact availability, and no independent benchmark replication yet. |
| Robot-team communication security | When Coordination Becomes a Threat | Studies communication attacks in LLM-planned multi-robot systems across DMAS and HMAS architectures. | Simulation-only evidence, architecture coverage, and no released benchmark artifacts. |
| Calibrated operator handoff | AutoIntervene | Scores action chunks against a visual-action support memory and transfers control to an operator with calibrated thresholds. | Project page only; no verified implementation or real-robot replication yet. |
| VLA and agent-state benchmarking | VLA-Arena, RoboGraph, WorldSimProbe | Measures structured VLA difficulty, task-state horizons, and whether action-conditioned world models satisfy observable simulator contracts. | Model results are author-reported; artifact availability, leaderboard coverage, and hardware portability are open. |
| Physical autoresearch harness | ENPIRE | Automates reset, verification, policy experiments, rollout auditing, and multi-agent evolution on real robot fleets. | Hardware wear, unsafe experiment generation, verifier gaming, retry-sensitive metrics, robot utilization, and token cost. |
| Middleware-level Physical AI harness | Harness Engineering for Physical AI | Places output projection, resource isolation, and verified fallback transfer at the ROS 2/DDS/Zenoh integration boundary. | Position paper only; profile implementation, timing analysis, and hardware validation remain open. |
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
