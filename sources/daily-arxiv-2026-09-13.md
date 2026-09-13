# Daily arXiv scan — 2026-09-13

## Scope

- Interval: since the 2026-09-12 run's cutoff, i.e. everything announced or
  indexed after **2026-09-10T17:59:55Z** (that run's newest visible stamp; max
  in-category ID `2609.11929`). The 09-12 record explicitly left the 09-11 and
  09-12 blocks to this run ("The 09-11 and 09-12 blocks therefore remain next
  run's scope").
- **No new announcement block has landed.** This run (started ~04:00 UTC on
  Sunday 2026-09-13) probed the index directly and found it unchanged: the
  newest base ID overall is **`2609.11930`** (a quant-ph paper, submitted
  2026-09-10), `2609.11929` is the newest ID carrying any of the five target
  categories, and `2609.11931`/`2609.11935`/`2609.11950`/`2609.12000` all
  return 404. That is exactly the previous run's boundary.
- **The export API was unavailable for this entire run.** Every request to
  `https://export.arxiv.org/api/query` returned **HTTP 429 "Rate exceeded."**
  (14 bytes), including a single-record `cat:cs.RO` probe. A patient fetcher
  with twelve rounds of linear backoff (~20 s → 240 s, roughly 50 minutes of
  retries) never received a 200. This is a *different* failure mode from the
  09-07 record (HTTP 500 / empty body): the endpoint is live and rate-limiting,
  not broken.
- Because the documented primary query tool was down, this run substituted two
  **other official arXiv endpoints** and records its evidence explicitly:
  - **OAI-PMH** at `https://oaipmh.arxiv.org/oai` (the `export.arxiv.org/oai2`
    URL 301-redirects here). Harvesting
    `verb=ListRecords&metadataPrefix=arXiv&set=cs:cs:<CAT>&from=2026-09-11&until=2026-09-13`
    for cs.RO, cs.AI, cs.CL, cs.CV, cs.LG returned, with no resumption token
    outstanding (i.e. complete):
    cs.RO **73**, cs.AI **290**, cs.CL **160**, cs.CV **167**, cs.LG **285**
    records = 975 set memberships = **716 unique base IDs**.
  - **Direct record fetches** on `arxiv.org/abs/<id>` and
    `arxiv.org/html/<id>v<n>` for every candidate below.
- **Block boundary is confirmed by OAI datestamps.** All 716 harvested records
  carry header datestamp **2026-09-11**; **zero** records carry datestamp
  2026-09-12 or 2026-09-13. So the newest announcement block is still the one
  announced 2026-09-11, and nothing new has been announced since the 09-12 run.
- Interpretation (consistent with the 08-30, 09-07 and 09-12 records): 2026-09-12
  was a **Saturday** and 2026-09-13 a **Sunday**. The 09-11 (Friday) and 09-12
  (Saturday) submission blocks land with the next weekday announcement, so they
  remain the next run's scope. This is the normal weekend lag.
- **Dates in scope:** no *new submissions* are visible. The period
  2026-09-10T17:59:55Z → 2026-09-13T04:00Z contains **zero new submissions** in
  the five target categories — but, as set out below, it does contain
  **replacement versions that the previous run's revision window could not
  see**, and those were this run's actual yield.

### Unchanged-batch re-screen (required, and performed)

Because the newest visible batch is identical to the previous run's (same max
ID, same newest stamp, same 09-11 datestamp block), the whole batch was
re-screened once from raw harvested XML rather than trusted, per the standing
procedure — and this time the re-screen was run over the **entire 716-record
block**, not only the new-submission window.

- **Two disjoint populations inside the block.** The 09-11/09-12 runs screened
  the **401 entries with base ID ≥ `2609.10627`** (the new-submission window
  published 2026-09-09T02:05:12Z → 2026-09-10T17:59:55Z). **315 of the 716
  block records sit *below* that window** and therefore fall outside any prior
  new-submission screen; 268 of those 315 carry a latest-version date of
  2026-09-09 or 2026-09-10, i.e. they are **revisions or cross-lists announced
  in the newest block**. All 315 were screened; 103 of the 268 matched
  repository-relevant concepts and each of those was read.
