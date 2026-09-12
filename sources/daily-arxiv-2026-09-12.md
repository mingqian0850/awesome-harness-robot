# Daily arXiv scan — 2026-09-12

## Scope

- Interval: since the 2026-09-11 run's cutoff, i.e. everything indexed after
  **2026-09-10T17:59:55Z** (that run's newest visible stamp; max ID
  `2609.11929`). The 09-11 record explicitly left the 09-11 block to this run
  ("this run started ~07:10 UTC 09-11, so it is next run's scope").
- **The arXiv index did not advance at all during this interval.** This run
  (started ~08:10 UTC on Saturday 2026-09-12) re-queried the export API and
  found the newest visible entry unchanged: `2609.11929v1` overall /
  `2609.11920v1` in cs.RO, newest stamp **2026-09-10T17:59:55Z**, exactly the
  previous run's boundary. Specifically:
  - `cat:<C> AND submittedDate:[20260911000000 TO 20260912235959]` →
    `opensearch:totalResults` **0** for cs.RO, cs.AI, cs.CL, cs.CV, cs.LG.
  - `cat:<C> AND lastUpdatedDate:[20260910180000 TO 20260912235959]` →
    `opensearch:totalResults` **0** for all five categories.
  - A `lastUpdatedDate`-descending probe over cs.AI returns entries whose
    `updated` stamp equals their `published` stamp (newest
    `2026-09-10T17:58:14Z`) — i.e. **no replacement versions of older papers
    are exposed either**.
- Interpretation (consistent with the 08-30 and 09-07 records): 2026-09-11 was
  a **Friday**, and the Friday announcement block lands with the next weekday
  announcement. The 09-11 and 09-12 blocks therefore remain next run's scope.
  This is the normal weekend lag, not an API failure (every query returned a
  clean feed, not the 500/empty-body failure mode recorded on 09-07).
- **Dates in scope:** none with visible content. The period
  2026-09-10T17:59:55Z → 2026-09-12T08:15Z contains **zero new submissions and
  zero revisions** in the five target categories.

### Unchanged-batch re-screen (required, and performed)

Because the newest visible batch is identical to the previous run's (same max
ID, same newest stamp), the whole batch was re-screened once from raw XML
rather than trusted, per the standing procedure.

- **Batch defined by ID, not by stamp:** the 401 entries with base ID
  ≥ `2609.10627` — the previous run's new-content window — published
  **2026-09-09T02:05:12Z → 2026-09-10T17:59:55Z**. Re-parsed from the
  per-category heads (5 × 1,000 records cached at run start in
  `.scratch/arxiv-2026-09-12/*.sub.xml`, 4,069 unique bases), independent of
  the previous run's parsed notes.
- **Coverage of the 401 by primary category:** cs.AI 76, cs.CV 74, cs.LG 71,
  cs.CL 59, cs.RO 38, plus 83 whose primary category lies outside the five
  (cs.CR 15, stat.ML 9, eess.AS 5, cs.SD 5, quant-ph 4, cs.SE 4, cs.DC 3,
  cs.HC 3, cs.MA 3, cs.CY 3, and a tail). 44 entries carry cs.RO.
- **286 of the 401 are not mentioned anywhere in the 09-11 record** — neither
  as inclusions, watch-list items, nor in its enumerated exclusion lists. All
  286 were re-examined. Of the 6 unmentioned cs.RO-carrying entries, each was
  read individually; all are control, perception, or driving work (listed
  under exclusions).
- **Concept sweeps over all 401** for `harness|scaffold`,
  agent-loop/runtime/orchestration, verification-gate/rollback/guardrail,
  recovery/retry/resume, robot-agent/embodied-agent, VLA, skill/tool library,
  and memory; plus a title sweep for `agent|embodied|robot|tool|skill|memory`.
  Result: **exactly one** unmentioned entry is a genuine harness-methodology
  contribution — `2609.10824` — and it is this run's single inclusion. The
  first pass missed it because the word *harness* appears only in its
  related-work framing ("task-agnostic adaptation of agent harnesses"), never
  in its title or abstract, so a keyword screen keyed on harness vocabulary
  would not surface it.
- The remaining unmentioned entries are digital-agent, benchmark, medical,
  physics, audio, vision, or systems papers with no harness angle and no
  artifacts; they are enumerated by group under exclusions so a later agent
  can distinguish deliberate rejection from an oversight.
- **Revisions:** none to re-check. No record in the five categories carries an
  `updated` stamp inside the interval, so no README entry and no watch-list
  item received a replacement version (contrast the 09-11 run, which examined
  469 replacement versions).
- All performance results below are author-reported unless stated otherwise.

## Included (README updates)

One entry: a general harness-methodology paper recovered by the re-screen.

