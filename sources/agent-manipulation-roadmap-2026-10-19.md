# Source Record: Simulation-First Agent Manipulation Roadmap

Date: **2026-10-19 (Europe/Berlin)**.

## Scope

This is a focused documentation update following a request to preserve a learning plan for a reader with ROS/manipulator experience who will start in simulation. It is not a daily arXiv scan, a full repository re-verification, an implementation release, or an independent reproduction of the cited papers.

Added [the Chinese learning roadmap](../docs/agent-manipulation-roadmap.zh-CN.md) and discovery links in the main README. The README remains in English. Existing paper entries are retained rather than duplicated, and the repository-wide historical verification badge is unchanged.

## Primary Sources Checked

| Source | Role in the roadmap | Verification boundary |
| --- | --- | --- |
| [Towards the Harness of Embodied Agents / Thea](https://arxiv.org/abs/2608.11246) | Tool orchestration, scene context, termination and result feedback | arXiv abstract and submission record checked; no implementation or real-robot result reproduced. |
| [Robo-Harness K1](https://arxiv.org/abs/2609.29389) | Perception and geometry exposed as callable tools | arXiv abstract and submission record checked; results remain author-reported; no fresh code-release audit claimed. |
| [World Action Agent](https://arxiv.org/abs/2609.29964) | Action rehearsal, visual correction, reusable skills and trace-based training | arXiv record checked, with full-text inspection in the supporting discussion; no independent reproduction or fresh code-release audit claimed. |
| [robosuite Basic Usage](https://robosuite.ai/docs/basicusage.html) / [Controllers](https://robosuite.ai/docs/modules/controllers.html) | Environment loop and controller/action interface | Official documentation used as the implementation starting point; local installation and execution are outside this documentation task. |
| [LeRobot ACT](https://huggingface.co/docs/lerobot/act) | Demonstration-based action-policy learning | Official documentation checked; no measured training time, hardware requirement, sample count or success-rate promise is made for the proposed project. |
| [LIBERO](https://github.com/Lifelong-Robot-Learning/LIBERO) | Standardized manipulation task evaluation | Official repository checked; dependency compatibility with a separate robosuite experiment is not assumed. |
| [openpi](https://github.com/Physical-Intelligence/openpi) | Parallel VLA route and later policy integration | Official repository checked; availability of checkpoints is not a guarantee of transfer to an arbitrary embodiment or compute budget. |

## Editorial Decisions

- Treat agent + harness as an active research direction, not an established exclusive architecture for manipulation.
- Separate procedural skill memory from learned motor-policy parameters; persistent text alone does not update a grasping policy.
- Preserve the eight-week plan, project scope, five conceptual tools, failure-recovery tests, reading order and first-week deliverables.
- Make oracle-state access explicit and keep agent-visible feedback distinct from independent final evaluation.
- Add development/validation/test separation, paired evaluation conditions, skill versioning, and metrics for success, recovery, latency, cost and failure attribution.
- Label the interfaces as proposed contracts rather than existing SDK APIs. No code, model training or simulator setup is included.
- The schedule and experiment design are recommendations. The suggested 30–50 initial episodes per condition are a starting point, not a power analysis or a completed experiment.

## Validation

- Checked patch whitespace, fenced blocks, the example JSON, both new documents' relative links and anchors, and the README discovery links.
- All eight distinct external URLs in the new documents returned HTTP 200 during the focused link check. Reachability does not establish artifact completeness or experimental reproducibility.
- Reviewed source coverage and limited the intended commit to the README, roadmap and this source record. Unrelated local architecture edits and untracked files are outside the update.
- This is documentation only; no simulator execution, policy training or robot experiment was performed. Publication verification compares the local commit with the remote main branch and the published document content.