- **Only 18 of the 315 had ever been mentioned anywhere in the repository**
  before this run (2,035 distinct arXiv IDs are cited across `README.md`,
  `docs/`, and `sources/`); the other 297 were screened from scratch. Of the 103
  repository-relevant revisions, **92 had never been mentioned**, and every
  inclusion below is a first-exposure paper.
- Concept sweeps over all 716 records for `harness|scaffold`,
  agent-loop/runtime/orchestration, verification-gate/rollback/guardrail,
  recovery/retry/resume, robot-agent/embodied-agent, VLA, skill/tool library,
  memory, world/foundation model, code-as-policy, self-improvement/evolution,
  evaluation/benchmark, and safety/monitoring; plus a full title read of every
  cs.RO-carrying record in the block (73).
- **Revisions — the reason this run had work to do.** The 09-12 run probed
  replacements with `lastUpdatedDate:[20260910180000 TO 20260912235959]`,
  i.e. starting at **18:00 UTC on 2026-09-10**. The 09-11 announcement block
  also carries replacement versions whose `updated` stamp is *earlier on
  09-10* — this run found v2/v3/v5/v6 revisions stamped 03:32, 04:44, 07:36,
  07:43, 11:31, 12:26 and 13:25 UTC on 09-10. A `submittedDate` query cannot see
  them (their v1 lies in the past) and a `lastUpdatedDate` window opening at
  18:00 misses them by up to 14 hours, so **those revisions were invisible to
  both of the previous run's filters**. The 09-11 run's 1,500-record
  last-updated-descending window did not reach them either. Reconstructing the
  block from OAI-PMH is what surfaced them — OAI datestamps records by
  *announcement*, not by version date, which is exactly the axis the export
  API's date filters do not expose.
- **Duplicate check:** `SHAPER`, `Skill-α`/`skill-alpha`, `Causal Past Logic`,
  `GitSkills` and `EBench` (the paper) appear nowhere in `README.md`, `docs/`,
  or `sources/`. EBench's earlier `sources/` hits are a baseline list inside an
  unrelated 09-09 entry, not this paper.
- All performance results below are author-reported unless stated otherwise.
  Artifact status is stated literally and dated to this run (2026-09-13).

## Included (README updates)

Five entries: one embodied harness/skill-evolution paper that had never been
curated, one skill-generation method with a released implementation, one robot
evaluation benchmark, one agent-skill corpus, and one runtime-verification
contract.

### Self-Evolving Embodied Agents via Skill-Harness Evolution (SHAPER)

- Paper: https://arxiv.org/abs/2608.11350 (v1 2026-08-11T18:55:58Z; **v2
  2026-09-10T07:36:49Z**, announced in the 09-11 block; cs.CL primary, cs.RO;
  Northeastern University and Microsoft Research — Peidong Wang, Zhiming Ma,
  Ying Chang, Xufang Luo, Yiqun Zhang, Zihan Wang, Xiaocui Yang, Shi Feng).
  No comment, journal reference, DOI, or project link on the record.
- **The revision is cosmetic; the paper is not.** A word-level diff of the v1
  and v2 HTML shows exactly three differences: the version stamp, the date, and
  two added author affiliations (Northeastern University). No claim, table, or
  section changed. This entry is therefore justified by the *paper* — which no
  prior run had curated — and not by the revision. It is recorded honestly as
  an important catch-up rather than as a substantive v2.
- Artifacts: **none located — treated literally as not open.** The arXiv record
  links nothing; the v2 HTML's only external links are bibliographic, funding,
  and LaTeXML boilerplate. GitHub repository search for the paper's method and
  benchmarks returns 0 results (verified 2026-09-13). No code, weights, data,
  or benchmark assets.
- Classification: Agentic Robot/VLA Harness; General Harness Methodology.
- Why included: it makes the harness the *whole* adaptation surface for an
  embodied agent. SHAPER keeps planner **and** executor weights frozen and
  improves only the non-parametric system — reusable skills plus a
  context-code harness — from target-environment rollouts, with the same frozen
  model acting as both planner and optimizer. That is the same
  "hold the model fixed, vary the harness" design the repository selects for,
  and it is a rare instance of that design applied to a *physical* embodied
  agent rather than to a digital tool user. It also connects two lines the
  README already tracks separately: harness evolution (Self-Harness, Ecdysis,
  HarnessEvolve) and skill-library learning for embodied agents (PRACTICE,
  SkillGate, REFACTOR-VLA).
