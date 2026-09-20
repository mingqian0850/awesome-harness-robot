# Daily arXiv scan — 2026-09-20

## Scope

- **Interval:** everything announced after the 2026-09-19 run's cutoff. That run
  (started ~17:25 UTC on Saturday 2026-09-19) screened the announcement block
  whose OAI datestamp is **2026-09-18** — 907 unique base IDs, newest base ID
  `2609.20822`. This run started ~16:00 UTC on **Sunday 2026-09-20** and covers
  everything announced since.
- **No new announcement block landed — for the second consecutive day.** The OAI
  harvest for the datestamp window `from=2026-09-19&until=2026-09-20` (sets
  `cs:cs:RO|AI|CL|CV|LG`) returned `noRecordsMatch` for **all five categories**.
  Re-harvesting the wider window `from=2026-09-18&until=2026-09-20` returns the
  same single-datestamp block: **1,235 memberships = 907 unique base IDs, every
  one carrying datestamp 2026-09-18 — zero records carry 2026-09-19 or
  2026-09-20.** Per-category memberships are unchanged: cs.AI 350, cs.LG 328,
  **cs.RO 195**, cs.CV 191, cs.CL 171 (plus stat.ML 38, cs.CR 37, cs.IR 25,
  cs.HC 23, cs.SE 22, cs.MA 21, cs.SY 21, eess.SY 21, cs.CY 17 among the
  cross-listed sets).
- **This is the required "unchanged batch" case, twice over.** The newest visible
  block is the same one both the 2026-09-18 and 2026-09-19 runs screened, so — as
  the procedure requires — the **entire 907-record block was re-screened once
  more**, with a deliberately different ranking emphasis (artifact-signal first,
  concept breadth second) rather than a repeat of yesterday's pass. The re-screen
  produced **five README additions** that the previous two passes did not carry;
  no earlier verdict was reversed.
- **Delta against the previous run's harvest (exact): 0.** 907 → 907 unique IDs,
  **0 IDs appeared, 0 disappeared, 0 records changed** in datestamp, title,
  abstract, comments, journal-ref, DOI, created date, or category set. The block
  is byte-stable across the two runs.
- **ID frontier unchanged.** Newest base ID in the block is still **`2609.20822`**.
  A direct existence probe confirms `2609.20823` exists (`quant-ph`, outside the
  five target categories) while `2609.20824`, `2609.20830`, and `2609.20900` all
  return **HTTP 404**. No IDs have been assigned past the frontier, so nothing is
  being held back by the harvest.
- **Composition of the re-screened block (907 unique IDs), by the submission date
  of the announced version** (OAI `<created>`): **615 carry 2026-09-17**, **156
  carry 2026-09-16**, **9 carry 2026-09-15**, and the rest reach back to 2018;
  **254 records carry pre-`2609.` IDs**, the signature of re-announced
  replacement versions (13 created 2026-07-29 and 12 on 2026-07-28 among them).
- **Export-API cross-check.** The `submittedDate` windows for **2026-09-19 and
  2026-09-20** return `opensearch:totalResults` **0 for all five categories**
  (cs.RO, cs.AI, cs.CL, cs.CV, cs.LG): the weekend's submissions have not been
  announced or indexed yet. A descending `submittedDate` probe per category shows
  the newest indexed entries are still the 2026-09-17 submissions — `2609.20822`
  (cs.RO and cs.AI), `2609.20821` (cs.LG), and `2609.20804` among them — i.e. the
  same frontier, viewed from the other direction. This confirms the block boundary
  rather than a truncation.
- **Weekend lag, consistent with the repository's own history.** arXiv announces
  Sunday–Thursday evenings (US Eastern); the last announcement produced the
  2026-09-18 datestamp, and the next genuinely new block is expected with the
  Sunday-night announcement (Monday 2026-09-21 UTC). The 2026-09-12, 2026-08-30,
  and 2026-09-07 records document the same Friday/weekend behaviour.
- **Late-batch re-check:** the previous day's range was re-harvested inside the
  wider window above, and the frontier probe shows no newer IDs exist; there is
  nothing further to re-check.
- **Withdrawals in this block:** five records carry a withdrawal notice
  (`2604.25323`, `2606.01670`, `2608.04765`, `2609.19422`, `2609.20217`) — the
  same set the two previous runs recorded, and **none of the five appears
  anywhere in `README.md` or `docs/`**. **No curated entry is affected by a
  withdrawal.**

