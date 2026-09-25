<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="docs/assets/banner-dark.svg">
  <img alt="PITWALL — driver profiles and coaching for ACC and LMU" src="docs/assets/banner-light.svg" width="100%">
</picture>

![Windows](https://img.shields.io/badge/Windows-10%20%7C%2011-0078D4?style=flat-square&logo=windows&logoColor=white)
![Python 3.14](https://img.shields.io/badge/python-3.14-3776AB?style=flat-square&logo=python&logoColor=white)
![uv](https://img.shields.io/badge/uv-managed-DE5FE9?style=flat-square&logo=uv&logoColor=white)
![ACC](https://img.shields.io/badge/ACC-supported-f0883e?style=flat-square)
![LMU](https://img.shields.io/badge/LMU-next-58a6ff?style=flat-square)
![Status](https://img.shields.io/badge/status-early%20testing-a371f7?style=flat-square)

**Driver profiles and coaching from your own telemetry, for Assetto Corsa Competizione and Le Mans Ultimate.**<br>
<sub>Record your laps · find where you lose time · learn how you drive</sub>

</div>

> [!NOTE]
> pitwall is in early testing with a small group of drivers. See [Access](#access).

<div align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="docs/assets/track-dark.svg">
  <img alt="Speed-coloured map of a Nordschleife lap, with a dot that drives the lap at real pace" src="docs/assets/track-light.svg" width="100%">
</picture>
<sub>A real Nordschleife 24h lap in the Aston Martin V8 Vantage GT3 (8:13.36). The colour is the speed. The dot drives the lap at 16× real pace.[^art]</sub>
</div>

## What we aim to deliver

pitwall aims to be a personal driving coach that learns from your own laps. It takes your laps from ACC and LMU
and compares them corner by corner with reference laps and with your own best laps. Most telemetry tools show you
graphs and leave the rest to you. pitwall tells you what the data means for your driving, and it gets more personal
with each session.

- **A coach that knows you.** pitwall builds a picture of how you drive from your whole history: every car, every
  track and every session. With no history, it starts from your first laps, and it gets better the more you drive.
- **Something for every driver, with no AI.** Every driver gets a debrief after each stint and session: where the
  time went, the likely cause, and what to try next. What code can conclude from the data, the debrief says.
- **Your own AI as your coach.** Connect the AI that you already use. It reads your pitwall data and talks with you
  about it: your habits, your trends, a plan for your next practice, and any question at any time.
- **Honest numbers.** Every number comes from code, and every number says where it came from. When the data cannot
  answer a question, pitwall says so. The AI quotes pitwall's numbers; it never makes them up.
- **Pace and racecraft.** In practice and qualifying, pitwall coaches your pace corner by corner. In a race, it also
  looks at what decides the result: the start, your positions, penalties, pit stops and contact.
- **Every level of driver.** Drivers who still use assists, such as the racing line, are not left out. pitwall
  meets you where you are and helps you take the next step.
- **Your data stays yours.** pitwall runs on your PC and uploads nothing. Other drivers in your races are stored
  only as race numbers.

The analysis gets faster with each version. The goal is a coach that can talk with you between stints, while the
session is still on.

## Features

### Coaching

| | Feature | Status |
|:-:|---|---|
| 📝 | **Debrief:** after each stint and session, the corners where you lost the most time and the likely cause, with drills; stint trends, tyres, and your progress since your last session, with no AI | ✅ Available |
| 🏁 | **Race debrief:** your start, positions by lap, penalties and their cost, pit stops, and contact with the pace after it | ⏳ Next |
| 🔒 | **Consistency:** how repeatable each corner is, and which corners are "locked" (fast and consistent) | ✅ Available |
| 🧑 | **Driver profile:** your habits across all your laps (brake point, brake release, coast, apex speed, throttle) against the reference drivers, built by your AI from pitwall's numbers | ⏳ Planned |
| 🤖 | **AI coach:** your own AI reads your data through an MCP server: trends, practice plans, and questions at any time | ⏳ Planned |

### Analysis

| | Feature | Status |
|:-:|---|---|
| 📐 | **Corner analysis:** a map of each track built from game telemetry, the same corners for every car and lap, and for each lap and corner: brake point, apex speed, coast time and the time gained or lost | ✅ Available |
| 🏆 | **Personal bests:** your PB at every track and the gap to the reference lap, and a PB message in the recorder | ✅ Available |
| 📊 | **Race-pace levels:** your pace for each track and car against [Ohne Speed](#credits)'s race-pace sheets, from Alien to Offline | ⏳ Planned |
| 📈 | **Dashboard:** session review and progress over time | ⏳ Planned |

### Data

| | Feature | Status |
|:-:|---|---|
| 🎙️ | **ACC live recording:** reads ACC telemetry at 100 Hz while you drive, splits it into laps, and saves each lap | ✅ Available |
| 🗄️ | **History import:** your ACC MoTeC files, GO FAST laps (ACC and LMU), and GO Setups reference laps | ✅ Available |
| 🚦 | **ACC race data:** positions, gaps, penalties, flags, damage and pit stops from shared memory, and race events from the ACC broadcasting API | ⏳ Next |
| 🏁 | **LMU live recording:** through the official `LMU_Data` shared memory | ⏳ Planned |

## Access

pitwall is in private testing with a small group of drivers, and the code is in a private repository.
This page describes what pitwall does. To join the tests, ask Jake.

## Tester build

Testers get a zip from Jake: no install needed.

1. Unzip it anywhere.
2. Double-click `pitwall.cmd`. The first start downloads Python and the packages once (1–2 minutes, needs internet).
3. Choose from the menu: record a session, import your history, show your last debrief, or your PBs.

Your laps are kept in `%LOCALAPPDATA%\pitwall`, so a new zip keeps them. `READ-ME.txt` in the zip has the details.

## Where your data is stored

| What | Where |
|---|---|
| One file for each lap | `data/laps/<game>/<track>/<car>/<session>/lap_NNN.parquet` |
| The lap catalog | `data/pitwall.sqlite` (SQLite: you can open it in any SQLite tool, also while the recorder runs) |

The analysis and the debrief run on your PC. When the MCP server arrives, your AI reads your data only when you ask it.

## Anti-cheat safety

> [!WARNING]
> pitwall only reads game telemetry. It opens the shared memory that the game publishes with `OpenFileMappingW`.
> It never creates a mapping, never writes to game memory, and never injects code. This matters for LMU, which uses EasyAntiCheat.

## How it works

```mermaid
flowchart LR
  subgraph live [Live]
    ACC[ACC shared memory]
    LMU[LMU shared memory]
  end
  subgraph history [Your existing laps]
    MOTEC[(ACC MoTeC files)]
    GOF[(GO FAST)]
    REF[(GO Setups<br/>reference laps)]
  end
  ACC --> COL[Recorder<br/>100 Hz]
  BRD[ACC broadcasting API] -.-> COL
  LMU -.-> COL
  MOTEC --> IMP[Importers]
  GOF --> IMP
  REF --> IMP
  COL --> STORE[(One Parquet file per lap<br/>+ SQLite catalog)]
  IMP --> STORE
  STORE --> ANA[Analysis]
  PACE[(Ohne Speed<br/>race-pace sheets)] -.-> ANA
  ANA --> DEB([Debrief])
  ANA --> MCP[MCP server] --> AI([Your AI coach])
  ANA -.-> UI([Dashboard])

  classDef done fill:#238636,stroke:#2ea043,color:#fff
  classDef building fill:#8957e5,stroke:#a371f7,color:#fff
  class ACC,COL,STORE,MOTEC,GOF,REF,IMP done
  class ANA,DEB done
  class BRD building
```

Green parts are available. Purple parts are next. Dotted lines are planned.

## How lap comparison works

pitwall compares two laps by distance, not by time. At distance $x$ into the lap, the time gap to a reference lap is:

$$\Delta t(x) = \int_{0}^{x} \left( \frac{1}{v_{\text{you}}(s)} - \frac{1}{v_{\text{ref}}(s)} \right) ds$$

The analysis splits every corner into three phases and adds up $\Delta t$ in each phase:

$$\Delta t_{\text{lap}} = \underbrace{\sum \Delta t_{\text{entry}}}_{\text{brake point} \to \text{apex}} + \underbrace{\sum \Delta t_{\text{exit}}}_{\text{apex} \to \text{full throttle}} + \underbrace{\sum \Delta t_{\text{straight}}}_{\text{full throttle} \to \text{next brake}}$$

This shows whether you lose time on corner entry, on corner exit or on the straights, and in which corners.

## Supported data sources

| Source | Format | What it gives | Status |
|---|---|---|---|
| ACC shared memory | Binary structs, 100 Hz | About 100 live channels for each sample, with positions, steering degrees and tyre data | ✅ |
| ACC MoTeC | `.ld` + `.ldx` | 55 channels at up to 200 Hz | ✅ |
| GO FAST | SQLite, Brotli-compressed JSON | ACC and LMU laps at 20 Hz, with validity and fuel | ✅ |
| GO Setups reference laps | MoTeC `.ld` | Esports reference laps | ✅ |
| LMU shared memory | `LMU_Data` (official header) | Live LMU telemetry | ⏳ |

## Credits

- **Ohne Speed** for the ACC and LMU race-pace sheets that the race-pace levels will use.
- **GO Setups** for GO FAST and the reference laps that pitwall imports from your own install.

[^art]: The art is generated from recorded telemetry by a script in the pitwall repository. The dot's path uses SVG `animateMotion`, with `keyTimes` from the real lap clock and `keyPoints` from the distance along the line. So the dot brakes where the driver braked.
