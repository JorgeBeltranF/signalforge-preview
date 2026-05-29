# Experimental Dynatrace

Dynatrace enrichment is optional, experimental, and advanced-only.

SignalForge remains JTL-first. The JTL report, Product Verdict, Release translation, Advisory content, Compare behavior, JSON output, and CI exit codes work without Dynatrace and are not changed by Dynatrace enrichment.

## What It Adds

When explicitly configured, SignalForge may request three bounded service-signal families from a Dynatrace tenant you control:

- service response-time context
- service error-rate context
- service throughput or request-count context

This context is enrichment only. It is not root-cause analysis, topology validation, service ownership validation, or release authority.

## Activation

Dynatrace activates only through a `dynatrace` block in a YAML context file.

Environment variables alone do not activate enrichment.

Example:

```yaml
schema_version: 2

dynatrace:
  tenant_id: ${DT_TENANT_ID}
  api_token: ${DT_API_TOKEN}
  namespace: example-namespace
```

Token values must be referenced with `${ENV_VAR}` syntax. Raw token values in YAML are rejected.

## Behavior Rules

- Valid block + env-var token reference + env var set: enrichment is attempted.
- Valid block + env-var token reference + env var missing: enrichment is safely disabled with a sanitized warning.
- Raw token in YAML: the block is rejected with a sanitized warning.
- Malformed or incomplete block: enrichment is disabled with a sanitized warning.
- No `dynatrace` block: no enrichment, no warning, no output, even if related environment variables exist.
- Legacy Dynatrace config shapes are not auto-migrated.

In all failure cases, the normal JTL workflow continues.

## Privacy And Safety

SignalForge runs locally and does not send telemetry to SignalForge-owned services.

If Dynatrace enrichment is configured, the run may make outbound requests to the user-configured Dynatrace tenant for that invocation. Tokens are read from environment variables and must not be logged, rendered, serialized, or committed.

This is a local-first posture with an explicit external-enrichment option, not an offline guarantee.

## Current Limits

- Dynatrace enrichment is experimental.
- Tenant configuration varies by organization.
- Missing data is treated neutrally.
- Fetch failures are operational, not evidence about the system under test.
- Scope remains conservative when service identity or namespace coverage is not verified.
- Enrichment-derived evidence must not be treated as confirmation, diagnosis, or cause.

Use this path only if you already understand your Dynatrace tenant and want bounded external context alongside the JTL report.
