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

## Tenant Variability

Dynatrace tenants vary significantly in how they are configured. SignalForge
draws a deliberate line between platform behavior it assumes is stable and
tenant configuration it expects to vary.

Assumed stable across tenants:

- Token-based authentication model
- Metrics API endpoint structure
- Standard HTTP semantics

Expected to vary by tenant:

- Service, host, and namespace naming conventions
- Instrumentation depth and coverage gaps
- Tagging discipline and tag availability
- Management zone definitions and RBAC scoping

SignalForge behavior under that variability:

- Enrichment is opt-in and never required for the JTL verdict
- Scope is conservative when namespace is unconfirmed
- Missing data is treated neutrally, not as evidence
- Fetch failures are operational, not semantic
- Confidence is capped at MEDIUM for any enrichment-derived signal
- No causation, root cause, or attribution claims are made from enrichment

The goal is to degrade safely and honestly across heterogeneous tenants,
not to support every tenant's configuration perfectly.

## What You Will See in the Report

When enrichment runs successfully, additional context appears in the
Technical Analysis tab as a sanitized block reporting what Dynatrace
observed during the JTL correlation window. Each signal carries an
availability and confidence label.

When enrichment is unavailable — no token, missing namespace, fetch failure,
no data, malformed response — the block either renders an empty state with a
sanitized reason or does not render. JTL analysis remains the source of
verdict and advisory in all cases.

## Current Limits

- Dynatrace enrichment is experimental.
- Tenant configuration varies by organization.
- Missing data is treated neutrally.
- Fetch failures are operational, not evidence about the system under test.
- Scope remains conservative when service identity or namespace coverage is not verified.
- Enrichment-derived evidence must not be treated as confirmation, diagnosis, or cause.

Use this path only if you already understand your Dynatrace tenant and want bounded external context alongside the JTL report.
