# Rolls-Royce Trent 1000 — Technical Specifications

**Engine Family:** Rolls-Royce Trent
**Application:** Boeing 787 Dreamliner
**Architecture:** Three-shaft high-bypass turbofan

---

## Physical Dimensions

| Parameter | Imperial | Metric |
| :--- | :--- | :--- |
| **Overall Length** | 186.5 in | **4,738 mm / 4.738 m** |
| **Fan Diameter** | 112 in | **2,850 mm / 2.85 m** |

> **Modelling Note:** Aspect ratio Length : Fan Diameter ≈ 1.67 : 1.
> Use Fan Diameter as the primary scale reference when importing the STL.

---

## Mass & Performance

| Parameter | Value |
| :--- | :--- |
| **Dry Weight** | 5,936 – 6,120 kg (13,087 – 13,492 lb) |
| **Take-off Thrust** | 265.3 – 360.4 kN (59,600 – 81,000 lbf) |
| **Bypass Ratio** | > 10 : 1 |
| **Overall Pressure Ratio (take-off)** | 50 : 1 |
| **Air Mass Flow** | 1,090 – 1,211 kg/s |

---

## Spool Architecture

### LP System (Low Pressure)

| Parameter | Value |
| :--- | :--- |
| **Fan Blades** | 20 (wide-chord, hollow titanium, snubberless) |
| **Fan Stages** | 1 |
| **LP Turbine Stages** | 6 |
| **LP Speed Range** | ~2,900 rpm |

### IP System (Intermediate Pressure)

| Parameter | Value |
| :--- | :--- |
| **IP Compressor Stages** | 8 (axial flow) |
| **IP Turbine Stages** | 1 |
| **IP Speed Range** | ~10,200 rpm |
| **Note** | Counter-rotates relative to HP spool |

### HP System (High Pressure)

| Parameter | Value |
| :--- | :--- |
| **HP Compressor Stages** | 6 |
| **HP Turbine Stages** | 1 |
| **HP Speed Range** | ~13,617 rpm |
| **HP Turbine Operating Temp** | > 1,500 °C (≈ 200 °C above blade melting point) |

---

## Combustion System

| Parameter | Value |
| :--- | :--- |
| **Combustor Type** | Single annular |
| **Fuel Spray Nozzles** | 18 |
| **EGT Rated Temp (Departure)** | ISA + 15 °C / ISA + 22 °C |

---

## Rotation

| Parameter | Value |
| :--- | :--- |
| **Direction of Rotation (front view)** | Clockwise |

---

## Internal Dimensions — Availability Note

Specific internal diameters for HP/IP/LP turbine and compressor stages are **proprietary Rolls-Royce data** and are not publicly disclosed. For modelling purposes, rely on the geometry of the source STL asset and validate against the Fan Diameter (2,850 mm) as the master scale reference.

---

## Sources

- Rolls-Royce factory display board (Flightradar24 tour, 2026-01-30) — `Flightradar24 - 2026.01.30 - How Rolls-Royce Jet Engines Are Built.md`
- [Rolls-Royce official product page](https://www.rolls-royce.com)
- [Wikipedia — Rolls-Royce Trent 1000](https://en.wikipedia.org/wiki/Rolls-Royce_Trent_1000)
- Delta TechOps Engine Datasheet
- stands.aero specifications database