### Studying Without a Syllabus: Task-Agnostic Environment Preprocessing

- Paper: https://arxiv.org/abs/2609.10824 (v1; cs.AI, cs.CL, cs.LG; published
  2026-09-09T20:52:55Z; Scale AI / University of Maryland, College Park — Vinay
  Samuel, Varun Ursekar, Vijay S. Kalmath, Apaar Shanker, Veronica Chatrath,
  Yuan Xue). It carries no comment, journal reference, or project link, and
  cites Lilian Weng's *Harness engineering for self-improvement* and
  Meta-Harness, VeRO, ACE, AWM, Dynamic Cheatsheet, PREPING, and Corpus2Skill
  as its immediate context.
- Artifacts: **none located — treated literally as not open.** The paper names
  `github.com/scaleapi/meta-study`; that repository returns **404** on both the
  GitHub REST API and a direct fetch, a repository search within the `scaleapi`
  organization for `meta-study` returns 0 results, and a global GitHub
  repository search for the paper's topic returns 0 results (all verified
  2026-09-12). The body text refers to "the implementation and released run
  records", so a release appears intended, but nothing is reachable today. The
  CC BY 4.0 notice in the HTML is the arXiv licence, not an artifact licence.
  This is recorded as "not open", not as "released" — promote if the
  repository becomes public.
- Classification: General Harness Methodology (pre-task harness adaptation /
  environment preparation).
- Why included: it isolates the harness layer that every other
  harness-adaptation paper in this repository presupposes — what gets built
  *before* the task distribution is known. Most automated adaptation methods
  (prompt optimizers, memory systems, harness-rewriters) need task examples,
  trajectories, or evaluation feedback to decide what to change, and existing
  task-agnostic approaches commit in advance to one preparation strategy for
  one environment type. Here a **studying system** explores an unfamiliar
  environment under a budget with neither signal, and emits **artifacts** —
  indices, scripts, procedural guidance — for a **frozen solver**, which makes
  the study phase the only variable under test and is the same
  "hold the model fixed, vary the harness" design this repository prefers. It
  is also a rare harness paper that reports its own negative results alongside
  the positive cost case (below), and it survives a literal reading of its
  artifact claim.
- Evidence: six heterogeneous benchmarks (BCP-G, OfficeQA, Harvey LAB,
  DABStep, Apex Agents, AppWorld) under Avg@3 and Best@3. A meta-agent variant
  achieves the highest Avg@3 on **five of six** benchmarks, and the
  archive-equipped meta-agent ranks first or second on every benchmark under
  both metrics; the fixed synthetic-practice baseline (PREPING) beats No-Study
  on all six but never ranks first; the fixed corpus-processing baseline
  (Corpus2Skill) is strongest on the largest corpus (BCP-G) yet falls below
  No-Study on OfficeQA, Harvey LAB, and DABStep — fixed preparation strategies
  trade breadth for corpus-specific strength. Reported negative results:
  study budgets of $1–$50 do **not** reliably improve downstream reward (curves
  are flat or non-monotonic with overlapping intervals; Harvey LAB is the clear
  exception, with the unaided meta-agent rising 0.271→0.348 and the
  archive-equipped one 0.249→0.332), and broader exploration does not reliably
  predict artifact quality (on OfficeQA the method that processes all 697 files
  trails methods that inspect fewer; on Harvey LAB, discovering the single
  benchmark-specific tool `labread` separates 0.35–0.38 from 0.22–0.31).
  Positive cost case: studied artifacts reduce the test-time sampling needed to
  reach a given score — to exceed the strongest studied Best@1 score, No-Study
  needs 8 rollouts on BCP-G, 2 on OfficeQA, 3 on Harvey LAB, 5 on DABStep, 2 on
  Apex Agents, and 4 on AppWorld, which costs 1.6–5.5× as much task-time
  inference as the corresponding studied rollout — but that comparison excludes
  the upfront study cost, so preparation pays only when artifacts are reused
  across enough downstream tasks. The workflow archive helps unevenly (mean
  +0.046 where it helps across four benchmarks, mean −0.021 where it hurts
  across two; largest gain +0.129 on DABStep). Digital agents only, **no robot
  experiment**; no independent reproduction; results author-reported.

## Watch list (re-screen findings)

- **Finishing the Task Is Not Enough** (`2609.10724`, cs.AI/cs.HC/cs.MA,
  09-09) — proposes *operational resilience* (how an agent recovers from
  blocked work while preserving progress and communicating its limits) and
  *considerate participation* (how adaptation accounts for affected people and
  role boundaries) as complementary evaluation axes under accumulating
  challenge, studied over 120 simulated healthcare trajectories, two models,
  and twelve stakeholder-derived tasks across light/medium/heavy challenge,
  comparing textual action plans, prompted internal assessments, and structured
  workload/affect reports. Directly adjacent to this repository's recovery line
  (AgentRewind, REVISE) and its "who validates the verifier" line, but the
  evidence is simulated healthcare workflows with human-factors coding, the
  paper's "artifact plans" are future work in an appendix, and no artifact was
  located — **watch** as a recovery-evaluation protocol, not yet an
  implementation.
