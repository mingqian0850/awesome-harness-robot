# Daily arXiv scan — 2026-09-11

## Scope

- Interval: since the 2026-09-10 run's cutoff. That run's newest visible stamp
  was **2026-09-09T17:59:32Z** (max ID `2609.10540`), and it recorded that the
  09-10 block had not yet landed ("the 09-10 block has not landed yet … so it is
  next run's scope"). The 09-10 block **did** land: the API index now runs to ID
  `2609.11929` with newest stamp **2026-09-10T17:59:55Z**, the normal end-of-day
  boundary. The 09-11 block has not landed yet (this run started ~07:10 UTC
  09-11), so it is next run's scope.
- Dates in scope: 2026-09-09 (the post-17:59:32Z tail) and 2026-09-10, plus
  late-arriving members of the 09-09 block that no prior run could see.
- **New content screened: 401 entries** — every entry with ID above the previous
  run's max visible ID (`2609.10540`), diffed **by ID** rather than by published
  stamp, per the late-arrival methodology the 09-08/09-09/09-10 records
  established. Of these, **323 carry published stamps on 09-10**
  (`10915`–`11929`, 00:00:32Z–17:59:55Z) and **78 are late-arriving members of
  the 09-09 block** (`10627`–`10914`, published `09-09T02:05:12Z`–
  `09-09T23:59:55Z`) whose IDs sit above the previous run's max, so a
  published-stamp filter alone would have dropped them. The remainder of the
  window query (311 entries with ID ≤ `2609.10540`, all published 09-09) was
  screened by the 09-10 run and is out of scope here.
- Coverage of the 401 new entries by primary category: cs.AI 76, cs.CV 74,
  cs.LG 71, cs.CL 59, cs.RO 38, with 83 whose primary category lies outside the
  five (cs.CR 15, stat.ML 9, eess.AS 5, cs.SD 5, quant-ph 4, cs.SE 4, cs.DC 3,
  cs.HC 3, cs.MA 3, cs.CY 3, and a long tail). Counting all entries that *carry*
  each target category (including cross-lists): cs.AI 167, cs.LG 158, cs.CL 91,
  cs.CV 87, cs.RO 44. All 401 titles were scanned; keyword-hit abstracts
  (~190) were read; every cs.RO-carrying entry was individually accounted for.
  Candidates were checked against the arXiv record, arXiv HTML full text,
  official project pages, and official code/model/data repositories with live
  HTTP / GitHub API / Hugging Face API checks on 2026-09-11.
- **Revisions.** The five categories were probed with a broad
  last-updated-descending window (1,500 records) so that replacement versions of
  papers submitted *before* this interval would surface (a submittedDate-range
  query cannot see them). **1,167 records carry an `updated` stamp in the
  window; 469 of them are replacement versions (v2+) of papers published before
  the cutoff**, of which **110 have scope-relevant titles**. All three
  README-listed papers among the revisions were checked: *Beyond Prompts*
  `2609.05736` v2 (09-09) and *Bit-Flip Attacks on VLA* `2608.15475` v3 (09-09)
  were already assessed as metadata-identical by the 09-10 run, and
  ***Motus2*** `2608.30237` v2 (09-10T16:10) is **cosmetic** — a word-level diff
  of v1/v2 HTML shows only affiliation markers and a corrected corresponding
  email, with the project page and all claims present in v1. **No README entry
  needs a revision edit.** The 23 watch-list papers with scope-relevant
  revisions are rechecked below.
- **Unchanged-batch re-screen requirement.** The newest visible batch *is*
  changed relative to the previous run (IDs advanced `10540`→`11929`, newest
  stamp `09-09T17:59:32Z`→`09-10T17:59:55Z`), so the full screening above is the
  required re-screen; no additional unchanged-batch pass was needed. A re-query
  of the 09-10 range after screening confirmed the batch did not move during
  this run (max ID `2609.11920` in cs.RO, newest stamp `09-10T17:58:03Z`).
- All performance results below are author-reported unless stated otherwise.

## Included (README updates)

Six entries: two agentic robot/VLA harnesses, two general harness
design/self-improvement systems, one runtime observability primitive, and one
evaluation-infrastructure promotion from the watch list.

### Harness Robotic OS (HROS)

- Paper: https://arxiv.org/abs/2609.11225 (cs.RO only; Vbot / residential
  inspection deployment; first exposure to the curation pipeline — no prior
  record, no duplicate in README/docs/sources)
- Artifacts: **none located.** The arXiv record carries no comment or link; the
  full HTML (verified 200 on 2026-09-11) references only third-party components
  (`openclaw/openclaw`, planners, VLM). No project page or code repository was
  found on the record, in the paper, or by GitHub search.
