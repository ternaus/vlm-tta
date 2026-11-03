# OpenMined grant commitments and milestones

This project was submitted to the OpenMined **Ensembles & Routing, Winter 2026**
research programme on 2026-10-16. The award was accepted on 2026-11-03. The scope
below records the research and delivery commitments in the submitted proposal and
accepted terms. It does not select the still-open experimental parameters.

The [public programme page](https://openmined.org/research/grants/) describes the
support and general expectations. Changes to the accepted project scope,
schedule, payment conditions, or terms require written agreement with OpenMined.

## Deliver the submitted study through ScreamingFace

The committed study compares single-pass inference, same-image resampling with
majority vote, fixed-view TTA, disagreement-triggered TTA with up to four views,
and a conventional cross-model ensemble. It starts with a pinned image-bearing
multiple-choice subset of MMMU and repeats the main comparisons across two VLM
families. See [the research description](research.md) for the view constraints
and measurement requirements.

The proposal explicitly includes working with ScreamingFace engineers to
represent view generation and answer aggregation in reproducible `url4` recipes
and onboard the pinned MMMU protocol. Existing compatible upstream support should
be reused. The exact division of engineering work remains to be agreed.

## Milestones use relative programme weeks

| Stage | Deliverables in the submitted proposal | Grant approval and payment |
| --- | --- | --- |
| Week 2 | Publish single-pass and same-image resampling baselines using the programme toolkit. | Establish the baseline composition. No separate stipend installment. |
| Week 6 | Publish the first fixed-view and adaptive TTA results against the agreed models and benchmarks. | Approval of the first method iteration triggers the first USD 1,000 installment. |
| Week 12 / completion | Publish the method comparison, ablations, Apache-2.0 code and recipes, final leaderboard submission, findings, and limitations. | Approval of the completed work triggers the final USD 1,000 installment. |

The offer describes the published November 1, 2026 to January 23, 2027 programme
dates as indicative. Individual milestone dates are to be confirmed during
onboarding. This document does not calculate deadlines from an assumed start
date.

The goal is a state-of-the-art or Pareto-frontier result. The accepted terms
explicitly allow an honest negative result, failed reproduction, or failure to
reach state of the art. Approval depends on completing the agreed work and
documenting it accurately and reproducibly.

## Use the awarded support for this project

The total stipend is USD 2,000, subject to the milestone approvals above.
OpenMined also provides up to USD 5,000 in subsidized project compute through
ScreamingFace. That compute is provided directly; unused capacity has no cash
value. Independently incurred expenses require prior written approval for
reimbursement, and additional programme compute requires written approval before
use.

## Release code, recipes, and results

Original code and recipes contributed to the funded project must be publicly
released under Apache License 2.0 with the required license text and notices.
Ownership of original contributions remains with their owners. Pre-existing and
third-party material retains its applicable license; conflicts must be resolved
before inclusion.

Project results and reproducible recipes are intended for the public leaderboard
and programme summaries with appropriate attribution. Publish the findings and
limitations even when the method does not improve the frontier. Access to model
weights is not required, and third-party models and datasets are not relicensed
by this project.
