# Daily arXiv scan — 2026-11-08 (the index has not moved, the owed 2026-11-06/07/08 days are still empty, and a required re-screen of the unchanged 2026-11-05 batch recovers six entries plus two artifact verdicts)

## Scope

- **Interval owed.** The previous successful run committed at **`2026-11-07T16:48` local** (`2026-11-07T15:48Z`) on
  `main`, producing `4750f01` and record `sources/daily-arxiv-2026-11-07.md`. That record screened the owed
  **2026-11-05** batch (**433** records) plus **91 records that had arrived late** into 2026-11-03 (3) and
  2026-11-04 (88), and closed with **"Still owed: 2026-11-06 and 2026-11-07. The next run must query both by
  `submittedDate` again — the Friday and Saturday batches should appear in the Sunday announcement — and must
  recount 2026-11-05 and 2026-11-04 before trusting either count."** This run started **2026-11-08 07:47 UTC** on
  `main` at `4750f01`, so the interval it owes is **submission dates 2026-11-06 → 2026-11-08 inclusive**, with
  2026-11-06/07 explicit debt and 2026-11-08 unknown, and a mandatory recount of 2026-11-04/05.

- **Every one of those ranges returns 0, and the emptiness is verifiable rather than assumed.**

  | query (one parenthesised OR-query over cs.RO/cs.AI/cs.CL/cs.CV/cs.LG per day) | 2026-11-04 | 2026-11-05 | 2026-11-06 | 2026-11-07 | 2026-11-08 |
  | --- | ---: | ---: | ---: | ---: | ---: |
  | count at 2026-11-08 07:47 UTC | **487** | **433** | **0** | **0** | **0** |
  | recorded by the previous run | 487 | 433 | 0 | 0 | — |
  | **late arrivals screened by this run** | **0** | **0** | — | — | — |

  The index itself has not advanced: a descending `cs.RO` probe returns still **`2611.08462`** at
  **`2026-11-05T18:53:16Z`** and a descending `cs.AI` probe still **`2611.08483`** at **`2026-11-05T18:59:29Z`** —
  the same two Thursday-evening submissions the previous run saw, with the harvest's newest record
  **`2611.08487`** at **`2026-11-05T18:59:42Z`** still the newest in the batch. 2026-11-05 is a **Thursday**;
  arXiv carries Friday's submissions into the **Sunday 20:00 ET** announcement, which at this host's clock is
  **Monday ~01:00 UTC** — so at 07:47 UTC on Sunday the Friday batch does not exist yet, and no Saturday or
  Sunday batch exists at all. The three days are **unharvestable, not screened**, and the record leaves
  2026-11-06, 2026-11-07 **and now 2026-11-08** owed.

- **Zero late arrivals, for the first time in five runs.** The previous four runs each found that a batch released
  membership after its record was written (3, 88, 120 and 91 records respectively). Recounting here returned
  **487** and **433**, exactly the numbers the previous run recorded, and the full-day re-harvest of 2026-11-05
  was **byte-comparable to the previous run's harvest**: `433` of `433` IDs match after normalising the version
  suffix, and a field-by-field comparison of `title`, `published`, `updated` and `summary` over all 433 records
  reports **zero content changes**. So the batch is unchanged, and it was **re-screened in full** rather than
  re-screened by title only (see Method).

