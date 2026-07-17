# Reference Geometry and Provenance

## Purpose

This document defines how Case 02 treats reference geometry, scale, and public
source constraints for the jet engine digital twin.

The goal is to make the visual proxy honest: recognisable as a Trent
1000-class high-bypass turbofan for portfolio and pipeline demonstration, while
remaining clear about what is public reference, what is inferred, and what is
not claimed.

## Source Geometry

Case 02 uses public reference material and external local assets as the basis
for a visual proxy engine. Exact source geometry must be documented per asset as
the project moves from reference collection to production USD packages.

Accepted source categories:

- public product specifications;
- public factory-tour and facility references;
- public photographs, diagrams, or datasheets;
- externally stored modelling or scan assets that are allowed for portfolio use;
- Houdini-authored proxy or cleaned geometry derived from those references.

Unaccepted source categories:

- proprietary Rolls-Royce CAD;
- confidential internal dimensions;
- real engine maintenance or test-cell telemetry;
- vendor files whose licence or provenance cannot be explained.

## Scale Anchor

The primary scale anchor is the public Trent 1000 fan diameter:

- **Fan diameter:** 2,850 mm / 2.85 m.
- **Overall length:** 4,738 mm / 4.738 m.
- **Approximate length-to-fan-diameter ratio:** 1.67:1.

When importing or cleaning engine geometry, validate global scale against the fan
diameter first. Internal module proportions may be visually adjusted to support
the cutaway, payload zoning, and telemetry demonstration, but they must not be
presented as exact proprietary dimensions.

## Public Reference Constraints

Public sources are sufficient for the showreel goal: a convincing, structured,
state-driven technical visualisation. They are not sufficient for a
certification-grade propulsion model.

Known constraints:

- internal compressor, combustor, and turbine dimensions are not publicly
  available at the required fidelity;
- exact material specifications and cooling-channel details are out of scope;
- real Testbed 80 sensor placement and high-frequency telemetry streams are not
  available;
- visual mode data is synthetic or procedurally authored unless explicitly
  stated otherwise.

## Visual Proxy Assumptions

The engine representation may use controlled visual proxy assumptions:

- module zoning follows the project USD hierarchy rather than exact proprietary
  part breakdowns;
- dense details may be represented by normal maps, instancing, or simplified
  payloads when they are not visible or useful to the demonstration;
- thermal and velocity visualisation may be physically inspired without being a
  CFD result;
- Houdini simulation caches represent state-driven visual proof, not runtime
  physics.

## What Is Not Claimed

Case 02 does not claim:

- access to proprietary Rolls-Royce CAD;
- exact Trent 1000 internal geometry;
- real Rolls-Royce telemetry;
- exact CFD or combustion fidelity;
- maintenance, safety, or certification suitability.

## Documentation Rule

When a new source asset is introduced, record enough provenance for a future
reviewer to understand:

- where the asset or reference came from;
- why it is acceptable for portfolio use;
- what scale or module assumption it supports;
- what limitations should be preserved in public claims.
