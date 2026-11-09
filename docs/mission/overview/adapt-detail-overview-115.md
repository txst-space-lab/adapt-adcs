# ADAPT Detail Overview (rev. 115): text

Text of [`adapt-detail-overview-115.pdf`](adapt-detail-overview-115.pdf), the mission concept presented at the University Nanosatellite Program Mission Concept final event, 2026-07-17. Headings use the slide numbers printed on each slide. Slide text is reproduced as written. Diagram-only slides are summarised in *italics*; the PDF is authoritative for them.

## 1. ADAPT: Adaptive Drag-Aware Pointing Testbed

University Nanosatellite Program MC Final Event Presentation, July 17, 2026.

## 2. Mission Overview

*(Section divider.)*

## 3. Mission Narrative

Satellites operating in Very Low Earth Orbit (VLEO) experience increased atmospheric drag that is further amplified during space weather events, accelerating orbital decay and degrading attitude pointing performance. Space weather forecasting can predict such events minutes to days in advance, limiting the time available for mission operators to plan and uplink spacecraft operational updates before storm conditions develop. Additionally, classical attitude controllers used are tuned on the ground and cannot adapt autonomously to the rapidly changing disturbances. ADAPT seeks to test the performance of a dynamic ML-based controller in comparison to a classical static controller in this environment.

## 4. VLEO Environment (hidden slide; not in the PDF)

Perturbations alter the drag environment of VLEO. It is the driver of the chaos.

How drag impacts attitude & trajectory:

- **The "Center of Mass" Offset:** If a satellite tilts even slightly, the fast-moving air hits its sides unevenly, creating an instant aerodynamic twisting force (torque) that threatens to spin it out of control.
- **Attitude-Trajectory Coupling:** If the satellite loses its streamlined pointing (attitude), its forward-facing surface area (cross-sectional area) increases instantly, ramping up drag and destroying its speed.
- **The Death Spiral (Orbital Decay):** High atmospheric density saps the satellite's forward velocity, dragging it down into even thicker air where drag multiplies exponentially until it burns up on re-entry.
- **The ML Solution:** Our ML attitude controller acts as an automated pilot, adjusting orientation in milliseconds to keep the satellite perfectly streamlined, minimizing drag to preserve its trajectory.

## 5. Military and NASA Relevance

*(Graphic: how the mission relates to military and NASA interest in VLEO.)*

## 6. Mission Statement and Objectives

| Ref | Description | Parent |
|---|---|---|
| MS | ADAPT will compare CubeSat pointing performance metrics between an ML-based attitude controller and a classical baseline controller in the VLEO environment. | |
| MO-1 | Demonstrate pointing control of satellite using an ML-based attitude controller. | MS |
| MO-2 | Demonstrate the ability to update the ML-based attitude controller on-board. | MS |
| MO-3 | Compare the three-axis pointing RMS error between an ML-based controller and the classical controller. | MS |
| MO-4 | Compare actuator effort between an ML-based attitude controller and a classical controller. | MS |

## 7. Success Criteria Metrics

**Pointing Performance Index:** accounts for three-axis RMS error, attitude jitter, and pointing deviation to compare error of the ML-based controller and the classical controller.

```
PPI_W = E_W,ML / E_W,PID        E = (w1·θ_RMS) + (w2·ω_RMS) + (w3·θ_max)
```

**Actuator Performance Index:** accounts for electrical power draw, torque RMS, momentum accumulation rate, and desaturation frequency to compare the ML-based controller and the classical controller.

```
API_W = J_W,ML / J_W,PID        J = (w1·P) + (w2·τ_RMS) + (w3·H_accum) + (w4·N_desat)
```

## 8. Success Criteria Metrics: orbital regimes

| Orbital regime | | Description | Kp (aP) index |
|---|---|---|---|
| Space weather | Quiet | F10.7 solar flux of 65 to 110 SFU. Stable baseline atmospheric density of roughly [1.5×10⁻¹³ kg/m³, 1.0×10⁻¹¹ kg/m³] [TBR]. | 0–4 (0–39) |
| | Storm | Storm Mode prioritized. F10.7 solar flux of 150 to 250+ SFU. Features a 4–8x expansion in localized atmospheric density. | 5–9 (56–400) |
| Illumination | Sunlight | CubeSat in sight of the Sun. | – |
| | Eclipse | CubeSat is not in sight of the Sun. | – |

## 9. Success Criteria