- Classification: Agentic Robot/VLA Harness (primary); General Harness
  Methodology (the governed self-evolution contract).
- Why included: it is a full embodied-agent *runtime* rather than another
  VLA — four planes (robot runtime, embodied autonomy skills, cognitive agent
  runtime, interaction/operations) behind one context bus, hierarchical
  working/episodic/semantic memory, grounded streaming ASR/TTS, per-capability
  status reporting (timestamp, execution state, confidence/failure code, output
  reference) that keeps the cognitive loop out of real-time control, and — the
  distinctive claim — a **safety-gated self-evolution loop**: traces produce
  candidate memory/prompt/tool/task-graph updates that must pass offline
  evaluation and a versioned gate before promotion, with rollback, so nothing
  changes online. That is exactly the "what does the harness own" boundary this
  repository maps, stated as an admission contract rather than a prompt recipe.
- Evidence: real-world deployment on a Vbot quadruped with Fast-LIO2,
  Hobot-Stereo, PCT-Planner, EGO-Planner, and OpenClaw-orchestrated Qwen3-VL in a
  residential property: 100% waypoint reachability, <10 cm outdoor localization
  error, <200 ms local obstacle-response latency, 85–95% hazard-detection rates,
  99% alarm-delivery/structured-report success. Author-reported, no independent
  verification, no simulator-to-real comparison.

### Ecdysis: Efficient and Effective Training of Runtime Harnesses for LLM Agents

- Paper: https://arxiv.org/abs/2609.11677 (cs.SE, cs.AI; BIT; first exposure)
- Code: https://github.com/cuiyu-ai/Ecdysis (verified live 2026-09-11 via GitHub
  API: `src/`, `scripts/`, `tests/`, `pyproject.toml`, README; 188 KB; created
  2026-07-01, last push 2026-09-06; **no license file**). The README calls it an
  "ongoing research release" that excludes local experiment configuration files,
  credentials, raw benchmark data, generated traces, and private run artifacts.
- Classification: General Harness Methodology (harness evolution / failure
  diagnosis).
- Why included: it names and attacks the diagnosis bottleneck that most
  harness-evolution work steps over — an observed failure can reflect
  model-specific accommodation or a systematic harness deficiency, and
  per-failure patching both overfits and wastes executions. Its batch-level
  cross-instance failure aggregation plus Failure-Driven Collaborative
  Refinement (analyst/critic/engineer/moderator) is a concrete, reusable
  protocol for deciding *what* to change in a harness, and it reports
  cross-model transfer of the evolved harness without re-evolution.
- Evidence: five task models (Qwen3-8B/14B/32B, MiniMax-M2.7, and one more) over
  τ²-Bench Airline/Retail and AgentBench. Baseline context: no harness 29.72%,
  fixed human harness 50.28%, serial self-evolution 43.33%. The authors report
  Ecdysis (w/ FDCR) at 59.33% average accuracy, +18.56% relative over serial
  self-evolution in the averaged 5-model × 3-dataset setting, up to 1.84× faster
  end-to-end harness training (τ²-Airline: 4,403 s against ~8,100 s for serial
  self-evolution; API cost $2.609 against $6.382), Pass^3 +55.2% relative to SE,
  and 10.5–12.2% lower final-evaluation token use. Digital agents only; no robot
  experiment; author-reported.

### 2AM: Grounding Agent-Side Memory as Guidance for Steerable Action Models

- Paper: https://arxiv.org/abs/2609.11308 (cs.RO, cs.AI; first exposure)
- Artifacts: **none located** — no comment, no link on the record, no artifact
  statement in the HTML (verified 2026-09-11).
- Classification: Agentic Robot/VLA Harness (memory ownership and steering
  interface).
- Why included: it is a clean interface experiment rather than another memory
  module. The Agent is the *sole* holder of task memory; a single RGB-only,
  episodically stateless Action Model is the *sole* executor of motion; the
  Agent compiles history into subtask language plus optional 2D grasp/place/move
  hints that bind intention at different time scales, and the VLA is trained
  with structured hint labels under condition dropout, spatial noise, and
  temporal jitter to tolerate imperfect Agent output. Because depth, online
  geometry, planner-based object motion, object IDs/poses, oracle subgoals,
  rewards, and stage flags are all removed, the reported gain cannot be
  attributed to richer observations or alternative motor tools — the harness
  variable under test is interface bandwidth.
