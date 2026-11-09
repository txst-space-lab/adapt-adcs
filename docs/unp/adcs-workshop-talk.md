# UNP ADCS workshop talk

A UNP instructor presented the [workshop deck](adcs-workshop-afrl-2023-2775.md) at a UNP summer workshop, followed by questions. These notes record points made in the talk that aren't in the slides. Slide numbers refer to the workshop deck.

**Recording:** [YouTube](https://www.youtube.com/watch?v=zWqOiQWSQBg)

## Points made in the talk that aren't in the slides

### Determination and control (slides 2–4)

- Spacecraft usually run several controllers, not one: B-dot for detumble out of the deployer, PID for ordinary pointing, and LQR, Lyapunov or other methods for higher-accuracy pointing, switching by scenario.
- The control loop typically runs at 1 to 10 Hz, depending on the accuracy needed.

### Sensors (slides 6–11)

- **Magnetometers** see every field on board, including power switching and conversion noise. Science missions that measure magnetic fields often put them on a boom.
- **Gyroscopes:** "no attitude system is basically complete without it." The gyro propagates motion between other measurements.
- **Sun sensors** detect the brightest object, not necessarily the sun; thresholds tell them apart. They're mainly for safe mode: find the sun, point the panels, make power.
- **Star trackers** lose lock when slewing too fast because the stars blur. A clean AIT process matters because contamination fouls the optics.
- **GPS receivers:**
  - Terrestrial receivers are locked against orbital speeds. Flying one requires getting it unlocked, which takes licensing work but is doable at a university.
  - Datasheet accuracy of 2–5 m becomes 10–20 m in orbit. Use about 20 m in pointing budgets.
  - A cold first fix can take 10–30 minutes.
  - Point the antenna toward zenith, where the GPS satellites are.
- **Build or buy:** students can gain the expertise to build ADCS hardware, but "by the time you get it you're about to graduate."

### Sensor Q&A

- **One sensor isn't enough.** A single vector, such as the sun direction, leaves a full 360° of ambiguity in roll about it. Knowing attitude takes two independent measurements. Three-axis stabilised spacecraft usually fly sun sensors, rate gyros and magnetometers together, plus star trackers if they need better pointing.
- **Solar panels as sun sensors** are rare. It puts sensing in line with power generation, and one panel plane can't find a sun that is behind it. Separate photodiodes are simpler.
- **Coarse vs fine sun sensors.** Coarse sensors are for safety: find the bright object anywhere in the field of view and point the panels. Fine sun sensors use better-characterised photodiodes and more estimation, and are "accurate to 5 to 10°, maybe a little better."
- **Power falls off as the cosine of the sun angle,** so 10° off-sun barely shows in power generation. (The talk said power halves at 30°; cos 60° = 0.5, so it halves at 60°.)
- **In eclipse without a star tracker,** use the magnetometer and gyro to propagate. Error builds up, probably to "a couple of extra degrees." A star tracker stays accurate throughout unless it is blinded.

### Actuators (slides 13–19)

- **Passive magnets** suit 1U–2U spacecraft. In a high-inclination orbit the magnet follows the field around and the spacecraft may slowly tumble once per orbit.
- **Torque rods are underactuated.** Torque is the cross product with Earth's field, so there is no torque about the field line and only two-axis control at any moment. Controllability shifts as the field direction changes around the orbit.
- **Torque rods for momentum dumping:** most three-axis LEO spacecraft use them continuously, which gives wheel-and-rod CubeSats effectively unlimited momentum capacity. Above mid-Earth orbit the field is too weak, and thrusters take over.
- **Reaction wheels:**
  - Three wheels is the usual minimum, and a fourth adds redundancy and capability. They are fairly expensive, but they can make a 6U or 12U point very well.
  - Wheels are biased to around ±100 RPM rather than held at zero, to avoid odd dynamics at the zero crossing.
  - Wheels wear out, but CubeSat lifetimes have usually been shorter than wheel lifetimes, so they have not generally been treated as limited-life items. Kepler lost a wheel after years of operation.
- **CMGs** suit very fast "snap" maneuvers. The speaker didn't know of any on the market at CubeSat scale.
- **Thrusters:** dumping momentum from all wheels needs at least three thrusters to cover all three axes. Propulsion is still an emerging technology: cleanliness when filling fluids matters, and valves get fouled. Relying on thrusters "may be a major mission risk."
- **Building actuators:** torque rods take a few attempts to get right but are cheap. Reaction wheels need good machinists and a good machine shop.
- **ADCS software** usually takes graduate-level work or more. It needs "a pretty rigorous staffing program to make sure that it doesn't stagnate."

### Requirements (slides 21–23)

- **Pointing accuracy and pointing knowledge are separate requirements.** Some missions need to know precisely where they point but don't need to point tightly.
- **Jitter** includes wheel imbalance (wheels are never perfectly balanced) and flexible modes from deployed panels or booms. If wheel or slew rates excite those modes, the spacecraft "can start to really bounce around."
- **Comm band tolerance:** UHF is very forgiving, S-band pretty forgiving, then X-band and Ka-band progressively less.
- **The 3% loss at 14°** (slide 22) is the cosine of the sun angle.
- **Every small satellite tumbles at least once,** at deployment. Resets and other faults may cause more tumbles, so the design must tolerate them.
- **Requirement flow** loops between finding solutions and revising requirements. Check that GPS antennas face space and that antenna fields of view line up.

### Analysis and testing (slides 25–27)

- **Atmospheric drag is the dominant disturbance below about 800 km.** Solar radiation pressure takes over above 800–900 km and dominates at GEO and beyond. Gravity gradient matters little for small satellites. Residual magnetic torque matters close to Earth.
- **Drag torque depends on symmetry and attitude.** A more symmetric body relative to the velocity vector feels less of it. LEO drag forces are around tens of micronewtons and usually manageable.
- **Control authority must exceed the sum of disturbance torques.** A held attitude builds momentum steadily. Disturbances that alternate as the attitude changes tend to cancel out.
- **Knowledge must be 5–10× better than control.** Pointing to 10° needs about 1–2° of knowledge.
- **Polarity tests** check both the wiring and the software sign conventions: is commanded positive actually positive?
- **Sanity checks can be crude.** Does the magnetometer read roughly Earth's field in roughly the right direction? Take the sun sensors outside, or shine a flashlight on them.
- **COTS simulators** help debug the system and plan CONOPS, even though they are not end-to-end tests.

### Further considerations (slides 29–33)

- **Error stack-up:** software can estimate and remove some biases, such as alignment and thermal errors, which tightens the budget.
- **Keep-outs:** prefer equipment for which a keep-out is functional, not damaging. Expect a reset-induced tumble at some point, which can point anything anywhere; lens covers help. Place the star tracker so it faces away from the sun during precise operations.
- **Power numbers** (400 mA typical, 2 A peak) come from the presenters' own experience. Peaks during slews last no more than a few tens of seconds.
- **Data:** ADCS can be a large data driver for CDH. The amount downlinked can be reduced.
- **Solar panel wiring:** avoid current loops in panel layout, which turn a lit panel into a magnet.

### Closing Q&A

- **Two-axis (underactuated) control** has no hidden gotchas beyond not being able to point anywhere at any time. Lay out antenna lobes so the null doesn't always face Earth in typical attitudes.
- **Gravity-gradient booms** are uncommon on CubeSats. The stowed boom takes too much of the small volume, and that volume is then wasted after deployment.
- **Kalman filtering** propagates a physics model, differences it with sensor measurements, and minimises the error between them to update the estimate. It does this every cycle. It assumes Gaussian noise, which is usually wrong. Bayesian and other estimators drop that assumption and handle less linear or faster-changing dynamics, but Kalman filtering is generally fine for student attitude determination.
