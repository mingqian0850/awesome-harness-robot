# Daily arXiv scan — 2026-09-08

## Scope

- Interval: since the 2026-09-07 run's cutoff (published/updated >
  2026-09-04T17:59:35Z, its newest visible stamp; state file `1788761136`,
  commit `80410dc`) through 2026-09-08 ~06:45 UTC. Dates in scope:
  2026-09-05 (Sat), 09-06 (Sun), 09-07 (Mon), and the 09-08 tail.
- **Batch status — the expected Sat/Sun/Mon block is still not indexed.**
  The newest announcement block visible to every probe is byte-for-byte the
  one the 09-07 run screened: `/list/{cs.RO,cs.AI,cs.CL,cs.CV,cs.LG}/new` all
  still show "Showing new listings for Monday, 7 September 2026" with max ID
  `2609.05416` (published 2026-09-04T17:59:35Z); abs pages beyond
  `2609.05416` return 404 (`2609.05417`–`2609.06001` probed); the
  five-category `submittedDate:[20260905..20260908]` query returns 0; the
  per-category heads sorted by `lastUpdatedDate` descending (1000 × 5, the
  documented workaround) contain no entry with `updated` after
  2026-09-04T17:59:35Z. The Sat/Sun/Mon submissions the 09-07 record
  predicted "should land with the next block" have not landed; they remain
  the next run's scope. (The Monday-evening announcement appears to have
  fired with no new content, or arXiv's announcement pipeline is still
  lagging ~3+ days as it did over the previous weekend.)
- **API health.** Date-field queries are healthy again (HTTP 200 with exact
  `totalResults`; last run they 500'd on all date queries). New finding,
  important for future runs: `lastUpdatedDate:` **field queries are silently
  coerced to `submittedDate`** by this API instance — the response title
  echoes `submittedDate`, and a `lastUpdatedDate:[20260904090000 TO
  20260904094000]` window around RedVLA's known v2 update stamp
  (09-04T09:20:28Z) returned only same-window *published* v1s, not RedVLA.
  This explains the 09-06 run's "lastUpdatedDate sweep returned 0": it was a
  `submittedDate` sweep over a weekend with no indexed content. Revisions
  must therefore be caught client-side via per-category heads sorted by
  `lastUpdatedDate` descending (the 09-07 workaround) — do not trust
  `lastUpdatedDate:` field queries. `sortBy=lastUpdatedDate` itself works.
- **Interval content: zero new submissions, zero new revisions.** Combined
  evidence: day-scoped and range `submittedDate` queries over
  [09-05..09-08] = 0; per-category enumeration max stamp unchanged at
  09-04T17:59:35Z; only revision with `updated` past the 09-07 revision-sweep
  endpoint (09-04T17:45Z) is `2606.29165v2` (updated 09-04T17:45:28Z,
  continuum-robot multi-contact force estimation) — title-screened,
  domain-excluded (perception/estimation; v1 predates the curation
  pipeline).
- **Unchanged-batch re-screen (per procedure, one full pass).** All 648
  unique entries across the five `/new` pages' New + Cross + Replacement
  sections were title-scanned (205 keyword hits re-checked against the
  09-07 record's Included/Watch/Excluded sets and prior records — all
  accounted for). Replacement sections (24 cs.RO + 93 cs.AI + 53 cs.CL +
  64 cs.CV + 91 cs.LG) are the same revision set the 09-07 run's 228-item
  sweep covered; no revision touches a README entry.
- **Late-arrival audit (new, and methodologically important).** Comparing
  block membership (today's `/new` pages = the Monday block) against prior
  records' coverage exposed a screening gap in the 09-07 run: it filtered
  block content by `published > 2026-09-03T17:59:55Z`, silently assuming
  everything published earlier was covered by the 09-04 run. But the Monday
  block contained **31 entries published before that cutoff** (IDs
  `2609.03693`–`2609.04305`) that were invisible to the 09-04 run (not yet
  announced/indexed) and are absent from every prior record. All 31 were
  individually screened this run (title + abstract where scope-relevant).
  Future runs should diff `/new`-page membership against record coverage
  rather than trusting published-stamp filters alone. (Separately, 8 old
  papers published 2025-05..2026-08 re-appeared as newly added
  cross-listings on the Monday pages; v1 only, no revisions — out of
  interval scope, speech/voice/media domains.)
- Candidate claims checked against the arXiv record (abs pages), official
  project pages, and official code repositories (live HTTP / GitHub API on
  2026-09-08); README, landscape, and prior source records checked for
  duplicates (none). All performance results are author-reported unless
  stated otherwise.

## Included (README updates)

### EVOHARNESSBENCH: Can Your Agents Keep Pace with an Evolving Harness?

- Paper: https://arxiv.org/abs/2609.04280 (published 2026-09-03, announced
  with the Monday 09-07 block — a late arrival the 09-07 run's
  published-stamp filter missed; first exposure to the curation pipeline)
- Project: https://mas-orchestra.salesforceresearch.ai/evoharness/ (live
  HTTP 200, 2026-09-08; Salesforce Research Reasoning & Agents hub; public
  leaderboard and Colab-based "evaluate your agent" path; no official code
  repository located — GitHub search 2026-09-08 returned no match; treated
  literally as not open)
