# aqua-iq
Autonomous Surface Vehicle (ASV) &amp; Virtual Monitoring Station for Water Quality Analysis

AQUA-PROBE RS-IV // Robotic Surface Vehicle with Retractable Multi-Sensor Payload

Autonomous Surface Vehicle (ASV) & Virtual Monitoring Station for Multi-Parameter Water Body Analysis
Designed for real-time limnological research across still water bodies (lakes, reservoirs, ponds, quarry impoundments, and stormwater basins).

1. Project Overview

The AQUA-PROBE RS-IV is an autonomous and remotely operated robotic surface vehicle (RSV) engineered to monitor freshwater quality. Equipped with a motorized retractable multi-sensor payload, the vehicle traverses still water bodies and lowers a multi-parameter sensor capsule to depths of up to 5.0 meters to capture stratified vertical limnological data.

Four Integrated Sensors:
Total Dissolved Solids (TDS) Sensor: Measures dissolved ionic species and general salinity in ppm (parts per million).
pH Sensor: High-impedance glass electrode measuring acidity/alkalinity across the standard 0.00 – 14.00 pH scale.
Water Temperature Sensor: Precision thermistor / DS18B20 digital probe recording water thermal stratification in °C.
Turbidity Sensor: Optical nephelometric sensor (850 nm NIR scattering) measuring suspended solids and water clarity in NTU.
2. Dashboard Features & Capabilities
Cyber-Marine Aesthetic: Navy dark background (#030712), cyan (#00f5d4), electric blue (#38bdf8), and amber/emerald alert accents with glassmorphism UI.
Centerpiece Vehicle & Winch Visualizer:
Live 2D cross-section animation of the twin-hull catamaran platform floating on still water.
Animated motorized winch spool paying out and retrieving the braided umbilical cable.
4-sensor cylindrical probe head with active indicator callouts and status glows.
Dynamic optical water simulation: water column opacity and suspended particulate count visually respond in real-time to current Turbidity (NTU).
Depth gauge and telemetry HUD (Heading, Pitch, Roll bobbing dynamics, GPS positioning).
One-click depth presets: 0.5m (Surface), 2.5m (Mid-depth), and 4.5m (Benthic).
Four Live Sensor Monitoring Cards:
Instantaneous readings, units, status badges, and sparkline trends.
Running Minimum, Maximum, and Average telemetry stats.
Automated Water Quality Analysis:
Radial WQI Gauge (0–100) computed via the Weighted Arithmetic Water Quality Index (WAWQI) method.
Parameter sub-index breakdown bars (
𝑞
TDS
,
𝑞
pH
,
𝑞
Temp
,
𝑞
Turb
q
TDS
	​

,q
pH
	​

,q
Temp
	​

,q
Turb
	​

).
Dynamic rule-based advisory engine (identifies algal bloom risks, sediment plumes, acidic drift).
Explicit scientific potability disclaimer distinguishing physical-chemical limnology from microbiological drinking-water certification.
High-Performance Multi-Parameter Charts:
Real-time synchronized time-series with selectable time windows (1m, 5m, 30m, 1h, All).
Parameter views: Combined Normalized view or dedicated individual parameter views with regulatory thresholds.
Interactive hover crosshairs and tooltips.
Dual-Mode Simulator & Hardware Bridge:
Simulation Mode: Real-time 2-second physics generator with stochastic Brownian fluctuations.
Manual Override Sliders: Instantly adjust any of the 4 parameters to evaluate custom conditions.
5 Environmental Presets: Pristine Alpine Lake, Agricultural Runoff / Algal Bloom, Industrial Effluent, Post-Storm Silt Inflow, Stagnant Alkaline Pond.
Hardware Mode: Ready to bridge with physical ESP32 or Arduino devices via standard JSON over MQTT / WebSockets (rsv/telemetry/sensors).
Data Logging & Report Generation:
Live tabular telemetry log with ring-buffer storage.
Export as CSV: Generates real downloadable .csv data files.
Executive Field Report: Printable research-grade monitoring summary with statistical metrics and field technician sign-off.
Safety & Audio:
Emergency Stop (E-STOP) with motor cutoff, flashing beacon, and safety override.
Interactive Low Battery Warning (<20%) alert system simulating 14% pack voltage drops.
Sensor Fault Injections: Test simulated probe drift, dry-sensor impedance spikes, and cable faults.
Payload Bus Diagnostics: Dedicated diagnostic telemetry modal monitoring individual probe health, sampling rates, and calibration validity.
Interactive System ONLINE/OFFLINE Toggle: Test loss-of-telemetry behavior and standby state.
Web Audio API procedural sound synthesizer (sonar pings, winch motor hum, telemetry beeps, alarm chimes).
3. Water Quality Index (WQI) Mathematical Formulation

The dashboard employs the Weighted Arithmetic Water Quality Index (WAWQI):

WQI
=
∑
(
𝑞
𝑖
⋅
𝑊
𝑖
)
∑
𝑊
𝑖
WQI=
∑W
i
	​

∑(q
i
	​

⋅W
i
	​

)
	​


Where:

𝑞
𝑖
q
i
	​

: Quality rating (sub-index) for the 
𝑖
i-th parameter calculated relative to standard permissible freshwater limits.
𝑊
𝑖
W
i
	​

: Unit weight assigned based on ecological significance:
pH: 
𝑊
pH
=
0.30
W
pH
	​

=0.30 (Ideal: 7.00, Standard: 6.50 – 8.50)
Turbidity: 
𝑊
Turb
=
0.25
W
Turb
	​

=0.25 (Ideal: 
≤
0.5
≤0.5 NTU, Standard: 25 NTU)
TDS: 
𝑊
TDS
=
0.25
W
TDS
	​

=0.25 (Ideal: 
≤
50
≤50 ppm, Standard: 500 ppm)
Temperature: 
𝑊
Temp
=
0.20
W
Temp
	​

=0.20 (Ideal: 20.0°C, Range: 10.0°C – 28.0°C)
Classification Scale:
90 – 100: EXCELLENT (Pristine oligotrophic condition)
75 – 89: GOOD (Acceptable freshwater ecosystem)
55 – 74: MODERATE (Slight eutrophication or runoff)
35 – 54: POOR (Significant contamination / degradation)
< 35: VERY POOR (Critical environmental alarm)
4. Hardware Architecture & ESP32 Integration

The repository includes a ready-to-flash Arduino/ESP32 sketch located at:
firmware/rsv_payload_esp32.ino

Pinout Mapping:
Component / Sensor	Interface	ESP32 GPIO Pin
Analog TDS Sensor	ADC1 Channel 6	GPIO 34
Analog pH Sensor	ADC1 Channel 7	GPIO 35
Turbidity Optical Sensor	ADC1 Channel 4	GPIO 32
DS18B20 Temp Sensor	OneWire Digital Bus	GPIO 4
Battery Voltage Divider	ADC1 Channel 5	GPIO 33
Winch Stepper STEP	Digital Output	GPIO 18
Winch Stepper DIR	Digital Output	GPIO 19
Winch Stepper ENABLE	Digital Output	GPIO 5
Winch Upper Limit Switch	Digital Input Pullup	GPIO 23
MQTT Telemetry JSON Schema:

Topic: rsv/telemetry/sensors (Published every 2.0s)

json
{
  "timestamp": 1731844000,
  "mode": "HARDWARE",
  "depth": 2.50,
  "tds": 245,
  "ph": 7.35,
  "temp": 21.4,
  "turbidity": 4.8,
  "battery": 88,
  "voltage": 14.8,
  "estop": false
}
5. How to Run the Dashboard

The dashboard is completely self-contained with zero external dependencies, build steps, or server requirements.

Simply open index.html in any modern web browser (Google Chrome, Microsoft Edge, Mozilla Firefox, Safari).
The simulation engine will automatically begin generating realistic 2-second telemetry packets.
Click DEPLOY SENSOR or use the depth presets to animate the winch and lower the multi-sensor capsule into the water.
Experiment with the Quick Scenario Presets (e.g. Agricultural Runoff / Algal Bloom) or adjust the manual sliders to observe real-time WQI score and visual clarity changes.
Click EXPORT CSV to download sequential mission data or click FIELD REPORT to view and print the research station report.
