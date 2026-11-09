# 2026-11-06: ADCS team sync

ADCS subsystem team meeting. Only ADAPT-relevant points are recorded here.

**Attendees:** Nate Gay, Jack Holden, Kelsey Morris

**Recording:** [SharePoint](https://txst.sharepoint.com/:v:/r/sites/GRP-texasstatespacelab2/Shared%20Documents/ADAPT/Meetings/ADCS/2026-11-06-team-sync.mp4?d=wca2b5512e8494fa1955af0ff40c12932&csf=1&web=1&e=meS9wp) (TXST login required)

## Reaction Wheel Demonstrator

- **Prior art: OreSat / Portland State.** Two requests to the Portland State lab for details of their lab-built reaction wheels got no reply, so we proceed on our own. From an earlier conversation, their wheels were built from drone motors. Their public material turned out to be plenty: a 2016 paper ("Open source reaction control wheel system…", on ResearchGate), the OreSat hardware repos with reaction wheel equations, a poster on a later upgrade of their wheel system, and test videos. In one video the mock-up hangs from a string; when pushed by hand, the wheels stop the rotation and then resist further pushes. That is a good model for our first test.
- **Decision: build a lab demonstrator, not flight wheels.** A simple rig: three motors, mounted as well as we can manage, wired up to our software. It's a learning project, and it lets the control software be written and ready before any flight unit exists.
- **Motors:** they need to be inexpensive, not accurate, so we can buy several plus spares in case some burn out during construction. Choose them by the next meeting.
- **Frame:** 3D print rather than machine it from lightweight aluminum. A senior design team in the lab printed a CubeSat case about three years ago, testing a carbon-filled, low-outgassing material for use in space. Their design files may still exist.

## Flight reaction wheels

- **Required accuracy is unknown.** Ask leadership and Payload how precise the flight wheels must be. Noise can't be eliminated, so the question is how much is acceptable.
- **Could lab-built wheels fly?** It was discussed but not decided. If we can measure their accuracy, it's possible in principle. The counterpoint is that pointing underpins the science, so the mission will likely buy a precise COTS ADCS and star tracker, and much of the budget will go there. Building our own would be rougher and much slower, and it depends on funding.

## Action items

- **Jack Holden:** ask leadership and Payload what flight reaction wheel accuracy is needed (#9)
- **Jack Holden:** draft a timeline for the Reaction Wheel Demonstrator, with steps first and dates where possible; the first milestone is motors chosen by the next meeting (#10)
- **Kelsey Morris:** 3D print the demonstrator casing; ask Evan for the earlier case design files (#11)
- **Kelsey Morris:** research motors and present options at the next sync; anyone else is welcome to compare notes (#12)
- **Nate Gay:** collect the OreSat / Portland State material and share it (#13)
- **Nate Gay:** post action items and resources to the team Discord, and find out whether the mission overview from the 2026-11-05 leadership meeting can be shared (it is now in [`../overview/`](../overview/))
