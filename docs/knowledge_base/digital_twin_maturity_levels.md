# Digital Twin Maturity Levels — Classification Framework

2:
3: **Source:** Academic research (Tao et al.) and industry frameworks
4: **Date:** 2026-02-01
5: **Context:** Reference documentation for Case 02 (Jet Engine) Digital Twin classification
6:
7: ---
8:
9: ## Overview
10:
11: Digital Twin maturity levels classify the sophistication and capabilities of a digital twin, progressing from basic descriptive models to highly autonomous and predictive systems. The classification extends from **L0 (static replica)** to **L5 (autonomous co-existence)**, with the core operational levels being L1-L3.
12:
13: ---
14:
15: ## Level Definitions
16:
17: ### L0: No Twin / Static Model
18:
19: **Descriptor:** Descriptive / Static Replica
20:
21: **Characteristics:**
22:
23: - No active digital twin or only a static 3D model
24: - Describes and depicts the physical entity without real-time data connection
25: - Measurements are disparate or non-existent
26: - Sensors are not interconnected
27:
28: **Use Cases:**
29:
30: - Visualisation
31: - Planning and communication
32: - CAD/BIM models for documentation
33: - Geometry and material representation
34:
35: **Key Limitation:** No dynamic behaviour or connection to the physical world.
36:
37: ---
38:
39: ### L1: Status / Reflecting the Real with the Virtual
40:
41: **Descriptor:** Informative (Real-Time Data) / Connected - Manual
42:
43: **Characteristics:**
44:
45: - Real-time data capture from sensors
46: - Replicates the real-time state and changes in the physical entity
47: - Continuous monitoring of asset condition and performance
48: - Primary function: **"What is happening right now?"**
49:
50: **Use Cases:**
51:
52: - Real-time status dashboards
53: - Condition monitoring
54: - Performance tracking
55: - Asset visibility
56: - **FUI/HUD overlays for real-time telemetry**
57:
58: **Data Flow:** Physical Asset → Sensors → Digital Twin (Visualisation)
59:
60: **Key Capability:** Reflects current conditions dynamically.
61:
62: ---
63:
64: ### L2: Controlling the Real with the Virtual
65:
66: **Descriptor:** Informative (Real-Time + Historical Data) / Connected - Supervisory / Decision Support
67:
68: **Characteristics:**
69:
70: - Real-time data + historical data or benchmarks
71: - Can indirectly control the physical entity's operations
72: - Enables informed decision-making and analysis
73: - Captures and estimates effects of real-world events using historical data
74: - Primary function: **"What should I do about it?"**
75:
76: **Use Cases:**
77:
78: - Predictive maintenance (early stage)
79: - Asset optimisation
80: - Decision support systems
81: - Supervisory control (operator-in-the-loop)
82:
83: **Data Flow:** Physical Asset ↔ Digital Twin ↔ Operator (Decision Support)
84:
85: **Key Capability:** Supports operational decisions through analysis and recommendations.
86:
87: ---
88:
89: ### L3: Anticipating the Real with the Virtual
90:
91: **Descriptor:** Predictive / Forecasting
92:
93: **Characteristics:**
94:
95: - Forecasts future state and operational processes
96: - Couples real-time + historical data with ML or physical process-based models
97: - Predicts future behaviour and performance
98: - Primary function: **"What will happen next?"**
99:
100: **Data Flow:** Physical Asset → Digital Twin → ML/Simulation Models → Predictions → Operator
101:
102: **Key Capability:** Predictive analytics and future state forecasting.
103:
104: ---
105:
106: ## Case 02 (Jet Engine) — Classification Justification
107:
108: **Target Maturity:** **L1 (Informative / Connected)**
109: **Current Status:** **L1-oriented prototype using synthetic telemetry; operationally L0 until connected to validated real-time asset data.**
110:
111: ### Scenario
112:
113: A cutaway visualisation of a Trent 1000-class jet engine in a Testbed 80-inspired context. The prototype communicates qualitative flow, thermal, combustion, and component-state behaviour through Houdini-authored caches and synthetic telemetry.
114:
115: ### L1-Oriented Capabilities Demonstrated
116:
117: - **Data visualisation:** Displays scenario-driven pressure, temperature, and RPM values through the FUI.
118: - **State reflection pattern:** Updates the visual model for the selected Idle, Takeoff, Cruise, or Max Thrust scenario.
119: - **Inspection UX:** Uses cutaway, thermal, and flow-vector modes to communicate otherwise hidden component relationships.
120: - **Provider contract:** Separates telemetry input from the USD and Omniverse visualisation layers so a validated source can be integrated later.
121:
122: ### Why More Than Static Geometry?
123:
124: Synthetic scenario streams drive visual properties, state selection, airflow cues, and heat maps. This validates the interaction and data-binding pipeline, not the condition of a real engine.
125:
126: ### Why Not Yet Operational L1?
127:
128: The current project does not ingest validated real-time sensor data from a physical engine or test cell. It therefore demonstrates the intended L1 workflow without claiming an operational digital connection.
129:
130: ### Why Not L2 or L3?
131:
132: The prototype does not control a physical asset, support operational decisions, or predict future engine performance.
133:
134: ---
135:
136: ## Project Focus Areas
137:
138: 1. **Engineering Visualisation:** Translating Houdini-authored flow, thermal, and combustion caches into real-time Omniverse assets.
139: 2. **X-Ray Aesthetics:** Developing shaders that reveal internal structure without losing form definition.
140: 3. **FUI Integration:** Displaying synthetic scenario telemetry (EGT, N1/N2/N3) in 3D space.
141:
142: ---
143:
144: ## References
145:
146: 1. Tao et al. — Digital Twin Maturity Framework
147: 2. [Digital Twin Atlas](https://digital-twin-atlas.com/)
148: 3. Rolls-Royce "IntelligentEngine" vision
149:
150: ---
151:
152: **Document Status:** Knowledge Base Entry
153: **Last Updated:** 2026-07-17
154: **Maintained by:** Case 02 project
