# Case 02: Jet Engine (Aerospace Propulsion Digital Twin)

> [!WARNING]
> **Work in Progress:** This project is currently under active development. Some links and assets may be placeholders.

---

> **Role:** L1-Oriented Digital Twin Visualisation Prototype (Mechanical Engineering)
> **Stack:** Houdini 21.0.596 (Sim/PDG), Omniverse 2024.x (USD/Python), Isaac Sim, Jira Integration

---

## 📋 Project Overview

This repository showcases a portfolio-grade aerospace propulsion digital twin
visualisation prototype targeting an L1 informative workflow. It demonstrates
state-driven representations of qualitative flow, thermal, combustion, and
component behaviour within a complex assembly and Testbed 80 context, without
claiming predictive engine physics.

**Key Use Case:**
The prototype integrates **pre-baked Houdini caches for each operational regime**
(Idle, Takeoff, Cruise, Max Thrust). These states are switched within Omniverse
to communicate qualitative flow direction, thermal zones, combustion states,
and component relationships. This demonstrates an L1-oriented visualisation
workflow without live physics or predictive propulsion modelling.

**Project Focus:**

- **Complex Assembly Management:** Handling 10,000+ parts using USD Variants and Payloads
- **Simulation Pipeline:** Houdini-authored flow and thermal visualisation caches optimised into lightweight USD assets
- **Data-Driven Visualisation:** Python-based sensor streams (RPM, EGT, Vibration) synchronised with visual states

## What This Case Proves

- Complex aerospace assembly management through USD Payloads, VariantSets, and
  module-level scene organisation.
- Pre-baked Houdini simulation states structured for real-time Omniverse
  playback.
- Synthetic telemetry binding for RPM, EGT, vibration, fuel flow, thrust, and
  engine state visualisation.
- Clear separation between public-reference geometry, external simulation
  caches, runtime HUD logic, and project claim boundaries.

## Scope & Limitations

Case 02 is a portfolio-grade, L1-oriented digital twin visualisation prototype.
Its Houdini-authored, pre-baked flow and thermal caches are physically inspired
representations for engineering communication and state-driven demonstration,
not validated CFD results.

The project does not claim predictive aerodynamic accuracy, certification
suitability, real Trent 1000 telemetry, or proprietary engine geometry.
Validation covers pipeline behaviour, USD composition, state switching,
telemetry binding, and visual consistency rather than real-engine gas dynamics.

See [Validation and Limitations](docs/knowledge_base/validation_and_limitations.md)
and [Reference Geometry and Provenance](docs/knowledge_base/reference_geometry_and_provenance.md)
for the full claim boundary.

> **Deep Dive:**
>
> - [Rolls-Royce Factory Tour Transcript](./docs/knowledge_base/transcripts/Flightradar24%20-%202026.01.30%20-%20How%20Rolls-Royce%20Jet%20Engines%20Are%20Built.md)
> - [Testbed 80 Technical Specifications](./docs/knowledge_base/reference_material/Testbed_80_Facility_Details.md)
> - [FUI and HUD Design Notes](./docs/knowledge_base/reference_material/FUI%20and%20HUD.txt)

---

## 🎯 Key Technical Workflows (Visualisation Pipeline)

- **Step 1: Geometry Foundation (Complex Assembly Management):** Leveraging USD Variants and Payloads to structure and standardize massive mechanical assemblies (10,000+ parts) for the digital twin.
- **Step 2: Production Simulation (Optimised USD Pipelines):** Processing Houdini-authored flow and thermal caches into lightweight USD payloads for real-time Omniverse playback across different operational regimes.
- **Step 3: Synthetic Telemetry Integration (Data-Driven Visualisation):** Generating scenario-based RPM, EGT, and vibration streams in Python and binding them to visual states.

## 👁️ Visual Proof

> *Placeholders for future GIFs - Replace with actual optimised media*

1. **Engine Idle State:** `![Idle Demo](docs/img/idle_demo.gif)`
2. **Telemetry Dashboard:** `![FUI Demo](docs/img/fui_demo.gif)`
3. **Pipeline Flow:** [Pipeline Diagram](docs/img/pipeline_diagram.md)

## 🏗️ Architecture & Decisions

This project follows a **README-driven structure** to manage the complexity of hybrid Houdini/Omniverse pipelines.

- [**Read our Engineering Decisions (ADRs)**](docs/adr/) for deep dives into Naming, Security, and Architecture.

## 📂 Repository Structure

