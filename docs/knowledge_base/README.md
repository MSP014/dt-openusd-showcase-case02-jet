# Jet Engine Digital Twin (Case 02) - Knowledge Base

This directory is the durable knowledge base for Case 02. It separates authored
project contracts from raw or source-specific material so the README, scope,
architecture, validation boundary, and public claims do not drift every time a
useful source appears.

## Authoritative Project Documents

| Document | Purpose |
| :--- | :--- |
| [Main Concept](main_concept.md) | Canonical product and experience specification: operational states, visual modes, LOD strategy, telemetry role, and semantic contracts. |
| [Validation and Limitations](validation_and_limitations.md) | Claim boundary for what the prototype validates, what it does not claim, and which evidence should be produced. |
| [Reference Geometry and Provenance](reference_geometry_and_provenance.md) | Rules for public-reference geometry, scale anchors, provenance, visual proxy assumptions, and non-claims. |
| [Pipeline Workflow](pipeline_workflow.md) | Source-agnostic production chain from public references through Houdini, USD composition, telemetry binding, Omniverse playback, and visual proof export. |
| [Digital Twin Maturity Levels](digital_twin_maturity_levels.md) | Theoretical framing for the maturity level Case 02 can reasonably demonstrate. |

## USD Architecture Contracts

| Document | Purpose |
| :--- | :--- |
| [Project USD Contract](usd_architecture/00_project_usd_contract.md) | Naming conventions, units, and structural principles for the USD scene graph assembly. |
| [Payloads and References](usd_architecture/01_payloads_references_assembly.md) | Composition rules for heavy engine geometry and close-up payload loading. |
| [Instancing and Radial Geometry](usd_architecture/02_instancing_radial_geometry.md) | PointInstancer strategy for repeated radial components such as compressor and turbine blades. |
| [Houdini Simulation Caches and States](usd_architecture/03_houdini_sim_caches_and_states.md) | Technical layout of the operational state and visual mode VariantSets. |
| [Custom Attributes and Telemetry](usd_architecture/04_custom_attributes_telemetry.md) | Schema parameters and USD attributes consumed by the telemetry/data provider layer. |

## Reference Material

[`reference_material/`](reference_material/) stores external or source-like
materials used to ground scale, facility context, telemetry labels, and digital
twin terminology. These files are inputs, not project contracts by themselves.

| Material | Use |
| :--- | :--- |
| [Engine Technical Specifications](reference_material/Trent%201000%20-%20Engine%20Technical%20Specifications.md) | Baseline public physical data for dimensions, thrust, and spool ranges. |
| [Testbed 80 Facility Details](reference_material/Testbed_80_Facility_Details.md) | Facility scale and narrative context for the test-cell environment. |
| [FUI and HUD Data Mapping](reference_material/FUI%20and%20HUD.txt) | Raw telemetry labels and UI-facing data names. |
| [Digital Twin Maturity Model PDF](reference_material/digital_twins_a_maturity_model_for_their_classification_and_evaluation.pdf) | Source material behind the maturity-level framing. |

## Research Intake

| Directory or Register | Purpose |
| :--- | :--- |
| [research_notes/](research_notes/) | Source-specific notes kept behind an intake gate before they affect project scope, architecture, claims, or README text. |
| [Source Influence Register](research_notes/source_influence_register.md) | Canonical register of which external sources influenced Case 02 and what was taken, excluded, or parked. |
| [transcripts/](transcripts/) | Raw transcripts and research logs used as supporting context. |

## Update Rule

Keep the simulation matrix and telemetry-facing contracts synchronised with
[Main Concept](main_concept.md) and the relevant USD architecture contract. Do
not create parallel matrix or schema documents unless the project deliberately
extracts them into a single new canonical contract.
