# Jet Engine Engineering and Manufacturing Reference Pack

## Purpose

This document curates the engineering and manufacturing material supplied with
the Veritasium episode *The Ridiculous Engineering of Jet Engines*. It retains
only the sources and observations that can improve Case 02 without expanding
the project into a general jet-engine encyclopaedia.

Use this pack as a source map for:

- Trent 1000 architecture and terminology;
- qualitative gas-path and thermodynamic behaviour;
- turbine-blade manufacture, materials and cooling;
- clearly labelled Trent XWB cross-family visual reference;
- optional future degradation research.

This document does not replace:

- [Reference Geometry and Provenance](../reference_geometry_and_provenance.md),
  which defines the project-wide provenance policy;
- [Trent 1000 Technical Specifications](./trent_1000_engine_technical_specifications.md),
  which records engine-specific values used by the project;
- [Source Influence Register](../research_notes/source_influence_register.md),
  which records how individual research sources affect project decisions.

## Source-use contract

Source authority depends on the type of claim. A single global ranking is not
sufficient.

| Claim type | Preferred source | Secondary source | Guardrail |
| :--- | :--- | :--- | :--- |
| Trent 1000 configuration, dimensions and published performance | Official Rolls-Royce Trent 1000 material | Traceable Trent 1000 training material | Record the engine variant and publication context. |
| Generic turbofan physics and thermodynamics | NASA or peer-reviewed engineering literature | University engineering material | Use for qualitative rules unless a validated model is available. |
| Turbine materials and cooling principles | Peer-reviewed literature, Cambridge Rolls-Royce UTC and Rolls-Royce | High-quality engineering explainers | Do not imply a proprietary Trent 1000 alloy, coating or cooling geometry. |
| Manufacturing and visual appearance | Official manufacturer footage | Veritasium or Animagraffs | Secondary visualisations are explanatory, not dimensional evidence. |
| Cross-family comparison | Rolls-Royce Trent XWB material | Secondary XWB material | Label it as cross-family reference and never silently transfer exact values. |

### Hard constraints

- **Trent XWB is not a dimensional source for Trent 1000.**
- Do not transfer XWB blade counts, rotational speeds, stage dimensions,
  airfoil shapes, shaft diameters, temperatures, pressure ratios or component
  proportions without an independent Trent 1000-specific source.
- Current Rolls-Royce Trent 1000 product material may describe the **Trent 1000
  XE**. XE-specific statements must not be presented as universal facts about
  every Trent 1000 variant.
- Public sources support a convincing visual proxy and qualitative engineering
  logic. They do not support certification-grade geometry, telemetry or CFD
  claims.

## Core Trent 1000 references

### Rolls-Royce — Trent 1000 product page

