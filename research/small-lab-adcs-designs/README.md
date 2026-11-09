# Open-source and published ADCS designs a small lab can build

Research for [#18](https://github.com/txst-space-lab/adapt-adcs/issues/18), on the [Physical Simulator map (#5)](https://github.com/txst-space-lab/adapt-adcs/issues/5). Gathered 2026-11-08.

**Question.** What other open-source ADCS projects and published papers describe ADCS hardware and software that can be built in a small lab with minimal specialized equipment?

OreSat / Portland State has its own ticket ([#13](https://github.com/txst-space-lab/adapt-adcs/issues/13)) and appears here only for comparison.

## Short answer

- **No open-source flight reaction wheel is ready to copy.** Every open reaction wheel found is either a ground test article (OreSat's own wheel test article uses "inexpensive COTS DC brushless motors" much stronger than flight needs) or a student prototype. The most useful references for the Reaction Wheel Demonstrator are the **ground** projects: reaction-wheel balancing cubes and the [SimpleFOC](https://github.com/simplefoc/Arduino-FOC) motor-control library, which drive cheap gimbal or drone brushless motors.
- **The closest published model for our Physical Simulator** is a 2025 reaction wheel hardware-in-the-loop paper from Florida Tech ([arXiv:2508.19164](https://arxiv.org/html/2508.19164v1)): real wheels in the loop, spacecraft dynamics simulated. Its lessons (a current deadband, Hall-sensor noise near zero speed, commanding speed instead of torque) apply directly to drone-motor wheels.
- **For magnetorquers, sun sensors and the whole small-lab approach,** the best single source is a 2022 CMU thesis ([Jensen](https://www.ri.cmu.edu/app/uploads/2022/08/ri_thesis_template.pdf)). It builds a magnetorquer-only ADCS on the open PyCubed board, plus a laser-cut wooden test box with an LED and magnetometer on each face, set inside a home-wound Helmholtz coil.
- **The flight software is already in the lab's ecosystem.** The [PROVES Kit F Prime reference](https://github.com/Open-Source-Space-Foundation/proves-core-reference) (Apache-2.0, which TXST Space Lab already forks) has an F Prime `DetumbleManager` (B-dot and hysteresis control of magnetorquer coils), an `ADCS` component that reads light sensors for sun sensing, and an `ImuManager`. No open F Prime reaction wheel component turned up, so that one is ours to write.
- **Flight-proven open hardware exists for everything except reaction wheels:** PCB-embedded magnetorquer coils (PROVES solar boards, CERN-OHL-W-2.0, flown), an analog sun sensor (Foresail-1 PSS, CERN-OHL-2.0), and star-tracker solvers (OpenStarTracker, MIT; tetra3, Apache-2.0).
- **UPSat's lesson is the strongest argument for a Physical Simulator.** UPSat was the first fully open-source satellite and flew a small reaction wheel ("spin torquer") with magnetorquers. Its open ADCS did not point correctly in orbit, and its own team gave the reason: time pressure meant the test campaign never covered the control algorithm ([LSF talk, 2019](https://events.libre.space/event/3/contributions/64/)).

## How to read the entries

Each entry gives: **what** it is, **licence**, **maturity** (flown or not), **equipment** needed to build and test it, and **lesson** for the Reaction Wheel Demonstrator (RWD) and the Physical Simulator. "No licence file" means all rights are reserved by default: read the design and learn from it, but don't copy files into our repos.

## 1. Reaction wheels

### OreSat reaction wheels (Portland State) — comparison only

- **What:** The 2016 paper "An Open-Source Reaction Wheel System for Oregon's First Satellite" ([ResearchGate](https://www.researchgate.net/publication/308007209_An_Open-Source_Reaction_Wheel_System_for_Oregon's_First_Satellite)) and the [oresat/reaction-wheels](https://github.com/oresat/reaction-wheels) repo, with a Hall-sensor BLDC controller, Python code and 1U SolidWorks CAD. The later controller board is [oresat-adcs-hardware](https://github.com/oresat/oresat-adcs-hardware) ("Controller PCB for OreSat reaction wheels, magnetorquers, IMU, and magnetometers").
- **Licence:** reaction-wheels has no licence file; oresat-adcs-hardware is GPL-2.0; [oresat-adcs-software](https://github.com/oresat/oresat-adcs-software) is GPL-3.0.
- **Lesson:** Covered in #13. It sets the pattern of a ground test article built from cheap, oversized brushless motors.

### Reaction-wheel balancing cubes (Cubli and open-source copies)

- **What:** ETH Zurich's Cubli is a 15 cm cube with three reaction wheels that balances on a corner, a "reaction wheel based 3D inverted pendulum" ([Gajamohan et al., ECC 2013](https://skoge.folk.ntnu.no/prost/proceedings/ecc-2013/data/papers/0829.pdf); first prototype in [IROS 2012](https://bvbvf1.bib-bvb.de/vufind/PrimoRecord/cdi_ieee_primary_6385896)). An open copy is [willem-pennings/balancing-cube](https://github.com/willem-pennings/balancing-cube): Fusion 360 mechanical parts, KiCad boards, Arduino code on an ESP32, three integrated reaction wheels.
- **Licence:** balancing-cube is GPL-3.0. Cubli is a paper, not an open design.
- **Maturity:** Ground only; these are robots, not spacecraft parts.
- **Equipment:** balancing-cube uses Maxon motors and Maxon ESCON drivers tuned in Escon Studio, plus ordered PCBs and machined parts. That is costlier than drone motors. Its README warns about trapped fingers between spinning flywheels.
- **Lesson for the RWD:** This is the nearest open mechanical analogue to a cube with three orthogonal wheels driven from one microcontroller. Its wheel guard warning applies to our string test too. The physics is harder than ours (balancing against gravity), so a string-hung mock-up is a gentler first step.

### SimpleFOC motor control (library, not a satellite part)

- **What:** [SimpleFOC / Arduino-FOC](https://github.com/simplefoc/Arduino-FOC), a field-oriented control library for brushless and stepper motors on Arduino-class boards, published in [JOSS (2022)](https://www.theoj.org/joss-papers/joss.04232/10.21105.joss.04232.pdf). There is an official [gimbal-motor balancing robot](https://github.com/simplefoc/arduino-foc-balancer).
- **Licence:** MIT (library and balancer).
- **Maturity:** Widely used on the ground. Not flight software.
- **Equipment:** A cheap gimbal or drone brushless motor, a SimpleFOC-class driver board, an Arduino or ESP32, and ideally a magnetic angle encoder.
- **Lesson for the RWD:** Drone ESCs are built to command throttle in one direction. A reaction wheel needs smooth, reversible torque or speed control through zero. FOC with an encoder gives that, which is why reaction-wheel hobby builds use it. Weigh this in the motor choice (#12): a gimbal motor driven by SimpleFOC may beat a drone motor with a stock ESC.

### Florida Tech reaction wheel hardware-in-the-loop (paper, 2025)

- **What:** "Real-time Testing of Satellite Attitude Control With a Reaction Wheel Hardware-In-the-Loop Platform" ([arXiv:2508.19164](https://arxiv.org/html/2508.19164v1)). Four Maxon EC 60 flat brushless motors stand in for reaction wheels. A Jetson Nano runs the controller over ROS2, and MATLAB/Simulink simulates the spacecraft. Measured wheel speeds feed the simulated equations of motion.
- **Licence:** Paper only; no code released.
- **Maturity:** Lab platform.
- **Equipment:** Maxon motors and EPOS4 drivers on CAN, a Jetson Nano and a Linux PC. No air bearing: the spacecraft is simulated.
- **Lessons for the RWD and the Physical Simulator:**
  - The wheels had a current deadband of about 300 mA, so small torque commands did nothing. The team switched to velocity commands.
  - Hall-sensor noise near zero speed, differentiated to estimate acceleration, added phase lag that destabilized the loop. The team biased the wheels to ±100 rad/s.
  - Torques that worked in simulation exceeded the hardware's limits, and controller gains had to come down.
  - This is the "real wheel, simulated spacecraft" architecture. It is one concrete answer to the map's open question on linking the Digital Simulation to the hardware.

### Student reaction wheel repos

| Project | What | Licence | Lesson |
|---|---|---|---|
| [UWCubeSat/hs2-reaction-wheels](https://github.com/UWCubeSat/hs2-reaction-wheels) | Driver code for one Faulhaber motor: DRV10970 brushless driver, BNO055 IMU, PID speed loop, PWM | MIT | A small, readable split: driver, IMU, speed loop. A good shape for the RWD firmware. |
| [yiqiangjizhang/CubeSat-Reaction-Wheel-control](https://github.com/yiqiangjizhang/CubeSat-Reaction-Wheel-control) (UPC PLATHON BSc thesis) | 1-axis control of a 1U CubeSat, with 3D-printed wheel parts, tested on an air bearing inside a magnetic field simulator, with Bluetooth to a ground PC and a live 3D display | No licence file | Shows a one-axis bench first, with a live attitude display as a debugging aid. |
| [dylanballback/CubeSat_Attitude_Control](https://github.com/dylanballback/CubeSat_Attitude_Control) | A planned 1U testbed: 3 BLDC wheels, IMU, Arduino and Raspberry Pi Zero in a 3D-printed cube, held in 3D-printed gimbal rings, building up from 1 to 3 axes | No licence file; milestones are not marked done | A 3D-printed gimbal is a cheap alternative to string. Its staged build plan (1 axis, then 2, then 3) suits the RWD. |

## 2. Magnetorquers

### PROVES Kit PCB-embedded coils (Bronco Space / Open Source Space Foundation)

- **What:** Each PROVES Kit [solar board](https://github.com/proveskit/solar_boards) carries a TI DRV2605 haptic motor driver. The [hardware docs](https://github.com/proveskit/Documentation/blob/main/docs/core_documentation/hardware/XY_solar_board.md) say it energizes "the magnetorquer coils inside the PCB": two coils drawn into the 4-layer board. The current is small, "but it is still enough to create enough of a magnetic field for detumbling."
- **Licence:** CERN-OHL-W-2.0 (solar_boards).
- **Maturity:** The solar boards list flight heritage on Pleiades-Yearling and Pleiades-Squared ([README](https://github.com/proveskit/solar_boards)).
- **Equipment:** Only an ordered 4-layer PCB. No winding.
- **Lesson:** This is the cheapest magnetorquer in the survey, and the lab already works with this kit. It is good enough for a desaturation demo in a Helmholtz cage, not for flight-grade dipole.

### CMU magnetorquer-only ADCS on PyCubed (Jensen, 2022)

- **What:** [A Low-Cost Attitude Determination and Control System and Hardware-in-the-Loop Testbed for CubeSats](https://www.ri.cmu.edu/app/uploads/2022/08/ri_thesis_template.pdf) (CMU-RI-TR-22-43). A 1U CubeSat on PyCubed with a BMX160 IMU (gyroscope and magnetometer), six sun sensors, and one magnetic torque coil on each outer panel. Flight software: detumble, magnetometer calibration, sun sensor calibration, a second detumble, then an MEKF.
- **Licence:** The thesis describes the hardware-in-the-loop simulator as open source. PyCubed hardware is CC BY-SA 4.0 and its software is MIT ([pycubed/hardware](https://github.com/pycubed/hardware)).
- **Maturity:** Lab-tested. PyCubed itself has flight heritage ([SmallSat 2019](https://digitalcommons.usu.edu/smallsat/2019/all2019/65)).
- **Equipment:** A test box of laser-cut wood (acrylic works too) with 3D-printed mounts. Each face has an LED and a LIS3MDL magnetometer, read by an Arduino Uno through an I²C multiplexer. The box sits in a Helmholtz coil laser-cut from wood, with radius 20 cm and 300 turns of magnet wire per coil, sized to null or replace Earth's field.
- **Lessons for the Physical Simulator:**
  - A bench rig doesn't have to move. The box tests sun sensing (switch LEDs on) and magnetorquers (measure the field the coils make) with the spacecraft standing still. It feeds the measured torquer fields back into the simulator dynamics.
  - The thesis notes the rig cannot test the gyroscope. That is the trade it makes for being small and simple.
  - A wooden Helmholtz coil built from magnet wire is within reach of a small lab. It is the cheap version of the Helmholtz cage listed under "What to build after the Reaction Wheel Demonstrator".

### Pico-torQuer

- **What:** "An open-source PocketQube magnetorquer", with software at [gitlab.com/deepspace-engineering/pico-torquer](https://gitlab.com/deepspace-engineering/pico-torquer) ([topic listing](https://gitlab.com/explore/projects/topics/PocketQube)).
- **Licence and maturity:** Not checked. Last updated April 2023.
- **Lesson:** Another small, PCB-scale design to compare if we ever build torquers.

## 3. Sun sensors

### Foresail-1 PSS analog sun sensor (Aalto University)

- **What:** A panel-mounted analog sun sensor: 24 × 16 × 6 mm, 4 g, about 100° field of view, ±1° accuracy, 0.13° resolution, up to 100 Hz, over I²C or RS-485 ([fs1_psd_sun_sensor](https://github.com/foresail/fs1_psd_sun_sensor)). The repo includes schematics, firmware, mechanical files and PC calibration tools. There is a sister digital sensor, [fs1_dss_sun_sensor](https://github.com/foresail/fs1_dss_sun_sensor). Aalto describes both as built from COTS parts and "rigorously" tested ([Aalto](https://www.aalto.fi/en/spacecraft/digital-and-analog-sun-sensors)). The test paper is [doi:10.1016/j.asr.2026.01.083](https://doi.org/10.1016/j.asr.2026.01.083).
- **Licence:** CERN-OHL-2.0 (PSS).
- **Maturity:** Designed for Foresail-1. Foresail-1 Prime launched in November 2025 ([SmallSat Europe abstract](https://smallsateurope.com/paper/foresail-1-prime-early-in-orbit-results-and-platform-performance/)), but no source confirms the PSS flew on it.
- **Equipment:** PCB assembly and a light source for calibration. Their calibration tools are included.
- **Lesson:** This is the best open fine sun sensor found, with calibration tooling to copy. It is a candidate if the Physical Simulator gets a sun simulator.

### Light-sensor sun sensing (PROVES, CMU)

- PROVES solar boards use a VEML7700 ambient light sensor whose reading "tracks roughly a bell curve" with sun angle ([docs](https://github.com/proveskit/Documentation/blob/main/docs/core_documentation/hardware/XY_solar_board.md)). The F Prime `ADCS` component polls six light sensors for visible, infrared and ambient light ([SDD](https://github.com/Open-Source-Space-Foundation/proves-core-reference/blob/main/PROVESFlightControllerReference/Components/ADCS/docs/sdd.md)).
- The CMU thesis uses six per-face sun sensors and calibrates them in flight. On the bench, calibration cut sun-vector error from 8.3° to 4.7° ([Jensen 2022](https://www.ri.cmu.edu/app/uploads/2022/08/ri_thesis_template.pdf), Fig. 5.6).
- **Lesson:** A coarse sun sensor is one light sensor per face plus calibration. An LED per face is enough to test it.

## 4. Star trackers (brief; the star tracker test method has its own research)

| Project | What | Licence | Maturity |
|---|---|---|---|
| [OpenStarTracker](https://openstartracker.org/) ([repo](https://github.com/UBNanosatLab/openstartracker)) | Star tracker software from the University at Buffalo Nanosatellite Lab, for its GLADOS and SORA missions | MIT | Flight not confirmed. OreSat's star tracker software is based on it ([oresat-star-tracker-software](https://github.com/oresat/oresat-star-tracker-software), GPL-3.0). |
| [tetra3](https://github.com/esa/tetra3) (ESA) | Python lost-in-space plate solver: about 10 ms per solve, about 10 arcsec, for 10–30° fields | Apache-2.0 | Software only. A maintained fork is [cedar-solve](https://github.com/smroid/cedar-solve) (Apache-2.0). |
| StarberrySense ([arXiv:2207.03087](https://arxiv.org/abs/2207.03087); JATIS 8(3) 036002, 2022) | A Raspberry Pi star sensor from COTS parts | Paper | Lab-built |

**Lesson:** The solver is free and good. The hard part for a small lab is optics and testing (a star-field display, a collimator, or the night sky), which is the map's open star tracker question.

## 5. Whole boards and open satellites

| Project | ADCS content | Licence | Maturity | Lesson |
|---|---|---|---|---|
| [PROVES Kit](https://github.com/proveskit) | Flight controller, PCB magnetorquers, light sensors, IMU | CERN-OHL-W-2.0 (hardware), Apache-2.0 ([F Prime software](https://github.com/Open-Source-Space-Foundation/proves-core-reference)) | Three launches in 2023 ([SmallSat 2024](https://digitalcommons.usu.edu/smallsat/2024/all2024/54)); results mixed per [BIRDS webinar slides](https://birds-project.com/open-source/pdf/BIRDS_BUS_OpensourceWebinar_20.pdf) | Already in TXST Space Lab's toolchain: the lab forks [proves-core-reference](https://github.com/txst-space-lab/proves-core-reference) and has a hardware-in-the-loop CI poster ([fprime-to-flight-faster](https://github.com/txst-space-lab/fprime-to-flight-faster)). |
| [PyCubed](https://github.com/pycubed) | One board with power, compute, radio and ADCS; BMX160 IMU (accel, gyro, mag) | CC BY-SA 4.0 (hardware), MIT (software) | "Flight heritage on a successful LEO mission" ([SmallSat 2019](https://digitalcommons.usu.edu/smallsat/2019/all2019/65)) | F Prime ports exist: [fprime-pycubed-baremetal-reference](https://github.com/fprime-community/fprime-pycubed-baremetal-reference), [fprime-pycubed-zephyr-reference](https://github.com/fprime-community/fprime-pycubed-zephyr-reference). |
| [UPSat](https://www.libre.space/projects/upsat/) (Libre Space Foundation, [GitLab](https://gitlab.com/librespacefoundation/upsat)) | Magnetometer, gyro, sun sensor and GPS for determination; a spin torquer "used as a reaction wheel for pitch control"; magnetorquers for roll and yaw ([Wikipedia](https://en.wikipedia.org/wiki/UPSat)) | CERN-OHL (hardware), GPL-3.0 (software) | Deployed from the ISS on 2017-05-18 | Targets were 15° pointing and 5° knowledge. A GPS fault broke determination, and the test campaign never covered the control algorithm ([LSF 2019](https://events.libre.space/event/3/contributions/64/)). Test the controller on the bench before trusting it. |
| [Dalhousie CubeSat-ADCS-Firmware](https://github.com/DalhousieSpaceSystemsLab/CubeSat-ADCS-Firmware) | ADCS firmware for an MSP430F5529 (LORIS project) | MIT | Student project | A worked example of a separate ADCS microcontroller behind the OBC. |
| [AMSAT CubeSatSim](https://github.com/alanbjohnston/CubeSatSim) | Low-cost educational CubeSat simulator, with an IMU on its sensor board | GPL-3.0 | Educational | F Prime ports exist: [fprime-amsat-reference](https://github.com/fprime-community/fprime-amsat-reference). A cheap, ready-made "satellite" to host demo software. |

## 6. Flight software, including F Prime

- **PROVES F Prime reference** ([proves-core-reference](https://github.com/Open-Source-Space-Foundation/proves-core-reference), Apache-2.0, F Prime on Zephyr): the relevant components are `ADCS` (light sensors), `DetumbleManager`, `ImuManager`, and drivers such as `Drv2605Manager`.
  - The [`DetumbleManager` SDD](https://github.com/Open-Source-Space-Foundation/proves-core-reference/blob/main/PROVESFlightControllerReference/Components/DetumbleManager/docs/sdd.md) describes a state machine of COOLDOWN, SENSING_ANGULAR_VELOCITY, SENSING_MAGNETIC_FIELD, ACTUATING_BDOT and ACTUATING_HYSTERESIS, with parameterized thresholds and coil properties.
  - **Lesson:** This is a ready pattern for an F Prime ADCS component: a passive component called by a scheduler, sensors on ports, and actuators behind start and stop ports. It even waits for sensors to settle after torquing. A reaction wheel component for the RWD can follow the same shape.
- **fprime-community sensor drivers:** [fprime-sensors](https://github.com/fprime-community/fprime-sensors) has an `MpuImu` `ImuManager` (MPU-family IMU), plus BMP280, NMEA GPS and radio drivers. Its README marks it "Alpha Software", and it has no licence file.
- **Gap:** No open F Prime component for reaction wheels or brushless motor control was found in fprime-community, the PROVES repos, or a GitHub search for "fprime adcs", "fprime attitude" or "fprime imu".
- **CircuitPython B-dot:** PROVES' [pysquared](https://github.com/proveskit/pysquared) has a `detumble.py` with unit tests. It is a short reference implementation to compare against.
- **Digital Simulation link:** The mission deck picks Basilisk partly because its "modular architecture mirrors FSW for HIL testing" (deck rev 115, slide 14). [Basilisk](https://hanspeterschaub.info/basilisk/) supports hardware-in-the-loop scenarios through real-time clock-tracking modules. The Schaub group describes a desktop-to-flight-software testbed in [Cols Margenet et al. 2020](https://hanspeterschaub.info/Papers/ColsMargenet2020.pdf).

## 7. Low-equipment test methods

| Method | Source | Equipment | Use for us |
|---|---|---|---|
| Static sensor box plus Helmholtz coil | [Jensen 2022](https://www.ri.cmu.edu/app/uploads/2022/08/ri_thesis_template.pdf) | Laser cutter, 3D printer, magnet wire, Arduino, LEDs, magnetometers | Sun sensor and magnetorquer checks without any moving rig |
| Real wheels, simulated spacecraft | [Sakal et al. 2025](https://arxiv.org/html/2508.19164v1) | Wheels, drivers, embedded computer, PC simulation | A Physical Simulator stage that needs no air bearing |
| Trifilar pendulum for moment of inertia | Stijnen et al., University College Dublin, ESA GNC-ICATT 2023, measured the 2U EIRSAT-1 at an estimated cost of about €450 excluding the camera, with periods tracked in Blender (via [search abstract](https://az659834.vo.msecnd.net/eventsairwesteuprod/production-atpi-public/eca73a8542194291a318d5c0ed6ea958); not read in full) | String, a plate, a phone camera | Measure the RWD mock-up's inertia. The controller and the Digital Simulation need it, and the string rig is already half of a pendulum. |
| 3D-printed gimbal | [dylanballback testbed](https://github.com/dylanballback/CubeSat_Attitude_Control) | 3D printer, 80/20 frame | A 3-axis stand with friction but no air supply |
| Spherical air bearing with a 3D-printed platform | USU / HSFL 3U ADCS testing assembly ([SmallSat paper](https://digitalcommons.usu.edu/cgi/viewcontent.cgi?article=3742&context=smallsat); not read in full, the server refused access) | A bought 100 mm air bearing, air supply, PLA platform | The map's stretch goal, if funding allows |

## What this means for the Reaction Wheel Demonstrator

1. **Choose motors for reversible, smooth control through zero speed,** not for maximum speed. A gimbal motor driven by SimpleFOC with an encoder, or a drone motor with a driver that supports reversing, avoids the deadband and low-speed noise reported in [Sakal et al.](https://arxiv.org/html/2508.19164v1). If the motor shows a deadband, bias the wheels to a nonzero speed or command speed instead of torque.
2. **Measure the mock-up's moment of inertia** with a bifilar or trifilar pendulum on the same string setup. Feed it to the classical controller and the Digital Simulation.
3. **Structure the firmware like hs2-reaction-wheels** (driver, IMU, speed loop). If it runs F Prime, model the wheel component on PROVES `DetumbleManager`: a scheduled passive component with parameters and a state machine.
4. **Add wheel guards** ([balancing-cube README](https://github.com/willem-pennings/balancing-cube)).

## What this means for the Physical Simulator

- **Stage the bench the way the published rigs do:**
  1. String test (the RWD).
  2. Real wheels with simulated spacecraft dynamics ([Sakal et al.](https://arxiv.org/html/2508.19164v1)).
  3. A static sensor box in a home-wound Helmholtz coil for magnetometer, magnetorquer and sun sensor tests ([Jensen](https://www.ri.cmu.edu/app/uploads/2022/08/ri_thesis_template.pdf)).
  4. A gimbal or air bearing only if needed.
- **Reuse what the lab already runs.** PROVES PCB magnetorquers and its F Prime `DetumbleManager` give a working desaturation demo for the Helmholtz stage at near-zero cost.
- **Test the control law on the bench before trusting it.** That is the lesson of UPSat.

## Gaps and caveats

- **Not read in full:** The NASA GSFC poster on a wheel shrink-fitted onto an outrunner motor bell (`ssed.gsfc.nasa.gov` didn't resolve), the ERAU undergraduate wheel paper using an MJ5208 brushless motor ([Beyond journal](https://commons.erau.edu/cgi/viewcontent.cgi?article=1138&context=beyond); access refused), the USU air bearing paper, and the UCD trifilar paper. Claims about them here come from abstracts only.
- **Flight status not confirmed:** OpenStarTracker on GLADOS or SORA, and the Foresail PSS on Foresail-1 Prime.
- **Licences not checked:** Pico-torQuer, and the CMU thesis simulator's repository (the thesis says "open source" but gives no link in the pages read).
- **PyCubed-Mini:** Its SmallSat 2023 paper page is marked withdrawn ([USU](https://digitalcommons.usu.edu/smallsat/2023/all2023/200)), so it isn't used here.
