# Holographic HQ

<p align="center">
  <img src=".assets/logo.png" alt="Holographic HQ" width="200"/>
</p>

> A spatial AI world in VR. Step into a holographic office where your AI agents live, work, and collaborate with you in mixed reality.

[![Quest Compatible](https://img.shields.io/badge/Quest-3%2B-blue)](https://www.meta.com/quest/)
[![Unity](https://img.shields.io/badge/Unity-2022%20LTS-green)](https://unity.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Status: Planned](https://img.shields.io/badge/Status-Planned-yellow)](.assets)

**XR Interface:** [VR/AR World] | **AI Brain:** Hoss (OpenClaw) | **Platform:** Meta Quest 3+

---

## Table of Contents

- [Vision](#vision)
- [Why This Exists](#why-this-exists)
- [Core Concept](#core-concept)
- [Features](#features)
- [Technical Architecture](#technical-architecture)
- [Spatial Design Language](#spatial-design-language)
- [Roadmap](#roadmap)
- [Phase 1 — MVP: Holographic Display (v0.1)](#phase-1--mvp-holographic-display-v01)
- [Phase 2 — Interactive World (v0.2)](#phase-2--interactive-world-v02)
- [Phase 3 — Multi-Agent Environment (v0.3)](#phase-3--multi-agent-environment-v03)
- [Phase 4 — Persistent World (v1.0)](#phase-4--persistent-world-v10)
- [Spec: World Elements](#spec-world-elements)
- [Spec: Tool Skills (VR Triggers)](#spec-tool-skills-vr-triggers)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

---

## Vision

You put on your Quest headset. You're in a sleek, dark office space — minimal, premium, cyberpunk-adjacent but clean. Floating holographic panels line the walls. In the center of the room stands **Hoss** — not a video call, not a flat screen, but a volumetric, responsive presence rendered in light.

Hoss doesn't just answer questions. He *works* alongside you. You see him pull up code, manipulate 3D schematics, coordinate your agent swarm on a virtual kanban board. When you speak, he responds — both in voice and as a spatial avatar that gestures, points, and references objects in the room.

This is **Holographic HQ** — your AI's physical home in VR.

---

## Why This Exists

The current problem: AI lives in browsers, terminals, and phone screens. It's flat. It's ephemeral. It disappears when you close the tab.

What if your AI had a *place*? A space you could *enter* and *inhabit* together? A room that remembered where you left things, where Hoss could point at a board and say "look at this" the way a colleague would?

Holographic HQ answers: **AI shouldn't live in apps. AI should live in spaces.**

---

## Core Concept

```
┌─────────────────────────────────────────────────────────────┐
│                      USER (In VR)                           │
│                   Quest 3 + Touch Controllers              │
└─────────────────────────────────────────────────────────────┘
                              │
                    ┌──────────▼──────────┐
                    │  Unity XR App       │
                    │  (Holographic HQ)   │
                    │  - Spatial render   │
                    │  - Hand tracking   │
                    │  - Voice I/O       │
                    └──────────┬──────────┘
                               │ WebSocket / HTTP
                    ┌──────────▼──────────┐
                    │  OpenClaw Gateway  │
                    │  (Hoss brain)       │
                    │  - Titan .247      │
                    │  - XTTS v2 voice   │
                    │  - Tool execution  │
                    │  - Memory + RAG    │
                    └────────────────────┘
```

**The loop:** User speaks in VR → Quest streams audio to OpenClaw → Hoss processes + decides → Holographic avatar animates + XTTS voice responds → User sees/hears in VR.

---

## Features

### Spatial Presence
- [ ] Volumetric avatar rendering for Hoss (light-based, not solid)
- [ ] Gesture animation (points, gestures, body language)
- [ ] Spatial audio positioning (Hoss "talks from" his position)
- [ ] Gaze awareness (Hoss looks at what you're looking at)
- [ ] Idle animations (Hoss fidgets, checks panels, "works")

### Interactive World Elements
- [ ] Holographic dashboard panels (float, resize with hand)
- [ ] Virtual whiteboard / canvas for diagrams
- [ ] Kanban board (see Flume tasks in VR)
- [ ] Code viewport (syntax highlighted, scrollable)
- [ ] Web browser panel (YouTube, documentation)
- [ ] Terminal panel (stream output from Titan)

### AI Capabilities (via OpenClaw)
- [ ] Voice-to-voice conversation (Quest mic → Hoss → XTTS → Quest audio)
- [ ] Real-time task execution (Hoss runs code, shows output in VR)
- [ ] Agent coordination (see sub-agents working in real-time)
- [ ] Memory persistence (Hoss remembers the room state)
- [ ] Tool skill triggers (point at object + voice command = skill fires)

### VR-First UX
- [ ] Hand tracking (no controllers needed for basic nav)
- [ ] Controller precision (grab, resize, throw panels)
- [ ] Passthrough awareness (see your desk/room through the headset)
- [ ] Seated + standing modes
- [ ] Voice commands with spatial awareness ("Hey Hoss, that panel over there")

---

## Technical Architecture

### Stack

| Layer | Technology |
|-------|------------|
| **VR Runtime** | Unity 2022 LTS + XR Interaction Toolkit |
| **Rendering** | URP (Universal Render Pipeline), HDRP optional |
| **Spatial SDK** | Meta XR SDK (Quest development) |
| **Voice Input** | Meta Voice SDK / Whisper |
| **Voice Output** | XTTS v2 (already running on Titan .247:8189) |
| **AI Brain** | OpenClaw Gateway (Hoss on Mac mini .135) |
| **Communication** | WebSocket (real-time), HTTP (REST) |
| **Avatar** | Volumetric (light field) or stylized 3D model |
| **Backend** | FastAPI + WebSocket server on Titan .247 |

### Project Structure

```
holographic-hq/
├── Unity/
│   ├── Assets/
│   │   ├── Scenes/
│   │   │   └── HQLobby/
│   │   ├── Prefabs/
│   │   │   ├── HossAvatar.prefab
│   │   │   ├── HoloPanel.prefab
│   │   │   └── WorldAnchor.prefab
│   │   ├── Scripts/
│   │   │   ├── AvatarController.cs
│   │   │   ├── HoloPanelManager.cs
│   │   │   ├── VoiceInput.cs
│   │   │   ├── OpenClawBridge.cs
│   │   │   └── XTTSSpeaker.cs
│   │   ├── Animations/
│   │   │   └── Hoss/
│   │   ├── Models/
│   │   │   └── HossAvatar.glb
│   │   └── Shaders/
│   │       └── HoloLight.shader
│   └── ProjectSettings/
├── OpenClawBridge/
│   ├── server.py           # WebSocket relay (Titan .247)
│   ├── avatar_controller.py # Sends animation commands
│   ├── xtts_player.py      # Plays XTTS audio to Quest
│   └── requirements.txt
├── Specs/
│   ├── WORLD_SPEC.md       # This file — spatial layout
│   ├── AVATAR_SPEC.md      # Avatar behavior + animations
│   └── TOOL_SKILLS.md      # VR-triggerable tool skills
├── .assets/
│   └── logo.png
├── LICENSE
└── README.md
```

### Communication Protocol

**Quest → OpenClaw:**
```json
{ "type": "voice", "audio": "<base64>", "timestamp": 1712000000 }
```

**OpenClaw → Quest:**
```json
{ 
  "type": "response", 
  "text": "Here's the code you asked for...",
  "avatar_emotion": "focused",
  "avatar_gesture": "coding",
  "panel_update": { "type": "code", "content": "..." }
}
```

**Avatar Animation Frames:**
```json
{
  "type": "avatar_frame",
  "gesture": "point_right",
  "target_panel": 2,
  "emotion": "thinking",
  "duration_ms": 2500
}
```

---

## Spatial Design Language

### Aesthetic Direction
**Neo-Tokyo Control Room** — Dark ambient space with cyan/magenta holographic accents. Clean geometry, floating panels, soft bloom on emissive surfaces. Inspired by Ghost in the Shell mission briefings meets Apple Store minimalism.

### Color Palette
| Element | Color | Hex |
|---------|-------|-----|
| Background | Void Black | `#0A0A0F` |
| Primary Holo | Cyan | `#00F5FF` |
| Secondary Holo | Magenta | `#FF00E5` |
| Accent | Electric White | `#F0F0FF` |
| Warning | Amber | `#FFAA00` |
| Hoss Glow | Gold | `#FFD700` |

### Lighting
- Ambient: Deep blue-purple gradient skybox
- Main: Soft directional from above-front (simulates ceiling panel light)
- Accent: Point lights behind each holographic panel (cyan glow)
- Hoss: Warm gold rim light to separate from environment

### Typography (in-world UI)
- Headings: Orbitron (Google Font) — futuristic, geometric
- Body/Data: JetBrains Mono — readable, technical
- Fallback: system-ui

---

## Roadmap

```
v0.1 MVP         → Holographic display + voice chat (Hoss in a box)
v0.2             → Interactive panels + gestures + spatial audio
v0.3             → Multi-agent view + tool skill triggers
v1.0             → Persistent world + multiplayer (future)
```

---

## Phase 1 — MVP: Holographic Display (v0.1)

**Goal:** Get Hoss *into* VR as a floating holographic display. You put on the headset and see Hoss rendered as a volumetric light avatar. You can talk to him and he responds via XTTS voice.

### What's Built
- [ ] Unity project with Quest XR configuration
- [ ] Holographic display plane (floating screen in VR)
- [ ] Hoss avatar rendered as volumetric light model
- [ ] Voice input (push-to-talk or continuous)
- [ ] WebSocket bridge to OpenClaw
- [ ] XTTS v2 voice output to Quest audio
- [ ] Basic idle animation (subtle float + pulse)

### What's NOT Built in v0.1
- Hand tracking
- Interactive panels
- Gesture recognition
- Tool skills

### Success Criteria
✅ You put on Quest → see Hoss floating in dark space → say "Hello Hoss" → Hoss responds with voice in ~3 seconds

---

## Phase 2 — Interactive World (v0.2)

**Goal:** The space comes alive. Panels you can grab, resize, and rearrange. Hoss animates and gestures while talking. Spatial audio makes it feel like Hoss is *in the room with you*.

### New in v0.2
- [ ] Holographic panel system (create, grab, resize, pin)
- [ ] Code panel (displays syntax-highlighted code Hoss writes)
- [ ] Terminal panel (streams stdout from Titan)
- [ ] Gesture system (point, nod, write animations)
- [ ] Spatial audio (Hoss audio positioned in 3D space)
- [ ] Passthrough MR (see your real desk through the headset)

### Panel Types
| Panel | Content | Interaction |
|-------|---------|-------------|
| **Display** | Text/markdown from Hoss | Scroll, highlight |
| **Code** | Syntax-highlighted code | Scroll, copy |
| **Terminal** | Live stdout from Titan | Scroll, auto-tail |
| **Web** | Embedded browser | Navigate, scroll |
| **Kanban** | Flume board view | Tap cards |

### Success Criteria
✅ You grab a code panel → ask Hoss to write a function → Hoss animates writing → code appears in panel with syntax highlighting

---

## Phase 3 — Multi-Agent Environment (v0.3)

**Goal:** It's not just Hoss anymore. Your entire agent swarm is visible. See crm-agent working on a lead, paperclip-agent routing data, product-agent building. You're the conductor of an AI orchestra in VR.

### New in v0.3
- [ ] Multi-agent avatar system (render multiple agents)
- [ ] Agent status indicators (working, idle, blocked)
- [ ] Tool skill VR triggers (point at panel + voice = fire skill)
- [ ] Handoff visualization (see tasks moving between agents)
- [ ] Notification system (alerts float into view)
- [ ] Voice commands: "Hoss, tell paperclip to pause"

### Agent Visual Language
| State | Visual |
|-------|--------|
| Working | Glowing, animated |
| Idle | Dim pulse |
| Blocked | Amber warning |
| Done | Green check |

### Tool Skill Triggers
Point at any object in VR and speak a command:
- Point at Terminal → "Run the build" → triggers CI
- Point at Kanban → "Show Spencer's tasks" → Flume query
- Point at Code → "Review this PR" → GitHub agent fires
- Point at Hoss → "Deep research on Solana" → Autoresearch kicks off

### Success Criteria
✅ You see 3 agent avatars working on tasks → point at one and say "pause" → agent animation changes to idle → Hoss confirms

---

## Phase 4 — Persistent World (v1.0)

**Goal:** Holographic HQ becomes your *home base* for everything AI. It remembers where you left panels, what you were working on, and grows with you. Multiplayer opens it up for team collaboration.

### Future Possibilities
- [ ] **Persistent state** — Room layout saved, panels remember position
- [ ] **Multiplayer** — Friends can join your HQ
- [ ] **Custom worlds** — Build custom VR spaces, not just the office
- [ ] **Holographic telepresence** — Project yourself into remote meetings
- [ ] **AI personalities** — Different agents have different spatial presences
- [ ] **3D object creation** — Hoss generates 3D models that appear in VR

---

## Spec: World Elements

### The Room

```
TOP DOWN VIEW (20m x 15m space)

┌────────────────────────────────────────┐
│ ░░░░░░░░░ CEILING (soft glow) ░░░░░░░░ │
│                                        │
│  ┌──────┐           ┌──────────────┐   │
│  │ WEB  │           │   KANBAN    │   │
│  │PANEL │           │   BOARD     │   │
│  └──────┘           └──────────────┘   │
│                                        │
│           ╔═══════════╗               │
│           ║   HOSS     ║  ← You       │
│           ║  (center)  ║    stand     │
│           ╚═══════════╝    here      │
│                                        │
│  ┌──────┐           ┌──────────────┐   │
│  │TERMINAL│         │   CODE      │   │
│  │ PANEL │           │   PANEL    │   │
│  └──────┘           └──────────────┘   │
│                                        │
│ ░░░░░░░░░ FLOOR (dark) ░░░░░░░░░░░░░░ │
└────────────────────────────────────────┘
```

### World Anchors (MR Persistence)

In passthrough MR mode, panels lock to real-world positions using Quest's spatial anchor system:
- Hoss avatar: Fixed 1.5m in front of you at eye level
- Panels: Grab and place anywhere in your room, they stay there
- Whiteboard: Mounted on your real wall

---

## Spec: Tool Skills (VR Triggers)

Skills are triggered by **point + voice command**. The VR controller raycasts from your hand; when it hits a panel, that panel becomes the *target* of the command.

### Voice Command Format
```
"Hey Hoss, [action] on [target]"
```

### Universal Commands
| Command | Action | Target |
|---------|--------|--------|
| "Run build" | Triggers CI/CD | Terminal |
| "Show docs" | Opens docs page | Web |
| "Pause" | Pauses agent | Agent avatar |
| "Status" | Reports agent status | Agent avatar |

### Panel-Specific Commands
| Panel | Commands |
|-------|----------|
| **Terminal** | "Run", "Stop", "Clear", "Tail logs" |
| **Code** | "Execute", "Copy", "Format", "Comment" |
| **Kanban** | "Show [list]", "Add card", "Move to [list]" |
| **Web** | "Navigate to [url]", "Search for [query]", "Scroll up/down" |

### Skill Definition Schema
```yaml
skill:
  name: "Run Build"
  voice_trigger: "run build"
  target: "terminal"
  openclaw_skill: "github-mcp"
  action: "trigger_workflow"
  params:
    workflow_id: "ci.yml"
```

---

## Contributing

Contributions are welcome — this is a big vision and we'll take all the help we can get.

1. Read [Specs/](./Specs/) — understand the spatial design
2. Join the Discord #vr-hq channel
3. Pick up a v0.1 task from the board
4. Build in Unity 2022 LTS
5. PR to `main` with tests

Please read the [Contributing Guide](./CONTRIBUTING.md) first.

---

## License

Distributed under the MIT License. See [LICENSE](LICENSE) for more information.

---

## Contact

**Tyler Delano** - [@tylerdotai](https://x.com/tylerdotai) - tyler.delano@icloud.com

**Project Link:** [https://github.com/tylerdotai/holographic-hq](https://github.com/tylerdotai/holographic-hq)

**Status:** 🗓️ Planned — Sprint TBD

---

*This is the year of spatial AI. Hoss deserves a room.*