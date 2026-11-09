# 2026-11-05: Mission overview walkthrough

Leadership meeting. The whole ADAPT Detail Overview deck ([`../overview/adapt-detail-overview-115.md`](../overview/adapt-detail-overview-115.md)) was presented, followed by team updates. Only ADAPT-relevant points are recorded here. Official minutes are on the lab SharePoint (leadership folder).

**Recording:** [SharePoint](https://txst.sharepoint.com/:v:/r/sites/GRP-texasstatespacelab2/Shared%20Documents/ADAPT/Leadership/2026-11-05%2017.32.43%20ADAPT%20Leadership%20Meeting/video1627458037.mp4?d=wf06ff11d08fe47dd98a25154fe1bdc0a&csf=1&web=1&e=I5yeiZ) (TXST login required)

## Context given for the deck

- The deck is the team's final presentation for the University Nanosatellite Program from summer 2026. The mission is expected to change a lot from it.
- The aim this semester is a baseline understanding of each subsystem. Next semester shifts to forward development work relevant to the mission. More detailed goals and benchmarks for the semester are being drafted.

## Points made in the talk that aren't in the slides

- **Orbital regime metrics (slide 8) are "largely under revision"** and may not be used going forward.
- **The classical controller is not settled.** PID is a placeholder and is likely to be replaced with something more modern (slide 13).
- **Component picks are placeholders for budgeting** (slides 24–25). They "aren't end all be all super solid decisions" and still need analysis.
- **The experiment points at stars.** Both controllers point the star tracker at the same star or constellation, so their pointing and effort can be compared directly (slide 21).
- **ML beating classical is only an extended goal.** The mission is a comparison; the ML controller performing better is extended success, not minimum or full (slide 9).
- **Storm mode exists to protect the comparison.** In storms the orbit decays faster, so the controllers alternate more often to keep the conditions they face as similar as possible (slides 20, 22).
- **The safety gate stops the ML controller from shortening the mission.** For example, it would block an attitude with high drag that speeds up deorbit. The classical controller remains available to take over (slide 19).
- **ADCS's role:** "regardless of the controller, we want a system capable of precise pointing" (slide 33). ADCS works most closely with Payload.

## Team updates relevant to ADAPT

- **Structures** will focus research on deployable solar panels in VLEO and what has worked before. It was suggested they also look at atomic oxygen erosion of hinges and fold joints.
- **CDH** plans to assign F Prime (F´) tutorials to members after the GitHub workshop.

## Follow-ups

- Slides and recordings are to be posted to the lab SharePoint.