- Classification: General Harness Methodology; Evaluation (benchmark that
  places non-stationarity in the *harness* rather than the task stream).
- Why included: the harness-evolution measurement complement to the
  curated self-improving-harness line. Existing agent continual-learning
  benchmarks change the task stream over time while keeping the harness
  fixed; EvoHarnessBench instead evolves the externally supplied harness
  (tools, skills, specialist agents) and measures the agent against it in
  two settings — deployment evaluation (retention of previously accessible
  competence as the harness expands) and self-evolving adaptation
  evaluation (whether accumulated experience stays useful as new
  capabilities arrive). Deterministic, verifier-based construction with no
  LLM in the loop: 17 multi-stage harness streams, 802 tasks, 520 tools,
  42 skills, 62 agents. The authors report three persistent gaps —
  harness expansion alone degrades previously solved tasks
  (*harness-induced forgetting*, the measurable counterpart of HCL's
  harness-level-forgetting formulation), self-evolving gains stay
  inconsistent across stages/axes/environments, and retention and
  adaptation can pull in opposite directions. Distinct from Evo-Bench
  (which separates transferable framework gains from base-model strength
  under *intrinsic* harness evolution) and from HCL (a guarded-evolution
  method): this is the controlled retention/adaptation benchmark for
  externally driven harness change.
- Boundary: digital agents only, no robot experiment; project page and
  leaderboard live, no code repository located as of 2026-09-08; results
  author-reported.

### RefactorPlatform (watch-list promotion: official artifact verified)

- Paper: https://arxiv.org/abs/2609.04898 (EMNLP 2026 System
  Demonstrations; screened 09-07 and watch-listed pending repository
  verification — none was found then)
- Code: https://github.com/PiSchool/refactor-platform (MIT; live
  2026-09-08; EMNLP camera-ready commits 2026-08-30, pushed 2026-09-07;
  Python 3.12 server + Next.js 15 web UI + docker/plugins/scripts, CI
  enabled, CONTRIBUTING/CODE_OF_CONDUCT present — real platform content,
  not a placeholder); project: https://pischool.github.io/refactor-platform/
  (live HTTP 200, 2026-09-08)
- Classification: General Harness Methodology (open-source evaluation
  harness for repository-scale refactoring agents); Evaluation.
- Why included (artifact-release-driven): the watch item resolved — the
  official repository is live and substantive. The harness holds the
  environment fixed and varies each design axis explicitly — model
  backbone (OpenRouter and GitHub Copilot CLI), execution regime
  (baseline, retrieval-augmented, multi-agent), and prompt specificity —
  executing each run in an isolated workspace with live terminal
  streaming, per-task logging of tokens/diffs/transcripts, AST-based
  verification, and exportable telemetry for audit and reproduction. Demo
  on 100 multi-file RefactorBench tasks across four model families: the
  authors report AST-aware chunking outperforming naive token-window
  chunking by 25–30% across prompt modes, naive retrieval falling below
  the retrieval-free baseline, a lean retrieval-augmented single agent
  (86%) beating the evaluated sub-agent configuration (66%), and
  retrieval's accuracy gains being absorbed by its token overhead (cost
  per successful refactoring unchanged). The claim that no existing
  harness isolates these design choices for refactoring agents, plus the
  released MIT platform, make this the evaluation-harness analogue for
  the repository-scale-refactoring line (HarnessEval-W-class benchmarks
  measure; RefactorPlatform isolates design axes).
- Boundary: digital coding agents, no robot experiment; results
  author-reported (EMNLP demo).

## Watch list (new candidates, rechecks)