- Evidence: ten LIBERO-Mem tasks (a **prior** benchmark, reference [8], not
  introduced by this paper). The authors report 76.3% average completion versus
  14.8% for the strongest reported baseline, 63.0% relaxed and 11.8% strict
  success. Simulation-only; author-reported.

### COBRA-Skills: Contextual Bandit-Guided Evolution for Agent Skill Optimization

- Paper: https://arxiv.org/abs/2609.11682 (cs.AI; first exposure)
- Code: https://github.com/Jerry-LuP/COBRA-Skills (Apache-2.0, verified live
  2026-09-11: `cobras/`, `cobras_core/`, `configs/`, `data/`, `task_envs/`,
  `scripts/`, `tests/`, `pyproject.toml`, LICENSE + NOTICE; 25 MB; last push
  2026-09-09)
- Classification: General Harness Methodology (skill discovery/optimization).
- Why included: it makes the *evaluation budget* the explicit resource in skill
  optimization — a contextual bandit spends each next evaluation on a promising
  or informative skill candidate while evidence-grounded evolution refines the
  population — and it explicitly tests robustness to changes in the surrounding
  agent harness, which is the property that makes a skill artifact reusable
  rather than harness-specific.
- Evidence: six heterogeneous agent benchmarks, three target models; the authors
  report the strongest average performance among compared methods, 55–58% lower
  optimization cost than SkillOpt, and only 50 unique optimization examples per
  benchmark, with effective self-generation when the target model writes the
  skills. Digital agents only; no robot experiment; author-reported. The release
  is real code + task environments, not weights.

### FARM: Reading Failure Signals from the Internal Predictive States of a Frozen Robotic World Model

- Paper: https://arxiv.org/abs/2609.11445 (cs.RO; CASIA; first exposure)
- Code: https://github.com/HaoranPei-casia/FARM (MIT, verified live 2026-09-11:
  `src/`, `configs/`, `data_indices/`, `examples/`, `tests/`, `pyproject.toml`,
  README, `DATA.md`; 55 KB; created 2026-09-10, last push 2026-09-10T08:19Z).
  The README is explicit that this is a "minimal public implementation": it
  excludes private robot data, model weights, experiment outputs, figures, and
  third-party model source trees; `DATA.md` publishes trajectory identifiers,
  rollout seeds, source sequence locators, and fixed splits, but **no trajectory
  content or extracted features**.
- Classification: Evaluation/Safety (runtime monitoring) + Robot
  Foundation/World Model (reuse of frozen predictive state).
- Why included: it turns an already-deployed world model into a monitor with a
  33,985-parameter readout instead of training a separate monitoring component
  or relying on proxy signals — the cheapest possible form of the "frozen
  backbone, small harness-side head" pattern this repository tracks, and one
  that reports latency (0.2256 ms mean added CUDA latency once the frozen state
  is available) rather than only AUROC.
- Evidence: five-fold out-of-fold evaluation over seven source tasks
  (85.68/88.59 pooled AUROC/AUPRC), best Seen result among 15 matched baselines
  on the ten-task benchmark, and transfer across four **real-robot** populations
  on PIPER X, SO-101, and Franka via fixed-readout transfer or readout-only
  adaptation without updating the backbone. Author-reported; no independent
  reproduction; the released readout cannot be re-run end-to-end without the
  excluded features and data.

### Harbor Adapters and Harbor-Index (promotion from the watch list)

- Paper: https://arxiv.org/abs/2609.04298 (v3, 2026-09-10; cs.AI/cs.SE; previously
  watch-listed on 09-08 and rechecked-unchanged on 09-10)
- Artifacts (all verified live 2026-09-11):
  - https://github.com/harbor-framework/harbor/tree/main/adapters — **87 adapter
    directories** (`aa-lcr`, `abc-bench`, `ace-bench`, `adebench`,
    `aider_polyglot`, `aime`, `algotune`, `arc_agi_2`, `autocodebench`, `bfcl`,
    `bigcodebench_hard`, `bird_bench`, `bixbench`, `clbench`, `codepde`, …) in
    the Apache-2.0 Harbor framework repository (5,118 stars, pushed
    2026-09-11T05:14Z).
  - https://github.com/harbor-framework/harbor-adapters-experiments —
    Apache-2.0 runner that executes one Harbor `JobConfig` and mirrors every job
    and trial into Supabase (rows plus a per-trial `tar.gz`); `src/`, `db/`,
    `examples/`, `pyproject.toml`; 53 MB.
  - https://github.com/harbor-framework/harbor-index — curated index repo with
    **80 task directories**, `leaderboard/` (submissions, judge-goldset,
    SETUP.md, SUBMIT.md), `ADAPTATIONS.md`, `VERIFIER_TIMEOUTS.md`,
    `job-config.yaml`, `scripts/`; no license asserted.
  - Live leaderboard hub: https://hub.harborframework.com/datasets/harbor-index/harbor-index/latest
    (HTTP 200; leaderboard release `harbor-index-1-3`).
  - **Stale tag, recorded literally:** the `harbor-index-1.0` branch linked from
    the paper returns 404 ("No commit found for the ref harbor-index-1.0"); the
    index has moved on to 1.3/1.4 (`cutover/leaderboard-1.3`, `1.4` branches).