- **Coverage.** Nothing new was harvested, so there is no new category distribution; the 433 records re-screened
  carry the previous run's distribution unchanged. Recounted over the batch: primaries `cs.LG` **112**, `cs.CV`
  **95**, `cs.RO` **65**, `cs.AI` **61**, `cs.CL` **47**, and `cs.RO` appears as primary or cross-list on **76**.
  The previous record's figure of **80** was measured over its **524**-record window rather than over this batch,
  so the two are not interchangeable and are not quoted as if they were. The re-screen covered every record in
  the batch and re-read **18** abstracts in full and **4** full-text HTML renderings.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`. **8 export-API requests**: **five**
  per-day count queries (2026-11-04 → 2026-11-08), **two** descending frontier probes (`cs.RO`, `cs.AI`) and
  **one** full-day harvest at `max_results=2000&sortBy=submittedDate&sortOrder=ascending` returning **433**
  entries (1.06 MB). Spaced 3–5 s; **zero HTTP 429**. The per-day counts use the parenthesised form
  `(cat:cs.RO OR cat:cs.AI OR cat:cs.CL OR cat:cs.CV OR cat:cs.LG) AND submittedDate:[...]`; the unparenthesised
  form still drops the date filter and returns a constant site-wide count, which is how a count query can look
  plausible while answering a different question.
- **Differential diff against the previous harvest.** The untracked `.scratch/arxiv-2026-11-07/` directory is
  still on disk, so `per_day.json` from that run was diffed against today's parse of `day_20261105.xml` as a
  **set difference over parsed XML** (not a comparison of prose): 433 vs 433 IDs, **0 added**, **0 removed**,
  **0 changed**. The raw XML files differ at byte 466 because the feed's own `<id>` request token and `<updated>`
  stamp change per request — the records do not.
- **Screening (the required re-screen).** With no new material, the whole newest visible batch was re-read:
  (a) all **433** titles in ascending ID order; (b) **18 abstracts** in full for titles carrying a
  harness/evaluation/monitoring/robot-agent signal that the previous pass had not recorded a decision on — the
  previous record's rejected list names its rejections explicitly, which makes the *unscreened* remainder
  identifiable rather than guessed; (c) **4 full-text HTML renderings** (`arxiv.org/html/<id>v1`) for artifact
  claims and scope statements (`2611.07461`, `2611.08269`, `2611.08286`, `2611.08452`); (d) every claimed
  artifact resolved at the source: **5 GitHub repository metadata reads**, **1 user repository listing**,
  **1 GitHub search**, **3 repository trees**, **2 raw READMEs**, **1 Hugging Face collection API read**,
  **4 project-page fetches (one a stub and its redirect)**, and **1 plain-HTTP 404 confirmation**.
- **`/tmp` is still not durable between tool calls**, reproducing every previous run's finding. All XML, HTML,
  JSON and page captures are in the untracked `.scratch/arxiv-2026-11-08/` directory, which is **not staged and
  not committed**.

## Included (6)

The previous run's first pass over this batch was a title-first pass with 32 abstracts and 11 full texts read;
these six records sat outside that reading and each meets the standing bar (a substantial paper, an important
interface or evaluation contribution, or a real artifact release). Every one is new to the repository: before
this run's edits, a grep of `README.md`, `docs/*.md` and `sources/*.md` returned **no match for any of the six
IDs**.

### General Harness Methodology and Verification (2)

- **`2611.07461` — HEELO: Agent-Native Primitives as Reusable Building Blocks for the Last Mile** (cs.CL, cs.AI,
  cs.LG, cs.MA, cs.SE; 102 pages, 21 figures, 54 tables). The harness paper of the batch, and the reason a
  title-first pass loses it: the title says "last mile to the real world", while the contribution is a **harness
  engineering framework** — *Harness Engineering around Evolving Agent-Native Primitives* — whose unit is a
  resource plus a resident LLM that applies it, composed per task into a graph, verified against environment
  evidence, revised where a failure implicates a primitive, and admitted only after validation. Reported gains:
  Terminal-Bench 4.0 **37.3% → 55.5%** on the same backbone, Terminal-Bench 2.0 Pass@1 **60.0% → 72.2%**,
  ToolBench **55.8% → 75.1%**, GAIA-Text-103 **45.6% → 58.3%**; on the **46** real-world tasks of the heelo.dev
  platform Claude Code completes **9 → 39** and Codex **9 → 35** through an MCP server, at **1.9×/2.3×** less
  time and **1.55×** lower cost per completed task. Digital-agent evidence only; no robot experiment;
  author-reported. **Artifact status literal (verified 2026-11-08): the project page is a live commercial
  product**, not a research release — `heelo.dev` sells API keys and MCP access (`/pricing`, `/dashboard/keys`,
  `hl_demo_…` key UI), the paper links no repository, and the primitive library, the 46-task protocol and its
  pass criteria are **not** released as code, data or weights. Placed in *General Harness Design and
  Self-Improvement*.
- **`2611.08269` — Accepted but Wrong: Silent Failures of LLM Code Repairs after API Migrations** (cs.SE, cs.AI).
  Measures a **release gate by what it releases**, which is the same question this list asks of runtime monitors
  and verifiers. Preregistered over **73 tasks / 60 documented changes** in CPython 3.9→3.12, Node.js 20→24 and
  SQLite 3.51→3.54, executed on the real runtimes against withheld contracts: the all-assertions gate on typical
  inputs released **0.410** of wrong repairs; **one** edge input written from the release note cut it to
  **0.065** (**0.061** on 15 unseen clusters); three blind-writer inputs reached **0.070**. On the harder frozen
  menu (**1,491 fragments**, then **233** distractors that match the legacy output on typical inputs) the edge
  input (**0.058**) and the frontier writer (**0.058**) pass their preregistered tests while **0.168** and
  **0.153** of hard wrong programs still pass. The stated scope is part of the result: repairs are **choices
  among documented fragments**, and the free-form arm that would have executed model-generated code **was not
  run** because its required sandbox could not be built on the study machine. **Artifact status literal
  (verified 2026-11-08): not open, with an explicit reason** — the package (task set, visible oracles, hidden
  contracts, pinned release notes, prompts, every model call, run receipts, freeze hashes, scripts) is
  "available from the authors on request" and explicitly "not posted on a public code host", because the hidden
  contracts are the benchmark's evaluation key and the records carry redistribution-uncleared commercial-model
  responses. Digital code-repair evidence; no robot experiment. Placed in *General Harness Design and
  Self-Improvement*.

### Evaluation (2)

- **`2611.08352` — USB Gym: A Gymnasium Testbed for Closed-Loop Ultrasound View Matching** (cs.CV, cs.LG,
  cs.RO; 75 pages + 62 supplementary; submitted to *Computer Methods and Programs in Biomedicine*). An
  evaluation testbed whose **slot interface is the contract**: three replaceable parts (organ segmenter,
  controller, reward), **20 published methods** each run on the **same 60 episodes of six held-out patients**,
  replacement only where a published method is detectably better on those patients, and the better combination
  compared on 20 new CTs against a **pose oracle** that reads the exact probe pose available only in simulation.
  The result is reported as a negative one: probe starts **16.0 mm** from the target view, the oracle reaches
  **1.5 mm**, and the best combination of published parts ends at **13.7 mm** — **16%** of the oracle's
  progress — with only an SSIM appearance reward detectably better. **Artifact verified open (2026-11-08):
  Apache-2.0 `tohnperfect/USBGym`** (4.1 MB, created 2026-11-04, pushed 2026-11-06) with a leaderboard, a
  `python -m usbg.contracts_bridge` contract check, one-clone install, tests and an `Errata` section. The
  artifact states the scope the paper relies on: **simulation only, no physical probe, phantom or robot**, rigid
  tissue model, the robot arm is an illustration, no clinical validity claimed. Placed in *Unified Evaluation*.
- **`2611.08286` — Interactive Visual Gym: Evaluating Visual Interaction Models with Simulated Users in
  Interactive Worlds** (cs.CV, 39 pages). Makes the **user part of the closed loop**, on the argument that an
  assistant supporting an ongoing physical task changes what the user does next and offline replay cannot
  capture that. Three simulators (cooking, household manipulation, mobile interface), a **persona-conditioned
  user-simulation harness** with controlled execution errors and out-of-plan behaviour, and **IVG-Bench** at
  **315** scenarios; the strongest model, Gemini 4 Argon, completes **53.4%** of tasks in the time limit, is
  weakest on factual grounding and actionable guidance, and is **often too slow to prevent the error it is
  describing** once latency counts. Validated against people at **78.5%** agreement between the
  interaction-quality metric and participant preference, and used as a training signal in a GRPO proof of
  concept (Qwen3.5-4B, cooking). Vision-language evidence, not a robot policy. **Artifact verified open
  (2026-11-08): MIT `SalesforceAIResearch/Interactive-Visual-Gym`** (30.2 MB, created 2026-10-20, pushed
  2026-11-06; 693 files with `cooksim/benchmark/` case, ledger and error-inventory files and release-check,
  submission-validation and leaderboard-export workflows). Placed in *Unified Evaluation*.

### Agentic Robot Harness (1)

- **`2611.08075` — Leave It Better Than Found: Value-Guided Cooperation in Sequential Tasks** (cs.RO; CoRL 2026
  Spotlight). The handoff between robots that never trained together, made an **explicit transmitted contract**:
  each robot transmits its **value function over environment states**, so a follower's preferences reach a leader
  that co-training cannot pair with, and a **k-NN uncertainty filter** gates the exchange because a value on an
  out-of-distribution state is overconfidence. The authors report up to **48.5** percentage points over naive or
  selfish agents in simulation and on real robots. **Artifact verified open (2026-11-08): MIT
  `proroklab/PAVE`** (22.9 MB, 475 files, created 2026-10-11, pushed 2026-11-06) with household and mobile-arm
  `configs/` (planner, guidance, cost, training, deployment) and a live project page carrying Paper, arXiv and
  Code links. Real-robot plus simulation; author-reported. Placed in *Agentic Robot and VLA Harnesses*.

### VLA (1)

- **`2611.07513` — Concertina: Embodying Manipulation and Cognition in One Diffusion Vision-Language Model**
  (cs.CV, cs.AI, cs.RO). Removes the external action mechanism rather than tuning it: continuous latent action
  token per control timestep on the shared diffusion backbone, **0.21%** of the 4.46B model's parameters
  action-specific against **16.5%–35.1%** dedicated action-expert capacity in comparable VLAs, plus an **Action
  Interface Audit** that locates cross-time coupling in the middle and late layers of the backbone rather than at
  the action interfaces. Reported **98.15%** LIBERO, **85.9%** LIBERO-Plus (10,030 tasks), **4.142** CALVIN
  ABC→D. **Artifact verified open (2026-11-08): MIT `lose4578/Concertina`** (667 KB, 150 files, created
  2026-11-02, pushed 2026-11-05), a **LeRobot policy plugin at a pinned commit** with the model, the paper's
  pretraining/fine-tuning configs, the CALVIN and LIBERO evaluations and a `docs/reproduction.md`; three
  **ungated** checkpoints (`Traly/concertina-pretrain-90k`, `-libero`, `-calvin`). The README separates the
  paper's numbers from its **own re-evaluation of the released checkpoints** — **97.80%** LIBERO, **4.153**
  CALVIN average length, described as within evaluation noise — and reports a fine-tuning re-run at **97.45%**.
  Placed in *Vision-Language-Action Models → Open and Reproducible*.

## Rejected and watch list

- **Watch list, with the artifact state recorded literally rather than as "unavailable":**
  **`2611.07639` Marginal Value of Verifier-Based RL** — a preregistered measurement with a written, pre-run
  protocol amendment (SFT 0.785 already nearly solved the benchmark; GRPO from base did not move it), a
  controlled 1,877-task verifier-graded benchmark and a total compute cost of **$8.69**, reporting **+0.22
  points** in-distribution (95% CI [-0.06, +0.51]) and **+0.33** under shift. It is genuinely on this list's
  measurement thread, but its only artifact claim is the comment's **"Code and artifacts:
  https://github.com/saloni-garg/gpu-train-debug"**, and that repository **returns 404** (the account exists
  with 85 public repositories; none of them is `gpu-train-debug`; a GitHub search returns two unrelated
  repositories). Recorded as an **unresolved artifact claim**, not as open.
  **`2611.08452` ReproBridge** — reproduces ML research artifacts through a refined artifact design
  (codebase, environment, launchers, reproduction guide) and an Auditor, raising mean reproduction success
  **76.4% → 92.6%** across four models over 1,029 tasks/387 experiments/36 papers. Kept on the watch list
  because the project page states **"Code — In review"** and **"Dataset — In review"** with an explicit
  "will be open-sourced" promise: a promise, not an artifact.
  **`2611.07367` Test-Time Self-Play** (no-verifier test-time scaling, **+35% absolute** mean@8 in the
  no-verifier limit — a decoding/scaling method, not a harness-owned interface; no artifact claimed).
  **`2611.07286` How Should Agents See Code?** (code-representation workflows as a harness design variable:
  recall@10 **0.717 → 0.757–0.829** and **25–39%** fewer tokens on LinuxFLBench while SWE-bench Pro resolution
  stays near baseline — a real interface-design measurement, but no artifact and no robot link).
  **`2611.07502` Coverage Hides Churn** (WACV 2027; coverage is unchanged (**≤0.01**) while **60–67%** of images
  change decision — excellent instrument-audit reasoning, digital VLM label registries, no artifact claimed).
  **`2611.07650` LLM API relays** (four of twenty relays return tool calls after permissions were disabled;
  five report 4,200–60,000 extra input tokens — harness-adjacent but a service-audit study).
- **Robot/embodied candidates re-read and rejected on scope** (models, controllers, perception or application
  results without an external runtime/interface contract this list would own): **`2611.08308` RM-VLM**
  (reward machines from language with a VLM labelling function; no artifact), **`2611.07825`
  Generate-then-Rank** (goal interpretation improved inside the Embodied Agent Interface benchmark; a model-side
  improvement to an existing harness), **`2611.08100` integrated ROS 2 rover stack** (honest field-integration
  reporting, but a conventional navigation stack rather than an agent/recovery/evaluation contract),
  **`2611.07876`** execution-free correctness predictors, **`2611.08341`** agent-memory quantization,
  **`2611.07953`** the loss-of-control behavioural framework (29 evaluation tasks, r = 0.84 prospective
  validity — strong safety content, no artifact, and frontier-model rather than robotic evidence), and the
  remainder of the 80 cs.RO-touching records already rejected by the previous run for unchanged reasons
  (GraspBench, dynamic watermarking, kinematic hypothesis context, Embodied RAG++, open-vocabulary search with
  absence detection, Spatial-UMI-Dream, La-JEPA, SLA, T3R, GPWM, RoboImagine, fractal impedance control,
  VisionSIM, GuidedWAM).
- **Re-confirmed exclusions from the previous run are not re-litigated.** The 14 inclusions and the
  Open-HumanScale → Open-HumanWAM revision handling in `sources/daily-arxiv-2026-11-07.md` stand as written;
  this run re-read the batch to find **what was missed**, not to re-open decisions that were taken on evidence.

## Validation performed

- **Markdown structure.** `README.md` renders as valid Markdown: the six new entries are single top-level
  bullets inside their existing sections, no nested list is broken, the `###` heading count and order are
  unchanged (**22** headings), and the table of contents anchors are unchanged. The new record has one `#`
  heading and uses only `##`/`###` below it.
- **`git diff --check`** reports no whitespace errors or conflict markers (see the run log in the completion
  report).
- **Links.** All **13** non-duplicate links added to `README.md` were resolved from this host on 2026-11-08:
  `arxiv.org/abs/{2611.07461,2611.08269,2611.08352,2611.08286,2611.08075,2611.07513}` (200), `heelo.dev` (200,
  commercial product page), `proroklab.github.io/PAVE/` (200 after a `website/` hop), and the four repositories
  `tohnperfect/USBGym` (Apache-2.0), `SalesforceAIResearch/Interactive-Visual-Gym` (MIT), `proroklab/PAVE`
  (MIT), `lose4578/Concertina` (MIT), plus `huggingface.co/collections/Traly/concertina` (200, three ungated
  model repositories).
- **Duplicate IDs.** All six new IDs were checked against `README.md`, `docs/*.md` and `sources/*.md` before
  insertion and **none was present**. After the edit, **54 of the 433** batch IDs are named somewhere in the
  repository — the previous record's inclusions and rejections plus these six — and each new ID occurs in exactly
  one entry and, where a landscape bullet cites it by arXiv URL, once more there.
- **Dates and categories.** Every included entry states its arXiv ID, and each `<published>` date is
  **2026-11-05** — inside the batch, and no entry cites 2026-11-06/07/08 because no such record exists. Category
  placement follows the contribution rather than the arXiv primary: `2611.07461` (cs.CL primary) and
  `2611.08269` (cs.SE) are general harness methodology, `2611.08352` (cs.CV primary, cs.RO cross-list) and
  `2611.08286` sit under *Unified Evaluation*, `2611.08075` under *Agentic Robot and VLA Harnesses*, and
  `2611.07513` under *VLA → Open and Reproducible*.
- **Cross-file consistency.** The README badge, the `## Current Landscape` `Last verified:` line and the
  Contents blurb now read **2026-11-08**, matching this record's filename. `docs/landscape.md` keeps its own
  earlier verification date because it is a historical note rather than a maintenance surface.
- **Artifact-status discipline.** Each of the six entries states one literal status: **verified open with a
  licence** (`USBGym` Apache-2.0, `Interactive-Visual-Gym` MIT, `PAVE` MIT, `Concertina` MIT with three ungated
  checkpoints), **commercial product with nothing released** (HEELO), and **on request, explicitly withheld**
  (Accepted but Wrong). Two watch-list items carry **404** (`gpu-train-debug`) and **"in review"** (ReproBridge)
  rather than being described as open.
- **Author-versus-independent separation.** Every number in all six entries is marked as what the authors
  report; no paper in this batch has an independent reproduction. Evidence scope is stated per entry:
  digital-agent-only for `2611.07461` and `2611.08269`; simulation-only for `2611.08352`; interactive
  VLM/user-simulation for `2611.08286`; simulation plus real robots for `2611.08075`; and standard VLA
  simulation benchmarks plus a released-checkpoint re-evaluation for `2611.07513`. One entry carries an
  author-measured discrepancy rather than a single number: Concertina's release reports **97.80%** against the
  paper's **98.15%**, and the README quotes the released figure alongside it.

## Operational notes and blockers

- **A run with no new material still has work in it, and the rule that produces that work is worth stating.**
  The standing instruction is to re-screen the whole newest visible batch when it is unchanged from the previous
  run. Six entries came out of it. The mechanism is not mysterious: the previous pass screened **32 abstracts and
  11 full texts** over 524 records, which is a title-level filter for most of the batch, and three of the six
  recovered records announce their contribution in the abstract rather than the title (`2611.07461` is a
  harness-engineering framework titled "the last mile to the real world"; `2611.08352` is a slot-interface
  evaluation contract titled "a Gymnasium testbed"; `2611.08269` is a release-gate measurement titled "silent
  failures"). A title-first pass is the right first pass and the wrong last one.
- **"Unchanged" is now checkable cheaply, and should be recorded as a measurement.** Re-harvesting the same day
  and diffing parsed records took seconds because the previous run's `.scratch/` directory persists: 433 vs 433
  IDs and zero field changes is a stronger statement than "the count is the same", and it is what licenses the
  re-screen. The raw XML checksums differ every time (request token, feed `<updated>`), so a checksum comparison
  alone would have reported a false change.
- **An artifact claim can be a 404 in the comment field rather than in the abstract.** `2611.07639`'s abstract
  says only that the study "is fully reproducible from archived generations, frozen configs, and tagged code";
  the dead link lives in the arXiv comment. Both places have to be read before an artifact verdict is written.
- **A commercial project page is not a research release, and the difference is visible in the page's own
  affordances.** `heelo.dev` returns 200 and is linked from the paper as its website, but it is a product site
  (pricing, dashboard keys, demo API key, MCP onboarding). Recording it as "project page live" without the
  product reading would have implied a released research artifact that does not exist.
- **Weekend lag again explains three empty days at once.** The previous run predicted that Friday's batch would
  appear in the Sunday announcement; at 07:47 UTC on Sunday that announcement has not happened yet (Sunday
  20:00 ET ≈ Monday 01:00 UTC), so 2026-11-06, 2026-11-07 and 2026-11-08 are all unharvestable. The next run must
  query all three again, and — because the Monday catch-up is the first slot after the announcement — should
  expect a large single block.
- **`/tmp` is still non-durable across tool calls**, confirmed again; `.scratch/arxiv-2026-11-08/` holds every
  response, the day harvest, the four full-text renderings, the GitHub/Hugging Face JSON and the parsed records.
- **OpenSSH workaround still required.** `git fetch`/`git ls-remote`/`git push` fail with a "Bad owner or
  permissions on /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf" error; `GIT_SSH_COMMAND='ssh -F /dev/null'`
  restores access with the user's key and `known_hosts` unaffected.
- **Still owed: 2026-11-06, 2026-11-07 and 2026-11-08.** The next run must query all three by `submittedDate`
  again and **must recount 2026-11-05 and 2026-11-04** before trusting either count.
