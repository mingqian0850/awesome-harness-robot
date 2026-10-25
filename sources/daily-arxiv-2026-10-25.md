# Daily arXiv scan — 2026-10-25 (unchanged-batch re-screen, plus 2026-10-19/2026-10-18 tail checks; 2026-10-23 → 2026-10-25 owes nothing new)

## Scope

- **Interval since the last successful run.** The 2026-10-24 12:19 CEST run finished on `main` at `baf7e62`
  and left an explicit debt in `sources/daily-arxiv-2026-10-24-2.md`: the export index had not yet indexed
  **2026-10-23** or later, so "the next run must re-scan 2026-10-23 at minimum". This run started
  **2026-10-25 07:55 UTC (08:55 CET)** on `main` at `baf7e62`, so the interval it owes is **submission dates
  2026-10-23 → 2026-10-25**.
- **That interval is still empty, on both date axes.** The index frontier has not moved in the ~24 hours
  since the previous run:

  | query (all five categories) | cs.RO | cs.AI | cs.CL | cs.CV | cs.LG |
  | --- | ---: | ---: | ---: | ---: | ---: |
  | `submittedDate:[20261023]` | 0 | 0 | 0 | 0 | 0 |
  | `submittedDate:[20261024]` | 0 | 0 | 0 | 0 | 0 |
  | `submittedDate:[20261025]` | 0 | 0 | 0 | 0 | 0 |
  | `lastUpdatedDate:[20261023]` | 0 | 0 | 0 | 0 | 0 |
  | `lastUpdatedDate:[20261024]` | 0 | 0 | 0 | 0 | 0 |
  | `submittedDate:[20261022]` (control) | 57 | 169 | 89 | 101 | 151 |

  An unrestricted `cat:cs.RO`, `sortBy=submittedDate&sortOrder=descending` query returns **2026-10-22T17:59:51Z**
  as the newest record (`2610.31670`), i.e. the frontier is pinned at 2026-10-22 for a third consecutive run.
  **No new submission and no new revision is visible in the owed interval.**
- **Consequence, per procedure.** Because the newest visible batch is unchanged from the previous run, that
  whole batch was **re-screened from scratch** rather than declared exhausted: all **443** unique records with
  `submittedDate = 2026-10-22` were re-harvested (identical count to the previous run) and **every title was
  read**, not only keyword hits. This is the pass that pays for itself: it found **six** on-topic records that
  appear nowhere in `sources/daily-arxiv-2026-10-24.md` or `-2.md` as either inclusions or recorded
  deferrals — first-pass misses, not deliberate rejections. All six are included below.