- Classification: Evaluation/Safety (unified agentic evaluation infrastructure).
- Why included: this is a real artifact release that resolves a standing
  watch-lite item — the promotion rests on the located, verified artifacts
  (adapters, runner, task set, live leaderboard), not on the paper's claims.
  The **v3 revision itself is cosmetic** (a word-level diff of v2/v3 HTML shows
  only the version string and date), so no claim in this entry depends on it.
- Evidence: adapters port 80+ benchmarks and are validated by code review and
  parity experiments; 8 models × 54 benchmarks each run under Terminus-2 and one
  of three native harnesses; Harbor-Index curates 82 difficult tasks across 29
  benchmarks (80 task directories currently on `main`), with no evaluated
  model–harness configuration above a 30% pass rate and GPT-5.5 + Codex
  strongest at 28.0%. Digital agents only; no robot experiment; author-reported.

## Watch list (new candidates, rechecks)

- **HuRo** (`2609.10706`, cs.RO/cs.CV/cs.LG, CoRL 2026) — robotization pipeline
  turning heterogeneous human video into robot-aligned observations and
  retargeted action trajectories, with a ~630K-episode / 142M-frame dataset from
  five human-video sources; across four real-world manipulation tasks the
  authors report completion rising 51.5%→80.3% and OOD completion 34.9%→72.2%
  with scale. **Treated literally as not open**: the project page
  (https://3587jjh.github.io/HuRo, HTTP 200) contains the code/dataset markup
  inside an HTML comment — `<!-- TODO: re-enable Code once the repository is
  public: -->` — and the linked repository `3587jjh/HuRo` returns **404** on
  both the GitHub API and a direct browser fetch (verified 2026-09-11). The
  abstract's "Code and data are released on our website" is therefore not yet
  true. Promote when the repository becomes public; the data-scaling result is
  otherwise the strongest VLA-pretraining claim in this batch.
- **ORCH** (`2609.11737`, cs.MA/cs.AI/cs.LG/cs.RO) — organizes heterogeneous
  embodied agent teams by pooling interdependence for concurrent work and
  sequential interdependence for prerequisite-governed work; across 25
  wildfire-response missions, up to 50 agents, and eight LLMs, the authors
  report human-designed ORCH organizations improving final score 63.97% and
  efficiency 74.29% (LLM-generated organizations 43.63%/52.53%) over four prior
  embodied multi-agent frameworks. The repository referenced in the paper
  (`github.com/generalroboticslab/ORCH`) returns **404**, and the project link
  resolves to the lab homepage — **no artifacts**; evidence is simulation-only.
  Watch.
- **MaP-WAM / Memory as Plans** (`2609.11561`, cs.RO) — episodic memory as
  completed segment records converted into segment-level plans plus visual
  guidance, executed by a World-Action-Progress model over unknown durations.
  Project page live; the linked repository `aipixel/MaP-WAM` contains **only
  `README.md`** (2 KB, created 2026-09-10, no license) — placeholder, not open.
  Watch.
- **UniMPA** (`2609.11875`, cs.RO, TPAMI submission) — unified
  memory-prediction-action model with persistent-selective future prediction and
  memory-grounded transition modeling. Project page live; `JiuTian-VL/UniMPA`
  contains only `README.md` and `asserts/` (project-page assets), so no
  code/weights. Watch.
- **SEED-UMI** (`2609.11753`, cs.RO, CoRL 2026) — human and robot wear the same
  20-DoF exoskeleton so joint encoders and matched wrist cameras become paired
  cross-embodiment supervision (3.0× data-collection efficiency, 70.0% average
  rollout success on five contact-rich tasks). Repo verified: `assets/`, `docs/`
  (built site), `website/`, README — **project site and PDF only**, no hardware
  files or policy code. Watch.
- **SwarmNxt** (`2609.11382`, cs.RO) — open-source software/hardware platform
  for agile aerial swarms (hardware assembly instructions plus video tutorial,
  parallel deployment automation, ROS 2 navigation stack with control/planning/
  depth estimation). Repository `lis-epfl/swarm-nxt` is live and substantive
  (`ros_packages/`, `docs/`, `ansible/`, `.readthedocs.yaml`, 66 MB, pushed
  2026-09-03) but **asserts no license** and offers no agent/harness contract;
  platform release rather than harness. Watch-lite.
- **BenchShield** (`2609.11028`, cs.CR/cs.AI/cs.SE) — lifecycle-model
  instrumentation for reward integrity in LLM-agent evaluation: static
  phase-aware taint analysis plus runtime infrastructure-side attribution, with
  a 456-trajectory human-adjudicated corpus drawn from 31,000+ public agent
  runs, reporting full-chain recall 77–100% vs 23–94% for an agentic scanner
  baseline and 96% runtime detection accuracy. Built on BenchFlow v0.6.4 and
  Harbor (both open), but **no BenchShield-specific code or corpus release was
  located** on the record or in the HTML — watch.
- **When Validation Stops Learning** (`2609.10873`, cs.AI) — audits update
  admission for continual embodied agents: a range-based confidence gate cannot
  certify unchanged old-task behavior, a paired-binomial construction admits
  31.6% of a common update stream at 2,000 episodes per stage versus zero, and
  a round-level missed-opportunity metric is proposed. Directly relevant to the
  verification-gate line, but the paper states that "physical-robot and VLA
  validation remain open" and its evidence is a constructed one-step pushing
  diagnostic plus a learned-dynamics stress test; no artifacts. Watch.
- **ActSafeGuard** (`2609.11697`, cs.RO/cs.AI) — differentiable,
  training-aligned safeguard layer that folds hard action feasibility into
  flow-matching policy learning through an analytical ray-scaling operator,
  reported at a 100% step safety rate while preserving or improving success on
  π0.5 and Fast-WAM backbones. No artifacts located (record, HTML, and search);
  8-page paper. Watch.
- **DriftNet** (`2609.10892`, cs.CR/cs.AI/cs.LG) — dual-head trajectory
  Transformer that classifies a logged tool-call trajectory as compromised and
  labels every step (benign / injection point / hijacked / failed injection)
  without access to the agent's model (12,536 trajectories, 71,024 labeled steps
  on the task-disjoint AgentDrift split). The companion **AgentDrift corpus is
  public** (`Asif-0209/AgentDrift`, CC-BY-4.0, `data/`, `data_taskdisjoint/`,
  `docs/`, `pools/`, 32 MB, pushed 2026-09-11) and was already watch-listed on
  09-09; the detector itself has no located release. Watch — promote the pair if
  detector code or weights land.
