# Four same-image samples on MMMU validation from a GCP client

Four same-image calls to Gemini 3 Flash Preview with voting scored
**730/847 (86.19%)**. ScreamingFace published and verified the result on
November 17, 2026.
[Leaderboard entry](https://leaderboard.screamingface.ai/v1/public/scores/e9cf5228-be44-4260-b752-fae652c8534e) ·
[Complete capture](https://leaderboard.screamingface.ai/v1/public/scores/by-pinned-cache/55785451-26f5-4ba0-bbdd-e979faf2e931) ·
[Configuration](mmmu-validation-gemini-flash-same-image-vote-2026-11-17-gcp.toml)

The local client lost its WebSocket before ScreamingFace returned a report.
This retry runs the same hosted evaluation from the existing GCP VM, where
the published single-pass baseline completed. The hypothesis is that the VM
client can keep the connection long enough to receive the full report. The
frozen inputs, model, four seeds, generation settings, grader, vote, capture
and cache policy are unchanged. The target is one complete ScreamingFace
report for benchmark revision `5b008891134ef4c1` and all 847 questions.

The local attempt's observed tokens project about $4.83 for one full run at
OpenRouter's standard rates. At 4096 output tokens per call and the first
baseline's input volume, the estimate is $43.94 before retries. The planning
budget is $50 in ScreamingFace grant credits. The existing `e2-small` VM stops
within 150 minutes and the systemd client has a 135-minute limit. At
$0.01675/hour, the VM compute ceiling is about $0.042, before network use.
[OpenRouter rates](https://openrouter.ai/google/gemini-3-flash-preview) ·
[Compute Engine rates](https://cloud.google.com/products/compute/pricing/general-purpose)

The checkout is based on `55bb44e1a5e494356c62790dd4ef81a59c6c8f1d` and the SDK
is ScreamingFace 0.1.12. The runner SHA-256 is
`91fcd743a117c5151d952f2e7d5e9cf94dec801890c37511f3d4ca40d955ac73` and the
lockfile SHA-256 is `78d6fa90a8f9475db6c3b0aa23fc4a291be4718429b78b98b19e2dc51c34999f`.

## Launch

The `vlm-tta-mmmu-4vote-20261117.service` systemd unit launched from the
existing ScreamingFace 0.1.12 environment. ScreamingFace started Engine run
`zxWrBx0tUTOTl3kmAZaPXoX4fN2KN0kzweDthu25FU4VR70nQYR1TOR6otwEW4mG` on
November 17, 2026 at 03:36:10 UTC. The first three provider spans were
observed by 03:36:28 UTC. Each span covered four model branches and bypassed
response caching. The runner, lockfile, and configuration hashes matched the
local files on the VM.

Ubuntu's unattended system update restarted systemd and stopped the client
at 04:10:30 UTC. The SDK received no final report or capture ID. The archived
provider events cover 464 questions and record 2,484,096 input and 501,990
output tokens, equivalent to about $2.75 at the listed rates. The automatic
restart found the temporary grant credential had been removed after use and
made no new model calls.

## Retry after the VM update

The system update finished at 04:10:38 UTC. The identical runner and
configuration launched again in `vlm-tta-mmmu-4vote-20261117-b.service`
with a 95-minute service limit, before either scheduled package timer could
fire. The original 150-minute Compute Engine stop limit still applies.

ScreamingFace started the run at 04:15:53 UTC and completed it at 05:14:05 UTC
on November 17, 2026. The exported report,
[summary](results/mmmu-validation-gemini-flash-same-image-vote-2026-11-17-gcp-after-update/summary.json),
[URL4 recipe](results/mmmu-validation-gemini-flash-same-image-vote-2026-11-17-gcp-after-update/recipe.url4),
and [provider events](results/mmmu-validation-gemini-flash-same-image-vote-2026-11-17-gcp-after-update/events.jsonl)
match the VM copies by SHA-256. The compiled URL4 matches both interrupted
attempts. Compute Engine confirmed the VM stopped at 05:16:25 UTC after
101 minutes of uptime. Its estimated compute charge is $0.028 before
networking and storage.

## Results

| Measure | Result |
| --- | --- |
| Four same-image calls with voting | **730/847 (86.19%)** |
| Published single pass | 708/847 (83.59%) |
| Paired changes relative to single pass | 31 improved, 9 regressed, net +22 correct (+2.60 percentage points) |
| Extracted votes | Four for all 847 questions, with 29 ties |
| Model calls | 3,388, all with response caching bypassed and normal finish |
| Tokens | 4,609,918 input, 932,161 output, and 12,174 provider prompt-cache reads |
| Duration | 58 minutes 12 seconds |
| Provider span latency per case | 3.35 seconds median and 10.03 seconds p95 |
| Inference cost | USD 5.09596370, reported as complete |
| Capture | Complete, pinned cache `55785451-26f5-4ba0-bbdd-e979faf2e931` |

The [case CSV](results/mmmu-validation-gemini-flash-same-image-vote-2026-11-17-gcp-after-update/answers.csv)
records the four extracted letters, vote and grade for every ID. The
[verification](results/mmmu-validation-gemini-flash-same-image-vote-2026-11-17-gcp-after-update/verification.json)
recomputed the votes and checked that every case has exactly four distinct
model operations and one vote. The 847 provider spans each cover one case and
list four model finish reasons. The comparison with single pass also changes
temperature and the output limit.

The two reports contain the same 847 question IDs and text. The serialized
inputs match on 845 cases. Cases 121 and 132 reuse one image whose PNG differs
between runs. Both copies are 503 × 1024 with the same color profile. The mean
absolute difference is 0.12 on the 0–255 RGB scale, with a maximum channel
difference of 4. Both methods answered both questions correctly. The 845
identical-input cases still show a net gain of 22 correct answers.

The public leaderboard entry and capture match the saved run's benchmark
revision, case count, score and compiled recipe.
