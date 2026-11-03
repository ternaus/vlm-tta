# Reuse ScreamingFace and implement the missing research pieces

ScreamingFace already provides model execution, answer aggregation, benchmark
scoring, cost reports, and publication infrastructure. The main project-specific
work is deterministic image-view generation, disagreement-triggered execution,
their reproducible integration, and the controlled experiment.

Assessment date: **2026-11-03**. The inspected source snapshot is
[`c4ed59bd719ef5e12214e910a7eacad03952f867`](https://github.com/ScreamingFace/screamingface/tree/c4ed59bd719ef5e12214e910a7eacad03952f867).
These are source-level findings. The deployed grant engine, model access, and
benchmark availability have not yet been verified by a project run.

## Map each commitment to reusable components

| Project requirement | Existing upstream support | Remaining project work |
| --- | --- | --- |
| Single-pass VLM baseline | `Model` and `evaluate`; image-bearing benchmark inputs. | Select routes, pin model and generation settings, run and preserve the baseline. |
| Same-image resampling | `samples`, `Fusion`, and `Vote`. | Verify provider seed support, sample identity, answer extraction, ties, and invalid answers. |
| Fixed-view TTA | Image transport and answer aggregation. | Generate and label deterministic views, preserve context, and supply different views to individual calls. A ready per-branch image-transform interface was not found in the inspected SDK and compiler. |
| Adaptive TTA | Parallel fusions and serial pipelines. | Check agreement between parsed answers and launch additional views only when needed, up to the four-view limit. A supported candidate-controlled agreement-stop interface was not found. |
| Cross-model ensemble | `Fusion` with model-based or deterministic aggregation. | Select participants and the aggregation rule; compare their measured cost with the other methods. |
| Pinned MMMU evaluation | `inspect-mmmu_multiple_choice`: 847 questions, revision `5b008891134ef4c1`, with a choice scorer. | Confirm the installed revision and leaderboard registration; agree the project's exact subset and protocol. |
| Separate development and evaluation splits | Versioned benchmarks and a public `limit` argument. | Establish disjoint question IDs and a supported selection or separate registered datasets. The inspected backend serves a prefix; `limit` alone does not create an independent evaluation split. |
| Full accounting | Reports contain costs, tokens, durations, calls, and cache information. | Include preprocessing and control overhead, check retries and failures, and separate fresh inference costs from cache savings. |
| Statistical evidence | Per-case predictions and grades. | Compute paired uncertainty estimates, quality-cost and quality-latency comparisons, ablations, and failure analysis. |
| Reproducible release | `url4`, captured results, replay, and leaderboard APIs. | Include view identity and control decisions in the supported execution recipe; save complete captures and publish the agreed artifacts. |

Relevant source modules are
[Model](https://github.com/ScreamingFace/screamingface/blob/c4ed59bd719ef5e12214e910a7eacad03952f867/packages/screamingface/src/screamingface/model.py),
[samples](https://github.com/ScreamingFace/screamingface/blob/c4ed59bd719ef5e12214e910a7eacad03952f867/packages/screamingface/src/screamingface/_samples.py),
[Fusion](https://github.com/ScreamingFace/screamingface/blob/c4ed59bd719ef5e12214e910a7eacad03952f867/packages/screamingface/src/screamingface/fusion.py),
[Vote](https://github.com/ScreamingFace/screamingface/blob/c4ed59bd719ef5e12214e910a7eacad03952f867/packages/screamingface/src/screamingface/reducer.py),
the [candidate compiler](https://github.com/ScreamingFace/screamingface/blob/c4ed59bd719ef5e12214e910a7eacad03952f867/packages/screamingface/src/screamingface/_evaluation/candidate.py),
the [benchmark registry](https://github.com/ScreamingFace/screamingface/blob/c4ed59bd719ef5e12214e910a7eacad03952f867/apps/screamingface-engine/src/screamingface_engine_inspect/benchmarks.py),
and [case selection](https://github.com/ScreamingFace/screamingface/blob/c4ed59bd719ef5e12214e910a7eacad03952f867/apps/screamingface-engine/src/screamingface_engine/benchmarks/case_selection.py).

## Resolve the two execution gaps with the engineers

The integration needs a supported way to transform the candidate's image inputs
for each view and to condition later calls on earlier answers. Agree the
extension points and division of work with ScreamingFace engineers before
implementing those parts.

`Fusion.quorum` is a floor on successfully completed members. It is checked
after execution and does not stop calls when their answers agree. The existing
`CorrectiveLoop` and `SelfCorrective` recipes use a benchmark-provided check;
MMMU's multiple-choice registration has no intermediate check surface. The
adaptive policy must make its decisions from model answers, without access to
the correct label.

An early-stop result must avoid launching unnecessary later calls. Selecting an
answer after all four views have already run does not save those calls.

## Preserve the exact image protocol

The pinned
[Inspect Evals MMMU importer](https://github.com/UKGovernmentBEIS/inspect_evals/blob/v0.20.0/src/inspect_evals/mmmu/utils.py)
applies `thumbnail((1024, 1024))` before saving images. Decide whether TTA operates
on those benchmark inputs or on full-resolution source images before fixing the
method. A different input protocol must be named, versioned, and compared with
appropriate controls.

The imported multiple-choice questions contain one to five images. Preserve all
image identifiers and their order. Specify tile labels, source coordinates,
interpolation, image encoding, and retained context. Check request-size and image
count limits for the deployed engine before selecting a tile policy.

## Keep cost measurement and replay distinct

The inspected
[`Client.evaluate` implementation](https://github.com/ScreamingFace/screamingface/blob/c4ed59bd719ef5e12214e910a7eacad03952f867/packages/screamingface/src/screamingface/client.py)
has capture off by default. Request capture explicitly when a run must be
replayable, then check capture completeness and replay the saved result. Ordinary
request caching and a complete captured run serve different purposes.

Record provider-supported sample seeds, model settings, image bytes or verified
content hashes, dependency versions, and the compiled recipe. Repeated cached
answers must not be counted as independent samples. Measure fresh execution for
quality-cost comparisons and retain cache accounting separately.

## Implementation stages and unresolved decisions

1. **Verify the execution environment.** Confirm grant access, engine and SDK
   revisions, available VLM routes, seed support, MMMU revision, and supported
   extension points. Pin development and evaluation question IDs.
2. **Establish the Week 2 baselines.** Reuse single-model execution, repeated
   sampling, and voting. Check answer parsing, cost accounting, and saved runs on
   a small workload before scaling.
3. **Implement the Week 6 methods.** Add deterministic views and their fixed-view
   recipe, then conditional extra-view execution and its agreed stopping rule.
   Verify exact inputs and reproducibility with the engineers.
4. **Complete the study.** Freeze the chosen policies before final evaluation,
   repeat the comparisons across two VLM families, perform ablations and paired
   analysis, and publish the artifacts in the
   [grant milestones](grant-commitments.md).

The model families, view dimensions and tile policy, definition of agreement,
tie and failure handling, split IDs, statistical procedure, ablations, and exact
integration APIs remain open. Before a paid pilot, estimate its call count and
cost budget. Use the smallest measurement that resolves the remaining runtime
uncertainty; no paid pilot has been run by this project yet.
