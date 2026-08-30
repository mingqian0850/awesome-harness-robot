# Daily arXiv scan — 2026-08-30

## Scope

- Interval: since the 2026-08-28 run's cutoff (~2026-08-28 05:32 UTC, per the
  automation state file) through 2026-08-30 ~08:00 UTC. Missed dates covered:
  **2026-08-29** (machine off; no main run, no catch-up) and **2026-08-30**
  (today). The 08-28 run's record already fully curated the `[20260827]`
  slice; the `[20260828]` and `[20260829]` slices (the batches announced
  2026-08-29 and 2026-08-30) are **not yet exposed** by the export API —
  `submittedDate:[20260828000000 TO 20260828235959]` and the 08-29 range both
  return `opensearch:totalResults > 0` → no entries, with and without category
  filters (batch lag, per handoff known-ops note).
- The arXiv export API (`https://export.arxiv.org/api/query`, HTTPS) was
  healthy. The newest visible batch is `[20260827]`. Per-category totals
  (cs.RO 41, cs.AI 109, cs.CL 86, cs.CV 102, cs.LG 105) **exactly match** the
  raw counts recorded by the 08-28 run, so the batch content is unchanged —
  the record's "341 unique" was an under-count (its raw hits 41+109+86+102+105
  = 443 with the true cross-list count of 93 gives 350 unique; the record's
  dedup implied 102 cross-lists). Per procedure, the whole `[20260827]` batch
  (350 unique entries, all v1 with `published == updated`, no revisions) was
  **re-screened once**, and the re-screen surfaced four qualifying papers the
  08-28 pass did not name (see Included). XML saved under the workspace scratch
  dir (`.scratch/arxiv-2026-08-30/`, removed after the run).
- Previous batch recheck: `[20260826]` now returns **390 unique entries** (vs
  334 at the 08-28 run's recheck; 317 at the 08-27 run) — the slice grew
  further via late additions, so its not-previously-named subset was screened
  (294 entries; scope-relevant finds: RAMP/2608.25241, NeuronFuzz/2608.26222,
  Anchoring Bias in LLM-as-a-Judge/2608.25869, BixBench3/2608.25286 — see
  watch list). All 13 entries with `updated != published` in `[20260826]`
  carry update timestamps of 2026-08-27, i.e. before the last run's cutoff —
  **no new revisions** since the 08-28 run (LM-X v2, Zero-WAM v2, Stale
  Constraints v2, VISA v2, 4DStreamCtrl v2, ClueWeaver v2, QLoRA facts v2,
  DESCENT v2, MACGen v2, and four others). `[20260827]` contains no revisions.
- Screening covered agent harnesses, harness engineering, self-improving
  harnesses, agentic robotics, robot agents, hierarchical/agentic VLA,
  vision-language-action, robot foundation models, robot/world-action models,
  code-as-policy, skill discovery, memory, recovery, evaluation, runtime
  monitoring, and safety. Candidate claims were checked against the arXiv
  record, official project pages, and official code/model/data repositories
  (live HTTP and GitHub API checks on 2026-08-30); README, landscape, and
  source records were checked for duplicates (no name/ID collisions). All
  performance results below remain author-reported unless stated otherwise.

## Included (new curated entries)

### Zero-Shot Self-Orchestration with Ledger-Based Control

- Paper: https://arxiv.org/abs/2608.26480
- Reproducibility bundle: https://github.com/slee-persis/GVS5H (live
  2026-08-30, pushed 2026-08-27; no license asserted; not linked from the arXiv
  record — located via web search)
- Classification: General Harness Methodology (harness-effects measurement;
  manager–worker scaffold).