## Method

1. **OAI-PMH harvest** (`oaipmh.arxiv.org`, `metadataPrefix=arXiv`, sets
   `cs:cs:RO|AI|CL|CV|LG`) for two windows: `from=2026-09-19&until=2026-09-20`
   (empty — `noRecordsMatch` for every category) and
   `from=2026-09-18&until=2026-09-20` (1,235 memberships). Pages saved to
   `.scratch/arxiv-2026-09-20/oai-*.xml` and `oai-full-*.xml`.
2. **Parse** to unique base IDs with datestamp, `<created>`, categories, sets,
   title, abstract, comments, journal-ref, DOI, and authors
   (`.scratch/arxiv-2026-09-20/oai-records.json`).
3. **Delta against the previous run's parsed harvest**
   (`.scratch/arxiv-2026-09-19/oai-records.json`): set difference for
   appeared/disappeared IDs plus field-wise comparison for any content change —
   **all zero**.
4. **Export-API cross-check** over the 09-19 and 09-20 `submittedDate` windows per
   category, plus a descending-date probe, used strictly as a subset/boundary
   check (the API cannot see held, cross-listed, or replacement records).
5. **Full re-screen of the whole 907-record block** with a new ranking emphasis
   (`.scratch/arxiv-2026-09-20/rescreen.py`): artifact-signal language
   (GitHub/Hugging Face/project-page links, "we release", "code available")
   weighted first, concept breadth second, over the **790 block IDs that had never
   been mentioned anywhere in `README.md`, `docs/*.md`, or `sources/*.md`** at
   screening time (785 before this run's five additions). 150 of those novel
   records carry artifact-signal language, and each was read at title/abstract
   level. In addition: every block record containing the word *harness* (21
   records — 17 already curated, 4 novel and all out of scope), every block title
   containing *harness* (8, all already curated), and **all 137 novel cs.RO
   records** were read individually, plus a targeted pass for self-evolution,
   agent-runtime, closed-world, skill-library, and regression-test phrasing.
6. **Dossiers and primary-source verification** for every shortlisted record: full
   title/abstract/comments, then the arXiv abstract page (version history), the
   arXiv full-text HTML (artifact links and release sentences extracted), and the
   official GitHub API (license field, size, creation and push dates, default
   branch, top-level tree) on **2026-09-20**.
7. **Duplicate screening** of every candidate against `README.md`, `docs/*.md`,
   and `sources/*.md` by arXiv ID, project name, and repository URL before
   writing.

## Included (README updates)

Five entries added, plus three "Current Landscape" bullets. Artifact status is
reported literally; every repository claim below was verified live on 2026-09-20.

### General Harness Design and Self-Improvement

- **Closed-World Resolution Against Tool Hallucination in LLM Agents**
  (`2609.19425`, cs.AI/cs.CR/cs.SE, single author) — the tool boundary's
  structural blind spot: a call naming a tool that does not exist was never a
  decision any gate made, so no permission gate can reject it, and defense must
  sit *before* the gate. A five-class taxonomy (H1–H5) separates fabricated tools,
  undeclared arguments, and coercion-detectable calls; the reference **Resolution
  Rung** is a training-free closed-world check (registry membership plus a
  signature check on the emitted call) that composes with any later gating or
  integrity layer; the paper also characterizes the irreducible residue (borrowed
  arguments that are schema-indistinguishable from a valid call) and formalizes
  the ordering dependency — the authors explicitly state the statements are a
  formalization rather than deep theorems. Measured across ten hosted models on
  two invocation surfaces: **322 genuine hallucinations** that a gating-only stack
  executes entirely, with fabricated tools concentrated on the unconstrained
  raw-JSON surface (34 vs. 3) and model scale not helping (a 675B model matching a
  7–8B one). Extended to MCP: merging servers into one namespace creates collision
  and shadowing surfaces a single registry cannot express (taxonomy M1–M5), with
  **154 MCP hallucinations** including from frontier models that were clean on the
  single-registry surface. The paper states it releases the versioned
  **Hallucinated-Tools Benchmark (HTB)** — an installable, deterministic suite
  scoring any resolver on H1–H5 and M1–M5 with a leaderboard — but **no public
  repository or download location could be located on the arXiv record or in the
  full text (verified 2026-09-20)**, so the artifact is treated literally as not
  yet reachable. Digital agents only, no independent reproduction.

### Robot Agent Systems — Planning, Tool Use, and Skill Composition

- **A-RAM: Kinematics-Grounded Agentic AI for Robotic Additive Manufacturing
  Process Planning** (`2609.19347`, cs.AI/cs.RO/cs.SY, 25 pages, 17 figures) — the
  agent–specialist–tool split applied to a six-axis arm used as an
  additive-manufacturing cell, where a slicer plan that looks favourable in part
  coordinates can be kinematically infeasible once part orientation and workspace
  placement are chosen, and existing AM tooling, LLM decision support, and digital
  shadows cannot evaluate those coupled decisions *before execution*. The LLM
  interprets objectives and constraints and emits a **schema-constrained request**
  naming prescribed and searchable planning variables; a deterministic Planning
  Agent instantiates the matching search workflow; and domain tools compute the
  quantitative evidence — slicing, placement, inverse kinematics, trajectory
  timing, Joint-6 jerk, extrusion — used to select candidates. Candidates are kept
  as structured artifacts, so the chain from user intent to G-code, robot motion,
  and extrusion commands stays traceable. The authors report up to **53.5% lower
  maximum and 48.3% lower mean absolute Joint-6 jerk** than the least favourable
  valid candidates across the evaluated candidate sets, and objective-specific
  infill screening cutting motion-plan completion time by up to **40.1%** and
  extrusion path length by up to **12.7%**. Evidence is case studies on one
  workcell, author-reported; **no official code, data, or benchmark release was
  located** in the record or the full text (verified 2026-09-20).

### Vision-Language-Action Models — Open and Reproducible

- **StarVLA-α: Reducing Complexity in Vision-Language-Action Systems**
  (`2604.11757`, cs.AI/cs.CV/cs.RO, **ECCV 2026**; v2 announced 2026-09-16; v1 was
  2026-04-13) — a deliberately minimal baseline (strong VLM backbone plus a
  straightforward action head) used to re-evaluate action modeling strategies,
  robot-specific pretraining, and interface engineering under controlled
  conditions, with one generalist checkpoint trained under a unified multi-benchmark
  recipe over LIBERO, SimplerEnv, RoboTwin, and RoboCasa. The authors report the
  single generalist model **outperforming π0.5 by 20% on the public real-world
  RoboChallenge benchmark**, and argue the result shows minimal design is already
  competitive without extra architectural complexity or engineering tricks.
  Artifact: the official, dedicated **[`starVLA/starVLA-alpha`](https://github.com/starVLA/starVLA-alpha)**
  repository (created 2026-08-04, pushed 2026-08-05, 71 MB; `starVLA/`,
  `deployment/`, `examples/`, `scripts/`, `tests/`, `Makefile`, `pyproject.toml`,
  `CITATION.cff`) with an **MIT `LICENSE`** (the GitHub API reports
  `NOASSERTION` because of additional rebase terms in the file; the license text
  itself is MIT) and the paper as its declared homepage. The parent StarVLA stack
  (`starVLA/starVLA`, default branch `starVLA_dev`, pushed 2026-09-17) is already
  listed in this repository; this entry is the paper's own artifact, not the same
  row twice. Results are author-reported and not independently reproduced.

### Runtime, Safety, and Observability — Safety

- **Runtime Safety Filtering for Two-Terminal Hazards in Robotic Battery
  Recycling** (`2609.19665`, cs.RO) — studies the runtime filter itself instead of
  wrapping one more policy in it. Unsafe states are usually encoded as unions of
  object-wise keep-out regions, which is unnecessarily restrictive when the hazard
  is a **joint spatial relation** — a conductive payload that can short a charged
  cell only by approaching both terminals at once. With the policy frozen
  (OpenVLA), the filter is factored into three separately varied choices:
  predicate structure (conjunctive, conventional two-site keep-out, composite),
  geometric margin, and the fallback applied when a commanded action is rejected.
  The reported headline: the three predicate families trace **nearly identical
  safety–utility frontiers** once each is evaluated over its own margin, while the
  **fallback dominates** — holding reduces task success by up to **0.302** relative
  to retreat without reducing hazard, and the two minimally invasive fallbacks
  leave substantially more residual hazard; the ordering transfers to a second
  policy and task suite (a LIBERO-Spatial checkpoint). Retreat-based filtering
  stays effective under standing clearance errors, but **correlated** error in the
  estimated payload size is more damaging than larger independent errors in
  terminal position. **LIBERO simulation only** (three workcells), author-reported,
  **no artifact located** (verified 2026-09-20). The transferable contract: a
  runtime filter's rejection path must be designed and reported, not just its
  predicate.

### Surveys and Reading Lists

- **Robotic Video World Models: A Survey of Applications, Research Challenges,
  Future Directions** (`2601.07823`, cs.RO/cs.SY/eess.SY; **v2 announced
  2026-09-17**, v1 was 2026-01-12) — reviews video models used as embodied world
  models rather than as video generators: synthetic data generation, policy
  learning, dynamics and reward modelling for RL, policy evaluation, and visual
  planning, with the benchmarks, datasets, and metrics used to score them. Its
  curation-relevant argument is that the capability which removes prohibitive
  simplifying assumptions from physics-based simulation is the same capability that
  produces physics-violating futures, poor instruction following, unsafe content,
  and large data/compute overheads, so the trust boundary decides whether a video
  model can sit inside a robot loop. Artifact: the companion reading list
  **[`irom-princeton/awesome-robotics-video-world-model-papers`](https://github.com/irom-princeton/awesome-robotics-video-world-model-papers)**
  (MIT; 44 KB README organized into foundations, world models, robotics
  applications, autonomous driving, self-supervised representations, affordances,
  benchmarks, uncertainty, and safety; created and pushed **2026-09-19**, verified
  live 2026-09-20). No code, weights, or dataset is released, as expected for a
  survey.

### Current Landscape additions

Three bullets: **tool-boundary resolution separated from the permission gate**
(Closed-World Resolution — resolution must precede gating, and MCP namespace
merging creates surfaces a single registry cannot express); **the runtime filter's
rejection path as the design variable** (Runtime Safety Filtering — fallback
dominates predicate structure at matched margins, simulation-only); and **open VLA
baselines re-established by removing complexity** (StarVLA-α — one minimal recipe
across four simulation suites plus a real-world RoboChallenge claim, MIT code
live). The A-RAM agent–specialist–tool pattern is carried by its entry.

## Rejected / watch list

### Re-screen of the unchanged block (third pass)

- **Harness-titled and harness-mentioning records:** 8 block titles contain
  *harness* and all 8 are existing README entries (`2609.19413`, `2609.19974`,
  `2609.20474`, `2609.20519`, `2609.20804`, `2609.20822`, plus `2609.15188` and
  `2609.18620`, both previously excluded as a narrative-generation "harness" and a
  physics solver respectively). Of the 21 block records mentioning *harness*
  anywhere, 17 are curated; the four novel ones are out of scope —
  `2511.06673` (soft pneumatic actuators; "harness" is a physical harness),
  `2604.25154` (tabular-data cleaning), `2609.07605` (below), and `2609.15964`
  (claim-level citation evaluation in clinical QA). No harness record was missed.
- **Everything else in the block:** the 2026-09-18 and 2026-09-19 records' per-item
  rejected/watch verdicts remain authoritative for the 907 IDs they screened, and
  this third-pass re-screen changed none of them.

### Candidates newly surfaced by the re-screen, verified and rejected

Each was read in full and checked against the arXiv record, the full-text HTML,
and (where a link exists) the GitHub API on 2026-09-20.

- **The Missing Complement / SERBench** (`2609.20050`, cs.AI/cs.CL/cs.IR): 500
  held-out captured agent states from 45 repositories, crediting only evidence
  *sets* that cover every fact the current decision requires (73.0% complete sets
  at five items vs. 61.4% for embedding+reranking). A real, live artifact
  (`LordTARN1SHED/SERBench`, 71 MB, created 2026-09-16, pushed 2026-09-18, no
  license asserted), but the contribution is context/evidence acquisition for
  coding agents, not a harness runtime contract. Excluded; **watch** if the
  evidence-sufficiency contract is generalized beyond code retrieval.
- **Not All AI Agents Are Equal: Characterizing Resource and Performance
  Dynamics** (`2609.19947`, cs.AI/cs.PF): characterizes how latency, CPU, disk
  I/O, and memory inter-mix across concurrent agent requests, finds that faster
  LLM responses or more cores do not always accelerate an agent, and demonstrates
  CPU-aware tool admission and task-aware CPU allocation (~5.4× latency
  improvement on CPU-sensitive tasks, ~32% average across tasks). Genuinely a
  scheduler/admission-control result, but digital-agent serving measurement with
  **no artifact located**, and the robot-side serving line is already carried by
  Robion in Runtime Building Blocks. Excluded; **watch**.
- **Search-to-World** (`2609.07605`, cs.CV): end-to-end evaluation of agentic
  3D-world delivery (Observed Retrieval Rate vs. World Delivery Rate) plus
  WorldSearcher, a reuse-then-reconstruction harness with a structured recovery
  controller that revises temporal grounding, replaces source videos, or
  reformulates queries. The artifact is real and MIT-licensed
  (`night-killer/Search-to-World`, created and pushed 2026-09-15, project page
  live), and the recovery-controller pattern is transferable — but the domain is
  retrieving or reconstructing *3D content from the live web*, with no robot or
  embodied component. Excluded; **watch** if the delivery contract is applied to
  robot world models.
- **PersistBench / Can 4D Foundation Models Remember?** (`2609.20819`, cs.CV,
  NeurIPS 2026 submission): 360°-video-grounded benchmark and metric suite for
  object permanence, motion continuity, and appearance preservation in 4D
  foundation models. The abstract says "dataset and code are available on the
  project page", but the project page's `[code]` and `[data]` entries are
  **disabled placeholders** (`aria-disabled="true"`, title "Code link coming
  soon" / "Data link coming soon"), verified 2026-09-20 — treated literally as not
  open. **Watch for the release**; the evaluation target (memory that survives an
  object leaving the field of view) is directly relevant to world-model fidelity.
- **SLAMSqueezeBench** (`2609.19533`, cs.RO): tests SLAM systems under imposed
  compute/memory limits and simulated frame drops, comparing nine systems. The
  framework "will be available for use by the community upon publication", so it
  is **not open**, and conventional SLAM benchmarking is outside the standing
  scope unless it introduces a harness contract. Excluded; **watch for release**.
- **From Intent to Action: Benchmarking LLM Safety in Vehicle Voice Command
  Authorization** (`2609.19630`, cs.AI/cs.CL/cs.RO): a 202-scenario,
  seven-class pre-action decision benchmark (execute, refuse, clarify, confirm,
  defer to manual, emergency response, no tool call) across speaker role,
  authentication, vehicle state, and tool availability; reports two-to-three False
  Executes even for the best API models and concludes that structured LLM
  decisions need an independent enforcement layer. The pre-action authorization
  framing is harness-relevant, but the benchmark "will be released ... upon
  acceptance" — **not open** — and the domain is automotive voice control rather
  than a robot harness. Excluded; **watch**.
- **AURORA** (`2609.19527`, cs.AI/cs.RO): natural-language-driven agentic
  framework for air-ground co-simulation that compiles scenarios into a typed
  Air-Ground Scenario Graph and adds simulator-grounded parsing, pre-execution
  feasibility checking, trace-based runtime verification, failure localization,
  and bounded repair, with an AURORA-Bench. Verification-and-repair shape is
  interesting, but the substrate is transportation co-simulation, **no artifact
  was located**, and the standing policy excludes generic driving/transportation
  work unless it contributes a reusable agent/VLA harness. Excluded.
- **Execution-Aware Pre-Execution Ranking for Grasp-Conditioned Robotic
  Placement** (`2609.19946`, cs.RO): a learned pre-execution ranking model over
  grasp–placement candidates with deployment-feasibility prediction (top-1 joint
  ranking 85.63 ± 1.08% on scene-group-held-out splits; 13 of 27 locked cases
  complete end to end on xArm7/MoveIt). A planning model rather than a harness or
  evaluation contract, **no artifact located**. Excluded under the standing
  manipulation-model policy.
- **EmbodiedMind** (`2609.19659`, cs.LG/cs.RO): rejection-sampling fine-tuning
  plus iterative/prefix-tree GRPO for embodied foundation models (70.02% average
  across 18 benchmarks). Training-method paper, and "our project will be released"
  is a placeholder, so **not open**. Excluded.
- **DexTouch-WM** (`2609.20649`, cs.CV/cs.RO, IROS 2026 workshop): action-
  conditioned tactile world model learned from human touch, evaluated as a
  surrogate environment for policy evaluation and as a synthetic-trajectory
  generator. World-model contribution with **no artifact located**. Excluded;
  **watch**.
- **Screened by title and category, set aside by the standing policy:** the
  remaining records the novelty pass surfaced — conventional manipulation,
  locomotion, navigation, SLAM, control, gripper/actuator design, medical,
  remote-sensing, autonomous-driving, and domain-agent papers — including
  `2609.19425`'s neighbours in the digital-agent cluster (`2609.20089`
  UnifiedPlayers, `2609.20082` MATCH, `2609.20377` MM-Future, `2609.19680`
  FinSkillOps), plus `2609.20114` (Universal Navigation Interface — robot-free
  rollator data collection, project page live but a data-collection proxy rather
  than a harness contract), `2609.20107` (AnyViewDex), `2609.19796` (LIFD scene
  memory), `2609.19330` (SemSafe-3DGS), `2609.20615` (INSPECT), `2609.19863`
  (off-road world models), `2609.20396` (Imagine-TAMP), and `2609.19542` (PerSeM).
  None introduces a reusable agent/VLA harness, recovery, safety, or evaluation
  contract beyond what the list already carries.

### Watch-list re-checks (no change)

- **FIERCE** (`github.com/ar-mine/FIERCE`) still ships no content (repository size
  0); **ContrAgent** (`github.com/yfxiao16/ContrAgent`) and **StageWAM**
  (`github.com/zhangzhongbo2213/StageWAM`) both still return 404. No watched
  artifact went live in this window.

## Operational notes

- **The OAI `noRecordsMatch` response is the decisive signal for a missing block.**
  Querying the empty datestamp window returned a 646-byte error document per
  category rather than a partial page; the wider window then reproduced the
  previous run's membership counts exactly (1,235 memberships), which is what
  distinguishes "no new block" from "harvest truncated".
- **A third screening pass on the same block still pays.** Yesterday's re-screen
  found four entries the first pass missed; this run's artifact-weighted pass
  found five more, including two that the previous passes had read as
  "no artifact" without checking the project page's disabled links (PersistBench)
  or the paper's own separate repository (StarVLA-α). Ranking by *artifact
  language* rather than by concept keywords is what surfaced them.
- **Distinguish the paper's artifact from the parent stack's.** `starVLA/starVLA`
  is already curated; `starVLA/starVLA-alpha` is a separate 71 MB repository
  created for this paper. Checking the repo README (not only the GitHub API) also
  resolved the license question — the API reports `NOASSERTION`, while the file
  itself is MIT with added rebase terms.
- **GitHub API rate limits did not appear in this run**; every repository check
  (starVLA, starVLA-alpha, the survey reading list, SERBench, Search-to-World,
  FIERCE, ContrAgent, StageWAM) returned clean responses.
- **`git ls-remote` requires the OpenSSH workaround on this host** —
  `GIT_SSH_COMMAND='ssh -F /dev/null'` — because
  `/etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf` is owned by `nobody`. The
  same workaround is used for `git push`.

## Validation performed

- Markdown structure checked for all five insertions (contiguous list items,
  intact `|` table row with five columns, no stray blank lines inside lists).
- `git diff --check` clean; no trailing whitespace or conflict markers.
- Every added link resolved live on 2026-09-20: `arxiv.org/abs/2609.19425`,
  `2609.19347`, `2604.11757`, `2609.19665`, `2601.07823`, plus
  `github.com/starVLA/starVLA-alpha` and
  `github.com/irom-princeton/awesome-robotics-video-world-model-papers`.
- Dates and cross-file consistency: README badge and "Last verified" now read
  **2026-09-20**, matching this record's filename; no curated entry references a
  withdrawn record; each new arXiv ID was checked against `README.md`,
  `docs/*.md`, and `sources/*.md` for prior inclusion and none was duplicated.
- Categories recorded per entry match the arXiv listings (cs.AI/cs.CR/cs.SE;
  cs.AI/cs.RO/cs.SY; cs.AI/cs.CV/cs.RO; cs.RO; cs.RO/cs.SY/eess.SY).
