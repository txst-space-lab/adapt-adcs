# UNP ADCS Workshop (AFRL-2023-2775): text

Text of [`adcs-workshop-afrl-2023-2775.pdf`](adcs-workshop-afrl-2023-2775.pdf), the "Attitude Determination and Control (ADC) Subsystem" deck from the University Nanosatellite Program (UNP) workshops, from AFRL's Exploration Research and Technology Programs. Every slide carries "Approved for public release; distribution is unlimited. Public Affairs release approval AFRL-2023-2775". Headings use the slide numbers printed on each slide. Slide text is reproduced as written. Diagram-only slides are summarised in *italics*; the PDF is authoritative for them. Notes from the recorded talk are in [`adcs-workshop-talk.md`](adcs-workshop-talk.md).

## 1. Attitude Determination and Control (ADC) Subsystem

Exploration Research and Technology Programs.

## 2. What is it?

- Attitude Determination and Control Subsystem (ADCS)
  - Also known as ADC, ACS, ADACS
  - "GNC" often means something different as it can imply orbit control
- Attitude Determination
  - Orientation defined with respect to a coordinate frame
  - Sensors provide input to estimation algorithms of where vehicle is pointing
- Attitude Control
  - Actuators and control law to angle vehicle where we want, or at the rate we want
- ADCS aims to figure out how the satellite is oriented in inertial space and then point to where ground operators desire

## 3. How is this achieved?

**Attitude Determination**

- Hardware
  - Sensors – device that measures a physical property
- Software (Determination/Estimation)
  - Software algorithm utilizing sensor information
  - Combines both dynamics/physics models of expected motion with the data from the sensors. Data filtering/estimation fits both of these sources of information together and 'smartly' computes the most likely 'true' attitude
  - E.g. Least Squares Method, Kalman filtering, Bayesian estimation

**Attitude Control**

- Hardware
  - Actuators – device that contributes to the physical movement of the ADCS
- Software (Control)
  - An algorithm
  - Utilizes estimation + desired (commanded) attitude/rates and computes method for achieving desired behavior
  - E.g. B-dot, Proportional-Integral-Derivative, LQR, Lyapunov

## 4. ADCS "Control Loop"

- A diagram showing the major steps, and continuously computed nature, of ADCS
- Generally systems perform determination several times a second
  - Sensors measured at least as often
- Generally systems compute control several times a second
  - Actuators continuously driven

*(Block diagram. Control path: reference attitude ρ̂_ref ("desired attitude based on the mission being performed") is differenced with the estimate ρ̂_est and fed to the attitude controller ("computes the necessary forces/torques to meet the pointing objective"), then actuator control ("computes the actuator commands to realize the necessary forces/torques"), then the actuator plant ("generates actual forces/torques based on actuator commands"). External disturbances ("the forces/torques experienced by the space environment") add in before the spacecraft plant ("the dynamic system that is the spacecraft and all of its components"), whose true attitude is ρ̂_true. Determination path: spacecraft sensors ("the components of the spacecraft that measure its behavior and the celestial phenomena") feed estimation algorithms ("computes the attitude of the spacecraft (relative to some frame of reference) based on celestial phenomena models and observed measurements"), which produce ρ̂_est. Boxes are coloured as tangible (actuator plant, external disturbances, spacecraft plant, sensors) or software (reference, controller, actuator control, estimation).)*

## 5. Sensors

*(Section divider.)*

## 6. Magnetometer

**What does it do?**

- Senses local magnetic field
- Typically (for CubeSats), an integrated three-axis MEMS (microelectromechanical systems) package that reports field strength along each sense axis

**Why is it used?**

- In combination with torque rods (actuators), they can be used to reduce system momentum, important to keeping stable over long durations
  - e.g. B-dot detumble & reaction wheel momentum reduction

**Other Considerations**

- Can be damaged by exposure to excessive fields
- Can pick up residual fields from various components as well as dynamic fields such as reaction wheel motors
- Require phasing with magnetorquers to avoid magnetorquer field dominating measurements

*Pictured:* HMC5883L MEMS Magnetometer, <https://cdn-shop.adafruit.com/datasheets/HMC5883L_3-Axis_Digital_Compass_IC.pdf>

## 7. Gyroscope

**What does it do?**