- Why included: a controlled same-model measurement of a manager–worker
  scaffold over a shared filesystem workspace (ledger-style notes, sample-test
  verifier, cut-off summarizer, workspace size bounds) against the same model
  answering in a single pass — no training, no per-benchmark tuning. Across
  nine models (five open-weight 9B–~2.8T, four frontier closed) on the 100
  latest hard LiveCodeBench problems the benefit is real but conditional:
  large and statistically significant for some arms (Qwen3.8-27B +23.4,
  GPT-5.6-Luna +10.6, GPT-5.6-Terra +8.0; Kimi-K3 +30.4, Minimax-M3 +11.0 with
  reasoning off; +42/+12 in a single pass at a 128k cap) and null or negative
  for others (Qwen3.6-35B −1 to −9), with transcript analysis attributing gains
  to context management (reduced truncation) and problem decomposition, and a
  cost analysis (managed GPT-5.6-Terra ≈ Fable 5 single-call at ~1/5 the price;
  Qwen-27B arm self-hostable). Direct continuation of the harness-effects
  measurement line (Same Model Different Harness). The GVS5H bundle is a real
  artifact: paper LaTeX, both scaffold versions, the benchmark harness, and
  complete transcripts/workspaces of every reported run, with all figures
  recomputed exactly.
- Boundary: digital coding agents; no robot experiment; bundle without
  license; not linked from the arXiv record.

### PILOT in the Loop: Live Self-Improvement for Long-Horizon Agents

- Paper: https://arxiv.org/abs/2608.26530
- Classification: General Harness Methodology (live self-improving
  supervisor–worker harness).
- Why included: makes self-improvement *live* instead of post-hoc: a separate
  supervisor redirects or aborts the active worker mid-run (live steering)
  while a live self-evolution mechanism distils procedures and failure modes
  revealed during execution into reusable skills and memory — capabilities
  single-agent self-correction and non-redirectable subagent delegation lack.
  Across two frozen backbones and three benchmarks PILOT ranks first in five of
  six configurations; on Terminal-Bench 2.0 it outperforms counterpart
  harnesses by up to 9.8 percentage points, and in the self-improvement
  setting gains 14.6 (GLM-5.1) / 12.4 (Kimi-K2.6) points while mean output
  tokens fall 42.9%/47.4% and successful evaluations per million output tokens
  rise 110.3%/134.0%. A concrete supervisor-worker harness design in the
  self-improving-harness line (Harness Continual Learning, JIT-Agent,
  OpsHarness).
- Boundary: digital agents; no robot experiment; the in-paper repository link
  (github.com/XiaoYang66/Pilot) returned 404 as of 2026-08-30 — no live
  artifact. Distinct from the earlier PILOT world-action-model paper
  (2608.06994) already noted in sources.

### ASIL: Replacing Screenshot-and-Click with Structured State and Semantic Actions

- Paper: https://arxiv.org/abs/2608.26991 (EMNLP 2026 Findings)
- Code: https://github.com/sharryXR/ASIL (Apache-2.0, "Official implementation
  of ASIL", pushed 2026-08-27; verified live via GitHub API)
- Classification: General Harness Methodology (agent-native software
  interface).
- Why included: a reusable interface contract for software-operating agents —
  structured JSON observations plus code-executable semantic actions realized
  through the deepest feasible access path per application, argued against
  screenshot-and-click as state-incomplete and semantically weak. Instantiated
  across 15 applications and 380 tasks (300 single-, 80 multi-application):
  above 80 strict success with closed models in fewer than five actions per
  task vs 6.6/26.6 strict success for screenshot-and-click under a 50-step
  budget; beats LibreOffice's UNO API by 28–38 strict points on matched tasks
  and matches draw.io's MCP content contract; the structured modality also
  trains (SFT 58.0→72.1 / 66.6→80.4, on-policy RL →74.4/82.2 on Qwen3.5-2B/9B).
  A real Apache-2.0 artifact release (EMNLP 2026 Findings) in the
  agent-interface/harness line (ASIL complements the harness-interface work in
  the repo's digital-agent cluster).
- Boundary: digital software agents; no robot experiment; results
  author-reported.

### AgentJudgeBench: A Multi-Difficulty Benchmark for Evaluating LLM Judges on Agentic Tool-Calling

- Paper: https://arxiv.org/abs/2608.26623
- Data: https://huggingface.co/datasets/ServiceNow-AI/AgentJudgeBench
  (Apache-2.0, updated 2026-08-26; verified live)
- Code: https://github.com/ServiceNow/SyGra (Apache-2.0; agent-judge eval tasks
  under `scratch/agent_judge_bench`)
- Classification: Evaluation/Safety (LLM-as-a-judge reliability for agentic
  tool-calling).
