# Case 02: Jet Engine Digital Twin (Trent 1000) — Main Concept

> **Philosophy:** "Engineering in Motion" — A data-driven, real-time Digital Twin of a Rolls-Royce Trent 1000 turbofan engine, running on Testbed 80, demonstrating the full spectrum of operational states through physically-grounded simulation and interactive visualisation.

---

## 1. High-Level Concept

**Project:** Autonomous NVIDIA Omniverse Kit Application.
**Mission:** Visualise the internal thermo-aerodynamic lifecycle of a Trent 1000 engine across four operational states — from standby to maximum thrust — through seamlessly switchable photorealistic, thermal, and flow-vector modes.
**Narrative Context:** The engine is situated on **Testbed 80** (see [Testbed 80 Facility Details](Testbed_80_Facility_Details.md)). The testbed is not mere decoration; it provides the essential physical context (sensor points, readouts, and physical mounts) that justifies the existence of the telemetry dashboard and grounds the UX in engineering reality.
**Engineering Logic:** A single pre-baked simulation matrix (4 states × looping USD VariantSets) drives all visual layers, with zero live computation at runtime.

---

## 2. Operational States (Simulation Matrix)

All states are **pre-baked in Houdini (Solaris/PDG)** and looped as USD VariantSets. Each state has a distinct aerodynamic and thermal signature.

| State | Throttle | Description | Visual Signature |
| :--- | :--- | :--- | :--- |
| **Idle** | ~5% | Ground standby / Testbed 80 warm-up | Low RPM, laminar flow, cool exhaust |
| **Takeoff** | 100% | Peak thrust demand | Max RPM all 3 spools, high-velocity jets, intense exhaust plume |
| **Cruise** | ~85% | Sustained efficiency envelope | Stable, efficient flow; exhaust moderate |
| **Max Thrust** | Max | Testbed stress condition | Turbulent compressor zones, extreme exhaust temps, max bypass velocity |

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
- Pre-booleaned cutaway geometry (`engine_cutaway.usd`) bisects the engine along its longitudinal axis, guaranteeing rendering stability (no artifacts on complex volumetric or refractive boundaries) while exposing all internal zones. Real-time dynamic clipping planes are explicitly disallowed.
- The Houdini simulation cache plays back in full — internal flow, rotating stages, combustion Pyro.
- **Thermal Map:** Global absolute temperature scale spanning the full engine thermal range (~50 °C → ~2,000 °C). The fan intake end of the scale reads blue; the exhaust reads deep red. The scale is fixed and consistent across operational states — only the intensity distribution changes as states change.
- **Velocity Vectors:** Internal primary (hot-section) and bypass (cold-section) streamlines are visualised. See §4 for the Ghost Material specification.

**ASSEMBLY Sub-Mode (Context & Scale):**

- The engine is shown in its complete, uncut exterior state (`engine_assembly.usd`) — exactly as it appears mounted on **Testbed 80** in Derby.
- **Camera Interactivity:** Unlike the fixed orthographic feel of the Cutaway, this mode acts as an interactive "Viewing Gallery". The user can explore the full scale of the engine and the test cell environment either via multiple pre-set camera angles (e.g., *Control Room View*, *Pylon Mount*, *Exhaust View*) or via a constrained Free-Fly camera.
- The same Houdini simulation cache is used, but the external casing remains fully opaque in Normal mode.
- **Thermal Map:** Axial temperature gradient along the engine's exterior, representing the temperature of the gas stream within. Cold (blue) at the intake, hot (red/orange) at the exhaust nozzle. The exhaust temperature and plume intensity increase progressively from Idle to Max Thrust.
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
- **Thermal Map:** The colour scale is **recalibrated per-module** to that module's specific physical temperature range. This maximises visual contrast and physical accuracy at each zoom level:

| Module | Approximate Thermal Range |
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
- **Close-Up:** Per-module adaptive scale; recalibrates min/max to the module's physical range.
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
- **Live Mode (upgrade path):** Exposes `get_telemetry() -> dict` accepting normalised float payloads. Real hardware feeds (HWiNFO64, Prometheus/Grafana, MQTT, Kafka) can be hot-swapped in with **zero changes to the visualisation layer**.
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

- [ ] **CFD / Fluid Sim:** Primary and bypass flow VDB sim.
  - Input: Engine collision geometry.
  - States: Idle (laminar, low velocity) / Takeoff (max velocity, max turbulence) / Cruise (steady) / Max Thrust (turbulent, high rejection).
- [ ] **Exhaust Plume Sim:** Separate VDB for external assembly view.
- [ ] **Vector Field Gen:** Convert VDB velocity fields to `BasisCurves` (streamlines), colour-coded by velocity magnitude.
- [ ] **Thermal Map Gen:** Generate per-component vertex colour Primvars for heatmap overlay.

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

> This section collects the "how we fake it" decisions so the concept sections above stay focused on *what we show*. Nothing here changes the vision; it is purely mechanics.
> **Why synthetic data?** We have no live engine telemetry feed. All parameters — RPM, EGT, fuel flow, thrust — are procedurally generated by the Data Provider in Demo Mode. The goal is to demonstrate a Digital Twin *concept* and prove technical artist competency: the ability to design and implement a convincing, data-driven, procedurally animated real-time visualisation. The system is architected such that real telemetry (OPC-UA from a test cell ACARS feed, or any HTTP/MQTT stream) can be connected with zero changes to the visualisation layer.

### 1. Pyro: State Caches + Transition Logic

- All aerodynamic and combustion simulations are **pre-baked in Houdini** into a state matrix.
- Cached as USD VariantSets (`.vdb` volumes + `BasisCurves` streamlines) — no live sim at runtime.
- All caches are **looped sequences** — start and end frames are continuity-matched.
- State transitions use a **Sequential Fade** (Fade-Out -> Swap -> Fade-In) on the Pyro Shader layer opacity. This is an explicit performance optimization: by driving the volume to 100% transparent *before* triggering the VariantSet swap, we avoid holding two massive `.vdb` sequences in memory/VRAM simultaneously.
- Static geometry is never touched or faded during this transition.
- **Why exclude Ignition?** Ignition and Spool-Up are non-looping, transient events. Simulating and caching a one-off 30-second fluid dynamic spool-up sequence requires massive disk I/O for a visual effect that fires only once. Focusing the budget on looping the 4 master sustained states provides overwhelmingly better ROI for the runtime environment.

### 2. Thermal Map: Absolute Temperature with Adaptive Scale

- The Data Provider outputs per-zone temperature floats, matched to the operational state.
- A Primvar writer maps these values to per-vertex/per-face colour attributes on the geometry.
- **External View:** A single global LUT covering ~50 °C – ~2,000 °C is applied. The LUT is fixed; temperature distribution shifts with operational state.
- **Close-Up View:** Per-module LUT with min/max values tuned to that module's physical range. This maximises colour contrast within the zone, avoiding the "everything is red" problem that would occur with the global scale at Close-Up resolution.
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
