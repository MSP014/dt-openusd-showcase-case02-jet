# Pipeline Workflow

## Purpose

This document describes the Case 02 production chain at a durable,
source-agnostic level. It connects public references, Houdini production, USD
composition, telemetry binding, Omniverse playback, and final visual proof
without turning the README into a technical passport.

## Workflow Chain

1. **Public reference collection**
   - Gather permitted references for the Trent 1000-class engine, Testbed 80,
     module layout, scale, visual details, and public performance ranges.
2. **Geometry cleanup and proxy preparation**
   - Import or author source geometry in Houdini.
   - Validate global scale against the 2.85 m fan diameter.
   - Split the engine into stable module zones.
3. **Module zoning**
   - Preserve explicit Fan, compressor, combustion, turbine, simulation, and
     telemetry scopes.
   - Keep heavy geometry behind USD payload boundaries.
4. **Simulation cache generation**
   - Generate or author a temporal VTI velocity dataset and manifest for Idle,
     Takeoff, Cruise, and Max Thrust.
   - Keep the dataset extent, frame count, and streamline count bounded and
     loopable for interactive playback. Houdini OpenVDB density caches may
     support offline authoring or reference, but are not played directly in the
     Omniverse runtime.
5. **USD Payload and VariantSet authoring**
   - Compose lightweight assembly layers, deferred payloads, LODs, and
     operational state variants.
   - Keep static geometry, simulation caches, materials, and telemetry metadata
     separable.
6. **Telemetry schema binding**
   - Bind synthetic telemetry fields to USD primvars and runtime data-provider
     outputs.
   - Preserve semantic separation between temperature, velocity, and material
     representation.
7. **Omniverse Kit HUD playback**
   - Consume the USD package and manifest-selected temporal VTI velocity data
     through the Kit runtime boundary.
   - Use Kit-CAE to expose the field to a bounded NVIDIA Flow smoke tracer;
     use streamlines as the primary geometric velocity representation.
   - Drive state, view, visual mode, camera, and HUD updates from explicit
     contracts.
8. **Visual proof export**
   - Capture stills, clips, or diagrams that prove the pipeline and state matrix
     are inspectable, not only described in prose.

## Boundaries

- Houdini is the production and cache-authoring environment.
- USD is the interchange, composition, payload, VariantSet, and metadata layer.
- Omniverse Kit is the runtime and HUD playback layer; Kit-CAE and NVIDIA Flow
  visualise temporal velocity fields rather than replaying density volumes.
- README stays compact and links to deeper technical documents.
- Validation and limitations are governed by
  [Validation and Limitations](validation_and_limitations.md).
