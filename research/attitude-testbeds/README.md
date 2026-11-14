# Low-cost attitude testbeds at university CubeSat labs

Research for [#17](https://github.com/txst-space-lab/adapt-adcs/issues/17), on the [Physical Simulator map (#5)](https://github.com/txst-space-lab/adapt-adcs/issues/5). The question: what low-cost attitude testbeds do university CubeSat labs use, what does each one test, and what does it cost? The answer feeds the choice of which Physical Simulator builds come after the Reaction Wheel Demonstrator.

"Testbed" here means another lab's rig. Our own bench is the Physical Simulator (see `GLOSSARY.md`).

## Summary

| Rig | What it tests | Rough cost | Specialized equipment or skills | Fit for us |
|---|---|---|---|---|
| String / torsion suspension | One rotation axis (yaw): reaction wheel control, magnetorquer authority, detumble | Under $100 (our estimate: line, frame, clamp) | None. Balancing the mock-up under the line | Already the Reaction Wheel Demonstrator's test |
| Trifilar / bifilar pendulum | Moment of inertia of the mock-up | Low (three lines, a 3D-printed platform, a camera) ([UCD open hardware](https://certification.oshwa.org/ie000005.html)) | Camera and video tracking | Cheap, and gives the Digital Simulation real inertia values |
| Single-axis turntable (mechanical bearing) | One axis; sun sensor angle calibration; rate checks when motorized | Low (aluminum, a thrust bearing, an encoder, an Arduino) | Basic machining | Good for sensor calibration. Too much friction for magnetorquers |
| Helmholtz cage | Magnetometer calibration, magnetorquer and desaturation tests, B-dot detumble | DIY target under $5,000 ([UC CubeCats](https://uccubecats.github.io/HelmholtzCage.html)); COTS cage €8,000 ([Bologna](https://doi.org/10.3390/aerospace7030031)) | Coil winding, current drivers, a reference magnetometer | High. OreSat publishes an open design |
| Sun simulator | Sun sensor calibration, sun-pointing modes | €3,500 for a collimated LED unit ([Bologna](https://doi.org/10.3390/aerospace7030031)); much less for an LED on a stepper arm ([Concepción](https://repositorio.udec.cl/handle/11594/11881)) | Optics (Fresnel lens) for collimation | Medium. Pairs with the turntable |
| 3-axis spherical air bearing | Full 3-axis attitude control | €50–100 in materials for an open 3D-printed 1U bearing ([Jovanovic et al.](https://jossonline.com/storage/2021/08/Final-Jovanovic-Design-and-Testing-of-Low-Cost-Open-Source-3-D-Printed-Air-Bearing-Based-Attitude-Simulator-for-CubeSat-Satellites.pdf)), plus a compressor; €7,500 for a stand with air bearing ([Bologna](https://doi.org/10.3390/aerospace7030031)) | Compressed air, mass balancing, precision surfaces | Stretch goal |
| Suspended universal joint | 3-axis motion over limited angles | Low (laser-cut acrylic, 3D-printed mounts, COTS parts) ([Texas A&M](https://peer.asee.org/25582.pdf)) | A stable overhead mount | A cheaper alternative to the air bearing |
| Planar air bearing | Translation and one rotation axis on a flat table ([Schwartz et al.](https://doi.org/10.2514/2.5085)) | Not costed here | A large, very flat table or floor | Not relevant: ADAPT does no formation flying |
| Sensor-stimulus box | Sun sensor and magnetometer inputs without motion | Low (LEDs, magnetometers, an Arduino, a wooden box) ([CMU](https://www.ri.cmu.edu/app/uploads/2022/08/ri_thesis_template.pdf)) | None | High. Closest to linking the Digital Simulation to hardware |

Costs are in the currency of the source and the year of publication. "Our estimate" marks a figure no source gives.

## What ADAPT needs to exercise

ADAPT plans to fly star trackers (2+), fine sun sensors (2–6), IMUs, reaction wheels (3+), magnetorquers (3+) and a magnetometer (`docs/mission/overview/adapt-detail-overview-115.md`, slide 16). The spacecraft layout shows a COTS ADCS, the XACT 50 (slide 25), and the 2026-11-06 ADCS team sync expects the mission to buy a precise COTS ADCS and star tracker (`docs/mission/meetings/2026-11-06-adcs-team-sync.md`). So the Physical Simulator mostly needs to exercise our software against cheap stand-ins for each sensor and actuator, not to qualify flight units.

## String and torsion suspension

**How it works.** The mock-up hangs from a single line and rotates freely about the vertical. The line adds a small restoring torque as it twists.

**What it tests.** One axis of attitude control. The University of Bologna team calls wire suspension "perhaps the simplest" type of dynamic hardware-in-the-loop facility ([Modenini et al. 2020](https://doi.org/10.3390/aerospace7030031), section 1). It is the test OreSat ran with its reaction wheels, recorded in our 2026-11-06 sync notes.

**Published example: CU Boulder's HelCaTS.** CU Boulder's QB50 team hung a fully integrated 3U CubeSat from braided nylon line inside a Helmholtz cage to verify its magnetorquers ([Hong et al., "Satellite Testbed for Attitude Response"](https://www.colorado.edu/aerospace/sites/default/files/attached-files/star_aiaa.pdf), section V):

- They chose string after finding that "even high quality mechanical bearings proved to have too much resistance", and that air-bearing disturbances were too large for magnetorquer torques of about 5 × 10⁻⁶ N·m.
- Their requirement was a resistive torque below 2.5 × 10⁻⁵ N·m, so that the magnetorquer authority is at least 20% of it.
- Air drag on the rotating satellite was negligible (modeled at 5.4 × 10⁻¹⁴ N·m).
- A center of gravity 1 cm off the line tilts a 3.6 kg satellite about 11°. This costs only about 4% of the magnetorquer torque.
- They report that MIT used the same string approach.

**Cost and skills.** Minimal: an aluminum-extrusion frame, braided line and a clamp. The skill is balancing the mock-up so that it hangs level.

**Limits.** One rotation axis only. The line's twist adds a spring torque that the controller sees as a disturbance. Pendulum swing couples into the motion when the mock-up is pushed.

**Related: measuring inertia with a trifilar pendulum.** The same hardware, three lines instead of one, measures the mock-up's moment of inertia from its oscillation period. University College Dublin, home of the EIRSAT-1 CubeSat, publishes a trifilar pendulum with video motion tracking for "pocket cubes, CubeSats, and drones" as certified open hardware under CC-BY-4.0 ([OSHWA IE000005](https://certification.oshwa.org/ie000005.html)). Measured inertia lets the Digital Simulation model the demonstrator rather than a guess.

## Single-axis turntables

**How it works.** The mock-up sits on a platform turning on a bearing about a vertical axis. The bearing is mechanical, or a rotary air bearing.

**What it tests.** One axis of reaction wheel control with a heavier payload than string allows. Motorized with an encoder, it becomes a calibration stage for sun sensors, and a known-rate input for gyros.

**Published examples.**

- **Dalhousie University (2019).** A senior design team put a reaction-wheel CubeSat mock-up on a low-friction bearing inside a single-axis Helmholtz coil, and the mock-up controlled its attitude from the coil's field. Their future work lists air or ceramic bearings, noting that "bearings with sufficiently low friction may permit the use of magnetorquers in the cage" ([poster](https://cdn.dal.ca/content/dam/dalhousie/pdf/engineering/decc-posters/2019/mech19_cubesataltitudecontrolposter.pdf)).
- **CU Boulder sun sensor turntable.** A motorized, computer-controlled turntable on a ball-thrust bearing, built in-house, measures its angle to within 0.5° to calibrate sun sensors on all six faces of a CubeSat ([Hong et al.](https://www.colorado.edu/aerospace/sites/default/files/attached-files/star_aiaa.pdf), section IV).
- **Rotary air bearings.** Physik Instrumente stated that a small rotary air bearing starts under $3,000 ([quoted in Jovanovic et al. 2019](https://jossonline.com/storage/2021/08/Final-Jovanovic-Design-and-Testing-of-Low-Cost-Open-Source-3-D-Printed-Air-Bearing-Based-Attitude-Simulator-for-CubeSat-Satellites.pdf), p. 876).

**Cost and skills.** Low for a mechanical turntable: shop machining, a bearing, a stepper or encoder and a microcontroller. A rotary air bearing adds a compressor and a few thousand dollars.

**Limits.** Mechanical bearing friction is fine for reaction wheels and sensor calibration but swamps magnetorquer torques (CU Boulder and Dalhousie above).

## Spherical and planar air bearings

**How it works.** A spherical air bearing floats a hemisphere on a thin air film, giving free yaw and limited roll and pitch. A planar air bearing floats a puck on a flat table, giving two translations and one rotation ([Schwartz, Peck & Hall 2003](https://doi.org/10.2514/2.5085), the standard historical review).

**What it tests.** Full 3-axis attitude control with near-frictionless dynamics. This is the closest a ground lab gets to free-floating attitude dynamics.

**Published examples.**

- **Open source, 3D-printed (Aalto University / Michigan Tech, 2019).** A 3D-printed air bearing for a complete 1U CubeSat ([Jovanovic, Pearce & Praks, *Journal of Small Satellites* 8(2)](https://jossonline.com/storage/2021/08/Final-Jovanovic-Design-and-Testing-of-Low-Cost-Open-Source-3-D-Printed-Air-Bearing-Based-Attitude-Simulator-for-CubeSat-Satellites.pdf)):
  - Design files are OpenSCAD sources, STLs and Python tracking scripts under GPL-3.0 ([OSF project k5zb8](https://osf.io/k5zb8/)).
  - Material cost is €50–100: about 550 g of PLA, hoses and fittings. Printing takes about 115 printer-hours, and sourcing, finishing and assembly take under 30 hours of labor.
  - It runs at 2–4 bar supply pressure.
  - Directional air nozzles compensate disturbance torques of about 0.001 N·m, but the authors could not cancel gravity torque with the design.
  - The rotor encloses a 1U CubeSat. Scaling up to larger CubeSats increases the platform's parasitic inertia and the supply pressure needed, which the authors flag as a safety concern (p. 876).
- **University of Bologna (2020).** A custom table-top air bearing with automatic balancing by shifting masses on linear motors, inside a COTS Helmholtz cage. It hosts mock-ups up to 6U ([Modenini et al.](https://doi.org/10.3390/aerospace7030031)):
  - The articulated stand with air bearing cost €7,500, and the balancing system €500.
  - Residual disturbance torque after balancing is under 5 × 10⁻⁵ N·m.
  - The authors note air-bearing facilities are heavy "due to balancing weights required to minimize the disturbance torque due to gravity".
- **Virginia Tech CSACS (2018).** A low-cost, lightweight air bearing built from COTS parts, with a reaction wheel array and a planned 1.5 m Helmholtz cage ([Wolosik thesis](https://vtechworks.lib.vt.edu/bitstreams/be8b9212-ac85-4548-a9af-ee9908d666ac/download)).
- **Texas A&M LASR Attitude Test-bed (2016).** Replaces the spherical air bearing with a suspended, neutrally stable universal joint, avoiding "the complexity and cost of spherical air-bearing systems" and the need for compressed air ([Probe, Goecks & Hurtado, ASEE 2016](https://peer.asee.org/25582.pdf)):
  - The base is laser-cut acrylic, with 3D-printed wheel mounts.
  - Three DC-motor reaction wheels with brass rotors are mounted so that their axes are mutually perpendicular.
  - A Raspberry Pi B+ drives a custom motor board (needed for the motors' high starting current), and a MEMS IMU with a magnetometer provides sensing.
  - The test-bed is driven from MATLAB.
  - It is the closest published relative of our Reaction Wheel Demonstrator.

**Cost and skills.** Compressed air, careful surface finishing, and above all mass balancing: the center of mass must sit at or just below the bearing's center of rotation, or gravity torque dominates. Planar air bearings also need a large, very flat floor.

**Limits.** Balancing a 6U-sized mock-up is hard. The open 3D-printed bearing is sized for 1U. Planar bearings test translation (rendezvous, formation flying), which ADAPT does not do.

## Helmholtz cages

**How it works.** Three orthogonal coil pairs cancel Earth's field and impose a commanded field vector. A reference magnetometer closes the loop.

**What it tests.** Magnetometer calibration, magnetorquer polarity and authority, desaturation logic, and B-dot detumble. With a time-varying field fed from the Digital Simulation, the cage replays the field along an orbit.

**Published examples.**

- **OreSat (Portland State), open source.** Our reference project already has a cage ([oresat-helmholtz](https://github.com/oresat/oresat-helmholtz)):
  - The repo holds construction documents, a Python command-line controller and KiCad controller boards, licensed GPL-3.0-or-later and CERN-OHL-S-2.0 ([LICENSES](https://github.com/oresat/oresat-helmholtz/tree/main/LICENSES)).
  - Version 2 runs on a Raspberry Pi Pico, with a motor driver per coil pair and an INA226 current monitor.
  - The coils have 58 turns and run at up to 4.92 A and 15.2 V ([SOP](https://github.com/oresat/oresat-helmholtz/blob/main/docs/SOP.md)).
- **UC CubeCats (University of Cincinnati).** Built to a cost target under $5,000 with Ohio Space Grant funding ([project page](https://uccubecats.github.io/HelmholtzCage.html)):
  - The coils are square, 7.56–8.00 ft on a side, on aluminum U-channel.
  - The uniform volume is 30 cm on a side, enough for a 3U in any orientation.
  - The cage reaches ±1.5 G at 10 A, driven by three HP 6038A supplies over GPIB.
  - A Python GUI with a truth magnetometer runs it.
  - The team's 2019 SmallSat poster is titled "An Open-Source Helmholtz Cage Design with Air-Bearing Table for 1U and 3U CubeSat Testing" ([USU](https://digitalcommons.usu.edu/smallsat/2019/all2019/12)).
- **Princeton TigerSats (2023).** A miniaturized 3-axis cage for 1U–2U with under 1% field deviation for 1U, citing low material cost and documented coil design ([SmallSat 2023](https://digitalcommons.usu.edu/smallsat/2023/all2023/252)).
- **CMU (2022).** A single-axis Helmholtz pair cut from wood on a laser cutter, radius 20 cm and 300 turns, designed for 125 µT. It only cancels Earth's field along one axis; the author names a 3-axis cage as future work ([Jensen thesis](https://www.ri.cmu.edu/app/uploads/2022/08/ri_thesis_template.pdf), section 4.2.1).
- **University of Bologna.** A COTS three-axis cage (1.3 m coils, ±10 G) cost €8,000 with control ([Modenini et al.](https://doi.org/10.3390/aerospace7030031), Table 6).

**Our lab once had one.** Asked about the CubeSat case a senior design team printed about three years ago, Evan asked whether that meant "the Helmholtz cage that we once had" (Kelsey Morris's casing update for [#11](https://github.com/txst-space-lab/adapt-adcs/issues/11), 2026-11-13). Where that cage and its design files are now is unknown. Kelsey pointed to the UC CubeCats cage above as an example. If the old cage or its files turn up, they are a head start on build 2 below.

**Cost and skills.** DIY cages run from a few hundred dollars (single axis, wood frame) to a few thousand (3-axis, room-sized, bench supplies). The skills are coil winding, current control and field mapping. The coils get hot at high current, so the cage needs a supervised operating procedure (see the OreSat SOP).

**Limits.** Nearby steel and electronics distort the field, and the uniform volume is a fraction of the cage size. A 6U mock-up needs a cage on the UC CubeCats scale.

## Sun simulators

**How it works.** A bright, ideally collimated lamp stands in for the Sun, and the sensor or mock-up is turned relative to it.

**What it tests.** Sun sensor calibration and sun-pointing or sun-acquisition modes.

**Published examples.**

- **University of Bologna.** A COTS 300 W LED studio light with a custom 400 mm Fresnel collimating lens, for €3,500 ([Modenini et al.](https://doi.org/10.3390/aerospace7030031), section 4 and Table 6):
  - Beam divergence is under 0.5° over a 35 cm diameter. The Sun's apparent diameter is 0.53°, which is the collimation target for sun sensors.
  - LEDs lack IR and UV output. This doesn't matter for the CMOS, CCD and PSD sensors most nanosatellite sun sensors use.
- **Universidad de Concepción (2024).** An undergraduate thesis built in one semester: full-spectrum 10 W LEDs switched by relays from an Arduino, moved by stepper motors. It notes that traditional ADCS testbeds with a sun simulator, field simulator and air bearing "often incur costs exceeding $20,000 USD" ([thesis record](https://repositorio.udec.cl/handle/11594/11881)).
- **OreSat solar simulator.** LED and halogen lamps driven by a Raspberry Pi Pico, with KiCad boards and Onshape mechanicals. It is meant for solar panel testing rather than attitude, but it is open and reusable ([oresat-solar-simulator-hardware](https://github.com/oresat/oresat-solar-simulator-hardware)).

**Cost and skills.** From tens of dollars (an LED on a stepper arm) to a few thousand (collimated). Collimation needs basic optics.

**Limits.** A small LED source is not collimated, so the angle a sun sensor reports depends on distance. Good sun-sensor work wants the lamp far away or collimated.

## Other common pieces

- **Sensor-stimulus box.** CMU placed a 1U CubeSat in a laser-cut box with an LED and a LIS3MDL magnetometer on each face, read by an Arduino through an I²C multiplexer, inside a Helmholtz coil ([Jensen 2022](https://www.ri.cmu.edu/app/uploads/2022/08/ri_thesis_template.pdf), section 4.2). It drives the sun sensors and measures magnetorquer output without any motion, and feeds the results back into the simulator.
- **Simulated-sensor interface board.** CU Boulder fed MATLAB-generated sensor data over I²C into the ADCS processor and measured magnetorquer PWM and power draw ([Hong et al.](https://www.colorado.edu/aerospace/sites/default/files/attached-files/star_aiaa.pdf), section III). This is pure hardware-in-the-loop: no motion and no field, just the flight computer in the loop with the Digital Simulation.
- **Ground-truth attitude.** Bologna built a monocular camera system for €500 that measures attitude to 10 arcmin rms ([Modenini et al.](https://doi.org/10.3390/aerospace7030031)). Texas A&M used a VICON motion-capture system ([Probe et al.](https://peer.asee.org/25582.pdf)). A camera and printed markers are a cheap way to score bench runs independently of the onboard IMU.
- **Star field.** Covered by a separate ticket on the map. Bologna lists a starry-sky simulator as future work.

## Suggested order of builds after the Reaction Wheel Demonstrator

This is a recommendation for the map, not a decision.

1. **Instrument the string rig.** Add a camera with markers for ground-truth yaw, and hang the mock-up from three lines to measure its inertia (trifilar). Cost: nearly zero. It turns the string-push test into scored runs and gives the Digital Simulation a measured inertia.
2. **Helmholtz cage around the string rig.** Start from OreSat's open design, sized for a 6U mock-up. Hanging the mock-up inside the cage is exactly CU Boulder's magnetorquer setup, and it lets us test magnetometer calibration, lab-built magnetorquers and desaturation of the demonstrator's wheels. If the budget is tight, start with a single-axis pair as CMU did.
3. **Motorized single-axis turntable with a lamp.** A stepper and encoder platform (CU Boulder, Concepción) gives sun sensor angle calibration and known rates for IMU checks. It's cheap and reuses skills from steps 1–2.
4. **Sensor-stimulus and interface-board hardware-in-the-loop.** CMU- or CU-Boulder-style, so that the Digital Simulation can drive real sensors and read real actuators without motion. This is the bridge named in the map's "Linking the Digital Simulation to the hardware" item.
5. **Stretch: 3-axis motion.** Choose between the Texas A&M suspended universal joint (cheaper, no compressor) and an air bearing. The open 3D-printed bearing is 1U-sized, and a 6U-class mock-up needs a stand like Bologna's (€7,500 plus €500 balancing) or a scaled print. Defer until steps 1–4 have shown which gaps remain.

## Sources

- Modenini, Bahu, Curzi, Togni. "A Dynamic Testbed for Nanosatellites Attitude Verification." *Aerospace* 7(3):31, 2020. https://doi.org/10.3390/aerospace7030031 ([PDF](https://cris.unibo.it/retrieve/e1dcb337-2705-7715-e053-1705fe0a6cc9/aerospace-07-00031%20%281%29.pdf))
- Jovanovic, Pearce, Praks. "Design and Testing of a Low-Cost, Open Source, 3-D Printed Air-Bearing-Based Attitude Simulator for CubeSat Satellites." *Journal of Small Satellites* 8(2):859–880, 2019. [PDF](https://jossonline.com/storage/2021/08/Final-Jovanovic-Design-and-Testing-of-Low-Cost-Open-Source-3-D-Printed-Air-Bearing-Based-Attitude-Simulator-for-CubeSat-Satellites.pdf); design files at [OSF k5zb8](https://osf.io/k5zb8/)
- Schwartz, Peck, Hall. "Historical Review of Air-Bearing Spacecraft Simulators." *Journal of Guidance, Control, and Dynamics* 26(4), 2003. https://doi.org/10.2514/2.5085
- Probe, Goecks, Hurtado. "Low-Cost Satellite Attitude Hardware Test Bed." ASEE Annual Conference, 2016. https://peer.asee.org/25582.pdf
- Hong et al. "Satellite Testbed for Attitude Response." University of Colorado Boulder, AIAA paper. https://www.colorado.edu/aerospace/sites/default/files/attached-files/star_aiaa.pdf
- Jensen. "A Low-Cost Attitude Determination and Control System and Hardware-in-the-Loop Testbed for CubeSats." CMU-RI-TR-22-43, 2022. https://www.ri.cmu.edu/app/uploads/2022/08/ri_thesis_template.pdf
- Wolosik. "Advancements in the Design and Development of CubeSat Attitude Determination and Control Testing at the Virginia Tech Space Systems Simulation Laboratory." MS thesis, Virginia Tech, 2018. https://vtechworks.lib.vt.edu/bitstreams/be8b9212-ac85-4548-a9af-ee9908d666ac/download
- Dalhousie University Mechanical Engineering Team 19. "CubeSat Satellite Attitude Control Testbed." Design poster, 2019. https://cdn.dal.ca/content/dam/dalhousie/pdf/engineering/decc-posters/2019/mech19_cubesataltitudecontrolposter.pdf
- UC CubeCats. "Helmholtz Cage." https://uccubecats.github.io/HelmholtzCage.html
- OreSat. `oresat-helmholtz`. https://github.com/oresat/oresat-helmholtz
- OreSat. `oresat-solar-simulator-hardware`. https://github.com/oresat/oresat-solar-simulator-hardware
- Singh, Galvin. "A Low-Cost, Miniaturized, Homogeneity-Optimized Helmholtz Cage for CubeSat Attitude Control Ground Testing." SmallSat 2023. https://digitalcommons.usu.edu/smallsat/2023/all2023/252
- Verbryke et al. "An Open-Source Helmholtz Cage Design with Air-Bearing Table for 1U and 3U CubeSat Testing." SmallSat 2019. https://digitalcommons.usu.edu/smallsat/2019/all2019/12
- Maricahuin Monsalves. "Construcción y validación de un simulador solar para verificación de algoritmo de actitud en Cubesats en órbitas LEO." Undergraduate thesis, Universidad de Concepción, 2024. https://repositorio.udec.cl/handle/11594/11881
- OSHWA certification IE000005, "Trifilar Pendulum - Mass Moment of Inertia Measurement," University College Dublin. https://certification.oshwa.org/ie000005.html
