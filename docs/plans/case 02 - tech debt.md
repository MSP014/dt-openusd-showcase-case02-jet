# Case 02 Technical Debt

## 1. Unresolved Technical Debts

_There are currently no unresolved technical debts._

## 2. Resolved Technical Debts

### [ENVIRONMENT] Install OpenUSD Python Bindings (`pxr`)

- **Status:** Closed (2026-07-17)
- **Severity:** Medium (Tooling)
- **Description:** Case 02 needed OpenUSD Python bindings in `case02-env` so maintainers and scripts can inspect stages, variants, primvars, material bindings, payloads, and invalid data such as NaN UVs without falling back to DCC-specific CLI tools.
- **Resolution:** Added `usd-core` to the dependency input file, regenerated the lock file, and installed the locked dependency into `case02-env`.
- **Resolution Actions (2026-07-17):**
  - Added `usd-core` to `requirements.in`.
  - Recompiled `requirements.txt` with `pip-compile`; the lock now pins `usd-core==26.5`.
  - Installed dependencies from `requirements.txt` into `case02-env`.

#### Evidence

- Local check: `python -c "from pxr import Usd, UsdGeom, Sdf; print('pxr ok', Usd.GetVersion())"` -> `pxr ok (0, 26, 5)`.
- Dependency check: `python -m pip check` -> `No broken requirements found.`
- Security check: `pip-audit -r requirements.txt` -> `No known vulnerabilities found.`

### [ENVIRONMENT] Python Baseline Migration Review

- **Status:** Closed (2026-07-03)
- **Severity:** Medium
- **Description:** Case 02 was previously anchored on Python 3.10.x by ADR 004. That baseline aligned with Omniverse Kit's embedded CPython 3.10 runtime, but Python 3.10 reaches end-of-life in October 2026. A baseline migration was required before the project environment became stale.
- **Resolution:** Adopted Python 3.11.x as the Case 02 baseline in ADR 004 after validating the current toolchain in a temporary Python 3.11 environment.
- **Resolution Actions (2026-07-03):**
  - Created a temporary Python 3.11 environment.
  - Verified that the Python 3.10 lock file is not directly installable on Python 3.11.
  - Re-resolved dependencies from `requirements.in` under Python 3.11.
  - Updated the canonical local `case02-env` environment to Python 3.11.15.
  - Ran tooling, QA, MCP, and Jira smoke checks under Python 3.11.

#### Evidence

- Local check: `python --version` -> `Python 3.11.15`.
- Static checks: `py_compile`, `black --check --no-cache`, `isort --check-only`, `flake8`, and `bandit` passed for the current tool scripts.
- Test check: `pytest` passed.
- Dependency check: `pip-audit` passed.
- Runtime helper checks: USD, Kit, and OmniUI MCP `list-tools` commands passed.
- Jira checks: `jira_link.py list` and `sync_jira.py` dry-run passed.
- Hook check: `pre-commit run --all-files` passed on the canonical `case02-env` environment.

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

- **2026-07-17:** Follow-up check completed. Restored live `case02-env` to `pip 26.1.1`; confirmed `pip-tools 7.5.3`; recompiled the lock file while adding `usd-core==26.5`; `pip check` and `pip-audit -r requirements.txt` passed.
- **2026-07-01:** Follow-up check completed. `pip-tools` remains current at `7.5.3` (installed/latest). `pip` reports a newer patch release (`26.1.2`) than the installed `26.1.1`; no lock refresh was performed during session bootstrap.
- **2026-05-21:** Tech debt closed after environment upgrade and lock-file regeneration.
