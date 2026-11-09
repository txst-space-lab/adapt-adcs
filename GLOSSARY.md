# ADAPT ADCS

The attitude determination and control subsystem of ADAPT, a 6U CubeSat in very low Earth orbit that flies a classical attitude controller and an ML attitude controller side by side and compares them.

## Language

### Mission and subsystems

**ADCS**:
The subsystem that determines and controls the spacecraft's attitude: its sensors, actuators, estimator and controllers.
_Avoid_: GNC, attitude system

**Payload**:
The ADAPT subsystem (and team) that owns the ML controller and the experiment it runs.
_Avoid_: ML team

**Payload computer**:
The separate on-board computer that runs the ML controller. It never commands actuators directly.
_Avoid_: payload OBC, GPU, Jetson

**Flight unit**:
A piece of hardware intended to fly on ADAPT, as opposed to anything built only for the lab.
_Avoid_: real hardware, flight model

### Hardware

**Reaction wheel**:
A motor-driven flywheel that changes the spacecraft's attitude by exchanging momentum with it.
_Avoid_: reaction control wheel, RCW, momentum wheel

**Magnetorquer**:
A coil that pushes against Earth's magnetic field, used mainly to dump momentum built up in the reaction wheels.
_Avoid_: torque rod, magnetic torquer

**Desaturation**:
An event in which the magnetorquers remove stored momentum from the reaction wheels.
_Avoid_: momentum dump, desat

### Control and estimation

**State estimator**:
The single on-board estimate of attitude, rates and related state, with its uncertainty, that both controllers act on.
_Avoid_: Kalman filter, KF, UKF

**Classical controller**:
The conventional attitude control law that serves as the experiment's baseline.
_Avoid_: PID controller, baseline controller

**ML controller**:
The learned attitude control policy that flies on the payload computer, distilled from the teacher model.
_Avoid_: AI controller, RL controller, student

**Teacher model**:
The large policy trained on the ground with full knowledge of the simulated environment, used only to train the student model.
_Avoid_: expert policy

**Student model**:
The small, noise-tolerant policy distilled from the teacher model; the ML controller is its flight build.
_Avoid_: distilled model

**Safety gate**:
The flight software check on the primary computer that accepts or rejects every ML controller command before it reaches an actuator.
_Avoid_: veto, filter, guard

**Mode**:
One of the spacecraft's ADCS operating states: Safe, Classical, ML or Storm.
_Avoid_: state, phase

### Experiment

**Orbital regime**:
A combination of space-weather condition (quiet or storm) and lighting (sunlight or eclipse) under which the controllers are compared.
_Avoid_: scenario, environment

**Pointing Performance Index**:
The ratio of the ML controller's pointing error score to the classical controller's.
_Avoid_: pointing score

**Actuator Performance Index**:
The ratio of the ML controller's actuator cost score (power, torque, accumulated momentum, desaturations) to the classical controller's.
_Avoid_: bare "API", which reads as application programming interface; efficiency score

### Simulation and test

**Digital Simulation**:
The software model of the spacecraft and its environment used to design, train and verify the controllers.
_Avoid_: digital twin, sim environment

**Physical Simulator**:
The lab bench of real sensors and actuators that runs ADCS software in the loop with the Digital Simulation. It is not built to flight accuracy.
_Avoid_: testbed, HIL rig, physical model

**Reaction Wheel Demonstrator**:
A low-cost, 3D-printed CubeSat mock-up turned by a lab-built reaction wheel, and the first build of the Physical Simulator. It is never a flight unit.
_Avoid_: RCW demo, demo CubeSat, physical model
