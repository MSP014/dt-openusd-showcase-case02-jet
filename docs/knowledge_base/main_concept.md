# Case 02: Jet Engine Digital Twin (Trent 1000) — Main Concept

> **Philosophy:** "Engineering in Motion" - A data-driven, interactive, L1-oriented digital twin visualisation prototype for a Trent 1000-class turbofan engine in a Testbed 80-inspired context, demonstrating predefined operating regimes through physically inspired, state-driven representations.

---

## 1. High-Level Concept

**Project:** Autonomous NVIDIA Omniverse Kit Application.
**Mission:** Communicate qualitative internal flow, thermal, combustion, and component-state behaviour across four operational states - from standby to maximum thrust - through seamlessly switchable photorealistic, thermal, and flow-vector modes.
**Narrative Context:** The engine is presented in a **Testbed 80-inspired** environment (see [Testbed 80 Facility Details](reference_material/Testbed_80_Facility_Details.md)). The testbed reference provides useful physical context for sensor points, readouts, and mounting structures without implying an exact digital replica of the real facility.
**Engineering Logic:** A single pre-baked visualisation matrix (4 states x looping USD VariantSets) drives all visual layers, with zero live physics computation at runtime.

---

## 2. Operational States (Simulation Matrix)

This document is the current source of truth for the Case 02 operational matrix:
engine regime, view mode, visual mode, and module selection. Do not create a
competing matrix document unless the matrix is deliberately extracted into a
single dedicated contract and linked back here.

All states are **pre-baked in Houdini (Solaris/PDG)** and looped as USD VariantSets. Each state has a distinct qualitative flow and thermal visual signature.

| State | Throttle | Description | Visual Signature |
| :--- | :--- | :--- | :--- |
| **Idle** | ~5% | Ground standby / testbed warm-up | Low rotational speed, smooth low-velocity flow cues, cool exhaust |
| **Takeoff** | 100% | Peak thrust scenario | Highest nominal spool speeds, high-velocity flow cues, intense exhaust plume |
| **Cruise** | ~85% | Sustained cruise scenario | Stable flow cues and moderate exhaust |
| **Max Thrust** | Max | Maximum-output demonstration | Pronounced turbulence cues, hottest exhaust representation, maximum bypass-flow visual intensity |

> **Note on Looping & Exclusions:** All simulation caches are engineered as seamlessly looping sequences. The "Ignition/Spool-Up" state is explicitly excluded (see §6.1 for engineering justification).

---

## 3. Interaction Design (UI/UX)

### A. Primary Controls (Always Visible in HUD)

**Operational State:**

```yaml
[IDLE]  [TAKEOFF]  [CRUISE]  [MAX THRUST]
```

**View Mode:**

```yaml
[EXTERNAL VIEW]  [CLOSE-UP]
```

**Visual Mode Toggles (available in all views):**

```yaml
[Normal]  [X-Ray Thermal]  [Velocity Vectors]
```

---

### B. View Modes & Sub-Selection

#### 1. External View

The engine is displayed at a distance sufficient for the full assembly to fit within the viewport. The user selects a sub-mode via a secondary toggle:

```yaml
[CUTAWAY]  [ASSEMBLY]
```

**CUTAWAY Sub-Mode:**

- **Geometry Zoning:** Engine topology is authored with explicit, hardcoded zone partitions (e.g., `grp_fan`, `grp_hp_compressor`, `grp_combustor`) in the USD scene graph. These explicit partitions dictate geometry load/unload (Payloads), material assignments, and act as the targets for telemetry data binding.
- Pre-booleaned cutaway geometry (`engine_cutaway.usd`) bisects the engine along its longitudinal axis, improving rendering stability on complex volumetric or refractive boundaries while exposing all internal zones. Real-time dynamic clipping planes are explicitly disallowed.
- The Houdini simulation cache plays back in full — internal flow, rotating stages, combustion Pyro.
- **Thermal Map:** A fixed, illustrative temperature scale (~50 °C to ~2,000 °C) maps the fan intake to blue and the exhaust to deep red. State-driven synthetic values change the intensity distribution; the map does not claim an exact engine temperature field.
- **Velocity Vectors:** Internal primary (hot-section) and bypass (cold-section) streamlines are visualised. See §4 for the Ghost Material specification.