- Senses angular rates
- Can be purchased as integrated three-axis packages or discrete single-axis components
  - Primary specifications are noise, bias, angular random walk
  - Can be quite sensitive to temperature changes or mechanical stress

**Why is it used?**

- KEY device to propagate attitude motion of spacecraft
- Defines the body coordinate frame of the attitude system (and possibly the spacecraft)

**Other Considerations**

- Fundamental technologies are MEMS, fiber-optic gyro (FOG), and ring-laser
- Cubesats are typically MEMS, some smallsats use FOGs
- MEMS are cheap and low space/weight/power (SWaP)
- MEMS are quite noisy and drift relatively quickly, this impacts attitude filter design
  - Many academic papers assume FOG- or RLG-level performance in their simulations
  - MEMS gyros likely require modelling of bias drift for meaningful assessment
  - Can buy your way out of some of this

*Source:* <https://www.digikey.com/en/articles/apply-sensor-fusion-to-accelerometers-and-gyroscopes>

## 8. Sun Sensors

**What does it do?**

- Senses the direction and position of the sun with respect to the spacecraft
  - Can be used to point solar array face
- Accuracy ranges from ~0.5° (image sensor) to ~5° (photodiodes)
- Need some other sensor (or allow drift) while in eclipse

**Why is it used?**

- Detect where the sun is to help point the solar panels
  - Safe Mode often uses these
- Simple sensor, easy to use and low likelihood to fail

**Other Considerations**

- Many varieties exist
- Discrete photodiodes: Simple intensity, are combined at the subsystem level to form an estimated sun vector
- Photodiode pyramid: Differential intensities on the faces of the pyramid estimate a pointing error
- Integrated IC: Report X/Y angles of the incident light source
- Image Sensor: Project light source onto an image sensor to produce a vector

*Pictured:* SolarMems Fine Sun Sensor, <https://www.solar-mems.com/wp-content/uploads/2020/04/Solar-MEMS-nanoSSOC-A60.pdf>

## 9. Star Tracker

**What does it do?**

- Takes a picture of the star field and references to internal database to determine what part of the sky it is looking at
  - Image provides both direction to identified centroid and rotation about that vector
  - ~5 arc-sec cross-boresight and ~15 arc-sec roll accuracy (at star tracker unit level) achievable in CubeSat hardware

**Why is it used?**

- Generally the most accurate sensor to provide attitude knowledge
- Secondary measurement of angular velocity (if slow enough rotation < 2°/s)

**Other Considerations**

- Sensitive to stray light so it requires a baffle and/or large keepouts around any bright object
  - More complex algorithms provide some resilience
- Sensitive to vehicle rotation as excess rotation smears stars, reducing accuracy and eventually breaking the algorithm
  - Some use optical-flow algorithms to provide rate information
- Introduce AIT handling considerations (cleanliness)
- Some systems use two star trackers (oriented 90° apart)
  - Allows correction of reduced accuracy in roll
  - Higher availability (one star tracker can track while the other is blinded)
  - Adds filter complexity to avoid jitter from switchover

*Pictured:* Blue Canyon Technologies, <https://cubesats.gsfc.nasa.gov/symposiums/2018/presentations/Day2/1115_Hegel.pdf>

## 10. GPS Receiver

**What does it do?**

- The Global Positioning System (GPS) receiver provides the satellite position and time on the satellite using GPS constellation (NavStar)
  - Provides Navigation
  - Provides Timing

**Why is it used?**

- When needing to point to Earth, ADCS needs to know where Earth center or locations on Earth are relative to the spacecraft
  - Imaging
  - Comm pointing
- Accurate Timing (events, precision of measurements across multiple systems)

**Other Considerations**

- Generally only works in LEO; higher orbits have worse geometry
- Knowledge to three unique points (GPS satellites) allows solution for receiver (satellite) position
  - In practice, four measurements are required to solve for receiver clock bias
  - makes four total unknowns of tR, X, Y, Z
- Pointing:
  - Site-tracking requires the satellite know where it is
  - Pointing is time-sensitive
  - Internal models (e.g. magnetic field) require absolute time

*Pictured:* NovAtel OEM719, <https://docs.novatel.com/OEM7/Content/Core_Installation/OEM7700_Overview.htm>