- Evidence: VLABench and ESI-Bench, chosen to span different low-level action
  interfaces. On VLABench the frozen upper-level planner is Qwen3.6-27B and the
  executor is the official π0 checkpoint; across four 200-episode splits
  (800 episodes) the authors report mean success **34.50%** for SHAPER versus
  28.25% for the seed agent, 33.50% for skill evolution alone, and 30.50% for
  harness evolution alone — so the two artifacts are complementary rather than
  redundant. On ESI-Bench's official-proportion 231-question subset SHAPER
  reports **49.8% micro / 42.9% macro** against 32.5%/31.2% for the seed agent
  and 41.1%/38.6% for skill-only, with the macro score numerically above the
  published GPT-5 Passive-Single-view reference (42.9% vs 40.x%). Baselines
  include pure execution, same-data supervised fine-tuning, and test-time
  scaling (verifier-free selection, trajectory voting). Acquisition cost is
  reported as a one-time ≈**$2.25 (VLABench) / $2.83 (ESI-Bench)** in
  API-equivalent tokens, excluding evaluation and simulator or GPU
  infrastructure, after which the artifacts are reused across all held-out
  episodes without per-episode search — the same cost-shifting argument the
  09-12 entry makes for pre-task preparation. The harness reads only official
  RGB observations and reference images, never pose or depth. **Simulation-only,
  no real-robot experiment; no independent reproduction.**

### Progressive Agent Skill Generation via Reinforcement Learning (Skill-α)

- Paper: https://arxiv.org/abs/2608.01678 (v2 2026-09-10T12:26:27Z; cs.LG,
  cs.CL; The Chinese University of Hong Kong / LIGHTSPEED — Junhao Shen,
  Zhanqiu Zhang, Yiwen Guo). The record names
  `https://github.com/ejhshen/skill-alpha`.
- Artifacts: **verified open.** `github.com/ejhshen/skill-alpha` is live,
  **MIT-licensed**, created 2026-07-29 and pushed **2026-09-11** (the day after
  the v2 submission), 31 stars, containing `LICENSE`, a 7.6 KB `README.md`,
  `pyproject.toml`, and `src/`, `training/`, `verl/`, `docs/`, `assets/`
  directories (GitHub API, verified 2026-09-13).
- Classification: General Harness Methodology (skill discovery / skill-library
  construction).
- Why included: the repository already tracks skill *selection* as a trained
  decision (SkillGate) and skill *optimization* as budgeted search
  (COBRA-Skills, SkillOpt); Skill-α closes the remaining gap by making skill
  **generation** itself a learned policy. It also supplies the supervision
  signal that line had been missing: skills have no natural
  relevance-or-correctness label, so the method scores each candidate edit by
  re-running the downstream task under the original and edited skill on an
  anchored query — a *rollback reward* — turning pipeline-style consolidation
  into a single RL problem over local edits, and unifying document-to-skill and
  experience-to-skill generation under one policy.
- Evidence: a Qwen3-8B policy (SFT warm-up on 6,481 synthesized examples, then
  GRPO) evaluated with a GPT-4o worker. The authors report average downstream
  success improvements over the strongest skill-generation baseline of
  **+3.1 points on CL-Bench** and **+6.7 points on tau2-bench**, and on the
  fully unseen BFCL multi-turn benchmark Skill-α is the only skill-generation
  method that beats the no-skill worker across backbones. On SpreadsheetBench it
  reaches 26.67 ± 0.76 against Anthropic Skill-Creator's 26.17 ± 0.29 while
  outperforming Trace2Skill and SkillPro; multi-seed runs with independently
  resampled anchors support stability, and ablations attribute the gain to the
  rollback reward and progressive editing. Digital agents only, **no robot
  experiment**; no independent reproduction.

### EBench: Elemental Diagnosis of Generalist Mobile Manipulation Policies

- Paper: https://arxiv.org/abs/2606.18239 (v3 2026-09-10T13:25:53Z; cs.RO;
  Shanghai AI Laboratory with Xi'an Jiaotong University, Institute for AI
  Industry, Zhejiang University, and Shanghai Jiao Tong University — Ning Gao,
  Jinliang Zheng, Weinan Zhang, Chunhua Shen, and co-authors).