| Ref | Description | Parent |
|---|---|---|
| **Minimum** | | |
| MSC-1 | Quantify the Pointing Performance Index (PPI), for at least 2 [TBR] orbital regime within a 50 [TBR] km altitude band. | MO-1, MO-3 |
| MSC-2 | Quantify the Actuator Performance Index (API), for at least 2 [TBR] orbital regime within a 50 [TBR] km altitude band. | MO-1, MO-4 |
| **Full** | | |
| FSC-1 | ML-based controller demonstrates reduction in pointing error, quantified as a [TBD] % decrease in the internal loss function relative to its initial on-orbit performance. | MO-2 |
| FSC-2 | Quantify the Pointing Performance Index (PPI), for at least 3 [TBR] orbital regimes. | MO-3 |
| FSC-3 | Quantify the Actuator Performance Index (API), for at least 3 [TBR] orbital regimes. | MO-4 |
| **Extended** | | |
| ESC-1 | Quantify Pointing Performance Index (PPI) to be PPI < (1 - ME) (i.e., pointing RMS error less than or equal to the classical PID controller) in at least 1 [TBR] orbital regime. | MO-3 |
| ESC-2 | Quantify Actuator Performance Index (API) to be API (1 - ME) (i.e., pointing RMS error less than or equal to the classical PID controller) in at least 1 [TBR] orbital regime. | MO-4 |

## 10. Mission Constraints / Key Drivers / Requirements

**Constraints:** Must be a CubeSat. Must communicate with TXST ground station.

**Drivers:** Low cost. Use of ML-based control.

| Number | Requirement | Rationale |
|---|---|---|
| SYS-1 | Spacecraft shall be designed to withstand the launch and on-orbit environments of the launch vehicle without failure that results in damage of launch vehicle and its contents or failure that causes injury to the ground handling crew. | Requirement of spacecraft safety from UNP |
| SYS-2 | The ADAPT system shall operate within a VLEO altitude range below 450km [TBR] for the mission duration of 360 days [TBR]. | System functionality in VLEO |
| SYS-3 | The ADAPT system shall have three-axis attitude control. | Attitude control needed to carry out experiment |

## 11. Mission Constraints / Key Drivers / Requirements (cont.)

| Number | Requirement | Rationale |
|---|---|---|
| SYS-4 | The ADAPT system shall execute a ML-based attitude control algorithm onboard. | Both controllers needed for head-to-head comparison |
| SYS-5 | The ADAPT system shall execute a classical PID attitude controller onboard. | Both controllers needed for head-to-head comparison |
| SYS-6 | The ADAPT system shall maintain attitude knowledge accuracy of ≤ [TBD] degree RMS (3-sigma) per axis. | Knowledge needed for PPI/API calculations |
| SYS-7 | The ADAPT system shall maintain positional knowledge within [TBD] meters (3-sigma). | Knowledge needed for PPI/API calculations |
| SYS-8 | The ADAPT system shall collect actuator usage data at a sampling rate of [TBD] Hz. | Knowledge needed for PPI/API calculations |
| SYS-9 | The ADAPT system shall be capable of updating on-board ML model parameters during mission. | Update functionality needed for ML-based controller |
| SYS-10 | The ADAPT system's payload and ADCS subsystems shall transition between operational modes within [TBD] ms of each other. | Switch time needed to minimize time without an active controller |

## 12. Experiment Plan

*(Section divider.)*

## 13. Payload Design

**Together:** Same sensor data & actuator suite. Different inputs from the KF pipeline. Exact Classical Controller TBD (contingent on manufacturer and AoA).

**COTS controller:** On flight computer. Reactive, linear (usually).

**ML controller:** On its own processor (NovoSpace GPU002SF card). Predictive, non-linear; adaptive. Takes actuator usage into consideration.

*Diagram: Sensors → State Est. / Kalman Filter. The filter sends the attitude error vector and angular velocity vector to the Classical Controller (error calc (target − current), P/I/D terms, command saturation), and the current and target state vector plus covariance matrix to the ML Neural Net (feature scaling, linear transform, activation functions, hidden layers). Classical raw commanded wheel torques and ML optimized predictive wheel torques both pass FSW checks before reaching the actuators.*

## 14. Orbit Simulation