- **FailureSpot** (`2609.04277`, cs.RO/cs.CV) — timestamp-level failure
  detection for VLA policies with a label-efficiency angle: action-chunk
  weak-supervision signals (inconsistent consecutive chunks, frozen/idle
  actions, aggressive random motion) plus active learning for
  timestamp-level annotation, claimed to improve both timestamp- and
  trajectory-level detection across multiple VLA policies. Directly
  relevant to the failure-detection line (FailBench's judging, LIBERO-
  RECOVER's recovery, LIBERO-VIFO cue safety) — but it is a training-
  method paper with no benchmark contract and no artifacts located on the
  arXiv record as of 2026-09-08; watch for a release/benchmark before
  promoting.
- **FailSAE** (`2609.04276`, cs.CV) — interpretable failure prediction for
  general VLMs via sparse autoencoders (not VLA/robot-specific); no
  artifacts; watch-lite.
- **Harbor Adapters / Harbor-Index** (`2609.04298`) — unified evaluation
  infrastructure porting 80+ agentic benchmarks, with an 8-model ×
  54-benchmark run under Terminus-2 and three native harnesses; digital
  eval infra; watch-lite.
- **Spectral-Target Physical Latent Structuring for JEPA-Style World
  Models** (`2609.04264`, cs.LG) — addresses "physical representation
  laziness" in LeWorldModel-class latent world models; video/vision
  domain, not robot-specific; watch-lite.
- **Interface-Induced Trajectory Censoring** (`2609.03966`, watch-listed
  2026-09-04 with live code) — unchanged since 09-04 (no revision, no new
  artifacts); watch status retained.
- Rechecked and unchanged from the 09-07 record (no revision, no located
  release as of 2026-09-08): RoboSPA (repo README still "code and dataset
  are currently being prepared and will be released soon" — not open),
  FWBC-VLA (still v2, 2026-09-04), RedVLA (v2 only; no guard release
  located), VLA-Precision (repo live, held), LIBERO-RECOVER (repo +
  ModelScope assets live; already included 09-07), Task-CoEvolve, TrapVLA,
  HINT, WISE, XR-2, StageWAM, ReflexVLA weights, DreamX-Phi, UniTexture,
  PRISM, GigaBrain-0.7/WBC, ForceU-VLA, LIBERO-VIFO, Agent Lightning,
  VLCP, Hydra-0, BATON, Q-Planning, AutoSaddler, JIT-Agent, UCAG-P,
  TemporalFlow-VLA, PredVLA, FlashVLA, WikiSkill, RedEvoAgent,
  SKILL.state, Agent Mesh, WALL-SS, R2M-Bench, INTENT-AS-A-TOOL,
  BTS-AgentBench, TraceBench, GraphMemix, UrbanGround, LM-X, Zero-WAM,
  VLAct, Code as Worlds, Aero Hand Open, CAITLYN, Dogwood, LongGuard,
  WebWorld, ASPIRE, S3Gym, StudyBench/S3Gym-class, WorldReward,
  Principia, Statebench, Puffin-World, Civilization Framework-class.

## Exclusions in the screened ranges (per policy)

- **Late-arrival gap set (31 entries, all screened this run; domain-
  excluded unless watch-listed above):** `2609.03693` AlcaTRAz (LLM
  jailbreak defense — model-level, no harness contract), `2609.04253`
  AVENUE (audio-video editing eval), `2609.04261`–`2609.04262` (molecular
  graph SSL; Japanese music-search spelling), `2609.04263` (quantized KV
  cache quality recovery — serving infra), `2609.04265` (ProToMEx
  explanations), `2609.04266` (analog softmax hardware), `2609.04267`
  (aerospace surrogate modeling), `2609.04269` (corporate-family entity
  resolution benchmark), `2609.04270` (LLM execute-review-revise
  pipelines — peer-review domain), `2609.04271` (quantum Wi-Fi HAR),
  `2609.04272` (forced-outage risk prediction), `2609.04273`–`2609.04274`
  (video/image compression), `2609.04282`–`2609.04283` (visual generation
  alignment/distillation), `2609.04286` (AI-recruitment systems review),
  `2609.04288` (voice-context serving), `2609.04289` (LETHE memory GAN),
  `2609.04290` (evidence integration in LLMs), `2609.04292` (BER-PEF
  mobility predictability), `2609.04300` (power-system contingency
  screening), `2609.04303` (Abstraction Agent — LLM game abstraction),
  `2609.04304` (Iris search agents — model+data pipeline paper, no
  harness contract), `2609.04305` (TNFlow astronomy), plus the
  watch-listed `2609.04264/04276/04280/04298` above and `2609.04281`
  (When Seeing Overrides Knowing — general-VLM personalized safety
  deferral; no robot/agent-harness contract).
- Interval-wide: no new submissions and no new revisions exist in the
  interval (see Scope); `2606.29165v2` (the single revision past the
  09-07 sweep endpoint) is domain-excluded (continuum-robot force
  estimation; v1 predates the pipeline).
- Old cross-listing re-announcements (8 entries, published 2025-05 ..
  2026-08, v1 only, newly carrying five-category tags): speech/voice/
  audio-video/media domains (`2609.04206` voice-AI bias audit, `2609.04222`
  speech-centric omni understanding, `2609.04223` ASR oral history,
  `2609.04232` dependency-distance corpus study, `2609.04239` finance
  forecasting, `2609.04242` GEPARD TTS, `2609.04249` Encore audio-video
  generation, `2506.04061` on-sensor camera arrays) — no revision, no new
  content; out of interval scope.

## Operational notes

- SSH: `git ls-remote`/`git fetch` failed initially with the known
  OpenSSH `/etc/ssh/ssh_config.d` ownership error;
  `GIT_SSH_COMMAND='ssh -F /dev/null'` works (user key and known_hosts
  unaffected).
- Working tree before the run: `docs/reference-architecture.md` modified,
  `docs/ring-harness.png` and `handoff.md` untracked — preserved untouched
  (never staged or committed). Scratch XML under `.scratch/arxiv-2026-09-08/`
  removed after the run.
- Consistency: README header badge and Current Landscape "Last verified"
  both updated to 2026-09-08; entries added to Harnesses/General Harness
  Design (EvoHarnessBench, RefactorPlatform); `git diff --check` clean;
  cross-file consistency with this record verified.