- Artifacts: **verified open.** Project page
  `https://internrobotics.github.io/EBench-home/`, code
  `github.com/InternRobotics/EBench` (**MIT**, created 2026-04-20, pushed
  2026-09-09, 137 stars; `baselines/`, `scripts/`, `third_party/`, `assets/`,
  bilingual README), and a leaderboard at
  `internrobotics.shlab.org.cn/eval/evaluation-list` — all linked from the paper
  and reachable on 2026-09-13.
- Classification: Evaluation/Safety (robot evaluation benchmark); VLA.
- Why included: it attacks the single-scalar problem in robot evaluation the
  way this repository's benchmark section asks for — 26 tasks across 9 scene
  categories, each tagged along **5 capability dimensions** and paired with
  **4 controlled generalization dimensions**, so one success rate decomposes
  into a capability and generalization profile rather than a ranking. It
  evaluates four frontier generalist policies (π0, π0.5, XVLA, InternVLA-A1)
  under one protocol and reports that they have genuinely different profiles —
  π0.5 highest test success, best train–test retention and strongest mobile
  manipulation; π0 leading dexterous fixed-base and high-precision tasks; XVLA
  and InternVLA-A1 complementary on atomic skills and operating regimes. It also
  reports large within-architecture pretraining gains (9.0–25.1 success-rate
  points), which is the property that makes a benchmark useful for measuring
  what pretraining contributes.
- Caveats recorded literally: **simulation only.** The paper states it does not
  claim simulation scores predict real-robot performance and frames EBench as a
  reproducible screening substrate preceding physical evaluation; the 26 tasks
  cover 9 scene categories sparsely, so scene-level rankings are described as
  preliminary, with expansion toward 50+ tasks on the roadmap. Author-reported;
  no independent reproduction.

### GitSkills: A Dataset of Agent Skills on GitHub

- Paper: https://arxiv.org/abs/2608.10906 (v2 2026-09-10T11:31:35Z; cs.SE,
  cs.AI; University College London, University of Hohenheim, University of
  Cagliari — Giuseppe Destefanis, Daniel Graziotin, Matteo Vaccargiu, Marco
  Ortu). To appear at MSR '27.
- Artifacts: **verified open.** A single self-contained SQLite file archived on
  Zenodo (DOI `10.5281/zenodo.21875637`), a Parquet mirror partitioned by table
  on Hugging Face (`huggingface.co/datasets/mvaccargiu/gitskills` — public,
  ungated, last modified 2026-09-11, 4,027 downloads, verified via the Hub API
  on 2026-09-13), and a GitHub sample repository
  (`github.com/giuseppedestefanis/gitskills-sample`). Note the mirror is the
  *dataset*; no code repository for the paper's analysis was located.
- Classification: Evaluation/Safety (data infrastructure for the skill layer).
- Why included: the repository's skill line (SkillGate, Skill-α above,
  COBRA-Skills, MaliciousSkillBench, ACES) all assumes a skill population; this
  is the first artifact that measures what that population actually is, and it
  documents the structural facts those systems depend on — **3,797,117
  `SKILL.md` files** from **282,200 public repositories** owned by **195,841
  accounts** (July 2026), collapsing to **1,877,981 distinct contents**, with a
  parsed front matter, folder contents, repository metadata, and partial commit
  history per representative, plus the finding that **50.5% of collected files
  are byte-for-byte copies of another file** and that skills have no registry or
  package manager, so reuse happens by copying folders. It also states the
  problem the harness-safety line runs into: a skill is predominantly natural
  language, is selected probabilistically at run time, and is not checked by any
  compiler or type checker.
- Scope note: this is a measurement/mining contribution rather than a runtime
  or interface contract, and it is included for its released data asset and its
  direct relevance to the skill library, not as a harness design.

### Causal Past Logic for Runtime Verification of Distributed LLM Agent Workflows

- Paper: https://arxiv.org/abs/2605.20923 (v2 2026-09-10T04:44:26Z, ICFEM 2026
  camera-ready; cs.LO, cs.AI, cs.PL; Université Paris-Saclay / CNRS / ENS
  Paris-Saclay, LMF — Benedikt Bollig). The v2 comment states the revision is
  substantive: CPL repositioned as an adaptation of PT-DTL, revised related
  work, and an expanded implementation and preliminary-evaluation section.