| Criteria | GMAT | Basilisk | STK | Orekit |
|---|---|---|---|---|
| Coupled orbit/attitude dynamics | Low (attitude is decoupled) | Excellent (built for fully-coupled ADCS/GNC) | Moderate (requires Solis/ODTK) | Low (primarily orbit-focused) |
| VLEO atmosphere & drag modeling | Good (MSISE-90, JR) | Great (faceted/CAD drag, MSISE, custom) | Excellent (JB2008, MSIS, custom) | Excellent (JB2008, DTM, MSIS, DSST) |
| Component-level integration | Low (trajectory/hardware decoupled) | Excellent (modular FSW, reaction wheels, sensors, power) | Moderate (heavy UI/API wrapper) | Low (requires heavy custom Java coding) |
| Estimation (UKF) out of the box | Yes (filter capabilities exist, but only EKF native) | Yes (native UKF modules for attitude/orbit) | Yes (via ODTK, expensive) | Yes (EKF, ESKF, UKF, USKF) |
| RL pipeline suitability & speed | Poor (not built for parallelized step-loops; has GMAT Python API) | Excellent (C++ core, Python wrapper, native bsk_rl library) | Poor (slow API, closed-source licensing) | Poor (Java-Python overhead, slow step speeds) |
| Open source? / cost | Open source, free | Open source, free | Proprietary, expensive | Open source, free |

**Why Basilisk?** High-fidelity VLEO physics: custom atmospheric models. Built-in machine learning: bsk_rl library. Modular architecture: mirrors FSW for HIL testing. 100% code reusability: the exact same FSW algorithms written and tested in early simulations are compiled and run directly on in-flight hardware.

**Mission usage:** Pre-launch ground training. Benchmarking. Trajectory estimation (with integrated atmospheric models and geomagnetic/solar storm data). Data for mission life, phases, and structural/component lifetimes. HIL validation.

## 15. In-House Controller Pipeline

**Concept:** A Kalman/UKF estimator for state, density, and drag coefficient, feeding a learned control policy (compact with distilled pipeline built from teacher-student method). Combines classical estimation with learned nonlinear control. Control policy can further learn changing parameters on orbit.

*Four stages:*

- *Ground Training (the teacher model): high-fidelity simulation embedded in Basilisk (bsk_rl); privileged state space (perfect environmental data); SAC framework optimization.*
- *Knowledge Distillation (the student model): the teacher outputs soft targets; hardware compression into a compact network; hidden state inference, since the student is denied privileged data.*
- *Baseline Benchmarking (prelim V&V): adversarial evaluation (bias injected into the sim); legacy comparison against the classical COTS controller; success metrics define the SIL/HIL accuracy bar.*
- *Operations (PH-3): precision piloting with continuous attitude maneuvers; on-orbit adaptation via low-power updates and downlinking only small model stats.*

## 16. On-Board Data Generation

**PPI**

Required hardware: Sensors: Star Trackers (2+), Fine Sun Sensors (2–6), Inertial Measurement Units (IMUs), Microcontroller (2).

| Calculation | Source |
|---|---|
| Estimated Attitude = Current Orientation Quaternion | Star Tracker |
| Target Attitude = Commanded Target Quaternion | Microcontroller |
| Angular Velocity Error = Residual Body Rate | IMUs |
| Sensor Health = Binary Status Flags | Individual Sensor Health Checks |

**API**

Required hardware: COTS ADCS: Reaction Wheels (3+), Magnetorquers (3+), Magnetometer (1), Motor, Hall Effect Sensors.

| Calculation | Source |
|---|---|
| Electrical Power Draw = The Actuator Rail Current and Voltage | Internal motherboard telemetry sensors from COTS ADCS system |
| RMS Torque = Commanded torque | Microcontroller |
| Momentum Accumulation Rate = The wheel angular velocity or RPM | Motor's Hall Effect Sensors or Optical Encoders |
| Desaturation Frequency = RW desaturation (dumping momentum) | Reaction Wheels (RW) |

## 17. CONOPS

*(Section divider.)*

## 18. Ground CONOPS

*Flow diagram:*

- *Basilisk (coupled attitude–orbit physics, CAD-based drag; Kp, Ap, density and drag via bsk_rl) feeds privileged states to the Teacher Model (heavy SAC RL; reward minimises orbit tracking error and penalises power draw, actuator saturation and decay).*
- *Knowledge Distillation compares the student's predictions against the teacher's.*
- *A second Basilisk stage injects noise, latency and bias into the sensor data. This feeds Kalman Filtering and then the Student Model (SAC), which only sees imperfect, Kalman-filtered telemetry.*
- *Model post-processing quantizes the student and freezes its weights.*
- *A baseline loop compares the classical control algorithm (e.g. PID) with the ADAPT student policy (a quantized, distilled neural net) under a Basilisk Injection Node (unmodelled environmental bias / adversarial perturbations).*
- *A Phase Gate Evaluator (SIL/HIL accuracy bar) passes the result to Flight Configuration and Packaging, which deactivates the teacher model and links the student to the NVIDIA Jetson Orin Nano / NovoSpace payload computer.*