**Source:** [Trent 1000: Boeing 787 engine](https://www.rolls-royce.com/products-and-services/civil-aerospace/widebody/trent-1000.aspx)

Use for manufacturer-approved terminology, high-level architecture and current
Trent 1000 technology context. Treat statements about current durability and
high-pressure turbine improvements as variant- and date-sensitive.

### Rolls-Royce — Trent 1000 high-resolution infographic

**Source:** [Trent 1000 high-resolution poster (PDF)](https://www.rolls-royce.com/~/media/Files/R/Rolls-Royce/documents/civil-aerospace-downloads/High-Res-posters/High-Res-poster-trent-1000.pdf)

This is the preferred public architecture anchor for:

- the three-shaft arrangement;
- fan, compressor and turbine stage counts;
- published fan diameter and high-level engine proportions;
- module naming and cutaway interpretation.

Use the project technical-specification document for adopted values rather than
copying values independently into implementation code.

### Cambridge Rolls-Royce UTC — operating overview

**Source:** [How does a jet engine work?](https://www.rrutc.msm.cam.ac.uk/outreach/what-do-we-do/how-does-a-jet-engine-work)

Use for a concise compressor → combustor → turbine explanation and educational
terminology. It is not a Trent 1000 dimensional source.

## Gas-path and thermodynamic references

These references define qualitative sanity rules for synthetic fields and
pre-baked visual states. They are not substitutes for validated CFD.

| Source | Supports | Case 02 use |
| :--- | :--- | :--- |
| [NASA Glenn — Turbofan Engine](https://www.grc.nasa.gov/www/k-12/VirtualAero/BottleRocket/airplane/aturbf.html) | Fan, bypass and core-flow relationships; rotor/stator concepts | Keep bypass and core flow visually distinct. |
| [NASA Glenn — Turbofan Thrust](https://www.grc.nasa.gov/www/k-12/airplane/turbfan.html) | Core- and bypass-stream contributions to thrust | Explain why fan flow is visually dominant without inventing Trent 1000-specific flow values. |
| [NASA Glenn — Power Turbine Thermodynamics](https://www.grc.nasa.gov/www/k-12/airplane/powtrbth.html) | Turbine expansion, temperature drop and work extraction | Ground qualitative pressure, temperature and shaft-load relationships. |
| [NASA Glenn — Isentropic Compression or Expansion](https://www.grc.nasa.gov/WWW/K-12/BGP/compexp.html) | Pressure/temperature relationships during idealised compression and expansion | Apply basic field-direction sanity checks. |
| [NASA Glenn — Brayton Cycle](https://www.grc.nasa.gov/www/k-12/airplane/brayton.html) | Compressor, combustor and turbine state sequence | Provide the backbone for simplified operating-state explanations. |

The minimum qualitative model is:

- compressor: total pressure rises and total temperature rises;
- combustor: total temperature rises sharply while fuel energy is added;
- turbine: total pressure and total temperature fall as work is extracted;
- shafts transfer turbine work to the fan and compressors;
- bypass and core are separate flow systems, even when shown together;
- all Case 02 fields remain synthetic or procedurally authored unless explicitly
  documented otherwise.

## Veritasium episode: usable extraction

**Source:** [The Ridiculous Engineering of Jet Engines](https://www.youtube.com/watch?v=QtxVdC7pBQM)

The episode is a secondary explanatory source with access to Rolls-Royce
facilities and engineers. Its greatest value for Case 02 is the manufacturing
and cooling narrative, not its numerical engine model.

### Useful sequences

| Timecode | Topic | Permitted use |
| :--- | :--- | :--- |
| `12:52–16:59` | Ceramic core, wax pattern and investment-casting shell | Manufacturing sequence and possible close-up storytelling. |
| `25:38–31:25` | Directional solidification and single-crystal casting | Explain why grain control matters in hot-section blades. |
| `32:22–34:35` | Internal passages, turbulators, film-cooling holes and coatings | Ground a simplified blade cutaway or material-layer visual. |

Reusable manufacturing sequence:

`ceramic core → wax pattern → investment casting → directional or single-crystal solidification → heat treatment → core removal → film-cooling holes → coatings → inspection`

### Numerical quarantine

The episode identifies its illustrated engine model and many quoted figures as
Trent XWB-based. The following examples must not become Trent 1000 facts merely
because they appear in the episode:

- approximately 1.3 tonnes of air per second;
- the illustrated core/bypass split;
- compressor-discharge and combustor temperature figures;
- 68 high-pressure turbine blades;
- 12,500 rpm high-pressure turbine speed;
- 97,000 lbf test-engine thrust;
- illustrated internal dimensions, blade shapes and component proportions.

If a value also appears in a Trent 1000 source, cite and adopt the Trent 1000
source independently.

## Turbine-blade manufacture, materials and cooling

| Source | Authority | Use for | Limitation |
| :--- | :--- | :--- | :--- |
| [Cambridge Rolls-Royce UTC — How is a Jet Turbine Engine like a Baked Alaska?](https://www.rrutc.msm.cam.ac.uk/outreach/articles/how-is-a-jet-turbine-engine-like-a-baked-alaska) | University / Rolls-Royce-linked outreach | Internal cooling, film cooling and thermal-barrier coatings | General principle, not proprietary blade geometry. |
| [Pollock and Tin — Nickel-Based Superalloys for Advanced Turbine Engines](https://doi.org/10.2514/1.18239) ([open-access copy](https://deepblue.lib.umich.edu/items/3aa9b264-de86-4858-aeb4-f66cd480f299)) | Peer-reviewed review paper | Nickel superalloys, creep, fatigue, processing and single-crystal context | Scientific backbone, not a Trent 1000 material specification. |
| [Langston — Each Blade a Single Crystal](https://www.americanscientist.org/node/225) | Engineering overview | Accessible explanation of grain boundaries and single-crystal blades | Explanatory bridge, not a primary dimensional source. |
| [Cambridge Rolls-Royce UTC — Nickel-Based Superalloys](https://www.rrutc.msm.cam.ac.uk/research-themes/nickel-base-superalloys) | University / Rolls-Royce-linked research | Concise blade-alloy and high-temperature materials context | Does not identify the exact project engine alloy. |

### Modelling rule

These sources explain why a hot-section blade may be represented as internally
cooled, film-cooled and ceramic-coated. They do not require Case 02 to model:

- serpentine cooling passages;
- every film-cooling hole or turbulator;
- microscopic coating layers;
- crystal lattices or alloy microstructure.

Add such geometry only when it materially affects the silhouette, a planned
close-up or visible engineering evidence.

## Trent XWB cross-family visual reference

> **Cross-family reference — not Trent 1000 dimensional data.**

| Source | Permitted use | Do not use for |
| :--- | :--- | :--- |
| [Rolls-Royce displays Trent XWB at ILA](https://www.rolls-royce.com/media/press-releases/2018/25-04-2018-rr-displays-trent-xwb-and-pioneering-technology-at-ila.aspx) | Trent-family visual language and assembly-level context | Trent 1000 dimensions or configuration values. |
| [Rolls-Royce Trent XWB infographic](https://ve42.co/TrentXWB2) | Cutaway communication, hierarchy and visual composition | Trent 1000 component proportions. |
| [Rolls-Royce — How We Assemble the Trent XWB](https://www.youtube.com/watch?v=K2R6NTgvEV4) | Casing, flange, accessory, surface-treatment and scale cues | Exact Trent 1000 construction. |
| [First A350-1000 delivery / Trent XWB-97 facts](https://www.rolls-royce.com/media/press-releases/2018/20-02-2018-rr-joins-airbus-and-qatar-airways-to-celebrate-delivery-of-first-a350-1000.aspx) | XWB-specific comparison context | Any unlabelled Trent 1000 numerical claim. |
| [Animagraffs — Inside a Jet Engine](https://animagraffs.com/inside-a-jet-engine/) ([video](https://www.youtube.com/watch?v=L24Wf0VlTE0)) | Rotor/stator readability and explanatory visual language | Exact geometry or engineering data. |

## Optional degradation research

Status: **parked; not part of the active production scope**.

| Source | Potential future use |
| :--- | :--- |
| [Rolls-Royce — A more robust Trent XWB-97 in the Middle East](https://www.rolls-royce.com/media/our-stories/discover/2024/a-more-robust-trent-xwb-97-in-the-middle-east.aspx) | Dust, sand and CMAS context; thermal-barrier-coating durability. |
| [DST Group — Characterisation of Dirt, Dust and Volcanic Ash](https://www.dst.defence.gov.au/publication/characterisation-dirt-dust-and-volcanic-ash-study-potential-gas-turbine-engine) ([PDF](https://www.dst.defence.gov.au/sites/default/files/publications/documents/DST-Group-TR-3367.pdf)) | Compressor erosion, ingestion, turbine deposition and coating degradation. |

These sources may support a future contaminated or degraded visual state only
after that state has explicit project scope and acceptance criteria. They do not
authorise a dust, CMAS or materials-degradation simulation by themselves.

## Application to Case 02

| Project decision | Reference basis | Constraint |
| :--- | :--- | :--- |
| Global scale and recognisable Trent 1000-class proportions | Official Trent 1000 material and the existing provenance document | Internal geometry remains a visual proxy. |
| USD module naming and engine zoning | Official Trent 1000 poster plus traceable Trent 1000 training material | Project hierarchy may simplify proprietary part breakdowns. |
| Synthetic temperature, pressure and velocity fields | NASA qualitative thermodynamics | Physically inspired, not validated CFD. |
| Hot-section construction story | Rolls-Royce/Cambridge material, Pollock and Tin, and selected Veritasium sequences | No claim of exact Trent 1000 alloy or cooling geometry. |
| Surface and assembly look development | Official Trent-family imagery and footage | XWB material stays visibly tagged as cross-family. |
| Degraded operating state | Parked Rolls-Royce and DST material | No production work until separately approved and scoped. |

## Material deliberately excluded from the active knowledge base

The full episode bibliography contains legitimate sources needed for its wider
materials-science and historical narrative. The following branches do not
currently improve visible evidence, architecture, technical trust or simulation
plausibility for Case 02:

- individual alloying-element studies;
- gamma-prime precipitate evolution at research-paper depth;
- grain-boundary excess volume, segregation and diffusion;
- crystallographic and dislocation-mechanism detail;
- generic steel-versus-titanium comparisons;
- Whittle history and early jet-engine chronology;
- airline ticket-price and inflation comparisons;
- generic cruising-altitude trivia.

They should remain outside the active project unless a future, approved visual
or validation requirement creates a direct need.

## Intake rule

A new source may change public Case 02 documentation only when it changes at
least one of the following:

- project scope;
- pipeline architecture;
- visible engineering evidence;
- explicit project limitations.

Otherwise, record it as source-specific research and keep production decisions
anchored to the established project documents.
