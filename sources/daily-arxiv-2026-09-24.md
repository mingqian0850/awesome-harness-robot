# Daily arXiv scan — 2026-09-24

## Scope

- **Interval:** everything announced after the 2026-09-23 run's cutoff. That run
  (started ~06:47 UTC on Wednesday 2026-09-23) screened the announcement block
  whose OAI datestamp is **2026-09-23** — 951 unique base IDs, newest base ID
  `2609.26796`. This run started ~07:37 UTC on **Thursday 2026-09-24** and covers
  everything announced since.
- **New announcement block.** The OAI-PMH harvest for
  `from=2026-09-24&until=2026-09-24` returned **824 unique base IDs, every one
  carrying datestamp 2026-09-24** (memberships per category: cs.LG 326, cs.AI
  305, cs.CV 184, cs.CL 168, **cs.RO 146**; 1,129 memberships total). This is the
  Wednesday-night (US Eastern) announcement clearing the 2026-09-23 submission
  day. Every category returned a single complete page with no resumption token,
  so the harvest is not paginated away.
- **Wider window, for the previous block and the delta:**
  `from=2026-09-23&until=2026-09-24` returns **1,753 unique base IDs**, split as
  **929 carrying datestamp 2026-09-23** and **824 carrying datestamp 2026-09-24**
  (no other datestamp is present in the window).
- **Composition by the submission date of the announced version** (OAI
  `<created>`): **550 carry 2026-09-23**, 159 carry 2026-09-22, and the remainder
  reach back through August and July to 2022 — the tail is re-announced
  replacement versions. **196 of the 824 block records carry pre-`2609.` IDs**,
  the signature of those replacement versions.
- **Delta against the previous run's screened window (exact).** Previous window
  (2026-09-22…2026-09-23) 2,631 unique IDs → this window (2026-09-23…2026-09-24)
  1,753 unique IDs. **776 IDs appeared, 1,654 disappeared**, and **132 field
  changes across 51 IDs** (datestamps 51, created 45, abstract 14, comment 10,
  title 5, DOI 3, categories 2, journal-ref 2). The disappearances are a
  **window shift, not retractions**: 1,680 records carried datestamp 2026-09-22
  in the previous window and are simply outside `from=2026-09-23`; of the
  previous run's 951-record 2026-09-23 block, **929 still carry datestamp
  2026-09-23 and 22 were re-datestamped to 2026-09-24**.
- **Only 51 of the 824 block IDs were present in the previous run's window**, so
  **773 block IDs are new to screening**. The 51 overlapping IDs were re-checked,
  and the five title changes in the window are all outside this list's topics
  (QUBO pruning/quantization, LLM valence readout, Apple-AMX matmul, ASR
  long-tail, and `OmniEcho`, whose new title is audio-visual spatial
  understanding for embodied agents — perception-only, no artifact).