- Artifacts: **verified open, with a licensing caveat.** The runtime is
  `github.com/zippergen-io/zippergen` (**Apache-2.0**, "Python DSL and runtime
  for structured multi-agent coordination", pushed 2026-09-12); the Lean
  mechanization is `github.com/zippergen-io/zippergen-lean` (created 2026-04-29,
  pushed 2026-09-08, **asserts no license**) with the `icfem/CPLMonitor/*.lean`
  development; and the experiments are pinned to a specific commit of the same
  repository (`experiments/icfem-2026-cpl`). The paper states the formalization
  is kernel-checked by Lean without `sorry` or custom axioms. Project site
  `zippergen.io` is live. The missing license on the Lean repository is recorded
  as-is.
- Classification: Evaluation/Safety (runtime verification contract).
- Why included: it is a *formal* runtime-monitoring contract for exactly the
  setting this repository's observability and guardrail entries run in —
  several LLM agents exchanging messages asynchronously, each with its own
  tools and local context — and it identifies the property informal monitors
  miss: **causal visibility**. In an asynchronous execution an event that
  appears earlier in a global log may be locally unknown to the lifeline that
  must decide, so a guard may only depend on events causally visible to it. CPL
  extends the ZipperGen agent-workflow framework with past-time modalities over
  guards in `if`-constructs and `while` loops, including inspection of another
  lifeline's latest causally visible event and selected stored variables; the
  owner evaluates the guard online to choose the next branch or loop step; and
  the paper adapts the knowledge-vector monitor and proves the locally computed
  monitor value coincides with the denotational semantics of the guard at the
  current event. That is the useful shape for a harness — a decision procedure
  whose correctness is established per-event rather than by inspecting a
  finished trace.
- Evidence: preliminary and deliberately scoped. A monitor microbenchmark over
  a four-lifeline code-review workflow with a 16-subformula merge guard reports
  roughly **0.047 ms local / 0.069 ms send / 0.072 ms receive per event** and
  **1.2 KB** of monitor data per message, with the interquartile range under
  2.7% of the median across nine batches of 300 events after 50 warm-up events
  (CPython 3.12, one Intel i9-9880H host). The evaluation is a single-workflow
  microbenchmark, not a deployed workflow study, and the paper labels it
  preliminary and lists broader workflows and guard generation from
  natural-language specifications as future work. The author discloses AI
  coding-agent assistance (OpenAI Codex, Anthropic Claude) with full
  responsibility taken. Digital agent workflows, no robot experiment;
  single author, no independent reproduction.

## Watch list (this run's findings)

- **Prompt-Induced Waste in Coding Agents** (`2608.01347`, cs.CL; **v6**
  09-10T07:43) — Treats prompt wording, inference effort, and harness policy as
  *interacting* experimental factors rather than independent controls, and
  argues coding-agent efficiency must be modelled as cost per **successful**
  task induced by the trajectory, with token and cache counts as measurements
  of that trajectory rather than optimization targets. Directly adjacent to the
  repository's harness-effects line (Same Model, Different Harness, Multi-Harness
  RL) and to the cost framing in Studying Without a Syllabus — but the abstract
  reports the qualitative finding, not effect sizes, and no code, data, or
  protocol artifact was located on 2026-09-13. **Watch** until the controlled
  measurements or an artifact surface.
- **From Agent Traces to Trust: A Survey of Evidence Tracing and Execution
  Provenance in LLM Agents** (`2606.04990`, cs.CR primary, cs.AI; **v5**
  09-10T03:32) — Survey of evidence tracing and execution provenance for LLM
  agents, i.e. the observability/provenance layer this repository tracks
  (TraceBench, SearchAtlas, Harbor Adapters). It is a survey with no artifacts
  located and no robot content, so it is a candidate for the Surveys and
  Reading Lists section rather than a harness entry; **watch** pending a read of
  whether it contributes a reusable provenance contract.
- **Causal Episodic Memory for Feedback-Driven Agent Repair (MERIT)**
  (`2608.05906`, cs.CL; v2 09-10) — Re-encountered revision of a watch item
  first recorded on 08-10; the new version still reports small gains and still
  states that MERIT is **not reliably separated from untyped dynamic retrieval**
  on either benchmark (Spider 66.34→69.79, BIRD 47.35→48.44, with strong paired
  evidence only on Spider). **Prior watch verdict stands.**
