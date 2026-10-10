<img src="https://cdn.jsdelivr.net/gh/Claytonee/Motion-Design-VFX-Showcase@main/assets/banner.svg" alt="Digital Motion Lab — AI-operated motion design and VFX studio" width="100%"/>

# Digital Motion Lab — Motion Design, VFX & Sound

![After Effects](https://img.shields.io/badge/After_Effects-ExtendScript_builds-00085B?style=flat-square&logo=adobeaftereffects&logoColor=17D9F9)
![Blender](https://img.shields.io/badge/Blender-5.2_headless-00085B?style=flat-square&logo=blender&logoColor=17D9F9)
![Python](https://img.shields.io/badge/Python-audio_%2B_QC_engine-00085B?style=flat-square&logo=python&logoColor=17D9F9)
![FFmpeg](https://img.shields.io/badge/FFmpeg-mastering-00085B?style=flat-square&logo=ffmpeg&logoColor=17D9F9)
![Loudness](https://img.shields.io/badge/Delivery-%E2%88%9214_LUFS-34D399?style=flat-square)

> A production studio run as software. Every video starts as a JSON spec, is built into an editable After Effects project by script, gets its sound mixed by a measuring engine, and is passed or failed by an automated QC gate before a human ever sees it.
> **Role:** creative director, technical director and pipeline engineer · **Client work for:** [Opportunity Education Tanzania](https://www.opportunityeducation.org/)

The studio codebase is private. This page shows the work and explains how it is made.

## Featured film — *Partnership*

A 70-second vertical film that asks owners of school networks, NGOs and government to bring their schools into Opportunity Education's learner-centred model.

It opens on a moment everyone remembers from school. A teacher asks Memphis to state Archimedes' principle. She recites it by heart, stalls on *"…upthrust…"*, and the word comes back once more, quieter, until the room goes silent. Nobody says "rote learning". The sound says it.

Then the same classroom, from the same camera, turns learner-centred on a single frame. Music drop, taiko hit and light wipe land together at 20.45 s.

<p align="center">
  <video src="https://github.com/user-attachments/assets/229762b9-d485-41e5-aeb3-b5e59d2009b9" width="360" controls playsinline></video>
</p>

<p align="center"><sub>Press play and tap the speaker icon for sound: the full 70-second film in Full HD (1080 × 1920) with its original sound. Also available as a <a href="media/Partnership_v010B.mp4">file in this repo</a>.</sub></p>

| | |
|---|---|
| Format | 1080 × 1920, 30 fps, 70.0 s, 2100 frames |
| Delivery | −14.0 LUFS, −0.8 dBTP, 0 black frames (automated QC: PASS) |
| Voices | 3 hook lines + 13 narration lines, every on-screen event timed to word timestamps |
| Sound | 34 timed effects, all real recordings, plus one music edit cut so its drop hits the turn |
| Status | Approved, 10 October 2026 |

<img src="https://cdn.jsdelivr.net/gh/Claytonee/Motion-Design-VFX-Showcase@main/assets/partnership-frames.jpg" alt="Six frames from Partnership: the question, the fading echo, the turn, a skill, the ripple map, the ask" width="100%"/>

### Every event on the clock

<img src="https://cdn.jsdelivr.net/gh/Claytonee/Motion-Design-VFX-Showcase@main/assets/partnership-timeline.svg" alt="Partnership timeline: story sections, voice lines, sound effects and the music level across 70 seconds" width="100%"/>

The chart is drawn straight from the film's production spec, so it is the real timing, not an illustration of it.

### Craft decisions that carried it

- **The fade is the message.** Memphis's last word returns 13 dB quieter, low-passed at 3.5 kHz and drowned in a 1.1 s reverb, while the picture drains to 65 % grey. Level, filter, space and colour fail together, so the audience feels the memory go.
- **Silence before the turn.** The music drops to −40 dB for 0.4 s right before the downbeat. That gap is what makes the turn hit.
- **No AI video in the opening.** Generated clips read as fake, so the scene is built in After Effects from four rear-view stills: push-ins, a handheld wiggle, grain, a creeping vignette. With no visible mouths, no lip-sync is needed.
- **The timeline follows the voice.** Segment lengths and word timestamps drive the edit. Map rings fire on *families*, *communities* and *nation*; product chips land on *coaches*, *Quest* and *Career*. Re-record a line and the whole film re-flows.
- **A 3D map that the graphics stick to.** Tanzania is a Blender camera move. Its per-frame screen positions become After Effects tracker nulls, so rings, labels and ten school dots stay locked to the moving map.
- **Sound in key.** Skill hits are timpani tuned to the track's tonic. The schools light up on a rising kalimba run in the same key. There are no arcade beeps anywhere.
- **Spoken in full.** On screen it reads *TIE-approved*; the narrator says *approved by the Tanzania Institute of Education*.

## How the studio works

<img src="https://cdn.jsdelivr.net/gh/Claytonee/Motion-Design-VFX-Showcase@main/assets/pipeline.svg" alt="Production cycle: research, design, build, preview, inspect, correct, verify, approve" width="100%"/>

```mermaid
%%{init: { "theme": "base", "themeVariables": { "primaryColor": "#00085B", "primaryTextColor": "#EAF7F3", "primaryBorderColor": "#0E4DFF", "lineColor": "#17D9F9", "secondaryColor": "#0127BC", "tertiaryColor": "#00072D", "clusterBkg": "#00072D", "edgeLabelBackground": "#00072D", "fontSize": "13px" } }}%%
flowchart LR
    SPEC["spec.json<br/>copy · palette · timing · cues"] --> AE["After Effects builder<br/>ExtendScript → editable .aep"]
    SPEC --> MIX["Mix engine<br/>cue alignment · SNR levels · stems"]
    BL["Blender<br/>headless renders + tracking data"] --> AE
    AE --> REN["aerender<br/>watchdog"]
    MIX --> REN
    REN --> MAS["Master<br/>−14 LUFS / −1 dBTP"]
    MAS --> QC{"QC gate<br/>duration · frames · black · loudness · sync"}
    QC -->|fail| SPEC
    QC -->|pass| REV["Human review → approve"]
    classDef a fill:#00085B,stroke:#0E4DFF,color:#EAF7F3,stroke-width:1.5px;
    classDef b fill:#00072D,stroke:#17D9F9,color:#EAF7F3,stroke-width:1.5px;
    classDef c fill:#0127BC,stroke:#34D399,color:#EAF7F3,stroke-width:1.5px;
    class SPEC,AE,MIX a; class BL,REN,MAS b; class QC,REV c;
```

- **Specs, not timelines.** Copy, colours, fonts, timings and every sound cue live in one JSON file. Brand is data, so a new client swaps in without touching the builder.
- **Editable output.** The builder writes a named, layered After Effects project with control layers and expressions. A motion designer can open it and keep working by hand.
- **A mixer that measures.** Each effect is aligned by its onset or its peak and set by its ratio to the music at that moment, so a whoosh never vanishes under a loud bar. Stems are exported so After Effects reproduces the mix exactly.
- **Evidence over claims.** A version only counts once it is rendered, probed and inspected frame by frame. QC checks duration, frame count, black frames, loudness, true peak and effect sync against the spec.
- **Research before design.** Reference films are measured (cut rhythm, type size, loudness) instead of guessed, and every project ships research records that the next project reuses.
- **Built on a laptop.** The whole pipeline runs on a 2-core Intel i5 with integrated graphics, which forced the scripts to be efficient: small previews, headless renders, cached passes.

## Toolchain

| Layer | Tools |
|---|---|
| Motion | Adobe After Effects 2026 (ExtendScript builds, aerender), Media Encoder |
| 3D / VFX | Blender 5.2 (headless Python, camera moves, tracking export) |
| Sound | Python (NumPy, SciPy) mix engine, FFmpeg loudness mastering, licensed and CC recordings |
| Voice | HeyGen voices, word-level timestamps |
| Imagery | Google Flow (Nano Banana) stills, Photoshop |
| QC | Python + OpenCV frame checks, ffprobe, EBU R128 loudness |

## Credits

Produced for **Opportunity Education Tanzania**. Music: *African night* by ECO-ART (Pixabay Content License).

Sound effects under CC BY 4.0, all from Freesound:
- "Movie Trailer Boom" and "Deepest Boom Yet" by hykenfreak;
- "Bar_Chimes_Sweep_Up.wav" by pjcohen;
- "COPLAND DRUM 01.wav" by sandyrb;
- "impact-reverse-soft" by newagesoup.

All other effects are CC0. Map data: Natural Earth. Classroom imagery is AI-generated and stands in until consented photography replaces it.

## Reach me

[![Email](https://img.shields.io/badge/Email-claytonecurth%40gmail.com-0E4DFF?style=for-the-badge&labelColor=00072D&logo=gmail&logoColor=17D9F9)](mailto:claytonecurth@gmail.com)

Happy to walk through the spec-driven builder, the mix engine or the QC gate in detail.

<sub>*Partnership* was produced for Opportunity Education Tanzania; its brand marks belong to Opportunity Education. The studio codebase is private. The artwork and written content of this page are © 2026 Claytone Curthberth Mhina and are not licensed for reuse without written permission.</sub>