## 11. Design/Build of Sensors

- Not practical to build sensors at the sense-element level
  - e.g. don't build your own magnetometer
- System design is identifying COTS parts (e.g. a magnetometer IC) that meets requirements and then doing the design to integrate it
  - You may still be integrating several low level COTS parts into a single useful sensor
    - E.g. a photodiode pyramid is your design using COTS photodiodes
- Star trackers are quite complex for baffle design and algorithm development but student teams have succeeded before
  - COTS solutions are expensive
  - References exist for the algorithms
  - Do not underestimate this development effort
    - Start work as early as possible to identify actual scope/resourcing to assess feasibility

**If you can buy, buy!** Generally, trying to reduce ADCS costs by adding risk of building it yourself does not work out.

## 12. Actuators

*(Section divider.)*

## 13. Passive Magnetics

**What does it do?**

- Permanent magnet(s) that will align with Earth's magnetic field (e.g. a compass needle)
  - Requires some material that exhibits magnetic hysteresis to provide damping of oscillations about the magnetic field vector
- Applies an external torque; it does change net system angular momentum

**Why is it used?**

- Extremely simple attitude control (passive) if rough magnetic alignment is sufficient

**Other Considerations**

- Provides coarse alignment of bar magnet axis to local field (~15°) and no control of roll about that axis
  - Suitable for some magnetic field measurement missions (e.g. CSSWE mission)

## 14. Torque Coils/Rods

**What does it do?**

- Electromagnets that interact with Earth's magnetic field to produce a torque
  - Typically, three on spacecraft's orthogonal axes to provide best actuation
- Same fundamental physics as the bar magnet but vehicle can actively control field strength and direction

**Why is it used?**

- Can achieve modest pointing accuracy
- Very useful for reducing stored momentum (reaction wheels or during a tumble)
  - Very common to have reaction wheels AND torque rods as a combination set of actuators

**Other Considerations**

- Coils have an air-core
- Rods have a high-permittivity metallic core
  - Rods produce a stronger field for the same number of turns and loop area (vs. coil) but are more complex and less flexible
  - Rods require degaussing
- Fairly coarse control (~5°) possible under the right circumstances
  - Control authority dependent on applied current and local field strength
  - Is not three-axis control since there is still no means of producing torque about the local magnetic field

*Pictured:* CubeSpace CubeTorquer, <https://www.cubespace.co.za/products/gen-2/actuators/cubetorquer/>

## 15. Reaction Wheels

**What does it do?**

- Generate torque by changing speed of a flywheel
  - Accelerating the flywheel in one direction accelerates the vehicle the opposite direction

**Why is it used?**

- High accuracy pointing of a spacecraft
  - ~Tens of arcsec if well balanced

**Other Considerations**

- Typically biased away from zero spin speed
  - Reaction wheel failure mode is typically bearing failure at zero speed crossings
- RWs produce "internal" torque so no change on net system angular momentum
  - Will eventually "saturate" if compensating for an external torque, requires some means (e.g. torque rods) to produce an external torque for the wheels to torque against to reduce flywheel speed
- Three (orthogonal) reaction wheels provide full coverage, four allows redundancy as well as nonzero-speed zero-net-momentum configurations for higher agility

*Pictured:* NanoAvionics RW Cluster, <https://nanoavionics.com/cubesat-components/cubesat-reaction-wheels-control-system-satbus-4rw/>

## 16. Reaction Wheels Physics

