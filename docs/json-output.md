# JSON Output

SignalForge can also emit machine-readable output for scripts and automation.

Use:

```powershell
signalforge run results.jtl --json
```

Preview expectations:

- JSON is useful for pipelines and lightweight automation
- exit-code behavior remains separate from display wording
- compare mode may include a compact compare block in addition to the single-run verdict data

If you are trying the preview for the first time, start with the HTML report and CLI verdict before adding JSON automation.
