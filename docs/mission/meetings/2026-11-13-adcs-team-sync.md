# 2026-11-13: ADCS team sync

ADCS subsystem team meeting. Only ADAPT-relevant points are recorded here.

**Attendees:** Nate Gay, Jack Holden, Logan Magaha, Collin Cook (Payload, sitting in)

**Recording:** SharePoint (TXST login required) <!-- TODO: add link -->

## Team and tools

- **Work is tracked on GitHub.** The goals from the mission overview's ADCS slide are broken into epics #1–#4, #7 and #8, plus #5, the Physical Simulator, added because hardware the team can touch helps retention and makes the work easier to picture. Comments on any ticket are welcome and get folded into its breakdown.
- **Workflow:** refine tickets together until everyone shares an understanding of the work, move them to Ready, then anyone picks them up. Dated work goes on the existing roadmap view of the [project board](https://github.com/orgs/txst-space-lab/projects/9/views/4). Milestones are broken into tickets that different people pick up, rather than each having a single owner.
- **This repo collects reference material** (OreSat papers, posters and datasheet, mission sources, UNP talks) so that people and AI tools can search it. Flight firmware won't live here: if ADAPT uses F Prime, the ADCS component belongs in a shared flight software repo.
- **Payload** will set up its own GitHub team with ADAPT as the parent team.

## Reaction Wheel Demonstrator

- **Motors:** OreSat's wheel motor costs about $13, so a demonstrator should be cheap to build. Motor research was posted to Discord and is now on #12.
- **Timeline (#10):** a first draft is on the `timeline` branch (`docs/reaction_wheel_demo/timeline.txt`). It adds two milestones not yet tracked:
  - **Reaction wheel placement draft (#32):** a rough drawing of where the three wheels sit on the frame and how the frame holds them. It can borrow heavily from OreSat, but the team should understand the design, not just copy the calculations. It's for the demonstrator, not the 6U: building flight wheels is advised against unless a lab can do it very well, and COTS wheels aren't prohibitively expensive.
  - **Initial 1U CAD model (#33):** needed before the placement draft, since the wheel mounts go on the model. A plain 10 × 10 × 10 cm box to the CubeSat standard is enough to start. Evan may still have the earlier students' 1U design, printed in a carbon composite filament. The "ask Evan" ticket (#31) was closed as a duplicate of #11.
- **Flight wheel accuracy (#9):** preliminary numbers are in the requirements verification matrix, though nothing is settled this early. Post what the sheet says to #9 and close it.

## Physical Simulator

- **Helmholtz cage:** the lab's existing cage is disassembled in a corner for lack of space. The lab has started cages twice before and never used one; the team expects to build and use one this time, as some of its first test equipment. Space and budget are tracked in #15.

## Education

- **Rotational dynamics is the core of the work,** and the team's background varies. Collect resources (a dynamics textbook covers angular velocity and momentum), and possibly give a short lecture (#30). A second session on magnetics would support the Helmholtz cage and magnetorquers.
- **Magnetic-only pointing:** a lab member's research on pointing with magnetorquers alone could be checked against ADAPT data as well as earlier mission data.

## Action items

- **Jack Holden:** post the flight reaction wheel accuracy figures from the requirements verification matrix and close #9
- **Jack Holden:** keep refining the timeline and put its milestones on the roadmap view as tickets (#10)
- **Jack Holden:** gather rotational dynamics resources by the weekend, with a lecture by the end of the year (#30)
- **Logan Magaha:** create or obtain the initial 1U CAD model and show it at the next sync, with CAD and 3D printing help from Collin Cook (#33). Jasmine, who assists in the mechanical engineering CAD lab, is another resource
- **Nate Gay:** add Logan Magaha to the GitHub organization and assign #33
- **Nate Gay:** walk Collin Cook through the GitHub organization setup (teams, projects)
