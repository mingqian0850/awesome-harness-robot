# Robot harness updates: from tool calls to verifiable execution

[Home](../README.md) · [中文](topic-progress-2026-10-21.zh-CN.md)

Checked **2026-10-21**. This is a recent topic increment, with paper, code-snapshot and announcement dates distinguished; historical entries were not all reverified. Method results are author-reported; selection and exercises are engineering guidance.

## Choose by the missing system component

| Gap | Recent work | Input to output | Boundary to verify |
| --- | --- | --- | --- |
| Explore a task, then consolidate it into a program | RAX, code snapshot Oct 19 | Exploration records → reusable skills/program | Main is an index; simulator and hardware branches differ |
| Agent-written low-level actions are hard to inspect | RobotUse, Oct 4 / v2 Oct 6 | Visual target/gripper pose → geometry, planning, execution | Simulation integration released; physical Panda adapter excluded |
| Adapt a frozen controller to a new process | InterEvolve, Oct 1 / v2 Oct 4 | Text → staged reward program → simulation validation → execution | No retraining still requires a prepared controller and simulator |

## RAX: separate exploration from execution

The [pinned RAX snapshot](https://github.com/robot-agent-exploration/rax/tree/2652c73f51514cbb84ef0fdf5a8aee2ba5b53fb7) lets coding agents explore through robot tools, then consolidate programs that run without the agent. Main indexes `humanoid-arena/*`, `dexjoco/*`, `real/franka`, `real/rainbow` and other branches; cloning main alone is not a complete runtime setup.

**Start with:** the [hardware adaptation guide](https://github.com/robot-agent-exploration/rax/blob/2652c73f51514cbb84ef0fdf5a8aee2ba5b53fb7/docs/REAL_ROBOT_ADAPTATION.md). Map tool contracts, control modes, recording and operator responsibilities before choosing a branch. Humanoid simulation differs from Franka/RB-Y1 hardware evidence; neither proves industrial humanoid operation.

**Acceptance record:** separate exploration time/model calls from independent program runtime, reset conditions and held-out performance. A faster compiled program is not faster model inference. Read interfaces/log structures on an 8GB laptop without assuming every simulator fits.

## RobotUse: separate model choice from geometric execution

[RobotUse](https://arxiv.org/abs/2610.04929v2) combines visual target/pose selection with backend geometry, planning and control, using subgoal contexts and persistent playbooks. Trace a decision through the pinned [playbook loader](https://github.com/robotuse-team/RobotUse/blob/60bdf7b31b9a9a45dbc4809d678c008c724f465b/src/agent/playbook/loader.py) and [controller interface](https://github.com/robotuse-team/RobotUse/blob/60bdf7b31b9a9a45dbc4809d678c008c724f465b/src/backend/controller.py).

The [official project](https://robotuse-team.github.io/) releases the core harness, visual tools and RoboLab integration, **excluding the physical Panda adapter**. Aggregate RoboLab results and selected robot replays are different evidence sets.

**Exercise:** retain source image, selected target, candidate pose, command, returned observation and success criterion. Identify which component detects a completed tool call that failed to place the object.

## InterEvolve: revise task programs within controller competence

[InterEvolve](https://arxiv.org/abs/2610.02196v2) expresses staged reward programs, revises structure with an LLM and constants with numerical optimization, and uses parallel simulation feedback before execution by a frozen controller. Its [project](https://sirui-xu.github.io/InterEvolve/) shows G1 hardware and notes external motion-capture object poses in some examples; not every experiment uses onboard-only perception. Executable code release was not verified here.

**Engineering implication:** decompose processes into stage objectives, completion criteria and tunable parameters. Check controller coverage before composing behaviors. No test-time retraining does not eliminate controller preparation.

## Place these components in a complete system

The [October 14 survey](https://arxiv.org/abs/2610.20442) organizes Model, Data, Environment and Harness responsibilities. Use it to find gaps, not as a deployment implementation. Draw observation → decision → command → feedback → verification, recording fields, deadlines, failures and retry rules on each edge. These are learning references; none was reproduced in this repository.
