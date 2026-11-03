# Budgeted visual test-time augmentation for vision-language models

Does spending an inference budget on different views of an image improve
accuracy more than spending the same budget on repeated answers to the unchanged
image? This project tests that question on frozen vision-language models (VLMs).
The contribution is intended to be empirical evidence about when visual diversity
is worth its cost, including conditions where it makes answers worse.

Test-time augmentation (TTA) changes the visual representation supplied at
inference and combines the resulting answers.

Status on 2026-11-03: scope documented; experiments and TTA implementation have
not started. The [grant commitments](grant-commitments.md) describe the promised
outputs. The [integration assessment](screamingface-integration.md) identifies the
existing infrastructure and remaining work.

## Compare five ways to produce an answer

Each model call receives a question and image content and returns a final answer.
Model weights remain fixed. Calls are single-turn; aggregation works with final
answers and does not require access to model weights or token logits.

| Method | Input and execution | Purpose |
| --- | --- | --- |
| Single pass | One call with the original image content. | Establish the starting accuracy, cost, and latency. |
| Same-image resampling | Repeat calls with unchanged image content and combine answers by majority vote. | Measure what additional answer samples buy without visual changes. |
| Fixed-view TTA | Use a predetermined set of image views and combine final answers. | Measure the value of visual diversity. |
| Disagreement-triggered TTA | Stop additional calls when answers agree; use up to four views when they disagree. | Test whether selective extra inference improves the quality-cost trade-off. |
| Cross-model ensemble | Combine answers from different models under a documented aggregation rule. | Compare visual diversity with model diversity. |

The precise number of fixed views, agreement rule, tie handling, and model routes
remain open. The four-view limit belongs to the adaptive policy. It does not
specify the view count for every fixed-view experiment.

## Preserve the question while changing the view

The initial policy covers the original image, resolution variants that preserve
aspect ratio, and the original image accompanied by labeled detail tiles. Each
detail view retains the original for context. Questions with several images must
preserve their order, identifiers, and relationship to the question.

Rotations, reflections, and color changes are excluded from the initial policy:
they can change the correct answer to text, spatial, or color questions.

The source of each view must be fixed before implementation. The existing MMMU
import reduces images to fit within 1024 by 1024 pixels. Views made from those
inputs and tiles extracted from the full-resolution source test different
effects. Any full-resolution variant needs an explicitly agreed input protocol
and controls with comparable access to the source information.

## Fix development and evaluation data separately

The initial target is the image-bearing multiple-choice portion of
[MMMU](https://mmmu-benchmark.github.io/), using a pinned subset and scoring
protocol. Report the exact question IDs, dataset revision, image preprocessing,
prompts, answer extraction, and scoring rules. A subset score must be labeled as
that subset's result.

Choose image policies and generation settings on a separate development split.
Freeze those choices before held-out evaluation. Held-out here describes the
separation from policy development; it makes no claim about a model's training
exposure to this public benchmark. Repeat the main comparisons across two VLM
families. Those families and the exact splits have not yet been selected.

## Measure quality together with its full cost

For matched question IDs, record accuracy, valid-answer coverage, model calls,
text and image tokens, measured dollar cost, and end-to-end latency. Include image
preprocessing, control logic, aggregation, retries, and failed calls in the
accounting. Describe how invalid answers and execution failures affect scoring.

Compare methods over measured cost budgets. Equal call counts can have different
costs because views and models consume different numbers of tokens. Keep fresh
inference measurements distinct from cached execution and replay.

Report paired uncertainty estimates for differences between methods. Include
ablations of the selected view policy and adaptive execution, plus failure cases
where transformations remove context or introduce mistakes. The exact statistical
procedure and ablation matrix remain to be fixed before the final runs.

## Publish a reusable result

The research target is a Pareto-efficient configuration: among the compared
configurations, another should not achieve at least the same accuracy for lower
cost, or greater accuracy for no greater cost. A well-supported negative result
will also be published with its measured cost and limitations.

Outputs include named experiment configurations, pinned dependencies and model
identities, reproducible ScreamingFace `url4` recipes, comparison results,
ablations, failure analysis, and original code under Apache 2.0. Completed
experiments and baselines retain their original configurations.

## Relation to previous work

[Kaya, Elliott, and Papadopoulos, *Efficient Test-Time Scaling for Small
Vision-Language Models*](https://arxiv.org/abs/2510.03574) study token-level
aggregation of augmented inputs and test-time parameter adaptation. This project
studies answer-level, budget-aware composition with frozen API-accessible models.
The study does not claim that visual TTA, voting, or early stopping is itself new.
