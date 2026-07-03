# Case 02 Technical Debt

## 1. Unresolved Technical Debts

### [ENVIRONMENT] Python Baseline Migration Review

- **Status:** Open
- **Severity:** Medium
- **Description:** Case 02 is currently anchored on Python 3.10.x by ADR 004. This remains aligned with Omniverse Kit's embedded CPython 3.10 runtime, but Python 3.10 reaches end-of-life in October 2026. A baseline migration should be evaluated before the environment becomes stale.
- **Current Recommendation:** Do not move the main `case02-env` directly to Python 3.12. Evaluate Python 3.11 first, because Houdini 21 targets the CY2025 VFX Reference Platform where Python 3.11.x is the relevant DCC baseline.
- **Required Checks:**
  - Create a temporary `case02-env-py311` environment.
  - Install dependencies from `requirements.txt`.
  - Run existing tooling smoke tests (`jira_link.py`, `sync_jira.py`, MCP helper compilation).
  - Validate USD/Houdini/Omniverse-facing scripts once those runtime scripts exist.
  - Update ADR 004 only after compatibility is proven.
- **Non-Goal:** Python 3.12 migration is not planned until Omniverse Kit and Houdini integration evidence justifies it.

#### Next Check Date

- **Date:** 2026-08-15
- **Scope:** Reassess Python baseline after MCP helper adoption and any Phase 3 Omniverse Kit runtime work.

## 2. Resolved Technical Debts

### [SECURITY] Pip Security Lock (CVE-2026-1703)

- **Status:** Closed (2026-05-21)
- **Severity:** High
- **Description:** Resolved. `case02-env` now runs `pip 26.1.1` and `pip-tools 7.5.3` (compatible with pip 26.x), removing the blocker that kept `pip` on 25.3.
- **Resolution Actions (2026-05-21):**
  - Verified upstream release availability via `pip index versions pip-tools`.
  - Upgraded environment tooling: `pip 25.3 -> 26.1.1`, `pip-tools 7.5.2 -> 7.5.3`.
  - Recompiled lock file with upgrade intent: `pip-compile --upgrade-package pip-tools requirements.in`.
  - Confirmed lock output now pins `pip-tools==7.5.3` in `requirements.txt`.
- **Evidence:**
  - Local check: `python -m pip --version` -> `pip 26.1.1`.
  - Local check: `pip-compile --version` -> `pip-compile, version 7.5.3`.
  - Upstream check: `pip-tools` latest version available is `7.5.3`.

#### Next Check Date

- **Date:** 2026-07-15
- **Scope:** Dependency hygiene follow-up (`pip-tools`, `pip-audit`, lock refresh necessity).

#### Check Log

- **2026-07-01:** Follow-up check completed. `pip-tools` remains current at `7.5.3` (installed/latest). `pip` reports a newer patch release (`26.1.2`) than the installed `26.1.1`; no lock refresh was performed during session bootstrap.
- **2026-05-21:** Tech debt closed after environment upgrade and lock-file regeneration.
