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

This repository currently contains the research scope and project setup. No TTA
implementation or experimental results have been published yet. Model choices,
image-view policies, and the evaluation protocol remain to be fixed.

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
`uv run --locked --extra dev pytest`. There are no behavior tests yet; CI starts
running pytest when test modules are added. Automated tests must run on CPU
without paid model calls.

CI checks Python 3.12 and 3.13, runs pre-commit, and verifies that a built wheel
installs and imports in an isolated environment.

## License

Original code and recipes are licensed under [Apache License 2.0](LICENSE).
Third-party models and datasets retain their own licenses.
