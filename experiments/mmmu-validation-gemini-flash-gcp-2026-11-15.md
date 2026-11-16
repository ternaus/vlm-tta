# Gemini 3 Flash Preview single-pass baseline on MMMU validation

Gemini 3 Flash Preview answered **708 of 847 questions correctly (83.59%)** in
one pass. ScreamingFace published and verified the result on November 15, 2026.
[Leaderboard entry](https://leaderboard.screamingface.ai/v1/public/scores/020151ad-8990-4133-bf21-806b0ad99d09) ·
[Complete capture](https://leaderboard.screamingface.ai/v1/public/scores/by-pinned-cache/8cba65f5-a510-4c4f-8a8a-a466b7ff7bc6)

## Configuration and inputs

The run establishes the one-call baseline for the TTA comparisons. It also
checks whether a 12-hour client window allows the hosted evaluation to complete
after the previous GCP client reached its six-hour limit in the queue. The retry
kept the model, prompt, image preprocessing, scorer, and case selection fixed.

- [Run configuration](mmmu-validation-gemini-flash-gcp-2026-11-15.toml)
- [Compiled URL4 recipe](results/mmmu-validation-gemini-flash-gcp-2026-11-15/recipe.url4)
- [Per-case answers and grades](results/mmmu-validation-gemini-flash-gcp-2026-11-15/answers.csv)
- [Run summary](results/mmmu-validation-gemini-flash-gcp-2026-11-15/summary.json)

The ScreamingFace 0.1.12 client ran on GCP and submitted
`inspect-mmmu_multiple_choice` revision `5b008891134ef4c1` with capture enabled.
The hosted benchmark supplied the images and grades. Its MMMU importer resizes
images to fit within 1024 × 1024 pixels. The candidate uses the supplied
images, without further resizing or crops. The model route was
`openrouter/google/gemini-3-flash-preview`, with temperature 0, a 1,024-token
output limit, web search disabled, and response-cache reuse disabled.
[MMMU image importer](https://github.com/UKGovernmentBEIS/inspect_evals/blob/v0.20.0/src/inspect_evals/mmmu/utils.py)

The source checkout was based on `3f9b3116c80271fdc760dc8ff29211910f6c55b9`.
The SHA-256 hashes of the runner and lockfile used on the VM were
`3b53e9e8e823e3acee66aa9a4f7fc1e0d4d87c14d68773acb68a710e1b192480` and
`78d6fa90a8f9475db6c3b0aa23fc4a291be4718429b78b98b19e2dc51c34999f`.

## Results

The client submitted at 10:50:26 UTC. The first provider call was observed at
10:50:35 UTC and the evaluation finished at 11:34:29 UTC, after 44 minutes.

| Measure | Result |
| --- | --- |
| Cases scored | 847 of 847, with all case IDs 1–847 present exactly once |
| Cases with an extracted answer choice | 832 of 847 |
| Accuracy | 83.59% (708 of 847) |
| Capture | Complete |
| Provider calls | 847, with zero case retries or response-cache hits |
| Tokens | 1,152,459 input and 227,792 output |
| Inference cost | USD 1.2596055, reported as complete |

All 15 responses that reached the output limit had no extracted choice and
scored zero. These rows have an empty `answer` and `finish_reason=length` in the
CSV. The CSV comes from the exported ScreamingFace report. The published
benchmark revision, score, case count, recipe, and capture match the saved run.
