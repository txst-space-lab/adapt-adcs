# How lab-built and open-source star trackers are tested in a lab

Research for [#16](https://github.com/txst-space-lab/adapt-adcs/issues/16), on the Physical Simulator map ([#5](https://github.com/txst-space-lab/adapt-adcs/issues/5)).

**Question.** How are lab-built and open-source star trackers tested in a lab, and what would each method take for us to set up?

**Decision it feeds.** Does the Physical Simulator test the star tracker physically (a star-field display through a collimator, or a night-sky test), or is the Digital Simulation enough?

## Short answer

Groups test star trackers at three levels, and most use more than one:

1. **Digital-only.** Feed the software either a modelled attitude measurement, synthetic star images, or an emulated sensor message. This costs nothing but time and is the only level that runs closed-loop with no optics.
2. **Star-field stimulator.** A small high-resolution screen shows a rendered star field, and a collimating lens puts it at infinity in front of the tracker's camera. University groups have built these for a few hundred dollars and used them on both lab-built trackers and a COTS CubeSat tracker. They are good enough for star identification and functional closed-loop tests, but they are less accurate than the real sky.
3. **Night-sky test.** Point the tracker at the real sky from the ground. This is the standard way to validate a lab-built tracker's accuracy and the reference that stimulators are checked against. It costs almost nothing, but it can't run in a closed loop and depends on the weather.

**For ADAPT:** the mission is likely to fly a COTS ADCS with an integrated star tracker (the placeholder is the XACT-50, slide 25), so the Physical Simulator should treat the star tracker as **digital-only**: model it in the Digital Simulation and emulate its output at the flight interface. Build a display-and-collimator stimulator, and do night-sky tests, only if the team builds its own tracker (for example a lab tracker riding on a turntable) or wants to exercise an image pipeline. See [Recommendation](#recommendation-for-adapt).

## ADAPT context

- The mission concept lists "Star Trackers (2+)" as required sensors, and the star tracker supplies the estimated attitude quaternion for the pointing metrics ([deck slide 16](../../docs/mission/overview/adapt-detail-overview-115.md)).
- The spacecraft overview calls for a COTS ADCS, and the CAD layout labels an "XACT 50 ADCS" ([deck slides 24–25](../../docs/mission/overview/adapt-detail-overview-115.md)). These component picks are placeholders for budgeting ([2026-11-05 mission overview](../../docs/mission/meetings/2026-11-05-mission-overview.md)).
- Both controllers point the star tracker at the same star or constellation so their pointing can be compared ([2026-11-05 mission overview](../../docs/mission/meetings/2026-11-05-mission-overview.md), slide 21).
- The mission will likely buy a precise COTS ADCS and star tracker, and building our own would be rougher and slower ([2026-11-06 ADCS team sync](../../docs/mission/meetings/2026-11-06-adcs-team-sync.md)).
- The XACT-50 is listed with 1–2 star trackers ([NASA Small Spacecraft Technology State of the Art, GNC chapter](https://www.nasa.gov/smallsat-institute/sst-soa/guidance-navigation-and-control)). Blue Canyon's Nano Star Tracker has an on-board catalog of more than 23,000 stars and lost-in-space star identification ([Blue Canyon IPPW 2018 talk](https://colorado.edu/event/ippw2018/sites/default/files/attached-files/15_-_ippw_adcs_-_dan_hegel.pdf)).

## Lab-built and open-source star trackers to know about

These are what a "lab-built star tracker" usually looks like: a board computer, a COTS camera and an open-source plate solver.

| Project | Hardware | How it was tested | Source |
|---|---|---|---|
| SOST (Univ. of Chile SPEL) | Raspberry Pi 3 B+ and its camera in the paper; the flight payload is a Pi Zero W with a Pi camera V2.1. Source Extractor and Match. GPL-3.0. | Night-sky pictures from the ground, thermal-vacuum chamber, and images from working satellites. Sub-arcminute boresight precision, 1–3 solutions per minute. Used on the SUCHAI-2, SUCHAI-3 and PlantSAT swarm, launched 1 April 2022. | [IEEE Access 2020 (ADS)](https://ui.adsabs.harvard.edu/abs/2020IEEEA...8p6320G), [GitHub](https://github.com/spel-uchile/Star_Tracker) |
| OreSat star tracker (Portland State, PSAS) | Octavo OSD335x Linux card, onsemi AR0134 colour CMOS camera (1280×960), 16 mm focal length. Solver was openstartracker until 2024 and is now LOST. Software GPL-3.0. | Night-sky field tests at observatories, astrometry.net calibration, regression tests on recorded star images, mock camera. Solves at about 1 Hz, 48 arcsec per pixel, "within 2 degrees" of offline calibration. Under $500 in parts. Flew on OreSat0 (2022); no published on-orbit results found. | [software](https://github.com/oresat/oresat-star-tracker-software), [hardware](https://github.com/oresat/oresat-star-tracker-hardware), [poster](https://spacegrant.oregonstate.edu/sites/spacegrant.oregonstate.edu/files/2022-11/nair_aakarsh_psu_internship_poster.pdf) |
| StarberrySense (Indian Institute of Astrophysics) | Raspberry Pi Zero, Pi camera, off-the-shelf lens | Assembled and calibrated on the ground, then flown on the PSLV C-55 PS4 orbital platform | [Part I, JATIS 2022 (arXiv)](https://arxiv.org/abs/2207.03087), [Part II, JATIS 2024](https://electronicimaging.spiedigitallibrary.org/journals/Journal-of-Astronomical-Telescopes-Instruments-and-Systems/volume-10/issue-02/026002/Low-cost-Raspberry-Pi-star-sensor-for-small-satellites-II/10.1117/1.JATIS.10.2.026002.full) |
| openstartracker (UB Nanosat Lab) | Any camera with set exposure and gain, Linux computer; MIT licence. Python front end over a dependency-free C++ back end (BEAST-2). Built for UB's GLADOS and SORA nanosatellites. | Calibrate from 3–10 of your own night-sky images via astrometry.net, then run a scripted "ESA test" (target score above 70). Ships sample night-sky images. The project site gives no accuracy or speed figures; it points to a SmallSat 2017 paper. | [GitHub](https://github.com/UBNanosatLab/openstartracker), [openstartracker.org](https://openstartracker.org/) |
| tetra3 (ESA) and Cedar-Solve | Plate solver only; about 10 ms per solve, about 10 arcsec accuracy; suggests a ~10° field of view and stars to magnitude 7. Apache 2.0. Cedar-Solve is a compatible fork. | Ships real images from a FLIR Blackfly S camera as test data | [tetra3](https://github.com/esa/tetra3), [Cedar-Solve](https://github.com/oakamil/cedar-solve) |
| NASA JSC and RPI open-source algorithms | Several single-board computers and COTS cameras | Run across board, OS and camera combinations; released with a setup guide | [NTRS 20200001376](https://ntrs.nasa.gov/citations/20200001376) |
| UIUC LASSI CubeSat tracker | Monochrome global-shutter 640×480 camera, 16 mm f/1.2 lens (13.8°×10.3° field of view) | Synthetic images, a display-and-collimator stimulator and night-sky tests (details under each method below) | [arXiv 2404.19108](https://arxiv.org/html/2404.19108) |

### OreSat (Portland State Aerospace Society)

OreSat is the closest match to a student lab building its own tracker, and it is fully open.

**How it's built.**

- The hardware is one of OreSat's standard cards: an Octavo OSD3358 "Linux box on a chip" with a custom camera board around an onsemi AR013x sensor ([hardware README](https://github.com/oresat/oresat-star-tracker-hardware)). The internship poster names the AR0134, 1280×960 pixels ([Nair poster](https://spacegrant.oregonstate.edu/sites/spacegrant.oregonstate.edu/files/2022-11/nair_aakarsh_psu_internship_poster.pdf)). OreSat lists the OreSat0 card as "Star tracker (v1.2)" with an AR0134 colour CMOS camera running openstartracker ([OreSat0 page](https://www.oresat.org/satellites/oresat0)). The boards are KiCad files; the hardware repo has no licence file.
- The lens isn't documented in either repo. The solver config sets a 16 mm focal length and 3.75 µm pixels ([`_lost_core.cpp`](https://github.com/oresat/oresat-star-tracker-software/blob/master/oresat_star_tracker/_lost_core.cpp)). That gives about 48 arcsec per pixel and about 17°×13° field of view (our arithmetic), which matches the poster's 48 arcsec per pixel.
- The software runs as a CANopen node on the satellite bus and publishes the attitude quaternion over CAN ([software repo](https://github.com/oresat/oresat-star-tracker-software)). It is GPL-3.0.
- **Solver.** Until April 2024 it was a modified openstartracker ("BEAST"). It now uses LOST from the University of Washington's Husky Satellite Lab, built in as a C++ module, using LOST's pyramid star identification, Davenport q-method attitude and a bundled bright-star catalogue ([README](https://github.com/oresat/oresat-star-tracker-software), [commit history](https://github.com/oresat/oresat-star-tracker-software/commits/master), [LOST](https://github.com/UWCubeSat/lost)). The LOST repo has no licence file.

**How it was tested.**

- **Night sky.** The poster reports field tests at Pine Mountain Observatory and the Pacific Northwest Regional Observatory, with astrometry.net used to debug the configuration. It also lists the OMSI Planetarium among the test sites but doesn't say what was done there ([Nair poster](https://spacegrant.oregonstate.edu/sites/spacegrant.oregonstate.edu/files/2022-11/nair_aakarsh_psu_internship_poster.pdf)).
- **Calibration.** Camera parameters (pixel scale, image variance, position variance) came from an astrometry.net blind calibration, the openstartracker workflow. A median image acted as a mask for fixed camera artefacts (poster).
- **Results.** About 1 solve per second at 48 arcsec per pixel. Online solutions were "within 2 degrees" of the offline blind calibration. Parts cost under $500, against "upwards of $10,000" for a commercial tracker (poster). That 2° figure is a field check, not a flight accuracy.
- **Recorded-image regression tests.** From 2018 to 2024 the repo carried sets of real star images at two exposures (1000 and 2500) with their calibration files. A unit test solved each image and checked RA, Dec and roll against the last known solution, with a 10-second time limit per image ([`tests/test_solver.py` before LOST](https://github.com/oresat/oresat-star-tracker-software/blob/e59e07eeee6405a6952325bc842132e505a3e6bc/tests/test_solver.py)). These were deleted with the switch to LOST.
- **Now.** The repo keeps a few star images, and the mock camera returns a real star field so the whole service runs without hardware (`-m camera`). CI runs formatting, linting, type checks and a package build, but it does not run the unit tests, and the one remaining test needs the camera device ([tests workflow](https://github.com/oresat/oresat-star-tracker-software/blob/master/.github/workflows/tests.yaml), [`tests/test_camera.py`](https://github.com/oresat/oresat-star-tracker-software/blob/master/tests/test_camera.py)). LOST itself can generate synthetic star images for a given attitude ([LOST README](https://github.com/UWCubeSat/lost)).
- **No stimulator.** Neither repo has a star-field display or collimator. The hardware repo has early camera scripts for a G-code gimbal, but nothing ties them to star-field tests ([`camera_testing`](https://github.com/oresat/oresat-star-tracker-hardware/tree/master/camera_testing)).
- **Their ADCS simulation is digital-only for the tracker.** OreSat's ADCS hardware-in-the-loop setup models the star tracker with Basilisk's `starTracker` module and an emulated slower update rate ([oresat-adcs-software HWIL](https://github.com/oresat/oresat-adcs-software/tree/master/HWIL)). This is our Method 1a.

**Flight.** A star tracker card flew on OreSat0, launched 15 March 2022 on Astra LV0009 ([OreSat0 page](https://www.oresat.org/satellites/oresat0)). OreSat's mission configs list a star tracker card for OreSat0, OreSat0.5 and OreSat1 ([oresat-configs](https://github.com/oresat/oresat-configs)). OreSat0.5 launched 16 August 2024 on Transporter-11 to demonstrate the ADCS and the Cirrus Flux Camera ([OreSat0.5 page](https://www.oresat.org/satellites/oresat0-5)). We found no published on-orbit star tracker results for either satellite.

## Method 1: Digital-only

There are three ways to test without any optics. They exercise different parts of the chain.

### 1a. Attitude-level sensor model

**What it tests.** The state estimator and controllers, given star tracker measurements with realistic noise, bias and dropouts. It doesn't test star detection or identification.

**How.** Basilisk's `starTracker` module takes the true spacecraft state and outputs a sensed quaternion message, corrupted by a Gauss-Markov error model set by a noise covariance (`PMatrix`), a propagation matrix (`AMatrix`) and walk bounds ([Basilisk starTracker docs](https://hanspeterschaub.info/basilisk/Documentation/simulation/sensors/starTracker/starTracker.html)). OreSat's ADCS hardware-in-the-loop setup uses this same module for its star tracker ([oresat-adcs-software](https://github.com/oresat/oresat-adcs-software/tree/master/HWIL)), even though OreSat builds its own optical tracker. The noise setup has changed between Basilisk versions, so check the unit test for the version in use ([2.6.0 test](https://hanspeterschaub.info/bskOlderDocs/bsk_2_6_0/_modules/test_star_tracker.html), [1.3.2 test](https://hanspeterschaub.info/bskOlderDocs/bsk_1_3_2/_modules/test_star_tracker.html)).

**Equipment and cost.** None beyond the Digital Simulation.

**Effort.** Low, and it's needed anyway for the Digital Simulation (estimate).

### 1b. Interface-level emulation (sensor emulator on the bench)

**What it tests.** The flight software's handling of the real star tracker message over the real bus: parsing, timing, faults and invalid-solution flags. This is how a COTS tracker enters a closed loop on a bench without optics.

**How.** A bench computer runs the Digital Simulation and answers on the physical interface as the sensor would. The USAT-I CubeSat team tested control laws against NASA's 42 simulator first, then tested "the ACS software … with the real serial interfaces against an emulator which translates them into the input/output variables of the 42 simulator", and only then against each piece of hardware in a flat-sat ([Hanisch and Servidia 2025, Metascience in Aerospace](https://www.aimspress.com/aimspress-data/mina/2025/3/PDF/mina-02-02-003.pdf)).

**Equipment and cost.** A microcontroller or USB-serial adapter and the vendor's interface control document. For a COTS ADCS, the vendor may supply simulation support: an Air Force SBIR article on Blue Canyon says "software is also available to support simulation, system integration and customization of ADCS functionality" ([Wright-Patterson AFB news](https://www.wpafb.af.mil/News/Article-Display/Article/819164/air-force-small-business-partnership-improves-control-of-mini-space-satellites/)). Whether that covers the XACT-50's tracker interface needs a question to the vendor.

**Effort.** Medium, and it depends on getting the interface specification (estimate).

### 1c. Image-level injection

**What it tests.** The star detection, centroiding, identification and attitude solution of a lab-built tracker's software, with no camera.

**How.** Render synthetic star images for a known attitude from a catalogue and feed them to the solver. UIUC trained and evaluated its detection method on simulated star images before going to hardware ([arXiv 2404.19108](https://arxiv.org/abs/2404.19108)). A universal verification platform for star trackers generates synthetic sky images with controllable noise and reuses the same test benches in a hardware-in-the-loop configuration ([Sensors 2021, 21(3):907](https://api.crossref.org/works/10.3390/s21030907)). openstartracker's test script can regenerate ESA test data and score the solver ([GitHub](https://github.com/UBNanosatLab/openstartracker)), and tetra3 ships real images for regression tests ([GitHub](https://github.com/esa/tetra3)). LOST generates synthetic images for a given RA, Dec, roll and noise level ([LOST](https://github.com/UWCubeSat/lost)). OreSat regression-tested its solver on recorded star images for six years (see [OreSat](#oresat-portland-state-aerospace-society)).

**Equipment and cost.** None.

**Effort.** Low to medium. It only matters if we write or run our own image pipeline. A COTS tracker computes its own solution inside the unit, so there is nowhere to inject an image (estimate; we found no vendor documentation of an image-injection mode for a COTS CubeSat tracker).

## Method 2: Star-field stimulator (display plus collimator)

**What it tests.** The whole tracker, from camera to quaternion: exposure, detection, identification, attitude output and, if the display is updated live, tracking while the attitude changes. Driven by the Digital Simulation, it puts a real optical tracker in a closed loop.

**How it works.** A screen shows the stars that should be in the tracker's field of view at the simulated attitude. A collimating lens one focal length from the screen makes each pixel look like it's at infinity, so the tracker's camera, focused at infinity, sees a star field. The whole path is covered to keep out stray light.

### Published examples

- **UIUC LASSI (2024–2025).** A 5-inch 1920×1080 IPS monitor, an achromatic collimating lens of 350 mm focal length and the test camera, on posts on an optical rail, under a cover lined with light-absorbing fabric. Stars from Hipparcos down to magnitude 6. They measured the displayed stars at about apparent magnitude 2.35, and the screen's resolution gives on average 12.9 arcseconds of star-vector error. They also varied sensor temperature with a fan and simulated stray light with a flashlight inside the cover ([arXiv 2404.19108](https://arxiv.org/html/2404.19108)). A companion SmallSat 2024 paper calibrated displayed star brightness against night-sky data and found the simulator "precise enough to allow for successful star identification and attitude determination in a closed loop test" ([SmallSat 2024](https://digitalcommons.usu.edu/smallsat/2024/all2024/212)).
- **Stellenbosch, COTS CubeStar (2021).** Star projection software drives a computer monitor, and a commercial CubeSpace CubeStar tracker images it. The projected images had about 2.5 arcseconds of boresight error. The CubeStar's attitude error was below 0.107° for stationary stars and below 0.145° for stars moving at up to 0.25°/s on each axis. The author notes the tracker "performed more accurate under real night sky conditions", but the emulator "allows functional testing under more convenient lab conditions" ([IAC 2021 abstract](https://p.iafastro.directory/iac/paper/id/66263/abstract-pdf/IAC-21,B4,6B,8,x66263.brief.pdf), [MEng thesis](https://scholar.sun.ac.za/items/3b249039-4253-48bb-8df7-cd89ffc7fe7a/full)). This is the clearest published case of a student lab stimulating a COTS CubeSat tracker.
- **Cal Poly (2024).** A small display, a simple in-house camera tracker and a microprocessor that can simulate any star field image. It is built for a stationary tabletop but is small enough to ride a spacecraft dynamics platform for control tests. The thesis notes that "commercial star trackers … are regularly tested with manufacturer provided star field images" ([Haraguchi MS thesis](https://digitalcommons.calpoly.edu/theses/2850)).
- **Cal Poly follow-on (2025).** An all-COTS star tracker and screen-based hardware-in-the-loop bench. After calibrating focal length, principal point and distortion, the mean attitude error was 750 arcseconds, and the main remaining error was uncorrected distortion from the screen and imaging optics ([Iverson MS thesis](https://digitalcommons.calpoly.edu/theses/3046)).
- **Rensselaer (2019).** A master's thesis on designing an in-lab star field simulator, including verifying collimation and star-field rotation against Hipparcos ([RPI thesis](https://dspace.rpi.edu/handle/20.500.13015/2381)).
- **Professional equivalents, for scale.** Airbus STOS, a "miniaturized dynamic planetarium" that runs full attitude control modes with a real star sensor in a closed loop and was used on JUICE ([Airbus](https://www.airbus.com/en/products-services/space/space-equipment/on-ground-solutions/stos-star-tracker-optical-simulator)). Solar MEMS STAR-D, a screen-based stimulator with a 19.5° field of view ([datasheet](https://solar-mems.com/wp-content/uploads/2026/02/Star-D-Simulator.pdf)). MINISTAR, a miniaturized device that mounts on the tracker and projects a synthetic star field through collimating optics ([SPIE 2019](https://proceedings.spiedigitallibrary.org/conference-proceedings-of-spie/11180/111807V/MINISTAR--a-miniaturized-device-for-the-test-of-star/10.1117/12.2536202.full)). JPL also published a miniaturized star tracker stimulator for closed-loop testing of CubeSats ([JGCD 2017 engineering note, citation](https://aminer.org/pub/5c757074f56def97986f293c/miniaturized-star-tracker-stimulator-for-closed-loop-testing-of-cubesats)).

### Equipment and rough cost

| Part | Example | Rough cost |
|---|---|---|
| Small high-resolution display | Waveshare 5.5" 1080×1920 AMOLED, HDMI | $122 ([Waveshare](https://www.waveshare.com/5.5inch-hdmi-amoled.htm)) |
| Collimating lens | Edmund Optics 50 mm diameter, 300 mm focal length achromatic doublet | about $194–231 ([Edmund Optics](https://www.edmundoptics.com/p/50mm-dia-x-300mm-fl-vis-0deg-coated-achromatic-lens-/9636/)) |
| Camera (lab-built tracker only) | Raspberry Pi High Quality Camera (Sony IMX477, C/CS or M12 mount) | see store page ([Raspberry Pi](https://www.raspberrypi.com/products/raspberry-pi-high-quality-camera/)) |
| Rail, posts, light-tight cover | 3D-printed or extruded aluminium; blackout fabric, as UIUC used | under $100 (estimate) |
| Render computer | Any laptop or Raspberry Pi driving HDMI | already in the lab |

Total: roughly **$300–500** of parts beyond what the lab already has (estimate built from the prices above).

### Sizing rule of thumb

With the screen one focal length *f* from the lens, a pixel of pitch *p* spans *p / f* radians, and a screen of width *w* covers about *w / f* radians. For UIUC's 5-inch 1080p screen (≈0.058 mm pixels, ≈111 mm wide) at 350 mm, that is about 34 arcseconds per pixel and about 18° across, just wider than their 13.8° camera (our arithmetic from the [UIUC numbers](https://arxiv.org/html/2404.19108)). Centroiding across several pixels is how they get the reported 12.9 arcsecond average error. Pick *f* so the screen fills the tracker's field of view.

### Limits

- **Accuracy.** Screen resolution, distortion and alignment cap the accuracy below the real sky ([UIUC](https://arxiv.org/html/2404.19108), [Cal Poly 2025](https://digitalcommons.calpoly.edu/theses/3046), [Stellenbosch](https://p.iafastro.directory/iac/paper/id/66263/abstract-pdf/IAC-21,B4,6B,8,x66263.brief.pdf)). This is fine for a Physical Simulator that isn't built to flight accuracy.
- **Brightness.** Displayed stars are bright and squeezed into a small range of magnitudes, so they need calibrating against real-sky data ([UIUC](https://arxiv.org/html/2404.19108), [SmallSat 2024](https://digitalcommons.usu.edu/smallsat/2024/all2024/212)).
- **On a moving platform.** The display and lens must ride with the tracker, so on a turntable or air bearing the stimulator is part of the payload, as Cal Poly suggests ([Haraguchi](https://digitalcommons.calpoly.edu/theses/2850)). The display then shows the simulated attitude, not the platform's real attitude, unless the two are synchronised.
- **COTS flight tracker.** Physical stimulation works on a COTS tracker (Stellenbosch did it with a CubeStar), but it needs the unit in the lab and enough knowledge of its optics. Vendors test their own trackers with their own star field images ([Haraguchi](https://digitalcommons.calpoly.edu/theses/2850)).

**Effort.** A few weeks of student work to build, align and calibrate a static version. Driving it live from the Digital Simulation adds a renderer and timing work (estimate).

## Method 3: Night-sky test

**What it tests.** The real optical chain against the real sky: true star brightness, the lens's true distortion, noise, and an accuracy figure that stimulators are judged against. It doesn't run in a closed loop, and the atmosphere, light pollution and weather get in the way.

**How.** Put the tracker or camera on a tripod, point it near the zenith from a dark site, and record images or video. Get ground truth by plate-solving the same images with an independent solver such as astrometry.net, and compare.

### Published examples

- **UIUC.** The same camera, on a tripod near the zenith with 500 ms exposures, at a forest preserve. A 5-minute baseline video, then 5 minutes with a flashlight as stray light ([arXiv 2404.19108](https://arxiv.org/html/2404.19108)).
- **SOST.** Night-sky pictures from the ground gave sub-arcminute boresight precision, and 97.3% of solutions succeeded ([IEEE Access 2020 (ADS)](https://ui.adsabs.harvard.edu/abs/2020IEEEA...8p6320G)).
- **openstartracker.** Calibration needs only 3–10 night-sky images of different sky regions, and the README suggests uploading images to nova.astrometry.net to check a camera is good enough ([GitHub](https://github.com/UBNanosatLab/openstartracker)).
- **OreSat.** Field tests at two Oregon observatories, with astrometry.net to debug the configuration, gave solutions "within 2 degrees" of the offline calibration at about 1 Hz ([Nair poster](https://spacegrant.oregonstate.edu/sites/spacegrant.oregonstate.edu/files/2022-11/nair_aakarsh_psu_internship_poster.pdf)). The poster concludes that "testing in a dark sky environment provides further confidence" in orbit performance.
- **Stellenbosch.** The COTS CubeStar was more accurate on the real night sky than on the monitor ([IAC 2021](https://p.iafastro.directory/iac/paper/id/66263/abstract-pdf/IAC-21,B4,6B,8,x66263.brief.pdf)).

**Equipment and cost.** A tripod, a laptop or Pi, and a dark site. Near zero.

**Effort.** One or two clear evenings once a tracker exists (estimate).

## Comparison

| Method | Tests | Closed loop | COTS tracker | Lab-built tracker | Cost | Effort (estimate) |
|---|---|---|---|---|---|---|
| 1a. Attitude-level model | Estimator and controllers | Yes | Yes | Yes | $0 | Low; needed anyway |
| 1b. Interface emulation | Flight software against the real message on the real bus | Yes | Yes, needs the vendor's interface spec | Yes | Under $50 | Medium |
| 1c. Image injection | Detection, identification and solve software | Open loop, or closed with a renderer | No | Yes | $0 | Low to medium |
| 2. Display plus collimator | Whole tracker, camera to quaternion | Yes, with a live renderer | Possible (Stellenbosch) | Yes | About $300–500 | Weeks |
| 3. Night sky | Real accuracy and brightness | No | Possible, if the unit can leave the lab | Yes | About $0 | Evenings |

## Recommendation for ADAPT

1. **Default to digital-only for the star tracker on the Physical Simulator.** The likely flight unit is a COTS tracker inside a COTS ADCS (slides 24–25, 2026-11-06 sync). For our controllers it's a source of attitude quaternions, so model it in the Digital Simulation (1a). When the Physical Simulator links to the Digital Simulation, emulate its message on the bench interface (1b). This is cheap, runs closed-loop, and fits "not built to flight accuracy".
2. **Ask the ADCS vendor** for the star tracker's interface document and any simulation or ground-support software, once the flight ADCS is chosen (Select ADCS Hardware, #8).
3. **Build a display-and-collimator stimulator only if a real optical tracker joins the bench.** For example, a lab-built Raspberry Pi and tetra3 tracker riding a single-axis turntable would let the classical controller and ML controller close the loop on real star images. The UIUC and Cal Poly builds show it's a few hundred dollars and a few weeks of student work.
4. **Do a night-sky check for any lab-built tracker.** It's nearly free and gives the accuracy figure the stimulator and the Digital Simulation's noise model should be checked against.
5. **Don't physically stimulate the flight star tracker** as part of the Physical Simulator. Flight unit acceptance is the vendor's job and is out of scope (flight qualification is out of scope on #5).

## Follow-up (2026-11-08)

We added OreSat, openstartracker.org and the canonical SOST repo. **The recommendation doesn't change.**

- OreSat backs the digital-only default. It builds and flies its own optical tracker, yet its ADCS hardware-in-the-loop setup still models the tracker as a Basilisk attitude sensor (Method 1a).
- OreSat backs the fallback too, and makes it cheaper. Its tracker was validated with night-sky field tests, astrometry.net calibration and regression tests on recorded star images. It never used a display and collimator. So if a lab-built tracker joins the bench, start with a night-sky check and a set of recorded images as regression tests. Build the stimulator only to close the loop on live star images.
- If we build a tracker, OreSat's open hardware and software (an AR0134 camera, about $500 in parts, LOST) are a worked starting point. Its 2° field figure is a functional check, not an accuracy target, and we found no published flight results.
- openstartracker.org adds the C++ back end (BEAST-2), the GLADOS and SORA missions it was built for, and a SmallSat 2017 paper. It gives no accuracy or test figures beyond the GitHub README.

## Open questions

- Which ADCS and star tracker does ADAPT fly, and does the vendor provide a sensor emulator or interface spec? (#8)
- Does the Physical Simulator ever carry a real camera, for example to test pointing at a star target (slide 21)? If so, the stimulator becomes worth building.
- What star tracker noise model should the Digital Simulation use? Vendor datasheet values, or a night-sky measurement?
