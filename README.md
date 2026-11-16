# VLM TTA

Research on budgeted visual test-time augmentation for frozen vision-language
models. The study asks whether different image views improve accuracy at a given
inference cost compared with repeated answers to the same image.

The planned comparisons are single-pass inference, same-image resampling with
voting, fixed-view TTA, disagreement-triggered TTA, and a cross-model ensemble.
The initial target is an explicitly versioned, image-bearing multiple-choice
subset of [MMMU](https://mmmu-benchmark.github.io/), with experiments across two
VLM families. Results will include measured cost, latency, paired uncertainty
estimates, ablations, and failure cases.

## Published baseline

Gemini 3 Flash Preview scored **83.59% (708/847)** in a single pass on the pinned
MMMU multiple-choice `validation` benchmark. The run took 44 minutes and used
$1.26 of inference. ScreamingFace verified the result on November 15, 2026.
[Leaderboard entry](https://leaderboard.screamingface.ai/v1/public/scores/020151ad-8990-4133-bf21-806b0ad99d09) ·
[Experiment record and artifacts](experiments/mmmu-validation-gemini-flash-gcp-2026-11-15.md)

The next comparison pairs four calls on the same image with four calls on
different views of that image on 141 MMMU `dev` questions. The pilot still needs a
way to pass the prepared views to the hosted model. ScreamingFace's
[attachment support](https://github.com/ScreamingFace/screamingface/pull/1588) is in review.

## Research documents

- [Research question and evaluation](docs/research.md): the five comparisons,
  image-view constraints, data protocol, measurements, and expected outputs.
- [Grant commitments and milestones](docs/grant-commitments.md): the submitted
  deliverables, relative schedule, publication obligations, and compute budget.
- [ScreamingFace integration](docs/screamingface-integration.md): verified upstream
  components, remaining implementation work, and decisions needed before coding.

## Research support

Vladimir Iglovikov, Albumentations LLC. This research is supported by the
[OpenMined Ensembles & Routing programme](https://openmined.org/research/grants/).
Integration with [ScreamingFace](https://github.com/ScreamingFace/screamingface)
is part of the research scope. Both improvements and well-supported negative
results will be reported.

## Development

Use Python 3.12 or 3.13 and [uv](https://docs.astral.sh/uv/):

```bash
uv sync --locked --extra dev
uv run --locked --extra dev pre-commit install
uv run --locked --extra dev pre-commit run --all-files
```

The quality configuration follows
[VoxelPipe](https://github.com/albumentations-team/voxelpipe/tree/ee7d5507ef368a6169844c7021bcab5b7fae1195):
Ruff lint and formatting, protected complexity diagnostics, mypy, Pyrefly,
lockfile validation, repository hygiene, spelling, OpenCV RGB usage, and NumPy
assertion checks. Runtime dependencies are added when an implementation needs
them. Complexity rules stay enabled in tests too, and a local hook keeps Ruff's
default complexity limits from being overridden.

Behavior tests belong in `tests/`. Run them with
`uv run --locked --extra dev pytest`. CI runs pytest when test modules are
present. Automated tests run on CPU without paid model calls.

CI checks Python 3.12 and 3.13, runs pre-commit, and verifies that a built wheel
installs and imports in an isolated environment.

## License

Original code and recipes are licensed under [Apache License 2.0](LICENSE).
Third-party models and datasets retain their own licenses.
