# UNP ADCS Hardware (AFRL-2025-2020): text

Text of [`adcs-hardware-afrl-2025-2020.pdf`](adcs-hardware-afrl-2025-2020.pdf), the "ADCS Hardware" deck from the University Nanosatellite Program (UNP). Every slide carries "Approved for public release; Distribution is unlimited. Public Affairs release approval AFRL-2025-2020". It is a later revision of the [workshop deck](adcs-workshop-afrl-2023-2775.md): it drops the control loop diagram and adds slides on fundamental modes, component selection, moving parts and integrated testing.

Headings use the slide numbers printed on each slide. Slide 19 is hidden, so the PDF has 39 pages: page *n* is slide *n* + 1 from slide 20 onward. Slide text is reproduced as written. Where a slide's text matches the workshop deck apart from punctuation, it points there instead of repeating it. Diagram-only slides are summarised in *italics*; the PDF is authoritative for them.

## 1. ADCS Hardware

## 2. Subsystem Overview

*(Section divider.)*

## 3. ADCS

*(Same text as [workshop slide 2](adcs-workshop-afrl-2023-2775.md#2-what-is-it).)*

## 4. Fundamental Modes

- Detumble
  - Reduce angular rates
- Single axis pointing
  - Point solar panels at sun
  - Align instrument with magnetic field vector
- Fully-determined attitude
  - Point communications antenna at ground site
  - Point optics at specified target
  - Local-vertical/local-horizontal (LVLH) hold (e.g., nadir)

## 5. Sensors

*(Section divider.)*

## 6–10. Magnetometer, Gyroscope, Sun Sensors, Star Tracker, GPS Receiver

*(Same text as [workshop slides 6–10](adcs-workshop-afrl-2023-2775.md#6-magnetometer).)*

## 11. Design/Build of Sensors

*(Same text as [workshop slide 11](adcs-workshop-afrl-2023-2775.md#11-designbuild-of-sensors).)*

## 12. Actuators

*(Section divider.)*

## 13–16. Passive Magnetics, Torque Coils/Rods, Reaction Wheels, Reaction Wheel Physics

*(Same text as [workshop slides 13–16](adcs-workshop-afrl-2023-2775.md#13-passive-magnetics).)*

## 17. Control Moment Gyros (CMG)

- Generate torque by gimballing a flywheel
  - Changing the spin axis, and therefore the angular momentum vector, of the flywheel produces a gyroscopic torque
  - Saturation behavior is alignment of all CMG axes
- Highest control authority of any actuator but also most complicated
  - Very few smallsat-scale options exist
- Conventional CMGs maintain a fixed flywheel speed, variable-speed CMGs can operate in a reaction wheel mode for singularity avoidance

*Pictured:* Honeybee Robotics CMGs.

## 18. Thrusters

- While it is technically possible to use thrusters for attitude control (assuming appropriate placement) this is not recommended
  - Current state of smallsat thrusters is not great for that level of reliability
  - Limited mission life due to propellant usage
  - Relatively coarse control since minimum impulse bit may be large and firing-to-firing consistency may be poor
  - Even large spacecraft only use ADCS thrusters for momentum management and stick with reaction wheels for primary control

*Pictured:* Vacco RCS Prop Module.

## 19. (hidden slide; not in the PDF)

## 20. Requirements

*(Section divider.)*

## 21. Overview

*(Same text as [workshop slide 21](adcs-workshop-afrl-2023-2775.md#21-requirements-overview).)*

## 22. Components

**Typical Integrated High-function CubeSat ADCS:**

- Magnetorquers (3)
- Reaction wheels (3)
- Coarse sun sensors
- Star tracker
- MEMS gyroscope
- Magnetometer

**Selection Flow:**

- Mission requires full three-axis control → reaction wheels
  - Momentum management then requires magnetorquers
- Initial detumble by B-Dot
  - Magnetometer to sense local field
- Solar panel configuration requires active pointing → coarse sun sensors
  - Sufficient to resolve attitude to necessary accuracy and simplest solution that meets requirements
- Payload requires geolocation to <1km → star tracker
  - 1km from ISS orbit is 0.14°, star tracker necessary for that accuracy
  - Still useful to retain sun sensors for lower-power coarse pointing and as a backup
  - Need gyroscope to propagate between star tracker solutions and residual-edit the star tracker

## 23. Requirement Drivers

- Payload
  - Optics will have a maximum allowable jitter (preventing blurring)
  - Pointing accuracy to put sensor on target
  - Pointing knowledge to characterize response and geolocate data
- Communications
  - Tracking a ground site requires up to ~1.5°/s slew rate (ISS orbit)
  - Nadir-track is 0.06°/s slew rate
- Power
  - Solar arrays generate less power as incidence angle decreases
  - Very coarse requirement (3% loss at 14° error)
- CONOPS
  - Sufficient slew rate to allow switching modes in acceptable time
  - Short enough detumble time to get on with it

## 24. Requirement Flow

- Determine top level drivers
  - "I need to point the camera to within 0.5° of the target"
- Determine derived requirements
  - e.g., Error stack to realize 0.5° absolute pointing might allocate 0.1° for knowledge
- Sanity check values
  - Do solutions that meet those requirements exist?
  - Can existing components be stitched together with new code to meet requirements?

Write requirements for either what your mission needs and see which sensor fits that need OR reduce mission capability to fit performance.

## 25. Error Stackup

*(Same text as [workshop slide 29](adcs-workshop-afrl-2023-2775.md#29-error-stack-up).)*

## 26. Dependencies

*(Section divider.)*

## 27. Moving Parts

- Occasionally something else on the satellite will be moving
  - e.g., PolarCube scanning payload mirror
- Non-ADCS moving parts can cause severe ADCS problems
  - If there is something moving at least provide direct state information to the ADCS controller
  - Preferably make the dynamics simple
    - A symmetric-inertia rotating piece on a primary axis just requires counter-rotation of the corresponding reaction wheel
    - A complex-inertia rotating piece on a weird axis (e.g., that mirror) causes all manner of undesirable behavior
  - Requires considerable analysis effort to assess effects
  - Overall, this is to be avoided if possible
- Some pieces are hard to avoid (e.g., solar panels)
  - Mix of multi-link rigid-body dynamics (movement at hinges) and full flexible dynamics (panel flex), some analysis necessary to show that these modes are not excited by the controller (required fidelity depends on how close to the line you are)
  - Stiffer pieces make this easier

## 28. Keep-outs

*(Same text as [workshop slide 30](adcs-workshop-afrl-2023-2775.md#30-keep-outs).)*

## 29. Vehicle Geometry

*(Same bullets as [workshop slide 31](adcs-workshop-afrl-2023-2775.md#31-vehicle-geometry), plus two plots against elapsed time over about a day: solar power generation in watts, and keep-out margin in degrees for Earth, Sun and Moon, with a few dips below zero.)*

CONOPS/design shown here has some reduction in power during payload operations, and some violations of star tracker keep-outs, but not severe.

## 30. Power/Data Impacts

*(Same text as [workshop slide 32](adcs-workshop-afrl-2023-2775.md#32-how-adcs-impact-epscdh).)*

## 31. Magnetics

*(Same text as [workshop slide 33](adcs-workshop-afrl-2023-2775.md#33-magnetics).)*

## 32. Analysis

*(Section divider.)*

## 33. First Checks

- Pointing budget
  - See Mission Design Course presentation
  - What do you actually need?
  - What system gets you there?
- Disturbance torques
  - Check magnitude of solar radiation pressure, atmospheric drag, residual magnetic, and gravity gradient torques to available control authority
    - If constant relative direction, then need to be able to correct with momentum management system
    - If periodic, then need sufficient reaction wheel momentum capacity to buffer (it will balance out)

## 34. Simulation

- Testing any system design, even if the subsystem is largely COTS, requires a simulation
  - Develop and exercise design, test mission modes, etc.
- More complicated dynamics require more simulation work to accurately capture system behavior, regardless of the complexity of any control/filter work
  - Also increases complexity of controls
- Full integrated analysis of what is achievable with specified sensor and actuator performance and with specified filter/control
  - Accuracy depends on fidelity of models
- Requires validation to be useful
  - Angular momentum conserved, kinetic energy rates match torques, etc.

## 35. Testing

*(Section divider.)*

## 36. Polarity

- Verifying that all sensor and actuator polarities are correct is absolutely critical
  - Reaction wheels spin the correct direction, torque rod fields are the expected polarity, gyroscope axes match vehicle axes, etc.
  - Incorrect polarities are like riding your bike with your hands crossed
- Verifying all sensors are responding with reasonable values
  - Verifies that sensors are not damaged or does not have interference with other components
- The same test configurations that check polarity also work well for verifying that all sensors are responding with reasonable values (no damage, interference, etc.)
  - Faulty sensors are also very dangerous

## 37. Reduced System

- Mission profile and software functionality exercised in simulation
- HiTL systems (e.g., the RDP for BCT's XACT) provide some verification of the actual hardware but are not an end-to-end test as sensors are bypassed and system relies on internal telemetry
  - More complicated systems (e.g., using a Helmholtz cage to drive a magnetic field instead of just passing simulated measurements to the filter) are possible but reaching full stimulus and free dynamics is prohibitive
- Can test individual pieces to higher fidelity
  - Known rotation rate input to gyroscope, reaction wheel jitter, etc.
- Tests to improve fidelity of simulation models and FSW responses
  - Some sensors (e.g., MEMS gyros) are quite temperature sensitive

## 38. Integrated System

- Really not practical to quantitatively fully test system function
  - Test fixture interferes with subsystem operation
    - Rotary air bearings cancel out any off-axis/gyroscopic effects
    - Spherical air bearings are extremely difficult to balance
  - Simulating inputs is tricky
- Constrained qualitative function tests are practical and valuable
  - Focus is on verifying safety modes (detumble and sun-track)
  - Rotating vehicle on turntable and verifying reaction wheels spin up in expected direction to compensate verifies gyroscope/RW axis consistency
  - 1D sun-track and detumble can be exercised on a rotary air bearing
- Focus of the ADCS verification test is polarity and safety modes

## 39. Conclusion

*(Same text as [workshop slide 34](adcs-workshop-afrl-2023-2775.md#34-conclusion).)*

## 40. Questions?
