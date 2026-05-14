# Evaluation Defaults

SignalForge Preview can analyze a JTL with no extra config. When you do that, the tool uses generic preview defaults so the report can still produce a readable verdict, compare posture, and threshold context.

That is useful for first-run evaluation, but those values are not meant to stand in for your production SLAs, SLOs, NFRs, or release policy.

## Simple mental model

| Category | Current preview support |
| --- | --- |
| Stable public overrides | Yes, a small subset |
| Advanced context enrichment | Partial |
| Full evaluation-policy tuning | Not yet exposed |

In practice, that means:

- the preview lets you tune a few visible decision inputs
- context and profile files help the report explain a run more accurately
- some evaluation policies remain fixed on purpose so first-run interpretation stays conservative and comparable

## Current preview defaults

| Default | Current preview value | Used for |
| --- | --- | --- |
| APDEX T | `1000 ms` | APDEX scoring |
| P95 latency target | `5000 ms` | default global latency evaluation |
| Error-rate target | `2.0%` | default global error evaluation |
| Compare degradation threshold | `10%` | meaningful compare delta classification |
| Minimum samples per endpoint | `200` | endpoint-level sufficiency checks |
| Normalization sample floor | `500 total samples` | aggregate sample-quality classification |
| Minimum representative duration | `10 min` | representative-duration checks |

## Why these defaults exist

- They let a JTL-first workflow produce a useful first verdict without forcing setup before first value.
- They make compare output readable even when a user has not defined team-specific policy yet.
- They are intentionally generic and should be treated as starting points, not release authority.

## When defaults become dangerous

Preview defaults become misleading when:

- your latency targets are tighter than the preview defaults
- your acceptable error rate is lower than `2.0%`
- your compare posture depends on a different regression threshold than `10%`
- your workload is too short or too small for the default sufficiency checks to feel representative

In those cases, the report is still useful, but the verdict should be treated as directional guidance until thresholds are aligned with your workload and release expectations.

## Override paths available today

The current preview intentionally keeps override paths narrow.

### Stable public overrides

- `apdex_t` can come from the run config CSV or be overridden with `--apdex-t`
- compare degradation can be overridden with `--degradation-threshold`

`--apdex-t` matters when your workload needs a different response-time tolerance for APDEX scoring than the preview default.

`--degradation-threshold` matters when you want compare mode to be stricter or looser about what counts as a meaningful delta.

Example CLI overrides:

```powershell
signalforge run results.jtl --apdex-t 1500
signalforge compare current.jtl baseline.jtl --degradation-threshold 15
```

Example run config CSV snippet:

```csv
run_id,release-candidate-01
track,preview
apdex_t,1500
```

### Context and profile files

`--context` and `--profile` are mainly for:

- workload context
- environment metadata
- compare context
- report provenance
- interpretation enrichment

They are useful when you want the report to better describe what kind of run was executed and under what conditions.

They are not yet a full public evaluation-policy tuning surface.

### What remains intentionally fixed

Some evaluation policies remain intentionally fixed in the preview today, including:

- sample sufficiency policy
- normalization floors
- representative-duration guidance

That is deliberate. Keeping those checks fixed helps preserve interpretation consistency, reduces noisy conclusions from low-sample runs, and keeps the preview focused on conservative directional guidance instead of policy-heavy tuning.

### Tuning tradeoffs

Tuning can help, but it also changes how easy the output is to trust.

- lower thresholds can increase noise sensitivity
- weak sample counts can destabilize percentile interpretation
- compare deltas become less trustworthy when the underlying evidence is thin
- mismatched APDEX settings between runs can make APDEX deltas less useful in compare mode

## Provenance in the generated report

Generated reports include a `Thresholds & Evaluation Context` section. That section is there to show whether visible decision criteria came from preview defaults or custom inputs.

Use that section as a quick trust check before treating a verdict as release-relevant.
