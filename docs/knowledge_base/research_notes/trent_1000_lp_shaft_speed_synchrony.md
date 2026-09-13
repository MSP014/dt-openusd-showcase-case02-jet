# Trent 1000 LP Shaft Speed Synchrony

**Status:** Research note. Candidate input to `JET-16` only; not a released
telemetry contract or a fault-model specification.

## Source

- **Document:** *RRT 1000 Updated (May 23)*, training material, Revision 0
  dated 1 January 2022, pages 293-294 in the text version.
- **Accessed:** 10 August 2026.
- **Link:** https://fliphtml5.com/ognnl/rypx/RRT_1000_Updated_%28May_23%29/

The hosted training material states that each EEC protection channel uses two
low-pressure compressor (`N1C`) speed probes and two low-pressure turbine
(`N1T`) speed probes. The EEC compares the `N1C` and `N1T` signals; a detected
difference between valid turbine and compressor speed signals triggers LP shaft
breakage protection and a protective engine shutdown.

The same material describes the probes as magnetic sensors reading phonic wheels
at the compressor and turbine ends of the LP shaft. It also distinguishes the
LP compressor probes, which support control, indication, overspeed, and shaft
breakage detection, from the LP turbine probes, which support shaft-breakage
detection and overspeed protection.

## Case 02 Interpretation

The source supports the inference that `N1C` and `N1T` are observations of the
same LP shaft angular state rather than two independently geared rotational
systems. It does not provide an approved Case 02 telemetry schema, calibration
values, alarm thresholds, or an EEC implementation contract.

For the normal Case 02 operating states, a future synthetic provider should use
one canonical LP shaft speed as the source value. Optional `N1C` and `N1T`
diagnostic readings should be derived from that shared value and remain
consistent when the model represents a mechanically intact engine.

## Candidate Inputs To JET-16

| Candidate | Role | Status |
| --- | --- | --- |
| `n1_rpm` | Canonical synthetic LP shaft speed for the normal runtime. | Candidate |
| `n1c_rpm` | Optional compressor-end diagnostic reading derived from `n1_rpm`. | Candidate |
| `n1t_rpm` | Optional turbine-end diagnostic reading derived from `n1_rpm`. | Candidate |
| `lp_speed_delta_rpm` | Derived diagnostic difference between the two readings. | Candidate |

`lp_speed_delta_rpm` must not be presented as a live engine measurement or a
validated health-monitoring signal. If exposed in the HUD, its quality must be
marked synthetic or derived according to the eventual provider contract.

## Deliberate Exclusions

- No LP shaft-breakage scenario, protective shutdown sequence, or fault
  simulation is added to the four-state showreel scope.
- No EEC logic, alarm threshold, sensor redundancy model, or maintenance claim
  is inferred from this note.
- This note does not change the L1 boundary: Case 02 remains a pre-baked,
  telemetry-driven visualisation prototype.

## Questions For JET-16

1. Should the standard HUD expose only `n1_rpm`, reserving `N1C` and `N1T` for
   a close-up engineering diagnostic view?
2. Should optional presentation noise be applied only after deriving both
   diagnostic readings from the canonical LP shaft speed?
3. Is an explicit normal-state consistency indicator useful, or would it add
   dashboard detail without improving the showreel evidence?
