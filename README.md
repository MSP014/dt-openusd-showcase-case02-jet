# Case 02: Jet Engine (Aerospace Propulsion Digital Twin)

> [!WARNING]
> **Work in Progress:** This project is currently under active development. Some links and assets may be placeholders.

---

> **Role:** L1 Digital Twin (Mechanical Engineering)
> **Stack:** Houdini 21.0.596 (Sim/PDG), Omniverse 2024.x (USD/Python), Isaac Sim, Jira Integration

---

## 📋 Project Overview

This repository showcases a prototype **Aerospace Propulsion Digital Twin (Level L1)** demonstrating visualisation of internal processes within complex mechanical systems. The case study centres on a jet engine mounted on a test stand, presented in cross-section to reveal thermodynamic and mechanical phenomena in real-time.

**Key Use Case:**
The digital twin integrates **pre-simulated Houdini caches for each operational regime** (Idle, Takeoff, Cruise, Maximum Thrust). These simulation states are seamlessly switched within Omniverse based on engine mode, visualising temperature distribution, combustion dynamics, and mechanical stresses. This exemplifies how L1 Digital Twins enable sophisticated engineering data visualisation without requiring real-time physics computation.

**Project Focus:**

- **Complex Assembly Management:** Handling 10,000+ parts using USD Variants and Payloads
- **Simulation Pipeline:** Houdini Pyro/Fluid simulations optimised into lightweight USD caches
- **Data-Driven Visualisation:** Python-based sensor streams (RPM, EGT, Vibration) synchronised with visual states

> **Deep Dive:**
>
> - [Rolls-Royce Factory Tour Transcript](./docs/knowledge_base/Flightradar24%20-%202026.01.30%20-%20How%20Rolls-Royce%20Jet%20Engines%20Are%20Built.md)
> - [Testbed 80 Technical Specifications](./docs/knowledge_base/Testbed%2080%20-%20Technical%20Specifications%20and%20Facility%20Details.txt)
> - [FUI and HUD Design Notes](./docs/knowledge_base/FUI%20and%20HUD.txt)

---

## 🎯 Key Technical Workflows

- **Optimized USD Pipelines:** Processing heavy Houdini Pyro/Fluid simulations into lightweight USD payloads for real-time Omniverse playback.
- **Data-Driven Digital Twin:** Python-based generation of realistic sensor streams (RPM, EGT, Vibration) linked to flight regimes (Idle, Takeoff, Cruise).
- **Complex Assembly Management:** Leveraging USD Variants and Payloads to structure massive mechanical assemblies (10k+ parts).

## 👁️ Visual Proof

> *Placeholders for future GIFs - Replace with actual optimised media*

1. **Engine Idle State:** `![Idle Demo](docs/img/idle_demo.gif)`
2. **Telemetry Dashboard:** `![FUI Demo](docs/img/fui_demo.gif)`
3. **Pipeline Flow:** `![Data Flow](docs/img/pipeline_diagram.png)`

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

- **[Flightradar24 Factory Tour Transcript](docs/knowledge_base/Flightradar24%20-%202026.01.30%20-%20How%20Rolls-Royce%20Jet%20Engines%20Are%20Built.md)**: Technical specifications (Trent 1000), sourced from Andy Dawkins (GM, Engine Overhaul Services) and Paul Flint (Chief of Capability Programs)
- **[Testbed 80 Specifications](docs/knowledge_base/Testbed%2080%20-%20Technical%20Specifications%20and%20Facility%20Details.txt)**: Facility dimensions, acoustic treatment, structural details
- **[FUI and HUD Design](docs/knowledge_base/FUI%20and%20HUD.txt)**: Parameters for Heads-Up Display telemetry screens

---

## 💾 Project Data / Assets

### 🏭 The "Factory" Narrative
>
> This repository follows a strict **"Source vs. Artifact"** philosophy:
>
> - **Houdini (Fabricator):** The procedural "factory" where assets are generated. Source files (`.hip`) are proprietary and **excluded** from this repository.
> - **USD (Artifact):** The "product" of the factory. These are the optimized files needed to run the Digital Twin in Omniverse.
> - **Synthetic Data:** Telemetry streams are emulated via Python generators to simulate robust edge cases (e.g., extreme thermal loads) that are rarely captured in real-world data.

### 📦 Asset Hydration

To keep this repository lightweight, heavy binary assets (USD Crates, Textures, HDRIs) are stored externally.

- [**Download Asset Pack (One Drive / S3 Link TBD)**](https://example.com/placeholder)

**Hydration Steps:**

1. Download the ZIP archive from the link above.
2. **Extract contents** directly into the `assets/_external/` folder.
    - *Note: This folder already exists (anchored by `.gitkeep`), so you simply unzip into it.*
    - *Result:* Your local path should look like `assets/_external/usd/my_asset.usd`.

## 📜 Technical Stack

- **Python**: 3.10+
- **Houdini**: 21.0.596 (PDG, Pyro, Fluid)
- **Omniverse**: 2024.x (USD, Isaac Sim 5.1)
- **Conda**: Environment isolation (`case02-env`)

---

## 📜 Changelog

- **2026-02-02:** Implemented external storage strategy for heavy assets (Git-agnostic).
- **2026-01-22:** Initial repository bootstrap. Established "Gold Standard" structure (ADRs, Pre-commit, Hybrid Access).
