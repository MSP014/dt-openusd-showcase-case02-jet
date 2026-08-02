# Validation and Limitations

## Purpose

Case 02 is a portfolio-grade, L1-oriented digital twin visualisation prototype
for a Trent 1000-class jet engine in a Testbed 80 context. It demonstrates
pipeline architecture, state-driven representation, telemetry binding, USD
composition, and technical storytelling. The current implementation uses
synthetic telemetry and is not connected to a physical engine or test cell.

It is not a certification-grade propulsion model, a CFD research contribution,
or a claim of access to proprietary Rolls-Royce geometry or telemetry.

The Houdini-authored temporal velocity data, streamlines, thermal treatment,
and bounded NVIDIA Flow smoke tracing are physically inspired qualitative
representations for engineering communication. They are not validated CFD,
combustion, thermal, or structural-analysis results.

## What Is Validated

The project validates the behaviour and structure of the artefact, not the
physical accuracy of an operational engine.

- **Scale consistency:** the engine is scaled against public Trent 1000
  reference dimensions, especially the 2.85 m fan diameter.
- **USD hierarchy:** the master stage follows the Case 02 USD contract and uses
  stable module names for Fan, compressor, combustion, turbine, simulation, and
  telemetry scopes.
- **Payload behaviour:** heavy geometry and simulation caches are kept behind
  USD payloads so the stage can open and be inspected without loading every
  heavy asset at once.
- **Operational state switching:** the `EngineRegime` / operational state
  structure switches between Idle, Takeoff, Cruise, and Max Thrust
  representations without multiplying caches by camera angle.
- **Visual mode separation:** Normal, X-Ray Thermal, and Velocity Vectors modes
  preserve semantic separation between photorealistic material, temperature,
  and velocity representation.
- **Telemetry queryability:** telemetry fields are exposed through the agreed
  schema and can be queried by Python or HUD logic.
- **Runtime boundary:** Houdini-authored assets and simulation caches remain
  separate from the Omniverse Kit runtime layer.
- **Performance intent:** payloads, LODs, proxy geometry, manifest-driven
  temporal VTI data, bounded Flow tracers, and reduced streamline counts are
  used to keep the project oriented towards interactive playback. Direct
  OpenVDB density-sequence playback is intentionally excluded from the runtime
  path.

## What Is Not Claimed

- No certification-grade CFD.
- No predictive aerodynamic, thermal, combustion, or structural performance.
- No flight-qualified, maintenance-qualified, or safety-critical analysis.
- No operational L1 connection to a physical engine or test cell.
- No real Rolls-Royce Trent 1000 telemetry.
- No proprietary Rolls-Royce internal geometry.
- No guarantee of exact internal dimensions for compressor, combustor, or
  turbine stages.
- No live physics simulation at runtime.
- No direct runtime playback of Houdini OpenVDB density caches.
- No claim that NVIDIA Flow smoke tracing is a combustion, buoyancy, fuel, or
  predictive fluid simulation.
- No claim that the synthetic telemetry matches a specific real engine run.
- No claim that visual thermal or velocity fields are physically exact.

## Validation Evidence To Produce

As implementation progresses, validation should be recorded with small,
inspectable checks:

- `pxr` stage-open smoke checks for key USD files.
- checks that expected VariantSets and variants exist.
- checks that payloads are present and load selectively.
- checks that telemetry primvars or schema fields are queryable.
- checks that README claims match the artefacts currently present.
- short visual captures proving the state and mode matrix is not just described
  in prose.

## Claim Discipline

Public documentation should describe Case 02 as a technical-art and pipeline
prototype. It may discuss physically inspired visualisation and engineering
constraints, but it should not imply that the project has produced a validated
propulsion simulation, proprietary engineering model, or real test-cell data
product.
