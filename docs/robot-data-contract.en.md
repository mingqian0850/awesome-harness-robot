# From demonstrations to execution: a robot data contract

[Home](../README.md) · [中文](robot-data-contract.zh-CN.md)

Sources reviewed: 2026-10-20; compiled: 2026-10-21. Engineering recommendations are not reproduced experiments.


## Keep three records distinct

This is a proposed interface design, not a disclosed factory implementation.

| Record | Key contents | Distinction to preserve |
| --- | --- | --- |
| Demonstration/policy trajectory | Images, state, action semantics, timestamps, calibration, task and outcome | Human references, commands and execution feedback differ |
| Harness execution events | Task/episode IDs, tool requests, acknowledgments, timeouts, cancellation and recovery | Request delivery does not establish physical completion |
| Process feedback | Part ID, equipment status, inspection, rework and human takeover | Completed handling does not establish product quality |

Join records with correlation IDs and explicit clock conventions; sensors need not share a frequency. Record acquisition time and receipt time separately, along with interpolation, resampling and gaps.

## Recovery after timeout

Physical actions usually cannot be blindly retried like side-effect-free queries. When placement times out, reconcile robot, part and fixture state before continuation, recovery or human intervention. Software deduplication does not establish exactly-once physical execution.

Distinguish model proposals, issued commands, controller feedback and quality checks. When connecting policy services to high-rate controllers, define action expiry, cancellation and takeover at the interface; language reasoning is not a substitute for feedback records.

## References and scope

[MCAP](https://mcap.dev/) provides a timestamped multichannel log container; the application still defines field semantics. [Longcheer](https://us.longcheer.com/en/col76/260.html) describes robot/test-equipment communication and result sorting without publishing a complete reusable production interface.

See [five capture routes](https://github.com/mingqian0850/awesome_vla_deployment/blob/main/docs/02-data-sources.md). Public learning components are not confirmed vendor deployment stacks.

## Exercise without training

Write a paper event trace for “grasp succeeded, placement acknowledgment lost,” including time, correlation IDs, state sources and unknowns. Design three recovery branches and identify a new observation that distinguishes them. Do not execute real robot actions.

Continue: [reference architecture](reference-architecture.md).