```text
.
├── assets/
│   ├── _external/   # [DOWNLOADED] Runtime Assets (USD, Textures, HDRI) - Git Ignored
│   │   ├── usd/
│   │   ├── tex/
│   │   └── hdri/
│   └── local/       # Lightweight assets tracked by Git
├── docs/                 # ADRs, plans, and knowledge base
│   ├── adr/              # Architecture Decision Records
│   ├── plans/            # Implementation plans & tech debt
│   └── knowledge_base/   # Reference materials (factory tours, specs, FUI design)
├── src/                  # Core logic and scripts
├── tests/                # Validation and testing suite
└── tools/                # Internal pipeline utilities (Jira, data generation)
```

### 📚 Knowledge Base

The `docs/knowledge_base/` directory contains curated reference materials:

- **[Knowledge Base Index](docs/knowledge_base/README.md)**: Central directory for all concepts, technical specs, and USD architecture rules.
- **[Flightradar24 Factory Tour Transcript](docs/knowledge_base/transcripts/Flightradar24%20-%202026.01.30%20-%20How%20Rolls-Royce%20Jet%20Engines%20Are%20Built.md)**: Technical specifications (Trent 1000), sourced from Andy Dawkins (GM, Engine Overhaul Services) and Paul Flint (Chief of Capability Programs)
- **[Testbed 80 Specifications](docs/knowledge_base/reference_material/Testbed_80_Facility_Details.md)**: Facility dimensions, acoustic treatment, structural details
- **[FUI and HUD Design](docs/knowledge_base/reference_material/FUI%20and%20HUD.txt)**: Parameters for Heads-Up Display telemetry screens

---

## 💾 Project Data / Assets

### 🏭 The "Factory" Narrative
>
> This repository follows a strict **"Source vs. Artifact"** philosophy:
>
> - **Houdini (Fabricator):** The procedural "factory" where assets are generated. Source files (`.hip`) are proprietary and **excluded** from this repository.
> - **USD (Artifact):** The "product" of the factory. These are the optimized files needed to run the Digital Twin in Omniverse.
> - **Synthetic Telemetry Generation:** Scenario-based telemetry streams are produced by Python generators to exercise defined operating states and edge cases. They demonstrate the visual response of the prototype and are not measurements from a real engine or evidence of Sim-to-Real validation.

### 📦 Asset Hydration

To keep this repository lightweight, heavy binary assets (USD Crates, Textures, HDRIs) are stored externally.

- [**Download Asset Pack (One Drive / S3 Link TBD)**](https://example.com/placeholder)

**Hydration Steps:**

1. Download the ZIP archive from the link above.
2. **Extract contents** directly into the `assets/_external/` folder.
    - *Note: This folder already exists (anchored by `.gitkeep`), so you simply unzip into it.*
    - *Result:* Your local path should look like `assets/_external/usd/my_asset.usd`.

### External Omniverse Template Reference

An external local reference copy of NVIDIA Omniverse Kit App Template and a
generated test application may be inspected to understand Omniverse Kit app
structure, extension layout, build/launch workflow, startup/playback/controller
patterns, and future runtime viewer architecture. It is not authored Case 02
content; do not modify it or mix project assets or documentation into it.

## 📜 Technical Stack

- **Python**: 3.11.15
- **OpenUSD Python**: `usd-core==26.5` (`pxr` diagnostics)
- **Houdini**: 21.0.729 (PDG, Pyro, Fluid)
- **Nvidia Omniverse**: 110.1.2
- **Conda**: Environment isolation (`case02-env`)

---

## 📜 Changelog

- **Week of 13 July, 2026:** Added OpenUSD Python diagnostics through `usd-core`, restoring `pxr` access in the Case 02 environment and closing the dependency hygiene gap.
- **Week of 6 July, 2026:** Added Omniverse MCP helper tooling and a runtime boundary ADR, then moved the project environment baseline to Python 3.11.x with the dependency lock refreshed.
- **Week of 25 May, 2026:** Resolved the pip security lock technical debt and refreshed the dependency lock to keep the validation toolchain current.
- **2026-03-04:** Refined Digital Twin core concept (`main_concept.md`). Formalized strict semantic discipline (no dual encoding), pre-computed VDB cutaway optimization, atomic UI switching, and integrated the Testbed 80 narrative context.
- **2026-02-02:** Implemented external storage strategy for heavy assets (Git-agnostic).
- **2026-01-22:** Initial repository bootstrap. Established "Gold Standard" structure (ADRs, Pre-commit, Hybrid Access).