## 19. On-Board CONOPS

*Flow diagram:*

- *Data Generation ((proposed) sensors: star trackers, IMUs, GPS receiver, coarse sun sensor) sends noisy raw hardware telemetry to Data Cleaning & Estimation (KF internal processing: real-time position, velocity and aerodynamic variances).*
- *The estimator sends the attitude error vector to the Classic Controller (classic legacy attitude control methods; exact controller TBD), and the clean current and target state profiles plus covariance matrix to the ML-Controller (student model processing; implicitly identifies hidden parameters such as density and drag coefficient).*
- *ML target attitude quaternions and panel toggle requests go to the Safety Gate (FSW supervision: cross-checks commands against hard limits: angular velocity bounds, actuator saturation limits, thermal state, electrical power availability, predicted orbital decay).*
- *Validated commands go to Physical Actuation (reaction wheels and magnetorquers; possibly deployable solar arrays altering exposure to the velocity vector).*
- *If an update is needed, Ground Re-Training [TBD] runs: store telemetry packet, downlink via pass, Basilisk injection, teacher retraining, ground distillation, binary uplink patch, active FSW hot-swap.*

## 20. Mission Life CONOPS

Based on injection at 450 km:

- PH-0 duration: TBD
- PH-1 duration: ~1–2 weeks
- PH-2 & PH-3 duration (combined): ~50 km (full drag ~75–95 days, least drag ~215–240 days)
- PH-4 duration: ~100 km (full drag ~35–50 days, least drag ~80–90 days)
- PH-5 duration: none required (weather based, contingent on Storm Regime metrics)

**Total in-flight expectation:** ~132–388 days. **Total mission expectation:** TBD + (~132–388 days).

*Phase diagram:*

- *PH-0 Pre-Launch: train teacher model → train student model → establish baseline → testing & integration → system checks.*
- *PH-1 Launch & Commissioning: safe mode, detumble, system checks, deploy deployables.*
- *PH-2 Classical Baseline Characterization: classical controller has full ADCS authority.*
- *PH-3 ML Acceptance and Run-In: ML has partial ADCS authority.*
- *PH-4 Head-to-Head Duel: alternating classical and ML full ADCS authority, downlinking system telemetry and model state.*
- *PH-5 Storm Response.*
- *PH-6 Decay and Decommissioning: power systems off, deorbit. Multi-system failure [TBR] leads to deorbit from any phase.*

## 21. Orbit CONOPS

*Single-orbit sequence:*

1. *Command slew to 1st target to begin pointing.*
2. *ML or Classical Mode.*
3. *Commanded slew to next target.*
4. *Command slew to downlink/uplink (if uplinking this orbit).*
5. *Downlink telemetry & any log packets; uplink model updates/commands.*
6. *Update model (if ML Mode & updating this orbit). Eclipse is marked on the orbit.*

## 22. Modes

- *Safe Mode: most subsystems off; ADCS disabled; default boot mode on first boot and off-nominal reboots. Enters from Classical on a system anomaly. Leaves for Classical by automated first boot or when commanded.*
- *Classical Mode: classical COTS controller full execution; classic magnetorquer cross-product desaturation; downlinking telemetry data and model state.*
- *Experiment, ML Mode: ML full pointing prediction; ML full ADCS execution (PH-3 after prediction acceptance via ground uplink; PH-4 and after); downlink anomaly log packets; update via hot-swap. Alternates with Classical while in storm mode.*
- *Experiment, Storm Mode: prioritized alternation between ML and Classical mode; downlink controller telemetry; downlink density-product logs (daily). Entered from ML or Classical when a geomagnetic storm is imminent.*

## 23. Mission System

*(Section divider.)*

## 24. Spacecraft Overview

6U CubeSat, deployable solar arrays for power generation.

Dual-OBC structure to eliminate risk of issues with ML model affecting critical flight component functionality. Payload OBC powered off when not in use for power saving.

Critical subsystem considerations:

- **ADCS:** COTS system, ideally with direct control over actuators for both the classical controller and ML-based controller.
- **FSW:** Handles communication between primary OBC and payload OBC, what ADCS controller is active, and safety checks before executing ML model ADCS commands.
- **Payload:** Generates ADCS commands, model must be updatable.
- **COMM:** Dual UHF & S-Band radios for increased data throughput. S-Band included for now based on expected data throughput requirements but may be removed if unnecessary.

