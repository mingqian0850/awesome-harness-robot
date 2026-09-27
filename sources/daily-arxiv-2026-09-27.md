# Daily arXiv scan — 2026-09-27

## Scope

- **Interval:** everything announced after the 2026-09-26 run's cutoff. That run
  (started ~08:00 UTC, committed 10:07 local on **Saturday 2026-09-26**) screened
  the announcement block whose OAI datestamp is **2026-09-25** — 902 unique base
  IDs, newest base ID `2609.30266`. This run started ~07:56 UTC on **Sunday
  2026-09-27** and covers everything announced since. There are **no missed
  dates**: no cron slot between the two runs was skipped, and every date in the
  interval is covered by the two OAI windows below.
- **No new announcement block landed.** The OAI-PMH harvest for
  `from=2026-09-27&until=2026-09-27` returned **`noRecordsMatch` for all five
  categories** (cs.RO, cs.AI, cs.CL, cs.CV, cs.LG) — 646 bytes per response,
  zero records. This is the expected weekend gap: arXiv announces Sunday through
  Thursday nights US Eastern, so the block after the one dated 2026-09-25 is the
  Sunday-night batch, which will carry datestamp **2026-09-28**. The same
  no-new-block situation was recorded on Saturday **2026-09-19**, Sunday
  **2026-09-20**, and Saturday **2026-09-26**.
- **This is the required "unchanged batch" case for a second consecutive run.**
  The newest visible block is the same 2026-09-25 block yesterday's run screened
  and re-screened, so — as the procedure requires — the **entire block was
  re-screened once more**, with an emphasis deliberately different from all three
  earlier passes (see Method step 4) rather than assumed settled. The re-screen
  produced **seven README additions** that no earlier pass carried; no earlier
  verdict was reversed.
- **Window and delta.** The wide window `from=2026-09-25&until=2026-09-27`
  returns **1,246 memberships = 902 unique base IDs, every one carrying datestamp
  2026-09-25** (memberships: cs.AI 365, cs.LG 341, cs.CV 206, cs.CL 193,
  **cs.RO 141**). No record in the window carries datestamp 2026-09-26 or
  2026-09-27.
- **Delta against the previous run's harvest (exact, with an
  entity-normalisation control).** Previous window (2026-09-25…2026-09-26) 902
  unique IDs → current window (2026-09-25…2026-09-27) 902 unique IDs.
  **0 IDs appeared, 0 disappeared, and 0 records changed** in datestamp, title,
  abstract, comments, journal-ref, DOI, created date, or category set. The
  entity-normalisation control (`&#34;`/`&quot;` versus `"`) was applied before
  comparison, so no parser-induced false diff is reported. Newest base ID is
  still **`2609.30266`**: the ID frontier did not move and nothing is being held
  back.
- **Byte-level confirmation that the block is unchanged.** The five `full`-window
  XML files differ from yesterday's only in the OAI header — the `<responseDate>`
  and the echoed `until=` request parameter. Record content and record **order**
  are identical: for every category the extracted identifier sequence is equal
  (RO 141, AI 365, CL 193, CV 206, LG 341 identifiers, `same_sequence=True`).
- **Block composition is unchanged.** 616 records carry a created date of
  2026-09-24, 144 of 2026-09-23, and the tail reaches back through August and
  July to 2025 (re-announced replacement versions); **198 records carry
  pre-`2609.` IDs**.
- **Withdrawals in this block: three, all out of scope and all already recorded
  on 2026-09-25.** `2608.06825` (multiscale reward hedging), `2608.24098`
  (uniform stability), `2608.25326` (transductive learning) carry the arXiv admin
  note "withdrawn by arXiv due to unverifiable authorship and affiliation". No
  author withdrawal appears. (`2609.30074` matches a raw text search for
  "withdrawn" only because the word occurs in its abstract; it is not a
  withdrawal.)
- **Export-API cross-check returns nothing usable this window.** With the range
  URL-encoded and called over **HTTPS**, `submittedDate` windows return **0
  entries for all five categories on both 2026-09-26 and 2026-09-27** — the same
  announcement-versus-indexing lag seen in previous runs. The export API is
  therefore used strictly as a boundary check; the OAI harvest is the
  authoritative source.
- **No curated record needed a revision edit.** **94** of the 902 block IDs
  already appear somewhere in the repository's markdown (README, `docs/`,
  `sources/`) at re-screen time, but because the block is byte-stable across the
  three runs their statuses carry forward unchanged from the 2026-09-25 and
  2026-09-26 records.
