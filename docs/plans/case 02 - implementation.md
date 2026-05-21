# Case 02: Jet Engine Digital Twin (L1) — Implementation Plan

**Jira Project**: JET
**Status**: In Progress
**Last Updated**: 2026-02-04

---

## ‼ Operational Protocol Addendum

- We communicate in Russian in chat, but all documentation, commits, and code comments must remain in British English!
- A question is not a command to execute actions: if a message is a question (including forms like `?`, `?!`, `!?`, `?!?!`), answer first and do not run actions until clarified!

---

## 🧩 EPIC: Research & Pre-production (JET-1)

**Status**: 🔄 In Progress | **Priority**: Medium

### ✅ [JET-10] Explore CAD models

**Status**: ✅ Done | **Priority**: Medium | **Estimate**: 3h
**Objective**: Investigate the downloaded CAD engine models and select the
  optimal “Hero Asset” for Case 02 – Jet Engine, suitable for the full pipeline
  (modeling, VFX, rendering).Process:Trent XWB Analysis:The model was
  incomplete; the package included only the assembly file (.SLDASM) without the
  actual geometry files (.SLDPRT).Result: The model is unsuitable.CAD Format
  Analysis (.STEP, .IGS, .CATPRODUCT):Investigated importing these CAD formats.
  Houdini Indie does not support direct import of .STEP or .IGS files, making
  this approach unfeasible..STL Format Analysis (Trent 900 vs. Trent 1000):Trent
  900:Supplied as a single .STL file.Pros: Includes a pre-made cutaway
  section.Cons: The mesh is monolithic—which is critical—rendering it unsuitable
  for simulation. The presence of an opening in the casing prevents accurate
  combustion simulation.Trent 1000:Supplied as a “kit” composed of multiple .STL
  files. Upon import into Houdini, all parts assembled seamlessly into a
  unified, watertight model.
**Logged**: 2h 22m

### ✅ [JET-12] Create Master Reference Board

**Status**: ✅ Done | **Priority**: Medium
**Objective**: Aggregate all visual references into a single board
  (Miro/Figma/Pureref) to serve as the project's visual compass.

#### ✅ [JET-13] Environment Refs

**Status**: ✅ Done | **Priority**: Medium | **Estimate**: 1h
**Objective**: Gather high-resolution references for the 'Testbed 80'
  environment. Focus on: 1. Overall architecture and scale. 2. Test stand
  mechanics and mounting points. 3. Wall/floor materials and lighting fixtures.
  Deliverable: A dedicated section in the Master Reference Board (Miro/Pureref).
**Logged**: 2h

#### ✅ [JET-14] FUI/HUD Refs

**Status**: ✅ Done | **Priority**: Medium | **Estimate**: 1h
**Objective**: Gather references for the technical FUI/HUD elements. Start with
  the authentic 'Testbed' monitor display as the primary style guide. Find
  additional examples of clean, minimalist, and functional data visualization
  (no "Hollywood" sci-fi). Deliverable: A dedicated section in the Master
  Reference Board.
**Logged**: 1h

#### ✅ [JET-15] Mood & Lighting Refs

**Status**: ✅ Done | **Priority**: Medium | **Estimate**: 1h
**Objective**: Gather cinematic references to define the final look and feel.
  Focus on industrial and automotive photography/cinematography. Define the mood
  (e.g., "clean lab", "dramatic reveal", "cold industrial") and lighting style
  (e.g., "soft diffused", "hard contrast"). Deliverable: A dedicated section in
  the Master Reference Board.
**Logged**: 30m

### ⏸️ [JET-16] Define Parameters & Create Data CSV

**Status**: ⏸️ To Do | **Priority**: Medium | **Estimate**: 2h
**Objective**: Finalize the list of key parameters for the digital twin and
  create a .CSV file to simulate this data over time.Example Parameters:
  timestamp, temperature_celsius, rpm_high_pressure, fuel_flow_kg_s,
  engine_mode_flag.

#### ✅ [JET-22] Define Trent 1000 Testbed FUI Parameters

**Status**: ✅ Done | **Priority**: Medium | **Estimate**: 1h
**Logged**: 1h

#### ⏸️ [JET-23] Create Data CSV with Trent 1000 Testbed Parameters

**Status**: ⏸️ To Do | **Priority**: Medium

### ✅ [JET-31] Setup Repository & Migration to Gold Standard

**Status**: ✅ Done | **Priority**: Medium | **Estimate**: 2h
**Objective**: Completed migration of Case 02 to the Showreel Gold Standard.  1.
  Strategy: Defined 'Tech Pack' GitHub strategy. 2. Protocol: Adopted strict
  operational protocols (Anti-Proactivity, English Docs). 3. Environment:
  Created 'case02-env' and locked dependencies. 4. Repo: Initialized 'dt-
  omniverse-showreel-case02-jet' with strict .gitignore. 5. QA: Configured pre-
  commit hooks.
**Logged**: 2h

### ✅ [JET-32] Collect & Document Trent 1000 Factory Tour References

**Status**: ✅ Done | **Priority**: Medium | **Estimate**: 1h
**Objective**: Collected 61 high-resolution reference screenshots from 'How
  Rolls-Royce Jet Engines Are Built' video (Flightradar24, 2026-01-30). Factory
  tour conducted by Andy Dawkins (GM, Engine Overhaul Services) and Paul Flint
  (Chief of Capability Programs). Video transcript with technical specifications
  saved to docs/knowledge_base/. Reference images stored locally in Refs/How
  Rolls-Royce Jet Engines Are Built/.
