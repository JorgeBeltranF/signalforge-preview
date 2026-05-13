# SignalForge Preview

Turn JTL results into a release-oriented review workflow with a compact CLI verdict, a compare-first HTML report, and endpoint-level evidence.

SignalForge Preview is a local, JTL-first tool for engineers who need to review a run quickly, compare it against a baseline, and understand whether the visible change is worth release attention. It is not a load generator, not an observability platform, and not a generic HTML export.

## Local CLI, Fast First Read

```text
signalforge compare examples/quickstart/sample.jtl examples/quickstart/baseline.jtl

=== SignalForge Product Verdict ===
GO - Ready
Release: Proceed

=== SignalForge Compare Summary ===
Overall Compare Posture: Comparable
Comparison Strength: HIGH
Signal: Regression observed
```

That combination matters:

- the CLI gives you a fast release-oriented read
- the HTML report gives you the evidence behind it
- the compare workflow makes regressions easier to review than a raw JTL diff

## Compare-First Review

![SignalForge compare report](./media/screenshots/compare-hero.png)

## Why SignalForge Preview

- Compare comes first. The strongest workflow is not just "render one report," but "show me what changed and whether it matters."
- The output is release-oriented. The report is built to help engineers, QA leads, and technical managers scan risk quickly.
- Endpoint evidence stays visible. You can move from executive summary to endpoint-level changes without leaving the report.
- The workflow stays local. JTL in, CLI plus HTML out, no hosted platform required for the basic preview path.
- Healthy runs still read cleanly. The tool is not only for red failure cases or catastrophic demos.

## Quickstart

The lowest-friction path is to run SignalForge from a preview bundle or local preview checkout that includes the packaged CLI files and the `examples/` folder.

```powershell
py -3 -m venv .venv
.\.venv\Scripts\python.exe -m pip install .\dist\signalforge_preview-<version>-py3-none-any.whl
cmd /c ".venv\Scripts\activate.bat && signalforge run examples/quickstart/sample.jtl --config examples/quickstart/sample_run_config.csv"
cmd /c ".venv\Scripts\activate.bat && signalforge compare examples/quickstart/sample.jtl examples/quickstart/baseline.jtl --config examples/quickstart/sample_run_config.csv --compare-config examples/quickstart/baseline_run_config.csv"
```

What to expect:

- the single-run command writes an HTML report next to the sample JTL unless `--output` is provided
- the compare command writes a compare report that highlights posture, deltas, and endpoint changes
- the sample config files make the demo report identity and context cleaner, but your own JTLs can still be reviewed without advanced setup

More detail:

- [Install notes](./docs/install.md)
- [Quickstart walkthrough](./docs/quickstart.md)

## Single-Run Executive View

![SignalForge single-run report](./media/screenshots/single-run.png)

Single-run still matters. A calm healthy run should be readable without noise, and a reviewer should be able to find the top-line verdict, release posture, and next action in a few seconds.

## Why Compare Matters

A single run can tell you whether a result looks healthy or risky. Compare tells you whether a new run changed in a way that deserves attention.

SignalForge Preview is strongest when you want to:

- review a candidate run against a baseline
- see whether the release posture actually shifted
- identify which endpoints changed the most
- keep the summary readable for both hands-on engineers and review stakeholders

## Endpoint-Level Evidence

![SignalForge endpoint comparison](./media/screenshots/endpoint-evidence.png)

The compare surface is not only a headline. It also gives you endpoint-level evidence so you can inspect changed transactions, search quickly, and sort the drilldown by the dimension that matters most.

## Preview Scope

This preview is intentionally focused:

- JTL-first input
- local and offline-friendly workflow
- CLI plus HTML review flow
- single-run interpretation
- compare-oriented regression review

This preview is intentionally not:

- a hosted SaaS product
- an observability platform
- a load generator
- an automated release authority

## Known Limitations

- This is still a preview surface, not a broad public release.
- The current public demo set is intentionally small and curated.
- Some deeper workflows and integrations are out of scope for the first preview.
- Endpoint demo data is useful today, but still lighter than a richer future curated fixture.

For a concise limitations summary, see [Known limitations](./docs/known-limitations.md).

## Download Preview

The intended install path for early users is a curated preview package rather than a broad source dump.

That package should include:

- the preview CLI package files
- safe sample JTLs
- sample config files
- install instructions
- the screenshot set shown here

## Preview License

SignalForge Preview is currently source-available for evaluation, internal experimentation, and technical feedback.

It is not yet offered under a final open source or commercial license. See [LICENSE](./LICENSE) for the current preview terms.

## Feedback

Useful feedback for this preview includes:

- install friction
- time to first useful insight
- compare usefulness
- confusing wording
- trust concerns

See [Feedback](./docs/feedback.md).

## Preview Notice

SignalForge Preview is meant to be practical, not theatrical. The goal is to make JTL review and compare easier to understand, easier to discuss, and easier to act on without pretending to replace engineering judgment.

## Preview Direction

Planned workflow expansion may include:

- historical compare and review workflows
- optional APM-compatible evidence correlation
- broader release-review analysis flows

These are not part of the current preview workflow.