- **Scaling Automatic Research Agents via World Models** (`2608.12564`, cs.LG;
  09-10) — World models for research-agent scaling; keyword-adjacent to both the
  world-model and agent lines, but the artifact and evidence status could not be
  established within this run's screening budget. **Watch.**
- **Verification of Adaptive Agentic Controllers through Finite Rule Revision**
  (`2607.09770`, cs.AI; 09-10) — Verification of adaptive agentic controllers by
  finite rule revision; a plausible verification contract with no artifact
  located on 2026-09-13. **Watch.**
- **GameWAM: A World Action Model for Video Games** (`2608.26200`, 09-10) and
  **StreamTTT: Reconciling Real-Time Perception and Long-Term Memory in
  Streaming VLMs** (`2608.13416`, 09-10) — World-action-model and streaming-memory
  entries outside robotics; relevant only if their interfaces transfer.
  **Watch-lite.**
- **Re-encountered revisions whose prior verdicts stand unchanged.** These
  appeared in this run's revision set but had already been screened and recorded
  by earlier runs, so they are not new findings and were not re-litigated:
  Motus2 (`2608.30237`, assessed cosmetic on 09-11), `2608.21057` (medical
  domain, excluded 08-24), SimSkill (`2609.03753`, watch since 09-04),
  OmegaUse-SOP (`2609.02149`, excluded 09-03), Gated-Memory Routing
  (`2609.00237`, watch since 09-02), Streaming4D (`2609.00610`, watch since
  09-02), Compact Visuotactile World Models (`2609.09597`), MuJoCable
  (`2609.09612`), Kalman tilt estimation (`2609.00730`), and Harbor Adapters
  (`2609.04298`, already a README entry and re-verified).
- **Standing watch list unchanged.** Only one of the 71 watch-list IDs carried
  by the 09-12 record (`2609.09597`, listed above) appears in the
  outside-window revision set at all, and none received a scope-relevant
  revision beyond the already-screened 401-entry window — the remainder still
  sits inside that window. Their previous verdicts therefore stand:
  StageWAM, ReflexVLA/ReflexBench, DreamX-Phi, UniTexture, PRISM,
  GigaBrain-0.7/WBC, ForceU-VLA, LIBERO-VIFO, Agent Lightning, VLCP, Hydra-0,
  BATON, Q-Planning, AutoSaddler, JIT-Agent, UCAG-P, TemporalFlow-VLA, PredVLA,
  FlashVLA, WikiSkill, RedEvoAgent, SKILL.state, Agent Mesh, WALL-SS, R2M-Bench,
  INTENT-AS-A-TOOL, BTS-AgentBench, TraceBench, GraphMemix, UrbanGround, LM-X,
  Zero-WAM, VLAct, Code as Worlds, Aero Hand Open, CAITLYN, Dogwood, LongGuard,
  WebWorld, ASPIRE, S3Gym, StudyBench, WorldReward, Principia, Statebench,
  Puffin-World, FailureSpot, FailSAE, Spectral-Target JEPA, Programmable World
  Model, DUET-DINO, GALATEA, RoboDrop, AXON, CT-SAFR, ViBe, InstantMimic,
  Semigroup-JEPA, Seven Sources, TRACE, TANGO, AgentAudit, IBIB, KVShareArena,
  MetroLLM-Bench, VidHalLoc, RD-Forget, Procedural Memory Under Change,
  Proof-Carrying Cognition, Belief-State Engine, A-JIT, HuRo (`2609.10706` —
  repository still 404), ORCH (`2609.11737` — no artifacts), MaP-WAM
  (`2609.11561` — README-only), UniMPA (`2609.11875` — README+assets), SEED-UMI
  (`2609.11753` — project site only), SwarmNxt (`2609.11382` — no licence),
  BenchShield (`2609.11028`), When Validation Stops Learning (`2609.10873`),
  ActSafeGuard (`2609.11697`), DriftNet (`2609.10892`), Compact Visuotactile
  World Models (`2609.09597`), Proxy Policy Steering (`2609.09148`), RevalExo
  (`2609.08090`), and the 09-09 digital-agent eval/safety set.

## Exclusions in the re-screened block