- **ID frontier moved and is not truncated.** Newest base ID in the block is
  **`2609.28473`** (the previous run's frontier was `2609.26796`). Direct
  existence probes: the next assigned ID `2609.28474` exists but is
  `astro-ph.GA` (outside the five screened category sets), while `2609.28500`,
  `2609.28550`, and `2609.29000` all return **HTTP 404 / zero entries**, so the
  harvest reached the assigned frontier and nothing is being held back.
- **Withdrawals in this block: none.** No record in the 2026-09-24 block carries
  withdrawal language in its comment field; the "withdraw" string matches inside
  this window are ordinary prose (teacher-guidance annealing, Kalman update,
  cutting-tool withdrawal). The five withdrawals recorded by the 2026-09-23 run
  remain outside this window.
- **Export-API cross-check.** With the range URL-encoded, `submittedDate`
  windows return `opensearch:totalResults` for **2026-09-22** (cs.RO 103, cs.AI
  167, cs.CL 73, cs.CV 114, cs.LG 167) and **2026-09-23** (cs.RO 77, cs.AI 117,
  cs.CL 73, cs.CV 86, cs.LG 126) — both indexed and matching the OAI block's
  submission-date composition. The **2026-09-24** window returns **0 for all five
  categories**: today's submissions are announced in the OAI block but not yet
  indexed by the export API, the same announcement-versus-indexing lag seen in
  previous runs. The export API is therefore used strictly as a boundary check;
  the OAI harvest is the authoritative source.
- **Two records already curated in `README.md` changed in this window.**
  `2604.12447` (**HazardArena**) moved to **v3** — the risk inventory grows from
  40 to 51 tasks, the paper widens to 23 pages ("revised version with additional
  experiments and updated analysis", 5 figures), and the abstract now reports
  physical-world experiments confirming that hazardous execution transfers
  beyond simulation; the README entry was **updated** (see Included).
  `2609.24124` (**ActiveArena**) only moved datestamps/created on a re-announced
  replacement with no content change, so no README edit was needed. Three other
  changed IDs that appear anywhere in the repository (`2609.16597`,
  `2609.23889`, `2609.25804`) live only in daily records or outside scope.
- **Uranus is the status reversal of the window.** `2609.24815` was watch-listed
  by the 2026-09-22 run for a 404 SDK repository and recorded by the 2026-09-23
  run as **withdrawn by the authors**. In this block the record's comment field
  changes from the withdrawal notice to a full artifact list, the OAI `<created>`
  moves from 2026-09-22 to 2026-09-23, and the export API reports **v3 updated
  2026-09-23T06:19:25Z**. The release is real but partial (see Included).
- **Watch-list promotion.** `cair-vinuni/FoldQuantVLA` (`2609.24433`) returned
  404 on 2026-09-22 and 2026-09-23 and is now **live under Apache-2.0** (created
  2026-09-04, pushed 2026-09-23) with the full `foldquant` package, TensorRT
  plugin kernels, per-family deployment scripts, and Jetson documentation; it is
  promoted from watch to included.

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`) for two windows: `from=2026-09-24&until=2026-09-24`
   (the new block) and `from=2026-09-23&until=2026-09-24` (previous block plus
   new block). Pages saved to `.scratch/arxiv-2026-09-24/oai-{new,full}-*.xml`;
   every category returned a single complete page with no resumption token.
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`.scratch/arxiv-2026-09-24/oai-records-{new,full}.json`, `parse_oai.py`).
3. **Delta against the previous run's parsed harvest**
   (`.scratch/arxiv-2026-09-23/oai-records-full.json`): set difference for
   appeared/disappeared IDs plus field-wise comparison for content change
   (`delta.json`), and a separate datestamp-movement analysis to separate window
   shift from retraction.
4. **Export-API cross-check** over the 2026-09-22, 2026-09-23, and 2026-09-24
   `submittedDate` windows per category, used only as a subset/boundary check,
   plus ID-existence probes at the frontier.
5. **Two full passes over the 824-record block with different ranking emphases.**
   Pass 1 (`analyze.py`) ranked by artifact-signal language (GitHub / Hugging
   Face / GitLab / project page / "we release" / "code available") and by concept
   breadth (harness, VLA, robot foundation, world action model, code-as-policy,
   skill, memory, recovery, evaluation, safety, agentic), and additionally listed
   **every record in the block mentioning "harness"** and **all 146 cs.RO
   records**. Pass 2 (`rescreen.py`) re-ranked the whole block by lifecycle and
   contract vocabulary instead — release lifecycle, runtime
   monitor/intervention, verification/audit/provenance, failure
   diagnosis/recovery, evaluation protocol/benchmark contract, skill
   library/discovery/evolution, robot deployment/integration, and harness
   engineering/agent runtime internals — and separately listed the 57
   robot/agent records carrying artifact language. Concept counts in the block:
   `harness` 17 records, `VLA` 21, world/action model 28, memory 80, recovery 37,
   evaluation-language 240, safety 77, agentic 55.
6. **Dossiers and primary-source verification** for every shortlisted record:
   full title/abstract/comments, the arXiv abstract page and export API for
   version history and submission/update dates, the arXiv full-text HTML for
   artifact links and release sentences, and the official GitHub / Hugging Face
   APIs (license field, size, creation and push dates, default branch, top-level
   tree, raw README/LICENSE) — all on **2026-09-24**. Project pages were fetched
   and inspected directly. Every README addition's link was then re-checked in
   one pass: **42 of 42 added URLs return HTTP 200**.
7. **Duplicate screening** of every candidate against `README.md`, `docs/*.md`,
   and `sources/*.md` by arXiv ID, project name, and repository URL before
   writing. No included candidate ID was already in `README.md`; `2609.24815`
   (Uranus) appeared only in the 2026-09-22 and 2026-09-23 daily records, where
   it was watch-listed and then recorded as withdrawn.
8. **Watch-list re-check**: the artifact placeholders recorded by the 2026-09-23
   run (vla.simd, LIBERO-VPro, ARSTAG, React When You Need To, SafeStage,
   AWM-3DFM, Toollery, CHART, DTOC, MATE, HABILIS) plus FoldQuantVLA.

## Included (README updates)

Eighteen entries added, one entry revised (HazardArena v3), and five "Current
Landscape" bullets. Artifact status is reported literally; every repository,
dataset, checkpoint, and project-page claim below was verified live on
**2026-09-24**.

### Harnesses and Development Platforms — General Harness Design and Self-Improvement

- **Bounded Loops** (`2609.27871`, cs.AI/cs.SE; v1 announced 2026-09-24,
  submitted 2026-08-20, 74 pages): the strongest general-harness contribution in
  the block. A bounded loop is a worker, an **independent gate the worker cannot
  write to**, and a declared budget; bounded-loop graphs add a repair relation
  that lets a downstream failure re-run a finished upstream node, and three
  properties are proved from control flow — termination under repair with a
  closed-form worst-case attempt total (given a global repair budget), no node
  reaching DONE without a gate verdict in an append-only hash-chained ledger, and
  a ceiling enforced inside an attempt rather than between attempts. The
  instrument half names **vacuity** and **self-attestation** as the two classes
  of gate that pass anything, and reports **47 vacuous gates in shipped, reviewed
  code** across a 69-loop catalogue, no false accepts over 209 destroying mutants
  (α ≤ 1.8%, Wilson 95%, explicitly labelled saturation), and a 23.3% false-accept
  rate once the frozen gates face a fresh operator family. **Verified open:** the
  Apache-2.0 `qualixar/bounded-loops` repository (engine, `catalog/`, `loops/`,
  `graphs/`, `paper/`, tests; created 2026-07-06, pushed 2026-09-03) is the
  release the paper cites. Digital agents only; no robot experiment; results
  author-reported.
- **Harness as a Language: A Minimalist Agent Framework With Maximal
  Expressivity** (`2609.26891`, cs.AI; v1 announced 2026-09-24, submitted
  2026-09-22, 25 pages): tests the minimal-harness hypothesis directly. JAZ
  exposes one LLM-backed primitive, `invoke`, plus hooks, and requires only that
  the model can write arbitrary executable code (including recursive `invoke`)
  and that every input and the whole interaction history are variables in the
  code environment. With prompting only — no tools, memory, or filesystem — the
  authors report beating Letta (MemGPT) by 8% at half the cost on the
  recall-heavy portion of StuLife, and ACE by 4% at lower cost on AppWorld.
  **Verified open:** Apache-2.0 `jaz-lang/jaz` (published on PyPI as `jaz-lang`
  0.2.0a4, four releases; created 2026-04-12, pushed 2026-09-11) and the
  MIT-licensed `jaz-lang/jaz-evals` reproduction package (ten arms, two
  benchmarks; created 2026-09-07, pushed 2026-09-23). Digital agents only; no
  robot experiment; results author-reported.
- **Realize What Matters: Principled Context Representation for Large-Scale
  Reasoning** (`2609.27173`, cs.CL; v1 announced 2026-09-24, submitted
  2026-09-23): makes the harness's context representation the design object,
  derives principles from relevance realization, and reports +20 and +8.4 points
  over the strongest of nine baselines, with 4B/9B models outperforming all
  evaluated 35B baselines and a 35B-A3B configuration beating Claude Code with
  Claude-Sonnet-5 at 3.7× lower cost. **Verified open:** MIT-licensed
  `michaeltheologitis/r3con` (created 2026-09-19, pushed 2026-09-23) plus
  `r3con-evaluation` (108 MB; no license asserted). Digital agents; no robot
  experiment; results author-reported.
- **SkillGym** (`2609.27717`, cs.CL; v1 announced 2026-09-24, submitted
  2026-09-23): converts human-written skills into executable, verifiable training
  environments and releases **2,756 environments across 12 categories** and
  **8,364 successful trajectories** (49 tool calls and >60k tokens on average).
  Reported SFT gains under Claude Code: +199 Elo on GDPval-AA v2, +19.10 points
  on Terminal-Bench 2.1, +28.13/+12.38 on SkillsBench v1.1 with and without
  skills. **Verified open (partial):** MIT-licensed `ECNU-ICALK/SkillGym`
  (221 MB, pushed 2026-09-24) and the public `ecnu-icalk/SkillGym` dataset
  (`Tasks.tar.zst` + per-harness trajectories, 14 files); the companion
  `ecnu-icalk/SkillGym-Agent` dataset is **gated** (API returns an authentication
  error) and is treated literally as not public. Digital agents; no robot
  experiment; results author-reported.
- **From Agent Output to Authorized Transition** (`2609.28216`, cs.AI/cs.SE; v1
  announced 2026-09-24, submitted 2026-09-23, 10 pages): specifies the
  evidence-and-receipt contract for letting a lifecycle act on an agent's claims
  — authoritative source profiles, binding to the exact artifact and a frozen
  policy baseline, dependency currency, risk-appropriate independence, gate
  decisions recorded as receipts, and **authorization rechecked at the effect
  boundary**. **Verified open:** CC-BY-SA-4.0 `Agile-V/agentic_agile_v`
  (created 2026-05-19, pushed 2026-09-14) and `Agile-V/agile_v_skills` (created
  2026-02-05, pushed 2026-09-17, 54 stars). Software/firmware/PCB engineering,
  not robotics; no robot experiment; contract-and-case paper, author-reported.

### Robot Agent Systems — Agentic Robot and VLA Harnesses

- **RegenHarness** (`2609.27612`, cs.RO; v1 announced 2026-09-24, submitted
  2026-09-23): the block's most complete robot-agent harness. A **model loop**
  for proposals is coupled to an **agent loop** owning dispatch, observation,
  verification, commitment, and bounded recovery; four **role-isolated contexts**
  separate planning, supervision, verification, and recovery; **versioned memory
  separates observed facts from accepted task progress**; an identity- and
  version-bound **commit gate** controls trusted task state; and the runtime adds
  duplicate-dispatch control, resource leases, and recovery budgets behind
  explicit backend contracts, re-checking the original user goal before reporting
  completion. Its **evidence-gated recursive self-improvement** protocol admits
  changes to context rules, task templates, routing, and recovery policies only
  through fixed regression checks and release authorization, with versioned
  rollout and rollback — and is explicitly forbidden from updating weights online
  or weakening the commit gate. Evidence: a real quadruped deployment
  (voice-triggered warehouse navigation, panoramic inspection, visual analysis,
  message delivery, return, spoken reporting, with linked audio, images,
  trajectories, receipts) plus a circuit case showing history-dependent
  completion. **No artifact located** on the arXiv record or in the full text;
  real-robot; results author-reported.
- **EmbodiedSWE** (`2609.27308`, cs.RO; v1 announced 2026-09-24, submitted
  2026-09-23): benchmarks coding agents on contact-rich, deformable, and
  long-horizon dexterous tasks (up to half an hour of continuous interaction),
  then turns verified solutions into VLA supervision through EmbodiedSWE-Gen;
  a VLA fine-tuned solely on generated simulation demonstrations completes a
  long-horizon task on a real robot. **Verified open:** Apache-2.0
  `EmbodiedSWE/EmbodiedSWE` (104 MB, created 2026-06-13, pushed 2026-09-24) and a
  live project page. Simulation benchmark plus one real-robot transfer result;
  results author-reported.
- **BEE** (`2609.27450`, cs.AI/cs.RO; v1 announced 2026-09-24, submitted
  2026-09-23): treats human corrections as evidence about a constraint rather
  than actions to reproduce — a Correction Model predicts the correction and its
  per-dimension consistency, and consistency sets per-dimension constraint
  tightness over a **frozen VLA**. Reported: 91.2% average success on three real
  tasks plus LIBERO-Pro against 57.5% for RLT and 42.1% for DSRL, and the lowest
  intervention rate on all real tasks. **No artifact located** in the full text
  (links found were third-party only); real-robot; results author-reported.
- **Spatial and Semantic Reasoning for LLM-Driven Robot Navigation via MCP**
  (`2609.27340`, cs.RO; v1 announced 2026-09-24, submitted 2026-09-23): converts
  occupancy grids into metric, pose-aware images and records waypoint-level
  observations, then exposes both through the **Model Context Protocol** as
  standardized tools usable by any MCP-compatible model against an unmodified ROS
  navigation stack. Reported >97% map coverage and correct target selection in
  simulation. Simulation-only; **no first-party artifact** (the only repository
  cited is the third-party Apache-2.0 `robotmcp/ros-mcp-server`); results
  author-reported.
- **TANDEM** (`2609.28314`, cs.RO; v1 announced 2026-09-24, submitted
  2026-09-23, under review): represents human teleoperation as an on-demand
  planning capability — VLM-extended predicates and human-executed magic
  operators let the planner interleave autonomous and human stages, and after
  each human stage it **re-perceives and checks the intended effects** before
  resuming. Reported: 2.9× the demonstrations of full-task teleoperation at
  equal human time; π0.5-DROID fine-tuned on 20 demonstrations per task rises
  from 0% to 60% on five tasks. **Verified partial:** project page live, no code
  or dataset repository located. Real-robot; results author-reported.
- **Watch, Recall, Act: Always-On Robots in Concurrent Embodied Streams** (ARMS,
  `2609.28429`, cs.RO; CoRL 2026; v1 announced 2026-09-24, submitted 2026-09-23):
  a streaming π0.5 policy with three asynchronous context providers — live
  perception, embodied state, and an **agent-causal self-history** of which arm
  did what and when — so watching and recalling never block acting and both arms
  act concurrently. Reported 45% on the combined task against 28% for the
  strongest of four baselines, with all three modules necessary in ablation.
  **No artifact located**; real dual-arm evidence; results author-reported.

### Benchmarks and Evaluation — Manipulation and VLA (revision)

- **HazardArena v3** (`2604.12447`, cs.RO): the README entry was **revised in
  place**. V3 (updated 2026-09-23) grows the risk inventory from 40 to **51
  risk-sensitive tasks**, widens the paper to 23 pages with additional
  experiments and updated analysis, reports the hazard-amplification pattern
  across **four representative VLA backbones**, replaces the v2 abstract's
  "training-free Safety Option Layer" framing with an **inference-time VLM safety
  layer** and **refusal-augmented fine-tuning**, and adds **physical-world
  experiments** showing the failure transfers beyond simulation. Artifact status
  unchanged and re-verified: MIT-licensed repository live (GitHub reports
  `NOASSERTION` because eleven copyright holders are listed in the LICENSE).

### Benchmarks and Evaluation — What to Measure

- **Counterfactual Memory Audit** (`2609.27247`, cs.RO; v1 announced 2026-09-24,
  submitted 2026-09-23): separates memory *sensitivity* from memory *guidance* by
  crossing two histories at a **verified-identical present**, querying a frozen
  policy under common randomness, and scoring each saved action under both pasts
  — yielding memory sensitivity, warranted choice, matched-world physical value,
  and per-pair reliability. On Mem-0 every audited Put Back pair changes action
  but only 20/64 are fully reliable; restoring a 4096-byte protected anchor
  recovers 38.9 points of Swap success lost to injected bank faults; on a
  dual-arm platform five of nine completed Put Back manipulations reach the wrong
  target. **No artifact located**; real-hardware evidence; results
  author-reported.

### Benchmarks and Evaluation — Agent and Embodied Reasoning

- **Ajar** (`2609.26900`, cs.AI/cs.CR/cs.SE; v1 announced 2026-09-24, submitted
  2026-09-22): adds **open privilege** as a third axis beside attack success and
  benign utility by attaching to AgentDojo, reusing its tasks and schemas, and
  presenting each benign task with unneeded candidate tool calls at every action
  point. Five defenses (Progent, CaMeL, AC4A, Permission Assistant, Claude Code
  Auto mode) leave widely different amounts of privilege open, two leak nearly
  equally while differing in completed tasks, and one buys tightness by refusing
  entitled calls. **Verified open:** MIT-licensed `reSHARMA/Ajar` (created
  2026-09-21, pushed 2026-09-22). Digital agents; no robot experiment; results
  author-reported.
- **CAVEAT** (`2609.27273`, cs.AI/cs.CL; v1 announced 2026-09-24, submitted
  2026-09-23): tests delegated agents where the environment has a stake in the
  outcome — nine marketplace environments, eight steering mechanisms, matched
  controls. User-optimal purchasing falls from 78.6% (control) to **17.3%**
  (steered) across five model families; ablations locate three entry points, and
  CAVEAT-Harness targets them for +55.0 points, with targeted post-training
  helping a smaller open model. **No artifact located**; digital agents; results
  author-reported.

### Vision-Language-Action Models

- **MemBodied** (`2609.28256`, cs.AI/cs.CV/cs.RO; v1 announced 2026-09-24,
  submitted 2026-09-23): fixed-size episodic memory for a VLA — a gated
  associative state plus an initial-scene anchor — so context and latency do not
  grow with episode length. Reported 7.81× a stateless policy and 2.98× vanilla
  recurrent memory on five RMBench memory tasks, 1.3× the strongest
  memory-augmented baseline with 10× fewer added parameters, and 90.6% on
  LIBERO-Long (+5.4% over stateless π0). **Verified open:** Apache-2.0
  `declare-lab/MemBodied` (created 2026-09-23, pushed 2026-09-24) ships the
  official implementation, π0/π0.5 RMBench backends, ablations, and the RMBench
  suite; the project page is live; **no released checkpoints were located**.
  Simulation evidence; results author-reported.

### Robot Foundation and World Models

- **Latent evolving World Action Model (LeWAM)** (`2609.27455`, cs.CV/cs.RO; v1
  announced 2026-09-24, submitted 2026-09-23): reports that **JEPA predictive
  embeddings support action generation better than compressed VAE latents**
  (I-JEPA best in their comparison), conditions action generation on those
  embeddings, and predicts future embeddings in the same space without a
  video-diffusion backbone; adds **Demonstration-Guided DPO** to derive
  preference supervision offline from demonstrations. With 0.4B trainable
  parameters the authors report 92.28% average success on RoboTwin 2.0 and
  real-world manipulation. **Verified open but unlicensed:** code at
  `XuejiFang/LeWAM` (created 2026-09-21, pushed 2026-09-23) and weights at
  `XuejiFang/LeWAM` on Hugging Face (predictor/vision-encoder safetensors, last
  modified 2026-09-23) are live; **neither asserts a license**. Simulation plus
  real-world; results author-reported.

### Simulation and Digital Twins

- **Uranus** (`2609.24815`, cs.AI/cs.RO; **v3 updated 2026-09-23**): the status
  reversal documented above. **Verified open (partial):** the Apache-2.0
  `D-Robotics-AI-Lab/Uranus-OSS` inference repository is live (created
  2026-09-02, pushed 2026-09-24) with streaming runner, DiT, VAE, text encoder,
  and skeleton modules; `D-Robotics/Uranus-1.3B` and `Uranus-1.3B-Distillation`
  publish real `.pt` weights (dit, vae, text encoder, Plücker adapter,
  tokenizer) under Apache-2.0; `D-Robotics/Uranus-Demo-Data` is public
  (agibot-world-2026 episodes with synchronized multi-view video, MJCF,
  metadata, temporal alignment; 1,201 downloads); the project page resolves.
  The advertised **`D-Robotics-AI-Lab/Uranus-SDK` still returns 404** and is
  treated literally as unreleased. Simulation-infrastructure results are
  author-reported.

### Runtime, Safety, and Observability — Runtime Building Blocks

- **FoldQuantVLA** (`2609.24433`, cs.RO; v1 announced 2026-09-22, promoted from
  the watch list): native W4A4/W8A8 post-training quantization with one
  consistent activation representation carried through calibration, rounding,
  and native integer execution, custom TensorRT plugins for Ada and Jetson AGX
  Orin, and no policy retraining. Reported 1.20–1.33× speedups on Orin,
  1.25–1.52× on desktop, and 80.0% → **92.5%** GR00T N1.7 real-robot success over
  80 trials per configuration at +1 ms Orin latency. **Verified open:** the
  Apache-2.0 repository is now live (17 MB, 1,145 entries, created 2026-09-04,
  pushed 2026-09-23) with `foldquant` (calibration, INT4/INT8 paths, export,
  evaluation protocol), CUDA/TensorRT plugin kernels, GR00T N1.5/N1.6/N1.7 and
  π0.5 deployment scripts, tests, and Jetson docs; **no checkpoints are
  published** (it quantizes upstream weights). Simulation plus real-robot;
  results author-reported.

## Rejected / watch list

Everything below was read against the arXiv record, the full-text HTML, and
(where a link existed) the live project page, repository API, or raw repository
content on 2026-09-24.

### Carried watch list — re-checked, status unchanged

- **vla.simd** (`2609.24274`) — project page still HTTP 200 and still promises
  code and checkpoints on acceptance. **Not open**; watch.
- **LIBERO-VPro** (`2609.24350`) — artifact links remain "Dataset (soon)".
  **Watch.**
- **ARSTAG** (`2609.24563`) — `boweili666/ARSTAG` (56 MB, no license, pushed
  2026-09-19) still resolves only to the project website, not code. **Watch.**
- **React When You Need To** (`2609.22587`) — code still promised on acceptance.
  **Watch.**
- **SafeStage** (`2609.21223`) — `JinzhuLuo/SafeStage` is still a 0 KB,
  license-free stub (created and last pushed 2026-09-17). **Not open**; watch.
- **AWM-3DFM** (`2609.21502`) — `dtc111111/AWM-3DFM` still returns 404.
  **Watch.**
- **Toollery** (`2609.22218`) — `XiangxiTian/toollery` still resolves, still
  asserts no license; first-party provenance remains unconfirmed. **Watch.**
- **CHART** (`2609.22247`) — still no first-party artifact. **Watch.**
- **DTOC** (`2609.26121`) — the only implementation link remains a personal fork
  of OpenCode (`chaturvediabhay24/opencode`, last pushed 2026-06-15, before the
  preprint). **Watch.**
- **MATE** (`2609.26520`) — `yerik-yu.github.io/MATE/` still live with
  demonstrations only; no code or dataset repository. **Watch.**
- **HABILIS Brain 0** (`2609.25558`) — `tommoro.ai` still returns 200 with no
  artifact section. **Watch.**

### Watch-list status changes

- **FoldQuantVLA** (`2609.24433`) — **promoted to included**: the advertised
  `cair-vinuni/FoldQuantVLA` repository, 404 on 2026-09-22 and 2026-09-23, is now
  live under Apache-2.0 with the full package (see Included).
- **Uranus** (`2609.24815`) — **watch status closed and reversed**: the 2026-09-23
  run recorded an author withdrawal; v3 (2026-09-23) restores the paper and adds
  a partial release (Apache-2.0 inference code, two weight repositories, a public
  demo dataset). The advertised SDK repository is still 404 and is recorded as
  unreleased.

### Verified and rejected — artifact claimed but not reachable

- **X2Real** (`2609.27449`, cs.RO; technical report): an evolvable Isaac
  Lab-Arena benchmark with a 10-dimension capability taxonomy, 44 hierarchical
  tasks, ~300 hours of annotated trajectory data, and a reported 0.84 linear
  correlation between simulated and real-robot evaluation. The full text marks
  `[Code] https://github.com/X-Square-Robot/x2real` and a project page, but
  **both return 404** (verified 2026-09-24), so the claim has no access point and
  is treated literally as not reachable. **Watch** — this qualifies immediately
  once the release appears.
- **Banana Kick / RISE** (`2609.27269`, cs.RO): response-informed skill evolution
  that turns an ordinary humanoid kick into a curved kick (11.55 rad/s mean spin,
  19.8% mean evaluation-score gain, 30 motion-capture physical trials) — a clean
  skill-evolution primitive. The only artifact link is the project website
  `https://haozhang-thu.github.io/bananakick/`, which **returns 404**.
  **Watch.**
- **Median Temporal Ensembling** (`2609.27167`, cs.LG/cs.RO): a one-line,
  training-free robust aggregation for action-chunked policies whose breakdown
  point under adversarial corruption is shown to be 0 for the exponential mean;
  the project page `https://avalon-s.github.io/MedianTE/` **returns 404** and no
  code repository was located. **Watch** — a cheap robustness primitive worth
  re-checking.
- **EmbodiedMemory-Bench** (`2609.28236`, cs.CV): 2,554 interactive episodes
  across four task families plus an external memory system (EMem) and an 8B
  policy, directly relevant to this list's memory line. The advertised project
  page `https://zju-omniai.github.io/EmbodiedMemoryBench` returns **404 ("Site
  not found")** and no repository was located. **Watch.**
- **SkillGym-Agent dataset** (`2609.27717`) — the companion dataset is gated (API
  returns an authentication error) while the environment/trajectory dataset is
  public; recorded as partially open in the entry.

### Verified and rejected — no artifact, out of scope, or both

- **InternW0** (`2609.27656`, cs.AI/cs.RO; technical report, 24 pages): Shanghai
  AI Laboratory's physical world model with asynchronous multi-frequency
  processing, ~7,200 hours of heterogeneous robot and egocentric training data
  (including a 275-hour EgoLab dataset), and an asymmetric video–action
  architecture with flow matching. Substantial, but the project page links only
  the arXiv record — **no code, weights, or data release** — so it is recorded as
  a watch item rather than an entry (`internrobotics.github.io/internw0` is live
  with video demonstrations only).
- **PaMER** (`2609.27286`) and **StateComp** (`2609.27298`, both cs.AI, 35/33
  pages): hidden-state memory-control signals and state-conditioned compression
  for long-horizon agents on WorkBuddyBench, with 52.27% token reduction reported
  by StateComp. Both are digital-agent context-management results with **no
  artifacts located**, and the list already carries the local context-management
  line (CliffCompaction, FIRE, R3Con); excluded this round, **watch** for code.
- **State-Grounded Conditioning** (`2609.27606`, cs.AI; EACL 2027 Industry Track
  submission): externalizing state-dependent control into rule kernels and three
  wrappers, with grounded accuracy rising from 61.1%/69.8% to 96.7%. The design
  principle is harness-shaped but the domain is in-game conversational coaching,
  the evidence is a 200-session anonymized benchmark, and **no artifact was
  located**; excluded as domain-specific.
- **DUGM-R** (`2609.27338`, cs.RO): an uncertainty-aware dynamic grid map plus a
  **risk-triggered recovery policy** trained after the nominal policy is frozen,
  deployed on a TurtleBot3 without fine-tuning. Genuinely on the recovery line,
  but the contribution is a representation plus a trigger with **no artifact
  located**, and the evaluation is one simulation benchmark plus one platform;
  excluded this round, **watch**.
- **Turning Safety into Competence (S2C)** (`2609.27312`, cs.AI/cs.LG/cs.RO):
  two-stage safety-filter RL with a non-exploitability proof, simulated
  touchdown games against eight baselines, and hardware stress tests against a
  human. No artifact located and the contract is a training recipe rather than a
  runtime interface; excluded, **watch**.
- **Talk2Escape** (`2609.28296`, cs.HC/cs.RO; IROS 2026): a model-agnostic
  dialogue intervention that monitors kinematics for looping or divergence and
  solicits corrective feedback — a real recovery contract with sim-to-real
  validation on a Unitree Go2. The project page is live but **no code was
  located**; excluded this round, **watch** for the release.
- **PointCast** (`2609.28393`, cs.RO) and **LiMA** (`2609.28431`, cs.RO): two
  world-model contributions with live project pages
  (`pointcast-wm.github.io`, `ccdcs.github.io/LiMA_repo/`) and no released
  code or weights located. Both join the standing WAM watch list.
- **Generalizable Robotic Insertion with World Models** (`2609.28258`, cs.CV/
  cs.LG/cs.RO; IROS 2026): one world model trained across up to 90 insertion
  tasks reaching 56% zero-shot success on unseen objects against 7% for a
  model-free baseline. A model contribution with **no artifact located**;
  excluded, consistent with the standing policy for the world-model line.
- **Marginally Correct Tool Caches Can Reverse Group-Normalized Policy Updates**
  (`2609.26866`, cs.LG): shows that sharing one stochastic tool result per group
  can reverse the expected group-normalized update despite marginal agreement,
  with an exact finite-group expression, a wrong-direction region, and an
  implementation audit in a pinned TVCache stack (MIT code at
  `shi1720/tool-cache-coupling` is live). Harness-relevant as a correctness
  warning for tool-result caching in agent RL, but it is an estimator-level
  theory result on a two-action model rather than a harness interface or an
  evaluation contract; excluded, noted here so later runs do not re-litigate it.
- **TimeEvo** (`2609.27277`), **EnSIMem** (`2609.27279`), **Shadow Memory**
  (`2605.03228`), **EnSIMem-style entity memory**, and the other memory-, tool-,
  and cache-management records surfaced by the second pass: each is either a
  digital-agent result with no located artifact or a narrow mechanism outside
  this list's harness contract. The memory line is now tracked through
  MemBodied, the Counterfactual Memory Audit, and the existing
  memory-benchmark entries.
- **`2609.27070` The Gaussian Is Enough**, **`2609.28161` Dissecting
  Advantage-Guided Post-Training**, **`2609.27513` Behavior-Aligned Action
  Tokenization**, **`2609.27068` HiRE**, **`2609.28339`**, **`2609.28107`**,
  **`2609.28131`**: VLA training-recipe and representation results with no
  artifacts located; excluded as training-recipe studies unless they supply a
  runtime interface or evaluation contract.
- **`2609.27095` Intelligence Across Embodiments** (ISRR 2026), **`2609.27536`
  Behaviora**, **`2609.27475` RoboCafé in the Open** (ICRA 2027 submission):
  position, architecture-sketch, and long-term HRI deployment papers. Each is
  interesting for the landscape notes but supplies neither a released artifact
  nor a measurable harness contract; excluded (RoboCafé's four interaction-
  continuity requirements are noted for the HRI line).
- **Conventional perception, control, navigation, medical, agricultural, and
  driving records** in the block (including `2609.28225` UAV map localization,
  `2609.27219` tether-suspended sensing, `2609.27442` SatUnreal, `2609.28064`
  SlackDrive, `2609.28317` voice activity projection, and the tactile/haptic
  entries): excluded per the standing scope policy unless they introduce a
  reusable agent/VLA harness, recovery, safety, or evaluation contract.
- **`2609.26927` socio-affective multi-agent simulation**, **`2609.27632`
  regulated-finance multi-agent infrastructure**, **`2609.28247` controlling
  collectives**, and similar non-robotic agent-population papers: excluded as
  outside scope.

## Current Landscape additions

Five bullets added to `README.md`:

1. Harness guarantees are being written as proofs and shipped with the
   instrument that checks them (Bounded Loops; Agile-V Assurance Spine).
2. The minimal-harness thesis is being tested against specialized systems, and
   context representation is being priced in model scale (JAZ; R3Con).
3. Robot agent harnesses now separate proposal, verification, and commitment
   explicitly (RegenHarness's role-isolated contexts, versioned memory, and
   commit gate).
4. Coding agents are being turned into robot supervisors and supervision
   generators (EmbodiedSWE; TANDEM).
5. Memory and world-model artifacts are consolidating into fixed-size state with
   verification paths (MemBodied; Counterfactual Memory Audit; Uranus reversal;
   LeWAM; FoldQuantVLA promotion).

## Validation performed

- `git diff --check` clean (no whitespace errors, no conflict markers).
- Markdown structure re-checked after every insertion: each new entry is a
  single top-level bullet with balanced brackets and parentheses (scripted
  check over all `- [` lines returned zero imbalance); section headings and
  Contents links unchanged.
- Every added link was fetched on 2026-09-24: **42 of 42 added URLs return
  HTTP 200** after excluding the shields.io badge; the full check is recorded in
  `.scratch/arxiv-2026-09-24/link-check.txt`. Repository license, size,
  creation/push dates, and trees were read from the GitHub and Hugging Face
  APIs; project pages and raw READMEs were fetched directly.
- Dates checked for cross-file consistency: the README badge, the
  "Last verified" line, and this record all carry **2026-09-24**.
- Categories checked against the arXiv record for every included entry.
- Cross-file consistency: the Uranus status change is recorded here, in the
  README entry, and against the 2026-09-22/2026-09-23 records that watch-listed
  and then withdrew it; HazardArena's v3 revision is reflected in both this
  record and the updated README entry.

## Operational notes and blockers

- The SSH remote required `GIT_SSH_COMMAND='ssh -F /dev/null'` because of the
  known `/etc/ssh/ssh_config.d` ownership problem; `git ls-remote` with that
  override confirmed `refs/heads/main` at the pre-run SHA.
- `/tmp` is per-command in this sandbox, so harvest XML, dossiers, dumps, and
  link-check output were kept under `.scratch/arxiv-2026-09-24/` (untracked, and
  never staged).
- GitHub REST API rate limits were not reached; the Hugging Face API and OAI-PMH
  endpoint were reachable throughout. No network, authentication, or conflict
  blockers.

## Commit

Task-owned files committed on `main`: `README.md`,
`sources/daily-arxiv-2026-09-24.md`. Unrelated user changes
(`docs/reference-architecture.md`, `docs/ring-harness.png`, `handoff.md`) were
left untouched and unstaged.