- **Watch-list re-check:** none of the eleven carried watch IDs (`2609.24274`,
  `2609.24350`, `2609.24563`, `2609.22587`, `2609.21223`, `2609.21502`,
  `2609.22218`, `2609.22247`, `2609.26121`, `2609.26520`, `2609.25558`) is
  present in this block, so all statuses are carried forward unchanged. The two
  IDs watch-listed by yesterday's run (`2609.29091` Passive→Active Exploration,
  `2609.29065` DA-GRD) are in the block; their records are unchanged, neither
  paper links any first-party repository in its full text, and no artifact was
  located on re-check, so both stay on the watch list.

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`) for two windows: `from=2026-09-27&until=2026-09-27`
   (the new block — empty) and `from=2026-09-25&until=2026-09-27` (previous block
   plus any new one). Pages saved to `.scratch/arxiv-2026-09-27/oai-{new,full}-*.xml`;
   every category returned a single complete page with no resumption token, and
   the new window returned `noRecordsMatch` for all five.
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`oai-records-{new,full}.json`, `parse_oai.py`), then **delta against the
   previous run's parsed harvest** with field-wise before/after values
   (`delta.json`), plus the **entity-normalisation control**. Record ordering was
   compared separately because OAI pages can reorder without changing content.
3. **Export-API boundary cross-check** over the 2026-09-26 and 2026-09-27
   `submittedDate` windows per category, over HTTPS.
4. **Fourth-pass re-screen with a deliberately different emphasis.** The three
   earlier passes ranked by (1) artifact-signal language plus concept breadth,
   (2) lifecycle, monitoring, verification, recovery, and governance vocabulary,
   and (3) the README's own taxonomy plus the harness-owned verbs. This pass used
   three emphases none of them used, all of them harness layers the README names
   but has covered thinly — an **operator layer** (teleoperation, human-in-the-loop
   intervention, oversight, correction: 53 hits / 47 not yet in the repo),
   an **interoperability layer** (schemas, formats, calibration, provenance,
   versioning, packaging, middleware: 39/31, 155/130, and 14/12) and an
   **evidence-quality layer** (evaluation validity, leakage, statistical rigour,
   negative and failure-mode studies, sim-to-real gap measurement and uncertainty
   calibration: 38/35, 30/27, 19/18). In parallel the pass produced three
   independent listings: **all 93 uncurated robot/embodied records** in the block
   (of 142 records matching a robot/embodied pattern and 141 carrying cs.RO),
   **every uncurated record naming a concrete release object**, and a ranked
   sweep of the **246 uncurated records touching harness-layer vocabulary**. This
   is what surfaced the additions below; the first three passes had carried none
   of them.
5. **Dossiers and primary-source verification** for every shortlisted record:
   arXiv API version history and dates, the arXiv abstract page and full-text
   HTML for artifact links and release sentences, official project pages, and the
   GitHub API (license, size, creation and push dates, default branch, top-level
   tree) plus the Hugging Face API for dataset gating — all on **2026-09-27**.
   Nothing was taken from a secondary source.
6. **Duplicate screening** of every candidate against `README.md`, `docs/*.md`,
   and `sources/*.md` by arXiv ID, project name, and repository URL before
   writing. No included ID was already present in `README.md`.
7. **Watch-list re-check** by ID against the block (see Scope).

## Included (README updates)

Seven new entries, in five README sections. Artifact status is reported
literally; every repository, license, dataset, date, project page, and tree claim
below was verified live on **2026-09-27**. No entry is a revision edit of an
existing curated record, because no curated record changed in this window.

### Harnesses and Development Platforms — General Harness Design and Self-Improvement

- **Safety Under Scaffolding** (`2603.10044`, cs.AI/cs.CL/cs.CY/cs.LG; v1
  2026-03-08, v2 2026-06-03, **v3 2026-09-24** announced in this block, 78 pages,
  12 figures, 43 tables, pre-registered at OSF DOI `10.17605/OSF.IO/CJW92`):
  tests whether a safety benchmark measures the model or the harness around it.
  Six leading models × four pre-registered safety benchmarks × a direct API plus
  three scaffolds (ReAct, multi-agent, map-reduce) yield **62,808 scored
  evaluations**. Reported: presentation format moves measured safety **5–20 pp**
  for otherwise-identical items, and because multiple choice and open-ended items
  are scored by different methods (answer extraction versus an LLM judge) the gap
  is measurement rather than latent safety — a heuristic refusal classifier would
  have changed the finding in five cases. **Benchmark choice explains 19.3% of
  outcome variance and scaffold architecture 0.4%** (≈45× less); map-reduce, a
  structure-destroying delegation that strips answer options, lowers pooled
  measured safety by **7.3 pp (95% CI 6.4–8.1)**, while pooled ReAct and
  multi-agent effects sit inside the pre-registered ±2 pp equivalence margin;
  pooled estimates hide model-specific reversals on the same items (Opus 4.6
  −16.8 pp, Llama 4 +18.8 pp). The authors' own summary statistic is the
  load-bearing one: composite reliability **G = 0.000 (95% CI [0.000, 0.752])**
  does not support a single composite safety measure as a deployment gate.
  **Verified open:** MIT-licensed `davidgringras/safety-under-scaffolding`
  (3.7 MB; `analysis/`, `data/`, `docs/`, `pipeline/`, `preregistration/`,
  `scaffold_safety/`, LICENSE; created and pushed 2026-07-27) and the OSF
  pre-registration resolves (HTTP 200). LLM safety benchmarks on digital models,
  no robot experiment; results author-reported.
- **Auditability Is Not One Property** (`2609.28581`, cs.AI/cs.LG; v1
  2026-09-23, 35 pages, four figures, fourteen tables, eight random seeds):
  defines auditability as **six separately testable predicates** — trace
  integrity, lossless coding, rule coverage, behavioral agreement, composition
  quality, and value-model reliability — tested through a shared frozen
  symbolizer, passive rule extraction, an **append-only hash-bound ledger**, exact
  environment replay, and offline confidence-ranked arbitration with an explicit
  blind-spot fallback. Reported bounds on what such a description layer can
  claim: rule-set overlap does **not** imply behavioral agreement (policies may
  share symbolic rules while matching near chance on fresh states), so the fused
  policy selects among existing rules rather than generating a new skill; on a
  conflict-dominated task an apparent fusion failure traces to an
  **induction/deployment mismatch** (rules induced from sampled actions but
  evaluated under argmax), and deployment-consistent re-induction reverses the
  arbitration ordering; a fitted-Q generalized-policy-improvement diagnostic
  fails in both environments; the single exploratory comparison favouring rule
  fusion is bounded by its authors as a post hoc comparator on a partly saturated
  task with the fused policy still below the strongest held-out actor. Presented
  as an evidence-bounded audit and composition protocol, not a claim of
  interpretability or autonomous skill generation. **Artifact status literal:**
  `cyrilliu1974/AuditableRL` is a real release (63.6 MB; created 2026-09-23,
  pushed 2026-09-26) that **asserts no license** — source-available, not open
  source. Classic-control benchmarks (CartPole-v1, Acrobot-v1); no robot
  experiment; results author-reported.

### Robot Agent Systems — Agentic Robot and VLA Harnesses

- **GenPHRI** (`2604.08664`, cs.RO; v1 2026-04-09, **v2 2026-09-22** announced in
  this block, 14 pages, 7 figures, 7 tables): makes scenario authoring itself the
  agentic object — VLM generator and critic agents iterate on each component of a
  deployment-ready pHRI scenario (room with furniture, task-appropriate human
  pose and placement, robot motion realizing the requested physical interaction)
  from a natural-language description, with an **orchestrator agent** resolving
  cross-stage decisions. The interface restriction is the design contribution:
  generated placement code may only call a single simulator API, so robot pose is
  computed **from human joint positions rather than absolute coordinates**, which
  keeps the interaction coupled to the person; critics inspect renders plus
  tagged metrics per stage. The same generated motion supports direct execution
  (training-free, contact coverage bounded by the accuracy of the fitted body
  mesh) or training a visuomotor policy entirely in simulation. Reported: 50
  assistive scenarios across nine task families generated with no manual
  intervention, and vision policies reaching about **80% zero-shot real-world
  target completion against about 90% in simulation** in a 12-participant user
  study, with prompt-adherence ratings consistent with the textual prompts.
  **Artifact status literal:** the project page
  (`rchi-lab.github.io/gen_phri/`, CMU Robotics Institute) is live and carries
  the generated scenarios and user-study footage, but **no code, model, or data
  repository is linked** from the paper or the page even though the abstract
  states "We release GenPHRI as a platform" — treated as a project page, not an
  open release. Human-in-the-loop, real-robot evidence; results author-reported.

### Benchmarks and Evaluation — Agent and Embodied Reasoning

- **RECLAIM** (`2609.28850`, cs.AI/cs.LG/cs.SE; v1 2026-09-23, 87 pages, 51
  figures, 14 tables): grades an agent on recovering a **published** result
  rather than maximizing a score, and makes what a paper actually released the
  benchmark's difficulty tier. Across 100 NeurIPS 2025 papers, **Run**-tier
  releases ship code, data, and weights, **Retrain**-tier releases lack weights,
  and **Reimplement**-tier releases lack code; each paper's target result,
  success criterion, and GPU-hour budget are fixed in advance, availability is
  audited by a classifier agent over a 1,000-paper sample under the explicit rule
  that **an artifact counts as available when a tool call can access it, and
  unpublished, paywalled, sign-up, or permission-request artifacts count as
  unavailable**, and a separate language model grades runs from logs and outputs
  rather than from agents' reports. Reported: the best agent in each tier
  reproduces **41% of Run-tier papers, 27% at Retrain, 15% at Reimplement**;
  failed attempts use on average **29% of budget**, so most stop with budget
  left; the most common failure is writing the method without checking any part
  against the paper's numbers (**63 of 400 runs**), and the published traces
  document released tags that crashed their own evaluation scripts and renamed
  branches that made a documented technique dead code. **Verified open (partial,
  literal):** the MIT-licensed `mithils3/reclaim` code and run-bundle release
  (41 MB; created 2026-09-05, pushed 2026-09-23) is described by its own
  repository as an "anonymized code and data release (under review)", so
  first-party identity is not yet confirmed; the audited benchmark dataset
  `Mithilss/reclaim` is public, ungated, and CC-BY-4.0 (created 2026-06-19, last
  modified 2026-09-20, 138 downloads); and the trace viewer
  (`reclaim-traces.vercel.app`) is live. Digital research agents; no robot
  experiment; results author-reported.

### Benchmarks and Evaluation — What to Measure

- **Pairwise Approximation Can Select the Wrong Multi-Robot Plan** (`2609.29929`,
  cs.MA/cs.RO; v1 2026-09-24, 6 pages, 4 figures, 1 table; IROS 2026 Workshop on
  Intelligent Information Gathering): measures the **plan-selection regret** of
  the truncated scoring functions multi-robot coordination methods use. Replaying
  all 16 robot subsets of each four-robot plan on an indoor exploration benchmark
  gives the exact delivered-coverage set function *F*; ranking by the exact
  order-2 Möbius truncation *F₂* instead of *F* changes the selected plan on
  **six of seven maps** at the 15 m candidate-generation range in each of two
  candidate families, with regret up to **0.337 of map coverage**; an
  equal-weight least-squares two-additive fit *G* reduces the regret but still
  changes the selection on three of seven maps in each family; and the additive
  singleton-only score *F₁* selects the exact winner on six of seven maps in one
  family and four of seven in the other, against one of seven for *F₂*. The
  general finding: **lower average reconstruction error does not guarantee lower
  selection regret**. **Verified open:** MIT-licensed `williamteo/pairwise-regret`
  (168 KB; code, recorded scores, project page; created 2026-09-23, pushed
  2026-09-25). Simulation evidence on frozen trajectories; results
  author-reported.

### Runtime, Safety, and Observability — Safety

- **Safe Skill Retirement for Physical Agents** (`2609.29543`, cs.AI; v1
  2026-08-25; under review at AAAI-27): gives skill **removal** an authority
  contract. Skills bundle procedural guidance with execution conditions governing
  authority, user consent, and live environment state; pruning clauses that look
  redundant on authorized benchmark tasks can silently remove dormant safety
  conditions. **Matched authority counterfactuals** hold the requested action,
  tool parameters, and intended effect fixed while varying exactly one governing
  predicate, and the decision is formalized as a **two-gate retirement
  certificate** — authorized utility preserved within a declared margin, and zero
  unauthorized protected effects. Reported, over four frontier and local model
  configurations and twelve skill bundles (2,592 evaluation cells):
  task-certified reductions remove **over 94% of skill clauses** and preserve
  authorized completion yet produce unauthorized protected effects **in every
  bundle**; boundary enforcement removes those effects on the declared audit but
  fails the utility gate for one configuration; one bounded combined protocol
  passes both gates across all four configurations with **zero utility
  headroom**; and an end-to-end check on a read-only Home Assistant camera chain
  verifies proposal, decision, and effect measurement on a real device.
  **No public artifact located:** the full text describes a code-and-data
  supplement (Appendix E, with `data/workload/`, `data/reductions/`,
  `data/guards/`, `data/results/g3/`) but publishes no link to it. Digital agents
  controlling physical and privacy-sensitive devices; no robot experiment;
  results author-reported.

### Runtime, Safety, and Observability — Observability and Replay

- **Don't Read the Log: Execution Traces Contaminate Verifiers in
  Video-Generation Agents** (`2609.28564`, cs.CR/cs.LG; v1 2026-09-23): isolates
  the harness decision that decides whether a verifier verifies anything —
  showing the judge the plan, tool calls, and narration rather than only the
  artifact. Holding the frames fixed on 109 labelled two-event clips in which the
  requested event is visibly completed or visibly missing, a trace reporting a
  successful tool call makes three open-weight Qwen-VL judges (7B, 8B, 32B)
  accept **78–90% of the failures**, up from 7–19% with no text, and a
  contradicting trace makes them reject up to **100% of correct clips**;
  instructing the judge to "use only the frames" does not remove the effect.
  Frontier closed judges are essentially unmoved, so the vulnerability is a
  property of the judge's learned trust in tool logs rather than of the task.
  Because plan-derived text carries no clip-specific information it can only
  shift the judge's operating point, and inside a repair loop that shift becomes
  a **cap on the true pass rate that no repair policy can exceed**, matching
  simulation to two decimals: an honest planner that always regenerates ends at a
  judge pass rate of 1.00 against a human-labelled 0.28, and a pipeline in which
  a cheap checker writes its verdict into the trace launders that checker's
  errors into a stronger final judge (**0.69 false accepts**). The remedy
  direction is **evidence routing** — withhold evidence that cannot bear on the
  requirement — and the paper separates trace- and narration-derived text from
  artifact-derived text produced by a checker that actually looked at the clip,
  trustworthy only to the extent of that checker's accuracy. **No artifact
  located:** the clips, labels, judging harness, simulation code, and per-call
  outputs are stated as "will be released". Digital video agents, no robot
  experiment; results author-reported.

## Current Landscape additions

Four bullets added to `README.md`:

1. The evaluation instrument is being audited as a harness variable, and the
   measurement usually outweighs the machinery (Safety Under Scaffolding's
   19.3% versus 0.4% variance decomposition and G = 0.000; pairwise plan
   selection changing on six of seven maps).
2. Verifier contamination is the quiet failure mode of "show the judge
   everything" harnesses — the evidence-routing counterpart to the trace-integrity
   entry the list already carries.
3. Skill and artifact lifecycles are getting authority contracts rather than
   capability tests (Safe Skill Retirement's zero-unauthorized-effect gate;
   RECLAIM's availability rule as a difficulty tier).
4. Agentic authoring is moving upstream of the robot episode (GenPHRI), and
   audit claims are being split into separately testable properties
   (Auditability Is Not One Property).

## Rejected / watch list

### Newly screened this pass and rejected

- **ViSTR-GP** (`2509.10948`) — runtime cyberattack detection that cross-checks
  encoder-reported measurements against a vision estimate from an overhead camera
  **outside the controller's authority**, validated on a real robotic testbed
  with graded replay attacks. Architecturally the most interesting item rejected
  this pass — it is the physical analogue of the independent-interception
  recommendation already curated — but it is a detection-method paper with **no
  artifact of any kind**, a 2025-09 preprint re-announced in this block rather
  than a new contribution. **Watch:** a first-party release would make it a
  runtime-observability entry.
- **On the Effectiveness of Kernel-Level Evidence for Agent Security**
  (`2609.28915`) — 53-page paired-evidence study introducing the **ACE corpus**
  (4,047 paired sessions, 17 threat models, 12 attack mechanics) and showing
  kernel syscall evidence is discriminative on its own and composes with
  application-layer signal. **Artifact not released:** "Upon acceptance we will
  release the corpus, featurizer, detector weights, LLM-judge harness, fine-tuned
  SLM adapters, and per-fold results." **Watch:** the corpus is the transferable
  object.
- **Dual-Frontier** (`2609.26293`) — verify-then-promote gate that admits a
  world-model-guided decision only when its predicted advantage exceeds a
  certified bound on decision-relevant world-model error, with a formal
  non-identifiability result for failure attribution. A crisp runtime-verification
  contract, but validated in controlled learned-model experiments plus tool-use
  benchmarks, with **no artifact located**. **Watch.**
- **Hardware Keystores for AI Agent Signing Workflows** (`2608.06130`, **v2**
  announced in this block) — five-layer zero-trust enforcement stack around a
  hardware keystore, with committed-intent checks and human-in-the-loop escalation
  dropping prompt-injection attack success from 18.1% to 0% (n = 144) and
  containing MCP tool poisoning, plus an honest negative result that an
  adversarially plausible substitute name defeats the semantic filter.
  **Artifact is an anonymous review mirror** (`anonymous.4open.science`), with a
  permanent open-source release promised only after peer review; excluded as a
  digital-agent signing workflow with no released artifact.
- **Cryptographically verifiable authorization for autonomous AI agents**
  (`2607.21325`, v3, Frontiers in Computer Science) — formalizes agent
  authorization as a verifiable relation with a Groth16 zk-SNARK proof of
  concept; the MIT-licensed `Imari91/zk-auth-agent-demo` demo is real. Excluded as
  a hypothesis-plus-proof-of-concept with no robot or embodied component, and
  thinner evidence than the authority-contract entries included above.
- **Scope Before You Persist** (`2609.29144`) — execution-grounded gate for
  persistent skill edits whose transferable result is that **certification scope
  and retrieval scope must match** (Scoped-ORC 0.816 versus Global-ORC 0.713
  hidden trajectory utility; 0/63 harmful acceptances versus six of eight).
  Excluded for this run: it is a persistent-**prompt/skill** memory result for
  frozen-model code-repair agents, no artifact is linked, and the list already
  carries the harness-memory line it belongs to (this is the closest call of the
  pass and a reasonable future addition).
- **SkillPivot** (`2609.29154`), **TARL** (`2608.03699`), **C3M** (`2609.29735`) —
  three skill- and memory-lifecycle contributions (deviation-guided skill
  updates; five-action memory ledgers with a benchmark; provenance-preserving
  cross-session multimodal memory, whose code at `HuzhouNLP/C3M` is live but
  asserts no license). Excluded to keep this pass selective: each is a method for
  an already-covered harness layer, and none contributes a robot-facing contract.
- **TWIST** (`2609.28575`), **Evaluation of Multi-Turn Consistency in LLM Agents**
  (`2609.29508`), **ExplorationBench** (`2609.30199`), **Metrics That Write
  Themselves** (`2608.18744`), **Where Cyber Agents Struggle** (`2609.28572`) —
  digital-agent evaluation and evaluator-engineering work with no robot component;
  excluded under the standing scope rule, though the evaluator-blind-spot and
  validator-consistency arguments overlap existing entries.
- **CALM/OC4M-adjacent robot method papers screened and rejected this pass** —
  `2609.29228` (BT↔FSM LLM conversion; simulation only, no artifact),
  `2609.28816` (FlyCNS connectome-grounded communication; method, simulation),
  `2609.28887` (HARP reconfigurable aerial-ground platform; hardware platform
  with no reusable software interface), `2609.29157` (OREN-X multi-modal mapping;
  representation), `2609.29908` (MorphIK morphology-conditioned IK; model),
  `2609.29407` (WRAP multi-robot assembly planner; planning method with code but
  no harness contract), `2609.29103` (TRACE cable tracing — unrelated to the
  curated TRACE), `2609.29092` (DAWN depth-denoising world model), `2609.29310`
  (EgoSpeedUp tempo transfer), `2609.29020` (impact-aware catching),
  `2609.30140` (contact selection), `2609.29423` (Temperament Engineering;
  perspective, no artifact), `2609.29861` (GPT-6-Astra VLN technical report; a
  single proprietary model version, no artifact).
- **Artifact-promise exclusions, carried or repeated** — `2609.29419` UCON
  ("The code will be open-sourced"), `2609.29934` Beyond Spatial Benchmarks
  ("All code and datasets will be publicly available"), `2609.28766` TAPESIM
  ("We will release the source code"), `2609.30023` Res-HIL (project page only,
  no repository), `2606.24552` simulator-in-the-loop cloth refinement
  (project page and PDF only; the inference-time simulator contract is
  interesting but the contribution is a manipulation method),
  `2609.28767` BirdsEye (MIT code verified at
  `harelab-ucsc/birdseye/tree/pose-bound`, but the contribution is a
  field-annotation workflow for UAV imagery with the dataset on request —
  excluded as domain-specific data collection rather than a reusable harness).
  The artifact placeholders recorded on 2026-09-25 (the 1 KB Maithili/GAP
  repository, HEXIS's HTTP 401 anonymous link, the RoboRecover dataset "being
  prepared for Hugging Face", and HarnessPAI's "not yet publicly released" code)
  are unchanged.
- **`2510.27065` MLPerf Automotive** — a real standardized benchmark with code at
  `mlcommons/mlperf_automotive`, but it measures automotive perception and
  infotainment **systems performance**, not robot/VLA agent behaviour, so the
  standing generic-driving scope rule applies.

### Carried watch list — unchanged this window

Carried forward without status change (none of the eleven appears in this block;
checked by ID): vla.simd (`2609.24274`), LIBERO-VPro (`2609.24350`), ARSTAG
(`2609.24563`), React When You Need To (`2609.22587`), SafeStage (`2609.21223`),
AWM-3DFM (`2609.21502`), Toollery (`2609.22218`), CHART (`2609.22247`), DTOC
(`2609.26121`), MATE (`2609.26520`), HABILIS Brain 0 (`2609.25558`). The two IDs
watch-listed on 2026-09-26 (`2609.29091` Passive→Active Exploration, `2609.29065`
DA-GRD) are present in the block and were re-checked: records unchanged, no
first-party artifact located, both carried forward. The artifact placeholders
recorded on 2026-09-25 (Maithili/GAP's 1 KB repository, HEXIS's HTTP 401
anonymous link, the RoboRecover dataset "being prepared for Hugging Face", and
HarnessPAI's "not yet publicly released" code) are unchanged.

## Validation performed

- `git diff --check` clean (no whitespace errors, no conflict markers).
- Markdown structure re-checked after every insertion: each new entry is a single
  top-level bullet with balanced brackets, parentheses, and emphasis markers
  (scripted check over all 470 `- [` lines returned zero imbalance); section
  headings (40) and the Contents list are unchanged.
- Every added link was fetched on **2026-09-27**: **15 of 15 URLs added to
  `README.md` in this run return HTTP 200** (project page, code repositories, OSF
  pre-registration DOI, Hugging Face dataset, and trace viewer included).
- Repository licenses, sizes, creation and push dates, default branches, and
  top-level trees were read from the GitHub API; Hugging Face gating and license
  from the Hub API; project pages and full texts were fetched directly.
- Dates checked for cross-file consistency: the README badge, the
  "Last verified" line, and this record all carry **2026-09-27**.
- Categories checked against the arXiv record for every included entry.
- Cross-file consistency: each of the four Current Landscape bullets refers only
  to entries now present in the main list, and the provenance caveats stated in
  the README (GenPHRI's missing code link, Auditability's missing license,
  RECLAIM's anonymized release, Safety Under Scaffolding's v3 provenance,
  Don't Read the Log's unreleased artifacts) match this record.

## Operational notes and blockers

- `/tmp` is per-command in this sandbox, so harvest XML, parsed JSON, dossiers,
  full texts, and logs were kept under `.scratch/arxiv-2026-09-27/` (untracked,
  never staged).
- The OAI full-window files differ from yesterday's byte-wise **only** in the OAI
  header (`<responseDate>` and the echoed `until=` parameter), which is why the
  delta is reported after parsing and after an ordering comparison rather than
  from a file hash.
- The arXiv export API returned zero records for both the 2026-09-26 and
  2026-09-27 `submittedDate` windows in all five categories; it is used only as a
  boundary check, and the OAI-PMH harvest is authoritative.
- No GitHub API rate limit, network, authentication, or conflict blockers; the
  OAI-PMH endpoint, export API, GitHub API, Hugging Face API, and all project
  pages were reachable.

## Commit

Task-owned files committed on `main`: `README.md`,
`sources/daily-arxiv-2026-09-27.md`. Unrelated user changes
(`docs/reference-architecture.md`, `docs/ring-harness.png`, `handoff.md`,
`.scratch/`) were left untouched and unstaged. No branches, no pull requests, no
force-push.
