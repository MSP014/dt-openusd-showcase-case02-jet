# 00. Master USD Contract: Case 02 (Jet Engine)

This document serves as the absolute baseline for all USD asset generation and assembly within the Case 02 (Aerospace Propulsion Digital Twin) project. It defines the rigid structural conventions required to handle 10,000+ mechanical CAD parts efficiently in Omniverse.

## 1. Scene Fundamentals

* **MetersPerUnit:** `1.0` (Meters)
* **UpAxis:** `Y` (Ensure Houdini exports are Y-up to match Omniverse natively).

## 2. Naming and Hierarchy (The Engine Topology)

All mechanical assemblies must follow a strict, logical hierarchy. Avoid flat structures.

* **Root Prim:** `/Engine`
* **Major Modules (Sub-assemblies):**
  * `/Engine/Fan`
  * `/Engine/Compressor_IPC` (Intermediate Pressure)
  * `/Engine/Compressor_HPC` (High Pressure)
  * `/Engine/Combustion_Chamber`
  * ...
* **Leaf Nodes (Geometry):** Must be clearly identifiable (e.g., `/Engine/Fan/Blade_01`).

## 3. LODs and Purpose Contract (GPU Optimization)

With 10,000+ CAD parts, rendering performance depends entirely on strict Purpose tagging.

* **VariantSet Name:** Exactly `LOD`.
* **Variant Names:** `LOD0` (highest, CAD-fidelity), `LOD1` (medium), `LOD2` (proxy bounding boxes or decimated meshes).
* **Purpose Application:** Purpose attributes (`render`, `proxy`, `guide`) must be applied exclusively to `Geom` (Mesh/Curve) primitives, **not** to the root `Xform` of the sub-assembly.

*Workflow:* This allows Omniverse to instantly hide millions of polygons by switching the viewport display filter to **Proxy**, ensuring smooth navigation across the entire engine test stand.

## 4. Asset Dependencies

* **Materials:** All shader graphs must be isolated in `.usd`/`.usda` files under `assets/materials/`.
* **Textures:** All bitmap textures must reside in `assets/textures/`.
* **Binding:** Material bindings must use relative paths resolving from the currently open layer.
