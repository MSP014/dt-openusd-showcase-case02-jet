# Guideline 03: Houdini Simulation Caches and States

A static jet engine is just a CAD model. To demonstrate the state-driven digital
twin visualisation workflow, Case 02 uses Houdini-authored temporal VTI velocity
datasets, manifests, and streamline geometry to qualitatively represent flow,
thermal, and combustion-state cues. In Omniverse, Kit-CAE exposes the selected
velocity field to a bounded NVIDIA Flow smoke tracer. These are visual
communication assets, not validated engineering simulation results.

Houdini OpenVDB density caches may remain useful during authoring, but direct
VDB density-sequence playback is not a runtime path: it does not meet the
interactive playback target.

These caches must switch dynamically based on the 4 engine regimes:

- **Idle**
- **Takeoff**
- **Cruise**
- **Maximum Thrust**

## 1. Rule of Segregation

Never mix static mechanical geometry with dynamic simulation caches in the same USD file.

- **Mechanical Assembly:** `/Engine/Combustion_Chamber` (Payload)
- **Simulation Data:** `/Engine/Simulations/Combustion_Flame` (Payload)

This ensures the mechanical team and FX team can work independently without locking files.

## 2. Managing Simulation States (VariantSets)

The 4 engine regimes are handled via a USD **VariantSet** applied to the `/Engine/Simulations` hierarchy.

1. **VariantSet Name:** `EngineRegime`
2. **Variants:** `Idle`, `Takeoff`, `Cruise`, `MaxThrust`.

By switching the `EngineRegime` variant at the top level, the runtime selects
the corresponding field entry from the temporal VTI manifest and updates the
streamlines and Flow tracer configuration for that regime.

## 3. Handling Time-Varying Data

Each regime uses a temporal VTI velocity dataset with explicit manifest metadata
for frame sequence, timing, spatial extent, units, and field names.

- Do **not** author one density-volume payload per frame for runtime playback.
- Export the temporal velocity field from Houdini and register it in a
  lightweight manifest.
- At runtime, Kit-CAE reads the selected field; NVIDIA Flow uses it only to
  advect a bounded smoke tracer. It is not a live CFD or combustion simulation.

---

## ✅ Definition of Done (DoD)

- [ ] Combustion-state and exhaust visualisation are separated from mechanical geometry.
- [ ] An `EngineRegime` VariantSet exists at the `/Engine/Simulations` root level to toggle states.
- [ ] Each regime has a temporal VTI velocity entry in the manifest, with
  documented timing, extent, and field metadata.
- [ ] Direct OpenVDB density-sequence playback is absent from the Omniverse
  runtime path.