**ASSEMBLY Sub-Mode (Context & Scale):**

- The engine is shown in its complete, uncut exterior state (`engine_assembly.usd`) within a **Testbed 80-inspired** presentation context.
- **Camera Interactivity:** Unlike the fixed orthographic feel of the Cutaway, this mode acts as an interactive "Viewing Gallery". The user can explore the full scale of the engine and the test cell environment either via multiple pre-set camera angles (e.g., *Control Room View*, *Pylon Mount*, *Exhaust View*) or via a constrained Free-Fly camera.
- The same Houdini simulation cache is used, but the external casing remains fully opaque in Normal mode.
- **Thermal Map:** A qualitative axial heat gradient runs from cold colours at the intake to hot colours at the exhaust nozzle. Synthetic state values increase the displayed heat and plume intensity from Idle to Max Thrust.
- **Velocity Vectors:** Streamlines of the exhaust plume extending behind the nozzle, plus the bypass airflow along the nacelle exterior. See §4 for the Ghost Material specification.

---

#### 2. Close-Up View

The camera cuts to a pre-defined, close-range position focusing on a specific engine module. Navigation between modules is **sequential**, front-to-back (or reverse), via arrow controls in the HUD:

```yaml
← [FAN]  [IP COMPRESSOR]  [HP COMPRESSOR]  [COMBUSTION CHAMBER]  [HP TURBINE]  [IP TURBINE]  [LP TURBINE] →
```

Each module has:

- A dedicated pre-set camera position in Omniverse.
- A dedicated HUD panel with component-specific telemetry (RPM, local temps, pressures).
- **Thermal Map:** The colour scale is **recalibrated per-module** to an illustrative, reference-informed temperature range. This maximises visual contrast without claiming physical accuracy at each zoom level:

| Module | Illustrative Thermal Range |
| :--- | :--- |
| Fan | 50 °C – 300 °C |
| IP Compressor | 200 °C – 600 °C |
| HP Compressor | 500 °C – 800 °C |
| Combustion Chamber | 1,000 °C – 2,000 °C |
| HP Turbine | 1,000 °C – 1,600 °C |
| IP Turbine | 800 °C – 1,200 °C |
| LP Turbine | 600 °C – 950 °C |

---

### C. Telemetry Dashboard

| Level | Core Performance | Environment & Systems |
| :--- | :--- | :--- |
| **External** | N1/N2/N3 RPM, EGT (Exhaust Gas Temp ℃), Fuel Flow (kg/s), Thrust (kN) | OAT (Outside Air Temp ℃), Oil Pressure (psi) & Temp (℃), Vibration (IPS) |
| **Close-Up** | Spool RPM for selected module | Local velocity (m/s), Local inlet/outlet pressures (psi) and temps (℃) |

---

## 4. Visual Mode Specifications

### Normal Mode

- Full photorealistic rendering of engine geometry.
- Houdini Pyro simulation cache active (flames in combustion, smoke/heat shimmer).
- State transitions use a **10-frame Sequential Fade** on the Pyro Shader layer (5 frames fade to 0 opacity -> VariantSet Swap -> 5 frames fade to full opacity) to ensure visual continuity *without* simultaneous memory loading of two heavy VDB caches.

### X-Ray Thermal Mode

- **Semantic Discipline (No Dual Encoding):** In this mode, color *strictly* encodes temperature (`TEMP °C`). By explicitly separating thermal data from velocity data, we reinforce the engineering rigor of the Digital Twin.
- Pyro smoke/flame hidden.
- Thermal heatmap projected onto geometry via per-component colour Primvars.
- **External View:** Global scale (~50 °C → ~2,000 °C), fixed across states.
- **Close-Up:** Per-module adaptive scale; recalibrates min/max to an illustrative, reference-informed range.
- Dense metric HUD overlays are simplified in Close-Up X-Ray to avoid clutter — only the temperature colour-scale legend is shown.

### Velocity Vectors (Streamlines)

> **Ghost Material (`M_Engine_Ghost`):** When Velocity Vectors mode is activated, the engine geometry (or the selected module in Close-Up) transitions to a **semi-transparent, holographic material** — a Fresnel shader that is transparent at face normals and luminous along silhouette edges, or a wireframe mesh representation. This visual effect allows the streamline geometry to be clearly visible through the engine's solid structure.