- Why included: the first benchmark to systematically study judge reliability
  on structured, dependency-driven agentic tool-calling workflows (workflow
  DAGs), distinct from open-ended text/preference judging — 3,808 instances
  across six DAG topologies and three difficulty tiers, five generators × six
  judges, with- and without-ground-truth conditions. Findings: judge alignment
  degrades monotonically with difficulty (1.5× faster without ground truth);
  all six judges converge to a 77–82% band on hard no-ground-truth queries
  (structural ceiling independent of scale); ground-truth exposure can reduce
  alignment (over-anchoring); CoT/temperature don't help while structured
  rubrics gain up to 6.5 pp. Real benchmark-data release (Apache-2.0) plus
  evaluation code, with practical guidelines for agentic-system evaluation.
- Boundary: digital agents; judge evaluation, not robot evaluation; results
  author-reported.

## Reviewed but not promoted (watch list)

- **RAMP / A Few Pages of Markdown** (`2608.25241`) — four-level cumulative
  maturity model for committed AI configuration artifacts (rules → named
  agents → multi-agent orchestration) across 441 repositories, re-estimating an
  agent-adoption panel within strata (no-config repos show ~2× the increase in
  cognitive complexity, +53% vs +27%). Relevant measurement of config-as-code
  (harness engineering), but hypothesis-generating and the stated RAMP release
  was not locatable as of 2026-08-30 (author page carries no code link); watch
  for the artifact. From the `[20260826]` growth subset.
- **Risks and Controls for Multi-Agent Systems** (`2608.26626`) — analytical
  framework for agent–agent risk across organizational boundaries with three
  deployment tiers (singular/federated governance); governance report, no
  implementation; watch.
- **AI Control Scientist** (`2608.26780`) — LLM-driven agent for automated
  control design (task-modeling/controller-design/parameter-tuning agents);
  conventional-control domain; watch for a reusable agent harness contract.
- **Rapid On-Robot Learning for Dynamic Manipulation Skills / Robot Juggling**
  (`2608.26800`) — regularized memory-based online learning with a retained
  global prior and safety constraints on a bimanual robot; learning-method
  paper, not a harness; watch.
- **Calibrated Enough to Know, Not Calibrated to Act** (`2608.27167`) —
  fabricated evidence lifts agent commitment to unknowable questions (6.5% →
  54.0% across 12 frontier models; invented panels still 36.8% vs 37.6%
  genuine); a measurement of evidence-packaging effects on agent action;
  no artifacts; watch.
- **Arrive and Survive** (`2608.26571`) — mass-weighted corrections for
  contrastive RL's failure-termination bias (one-bit failure signals); robot RL
  method with safety implications; no artifacts; watch.
- **NeuronFuzz** (`2608.26222`) — safety-neuron-guided white-box fuzzing for
  LLM safety evaluation (prefill-time feedback); LLM-safety testing, not an
  agent harness; watch. From the `[20260826]` growth subset.
- **Beyond F1: Evaluating Coverage and Failure Recovery in AI Model Security
  Scanners** (`2608.27424`) — artifact-backed evaluation of ModelScan/
  ModelAudit/Fickling on 170 pickle/PyTorch artifacts; ML-supply-chain
  scanning evaluation; watch.
- **Anchoring Bias in LLM-as-a-Judge Systems** (`2608.25869`) — prior scores
  as metadata anchor judgments across 192,000 evaluations; judge-independence
  measurement; watch. From the `[20260826]` growth subset.
- **Multi-Expert Conformal Risk Control for Pairwise LLM Judging**
  (`2608.26529`) — MC3 aggregation for judge risk control; evaluation
  methodology; watch.
- **VERA-RL / Not Just Reason, Not Just Scan** (`2608.26596`) — RL formulation
  for scientific error verification over papers (VERA-13K); scientific-agent
  line; watch.
- **BixBench3** (`2608.25286`) — benchmark for AI agents on research-study-
  scale computational biology tasks; scientific-agent line; watch. From the
  `[20260826]` growth subset.
- **Beyond Shallow-Water Photorealism** (`2608.26888`) — physics- and
  sensor-grounded Stonefish extension for deep-sea robotics; simulation
  fidelity, no agent/harness contract; watch.
