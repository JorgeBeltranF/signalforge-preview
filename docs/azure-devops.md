# Azure DevOps

This preview can be used in Azure DevOps when the SignalForge preview package is available to the pipeline.

The CI path is intentionally simple:

- install the preview package
- run SignalForge with `--ci`
- publish the generated HTML report
- optionally capture JSON with `--json`

## Install Package

Adapt the package path to wherever your pipeline downloads or stores the preview bundle.

```yaml
- task: PowerShell@2
  displayName: "Install SignalForge Preview"
  inputs:
    targetType: inline
    script: |
      py -3 -m pip install .\dist\signalforge_preview-0.1.0-py3-none-any.whl
```

## Single Run

```yaml
- task: PowerShell@2
  displayName: "Run SignalForge"
  inputs:
    targetType: inline
    script: |
      signalforge run results.jtl --ci --output signalforge-report.html
```

## Compare

```yaml
- task: PowerShell@2
  displayName: "Run SignalForge Compare"
  inputs:
    targetType: inline
    script: |
      signalforge compare current.jtl baseline.jtl --ci --output signalforge-compare.html
```

## JSON Artifact

Use `--json` when another pipeline step needs structured output.

```yaml
- task: PowerShell@2
  displayName: "Run SignalForge JSON"
  inputs:
    targetType: inline
    script: |
      signalforge run results.jtl --ci --json > result.json
```

With `--json`, human-readable logs and warnings stay out of `stdout` so the JSON stream remains parseable.

## Exit Codes

Default local mode:

- `GO` -> `0`
- `INVESTIGATE` -> `0`
- `FAIL` -> `2`
- runtime or tool error -> `3`

Strict CI mode with `--ci`:

- `GO` -> `0`
- `INVESTIGATE` -> `1`
- `FAIL` -> `2`
- runtime or tool error -> `3`

`Release` wording is a human-facing translation in the report and CLI. The pipeline gate remains the process exit code.
