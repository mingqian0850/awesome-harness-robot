# Daily arXiv scan — 2026-08-02

## Scope

- Incremental interval: after the 2026-07-31 successful scan through
  2026-08-02 11:22 UTC.
- Official arXiv API queries:
  - latest 200 `cs.RO` records sorted by `lastUpdatedDate`;
  - latest 100 records matching agent harness, robot agent,
    vision-language-action, robot foundation model, robot world model, or
    agentic robot, also sorted by `lastUpdatedDate`.
- Additional first-party artifact checks: official project pages and official
  GitHub repositories for curated papers whose code was previously unavailable.
- Existing `README.md`, `docs/landscape.md`, and `sources/` were checked for
  duplicate entries.

`parallel-cli` was installed but unauthenticated, and neither the Parallel nor
OpenRouter credential was configured. The search therefore used arXiv's official
API plus arXiv, author-institution, project, and official GitHub pages directly.

## Incremental arXiv result

No records in either query had an arXiv `updated` timestamp on or after
2026-07-31. This is consistent with the scan running on a Sunday, outside the
normal weekday announcement cycle. No new paper or important arXiv revision was
therefore added from the strict incremental window.

## Included backlog correction

### ENPIRE: Agentic Robot Policy Self-Improvement in the Real World

- Paper: https://arxiv.org/abs/2606.19980
- DOI: https://doi.org/10.48550/arXiv.2606.19980
- Official project: https://research.nvidia.com/labs/gear/enpire/
- Submitted: 2026-06-18.
- Classification: General Harness Methodology / Self-Improving Agentic Robot
  Harness / Evaluation.
- Why included now: an official NVIDIA GEAR project-page sweep surfaced this
  high-value paper, which was absent from the repository despite directly
  matching its scope. ENPIRE defines a repeatable physical-autoresearch loop
  with four modules: Environment for reset, safety, observation, and
  verification; Policy Improvement for policy or training-code changes; Rollout
  for budgeted physical trials and logs; and Evolution for multi-agent branch
  comparison and recipe reuse.
- Evidence boundary: the authors report real YAM-robot and RoboCasa experiments.
  The official page describes 99% **pass@8** on showcased dexterous tasks, where
  a long-horizon rollout may use up to eight failure-conditioned retries per
  subtask. This is not equivalent to 99% pass@1 or independent validation.
- Artifact status: the project exposes interactive results and code snippets,
  but no public implementation repository was linked as of 2026-08-02.

## Artifact-status checks

- **Robot-Factored World Models / RoFacto**:
  https://github.com/bjkim95/rofacto is now reachable from the official project
  page. It is a public placeholder repository whose README says “Code coming
  soon”; it contains no implementation, so the project remains classified as
  not yet open source.
- **ViTacWorld**: the official project page still says “GitHub Coming Soon.”
- **Guava**: the official project page still marks code as “coming soon.”
- **ASPIRE**: the official project page exposes task and skill snippets, but no
  public implementation repository was verified in this scan.
- **Harness VLA, Physical Agency, CheckVLA, and HERO**: no new official public
  code or weights release was verified.

## Rejected search results

- Web results that only matched date-like version strings or generic software
  release pages were unrelated to robotics and excluded.
- Noisy navigation, autonomous-driving, conventional control, sensing, and
  hardware results were excluded unless they introduced a reusable harness,
  foundation-model interface, or broadly useful evaluation pattern.