- Domain exclusions in the screened ranges (per policy): driving (CAV platoon
  `2608.26860`, lane-change `2608.26669`, SecureDrive-FL `2608.27108`),
  conventional robot control/locomotion (Poppy humanoid `2608.26505`, SOLO
  humanoid `2608.26583`, rehab-robot torque control `2608.26739`, crowd
  navigation `2608.27158`, soft-actuator control `2608.27186`, tensegrity
  `2608.27221`, reconfigurable gripper `2608.26883`, LiDAR calibration
  `2608.26789`, MAPD `2608.26939`/`2608.26759`, disassembly fixtures
  `2608.27151`), perception-only (motion forecasting `2608.27039`,
  pose estimation `2608.26859`, glass detection `2608.26752`, CGS-SLAM
  `2608.26868`), medical/CV/CL/LG papers without a reusable agent/VLA harness,
  recovery, safety, or evaluation contract, general-LLM agent papers in
  finance/recommender/serving/creative domains (Magpie `2608.27168`, SWE-Prime
  `2608.27449`, MCR-Bench `2608.27442`, TTPO `2608.27448`, RuleWeaver
  `2608.26832`, DuMateBench `2608.26546`, MemToC, LivingRAG, AesCanvas
  `2608.26713`, ClueWeaver `2608.25531`), LLM-security without agent-harness
  scope (Basin-Aware Jailbreaks `2608.26506`, TempJail `2608.26971`), RLVR/
  post-training methods (Boosting LLM Exploration `2608.27420`, Consolidating
  RLVR `2608.27409`), scientific-domain agents without reusable contracts
  (VERA-RL watch, BixBench3 watch, Gemini real-world science `2608.26701`),
  and vendor/industry case studies (Sophistication in GenAI Use
  `2608.27364`).

## Watch-list status (rechecks)

- No new revisions or artifact releases detected for previously listed watch
  items: `[20260827]` contains no `updated != published` entries, and all
  `[20260826]` revisions predate the 08-28 run. Watch items with pending
  artifact promises (StageWAM, ReflexVLA, DreamX-Phi, UniTexture, PRISM,
  GigaBrain-0.7/WBC, ForceU-VLA, LIBERO-VIFO, Agent Lightning, VLCP, Hydra-0,
  BATON, Q-Planning, AutoSaddler, JIT-Agent, Task-CoEvolve code, UCAG-P,
  TrapVLA code/benchmarks, TemporalFlow-VLA, PredVLA, FlashVLA, WikiSkill,
  RedEvoAgent, SKILL.state, Agent Mesh, WALL-SS, R2M-Bench, INTENT-AS-A-TOOL,
  BTS-AgentBench, TraceBench, GraphMemix, UrbanGround, Stale Constraints v2,
  LM-X v2, Zero-WAM v2) — unchanged.
- The 08-28 run's "341 unique" figure for `[20260827]` is superseded by this
  run's verified 350 (identical per-category totals); noted here so future
  runs do not treat 341→350 as growth.

## Operational notes

- The arXiv API was healthy for the entire run (no 429/500). `submittedDate`
  ranges for 2026-08-28/29 return 0 entries — the environment's newest exposed
  data remains `[20260827]`; the next run should re-check `[20260828]` and
  `[20260829]` (and growth in `[20260827]`/`[20260826]`).
- Artifact checks on 2026-08-30 (live HTTP/GitHub API): GVS5H bundle (live,
  pushed 08-27, no license), ASIL repo (Apache-2.0, pushed 08-27, "Official
  implementation", EMNLP 2026 Findings), AgentJudgeBench HF dataset
  (Apache-2.0, updated 08-26) and ServiceNow/SyGra (Apache-2.0),
  XiaoYang66/Pilot (404), RAMP (no code link on the authors' publication
  page). PILOT in the Loop, LoopHarness-era entries, and the rest: no new
  artifacts located.
- Scratch XML kept under `.scratch/arxiv-2026-08-30/` during the run and
  removed afterward (not committed).
- Working tree before the run: `docs/reference-architecture.md` modified,
  `docs/ring-harness.png` and `handoff.md` untracked — preserved untouched.
- Consistency: README header badge and Current Landscape "Last verified" both
  updated to 2026-08-30 (both were 2026-08-28 after the previous run).
