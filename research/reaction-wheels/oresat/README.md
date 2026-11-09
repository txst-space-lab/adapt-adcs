# OreSat / Portland State reaction wheels

The Portland State Aerospace Society (PSAS) builds reaction wheels for its OreSat CubeSats from drone motors. Their 2016 ground test article, hung from a string and pushed by hand, is the model for our Reaction Wheel Demonstrator ([ADCS team sync, 2026-11-06](../../../docs/mission/meetings/2026-11-06-adcs-team-sync.md)). Two requests to the lab for details got no reply, so this folder collects their public material.

## Sources

| Source | What it is |
|---|---|
| [2016 paper](2016-louke-open-source-reaction-wheel-system.pdf) | Louke, Schmidt and Young, *An Open-Source Reaction Wheel System for Oregon's First Satellite*, AIAA, 2016 (7 pages; also on ResearchGate). Covers the ground test article, its controller model, and a drop tower test. |
| [Reaction Wheel Equations RevA](https://github.com/oresat/oresat-adcs-hardware/blob/master/documentation/Reaction%20Wheel%20Equations%20RevA.pdf) | 8 hand-written pages, hosted in OreSat's repo: the pyramid geometry, wheel inertia, required wheel speeds, and wheel mass for a 2U CubeSat. |
| [oresat/reaction-wheels](https://github.com/oresat/reaction-wheels) | The 2016 project repo, archived in 2019: CAD, the Python controller (`Edison_RW_Code_Python/`), Octave models (`Modeling/`), and raw drop test data (`Drop Test 2016-04-26/`). No license file; the paper says PSAS releases its work under GPL v2. |
| [2025 poster](2025-kane-pardy-advancing-oresat-adcs-poster.pdf) | Kane-Pardy, *Advancing the OreSat ADCS*, PSU graduate internship poster, 2025: flywheel sizing, a new controller board, and a plan to compare motor control algorithms. |
| [2026 poster](2026-lutz-oresat-reaction-wheel-upgrade-poster.pdf) | Lutz et al., *OreSat Reaction Wheel Upgrade*, OSGC Student Symposium, 2026-05-22: building flight units from drone motors and hybrid bearings. |
| [Captive hanging test](https://www.youtube.com/watch?v=Is1gPHlewtg) (video) | The 2016 ground test article hanging from a string. Pushed by hand, the wheels stop the rotation and then resist further pushes. This is our string-push test. |
| [Drop tower test](https://www.youtube.com/watch?v=cC2FpmCP5B4) (video) | The same article in PSU's 2.1 s Dryden Drop Tower. |
| [OreSat walk-through, part 6](https://www.youtube.com/watch?v=S82_q8WI_1M) (video) | PSAS tour of OreSat's reaction wheels and magnetorquers. |

Current OreSat work lives in other repos: [oresat-reaction-wheel-hardware](https://github.com/oresat/oresat-reaction-wheel-hardware) and [oresat-reaction-wheel-firmware](https://github.com/oresat/oresat-reaction-wheel-firmware) (wheel v8, no license file), [oresat-adcs-hardware](https://github.com/oresat/oresat-adcs-hardware) (controller boards, CERN-OHL-S v2), and [oresat-adcs-software](https://github.com/oresat/oresat-adcs-software) (Python quaternion, frame and attitude estimation code, GPL-3.0).

## Key takeaways

### The 2016 ground test article

- **Build.** A 1U mock-up printed on an FDM 3D printer in about 8 weeks. On it: four COTS brushless drone motors driving the wheels, a 4-cell LiPo, a 9-DoF IMU (LSM9DS0), and an Intel Edison with a GPIO breakout and a motor driver board. The flight design was 2U.
- **Wheel layout.** Four wheels in a pyramid, chosen for redundancy on the flight unit. Any three can control all three axes.
- **Motors.** Deliberately overpowered: rated to 22,000 RPM, with far more torque than flight needs. The aim was to test how fast the controller reacts, without power or heat limits. The equations size against a Makerfire D1104 7500KV.
- **Control.** A proportional (paper: proportional-integral) loop in Python on the Edison. It reads the gyro and sets each motor's PWM duty and direction pin. The repo's controller uses a gain of 0.3, tuned by hand to be slightly underdamped so the results were easier to compare with the model.
- **Model.** For one axis, they summed torques from the wheels with a damping term and a spring term, and took a transfer function from one wheel's angle to the cube's angle (paper, eq. 2). They simulated it in GNU Octave. On a hanging rig, the spring term is the torsion of the string.
- **Tests.** First the captive hanging test, then the drop tower: a random push, then the controller damps the roll in about 1.9 s. Gyro data matched the model's frequency and damping, with a 0.03 s lag put down to controller delay. The raw CSVs are in the repo.

### The equations

- **Wheel speed.** For a pyramid, the required wheel speed is |ω_rw| = I_sat·ω_sat / (4·cos θ·I_rw), where θ is the angle between each wheel's axis and the body axis.
- **Pyramid angle.** For a cube, the angles are equal: 54.74° to each axis, 35.26° from the base plane. For OreSat's 2U body they tilt the wheels to 28.36° from the base plane, because the long axes have 1.31 times the inertia of the short one.
- **Wheel design.** A ring on a disc mounted on the outrunner's case. The aluminum wheel's inertia is about 4.1×10⁻⁶ kg·m², stainless steel about 1.0×10⁻⁵ kg·m².
- **Speed versus slew rate.** Slewing at 1 RPM needs about 1,400 RPM on an aluminum wheel. Slewing at 10 RPM needs about 14,000.
- **Mass.** Four motors with aluminum wheels come to about 80 g, before the controller board.

### Later work

- **2025: motor control.** The plan was to implement three algorithms on a new controller board running Zephyr RTOS and compare them: trapezoidal (simple, but its torque ripple shakes the sensors), sinusoidal, and field-oriented control (FOC, smoothest across all speeds). The flywheel grew from 42 g to 70 g after modeling the 2U and 3U spacecraft.
- **2026: flight units.** They now build flight wheels from drone motors and hybrid bearings, at a fraction of COTS cost. The hard part was hand assembly. They succeeded more often after changing their procedures and redesigning the wheel.

## How this applies to us

**Reaction Wheel Demonstrator**

- **The string-push test is proven.** OreSat used it before the drop tower, and it fits with string suspension being a published method (see [What low-cost attitude testbeds do university CubeSat labs use?](https://github.com/txst-space-lab/adapt-adcs/issues/17)).
- **Three wheels, not four.** Our demonstrator has three motors, one per axis. That needs no pyramid geometry, but it has no redundancy, which a ground rig doesn't need.
- **Motors** ([Research motors for the Reaction Wheel Demonstrator](https://github.com/txst-space-lab/adapt-adcs/issues/12)):
  - Cheap drone motors work.
  - OreSat's own later work moves toward FOC for smooth torque, which matches the advice to drive a gimbal motor with SimpleFOC rather than a drone ESC.
  - Overpowered motors make a forgiving first rig.
- **Casing** ([What form factor is the Reaction Wheel Demonstrator casing?](https://github.com/txst-space-lab/adapt-adcs/issues/19)): OreSat printed a 1U mock-up even though they flew 2U. Their SolidWorks parts (`SW_CAD_1U_cubesat/`, `wheel.SLDPRT`, `motorBracket.SLDPRT`) can serve as a reference.
- **Electronics** ([Which controller board, motor drivers and IMU drive the Reaction Wheel Demonstrator?](https://github.com/txst-space-lab/adapt-adcs/issues/20)):
  - The Intel Edison has been discontinued, so their board isn't one we can copy.
  - The pattern is still useful: a Linux board running Python, an I²C IMU, and PWM plus direction pins to a motor driver.
  - OreSat has since moved to a microcontroller running an RTOS.
- **Model and data.** Their single-axis model, with string torsion as the spring term, is a ready template for predicting the string-push response. Their drop test CSVs can serve as a first check of the Digital Simulation.

**Flight unit**

- **Lab-built wheels have flown.** OreSat builds its flight wheels from drone motors, so "could lab-built wheels fly?" (2026-11-06 sync) has a real precedent. Their pointing need was 5–10° for an antenna, though. ADAPT points a star tracker at stars and has a much tighter need ([Ask leadership and Payload what flight reaction wheel accuracy is needed](https://github.com/txst-space-lab/adapt-adcs/issues/9)).
- **Sizing method.** The equations give a method to repeat for ADAPT's 6U inertia once the flight hardware list is set ([Select ADCS Hardware](https://github.com/txst-space-lab/adapt-adcs/issues/8)).

**Licensing.** Before copying OreSat code or hardware designs into ADAPT, check the license:
- oresat-adcs-software is GPL-3.0 and the boards are CERN-OHL-S, both copyleft.
- The 2016 repo and the v8 wheel repos have no license file at all.
- Reading them and reusing their ideas is fine.
