# ADR 004: Environment Baseline

## Status

Accepted

## Context

Different Python versions introduce subtle incompatibilities in libraries like `usd-core`, `pandas`, or `numpy`. To ensure the "NVIDIA Showreel Standard" is maintainable, we must lock the Python version across the estate.

## Decision

We anchor all Case 02 environments on a specific Python baseline:

### 1. Python Version

* **Baseline**: **Python 3.11.x** (specifically 3.11.15+).
* **Rationale**: This version aligns Case 02 with the CY2025 VFX Reference Platform used by Houdini 21 while keeping the local Omniverse helper tooling compatible. The Omniverse Kit runtime may still embed its own Python interpreter; project-side tooling remains isolated in the `case02-env` conda environment.

### 2. Base Configuration

* **Package Manager**: `conda` (Miniconda/Anaconda) for environment creation.
* **Installer**: `pip` (latest) for package installation within the activated conda environment.

## Consequences

* **Positive**: Keeps the project environment aligned with the active DCC baseline and avoids carrying Python 3.10 past its support window.
* **Negative**: Requires regenerating the lock file under Python 3.11 and rebuilding environments that were created on Python 3.10 or older.
