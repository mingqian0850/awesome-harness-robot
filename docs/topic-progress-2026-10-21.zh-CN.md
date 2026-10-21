# 机器人 harness 近期进展：从工具调用到可验证执行

[首页](../README.md) · [English](topic-progress-2026-10-21.en.md)

核查日期：**2026-10-21**。本页是近期主题增量，论文日期、代码快照和产品公告分别标注；不代表全部历史条目已重新核验。方法效果为作者报告，选择与练习为工程建议。

## 根据系统缺口查实现

| 当前缺口 | 近期实现/工作 | 输入到输出 | 必须核对的边界 |
| --- | --- | --- | --- |
| 希望 agent 探索任务，再固化成程序 | RAX，代码快照 10-19 | 探索记录 → 可复用技能/执行程序 | main 只是索引；各仿真/真机分支环境不同 |
| agent 一直写底层动作，难以检查选择 | RobotUse，10-04 / v2 10-06 | 视觉目标/夹爪姿态 → 几何规划与执行 | 已公开仿真接入；Panda 真机 adapter 未随发布包含 |
| 冻结控制器后，如何适配新工序 | InterEvolve，10-01 / v2 10-04 | 文本 → 分阶段 reward program → 仿真验证 → 执行 | 无需重新训练不等于不需要预训练控制器与仿真 |

## RAX：探索阶段与运行阶段分开看

[RAX 固定快照](https://github.com/robot-agent-exploration/rax/tree/2652c73f51514cbb84ef0fdf5a8aee2ba5b53fb7)让编码 agent 通过机器人工具探索，再整理为可以脱离 agent 执行的程序。main 是入口索引，实际代码分布在 `humanoid-arena/*`、`dexjoco/*`、`real/franka`、`real/rainbow` 等分支；不要只克隆 main 就假设已有完整运行环境。

**先读哪里：**[真实机器人适配说明](https://github.com/robot-agent-exploration/rax/blob/2652c73f51514cbb84ef0fdf5a8aee2ba5b53fb7/docs/REAL_ROBOT_ADAPTATION.md)。先列出工具输入/输出、控制模式、记录器和人工接管的责任，再看对应 embodiment 分支。其人形仿真与 Franka/RB-Y1 真机证据需要分别理解；分支存在不能证明实物人形产线运行。

**你的验收表：**分别记录探索耗时、模型调用、程序独立执行耗时、重置条件和保留场景表现。不要把“程序比探索 agent 快”理解成模型推理本身加速。8GB 电脑可先读接口与日志结构，不保证各仿真环境可运行。

## RobotUse：把模型选择与几何执行的职责拆清

[RobotUse](https://arxiv.org/abs/2610.04929v2)让 agent 选择视觉目标与姿态，后端负责几何、规划与控制；子目标上下文和持续 playbook 管理执行经验。可从[固定版本的 playbook loader](https://github.com/robotuse-team/RobotUse/blob/60bdf7b31b9a9a45dbc4809d678c008c724f465b/src/agent/playbook/loader.py)和[控制接口](https://github.com/robotuse-team/RobotUse/blob/60bdf7b31b9a9a45dbc4809d678c008c724f465b/src/backend/controller.py)追踪一次决策。

[官方项目页](https://robotuse-team.github.io/)明确：发布包含核心 harness、视觉工具与 RoboLab 接入，**不包含物理 Panda adapter**。它报告的 RoboLab 汇总与精选真机回放不是同一评测集合；不要互相替代。

**练习：**为一次抓取保留原图、目标选择、候选姿态、最终命令、返回观测和结果判据。然后找出哪个组件能发现“工具返回了，但物体没有被放好”。

## InterEvolve：修改任务程序，而非假定控制器无所不能

[InterEvolve](https://arxiv.org/abs/2610.02196v2)以分阶段 reward program 表达任务，LLM 改结构、数值优化器调常数，通过并行仿真反馈筛选候选，再利用冻结控制器执行。[项目展示](https://sirui-xu.github.io/InterEvolve/)包括真机 G1；页面也标明部分例子依赖外部动捕物体位姿，不能概括成所有实验只有机载感知。本次未核实可运行代码发布。

**工程启发：**工序描述可拆成阶段目标、完成判据与允许调节的参数。先验证控制器已有能力是否覆盖需求，再尝试组合；测试时不重训并不意味着可以省去控制器准备。

## 如何把这些放进完整机器人系统

[10-14 的综述](https://arxiv.org/abs/2610.20442)以 Model、Data、Environment、Harness 四类组织系统职责，可用于检查缺项，但不是一套部署实现。先画自己的观测 → 决策 → 命令 → 执行反馈 → 验收闭环，对每条边写字段、超时、失败与重试规则。上述实现是学习参考，均未在本仓库复现。
