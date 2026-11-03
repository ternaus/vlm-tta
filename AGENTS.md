# VLM TTA

This repository contains research on budgeted visual test-time augmentation for
frozen vision-language models. See README.md for scope and verification commands.

Keep model inference, image-view generation, answer aggregation, and evaluation
protocols explicit. Adapt existing ScreamingFace components where they satisfy
the required behavior. Pin dataset and model identities and record image
preprocessing in every experiment.

Every experiment gets a named configuration and a matching experiment document.
Record the hypothesis, exact changes, source and dependency revisions, metrics,
run links, and results. Preserve completed experiments and baselines. Select
policies on development data before evaluating on held-out data.

Automated tests run on CPU without network model calls. Paid inference runs are
separate experiments with an explicit workload and cost budget. Estimate cost
before running them and record actual usage, retries, cache behavior, and latency.

Keep Ruff's full rule set and protected complexity diagnostics enabled. Fix
complexity through clearer code. Do not add suppression comments or broad ignores
to bypass lint or type checks. Add behavior tests with implementation; do not add
placeholder tests to make an empty suite green.

Original code and recipes must be publishable under Apache 2.0. Preserve the
licenses of third-party material. Keep credentials and private correspondence out
of this repository.
