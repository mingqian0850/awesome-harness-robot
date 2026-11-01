# Daily arXiv scan — 2026-11-01 (no announcement batch for a third consecutive day; third independent pass over the unchanged 2026-10-28 and 2026-10-29 batches: two records no earlier pass had named)

## Scope

- **Interval owed.** The previous successful run started **2026-10-31 09:30 UTC**, committed at **2026-10-31 09:38 UTC**
  (`2026-10-31T10:38:41+01:00`) on `main` at `764bf62` with record `sources/daily-arxiv-2026-10-31.md`, and closed
  2026-10-29 at frontier ID `2610.40646` (`2026-10-29T17:59:20Z`). This run started **2026-11-01 10:04 UTC** on
  `main` at `764bf62`, so the interval it owes is **submission dates 2026-10-30 → 2026-11-01**.
- **The frontier has still not moved, and that is now a three-day observation.** An unrestricted descending `cs.RO`
  query returns **`2610.40646`** (`2026-10-29T17:59:20Z`) — byte-identical to the frontier the previous run recorded.
  A union probe over all five target categories, descending, returns **`2610.40655`** (`2026-10-29T17:59:53Z`) as the
  newest record overall. Neither date advanced in the ~24 h since the previous run, and neither had advanced in the
  ~23 h before that.

  | query (one OR-query over cs.RO/cs.AI/cs.CL/cs.CV/cs.LG per day) | 2026-10-28 | 2026-10-29 | 2026-10-30 | 2026-10-31 | 2026-11-01 |
  | --- | ---: | ---: | ---: | ---: | ---: |
  | `submittedDate:[YYYYMMDD000000 TO YYYYMMDD235959]` | **317** | **498** | **0** | **0** | **0** |
  | recorded by the previous run | 317 (unchanged) | 498 (unchanged) | — | — | — |

  The three zeros are **not** zero-submission days: 2026-10-30 was a Friday and 2026-10-31/11-01 a weekend, and arXiv
  announces on a weekday schedule, so nothing from those dates has reached the export API. `cs.RO`-only and union
  probes agree, so this is an announcement stall rather than a category-filter artefact. **The three unannounced days
  remain owed to the next run**, which must query them again rather than assume they were screened here.
- **The batch re-screened, for the third time.** Because the newest visible batch was unchanged from the previous run,
  the standing instruction is to re-screen that whole batch once before concluding that nothing qualifies. This run
  re-screened **all 815 records**: **317** of 2026-10-28 (`2610.38086`–`2610.39065`, `00:00:17Z` → `23:51:50Z`) and
  **498** of 2026-10-29 (`2610.39070`–`2610.40655`, `00:00:46Z` → `17:59:53Z`). Live day counts matched this run's
  harvest exactly (317 / 498), so the batch is unchanged in both size and content.
- **The third pass was not redundant.** The previous record's own conclusion was that an unmentioned ID inside a
  screened range is an *unscreened* record. Applying that test directly — grepping all 815 IDs against every markdown
  file in the repository — **677 of 815 records (83%) are named nowhere**, so "was it named last time?" is not a
  usable proxy either. This run therefore read **all 677 unnamed titles** and screened them on their own merits
  rather than inheriting the earlier passes' judgments. That produced **two inclusions**, both named in neither prior
  record and absent from the repository.
- **Coverage.** All 815 carry at least one of the five target categories. Primary category counts — 2026-10-28:
  `cs.LG` **72**, `cs.AI` **57**, `cs.CV` **55**, `cs.CL` **41**, `cs.RO` **34**, `cs.CR` 9, `stat.ML` 6, `cs.HC` 5,
  `cs.SE` 5; 2026-10-29: `cs.CV` **126**, `cs.LG` **103**, `cs.AI` **77**, `cs.RO` **62**, `cs.CL` **43**, `stat.ML` 12,
  `cs.CR` 10, `eess.SY` 5, `cs.SE` 5. `cs.RO` appears as primary or cross-list on **112** records (**37** + **75**).