- **When Synthetic Data Hurts** (`2609.10750`, cs.IR/cs.AI/cs.LG, EMNLP 2026
  Industry Track, 09-09) — a production skill router over **34,396 skills**
  whose synthetic-data fine-tuning improves in-distribution retrieval while
  causing catastrophic forgetting on real and out-of-distribution skills, with
  continual-learning mitigations (embedding anchors, LwF, EWC, L2-init)
  recovering OOD retrieval and adding +13.98% on synthetic in-distribution
  skills for a 0.6B Qwen retriever/reranker; ~1K trials over 109 tasks from
  SkillsBench and Terminal-Bench 2 inside the Harbor environment. This is
  harness-state forgetting for a *skill library*, complementing Harness
  Continual Learning's harness-level forgetting and EVOHARNESSBENCH's
  harness-induced forgetting, and the companion repository
  `manulife-ai/emnlp2026` is live (created 2026-08-25, pushed 2026-09-04;
  `build_training_pairs.py`, `preprocess_trackb.py`, `load_trackb.py`, `data/`,
  `requirements.txt`) but **asserts no licence** and ships data-prep scripts
  rather than the router. The contribution is a retrieval fine-tuning recipe
  and empirical study rather than a runtime/interface contract, so it stays
  **watch** — promote if the router or its index is released.
- **SearchAtlas** (`2609.10901`, cs.CL, EMNLP 2026 Findings, 09-09) — converts
  raw agentic-search trajectories into structured evidence graphs whose edges
  record how evidence propagates from the query that retrieved it to the final
  answer (mean edge F1 86.0% against human-annotated graphs), then uses them to
  expose process failures — fragmented answer support, question constraints
  that never reach the answer, unverified parametric knowledge — that track
  incorrect answers more strongly than an LLM judge. Process-level
  observability for search agents; digital only, no artifacts located;
  watch-lite.
- **AgentActionBench / NLPCC 2026 Shared Task 11** (`2609.11117`, cs.CL,
  09-10) — a process-oriented benchmark for agent-based experiment
  reproduction across ML and AI4Science, with an MCP-based Action Recorder
  capturing agent behaviour throughout reproduction and paper-specific
  evaluation of the resulting traces. Evaluation infrastructure with a trace
  recorder, but it is a shared-task overview running under registration and no
  artifact was located; watch-lite for the Benchmarks section.
- **Standing watch list unchanged.** No item on it received a scope-relevant
  revision in this interval (the index is frozen), so every entry below keeps
  its previous verdict: StageWAM, ReflexVLA/ReflexBench, DreamX-Phi,
  UniTexture, PRISM, GigaBrain-0.7/WBC, ForceU-VLA, LIBERO-VIFO, Agent
  Lightning, VLCP, Hydra-0, BATON, Q-Planning, AutoSaddler, JIT-Agent, UCAG-P,
  TemporalFlow-VLA, PredVLA, FlashVLA, WikiSkill, RedEvoAgent, SKILL.state,
  Agent Mesh, WALL-SS, R2M-Bench, INTENT-AS-A-TOOL, BTS-AgentBench, TraceBench,
  GraphMemix, UrbanGround, LM-X, Zero-WAM, VLAct, Code as Worlds, Aero Hand
  Open, CAITLYN, Dogwood, LongGuard, WebWorld, ASPIRE, S3Gym, StudyBench,
  WorldReward, Principia, Statebench, Puffin-World, FailureSpot, FailSAE,
  Spectral-Target JEPA, Programmable World Model, DUET-DINO, GALATEA, RoboDrop,
  AXON, CT-SAFR, ViBe, InstantMimic, Semigroup-JEPA, Seven Sources, TRACE,
  TANGO, AgentAudit, IBIB, KVShareArena, MetroLLM-Bench, VidHalLoc, RD-Forget,
  Procedural Memory Under Change, Proof-Carrying Cognition, Belief-State
  Engine, A-JIT, HuRo (`2609.10706` — repository still 404), ORCH
  (`2609.11737` — no artifacts), MaP-WAM (`2609.11561` — README-only), UniMPA
  (`2609.11875` — README+assets), SEED-UMI (`2609.11753` — project site only),
  SwarmNxt (`2609.11382` — no licence), BenchShield (`2609.11028`), When
  Validation Stops Learning (`2609.10873`), ActSafeGuard (`2609.11697`),
  DriftNet (`2609.10892` — AgentDrift corpus public, detector not), Compact
  Visuotactile World Models (`2609.09597`), Proxy Policy Steering
  (`2609.09148`), RevalExo (`2609.08090`), and the 09-09 digital-agent
  eval/safety set.

