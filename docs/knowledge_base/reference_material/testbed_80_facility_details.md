# Testbed 80: Technical Specifications & Context

## 1. Overview and Narrative Role

**Testbed 80**, located in Derby, UK, is the world's largest and most advanced indoor testing facility for aerospace engines. Opened in 2021 by Rolls-Royce (£90M investment), it is designed to test powerful turbofans like the Trent XWB, Trent 1000, and next-generation engines (UltraFan, hybrids).

In the context of the **Jet Engine Digital Twin (Case 02)**, Testbed 80 is not merely a decorative HDR lighting environment. It provides the **essential physical context** for the simulation.

- It grounds the UX in engineering reality.
- It provides a logical origin for the telemetry data (sensors, readouts).
- It serves as the interactive "Viewing Gallery" for the Assembly Sub-Mode.

## 2. Facility Architecture & Scale

- **Dimensions:** 130 meters long, with an internal area of 7,500 m² (larger than a football pitch).
- **Acoustic & Structural Integrity:**
  - Walls are up to 1.7m – 2.7m thick concrete.
  - Constructed using 3,128 tons of steel and 27,000 m³ of concrete.
- **Access:** Massive blast doors weighing 380–400 tons (up to 12m long, 10m high, 1.7m thick).
- **Aerodynamic Design:** The facility uses a "U"-shaped internal structure. Intake air flows through massive flow screens and filters to remove debris (FOD) and smooth the airflow before it reaches the engine. Exhaust is routed up through a 37-meter high exhaust tower to ensure safety and acoustic damping.

## 3. Testing Capabilities

Testbed 80 is a **static indoor testbed**. The engine is fixed to a structural pylon. It is *not* a flight-envelope wind tunnel (it does not simulate altitude pressure, mach-speed aerodynamics, or weather conditions like ice/rain). The facility forces ambient airflow through the engine using the engine's own suction and facility fans.

- **Thrust Capacity:** Can tether and test engines weighing up to 66 tons, producing up to 155,000 lbf (689 kN) of thrust.
- **Fuel Systems:** Features a 140,000-liter fuel tank, specifically designed to test 100% Sustainable Aviation Fuel (SAF).
- **Advanced Diagnostics:**
  - **In-operation X-Ray:** Can capture X-ray images of the engine internals *while running* at up to 30 frames per second (a unique capability in aerospace).
  - **Data Acquisition:** The sensor network captures up to 200,000 samples per second across 10,000+ distinct parameters, streaming directly to secure cloud infrastructure for analysis.

## 4. Digital Twin Relevance

In the Digital Twin application, the Testbed 80 environment justifies the following features:

1. **The Telemetry Dashboard:** The 10,000+ data points captured in real-life on Testbed 80 are abstracted into our UI (N1/N2/N3 RPM, EGT, Vibration, etc.).
2. **Interactive Cameras (Assembly Mode):** The sheer scale of the 7,500 m² room provides the spatial volume needed for the user to fly an interactive camera around the fully assembled engine, viewing it from the Control Room glass, the Pylon mount, or the Exhaust tower.
