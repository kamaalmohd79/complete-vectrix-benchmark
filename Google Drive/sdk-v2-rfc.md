# RFC: SDK v2 — Standardized Interface & Auto-Instrumentation

**Author:** Liam Nakamura (liam@vectrix.io)  
**Date:** January 15, 2026  
**Status:** DRAFT — Planning Phase  
**Linear Project:** VEC-SDK-V2  
**Target:** April 2026

---

## Summary

The current Vectrix SDKs (Python, Node.js, Go) work but have accumulated inconsistencies that hurt developer experience and create maintenance burden. This RFC proposes a full rewrite of all three SDKs under a unified design spec, plus the addition of auto-instrumentation as a first-class feature.

---

## Problems with SDK v1

1. **Inconsistent interfaces across languages.** Python uses `vectrix.track_request()`, Node.js uses `client.emit()`, Go uses `client.Send()`. Same action, three different names.
2. **No auto-instrumentation.** Every customer writes manual wrapping code. This is friction at signup.
3. **Sparse documentation.** README-level docs only. No type references, no examples for edge cases.
4. **Error handling is inconsistent.** Python silently swallows errors; Go panics if the channel is full; Node.js returns a rejected Promise.
5. **No retry logic.** If the Vectrix endpoint is unavailable, events are lost. No buffering, no retry.
6. **Batch size is hardcoded.** 100 events per batch in all SDKs, no configurability.

---

## Proposed Design

### Unified Method Naming

All SDKs will expose the same logical interface:

```
client.capture(event)           # Single event
client.capture_batch(events)    # Batch of events
client.flush()                  # Flush buffer
client.close()                  # Shutdown gracefully
```

Language-idiomatic naming (snake_case for Python, camelCase for JS) is fine as long as the logical names map 1:1.

### Auto-Instrumentation

Each SDK will include optional auto-instrumentation that hooks into the framework's middleware layer without requiring manual `capture()` calls:

**Python:**

```python
import vectrix
vectrix.instrument_flask(app)    # or instrument_fastapi, instrument_django
```

**Node.js:**

```javascript
import { instrumentExpress } from "@vectrix/sdk";
instrumentExpress(app);
```

**Go:**

```go
// TBD — evaluating middleware approach vs net/http wrapper
// Options: chi middleware, stdlib http.Handler wrapper, or OpenTelemetry-compatible
```

### Error Handling — Standard Behavior

All SDKs will:

1. Never throw/panic on event capture failures — errors go to an optional error callback
2. Buffer events locally (configurable size, default 1000) if network is unavailable
3. Retry with exponential backoff (3 attempts, max 30s backoff)
4. Drop events silently if buffer is full (configurable: drop vs block)

### Configuration

```python
# Python example — same options available in all SDKs
client = vectrix.Client(
    api_key="vx_live_...",
    endpoint="https://ingest.vectrix.io/v1/events",
    batch_size=200,           # default: 100
    flush_interval_ms=1000,   # default: 1000
    buffer_max=5000,          # default: 1000
    on_error=lambda e: logger.warning(f"Vectrix error: {e}")
)
```

---

## Docs Strategy

Moving from README-only to a full Mintlify docs site at docs.vectrix.io. Current docs are ~40% complete. SDK v2 launch is blocked on docs reaching at least 80%.

**Docs structure:**

- Quickstart (5 minutes to first event)
- SDK reference: Python, Node.js, Go
- Auto-instrumentation guides per framework
- Configuration reference
- Troubleshooting

**TBD:** Who writes the docs? Liam writes the technical reference; marketing copy and quickstart needs Dana's input. Timeline unclear — this is the most likely thing to slip the April target.

---

## Breaking Changes

SDK v2 is a breaking change. We'll need to:

1. Publish v2 under new package names (`vectrix-sdk` → `@vectrix/sdk` for Node.js, etc.) OR bump major version
2. Maintain v1 in security-only mode for 6 months post-v2 launch
3. Notify affected customers (14 paying, all on v1) in advance

**Customer communication plan:** TBD. Tessa and Dana to align on messaging. Target: 30 days advance notice before deprecation.

---

## Open Questions

1. **Go auto-instrumentation approach** — middleware vs wrapper is undecided. Needs spike (2 points). Liam to prototype both and choose by end of Sprint 13.
2. **Docs ownership** — see above
3. **Python packaging** — stay on PyPI `vectrix` or move to `vectrix-sdk`? Preference for staying on `vectrix` with major version bump, but needs confirmation.
4. **OpenTelemetry compatibility** — multiple enterprise prospects have asked about OTEL integration. Out of scope for v2, but worth designing around.

---

## Timeline

| Milestone             | Target       | Owner       |
| --------------------- | ------------ | ----------- |
| Design spec finalized | Feb 28, 2026 | Liam        |
| Python SDK v2 alpha   | Mar 14, 2026 | Liam        |
| Node.js SDK v2 alpha  | Mar 14, 2026 | Liam        |
| Go SDK v2 alpha       | Mar 21, 2026 | Liam        |
| Docs 80% complete     | Mar 28, 2026 | Liam + Dana |
| Customer beta         | Apr 1, 2026  | TBD         |
| GA launch             | Apr 2026     | TBD         |

---

_Liam Nakamura — January 15, 2026_