*(Video still: the CUBLI project at ETH Zurich, a cube balancing on its corner using internal reaction wheels. <https://www.youtube.com/watch?v=n_6p-1J551Y>)*

## 17. Control Moment Gyros

**What does it do?**

- Generate torque by gimballing a flywheel
  - Changing the spin axis, and therefore the angular momentum vector, of the flywheel produces a gyroscopic torque

**Why is it used?**

- High accuracy (similar to reaction wheels), high torque capability
  - Allows for very fast slews or higher inertia systems

**Other Considerations**

- Saturation behavior is alignment of all CMG axes
- Highest control authority of any actuator but also most complicated
  - Very few smallsat-scale options exist
- Conventional CMGs maintain a fixed flywheel speed, variable-speed CMGs can operate in a reaction wheel mode for singularity avoidance

*Pictured:* TensorTech CMG, <https://tensortech.com.tw/reaction-sphere/>

## 18. Thrusters

**What does it do?**

- Use thrust to maneuver spacecraft (attitude and/or orbit)
  - e.g. Space shuttle

**Why is it used?**

- Chemical propulsion
  - High thrust, low efficiency
- Electrical propulsion
  - Low thrust, high efficiency
- Can be dual use for orbit and attitude maintenance

**Other Considerations**

- While it is technically possible to use thrusters for attitude control this is not recommended
  - Current state of SmallSat thrusters generally do not have enough reliability
  - Limited mission life due to propellant usage
  - Relatively coarse control since minimum impulse bit may be large and firing-to-firing consistency may be poor
  - Even large spacecraft only use ADCS thrusters for momentum management and stick with reaction wheels for primary control

*Pictured:* Enpulsion Nano, <https://www.enpulsion.com/order/>

## 19. Design/Build of Actuators

- Magnetic coils/rods are easy to make
  - Easy mechanical design for the structure, equations for determining loop count/area are well understood
  - Need some winding jig
- Reaction wheels are tricky but possible
  - Flywheel has to be carefully balanced
  - Bearing design must survive vibe
  - Student teams have built and tested wheels before, though flight data is limited
  - Decent COTS options for reasonable costs (for student resources)
- Don't even think about building CMGs or thrusters (at least not for primary ADCS)
- All active control requires control algorithms / software. Design can be easy but the design, coding, testing, verification is a significant work effort and is challenging for grad students let alone undergraduates
  - Be careful here. Its possible; but staff it correctly!

## 20. Requirements

*(Section divider.)*

## 21. Requirements Overview

- Fundamental ADCS parameters
  - Absolute pointing accuracy: "How well it is pointed?"
  - Absolute pointing knowledge: "How well do we know how it is pointed?"
  - Pointing Stability: "How well does it stay on target?"
  - Jitter: "How shaky is it?"
  - Slew Rate: "How fast can it point to a new target?"

## 22. Requirement Drivers

- Drivers:
  - Payload
    - Pointing accuracy to put sensor on target
    - Pointing knowledge to characterize response and geolocate data
    - Optics will have a maximum allowable jitter (preventing blurring)
  - Communications
    - Pointing requirements for comm system to ground station can vary (S-band vs UHF)
      - May drive pointing if laser comm
    - Tracking a ground site requires up to ~1.5°/s slew rate (ISS orbit)
    - Nadir-track is 0.06°/s slew rate
  - Power
    - Need to point solar panels at the sun
    - Solar arrays generate less power as incidence angle decreases
    - Very coarse requirement (3% loss at 14° error)
  - CONOPS
    - Sufficient slew rate to allow switching modes in acceptable time
    - Need to factor in detumble and the time that takes / how often will it occur

## 23. Requirement Flow

- Determine top level drivers
  - "I need to point the camera to within 0.5° of the target"
- Determine derived requirements
  - e.g. Error stack to realize 0.5° absolute pointing might allocate 0.1° for knowledge
- Sanity check values
  - Do solutions that meet those requirements exist? Can existing components be stitched together with new code to meet requirements?
- Check other subsystem / system needs for pointing
  - Fields of view
  - Alternate pointing requirements, ADCS power draw vs. generation, etc.

**Important:** write requirements for either what your mission needs & see which sensor fits that need OR reduce mission capability to fit performance.

## 24. Analyses and Testing

*(Section divider.)*

## 25. First Checks

- Pointing budget
  - What do you actually need? What system gets you there?
  - See CarSat pointing budget example from Mission Design Course as an example
- Disturbance torques
  - Check magnitude of solar radiation pressure, atmospheric drag, residual magnetic, and gravity gradient torques to available control authority
    - If constant relative direction, then need to be able to correct with momentum management system
    - If periodic, then need sufficient reaction wheel momentum capacity to buffer (it'll balance out)

## 26. Simulation

- Very, very hard to know how ADCS will work through ground testing alone. Simulations will help determine system performance even if its an integrated COTS
- Expect attitude knowledge to be 5-10x better than your control capability
  - Because your knowledge is an input to the control, you can never control better than the knowledge
- Generally Runge-Kutta methods (RK4 or RK45) are good enough for your attitude dynamics and control integration
  - Orbit dynamics and attitude dynamics happen on different time scales. This can cause an integrator issue where the time step is driven to be very small increasing run-time for simulations.
  - To improve do a run-time analysis of computationally expensive areas of the code, parallel processing (matlab has toolboxes for this), and look into "integration stiffness" which might give ideas on fixes

## 27. Tests

**Polarity**

- Verifying that all sensor and actuator polarities are correct is absolutely critical
  - Reaction wheels spin the correct direction, torque rod fields are the expected polarity, gyroscope axes match vehicle axes, etc.
  - Incorrect polarities are like riding your bike with your hands crossed
- Verifying all sensors are responding with reasonable values
  - Verifies that sensors aren't damage or doesn't have interference with other components

**Hardware in the Loop**

- COTS systems have simulations to help with verification of hardware functionality (e.g. RDP for BCT's XACT) but are not an end-to-end test

## 28. Further Considerations

*(Section divider.)*

## 29. Error Stack Up

- System level requirements specify overall performance, not how to achieve it
  - Subsystem requirements must allocate portions of the overall requirement to the contributing pieces, e.g. sensor level specification, alignment tolerances, etc
  - Analysis must show that the combined uncertainty of the contributing factors meets the requirement
  - Tightly coupled design process between hardware and software
    - Final system performance is what the control law and attitude determination filter are able to do with the provided hardware specifications
- Stackup Example: Pointing a payload image sensor at a specified target
  - Base knowledge error: How accurate the star tracker is
  - Alignment error: How well the relative orientation of the star tracker image sensor and payload image sensor is known
  - Thermal shift: How much the relative orientation changes as a function of temperature
  - Filter performance: What the final state estimate accuracy is given measurement accuracy and density

## 30. Keep outs

- Many instruments will have keepouts
  - Specified region around an object, typically the sun, that the instrument should not look at
  - May be for functionality (e.g. not washing out stars) or damage (e.g. not overheating the sensor)
- There is no way to unconditionally guarantee keepouts on a CubeSat
  - ADCS operations within a specified timer of launch vehicle separation are typically prohibited
  - CubeSat ADCS are not sufficiently reliable to guarantee pointing at all times
  - → Avoid sensors that will be damaged if keepouts are violated, if they are absolutely required then they need a shutter
    - Some probability argument on how long it takes to damage the sensor, e.g. highly unlikely to end up staring directly at the sun for an extended period

## 31. Vehicle Geometry

- How the various satellite components are aligned has major ADCS implications
  - Where does satisfying one pointing constraint place the other components?
    - Does pointing the solar panels at the sun put the star tracker on the earth? Does pointing the antenna and the payload generate no power?
    - Do mode transitions require significant maneuvers?

## 32. How ADCS impact EPS/CDH

- ADCS has a significant impact on power system design and the power budget
  - ADCS is generally one of the higher-power subsystems
  - Actuators have highly variable power draws, e.g. reaction wheels may typically draw 400mA but can draw up to 2A for ~10s
    - This requires a power system that is efficient at normal draw but can handle spikes
    - How this is modelled in the power budget can significantly affect results
    - Control scheme will affect this profile
- ADCS produces a lot of data, at least internally
  - Effectively debugging the system may require high-rate telemetry from a number of sensors
  - Consideration for orbit, having a high-rate EGSE interface can be very nice

## 33. Magnetics

- While intentional magnetic control works it is also possible to inadvertently create magnetic torques
  - Current flow on solar panels creates a field, commonly see string directions alternated to balance this
  - Steels are variously magnetizable, 316 and A286 are preferred over 18-8 and alloy steels for fasteners for this reason (as well as strength)
  - Vehicle (or certain subassemblies) may be "degaussed" to reduce residual magnetic fields

## 34. Conclusion

- ADCS is very complex and requires detailed system knowledge but also detailed electrical, algorithm, and dynamics
  - If you can buy systems that already work, do this! (Reduce risk by using money)
- You always need to understand what pointing is required of your system
- You always need to understand what pointing constraints (FOVs) exist
- You always need to do some level of testing to ensure the system is working together
  - Polarity, sensor functionality, etc.
- Seek out UNP, professors, etc. for help here!
