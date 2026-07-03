# ADR 006: Omniverse Runtime Boundary and Portability

## Status

Accepted

## Context

Case 02 separates Houdini/Solaris production from the Omniverse runtime layer.
Houdini produces USD, VDB, textures, cache metadata, telemetry primvars, and
simulation state packages. The Omniverse Kit application consumes these outputs
to present an interactive L1 digital twin.

The runtime must not become a workstation-bound monolithic scene. It should be
structured around explicit contracts so the project remains reproducible,
inspectable, and portable after the USD/simulation package is shipped.

Full containerisation, browser streaming, cloud GPU execution, and distributed
service splitting are not part of the current production scope. They remain
possible future packaging paths, not immediate implementation requirements.

## Decision

The Case 02 Omniverse application must be designed as a contract-driven runtime
layer, not as a local-only scene file.

The runtime must consume explicit contracts:

- USD composition root;
- hydrated external asset package;
- operational state definitions;
- visual mode definitions;
- telemetry schema;
- camera and view presets;
- relative asset paths;
- performance boundaries for payloads, LODs, and simulation caches.

Houdini `.hip` files, raw simulation authoring workflows, exploratory renders,
and workstation-specific paths remain outside the runtime boundary.

The runtime architecture must remain portable enough to support future
containerisation or remote execution. Container images, cloud GPU execution,
browser streaming, and orchestration are explicitly out of scope until the
USD/simulation proof is shipped.

## Consequences

- The Omniverse app stays aligned with the existing USD architecture documents
  instead of inventing parallel service boundaries too early.
- Asset hydration from ADR 005 becomes part of the runtime contract.
- Future packaging work has a clear direction without expanding the current
  Houdini/USD production scope.
- Any future viewer implementation must prove portability through explicit
  configuration, relative paths, documented launch steps, and no hidden local
  state.