- **MCP registry audit** (`2609.10962`, cs.SE/cs.AI) — a seeded, re-runnable
  probe of 400 randomly drawn npm/stdio MCP servers from a 24,135-server census
  (only 48.8% complete an `initialize` handshake; 37.5% never start; 58.8%
  tool-level safety-annotation omission vs 41.5% on a curated frame), with
  artifacts at `github.com/itguruhaseeb/mcp-probe` and a Zenodo archive
  (doi:10.5281/zenodo.21347997). Valuable as tool-use benchmark methodology
  rather than a harness; watch-lite.
- **Agent Incident Registry** (`2609.11030`, cs.AI) — source-linked catalog of
  agent-related incidents with evidence, stable identifiers, and
  missingness-aware labels, plus an evaluation-scope audit against InjecAgent.
  No artifacts located, and the abstract still carries unresolved LaTeX
  template macros (`\N{}`, `\Yfirst{}`, `\Pprimary{}`), so the record is an
  unpolished preprint; watch-lite.
- **Fork ledger** (`2609.10954`, cs.LG) — counterfactual utility protocol for
  continual world-model updates: a deployment stream is branched at
  pre-registered decision points into matched update/hold continuations under
  common random numbers, and always-updating lowers return on CartPole, Walker,
  and Cheetah. Conceptually adjacent to the update-admission and Phantom-Gains
  line, but simulated control tasks only and no artifacts; watch-lite.
- **Environment-probing memory curation** (`2609.11060`, cs.AI) — gives an
  asynchronous memory-curator agent least-privilege read-only world tools to
  check, scope, and refresh candidate memories without retraining or changing
  the task agent, retriever, or write authority, evaluated in a production-like
  GitHub Copilot harness (CLBench plus 90 adapted APEX tasks). Digital agents,
  no artifacts; watch-lite.
