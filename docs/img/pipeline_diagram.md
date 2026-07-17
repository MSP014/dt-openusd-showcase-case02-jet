# Case 02 Pipeline Diagram

```mermaid
flowchart LR
    A[Public references] --> B[Houdini geometry cleanup]
    B --> C[Module zoning]
    C --> D[Pre-baked simulation caches]
    D --> E[USD payloads and VariantSets]
    C --> E
    E --> F[Telemetry schema binding]
    F --> G[Omniverse Kit HUD playback]
    G --> H[Visual proof export]
```

This diagram is a lightweight proof layer for the Case 02 pipeline. It shows the
intended production flow without claiming live physics, proprietary geometry, or
certification-grade CFD.
