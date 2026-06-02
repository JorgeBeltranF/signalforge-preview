# Experimental Dynatrace

Dynatrace enrichment is optional, experimental, and advanced-only.

SignalForge remains JTL-first. The JTL report, Product Verdict, Release translation, Advisory content, Compare behavior, JSON output, and CI exit codes work without Dynatrace and are not changed by Dynatrace enrichment.

## What It Adds

When explicitly configured, SignalForge may request three bounded service-signal families from a Dynatrace tenant you control:

- service response-time context
- service error-rate context
- service throughput or request-count context

This context is enrichment only. It is not root-cause analysis, topology validation, service ownership validation, or release authority.

## Activation — from zero to first enrichment

Dynatrace activates only through a `dynatrace` block in a YAML context file.
Environment variables alone do not activate enrichment.

A ready-to-copy template lives at `examples/config/dynatrace.yaml`.

### 1. Create a Dynatrace API token

Create an API token with a single scope:

- Read metrics (`metrics.read`)

No other scope is required. SignalForge only calls the Metrics v2 API
(`/api/v2/metrics/query`) for three service metrics: service response time,
service error rate, and service request count.

### 2. Find your environment ID

Use only the Dynatrace SaaS environment ID, not the full URL.

```text
URL:        https://abc12345.live.dynatrace.com
tenant_id:  abc12345
```

Preview limitation: Dynatrace Managed and `*.apps.dynatrace.com` tenants are
not supported by this client path.

### 3. Set the environment variables

The raw token must never be written in the YAML. Reference it from an
environment variable.

PowerShell:

```powershell
$env:DT_TENANT_ID = "abc12345"
$env:DT_API_TOKEN = "dt0c01.XXXXXXXX..."
```

### 4. Create the context file

```yaml
schema_version: 2

dynatrace:
  tenant_id: ${DT_TENANT_ID}
  api_token: ${DT_API_TOKEN}
  namespace: my-load-test-scope
```

See `examples/config/dynatrace.yaml` for a fully commented version.

### 5. Run with the context file

```powershell
signalforge run results.jtl --context dynatrace.yaml
```

If enrichment succeeds, a Dynatrace block appears in the Technical Analysis
tab, and — when the data is actionable — an Evidence Relationships
(Experimental) section appears in the Insights tab.

## Field Reference

### `tenant_id`

The Dynatrace SaaS environment ID (the subdomain only), not the full URL.
May be a literal or an `${ENV_VAR}` reference.

Preview limitation: Dynatrace Managed and `*.apps.dynatrace.com` tenants are
not supported by this client path.

### `api_token`

Use an API token with the Read metrics (`metrics.read`) scope. Do not place the
raw token in the YAML. Use an environment variable such as `${DT_API_TOKEN}`.
Raw token values in YAML are rejected.

### `namespace`

`namespace` is a user-provided execution scope hint.

In this preview, SignalForge does not use `namespace` to filter Dynatrace API
queries or verify service ownership. Dynatrace metrics are queried at the
tenant service level and grouped by service entity.

When `namespace` is configured, SignalForge treats the Dynatrace evidence as
eligible for bounded enrichment with `MEDIUM` confidence, because the user has
explicitly provided an expected workload scope. Without it, the evidence stays
at `LOW` confidence and is treated as unscoped.

Service-entity scope verification is deferred for this preview.

Use `namespace` only when it meaningfully describes the workload or environment
you are evaluating. Do not treat it as attribution or proof that a Dynatrace
service belongs to the JTL workload.

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

When enrichment runs with actionable data, an Evidence Relationships
(Experimental) section also appears in the Insights tab. It shows, per signal
family, whether the JTL observation and the Dynatrace signal were
independently corroborated during the same execution window. It does not claim
causality, attribution, or root cause.

## Current Limits

- Dynatrace enrichment is experimental.
- Tenant configuration varies by organization.
- Missing data is treated neutrally.
- Fetch failures are operational, not evidence about the system under test.
- Scope remains conservative when service identity or namespace coverage is not verified.
- Enrichment-derived evidence must not be treated as confirmation, diagnosis, or cause.

Use this path only if you already understand your Dynatrace tenant and want bounded external context alongside the JTL report.