**Logged**: 45m

---

## 🧩 EPIC: Hero Asset: Jet Engine (JET-3)

**Status**: 🔄 In Progress | **Priority**: Medium

### 🔄 [JET-17] Retopologize Trent 1000 Core

**Status**: 🔄 In Progress | **Priority**: Medium | **Estimate**: 6h
**Objective**: Description: (Key Task). Create a clean, optimized (mid-poly)
  polygon mesh based on the "dirty" high-poly .STL model. This is critical for
  UV unwrapping and real-time performance in Omniverse.

### 🔄 [JET-18] Create Procedural Cables & Pipes

**Status**: 🔄 In Progress | **Priority**: Medium | **Estimate**: 6h
**Objective**: Develop a procedurally generated system for the external 'dress
  kit' (cables, pipes, harnesses). Based on Flightradar24 refs: hydraulic lines,
  electrical harnesses, protective conduits, bleed air ducting. This
  demonstrates core procedural modeling skills and faithfully represents the
  'heavy metal' aesthetic.

### 🔄 [JET-19] UV Unwrapping & Map Baking

**Status**: 🔄 In Progress | **Priority**: Medium | **Estimate**: 6h
**Objective**: Create clean UVs for all retopologized components. Bake utility
  maps (Normal, AO, Curvature) from the high-poly .STL source mesh to the new,
  clean mesh.

### 🔄 [JET-20] Texturing & Material Creation

**Status**: 🔄 In Progress | **Priority**: Medium | **Estimate**: 6h
**Objective**: Author photorealistic PBR materials for all engine components:
  titanium alloys (fan blades), nickel-based superalloys (turbine stages),
  painted steel (casings), carbon composites (nacelle parts). Use Substance
  Designer/Painter or Houdini's procedural workflows. Reference Flightradar24
  factory tour imagery for colour accuracy and wear patterns.

### 🔄 [JET-21] Final Asset Assembly & USD Export

**Status**: 🔄 In Progress | **Priority**: Medium | **Estimate**: 6h
**Objective**: Collect all geometry, materials, and variants into a single,
  clean .USD file. This is the final deliverable for this Epic, ready for the
  main scene assembly.

---

## 🧩 EPIC: Environment: Testbed 80 (JET-4)

**Status**: 🔄 In Progress | **Priority**: Medium

### 🔄 [JET-24] Model Architectural Shell (Testbed 80)

**Status**: 🔄 In Progress | **Priority**: Medium | **Estimate**: 6h
**Objective**: Create the main "bunker" structure based on refs: walls
  (including their massive 1.7m+ thickness), floor (including the embedded
  rails), and ceiling.

### 🔄 [JET-25] Model Environmental Details

**Status**: 🔄 In Progress | **Priority**: Medium | **Estimate**: 6h
**Objective**: Add key high-fidelity details: wall acoustic panels, ceiling
  crane, the main air intake/exhaust structures, doors, and all visible light
  fixtures.

### 🔄 [JET-26] UV Unwrapping & Texturing (Environment)

**Status**: 🔄 In Progress | **Priority**: Medium | **Estimate**: 6h
**Objective**: Create clean UVs for all environmental assets. Author PBR
  materials (raw concrete, painted metal, acoustic paneling) to achieve a
  realistic, heavy industrial look.

### 🔄 [JET-27] Export Environment as USD

**Status**: 🔄 In Progress | **Priority**: Medium | **Estimate**: 6h
**Objective**: Assemble all environmental components into a single, optimized
  USD file (e.g., "Testbed_80.usd"), ready for scene assembly.

---

## 🧩 EPIC: Asset Creation (Test Stand) (JET-5)

**Status**: 🔄 In Progress | **Priority**: Medium

### 🔄 [JET-28] Model Test Stand Rig

**Status**: 🔄 In Progress | **Priority**: Medium | **Estimate**: 6h
**Objective**: Model the main mechanical rig/gantry that holds the engine, based
  on visual references. Focus on the connection points to the engine and the
  ceiling mount.

### 🔄 [JET-29] UV Unwrapping & Texturing (Test Stand)

**Status**: 🔄 In Progress | **Priority**: Medium | **Estimate**: 6h
**Objective**: Create UVs and PBR materials (heavy painted industrial metal,
  hydraulic elements) for the test stand.

### 🔄 [JET-30] Export Test Stand as USD

**Status**: 🔄 In Progress | **Priority**: Medium | **Estimate**: 6h
**Objective**: Export the stand as a separate, optimized USD asset (e.g.,
  "Test_Stand.usd").

---

## 📊 Progress Summary

| Epic | Status | Priority | Completion |
| --- | --- | --- | --- |
| Research & Pre-production | 🔄 In Progress | Medium | 80% (4/5) |
| Hero Asset: Jet Engine | 🔄 In Progress | Medium | 0% (0/5) |
| Environment: Testbed 80 | 🔄 In Progress | Medium | 0% (0/4) |
| Asset Creation (Test Stand) | 🔄 In Progress | Medium | 0% (0/3) |

---

## 🎯 Next Priorities

1. **JET-16**: Define Parameters & Create Data CSV (Priority: Medium)
2. **JET-17**: Retopologize Trent 1000 Core (Priority: Medium)
3. **JET-18**: Create Procedural Cables & Pipes (Priority: Medium)
4. **JET-19**: UV Unwrapping & Map Baking (Priority: Medium)
5. **JET-20**: Texturing & Material Creation (Priority: Medium)