## 25. Spacecraft Layout

*CAD layout labelled:* XACT 50 ADCS, SOLO-EPS 4, EnduroSat OBC, GPU002SF, S-Band Transceiver, UHF Transceiver, UHF Antennas, S-Band Patch Antenna.

## 26. Link Budgets

*Screenshots:* UHF downlink, UHF uplink, S-Band downlink, S-Band uplink.

## 27. Power Draw

*Table of component power draw per power mode (see PDF).*

## 28. Power Generation

*(See PDF.)*

## 29. Power Balance

*Plot showing the system is power positive (see PDF).*

## 30. Data Budget

| | UHF radio | S-Band radio |
|---|---|---|
| Pass time | 10.00 minutes | 10.00 minutes |
| Passes per day | 8 | 8 |
| Radio data rate | 19,600 bps | 125,000 bps |
| Margin | 50 % | 50 % |
| Passes to downlink all data | 3 | 1 |
| Days to downlink all data | 0 | 0 |
| Total bytes generated (1 day) | 2,263,632 | 5,704,285 |
| Total bytes with margin | 3,395,447 (3,316 kB, 3 MB) | 8,556,428 (8,356 kB, 8 MB) |

## 31. Mass Budget

*(See PDF.)*

## 32. Subsystem Contribution

*(Section divider.)*

## 33. Attitude Determination and Control System (ADCS)

Develop spacecraft attitude determination, pointing, and control capabilities to meet mission performance requirements. Close relation to: Payload.

- **Key development areas:** Attitude determination and state estimation. Controller development and tuning. Pointing performance characterization.
- **Key deliverables:** Validated attitude control architecture. Pointing performance verification. Baseline controller performance dataset. ADCS simulation environment.

## 34. Communications System (COMMS)

Develop reliable command, telemetry, and data-transfer capabilities between the spacecraft and ground segment. Close relation to: Payload, CDH.

- **Key development areas:** Communications architecture and link analysis. Ground segment integration. Mission communications operations.
- **Key deliverables:** Communications budget. Telemetry and command definitions. Downlink strategy. Ground segment operations concept.

## 35. Command and Data Handling (CDH)

Develop the flight software and onboard computing infrastructure that manages spacecraft operations and mission data. Close relation to: Payload, ADCS.

- **Key development areas:** Flight software architecture. Data handling and telemetry management. Fault protection and mission operations logic.
- **Key deliverables:** Flight software architecture document. Mode transition diagram. Telemetry database. Fault management framework.

## 36. Electrical Power System (EPS)

Develop the spacecraft power generation, storage, and distribution system to support all mission operations. Close relation to: Payload, ADCS.

- **Key development areas:** Power budgeting and energy management. Solar array and battery sizing. Operational power planning.
- **Key deliverables:** End-to-end power budget. Energy balance analysis. Operational power modes. Battery management strategy.

## 37. Payload System

Develop and validate the onboard machine learning experiment, including algorithm execution, data collection, and performance assessment. Close relation to: ADCS.

- **Key development areas:** ML controller development and validation. Onboard model execution and experiment operations. Performance assessment and data analysis.
- **Key deliverables:** Flight-ready ML controller. Experiment operations concept. Performance evaluation framework. On-orbit experiment dataset.

## 38. Structures/Thermal System (STR/THERM)

Develop the mechanical and thermal systems that ensure spacecraft survivability and functionality in launch and space environments. Close relation to: Payload, all systems.

- **Key development areas:** Structural design and spacecraft packaging. Mass and mechanical analysis. Thermal control and environmental qualification.
- **Key deliverables:** Mechanical layout and CAD model. Mass budget. Structural analysis report. Thermal control plan.

## 39. Summary

- **Payload:** Develop and validate the ML controller.
- **ADCS:** Measure and optimize control performance.
- **CDH:** Manage software integration and mission operations.
- **Comms:** Return experiment data to the ground.
- **EPS and Structures/Thermal:** Keep the spacecraft powered, stable, and operational.

## 40. Conclusions

*(Section divider.)*

## 41. Key Takeaways

- The mission's primary goal is to demonstrate an AI-based attitude controller in low earth orbit.
- Success depends on collecting enough data to compare AI control against traditional control methods.
- Software integration and testing will be major development challenges.
- Safety and verification are essential before allowing AI-generated control actions.
- Most engineering effort will focus on integration, validation, and performance evaluation rather than new hardware development.

## 42. Thank you!