- **The 165 revision records of the 268 that are not repository-relevant.**
  Each was screened by title and abstract against the repository's scope; none
  introduces a reusable agent harness, embodied-agent harness, robot/VLA
  contract, skill-library or memory mechanism, recovery procedure, or
  evaluation/safety contract. They split into the standing out-of-scope groups
  the previous records already enumerate: conventional robot control and
  perception (`2508.02604` rock chop, `2603.02856` Rhythm whole-body control,
  `2603.03067` CMoE humanoid terrain, `2509.06285` LiDAR registration,
  `2506.07350` semantic map completion, `2601.00702` deformable VIO,
  `2607.26283` collaborative perception, `2609.10283` SwingBot brachiation,
  `2609.06820` occupancy world models for active mapping), driving and aerospace
  (`2602.14948`, `2609.00730`, `2609.11717` already excluded on 09-12), medical,
  biological, audio/speech, graphics, and general vision/language-model work,
  and domain-agent papers (legal, financial, educational, agricultural, sports,
  archaeology) with no harness contract.
- **The remaining 613 block records** — the 401-entry new-submission window
  already screened by the 09-11 and 09-12 runs, the 165 non-relevant revisions
  above, and the 47 records outside the window whose latest version predates
  2026-09-09. The last group is revisions of long-published papers outside this
  repository's scope (`2001.03346`, `2303.09136`, `2308.01030`, `2310.10092`,
  `2403.11169`, `2403.17606`, `2403.19448`, `2404.13381`, `2406.04163`,
  `2406.10015`, `2407.03900`, `2408.10077`, `2410.22912`, `2411.00684`,
  `2411.16975`, and the rest); each was checked, and none is harness-relevant or
  carries an artifact relevant to this repository.
- **Driving-domain, perception-only, and conventional control papers:** out of
  scope under the standing policy unless they introduce a reusable agent/VLA
  harness, recovery, safety, or evaluation contract.
- **Withdrawn/withdrawal risk:** none. No record in the block carries a
  withdrawal marker (the 08-21 HODAgent withdrawal remains the only one on
  record).

## Operational notes

- **The arXiv export API was rate-limited for the whole run** — `HTTP 429`,
  body `Rate exceeded.`, on every request including a one-record probe, with
  twelve rounds of linear backoff (≈20 s → 240 s) all failing. This is recorded
  as a blocker for the documented primary query path, **not** as evidence of an
  empty index. The substitute evidence (OAI-PMH block harvest, direct `abs`/`html`
  fetches, and an ID-existence bisect) is official arXiv infrastructure and is
  strictly stronger for the "did the block land?" question, because OAI
  datestamps by announcement while the export API's date filters do not.
- The OAI-PMH endpoint is reached at `https://oaipmh.arxiv.org/oai`;
  `https://export.arxiv.org/oai2` returns **301** and must be followed with
  `curl -L`. Per-category sets are `cs:cs:RO`, `cs:cs:AI`, `cs:cs:CL`,
  `cs:cs:CV`, `cs:cs:LG` (note `cs:RO` — without the archive prefix — returns an
  empty feed rather than an error).
- OAI metadata semantics recovered this run: the header `<datestamp>` is the
  **announcement** date, while the metadata `<created>` field carries the
  **latest version's** submission date — so `created == 2026-09-10` on an
  ID from an earlier month identifies a replacement version, which is how the
  missing revision set was reconstructed.
- `/tmp` is **not persistent between shell invocations** on this host, so the
  harvest, parsed JSON, screening scripts, and fetched HTML were all staged
  under `.scratch/arxiv-2026-09-13/` (untracked, deliberately not staged).
- Artifact verification used live HTTP checks on `arxiv.org/abs` and
  `arxiv.org/html`, the GitHub REST API (repository lookup and search), and the
  Hugging Face Hub API. The GitHub API returned normal results throughout — no
  403 rate limiting. `git ls-remote` (with the SSH workaround) confirmed the
  remote ref.
- `git push` over SSH failed with the known `/etc/ssh/ssh_config.d/… owned by
  nobody` OpenSSH error and was retried successfully with
  `GIT_SSH_COMMAND='ssh -F /dev/null'`.
- The working branch was `main` throughout; the run committed task-owned files
  only (`README.md` and this record) and preserved the unrelated user changes
  (`docs/reference-architecture.md` modified; `docs/ring-harness.png` and
  `handoff.md` untracked).