- **Legible Failures** (`2609.11216`, cs.LG) — shows wrong answers can coexist
  with a correct binding recoverable by a linear probe from frozen hidden states
  (16 public checkpoints), and converts probe-sign disagreement into a failure
  detector that beats the model's own confidence by +0.079 AUROC. Adjacent to
  the internal-state-monitoring line (FARM, Do Agents Know When They Succeed?);
  digital only, no artifacts; watch-lite.
- **Benchmark Radar** (`2609.11115`, cs.AI) — living database/search engine for
  AI benchmarks with daily discovery from 37 sources and a 1,283-entry catalog;
  project site and code (`ktwu01/benchmark-radar`) are live. Evaluation
  infrastructure rather than a robot harness; watch-lite for the Surveys and
  Reading Lists section.
- **Defining AI Agents compendium** (`2609.11018`, cs.AI) — survey of agenticness
  along five dimensions plus a public compendium site
  (`agent.duketrustlab.com`, HTTP 200); background reading, watch-lite.
- **Reactive embodied decision benchmark** (`2609.10895`, cs.RO/cs.AI) —
  ReactHuman, a physics-grounded benchmark for human-like reactive
  decision-making in embodied multimodal LLMs (MuJoCo/Genesis-based tasks over
  unitree_rl_gym), with a public Hugging Face dataset
  (`Alan123/reacthuman-benchmark-scaled`, last modified 2026-07-11). Dataset is
  live; benchmark scope is human-likeness rather than harness behavior;
  watch-lite.
- **Topological Necessities** (`2609.11014`, cs.LG) — mechanism-invariant
  cross-embodiment subgoals certified as separating sets by homology, with code
  and data on OSF (link live). Interesting cross-embodiment interface claim, but
  no robot evaluation in the abstract and no harness artifact; watch-lite.
- **IMLE-VLA** (`2609.10915`, cs.RO, IROS 2026) — single-step action generation
  for VLA policies. Project page live (`kianhk6.github.io/IMLE-VLA/`) but it
  exposes **no code or weights link** (page is built from the academic project
  template with no release button); watch-lite.
- **Safety-aware skill adaptation / Dist-GPRL** (`2609.11433`, cs.RO, IROS 2026)
  — distance-aware, safety-guided RL that adapts overlapping local windows of
  sparse trajectory via-points with a Hausdorff-approximation-planner
  safe-subspace prior and dynamic distance-field clearance. No artifacts
  located; watch-lite.
- **MCPSEC / no-box vulnerability analysis** (`2609.10854`, cs.CR/cs.AI) —
  detects indirect prompt-injection vulnerabilities in MCP servers from
  registration metadata alone, without access or interaction (20 servers, 177
  tools). No own artifact located; MCP-security line; watch-lite.
- **Agent-Integrated Software** (`2609.11381`, cs.SE/cs.AI) — position paper
  defining interaction contracts (task bindings, role-specific authority,
  control transitions, outcome evidence) and continuous assurance for
  applications with an embedded agent; no implementation. Watch-lite.
- **CARLAverse** (`2609.11478`, cs.RO) — modular, distributed, multimodal
  human-in-the-loop driving-simulation framework; code is live at
  `git.ieem-ka.de/simulator-environments/carlaverse` (HTTP 200). Generic driving
  simulation is out of scope per policy; watch-lite only because a reusable
  human-in-the-loop harness could matter if it grows an agent contract.
- **AV steering verification** (`2609.10951`, cs.RO) — uses bound propagation
  over trained end-to-end steering weights to cover disturbance strengths
  between captured images, with captures and a Zenodo archive public
  (`doi:10.5281/zenodo.22101297`, HF dataset
  `AD-Assurance-Lab/steering-verification-captures`). Verification methodology
  for automated driving; excluded from the main list under the driving policy,
  recorded here because the artifact is real and the method is reusable.
- **World in World** (`2609.11548`, cs.CV) — training-free inference-time
  interface that converts heterogeneous control evidence into camera- and
  time-labelled clean visual states read through a frozen causal video model's
  native self-attention; project page live. Non-robot world-model interface;
  watch-lite.

**Rechecked (revision present; no README change needed):**

- **Motus2** (`2608.30237` v2, 09-10T16:10) — cosmetic only (affiliations,
  corresponding email); project page and every claim already present in v1. No
  README edit.
