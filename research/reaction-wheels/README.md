# Open-source and lab-built reaction wheels

A survey of reaction wheels that students, hobbyists and small labs have built, gathered 2026-11-08 for the [Physical Simulator map (#5)](https://github.com/txst-space-lab/adapt-adcs/issues/5). It extends section 1 of [Open-source and published ADCS designs a small lab can build](../small-lab-adcs-designs/README.md) (#18) and doesn't repeat it. That section covers:
- OreSat
- Cubli and `willem-pennings/balancing-cube`
- SimpleFOC
- the Florida Tech hardware-in-the-loop paper
- the UW, UPC and `dylanballback` student repos

OreSat / Portland State has its own folder: [`oresat/`](oresat/).

## Short answer

- **Still no open flight reaction wheel to copy.** Every open design found is a ground rig or an unflown student prototype.
  - None of the university-built wheels found has published on-orbit wheel data.
  - Delfi-n3Xt (TU Delft, launched 2013) flew wheels developed in-house. Its team reported the ADCS "functions as expected," but published no wheel telemetry, and two of its wheels broke in ground vibration tests.
  - UPSat's spin torquer is covered in #18.
- **Small labs drive wheels with three cheap motor families:**
  - drone or gimbal outrunners (OreSat, SimpleFOC builds)
  - the **Nidec 24H**, which has a driver, encoder and brake built in
  - **hard disk drives** (HDDs), whose platters are already balanced
- **Our string-push test is a standard method.** UC Davis and Cornell call it fishing-line testing. UC Davis documents it in detail, including a pitfall: the key ring the CubeSat hung from added a 3.5 Hz pendulum wobble to the gyro data.
  - With 2.5-inch HDD wheels on the line, the controller reached its target to within 0.15–1.24°, with body rates below 1 deg/s ([Negi 2023](https://escholarship.org/uc/item/3wc05512), §3.3.4).
- **Start with an off-the-shelf motor controller.** UC Davis dropped its custom ESC (electronic speed controller) and used drone ESCs, which saved its project schedule.
  - OreSat's 2026 thesis found field-oriented control (FOC) gives the strongest dynamic response and the most consistent torque, and trapezoidal control uses the least energy.
- **An open F´ reaction wheel component exists.** NASA's NOS3 project publishes one, with a matching simulated wheel. It's a model for our own component, and an example of one set of flight software driving either a simulated wheel or a real one ([Flight software](#flight-software)).
- **Machined, precision wheels are hard for students.** Cal Poly's senior project needed CNC tools, outsourced balancing and several tries at bonding the wheel to the shaft. In the end it built one wheel.

## Motor families

| Family | How it's driven | Pros | Cons | Who used it |
|---|---|---|---|---|
| Drone or gimbal outrunner (e.g. 2204–2804 gimbal motors, D1104 drone motor) | SimpleFOC board with a magnetic encoder, or a drone ESC | Cheap and plentiful. The rotor bell can carry the wheel. FOC gives smooth torque through zero speed | A drone ESC runs one direction at throttle; a reversible ESC or FOC is needed. Low-speed behavior is poor without an encoder | OreSat ([`oresat/`](oresat/)), SimpleFOC pendulum, [samdearing/cubli-reaction-wheel](https://github.com/samdearing/cubli-reaction-wheel), [Hackaday cube](https://hackaday.io/project/202695-reaction-wheel-self-balancing-cube) |
| Nidec 24H series | PWM speed, direction pin, brake pin and a built-in encoder; 12–24 V by listing | No separate motor driver or encoder to wire. It's the motor in the most-copied DIY balancing cube | Pinouts differ between variants and the documentation is unofficial: one teardown found the 24H055M020 takes pulse-frequency input, not PWM ([teardown](https://structables.lunar.icu/Reverse-Engineering-the-Nidec-24H055M020-Encoder-S)). Usually bought surplus. One builder burned theirs out | [remrc/Self-Balancing-Cube](https://github.com/remrc/Self-Balancing-Cube) (ESP32, MPU6050), [IFMG ESP32 ADCS thesis](https://repositorio.ifmg.edu.br/bitstreams/1ae98d3a-e935-427d-956c-8aa3d67369a4/download) |
| Hard disk drive (1.8, 2.5 or 3.5 inch) | The HDD's own driver runs one direction only, so it's replaced with a bidirectional drone ESC (UC Davis: Readytosky 20A; iFlight SucceX-E F4 4-in-1) | Balanced platter, fluid bearing, free from e-waste. Cost $50–200 against $2,700–35,000 for commercial CubeSat wheels (Negi 2023, 2023 prices) | Needs the drive opened and the heads removed. Without an encoder, speed comes only from ESC command mapping | UC Davis ([Negi 2023](https://escholarship.org/uc/item/3wc05512)), Technion (Sahar et al., *J. Spacecraft and Rockets* 55, 2018), Manitoba TSAT5 ([IAC 2019](https://iafastro.directory/iac/archive/browse/IAC-19/E2/4/54431/)), Cornell ([SmallSat 2025](https://digitalcommons.usu.edu/smallsat/2025/all2025/136)) |
| Precision BLDC (Maxon, Faulhaber) | The manufacturer's driver, or a small BLDC driver IC | Datasheets, Hall sensors, built for long life | About $300 per motor (Cal Poly). Machined wheels and balancing are needed to get any benefit | Cal Poly senior project, GSFC interns, Lisbon / ECOSat-III, [UWCubeSat hs2](https://github.com/UWCubeSat/hs2-reaction-wheels), [balancing-cube](https://github.com/willem-pennings/balancing-cube) |

## Projects with files

| Project | What | Licence | Status |
|---|---|---|---|
| [remrc/Self-Balancing-Cube](https://github.com/remrc/Self-Balancing-Cube) | 3-wheel balancing cube: ESP32, MPU6050, three Nidec 24H, 500 mAh LiPo. Includes a motor test sketch that checks each motor's direction and encoder, and gains you can tune over Bluetooth. Printable parts on [Thingiverse](https://www.thingiverse.com/thing:6695891) | None (all rights reserved) | 551 stars, active to 2024; widely copied |
| [samdearing/cubli-reaction-wheel](https://github.com/samdearing/cubli-reaction-wheel) | 3-wheel cube: Teensy 4.1, FOC on 2804 220 KV gimbal motors, IMU fusion, nested PID/LQR | MIT | Personal project, 2026 |
| [simplefoc/Arduino-FOC-reaction-wheel-inverted-pendulum](https://github.com/simplefoc/Arduino-FOC-reaction-wheel-inverted-pendulum) | One gimbal motor (e.g. GM4108H-120T) as a reaction wheel on the SimpleFOC shield; control loop about every 25 ms ([docs](https://docs.simplefoc.com/simplefoc_pendulum)) | MIT | Official SimpleFOC example |
| [Hackaday: Reaction Wheel Self Balancing Cube](https://hackaday.io/project/202695-reaction-wheel-self-balancing-cube) | Student build log: Nidec 24H motors (burned out) replaced by a 2208 90 KV gimbal motor on a SimpleFOC shield with an Arduino Uno R4; MPU6050; AS5600 encoder; screws in the wheel rim to tune its inertia | None stated | Ended "very jittery" and the motor ran hot; a useful list of failure modes |
| [UWCubeSat/HS2-RW](https://github.com/UWCubeSat/HS2-RW) ("COMPASS") | Husky Satellite Lab's successor to `hs2-reaction-wheels`: firmware for 4 pyramid wheels on an Arduino Mega (PlatformIO) with a BNO085 IMU, which outputs orientation as a quaternion. The control chain: a quaternion PD loop gives the body torque, `FourWheelMode` splits it across the four wheels, and a PD loop per wheel turns speed error into PWM and direction-pin commands. Wheel speed comes from interrupt pins | MIT header in `main.cpp`, no licence file | 29 commits, 2023-05 to 2024-02. The closest open firmware to what the demonstrator needs: replace the four-wheel split with one wheel per axis |
| [jalouke/CubeSAT-Reaction-Wheel](https://github.com/jalouke/CubeSAT-Reaction-Wheel) | The upstream of `oresat/reaction-wheels` | None | 2016 |
| [kcglab/satllazero](https://github.com/kcglab/satllazero) | Ariel University's open SATLLA-0 CubeSat | GPL-3.0 | Not checked for a reaction wheel |
| [SmallSatGasTeam/CubeWorks](https://github.com/SmallSatGasTeam/CubeWorks) | Flight software for GASPACS. A search summary credits it with a 3D-printable reaction wheel, but no wheel files turned up in its tree | None | Not confirmed |

## Flight software

[nasa-itc/generic_reaction_wheel](https://github.com/nasa-itc/generic_reaction_wheel) is the reaction wheel component from NASA's [NOS3](https://github.com/nasa/nos3) small-satellite simulator. One repo holds a cFS app, an F´ component, a standalone checkout program, ground system definitions, and a NOS3 simulated wheel. Licence: NOSA 1.3, stated in the README; there is no licence file. Last pushed 2026-10-29.

- **The F´ component** (`fsw/fprime/rw_src/Generic_reaction_wheel.fpp`) is an active component.
  - Two scheduler inputs: one to update data, one to send telemetry.
  - A data output port.
  - Commands: `SET_TORQUE` for wheel 0–2 (in units of 10⁻⁴ N·m), `REQUEST_DATA`, per-wheel `ENABLE`/`DISABLE`, `RESET_COUNTERS` and `NOOP`.
  - Its design doc (`docs/sdd.md`) is an unfilled template.
- **The device layer** (`fsw/shared/`) talks to each wheel over a UART in plain text: it sends `CURRENT_MOMENTUM` and parses `CURRENT_MOMENTUM=<value>`, and sets torque the same way. The simulated wheel (`sim/`) answers on the same UART, using wheel state from the 42 dynamics simulator.
- **What it means for us.** This is an open F´ reaction wheel component to model ours on, alongside PROVES `DetumbleManager` (see #18). Its split is one answer to two open questions on the map: where the demonstrator's software runs, and how the Digital Simulation links to hardware. The flight software talks to a "wheel" over a serial link, and either a simulated wheel or the real one can be on the other end.
- **Licence caution.** NOSA 1.3 is OSI-approved but is not compatible with the GPL. Read it before copying files into ADAPT's flight software.

## Attitude simulators with reaction wheels

These are software only. They're more relevant to the Digital Simulation (#4) and the controller comparison than to wheel hardware.

| Project | What | Licence | Note |
|---|---|---|---|
| [fallow24/neuro-sat](https://github.com/fallow24/neuro-sat) | C++/Qt app that controls a spacecraft's rotation rate with reaction wheels, using either a PID or a deep neural network controller. It generates training data, trains and saves networks (MiniDNN), and adds precession disturbances | None | Inactive since 2021. A small-scale version of ADAPT's ML-versus-classical comparison |
| [sebastianabelloc/openlodestar](https://github.com/sebastianabelloc/openlodestar) | Python ADCS simulator (v0.3): quaternion kinematics, rigid-body dynamics, reaction wheels with torque and speed saturation and gyroscopic coupling, and quaternion PD control split across 3 orthogonal wheels | MIT | 49 tests, including physics checks: kinetic energy constant in torque-free motion, and momentum changing exactly with applied torque. A test pattern for the Digital Simulation. New, no users yet |
| [brunopinto900/attitude_control_reaction_wheels](https://github.com/brunopinto900/attitude_control_reaction_wheels) | Python simulation: 4 pyramid wheels, a two-loop controller (attitude error to rate command, then rate tracking), wheel saturation, a failed-wheel case, and a 3D animation | None | A teaching simulation |
| [Bespi123/ADCSim](https://github.com/Bespi123/ADCSim) | MATLAB ADCS simulator with reaction wheel and sensor models | GPL-3.0 | A possible reference for the Digital Simulation's wheel model |

## Papers and theses

| Source | What it adds | Read |
|---|---|---|
| Negi, *CubeSat Attitude Control Using Hard Disk Drives as Reaction Wheels*, MS thesis, UC Davis, 2023 ([eScholarship](https://escholarship.org/uc/item/3wc05512); 112 pages, 102 MB, not copied here) | The most complete lab-built wheel study found. It covers HDD conversion, a bidirectional drone ESC, 18650 packs, an actuator model from step tests, vacuum and vibration tests, fishing-line testing, and 3-axis stabilization and pointing in parabolic flight (TRL 4 to 6). Chapter 4 is a how-to guide. A 2.5-inch HDD wheel was delivered to NASA JSC in July 2023 for a 6U flight. It also reviews student wheel projects (refs. 10–16) and notes none had flown | In full: abstract, §1.2.4, §2.3.4, §3.3.4 and ch. 5 |
| Sakal, Nehma, Riano-Rios and Tiwari, *Real-time Testing of Satellite Attitude Control With a Reaction Wheel Hardware-In-the-Loop Platform*, arXiv:2508.19164, 2025 ([copy](papers/2025-sakal-reaction-wheel-hil-arxiv-2508.19164.pdf), CC BY 4.0) | The Florida Tech rig from #18: real wheels with simulated spacecraft dynamics; current deadband; noise near zero speed | Covered in #18 |
| McCarver, Casillas, Lee and Leon, *3-Axis Reaction Wheel System for CubeSats*, Cal Poly senior project, 2021 ([expo page](https://projectexpo.calpoly.edu/?p=9029)) | Maxon EC-10 8 W motor with a stainless wheel and HyMu-80 shielding, CNC-made. Measured: 65.45 g, 1.13 mN·m maximum torque, 60,000 RPM. Balancing was outsourced (about 2.5% of the mass removed). RT-48 retaining compound didn't cure, so 3M Scotch-Weld epoxy held the wheel. Only one wheel was built. Its budget limit was $2,000; Negi puts the estimated cost of three wheels at $2,237 | Expo page; summarized in Negi §1.2.4 |
| Bonafede, *Low-Cost Reaction Wheel Design for CubeSat Applications*, MS thesis, Cal Poly, 2020 ([record](https://digitalcommons.calpoly.edu/theses/2558/)) | The design and simulation behind the Cal Poly senior project, for 3U CubeSats | Abstract only (PDF refused automated download) |
| Maymon, Oslin and Cruz-Ortiz, *Reaction Wheel for CubeSat Attitude Control*, NASA GSFC poster, CubeSat symposium, 2017 ([PDF](https://science.gsfc.nasa.gov/attic/cubesats.gsfc.nasa.gov/symposiums/2017/posters/009P-Gerardo_CubeSat_ReactionWheels.pdf)) | Two thin wheels: a shaft-mounted design, and a brass wheel shrink-fit onto a Maxon outrunner's bell (heated to 250 °C). The bell-mounted design hit 0.67 g·mm static imbalance against a goal of under 1.5; the shaft-mounted one reached 11. Set screws in the rim balance the wheel. An Arduino PWM drives the motor driver, with PID speed control | In full |
| Gomes, *Development of a reliable and low cost miniaturized Reaction Wheel System for CubeSat applications*, MS thesis, IST Lisbon / UVic ECOSat-III, 2016 ([PDF](https://fenix.tecnico.ulisboa.pt/downloadFile/281870113702885/Thesis.pdf)) | Faulhaber 1202 BLDC; disc-and-rim flywheel optimized for mass and stress; a pyramid layout chosen to trade redundancy against power; a simplified BLDC model in Simulink; MTTF analysis. Simulation only | Conclusions |
| Monteiro, *Sistema de controle e determinação de atitude para nanosatélites* (Portuguese), IFMG, 2026 ([PDF](https://repositorio.ifmg.edu.br/bitstreams/1ae98d3a-e935-427d-956c-8aa3d67369a4/download)) | A CubeDesign 2025 competition ADCS: ESP32, MPU6050, a Nidec 24H wheel (after testing an A2212 drone motor), gray-box friction and actuator models from bench tests, and a test at INPE (Brazil's space agency) | Abstract and table of contents |
| Shields et al., *Characterization of CubeSat Reaction Wheel Assemblies*, JoSS 6(1), 2017 ([PDF](https://www.jossonline.com/wp-content/uploads/2021/08/Final-Shields-Characterization-of-CubeSat-Reaction-Wheel-Assemblies.pdf)) | JPL dynamometer tests of BCT and Sinclair wheels: imbalance and harmonics. On ASTERIA, wheels produce about 0.1 arcsec RMS jitter. Relevant to the flight unit, not the demonstrator | Abstract and conclusions |
| Dekens, Brouwer, Bouwmeester and Kuiper, *Development of a Nano-Satellite Reaction Wheel System with COTS Motors*, ESA Round Table, 2010 ([slides](https://escies.org/download/webDocumentFile?id=60590)); Guo, Bouwmeester and Gill, *In-orbit results of Delfi-n3Xt*, Acta Astronautica 121, 2016 ([doi](https://doi.org/10.1016/j.actaastro.2015.12.003)) | TU Delft chose to build around a COTS motor, over buying or a full in-house build, for cost and schedule. In ground vibration tests one wheel's motor "completely disintegrated" and another's flywheel came off its axle | Search summaries only |
| A low-cost method for reaction wheel torque characterization, SmallSat 2016, SSC16-VIII-6 ([PDF](https://digitalcommons.usu.edu/cgi/viewcontent.cgi?article=3395&context=smallsat)) | Measures wheel torque from angular acceleration with a Pixy camera and an Arduino, for under $100 against about $40,000 for a Kistler table | Search summary only (PDF refused automated download) |

## Fishing-line testing, as UC Davis did it

UC Davis's account (Negi 2023, §3.3.4) is the most detailed description found of our string-push test:

- **Line:** 30 lb fishing line, 1–2 m long. A longer, thinner line has less twist stiffness and damping, which would otherwise make the controller look better than it is.
- **Hanging point:** from a **corner**, so a rotation about the vertical engages all three wheels. Hanging from the center of a face tests one wheel at a time.
- **Attachment:** a key ring for quick on and off, and foam underneath in case the knot fails.
- **The push:** spin it by hand, with one hand on each side, to avoid starting a swing or a bounce.
- **Pitfall:** the 2 cm key ring acted as a short pendulum and showed up as a 3.5 Hz oscillation in the gyro data, matching f = (1/2π)·√(g/L).
- **Their recommendation:** model the line's twist, spring and pendulum dynamics in simulation, so ground results reflect the wheels and controller rather than the rig.

Cornell's 2025 HDD rig used a fishing line with a swivel hook for its engineering unit, then an air bearing.

## What this means for the Reaction Wheel Demonstrator

- **Motor choice** ([Research motors for the Reaction Wheel Demonstrator](https://github.com/txst-space-lab/adapt-adcs/issues/12)): compare the three cheap families above, not just drone motors.
  - A Nidec 24H needs the least wiring, but buy one and confirm its pinout before buying three.
  - An HDD wheel is almost free and comes balanced, but needs a reversible ESC.
  - A gimbal motor with SimpleFOC gives the smoothest control through zero speed.
- **Get it spinning with an off-the-shelf motor controller first.** Write a custom driver later, if ever (Negi, Lessons Learned).
- **Measure wheel speed directly** with an encoder or ESC telemetry, rather than inferring it from the command (Negi, Recommendations). Every family above has a way to do this except the bare HDD.
- **Start with a PD controller.** Negi recommends one as the baseline; our classical controller placeholder is PID (deck slide 13).
- **For the string-push test:** hang from a single swivel or tie-off rather than a ring, and use a long, thin line. Record the line's natural twist period with the wheels off, so the Digital Simulation can model the rig.
- **Balance with set screws in the rim.** Both the GSFC poster and the Hackaday build tune the wheel with screws, which is cheaper than machining a precise wheel.

## Gaps

- **Not read in full:** Bonafede, the 2016 SmallSat torque paper, the ERAU *Beyond* paper and the full 2026 OreSat thesis (Kane-Pardy, [PDXScholar](https://pdxscholar.library.pdx.edu/open_access_etds/7192)). The repository servers refused automated downloads; open them in a browser.
- **Not confirmed:** whether any OreSat reaction wheel has flown. The 2026 poster calls the completed wheel "ready to be integrated… and hopefully make it to space."
- **Prices** for the Nidec 24H and HDDs weren't checked against current listings.