- **Semantic Discipline (No Dual Encoding):** In this mode, color *strictly* encodes velocity magnitude (`VEL m/s`). Temperature is never represented by color here to avoid cognitive overload with the Thermal mode.
- **Primary Flow (hot-section):** Streamlines trace the gas path from intake → compressor stages → combustion → turbines → exhaust nozzle. Blending from `Blue` (low velocity) through `Green` / `Yellow` to `Red` (high velocity).
- **Bypass Flow (cold-section):** Streamlines trace the high-volume bypass air around the engine core. Predominantly `Cyan / Blue` (slower bypass air), shifting toward `Green` at higher operational states.
- **Assembly External Mode:** Streamlines extend behind the exhaust nozzle, visualising the hot exhaust plume, with cool bypass-mixed air framing it.
- **Intensity scales with Operational State:** In Idle, lines are sparse and slow; in Max Thrust, dense and fast.

---

## 5. Technical Architecture

### Layer 1: Data Provider (The Brain)

A Python module (`src/data_provider`) acting as the "Single Source of Truth" for all telemetry values.

- **Demo Mode (default):** Generates procedural sine-wave/noise data keyed to the selected Operational State enum. This is the standard mode for the showreel.
- **Live Mode (upgrade path):** Exposes `get_telemetry() -> dict` accepting normalised float payloads. A real data source could be integrated behind this provider contract, subject to schema mapping, calibration, validation, and operational constraints.
- **Output:** The provider must generate and return *all* these synchronised values: `n1_rpm`, `n2_rpm`, `n3_rpm`, `egt_c`, `fuel_flow_kg_s`, `thrust_kn`, `oil_press_psi`, `oil_temp_c`, `vibration_ips`, `oat_c`.

### Layer 2: Simulation Core (The Factory)

Pre-baked assets generated in Houdini (Solaris/PDG). **All simulation is looped.**

- **Format:** USD VariantSets.
- **Matrix:** Exactly 4 master states (`Idle`, `Takeoff`, `Cruise`, `Max Thrust`). Camera and view modes (External/Close-Up) control USD Payload loading only, *not* the VDB simulation caches. There is no multiplication of caches based on camera angles.
- **Artifacts:**
  - `.vdb` (Density/Temperature/Velocity grids for Pyro)
  - `.usd` (BasisCurves for streamlines)
  - Thermal Primvars embedded in geometry USD

### Layer 3: Omniverse App (The Frontend)

A Kit-based application assembling the interactive experience.

- **State Machine:** Listens to Data Provider and drives USD VariantSet swaps.
- **USD Composition:** Swaps active VariantSets / material bindings based on combined State × View × Visual Mode.
- **UI:** Viewport-embedded HUD panel (`omni.ui.scene` overlay).
- **Runtime Boundary:** The Omniverse app is treated as a contract-driven runtime layer, not a workstation-bound scene file. Packaging and portability constraints are governed by [ADR 006: Omniverse Runtime Boundary and Portability](../adr/006-omniverse-runtime-boundary.md).
- **Template Reference:** The local `E:\omniverse_kit_app` folder may be
  inspected as a read-only reference for Kit app structure, extension layout,
  build/launch workflow, and future viewer/controller patterns. It is not Case
  02 content and must not be modified or used as an asset/documentation
  workspace.

---

## 6. Implementation Strategy

### Phase 1: Houdini Production (The Factory)

**Goal:** Generate the simulation matrix and export optimised USD foundations.

#### 1.1 Base Geometry (Static)

- [ ] **Engine Hero Asset:** Import and clean Trent 1000 STL mesh. Validate scale against reference: Fan Ø = 2,850 mm, Length = 4,738 mm.
- [ ] Internal zone separation: Fan, IP/HP Compressor, Combustion Chamber, HP/IP/LP Turbine — as separate geometry groups for individual material assignment and clipping.
- [ ] Texture baking (Diffuse / Roughness / Normal).

#### 1.2 Simulation Setup (Dynamic)

- [ ] **Flow Visualisation:** Primary and bypass flow VDB caches.
  - Input: Engine collision geometry.
  - States: Idle (smooth, low-velocity cues) / Takeoff (high-velocity, energetic cues) / Cruise (steady cues) / Max Thrust (pronounced turbulence cues).
