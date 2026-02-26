# Case 02 Technical Debt

## [SECURITY] Pip Security Lock (CVE-2026-1703)

- **Status:** Open
- **Severity:** High
- **Description:** Environment (`case02-env`) is running `pip 25.3`, which contains **CVE-2026-1703**. CANNOT upgrade to `pip 26.0+` because it breaks `pip-tools`.
- **Tasks to Resolve:**
  - Do **NOT** upgrade pip until `pip-tools` releases a fix (approx. Late Feb 2026).
  - Check `pip-tools` updates weekly.
- **Notes:** If `pip-audit` flags this, verify that `pip` is on the ignore list (or accept the risk).