- **Revisions.** No in-batch `<updated>` > `<published>` exists in either day (both are single-vintage v1 blocks).
  Because the frontier has not moved at all, no replacement could have been announced either; the `id_list` sweep
  below confirms it.

## Method

- **Harvest (export API, HTTPS).** `https://export.arxiv.org/api/query`. One `cs.RO` frontier probe, one five-category
  union probe (`max_results=25`, descending), five day counts, and two full-day harvests (`max_results=2000`,
  ascending) — **9 batch requests**, plus **6** `id_list` sweep requests, **8** `abs`/`html` page fetches and **3**
  GitHub REST calls, all at 3–5 s spacing with **zero HTTP 429**.
- **`/tmp` is not durable between tool calls in this environment.** A directory created under `/tmp` in one call was
  gone in the next, so all XML, HTML and API responses were kept under `.scratch/arxiv-2026-11-01/` (untracked, never
  committed) — matching the previous run's finding and its workaround.
- **Query syntax matters for the counts.** The unparenthesised form
  `cat:cs.RO OR cat:cs.AI OR ... AND submittedDate:[...]` returns **497,520** for every day: the date clause binds
  only to the last `OR` term, so the filter is effectively absent and the count is site-wide. Parenthesising the
  category disjunction — `(cat:cs.RO OR cat:cs.AI OR cat:CS.CL OR cat:cs.CV OR cat:cs.LG) AND submittedDate:[...]` —
  yields the 317 / 498 / 0 / 0 / 0 above, matching the previous run's recorded counts exactly. Any agent re-deriving
  these numbers should assert that the count changes when the date changes.
- **Screening.** (a) All **815** titles were re-read in ascending ID order. (b) The **677** that no record names were
  then screened as a set with a robot-anchor and harness-anchor term filter, which left **41** to rank — **33**
  robot-anchored and **8** non-robot harness-heavy. (c) **16** abstracts were read in full. (d) Full-text HTML of
  **4** records was read for release statements, gate internals and result tables.
- **Parser sanity check first.** The previous run lost a pass to an extractor that asked for `<abstract>`; the arXiv
  Atom feed carries the abstract in **`<summary>`**. This run asserted non-empty abstracts before screening: **0 of
  815 records** had an empty or sub-40-character abstract.
- **Deduplication.** All **815** IDs were grepped across `README.md`, `docs/` and `sources/`, which is what produced
  the 677-unnamed set. Both inclusions were then checked again against the full repository and are absent.
- **Artifact verification against live sources:** the GitHub REST API for **3** repositories (licence, size,
  creation/push timestamps, stars), a GitHub **repository search** for the included system's name, and full-text
  release-statement extraction on all four papers read in depth.

## Included (2)

### Runtime Gates and Verification (1)