- [ ] **Exhaust Plume Visualisation:** Separate VDB for external assembly view.
- [ ] **Vector Field Gen:** Convert VDB velocity fields to `BasisCurves` (streamlines), colour-coded by relative velocity magnitude.
- [ ] **Thermal Map Gen:** Generate per-component vertex colour Primvars for a state-driven synthetic heatmap overlay.

#### 1.3 Caching & Export

- [ ] Batch Process via ROP: iterate `[Operational State]`. Camera modes do not multiply caches.
- [ ] Loop all caches: ensure first and last frames connect seamlessly.
- [ ] Volume Optimisation: crop the VDB export using the pre-booleaned cutaway geometry. Caching only the visible slice (rather than the full 360-degree cylindrical volume) will massively reduce disk footprint and I/O overhead. Prune remaining VDB density below threshold.
- [ ] Curve Optimisation: resample streamlines, cull insignificant by length/velocity attribute.

### Phase 2: Asset Packaging (USD Structure)

**Goal:** Assemble switchable USD assets using VariantSets.

#### 2.1 Material Library

- [ ] `M_Engine_Standard` — full PBR, opaque.
- [ ] `M_Engine_Ghost` — Fresnel semi-transparent / wireframe hologram (for Velocity Vectors mode).
- [ ] `M_Engine_XRay` — thermal heatmap material driven by Primvar colour attribute.
- [ ] `M_Airflow_Lines` — curve emission shader (velocity streamlines).
- [ ] `M_Airflow_Smoke` — volume shader for Pyro.

#### 2.2 Composition (VariantSets)

- [ ] `variantSet: "OperationalState"` → {`Idle`, `Takeoff`, `Cruise`, `MaxThrust`}
- [ ] `variantSet: "ViewMode"` → {`ExternalCutaway`, `ExternalAssembly`, `CloseUp_Fan`, `CloseUp_IPComp`, `CloseUp_HPComp`, `CloseUp_Combustion`, `CloseUp_HPTurb`, `CloseUp_IPTurb`, `CloseUp_LPTurb`}
- [ ] `variantSet: "VisualMode"` → {`Normal`, `XRayThermal`, `VelocityVectors`}

### Phase 3: Omniverse App Logic

**Goal:** Create the interactive runtime environment.

#### 3.1 Data Provider (Python)

- [ ] `src/data_provider.py` — class `TelemetryGenerator`:
  - `get_data(state_enum)` → returns dict `{n1_rpm, n2_rpm, n3_rpm, egt_c, fuel_flow_kg_s, thrust_kn, oil_press_psi, oil_temp_c, vibration_ips, oat_c}`.
  - Sine-wave generators per parameter for "alive" feeling between state changes.

#### 3.2 Kit Extension

- [ ] `exts/omni.ai.jet_engine/`
  - **Operational State switcher:** `[IDLE] [TAKEOFF] [CRUISE] [MAX THRUST]`
  - **View Mode switcher:** `[EXTERNAL] [CLOSE-UP]`
  - **Sub-mode control:**
    - External → `[CUTAWAY] [ASSEMBLY]` toggle
    - Close-Up → `← [MODULE NAME] →` sequential navigator
  - **Visual Mode toggles:** `[Normal]` / `[X-Ray Thermal]` / `[Velocity Vectors]`
  - **Telemetry readout:** live display of RPM × 3 spools, EGT, Thrust, Fuel Flow.

#### 3.3 State Machine Wiring

- [ ] **Atomic Switching:** On any UI input, the system resolves a single, combined VariantSet key. This swap is strictly atomic: it guarantees one coherent USD layout configuration. There are no mixed or undefined states, ensuring scene stability regardless of rapid user inputs.
- [ ] On View Mode → CLOSE-UP: trigger camera jump to pre-set position for selected module.
  - [ ] On Visual Mode → VELOCITY VECTORS: swap geometry material to `M_Engine_Ghost`.
  - [ ] State transitions for Volumetrics (Pyro): Utilize a **10-frame Sequential Fade**. The current VDB fades to complete transparency over 5 frames. Once invisible, the `OperationalState` VariantSet is atomically swapped (unloading the old cache, loading the new). The new VDB then fades in from transparency over 5 frames. This completely eliminates the I/O bottleneck of loading two overlapping VDB sequences.
  - [ ] HUD telemetry values concurrently lerp over these same transition frames between the old and new base values to ensure data/graphics synchronicity. Procedural "breathing" noise applies only during a stable state.

