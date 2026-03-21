# Holographic HQ

<p align="center">
  <img src=".assets/logo.png" alt="Holographic HQ" width="180"/>
</p>

> A concept repo for a spatial AI workspace where Hoss and other agents live in VR.

[![Quest](https://img.shields.io/badge/Quest-3%2B-blue)](https://www.meta.com/quest/)
[![Docs](https://img.shields.io/badge/Repo%20Type-specs%20only-lightgrey)](#current-state)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## About

Holographic HQ sketches a VR-first operating environment for AI collaboration: a mixed-reality room, a holographic Hoss avatar, and spatial panels for code, tasks, terminal output, and agent orchestration.

This repository currently holds the product and world-design specs for that vision. It does not yet include the Unity project, bridge server, or runtime code described in the roadmap.

## Current State

- `planned`: concept and specification repo
- `included`: world, avatar, and tool-trigger design docs
- `not yet included`: Unity app, Quest build, backend bridge, tests, deployment config

## Repository Contents

| Path | Purpose |
|------|---------|
| `README.md` | Product overview and roadmap |
| `SPECs/WORLD_SPEC.md` | Room layout, interaction model, spatial design language |
| `SPECs/AVATAR_SPEC.md` | Hoss avatar behavior and animation expectations |
| `SPECs/TOOL_SKILLS.md` | Voice + pointing interaction model for triggering tools |
| `CONTRIBUTING.md` | Contribution guidance for future implementation work |

## Product Direction

### Core Experience
- Enter a persistent VR workspace on Quest hardware
- Interact with Hoss as a spatial, voice-enabled presence
- Open holographic panels for code, terminal output, kanban, and web content
- Trigger agent actions with voice commands and spatial targeting

### Planned Platform
| Layer | Planned Technology |
|------|---------------------|
| XR client | Unity 2022 LTS |
| Device target | Meta Quest 3+ |
| Voice input | Quest mic + speech pipeline |
| Voice output | XTTS v2 |
| Agent backend | OpenClaw / Hoss gateway |
| Transport | WebSocket + HTTP |

## Roadmap

| Phase | Focus |
|------|-------|
| `v0.1` | Holographic display + voice chat |
| `v0.2` | Interactive panels + gestures + spatial audio |
| `v0.3` | Multi-agent environment + tool triggers |
| `v1.0` | Persistent world + multiplayer direction |

See `SPECs/WORLD_SPEC.md`, `SPECs/AVATAR_SPEC.md`, and `SPECs/TOOL_SKILLS.md` for the detailed product definition.

## Contributing

If you want to help, start with `CONTRIBUTING.md` and read the spec documents before proposing implementation work.

## License

MIT License - see `LICENSE`.