- **Think Before You Act: Decision Gate for LLM Agents** (`2610.38620`, cs.AI; IBM SIL, eight authors; CC BY-SA 4.0) —
  why it qualifies: it separates **who acts** from **who checks** more cleanly than anything already on this list.
  The ReAct harness keeps proposing actions; a **separate decision model** — a fixed-choice "Jev-type" judge that
  returns a probability per option in **one forward pass, with no generation at all** — scores structured questions
  about the proposed action, alongside deterministic checks of tool-state facts that are **compiled offline from the
  written domain policy** into questions, a decision rule, executable code over the records it reads, and a hand-off
  checklist. The split is by evidence type: what the tool state settles is settled by code, what depends on
  conversational context is settled by a thresholded probability, and a flagged action returns targeted feedback for
  **exactly one bounded repair round**. The gate also catches **premature task termination**, not only invalid writes.
  Because it reads only the conversation and tool state, it needs no access to the agent's parameters or tools, so one
  unchanged gate is applied across eleven agents. Reported, on τ²-bench Airline and Telecom and BFCL with **pass³**
  (success on all three runs): Telecom **39.5 → 80.7** (Opus 4.6), **57.9 → 81.6** (Qwen3.8-27B), **28.9 → 51.8**
  (gpt-oss-120b), **11.4 → 17.5** (GPT-4.1), with all eight agents tested there improving both at-least-once and
  pass³; BFCL **38.2 → 47.8**, **55.9 → 59.1**, **39.9 → 47.6**, **48.1 → 49.9**; Airline mixed but non-negative
  (**36.0 → 46.0**, **72.0 → 76.0**, **42.0 → 50.0**, **74.0 → 76.0**). The two ablations are the reusable part
  rather than the headline: executable state checks supply a substantial share of the Airline benefit, and selective
  verification beats indiscriminate revision for GPT-4.1 but **not** for gpt-oss-120b; and judge size is strongly
  sub-linear — on **609 identical hand-off states** Kev-9B agrees with Kev-27B on **97.0%** of decisions and Kev-0.8B
  on **90.6%**, at **0.53 / 1.22 / 2.72 s** per request on one A100-80GB (Qwen3.8-27B 4.02 s, Gemma-4-31B 6.10 s).
  Digital tool-using agents; no robot experiment; author-reported. **Artifact status literal (2026-11-01): no
  first-party release** — GitHub search for `JevAct` returns **0** repositories, the paper's only GitHub link is the
  cited Apache-2.0 `zjunlp/JevLoop` runtime (a *different* work, 9.7 MB, pushed 2026-10-05), the judge is a
  third-party decision-model family, and no data, checkpoint or gate implementation accompanies the paper.

### Embodied Accountability (1)

- **When "No Worries" Went Wrong: HRI Lessons for Foundation-Model Robots** (`2610.38400`, cs.RO; Eliahu Khalastchi,
  The College of Management Academic Studies; submitted to IEEE Robotics & Automation Magazine; 8 pages) — why it
  qualifies: it names a failure mode the list does not otherwise carry — **moot success**, where a system reports
  `Success` for the successful execution of an effect **whose purpose has expired**. Two episodes from three years of
  a ROS 2 HRI consortium on one office robot carry it. An LLM made replies more natural but added generation latency;
  speech and motion were allowed to run asynchronously to hide the wait, so the disengaging gesture completed *before*
  the reassurance was spoken — every component behaving as programmed, the **ordering alone** turning a polite line
  into apparent sulking. A second episode announced a visitor as arriving while he stood beside the host. The
  generalisation is that capability does not ensure a changed situation reaches the person, because **evidence may
  never be acquired, or a purpose or constraint may fail to survive planning and execution** — which moves the object
  of review from the pipeline to the *premise*. Five lessons follow and map onto what this list calls harness state:
  attention is a **budget**; stored knowledge is **evidence, not fact**; preserve **task purpose as well as task
  triggers**; check social effects **at execution, including their order**; and **separate proposing from committing**.
  The method, **TRACE**, changes one interaction premise, follows it to a decision or enforcement boundary, and emits
  an actionable architecture finding through four questions with explicit *absent* / *unknown* dispositions; it is
  stated as **not a defect-detection benchmark** and is supplied as a fillable worksheet in the supplementary
  material. Its worked example is a bounded analysis of a published LLM-generated behaviour-tree robot whose public
  source (commit `477bfcc`) contains a question action and answer condition that *could* have formed a relevance check
  and were never composed into the illustrated tree — the authors state explicitly that this establishes neither the
  deployed configuration nor any adverse outcome, and that the same chain returns *no gap* elsewhere. The engineering
  conclusion is deliberately anti-pipeline: **preserve accountability boundaries, not the computational pipelines that
  once exposed them**. **Artifact status literal (2026-11-01): no code, model or data repository** — the only GitHub
  link in the paper is the third-party system being analysed; the worksheet is arXiv supplementary material.
  Real-robot practice plus one bounded case analysis rather than an experiment; author-reported. **Name collision:**
  the unrelated `TRACE` at `2609.30258` (gradient inversion of distributed embodied training streams) is already in
  this list; the README entry keys to `2610.38400` and says so.