### Phase 4: Polish & Delivery

- [ ] **Lighting:** Testbed 80 environment — industrial facility HDR, directional key light simulating the facility's overhead rigs.
- [ ] **Performance Tuning:** Target 30+ FPS in External view; 24+ FPS in Close-Up with active Pyro.
  - Adjust Point Instancer settings.
  - Reduce VDB and curve resolution if GPU-bound.
- [ ] **Documentation:** `README.md` "How to Run".

---

## Implementation Notes (for future me)

> This section records representation and data-boundary decisions so the concept sections above stay focused on *what we show*. Nothing here changes the vision; it describes implementation mechanics.
> **Why synthetic data?** We have no live engine telemetry feed. All parameters - RPM, EGT, fuel flow, thrust - are procedurally generated by the Data Provider in Demo Mode. The goal is to demonstrate an L1-oriented digital twin visualisation concept and technical-art pipeline competency. A real telemetry source could later be integrated through the provider contract, but would require schema mapping, calibration, validation, and operational review.

### 1. Pyro: State Caches + Transition Logic

- All flow and combustion visualisation caches are **pre-baked in Houdini** into a state matrix.
- Cached as USD VariantSets (`.vdb` volumes + `BasisCurves` streamlines) — no live sim at runtime.
- All caches are **looped sequences** — start and end frames are continuity-matched.
- State transitions use a **Sequential Fade** (Fade-Out -> Swap -> Fade-In) on the Pyro Shader layer opacity. This is an explicit performance optimization: by driving the volume to 100% transparent *before* triggering the VariantSet swap, we avoid holding two massive `.vdb` sequences in memory/VRAM simultaneously.
- Static geometry is never touched or faded during this transition.
- **Why exclude Ignition?** Ignition and Spool-Up are non-looping, transient events. Authoring and caching a one-off 30-second spool-up visualisation requires substantial disk I/O for an effect that plays only once. Focusing the budget on looping the 4 master sustained states provides better value for the runtime experience.

### 2. Thermal Map: Synthetic Temperature with Adaptive Scale

- The Data Provider outputs synthetic per-zone temperature values keyed to the operational state.
- A Primvar writer maps these values to per-vertex/per-face colour attributes on the geometry.
- **External View:** A single illustrative LUT covering ~50 °C to ~2,000 °C is applied. The LUT is fixed; the synthetic distribution shifts with operational state.
- **Close-Up View:** A per-module LUT uses reference-informed illustrative bounds. This maximises colour contrast within the zone, avoiding the "everything is red" problem that would occur with the global scale at Close-Up resolution.
- The `M_Engine_XRay` MDL material samples the Primvar colour attribute directly.

### 3. Ghost Material: implementation logic

- When Velocity Vectors mode is active, the active geometry partition is assigned `M_Engine_Ghost`.
- Implementation: Fresnel-based MDL shader with near-zero opacity at face normals, high opacity/luminosity at grazing angles. Alternatively, a wireframe representation may be used if MDL Fresnel proves computationally prohibitive.
- Effect is applied only to the geometry relevant to the current view; the rest of the scene remains fully opaque to save raytracing bounces.

### 4. Streamlines: Geometry Generation

- Curve counts are aggressively reduced in the Houdini pre-export phase.
- Insignificant curves are culled dynamically based on a velocity threshold attribute to maintain the 30+ FPS budget in External view.

### 5. Performance Contract: LOD & Payload Boundaries

To guarantee the **30+ FPS** target in External view and **24+ FPS** in isolated Close-Up (with active Pyro), strict rendering budget constraints apply:

- **External View (Wide):** No dense volumetrics. VDB caches of smoke/fire are either disabled entirely (relying on geometry and colour emission) or replaced with severely downsampled proxy representations. Streamline curve counts are sparse.
- **Close-Up View (Macro):** High-resolution volumetrics are allowed, but strictly *bounded* to the local module currently in focus. Secondary bounces and shadows from smoke in adjacent modules are culled.
- **Payloads:** Full internal PCB-level geometry detail (Close-Up HD meshes) is loaded as a USD **Payload** — only pulled in when Close-Up view is active. External view uses lower-resolution proxy meshes for internal zones visible through the cut plane.
