# Quickstart

Goal:

- go from local install to first useful SignalForge report quickly

## 1. Get the preview package

The public preview is intended to be installed from a curated release package rather than directly from this repository surface.

## 2. Install

```powershell
py -3 -m venv .venv
.\.venv\Scripts\python.exe -m pip install .\dist\signalforge_preview-0.1.0-py3-none-any.whl
cmd /c ".venv\Scripts\activate.bat && signalforge --version"
```

This Windows path is used because it is reliable for first-run preview validation and avoids common PowerShell activation friction.

## 3. Generate the first report

```powershell
cmd /c ".venv\Scripts\activate.bat && signalforge run examples/quickstart/sample.jtl"
```

Expected result:

- the CLI prints a compact `Product Verdict`
- an HTML report is written next to the sample JTL unless `--output` is provided
- the preview also writes `resolved_context.json` and an `__all_transactions.csv` export next to the generated HTML

## 4. Try compare

```powershell
cmd /c ".venv\Scripts\activate.bat && signalforge compare examples/quickstart/sample.jtl examples/quickstart/baseline.jtl"
```

Expected result:

- the CLI prints the compare summary
- the HTML report highlights compare posture, deltas, and endpoint-level changes
- the compare path also writes an `__all_transactions.csv` export for the generated compare HTML

## 5. Try the degraded sample

```powershell
cmd /c ".venv\Scripts\activate.bat && signalforge run examples/quickstart/sample_degraded.jtl"
```

Expected result:

- the report shows how SignalForge presents a problematic run
- a `FAIL` exit code is expected for this sample
- this is a demo fixture, not a production benchmark

## 6. Read the report in this order

1. `Product Verdict`
2. `Release`
3. `Action`
4. `Evidence behind this verdict`
5. `Endpoint Comparison`

## 7. Optional context

SignalForge preview is JTL-first. You can get a meaningful report without extra setup.

Optional config files in `examples/config/` are there for teams who want reusable metadata or controlled overrides later.
