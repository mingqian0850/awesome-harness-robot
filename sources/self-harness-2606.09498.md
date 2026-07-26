# Self-Harness — Source Note

Verified on 2026-07-26 against the arXiv abstract and HTML full text.

## Primary source

- **Title:** Self-Harness: Harnesses That Improve Themselves
- **Authors:** Hangfan Zhang, Shao Zhang, Kangcong Li, Chen Zhang, Yang Chen, Yiqun Zhang, Lei Bai, Shuyue Hu
- **Submitted:** 2026-06-08
- **arXiv:** https://arxiv.org/abs/2606.09498
- **HTML:** https://arxiv.org/html/2606.09498v1
- **DOI:** https://doi.org/10.48550/arXiv.2606.09498
- **Subject:** Computation and Language (cs.CL)

## Method

Self-Harness holds the base model and evaluator fixed while iteratively changing the
non-parametric agent harness:

1. **Weakness Mining:** cluster verifier-grounded failures from execution traces.
2. **Harness Proposal:** use the same target model to propose diverse, bounded, and
   minimal changes to editable harness surfaces.
3. **Proposal Validation:** promote a change only when it improves at least one of the
   held-in or held-out splits without degrading the other.

The editable harness can include instructions, tools, memory/state mechanisms, runtime
control, verification behavior, skills, and subagents.

## Author-reported results

On Terminal-Bench-2.0, held-out pass rates changed as follows:

- MiniMax M2.5: 40.5% to 61.9%
- Qwen3.5-35B-A3B: 23.8% to 38.1%
- GLM-5: 42.9% to 57.1%

## Relevance and limitations

This is not a robotics paper and contains no robot experiments. Its contribution to this
collection is the general trace–edit–validate pattern for harness optimization. A robot
adaptation would require simulation or offline replay, immutable safety constraints,
independent promotion approval, versioning, rollback, and shadow evaluation before
hardware deployment.

The split called held-out is hidden from the proposal context, but its pass count is used
to accept or reject every candidate. Repeated selection can therefore adapt to that split;
it should not replace a final untouched test set.

No official project or code link was present in the arXiv record or paper when checked.