## Exclusions in the re-screened batch (unmentioned entries re-examined)

- **cs.RO-carrying entries that the 09-11 record never mentioned** (all read
  individually; none introduces a reusable harness, recovery, safety, or
  evaluation contract): `2609.10726` (Behavioral Valuation for Hazardous
  Robotic Exploration — a risk-augmented *planning objective* layered on a
  fixed belief update, sensor model, risk model, and informative planner; ISRR
  2026; no artifacts), `2609.10756` (GRADE radar depth estimation), `2609.11357`
  (wheeled-humanoid motion retargeting), `2609.11698` (tail-sitter UAV
  trajectory generation and tracking), `2609.11717` (MC-DeTra driving detection
  and forecasting), `2609.10986` (pragmatic information theory, cs.IT).
- **Digital-agent, systems, and infrastructure papers with no harness angle
  and no artifacts** among the 286 previously unmentioned entries, including
  `2609.11133` (datacenter power control for disaggregated LLM serving),
  `2609.10790` (CXL shared memory for LLM serving), `2609.11744` (py-kvcache
  characterisation), `2609.10632` (Numbat ML stack in Zig), `2609.10762`
  (static-pass dynamic-fail gap in generated Python), `2609.11799` (SpecGuard
  inference-time backdoor detection), `2609.11152` (terms.txt agentic-web
  consent protocol — web standardization, not an agent runtime),
  `2609.11910` (bounded claims for responsible AI), `2609.11489` (convention
  gap in cooperative-AI evaluation), `2609.11085` (autoformalization reward
  models), `2609.11699` (negative self-distillation), `2609.10817`
  (autopoietic game theory), `2609.11431`, `2609.11319`,
  `2609.11261`, `2609.11244`, `2609.11231`, `2609.11209`, `2609.11170`,
  `2609.11163`, `2609.11127`, `2609.11126`, `2609.11109`,
  `2609.11058`, `2609.10943`, `2609.10923`, `2609.10866`,
  `2609.10826`, `2609.10739`, `2609.10728`,
  `2609.10712`, `2609.10629`, `2609.11899`,
  `2609.11752`, `2609.11450`, `2609.11360`, `2609.11321`,
  `2609.11904`.
- **Benchmarks and datasets outside the robot/harness scope** (remaining
  unmentioned entries): `2609.11894`, `2609.11877`, `2609.11838`, `2609.11498`,
  `2609.11144`, `2609.11580`, `2609.10815`.
- **Medical, biological, physics, audio/speech, graphics, and general
  vision/LM papers**: the remainder of the 286 unmentioned entries — no
  harness, recovery, safety, or evaluation contract, and no artifact relevant
  to this repository.
- **Driving-domain, perception-only, and conventional control papers**: out of
  scope under the standing policy (see the cs.RO list above).
- **Withdrawn/withdrawal risk:** none. No entry in the batch carries a
  withdrawal marker (the 08-21 HODAgent withdrawal remains the only one on
  record).

## Operational notes

- The arXiv API was called over HTTPS
  (`https://export.arxiv.org/api/query`); plain HTTP returns empty responses.
  Every query returned a clean feed this run — the 09-07 failure mode
  (`submittedDate`/`lastUpdatedDate` queries returning HTTP 500) did not recur.
- The per-category heads cached at run start in
  `.scratch/arxiv-2026-09-12/` (5 × 1,000 records, `*.sub.xml`) were the
  re-screen substrate; parsed entries, screening scripts, and fetched HTML
  live in the same directory, which is **untracked and deliberately not
  staged**.
- `/tmp` is **not persistent between shell invocations** on this host — each
  `bash` call receives a fresh ephemeral `/tmp`, so all XML, parsed JSON, and
  fetched pages were staged under the workspace scratch directory instead.
- Artifact verification used live HTTP checks on `arxiv.org/abs` and
  `arxiv.org/html` plus the GitHub REST API (repository lookup, organization
  listing, and search). The GitHub API returned normal results throughout (no
  403 rate limiting); `git ls-remote` was used to confirm the remote ref.
- `git push` over SSH failed with the known
  `/etc/ssh/ssh_config.d/… owned by nobody` OpenSSH error and was retried
  successfully with `GIT_SSH_COMMAND='ssh -F /dev/null'`.
- The working branch was `main` throughout; the run committed task-owned files
  only (`README.md` and this record) and preserved the unrelated user changes
  (`docs/reference-architecture.md` modified; `docs/ring-harness.png` and
  `handoff.md` untracked).