## Revision handling (no entry changed)

- **`id_list` sweep.** All **570** distinct arXiv IDs in `README.md` were resolved in six requests; **174** resolve at
  v2 or later.
- **In-window check.** Exactly **three** carry `<updated>` ≥ 2026-10-28T00:00:00Z with `<updated>` ≠ `<published>`:
  `2610.37255v2` (updated 2026-10-28T05:58:44Z), `2609.38905v4` (2026-10-29T03:27:11Z) and `2609.19906v2`
  (2026-10-29T10:18:41Z). These are **the same three the 2026-10-31 run already handled**, with identical
  timestamps — no new version appeared.
- **The strongest signal is the maximum.** The newest `<updated>` across all 570 README IDs is
  **`2610.40565v1` at 2026-10-29T17:42:32Z**, i.e. inside the already-closed 2026-10-29 day. Nothing in the curated
  list has moved since the frontier stopped, so **no entry needed a revision update**.
- **In-batch check.** Neither re-screened day contains an `<updated>` > `<published>` record (0 of 815), consistent
  with both being single-vintage blocks.

## Rejected, deferred and watch list

- **Read at abstract level and deferred as on-scope but not qualifying this run:**
  **Minimum Sufficient Communication for Independent Behaviour Tree Robot Teams** (`2610.38176`, cs.RO, ACRA 2026 —
  a genuinely careful *measurement* procedure: a declared policy family, recipient-weighted bits per robot decision
  under one-sided simultaneous confidence bounds, and a four-valued verdict proper to the evidence — exact minimum,
  positive bracket, sufficient upper bound, or unresolved boundary, reported here as zero communication exact under
  low contention and a gate at `q=0.992222` sufficient at 96.33% success for **0.544%** of full-state traffic.
  **Verified open: MIT `sherryjiang-hub/Multi-robot-collaboration-`** (2.8 MB, created and pushed 2026-10-04) with
  experiment source, seeds, per-episode CSVs, analysis scripts and tests. Deferred because the object measured is
  multi-robot communication policy, not an agent/VLA harness — the evidence-valued verdict style is right for this
  list, the subject is not, and the multi-robot line is already covered). **PrimaCons / Cross-Primitive Consistency in
  Jev** (`2610.38623`, cs.CV — the same semantic state expressed as different query primitives yields inconsistent
  decisions across three decision-model sizes, with the cross-primitive discrepancy predicting decision failure at
  ROC-AUC **0.91**; it is the natural *measurement* companion to JevAct's gate, by a different group, Xu Liu et al.,
  but it benchmarks a niche verifier format and was not artifact-checked, so it is deferred rather than paired).
  **CORAL / Don't Lose the Thread** (`2610.40350`, cs.CL, COLM 2026 — agent-callable working-memory toolset that
  checkpoints progress and verified facts and resets cluttered context; MIT-verifiable code at
  `JIA-Lab-research/CORAL`, but the memory line here is already dense and the contribution is a per-episode context
  tool rather than a new contract). **Pre-Crime for Agents, at the Last Second** (`2610.40588`, cs.AI — a linear probe
  read at each model call before the output is written catches **82%** of eventual leaks at a 5% benign flag rate
  where a zero-shot decision-model monitor catches **26%**; an observability contribution with code at
  `leonidas1712/precrime-agents`, deferred as activation-level probing of digital agents rather than a robot-side
  monitor). **Is it my preference? Role Grounding Attack against Agent's Memory** (`2610.38209`, cs.CR — memory
  poisoning via role-to-principal misattribution through nested social-platform conversation; on-scope for the memory
  trust line, deferred for scope density). **UMI-Live** (`2610.39385`, cs.RO — full-stack open-source bimanual UMI
  data-collection system; real infrastructure, but data-collection hardware and tooling rather than a runtime
  contract).
