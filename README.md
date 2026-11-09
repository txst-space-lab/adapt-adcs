# adapt-adcs

ADAPT ADCS Collaboration Repo

This repository collects research, small scripts, and documentation for the attitude determination and control subsystem (ADCS) of ADAPT, the Adaptive Drag-Aware Pointing Testbed. ADAPT is a 6U CubeSat from the Texas State Space Lab that will compare an ML attitude controller with a classical one in very low Earth orbit.

## Start here

- [ADAPT Detail Overview deck](docs/mission/overview/adapt-detail-overview-115.pdf) ([text version](docs/mission/overview/adapt-detail-overview-115.md))
- [Mission overview walkthrough, 2026-11-05](docs/mission/meetings/2026-11-05-mission-overview.md): notes, with a link to the [recording](https://txst.sharepoint.com/:v:/r/sites/GRP-texasstatespacelab2/Shared%20Documents/ADAPT/Leadership/2026-11-05%2017.32.43%20ADAPT%20Leadership%20Meeting/video1627458037.mp4?d=wf06ff11d08fe47dd98a25154fe1bdc0a&csf=1&web=1&e=I5yeiZ) (TXST login required)
- [UNP ADCS Presentation](https://www.youtube.com/watch?v=zWqOiQWSQBg)
- [Glossary](GLOSSARY.md): the terms we use, and the ones we avoid

## Work tracking

Work is tracked in [GitHub Issues](https://github.com/txst-space-lab/adapt-adcs/issues). Each large area of work is an [epic](https://github.com/txst-space-lab/adapt-adcs/issues?q=is%3Aissue%20label%3Aepic), and smaller tickets sit under it as sub-issues.

## Layout

| Path | Contents |
|---|---|
| [`GLOSSARY.md`](GLOSSARY.md) | Domain language for ADCS and ADAPT |
| [`docs/mission/`](docs/mission/) | Mission source material: the overview deck and ADCS-relevant meeting notes |
| [`docs/agents/`](docs/agents/) | Configuration for AI coding agents (see also [`AGENTS.md`](AGENTS.md)) |