- **Compact Visuotactile World Models** (`2609.09597` v2, 09-10T04:45) — v2
  restructures the paper (section renumbering/retitling, e.g. a new "Task and
  Experimental Protocol" section) and its comment field now advertises "Code and
  tabulated results included as ancillary material"; the paper remains
  explicitly simulation-only on a lifting task. Watch status retained.
- **Proxy Policy Steering** (`2609.09148` v2, 09-09T14:08) and **RevalExo**
  (`2609.08090` v2, 09-09T06:39, BMVC 2026 Oral) — replacement versions of
  already-watch-listed items; no new artifact or claim change located on the
  records. Watch status retained.
- **Hi-FLoop** (`2609.08796` v2), **MetaRSI/RSI2** (`2609.06396` v2),
  **RoboReel / Monkey See, Can Monkey Do?** (`2609.08209` v2), **PGMT**
  (`2609.08511` v2), **Dex-X** (`2609.07747` v2), **EMERGE-Policy**
  (`2608.29896` v2), **LightNav-0** (`2608.30935` v2), **Sumo** (`2604.08508`
  v3), **RubricRefine** (`2605.09730` v5), **StateVLM** (`2605.03927` v3),
  **SpecBench** (`2605.21384` v2), **FiberTune** (`2606.08653` v2), **Running
  the Gauntlet** (`2606.14397` v3), **VLA-Precision** (`2609.04355` v2),
  **Beyond Prompts** (`2609.05736` v2), **Bit-Flip Attacks on VLA**
  (`2608.15475` v3), **MOSAIC** (`2603.01260` v3) — all were already rechecked
  by the 09-10 run with unchanged conclusions; re-listed here only to record
  that their in-window revision stamps were re-verified against the same
  verdicts.
- **Standing watch list unchanged:** StageWAM, ReflexVLA/ReflexBench,
  DreamX-Phi, UniTexture, PRISM, GigaBrain-0.7/WBC, ForceU-VLA, LIBERO-VIFO,
  Agent Lightning, VLCP, Hydra-0, BATON, Q-Planning, AutoSaddler, JIT-Agent,
  UCAG-P, TemporalFlow-VLA, PredVLA, FlashVLA, WikiSkill, RedEvoAgent,
  SKILL.state, Agent Mesh, WALL-SS, R2M-Bench, INTENT-AS-A-TOOL, BTS-AgentBench,
  TraceBench, GraphMemix, UrbanGround, LM-X, Zero-WAM, VLAct, Code as Worlds,
  Aero Hand Open, CAITLYN, Dogwood, LongGuard, WebWorld, ASPIRE, S3Gym,
  StudyBench, WorldReward, Principia, Statebench, Puffin-World, FailureSpot,
  FailSAE, Spectral-Target JEPA, Programmable World Model (`2609.10540` — still
  README+assets only, 122 stars, pushed 2026-09-10), DUET-DINO, GALATEA (still
  README+assets), RoboDrop, AXON, CT-SAFR, ViBe, InstantMimic, Semigroup-JEPA,
  Seven Sources, TRACE, TANGO, AgentAudit, IBIB, KVShareArena, MetroLLM-Bench,
  VidHalLoc, RD-Forget, Procedural Memory Under Change, Proof-Carrying
  Cognition, Belief-State Engine, A-JIT, and the 09-09 digital-agent
  eval/safety set — none received a scope-relevant revision in this window
  beyond the items listed above.

## Exclusions in the screened ranges (per policy)

- **Generic VLA/model papers without a harness interface or artifact**:
  `2609.11270` (dual-latent RL for generative robot policy), `2609.10918`
  (ObstaDiff), `2609.11043` (LTLDiff multi-agent diffusion policies),
  `2609.10905` (differentiable IK charts), `2609.11661` (contact-aware MPC for
  an aerial manipulator), `2609.11871` (agent-based MPC for vehicle
  performance), `2609.11633`/`2609.11338`/`2609.10748` (mechanism kinematics),
  `2609.11059` (quadruped gait mechanics), `2609.11579` (underactuated hand
  quasi-statics), `2609.11733` (muscle-driven locomotion RL), `2609.11775`
  (in-hand pen writing), `2609.11553` (perceptive-blind humanoid locomotion),
  `2609.11766` (greenhouse Visual-SLAM), `2609.11079` (RIDE relocalization),
  `2609.11920` (EVPeriscope), `2609.10844` (robotic pianist), `2609.11361`
  (GeoTrussRover), `2609.10855` (coastal VLM evaluation), `2609.11078`
  (freehand swarm sketching) — control/perception/model contributions with no
  reusable harness, recovery, safety, or evaluation contract.
- **Driving-domain papers**: `2609.11549` (operational-data safety
  confirmation), `2609.11527` (Formula Student cone detection), `2609.11476`
  (maritime air-to-sea comms), `2609.11478`/`2609.10951` (recorded above as
  watch-lite only) — excluded under the standing policy unless they introduce a
  reusable agent/VLA harness or evaluation contract.
- **Digital-agent papers with no harness angle and no artifacts**:
  `2609.10922` (Auto-RecSys), `2609.10939` (clinical interview training),
  `2609.11141` (database normalization agents), `2609.11190` (agentic
  share-of-search), `2609.11243` (Sci-MMR), `2609.11318` (Mr.LHDR), `2609.11101`
  (ProMediConv), `2609.11147` (chemical mechanistic discovery), `2609.11176`
  (Debate-to-Skill), `2609.10964` (tail-aware turn-release scheduling — a
  serving/scheduling contribution, not a harness contract), `2609.11042` (T1
  terminal-agent RL — model-centric: a 122B MoE post-training recipe),
  `2609.11061` (belief-shift branching RL), `2609.11294` (AgentZip sandbox
  memory compression — systems infrastructure below the harness boundary),
  `2609.11393` (stability-aware test-time adaptation), `2609.11452`
  (RouteRepair), `2609.11873` (recursive self-improvement roadmap/position
  paper), `2609.11911` (Artificial Id position paper), `2609.11234` (NovGauge),
  `2609.11180` (SemVerBench), `2609.11504` (DeFiFlowBench), `2609.11758`
  (RAG-Safety-Bench), `2609.11065` (MOSAIC GraphRAG), `2609.11636` (MAPLE
  memory-augmented planning), `2609.11660`/`2609.11709`/`2609.11900`/`2609.11859`
  (agent/LLM analysis without runtime artifacts).
- **Benchmarks and datasets outside the robot/harness scope**:
  `2609.10722`, `2609.10787`, `2609.10801`, `2609.11134`, `2609.11185`,
  `2609.11282`, `2609.11334`, `2609.11399`, `2609.11601`, `2609.11115`
  (recorded above as watch-lite), `2609.11897` (CausalArena), `2609.11916`
  (edge VLM species ID).
- **Medical, biological, physics, audio/speech, and vision/graphics papers**:
  the remainder of the 401-entry scan, including `2609.10914`, `2609.10988`,
  `2609.11237`, `2609.11271`, `2609.11477`, `2609.11650`, `2609.11236`,
  `2609.11378`, `2609.11689`, `2609.11864`, `2609.11772`, `2609.11274`,
  `2609.11892`, `2609.11265`, `2609.11638`, `2609.11929`, `2609.11538`,
  `2609.11499` (Recursive Code World Models — non-robot 3D world generation
  from a single image), `2609.11548` (watch-lite above), `2609.11439`,
  `2609.11223`, `2609.11279`, `2609.11703`, `2609.11708`.
- **Withdrawn/withdrawal risk:** none of the 401 new entries carries a
  withdrawal marker as of 2026-09-11 (the 08-21 HODAgent withdrawal remains the
  only one on record).

## Operational notes

- The arXiv API was called over HTTPS
  (`https://export.arxiv.org/api/query`); plain HTTP returns empty responses.
  Two paged queries were used: a submittedDate window
  `[20260909000000 TO 20260911235959]` across cs.RO/cs.AI/cs.CL/cs.CV/cs.LG
  (712 unique entries, 500 + 212) and a last-updated-descending probe (1,500
  records) for replacement versions.
- `/tmp` is **not persistent between shell invocations** on this host — each
  `bash` call receives a fresh ephemeral `/tmp`, so XML and parsed JSON were
  staged in a workspace scratch directory (`.arxiv-scratch/`) and deleted before
  staging. Future runs on this host should not rely on `/tmp` surviving across
  commands within a run.
- arXiv HTML full text (`https://arxiv.org/html/<id>v<n>`) was the most
  productive artifact-discovery route: it surfaced `FARM`, `Ecdysis`,
  `COBRA-Skills`, `ORCH`'s dead repository link, `HuRo`'s disabled release
  button, `SwarmNxt`, `BenchShield`'s dependency-only GitHub links, and the
  Harbor adapter/index repositories, none of which appear in the API comment
  field.
- GitHub REST API returned normal results throughout (no 403 rate limiting this
  run); `git ls-remote` was still used as the remote-ref fallback check.
- `git push` over SSH failed with the known
  `/etc/ssh/ssh_config.d/… owned by nobody` OpenSSH error and was retried
  successfully with `GIT_SSH_COMMAND='ssh -F /dev/null'`.
- The working branch was `main` throughout; the run committed task-owned files
  only (`README.md` and this record) and preserved the unrelated user changes
  (`docs/reference-architecture.md` modified; `docs/ring-harness.png` and
  `handoff.md` untracked).