- **Supplementary tail re-screens.** Because the frontier refuses to move, the two batches immediately behind it
  — **2026-10-19** (**691** unique records) and **2026-10-18** (**299** unique records) — were re-harvested as
  recall checks after the previous run's own tail check of 2026-10-20/2026-10-21 recovered twelve earlier
  misses. Both batches harvested completely on the first try at 18–20 s spacing (10 category requests).
  Screening was keyword-led (harness / agentic / skill / memory / recovery / runtime / monitoring / safety /
  verification / audit / benchmark against robot / embodied / manipulation / VLA / world model / dexterous),
  giving 131 + 27 on-topic-looking hits, with abstract review of **8**. Result: **three** inclusions
  ([OpenVLHarness](https://github.com/uclanlp/OpenVLHarness), [Skill-RSI](https://arxiv.org/abs/2610.25114),
  [SkillClash](https://arxiv.org/abs/2610.25064)) and four records moved to the watch list instead (below).
  Given that both tail batches were already inside the 21-day catch-up run of 2026-10-24, this is a direct
  measurement of first-pass recall: **3 on-topic inclusions per ~990 re-screened records**, on top of the 12
  the previous run found in an equivalently sized window.
- **Categories.** `cs.RO`, `cs.AI`, `cs.CL`, `cs.CV`, `cs.LG`, as in every run.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`, one request per category per day with
  `max_results=700`, XML under `.scratch/arxiv-2026-10-25/` (untracked, never committed). Counts for 2026-10-23
  → 2026-10-25 (16 probes), full harvest of the 2026-10-22 batch (5 requests), and a supplementary
  2026-10-18/2026-10-19 tail recall attempt (see *Blockers*).
- **Rate limiting.** A first pass at 4 s spacing drew **fifteen consecutive HTTP 429** responses; widening to
  18–20 s spacing with 45 s backoff made every subsequent batch request succeed on the first try. Request
  spacing, not request count, is what the endpoint punishes — worth carrying forward.
- **Screening.** (a) Full title review of all 443 records in the frontier batch, reopened from the raw harvest
  rather than from the previous run's shortlist. (b) Abstract review of **15** shortlisted records.
  (c) Artifact verification of **12** against live sources: GitHub REST metadata (licence, size, creation and
  push dates, tree listing), raw `README.md`/`LICENSE`/`checkpoints.json`, the Hugging Face model API
  (gating, licence tags, file list) and the project pages. (d) Full-text search of **8** arXiv HTML renderings
  for release statements and repository URLs, because three of the six inclusions carry no link in their API
  metadata.
- **Deduplication.** Every shortlisted ID and title was grepped across `README.md`, `docs/`, and `sources/`
  before inclusion — none of the six was present. Name collisions were checked explicitly: **SCOPE**
  (`2610.31518`, scene-graph TAMP data) collides with the listed [SCOPE](https://arxiv.org/abs/2608.15043) and
  is deferred for that reason as well as for its missing artifact.
- **Revisions.** The `lastUpdatedDate` sweep for 2026-10-23 → 2026-10-25 returned zero records in all five
  categories, and every record in the 2026-10-22 batch is still `v1`. **No existing entry needed a status or
  number update.**

## Included (9)

### General Harness Design and Self-Improvement

- **Compile Once, Run Cheap (Skill-RSI)** (`2610.25114`, cs.RO/cs.AI/cs.LG; v1 2026-10-18 — 10-18 tail find;
  28 pages) — why it qualifies: it moves the recursive-self-improvement question onto a **compiled artifact**
  and states its protocol precisely — a strong general model acts only as an offline author that writes, runs,
  diagnoses and repairs a manipulation skill in simulation until it passes an exam on layouts drawn *after*
  freezing; the frozen skill then runs as code alone or under a cheap caller that reads only its description.
  The compile protocol (isolated audited author, hidden validation layouts, no-feedback and inherit-only
  controls, tamper-evident ledger) and the reuse protocol (identical files executed by other model families)
  are what make the claim checkable, and the negative results carry the paper: on 7 RoboTwin 2.0 tasks,
  **3 never compiled in 18 campaigns** — including five GPT-6 campaigns that scored 78–95/100 but carried 3–16
  safety aborts against a zero-abort gate, where a 100-episode exam bounds abort rate only to 3.6%; inheritance
  beat from-scratch rewriting **8 of 8** (sign test p = 0.004) with an inherit-only lineage attributing most of
  the gap to inheritance rather than development feedback; and write-once campaigns found **no detectable
  first-draft practice effect** (MDE 14 points). Cost is reported in the same currency as quality: USD
  4.29–8.22 of author cost per skill (USD 55 counting the 13 failed campaigns), then **USD 0.00028–0.00054 per
  episode** at reuse with 147/150 held-out successes under a DeepSeek-flash caller — and the authors note
  plainly that **no direct-control baseline was run**. **Artifact verified open (2026-10-25):** Apache-2.0
  `Orillusion/embodied-skill-rsi` (823 files, created and pushed 2026-10-17; both protocols, per-campaign
  `analysis/data/*.jsonl` including the safety and ablation runs, `CITATION.cff`, separate `DATA_LICENSE`).
  Simulation only; author-reported.

- **One Skill Takes Over (SkillClash)** (`2610.25064`, cs.AI; v1 2026-10-18 — 10-18 tail find) — why it
  qualifies: it measures what a skill *file* does when two of them are in context at once, the configuration
  every multi-author skill directory reaches and no skill benchmark tests. In SkillClash — 16 pairs of same-job
  skills, code-checked rules, a reference output per pair — a skill's rules are followed **93–99%** of the time
  when it is loaded alone and **61–62%** when a same-job skill sits beside it, and the failure mode is not
  blending but **takeover**: every rule only one skill asks for is followed, every rule only the other asks for
  is silently dropped, driven by the worked examples inside the files. The fix is the unusually actionable
  part: one trailing worked example that follows **both** skills cuts takeover to **1–15%**, while telling the
  model which skill has priority does not restore the dropped skill and telling it to apply both works only on
  the two strongest models; a single merged file written by Claude Opus 5.5 reaches 96–99% of achievable rules
  across all seven models, with most of that gain carried by its one worked example — which helps only when it
  is correct. **Artifact status literal:** `prazmara/skillclash` (2 stars, 8.9 MB, created 2026-10-15, pushed
  2026-10-18) publishes the harness, spec, job runners, variant builders and human-validation records, but
  carries **no LICENSE file**, so it is source-available rather than open. Digital coding agents, no robot
  experiment; author-reported.

- **OpenVLHarness** (`2610.26860`, cs.CL/cs.AI/cs.CV/cs.LG/cs.MM; v1 2026-10-19 — 10-19 tail find) — why it
  qualifies: it treats evidence *acquisition* as the harness's job. Nine capabilities (visual grounding, OCR,
  depth, camera trajectory, zoom, text search, image search, webpage visits, and a Python coding agent) sit
  behind task-level interfaces that render output as **model-readable evidence**, and a persistent multimodal
  memory keeps images, boxes, masks, depth maps and web references addressable for the whole question so later
  tools and generated code can compute over earlier results. The motivating measurement is a harness argument:
  a controlled study over 22 benchmarks shows that raising the base model's inference budget alone buys only
  modest gains. Training-free across eight backbones, with **+9.7 to +12.6** average points over matched
  no-tool baselines (+11.2 Qwen3-VL-8B, +9.7 Kimi K3, +9.9 GPT-6 Astra), and GPT-6 Luna at medium reasoning
  effort beats the base model, Codex and Visual Sketchpad at maximum effort by 7.7–8.5 points at **≤37%** of
  their model-token cost. **Artifact verified open (2026-10-25):** MIT `uclanlp/OpenVLHarness` (**246 stars**,
  22 MB, created 2026-10-15, pushed 2026-10-22; 310 files — CI workflow, per-backbone paper configs, the nine
  tool modules, `docs/`, tests, and vendored third-party licence notices), plus a live project page, an
  ungated Hugging Face demo space and an ungated evaluation-dataset release. Digital multimodal agents, no
  robot experiment; author-reported.

- **AgenticEvolve** (`2610.31326`, cs.NE/cs.AI/cs.LG; v1 2026-10-22) — why it qualifies: it changes what the
  variation operator *is*. Where prior LLM evolutionary search issues one request that returns a mutated
  program from parents and scores, each island here is a **persistent agent session** with six controlled tools
  (`read`, `write`, `edit`, `run_analysis`, `commit`, `read_commit`) that designs and runs its own experiments
  between evaluations, while the framework owns evaluation, the archive, per-island MAP-Elites, migration, and
  compaction. The controlled arm the design needs — *No-analysis*, identical except experimentation is
  disabled — is present, so the harness's contribution is separated from the model's: better programs than
  *No-analysis* and OpenEvolve on hidden and unseen instances, baselines' final quality in far fewer
  evaluations, and more recovered ground-truth structures on twelve equation-discovery problems; removing
  only the data-analysis tool drops problems fitted within 0.01 tolerance from **63.6% → 35.7%** for one
  model (smaller for two others). **Artifact verified open (2026-10-25):** MIT `pengzy-cosmo/AgenticEvolve`
  (1 star, 698 KB, created 2026-08-31, pushed 2026-10-24; `agentic_evolve/{controller,executor,island,archive,
  config}.py` plus a CLI, five task packages, `CONFIG.md`, and `bubblewrap`/`sandbox-exec` OS sandboxing
  documented). Digital program search, no robot experiment; author-reported.

- **Conservative Meta-Agent Search (CMAS)** (`2610.31436`, cs.AI/cs.LG/cs.MA; v1 2026-10-22) — why it
  qualifies: it treats changing a *deployed* agent's architecture as **change control** instead of offline
  search, which is the contract this list's gate/rollback line keeps circling. Candidates are typed graph
  edits scored against the incumbent by **paired shadow replay under common random numbers** with
  interventional component credit; promotion requires a variance-penalized simultaneous score above a
  fixed margin, is followed by a simulated canary stage, and carries a **pre-specified rollback**; the whole
  thing is formalized as sequential incumbent replacement with switching costs and hard risk constraints.
  The honesty is the interesting part: observed per-update false-promotion **3.4%** against a nominal 5%
  (4.8% sealed rerun), explicitly only an update-level replay statistic on the authors' own benchmark, with
  no guarantee of full-utility improvement, no absence of canary-stage harm, no sequence-level error control,
  and the **highest** search-inclusive token cost of the compared methods. Reported on **MetaInvest-Bench**
  (96 regime-switching streams): normalized dynamic-oracle regret 0.116 → 0.104, held-out utility 0.44 → 0.48
  over the strongest adaptive baseline, contracting toward near-single-agent teams when extra workers add no
  value. **Artifact status literal:** the harness, baseline adapters, seed manifests and ten traced streams
  are "in preparation for public release" (checked 2026-10-25); every baseline is a common-interface
  reimplementation, not an official release, and the authors scope their claims accordingly. Financial
  testbed, no robot experiment; author-reported.

### World and Physical-Reasoning Models

- **DeXplicit** (`2610.30781`, cs.CV; v1 2026-10-22) — why it qualifies: it splits the entanglement that video
  world models accept by default. A lightweight 3D dynamics network predicts **object-centric motion and
  deformation in metric 3D** from the initial scene and a future hand trajectory; a renderer then transports
  the initial appearance along that motion instead of learning dynamics implicitly from pixels. Reported:
  27–40% lower moved-point error than PointWorld, **+47% object-fidelity IoU** on HOT3D under a shared
  renderer, and **57×** faster video synthesis at two denoising steps per block against the same group's 14B
  bidirectional configuration. The reusable surface is the interface — metric geometry becomes a first-class
  inspectable state that the renderer and a downstream consumer can both read. **Artifact verified open
  (2026-10-25):** Apache-2.0 `Abecid/DeXplicit` (5 stars, 8.9 MB, created 2026-10-12, pushed 2026-10-23;
  configs, dataset components, `checkpoints.json` pinning a sha256 per tensor file) plus **ungated**
  Apache-2.0 Hugging Face weights `Abecid/DeXplicit` (0 downloads, `SHA256SUMS`, 3D model + three renderer
  variants, LoRA adapter with base model and revision recorded). S-Lab NTU; author-reported, and evaluation
  is on recorded HOT3D/OakInk2/PhysTwin data rather than closed-loop robot control.

- **UniFlowWAM** (`2610.30991`, cs.RO; v1 2026-10-22) — why it qualifies: it refuses to keep 3D scene flow
  *outside* the policy — as an intermediate prediction, an inference-time prior, or an auxiliary head — and
  makes flow a **third modality on equal footing with vision and action**: a Mixture-of-Transformers with one
  expert per modality, flow carrying its own supervision, and a ten-objective role grid that assigns each
  modality exactly one of target / condition / absent, so one weight set acts as policy, forward dynamics,
  inverse dynamics, and a predictor of future features or 3D flow. Reported: RoboCasa-GR1 **52.1% → 56.4%**
  over its flow-free base, LIBERO **98.1%** held in-distribution with **87.7%** zero-shot on LIBERO-Plus,
  and **87.0%** average success over four real bimanual tasks against **68.5%** for the strongest baseline.
  **Artifact status literal:** no code, weights, or data are linked on the arXiv record, in the full text, or
  on the project page (all checked 2026-10-25); the model initializes from the released LDA checkpoint but
  its own receipt is not public. Simulation plus real-robot evidence; author-reported.

### What to Measure

- **Sensorimotor Alignment (S²)** (`2610.31670`, cs.CV/cs.LG/cs.RO; v1 2026-10-22) — why it qualifies: it turns
  the pre-rollout ranking everyone already performs informally into a **measured proxy** with a stated
  validity bound. S² is a training-free offline metric for how far a model's sensory representation has
  converged onto the geometry of physical motion, and it is validated rather than asserted: SimplerEnv
  success across **12 VLAs** at Pearson **r = +0.717**, positive across three model families and real-robot
  policies, better than aggregate VQA scores for choosing among **8 raw VLM backbones** before any VLA
  fine-tuning, and better than FID/SSIM/PSNR for world-action models. It also localizes the signal — fine-tuned
  VLAs that ignore the instruction succeed in proportion to the S² of their **vision** features, not their
  text features — which is a representation-level explanation a closed-loop number cannot supply.
  **Artifact verified open (2026-10-25):** MIT `yunfeixie233/sensorimotor-alignment` (4 stars, ~129 MB,
  created 2026-05-22, pushed 2026-10-23; metric code, released result tables readable without downloading
  models, CPU-only figure regeneration, and per-family reproduction guides for the 12 VLAs, 8 VLMs and the
  WAM analyses). Rice University and NVIDIA; author-reported, and what the paper establishes is a
  **rank-ordering** proxy, not a task-success predictor.

- **Post-Audit Mirage** (`2610.31130`, cs.AI/cs.LG; v1 2026-10-22; NeurIPS 2026 workshop) — why it qualifies:
  it states the limit an agent-update audit cannot escape rather than offering another audit protocol. When a
  safe and a harmful deployment emit **byte-identical offline evidence**, any offline rule abstaining with
  probability *r* is wrong on at least one matched world with probability **(1−r)/2**; a verifier that scores
  better offline is therefore not thereby the safer gate. The contribution is making the boundary executable
  — matched safe/harmful lifecycles across AuthGate, ConstraintPlan and BatchTriage, protected truth behind
  an exact scorer, and a construction that fails unless public records match and protected answers reverse —
  and then measuring what *fresh* post-update evidence buys: IdentifiedRangeMonitor returns
  deploy / hold / **cannot determine** / unsupported from group-labelled harm outcomes, recovering the
  distinction in favourable cells while remaining undecided for rare groups, small effects, and missing
  observations. **Artifact verified open (2026-10-25):** MIT `beingamanforever/Post-Audit-Mirage` (6.3 MB,
  pushed 2026-09-01; sources, serialized lifecycle data, 23 end-to-end tests, and the compiled `paper.pdf`;
  a vendored third-party licence sits beside the MIT file, which is why the API reports `NOASSERTION`).
  Digital agents, no robot experiment; author-reported.

## Revision edits to existing entries

None. No record in the owed interval and no `v2`+ of any curated ID appeared; every one of the 443 records in
the frontier batch is still `v1`.

## Current Landscape additions

**Three** bullets were appended to the existing list:

1. **Pre-rollout ranking is becoming a measured proxy rather than an assumption** —
   [Sensorimotor Alignment (S²)](https://yunfeixie233.github.io/s2/), with its validity bound stated
   (rank-ordering, not success prediction).
2. **Verification is being bounded by what the audit record can identify** —
   [Post-Audit Mirage](https://arxiv.org/abs/2610.31130), cross-referenced to
   [Look Before You Leap](https://arxiv.org/abs/2609.11957)'s refuse-when-unsure applier and to
   [The reach of a verification tool](https://arxiv.org/abs/2608.28795)'s tool-coverage effect, because all
   three say the same thing from different directions: what a gate can decide depends on what its evidence can
   distinguish.
3. **World models are splitting dynamics from rendering, and the split is the interface** —
   [DeXplicit](https://dexplicit.github.io) and [UniFlowWAM](https://kksham111.github.io/UniFlowWAM_ProjectPage/),
   tied back to the visual-track and Gaussian-scene entries already listed.

CMAS, AgenticEvolve, OpenVLHarness, Skill-RSI and SkillClash are **not** given landscape bullets: the list
already carries adjacent bullets for harness optimisation governance (One Gate Is Not Enough, Harness Continual
Learning, GUI-HARVEST's promote-or-rollback), for program-evolution harnesses (Loreley), for harness-versus-model
attribution, and for the recursive-self-improvement measurement gap, and five more would restate them. They are
reachable from their sections instead.

## Rejected / watch list

### From the 2026-10-19 / 2026-10-18 tail re-screens

- **Refuses When Asked, Complies When Acting** (`2610.27350`, cs.RO/cs.AI; 4-page CoRL 2026 SPAIS workshop
  paper) — the sharpest single finding of the tail batch, and the reason it is recorded rather than dropped:
  with GPT-6 Astra driving a low-cost SO-101 arm through the Inspect Robots harness, the action loop **refused
  0% of 15 hazardous household instructions**, attempted the harm every time and completed 27% of it, while
  `give_up` — its only exit — never cited the hazard; adding an optional pre-motion self-check over the same
  camera frames would have stopped all 15 hazards (including the 4 that completed them) and flagged none of
  15 harmless controls. That is the activation-versus-action gap this list tracks, measured end to end.
  **Artifact status literal:** `Arnav-Gr0ver/Refuses-When-Asked-Complies-When-Acting` (1 star, 109 MB,
  created 2026-10-01, pushed 2026-10-19) is publicly readable and unusually complete — `astra_runtime.py`,
  `run_harm.py`, per-trial CSV/JSONL diagnosis logs, and all **15 trial videos** — but it carries **no LICENSE
  file**, so it is source-available rather than open. **Watch** (and note the size: the evidence is videos,
  not code).
- **Failure Attribution for VLA Models** (`2610.27277`, cs.RO/cs.LG) — asks which corruption's *removal* would
  change the current episode, learns a factorized risk model that estimates removal benefit by latent
  intervention rather than paired counterfactual rollouts, reports roughly **3× lower attribution error than
  frontier VLMs** on OpenVLA and π0.5 in simulation and π0.5-DROID on a Franka, and demonstrates online failure
  monitoring on hardware. On-topic for the observability line, but **no artifact located** in the full text
  (checked 2026-10-25). **Watch.**
- **When Does an Added Planning Capability Create Operational Value?** (`2610.27305`, cs.AI) — an observable
  evidence standard for attributing a planning addition's contribution (opportunity to act, executed-plan
  change, consequence, equivalence, uncertainty, scope), whose three negative results travel beyond industrial
  planning: a composite policy can hide a component that changed 15 of 681 plans, a capability that helps on
  long-lead settings can be 7.6% costlier under tight capacity, and a cheap diagnostic matched the full
  evaluation's winner in only 55.0% of cases. Industrial forecasting testbed, not robotics and not a harness;
  **deferred** as out of scope, recorded so it is not re-litigated.
- **Policy Is Not Capability** (`2610.25060`, cs.LG/cs.AI/cs.CL/cs.CR; NeurIPS 2026 TAE workshop) — exploits a
  deployment accident in which ~53k guardrail prompts were labelled twice under two operational definitions,
  training ten arms that share architecture, codebase and optimizer, to show that a safety benchmark can be
  moved across its whole range by the **label definition** rather than the model: with architecture fixed the
  between-arm range is 0.99 on HarmBench-copyright and 0.043 on WildGuard-benign (a 23× spread), 0.03% of the
  parameters can move a score across its range, and validation accuracy tracks agreement with the annotator's
  policy (rho = +0.74) more than detection capability (rho = +0.34). Strong evidence for this list's "report
  the grading contract" stance, but it is a guardrail-classifier study with no artifact and no robot bearing.
  **Recorded, not listed.**
- **Auditing Tool Traces** (`2610.25540`, cs.AI/cs.CL; NeurIPS 2026 AgenticLS workshop) — a two-stage audit that
  localizes label-predictive shortcuts to individual tool results (per-tool TF-IDF decomposition) and then
  counterfactually redacts them, overturning an apparent tool-augmented gain on TCR-pMHC binding: the strongest
  signal sits in **identifiers inside one tool result** and redacting them returns performance to chance. Good
  observability practice, but domain-specific, no artifact, and the list already carries the source-of-evidence
  audit line. **Recorded, not listed.**
- Also seen and deferred from the tail batches on title + abstract grounds: `2610.26765` HandsOn, `2610.26663`
  RoboValue, `2610.27039` Embodied Playground (three more robotics benchmarks with no artifact visible on the
  record), `2610.27003` Replan for Error Recovery, `2610.26487` SPAR, `2610.25931` EvoSkill v2, `2610.26554`
  BetaMem, `2610.26811` State World Models, `2610.26846` GPT-6 Astra, `2610.26848` Physical RSI scaling, and
  `2610.25736` fleet learning across 96 robots. Each is on-topic but adds no harness contract this list does
  not already carry, and none had a verifiable artifact at screening depth.

### From the 2026-10-22 frontier batch

- **FocusVLA** (`2610.31423`, cs.RO/cs.AI; CoRL 2026) — future-intent-guided dynamic **view routing** that is
  non-invasive to the visual encoder, language model and action backbone; reported +10.10 pp over π0.5 on
  RoboTwin 2.0 and 71.7% → 85.0% real-world. Substantial, but model-side routing rather than a harness or
  evaluation contract, and **no code, weights, or project page are linked** on the record or in the full text
  (checked 2026-10-25). **Watch** for a release.
- **Adaptive Skill Granularity for Coding Agents** (`2610.31461`, cs.AI/cs.SE) — skill granularity as an
  action-abstraction policy with a feature-based controller and a finite-state macro-to-micro recovery
  executor; on-topic for the runtime-routing line. **Artifact status literal:** the only repository,
  `Jayceon-3/Adaptive-Skill-Agent` (731 KB, 1 star, pushed 2026-06-28, i.e. months *before* the paper), carries
  **no LICENSE file** and ships committed `__pycache__`; source-available, not open. **Watch.**
- **What the Specification Did Not Say (specification–oracle gaps)** (`2610.30635`, cs.CL) — a strong result
  for how a benchmark's grader decides what it measures (25 of 98 graded milestones depend on unstated names
  or paths; affected units fail 80.6% of runs against 48.4%). The paper says "we release the audit, corrected
  filter lists and specification addenda", but **no location is given**: the only repository in its full text,
  `DeepCommit-ai/DeepCommit` (Apache-2.0, 12 stars), is the SWE-Milestone task-generation pipeline of
  arXiv 2603.13428, not this audit. Not verifiable as this paper's artifact. **Watch.**
- **EpiSEM-Bench** (`2610.31650`, cs.AI/cs.LG) — 300 matched unsafe/safe pairs measuring unsafe execution
  under suppressed evidence, with an independent audit tool that checks whether an agent *seeks* the missing
  fact and then withholds the prohibited action. Exactly on topic, but no artifact was located on the record
  or in the full text (checked 2026-10-25). **Watch.**
- **RIPPLE** (`2610.31276`, cs.AI) — names the *Critique-to-Action Handoff Gap* and internalizes recovery into
  a single policy (+4.8 mean points over four repository-level benchmarks, no deployment critic). No artifact
  located; and the contribution deliberately moves recovery *out* of the harness and into the weights, which
  the list already covers from the opposite direction. **Deferred.**
- **DRILL** (`2610.30814`, cs.CV; NeurIPS 2026) — trains the world model on the policy gradient its imagined
  rollouts produce rather than on pixel MSE (+13.1 points over WMPO across five simulators and two VLA
  backbones). **Code is supplementary-material only** and checkpoints are promised "alongside the code", so
  nothing is publicly open (full text checked 2026-10-25). **Watch.**
- **SkeLAM** (`2610.31474`, cs.RO) — skeleton-grounded latent actions that separate kinematics from residual
  visual dynamics in a robot world model; project page live, **no repository located** on the page or in the
  full text. **Watch.**
- **PiHand** (`2610.31299`, cs.CV) — latent hand-object interaction prior for physics-based policy learning;
  the abstract ends "We will release the processed data and model checkpoints", i.e. future tense. **Watch.**
- **SmolVLA-MoE** (`2610.30732`, cs.CV) — sparse MoE action expert, 84.25% average on four LIBERO suites. The
  paper names `github.com/dudu435/smolvla-moe`; **that repository returns 404** (checked 2026-10-25). Literal
  status: not open.
- **KinoUAV** (`2610.30609`, cs.CV) — kinodynamic UAV VLN benchmark with directional free-space distance
  labels for safety-aware motion; a genuine safe-navigation benchmark, but UAV-navigation-specific with no
  artifact visible. **Deferred.**
- **MPRF / anticipating navigation retreat** (`2610.31028`, cs.RO) — anticipates an operator's retreat from
  command/EM/video progress **residuals** (0.580 AUPRC over 150 colon-phantom episodes), which is a recovery
  signal on the human side of the loop. Single phantom dataset, no artifact. **Deferred.**
- **SCOPE scene-graph TAMP data** (`2610.31518`, cs.RO; IROS 2026) — uses planner execution failures as
  feedback to repair hallucinated scene graphs (+12.33% downstream TAMP success over DELTA-generated data).
  Project page live but no code located, and the name collides with the listed
  [SCOPE](https://arxiv.org/abs/2608.15043). **Deferred.**
- **Screened and deferred without further action:** `2610.30953` (reasoning–action coupling in driving VLAs —
  useful negative result about explainability, but driving-domain and artifact-free), `2610.30789` and
  `2610.31066` (already deferred by the previous run), `2610.31013`, `2610.30876`, `2610.31569`, `2610.31121`
  (all already recorded by the previous run). The twelve 2026-10-20/21 records the previous run recovered in
  its own tail re-screen are likewise not re-litigated here.
- **Carried watch list — no change this pass:** StageWAM (still v3), ReflexVLA/ReflexBench, DreamX-Phi
  (README-only), UniTexture (no code), PRISM ("Dataset soon"), GigaBrain-0.7/WBC (release unconfirmed),
  ForceU-VLA (README-only), LIBERO-VIFO, Agent Lightning, VLCP, Hydra-0, BATON, CarryOn, EMHO,
  Self-Evolving Agent Harness, Skill2Real, RoboBridge, Local-Recovery-RSI, AgenticTactileVLA, DiVeR, R²-WAM,
  UniIntervene++. None appeared as a new version or a new artifact in this interval.

## Validation performed

- **Markdown structure:** `README.md` is now **1,213 lines / 40 section headings**; the nine new entries sit
  inside existing sections (`### General Harness Design and Self-Improvement`,
  `### World and Physical-Reasoning Models`, `### What to Measure`) and the three new `## Current Landscape`
  bullets were appended to the existing list rather than creating a heading. The `UniWAM` entry above the
  insertion point was re-read line-by-line after the edit to confirm no content was truncated.
- **Added lines:** all 14 content lines added checked for balanced `[`/`]` and `(`/`)` — none unbalanced; every
  `[text](url)` resolves to an absolute URL (the only non-absolute target is the pre-existing in-page badge
  anchor); every content line begins with `- `.
- **`git diff --check`:** clean.
- **Links:** all **25** distinct content URLs added to the README were fetched — **25/25 HTTP 200** (11 arXiv
  abs pages, two of which are pre-existing cross-references; 7 GitHub repositories/page; 3 Hugging Face pages;
  4 project pages or project sites).
- **Artifact claims checked against live metadata, not paper text (2026-10-25):** MIT for
  `pengzy-cosmo/AgenticEvolve` (698 KB, pushed 2026-10-24), `yunfeixie233/sensorimotor-alignment` (~129 MB,
  pushed 2026-10-23) and `uclanlp/OpenVLHarness` (246 stars, 22 MB, 310 files, pushed 2026-10-22); MIT for
  `beingamanforever/Post-Audit-Mirage` (6.3 MB; API reports `NOASSERTION` because a vendored third-party
  licence sits beside the MIT file, read directly); Apache-2.0 for `Abecid/DeXplicit` (8.9 MB, 162 files).
  Gating checked through the Hugging Face API: `Abecid/DeXplicit` is `gated: false`, `private: false`, licence
  `apache-2.0`, with seven checkpoint files plus `SHA256SUMS`; the OpenVLHarness demo space and evaluation
  dataset both resolve 200.
- **Negative artifact checks, recorded literally:** `dudu435/smolvla-moe` → 404; `Jayceon-3/Adaptive-Skill-Agent`
  → present but no licence; `DeepCommit-ai/DeepCommit` → present, Apache-2.0, but it is the SWE-Milestone
  task-generation pipeline rather than the specification audit; UniFlowWAM, FocusVLA, SkeLAM and EpiSEM-Bench
  → no repository anywhere in record, full text, or project page.
- **Duplicates:** all six included IDs and their titles grepped across `README.md`, `docs/`, and `sources/`
  before insertion; none present. `SCOPE` name collision handled explicitly (see Screening).
- **Dates and categories:** each included ID's v1 date, latest version and primary category were re-read from
  the harvest records and cross-checked against the README text; all six are still `v1`.
- **Cross-file consistency:** the README badge, the `## Current Landscape` "Last verified" line, and this
  record all carry **2026-10-25**; the scope statement here matches the previous record's stated debt
  (2026-10-23 onward).

## Operational notes and blockers

- **Index lag (the interval's blocker, not workable around).** The export API has still not indexed
  2026-10-23, 2026-10-24 or 2026-10-25 on either date axis as of 2026-10-25 08:05 UTC, while 2026-10-22 is
  fully present and is also the newest record under an unrestricted descending sort. **The debt now passes to
  the next run: re-scan 2026-10-23 onward, and if the frontier is still 2026-10-22, re-screen that batch
  again** — this run is evidence that repeat passes keep finding real misses (six this time, after twelve in
  the previous run's tail re-screen).
- **HTTP 429 is about spacing, not volume.** Five category requests at 4 s spacing drew 15 consecutive 429s;
  the same requests at 18–20 s spacing all succeeded first try, including all ten tail re-screen requests.
  No mirror or proxy fallback was needed.
- **`/tmp` does persist within a single tool call on this host**, but not reliably across calls; all harvests,
  HTML captures and URL lists live under `.scratch/arxiv-2026-10-25/`, which stays untracked.
- **Repository hygiene.** `docs/reference-architecture.md` (modified by the user), `docs/ring-harness.png`,
  `handoff.md`, and `.scratch/` are pre-existing user state and were left untouched. Only `README.md` and this
  file are task-owned changes.
