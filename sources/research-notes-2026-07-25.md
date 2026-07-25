# Research Notes — 2026-07-25

These notes preserve the primary sources used to build the first curated version of Awesome Harness Robot. Search was performed on 2026-07-25. Dynamic facts in the README are deliberately limited and marked with a last-verified date.

## Research Questions

1. What does "agent harness" mean when the agent can affect a physical robot?
2. Which open stacks currently cover VLA training, fine-tuning, evaluation, and deployment?
3. Which robot foundation and world models have usable public artifacts?
4. Which datasets, benchmarks, simulators, and middleware form the surrounding implementation stack?
5. What implementation patterns are converging across current systems?

## Key Primary Sources

### Harness framing and evaluation

- Harness VLA project: https://harnessvla.github.io/
- Harness VLA paper, arXiv:2607.08448v3: https://arxiv.org/abs/2607.08448
- Microsoft, "Agent harnesses": https://learn.microsoft.com/en-us/agent-framework/agents/harness
- Allen Institute for AI, vla-evaluation-harness: https://github.com/allenai/vla-evaluation-harness
- vla-eval paper, arXiv:2603.13966: https://arxiv.org/abs/2603.13966
- LeRobot: https://github.com/huggingface/lerobot
- StarVLA: https://github.com/starVLA/starVLA
- NASA JPL ROSA: https://github.com/nasa-jpl/rosa
- EmbodiedBench: https://github.com/EmbodiedBench/EmbodiedBench

### Open VLA and robot-policy stacks

- OpenVLA project: https://openvla.github.io/
- OpenVLA code: https://github.com/openvla/openvla
- OpenVLA-OFT project: https://openvla-oft.github.io/
- OpenVLA-OFT code: https://github.com/moojink/openvla-oft
- Physical Intelligence openpi: https://github.com/Physical-Intelligence/openpi
- SmolVLA: https://huggingface.co/blog/smolvla
- NVIDIA Isaac GR00T: https://github.com/NVIDIA/Isaac-GR00T
- MolmoAct 2 announcement: https://allenai.org/blog/molmoact2
- MolmoAct 2 code: https://github.com/allenai/molmoact2
- X-VLA: https://github.com/2toinf/X-VLA
- CogACT: https://github.com/microsoft/CogACT
- RDT-1B: https://github.com/thu-ml/RoboticsDiffusionTransformer
- RoboVLMs: https://github.com/Robot-VLAs/RoboVLMs
- Octo: https://github.com/octo-models/octo

### Partial and closed frontier systems

- Google DeepMind Gemini Robotics: https://deepmind.google/models/gemini-robotics/
- Gemini Robotics On-Device announcement: https://deepmind.google/blog/gemini-robotics-on-device-brings-ai-to-local-robotic-devices/
- Google DeepMind model cards: https://deepmind.google/models/model-cards/
- Figure Helix 02: https://www.figure.ai/news/helix-02
- Microsoft Research Rho-alpha: https://www.microsoft.com/en-us/research/story/advancing-ai-for-the-physical-world/
- Physical Intelligence π*0.6 report: https://www.physicalintelligence.company/download/pistar06.pdf

### World and foundation models

- Meta V-JEPA 2 announcement: https://ai.meta.com/blog/v-jepa-2-world-model-benchmarks/
- V-JEPA 2 code: https://github.com/facebookresearch/vjepa2
- NVIDIA Cosmos: https://github.com/NVIDIA/Cosmos
- Foundation Models in Robotics survey: https://arxiv.org/abs/2312.07843
- Robotics with Foundation Models survey: https://arxiv.org/abs/2402.02385

### Data, benchmarks, and implementation infrastructure

- Open X-Embodiment: https://robotics-transformer-x.github.io/
- DROID: https://droid-dataset.github.io/
- BridgeData V2: https://rail-berkeley.github.io/bridgedata/
- LIBERO: https://github.com/Lifelong-Robot-Learning/LIBERO
- CALVIN: https://github.com/mees/calvin
- SimplerEnv: https://github.com/simpler-env/SimplerEnv
- VLABench: https://github.com/OpenMOSS/VLABench
- RoboCasa: https://github.com/robocasa/robocasa
- RoboTwin: https://github.com/robotwin-Platform/RoboTwin
- ManiSkill: https://github.com/haosulab/ManiSkill
- ROS 2: https://github.com/ros2/ros2
- ros2_control: https://github.com/ros-controls/ros2_control
- MoveIt 2: https://github.com/moveit/moveit2
- BehaviorTree.CPP: https://github.com/BehaviorTree/BehaviorTree.CPP

## Findings Used in the Synthesis

1. A robot harness must own deterministic execution and safety because a foundation model cannot provide timing or physical guarantees.
2. The leading open development stacks increasingly standardize data, observation/action schemas, model servers, and benchmark adapters.
3. Specialized continuous action heads—diffusion, flow matching, or regression—are now at least as important as the VLM backbone.
4. Action chunking and asynchronous execution move latency management into the harness.
5. Cross-embodiment transfer requires explicit embodiment metadata and normalization; a generalist checkpoint is not plug-and-play.
6. Current evaluation is strongest in simulation. Comparable multi-site real-robot evaluation remains an open infrastructure problem.
7. World models, embodied reasoning models, and hierarchical planner/policy/controller systems are converging, but they operate at different time scales and should remain separately observable.
8. Harness VLA provides a direct robot-manipulation example of this convergence: a memory-guided coding agent orchestrates a fixed primitive library and invokes a frozen VLA only for contact-rich local phases, extending its operating range without fine-tuning.

## Limitations

- Upstream projects change rapidly; licensing and access must be rechecked before use.
- Harness VLA was available as an arXiv v3 preprint and project page, but no public code repository was linked when checked on 2026-07-25.
- Vendor-reported results and benchmark scores were not treated as directly comparable.
- Inclusion means the project is useful to study or implement, not that its safety or performance claims were independently validated.
- Search results were used to discover projects, but the curated descriptions prefer the primary sources listed above.
