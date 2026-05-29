# Known Limitations

This preview is intentionally focused. Current limitations include:

- installation still depends on normal Python package downloads
- the supported Windows onboarding path favors `cmd /c` activation over PowerShell-first activation
- preview defaults are visible, but they are still generic and may not match your organization-specific thresholds
- sample inputs are intentionally small and safe; they demonstrate workflow, not production complexity
- the degraded sample is included to show report behavior for a problematic run, not to represent production load complexity
- advanced configuration exists, but it is optional and not the recommended starting path
- Dynatrace enrichment is optional, experimental, advanced-only, YAML-only, and limited to three bounded service-signal families
- Dynatrace enrichment does not influence Product Verdict, Release, Advisory, Compare, JSON, or CI exit-code behavior
- tenant-specific Dynatrace configuration can vary, so enrichment may be unavailable or incomplete even when the JTL report succeeds

The preview is designed to be useful quickly, not to represent every advanced workflow on day one.