- **Read and excluded as off-scope:** **Beyond LLM Wrappers** (`2610.39230`, cs.AI — a production financial
  multi-agent architecture with a "context engineering" coordination layer; domain-specific enterprise deployment,
  and its 7–9% → <1% hallucination result is explicitly observational, so it carries no reusable harness contract).
  **Retaining Repair Hypotheses for Cost-Aware Execution** (`2610.39410`, cs.AI — a repair-to-execution interface that
  keeps alternative finite guard repairs and conditions their weights on observations; conceptually neat, but the
  domain is 24 synthetic two-stage devices and there is no agent or robot). **HAIDL / Human–AI Interactions as
  First-Class Data** (`2610.40006`, cs.AI — a formal interaction model and JupyterLab middleware for capturing
  prompts and responses as replayable data; notebook tooling, not an embodied or agentic runtime). **Is It the
  Curriculum or Just the Easy Start?** (`2610.40364`, cs.LG — fourteen UED and domain-randomisation curricula under
  one protocol on five MuJoCo quadruped models, with a released repository; this is conventional RL environment
  design, not an agent/VLA harness, recovery, safety or agent-evaluation contract). **Decision-Level Error
  Attribution … of Monocular RGB Human Tracking** (`2610.40199`, cs.RO — a two-factor Shapley split of stop/proceed
  decision error into safety-rule and perception contributions; good attribution discipline, but perception-only for
  navigation falls under the standing perception exclusion). **Decoding Geometry, Not Tokens** (`2610.39416`, cs.RO,
  WACV 2027 — VLMs with a geometry-compatible head for affordance and trajectory prediction; a perception result).
  **Quantize by Doing** (`2610.38661`, cs.RO — task-guided mixed-precision quantization of VLAs; model-efficiency,
  and `VLAQuantBench` already covers the measurement side).
- **Off-scope remainder (representative, out of the 677 unnamed titles):** SLAM, localization, mapping and calibration;
  robot hardware, mechanisms, actuation and tactile gloves (`2610.39076`, `2610.40600`); conventional control,
  reachability and motion planning; legged locomotion and legged navigation stacks (`2610.39267`); smart-wheelchair
  and gaze-teleoperation interfaces (`2610.38569`, `2610.39998`); autonomous-driving testing and datasets; the large
  LLM/post-training remainder (on-policy distillation, quantization, KV-cache, speculative decoding, serving, RLVR,
  RAG, ASR/TTS); medical, financial, legal and scientific-agent applications; film, gaming, 3D and video generation.
- **Watch list carried forward, re-checked only at link level this run:** **RoboSeed** weights ("Pending release" for
  all nine checkpoints), **ReSTEP** dataset and code, **EvoLife** ("Code is coming soon"), **Sandman**, **LOGIV** and
  **AgentHeal** (public, unlicensed), **METATRON** and **ELEVATE** (project page and demo only), **Scan Before You
  Install** ("upon acceptance"), **SkillSense** (`anonymous.4open` returns 401), **PASA-KV**, **Soei** and the
  **Assurance Stack** agenda (nothing located), and nothing-located **KARMA**, **VEvo-Harness**, **Masked Erosion**,
  **Prefix Executability**, **REALM** and the oversight-directives study. Newly added: **JevAct** (no first-party
  repository), **PrimaCons** and **Minimum Sufficient Communication** (both artifact-verified, deferred on scope).
- **Corrected status of last run's inclusions.** The ten entries added on 2026-10-31 were re-checked as a set against
  the 677-unnamed computation and all ten are now named in the repository, so that pass's work is intact and this run
  added to it rather than replacing it.

## Validation performed

- **Markdown structure.** 2 new entries are single-line list items appended under the existing `### Safety` heading;
  1 new bullet added under `## Current Landscape` in the same `- **claim.** [entry] …` shape as its neighbours. No
  heading levels changed, no list nesting introduced, no HTML added.
- **`git diff --check`.** Clean — no whitespace errors, no conflict markers, no trailing whitespace.
- **Links.** Both added arXiv `abs` URLs were fetched during verification (HTTP 200, 41 KB each) and the full-text
  HTML for each was read; the two in-paper GitHub URLs were resolved through the REST API (`zjunlp/JevLoop`
  Apache-2.0; the TRACE analysis target is a third-party repository, stated as such). The pre-existing `TRACE` link at
  `2609.30258` was left untouched.
- **Duplicate IDs.** All 966 ID occurrences in `README.md` were counted; both new IDs occur exactly twice — once in
  the landscape bullet and once in the main list — which is this repository's established pattern, not duplication.
  The name collision between the two `TRACE` works is disambiguated inside the entry.
- **Dates and categories.** Both entries state their arXiv ID, primary category and the 2026-10-28 publication date;
  both were published inside the re-screened batch, not in an announced 10-30/10-31/11-01 batch (there is none).
- **Cross-file consistency.** The README badge and `Last verified:` now read **2026-11-01**, matching this record and
  the `sources/daily-arxiv-2026-11-01.md` filename. The previous record and the README's earlier history are
  unchanged.
- **Artifact-status discipline.** The two entries record two distinct literal statuses — **no first-party release**
  (JevAct, with the specific search that returned zero) and **no repository at all** (TRACE, with the third-party link
  identified as such). Neither is described as open source.
- **Author-versus-independent separation.** Every number in both entries is marked author-reported; neither paper has
  an independent reproduction, and neither reports a robot experiment (JevAct) or more than a bounded case analysis
  (TRACE).

## Operational notes and blockers

- **Three days without an announcement batch is a state, not an error, and it must not produce an empty commit.**
  The frontier has not advanced since 2026-10-29T17:59:53Z. The correct action is still to re-screen the visible
  batch, but the *reason* to commit must be a found paper, never the passage of a day. This run committed because it
  found two.
- **"Named somewhere" is the wrong dedupe key, and "named in the last record" is worse.** 677 of 815 records in this
  batch are named in no file in the repository, and 2 records worth including were among them *after two prior full
  passes*. The reliable screen is: compute the unnamed set against the whole repository, then read that set's titles.
  An ID that no record mentions is an unscreened record, exactly as the previous run concluded — and that set stays
  large even when the batch is "done".
- **A third pass over the same bytes is cheap and still pays.** Re-screening cost parsing, not requests: the whole
  run used 9 export-API requests plus 6 `id_list` requests and never approached a rate limit. The expensive part of a
  re-screen is reading, and reading 677 titles is tractable; assuming a prior pass was complete is not.
- **Unparenthesised OR-queries silently drop the date filter.** They return a constant site-wide `totalResults`
  (~497,520) and would have made every owed day look screened. Parenthesise the category disjunction and assert the
  count changes between days.
- **`/tmp` is still not durable across tool calls in this environment.** All XML, HTML and API responses were kept in
  the untracked `.scratch/arxiv-2026-11-01/` directory. Any agent reusing the previous runs' `/tmp` convention will
  lose its harvest between calls.
- **OpenSSH workaround still required.** `git ls-remote`/`fetch`/`push` fail with `Bad owner or permissions on
  /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf`;
  `GIT_SSH_COMMAND='ssh -F /dev/null'` restores access with the user's key and `known_hosts` unaffected.
- **Nothing new is owed but the three unannounced days.** The next run must query `submittedDate` for **2026-10-30,
  2026-10-31 and 2026-11-01** and, if the frontier is still unmoved, decide whether a fourth pass over the same 815
  records is worth its cost — this run's evidence is that the marginal return is falling but not zero (2 inclusions on
  pass three against 10 on pass two).
