# World Spec — Holographic HQ Spatial Design

## Overview

This document details the spatial layout, environment design, and world anchor system for Holographic HQ in VR.

---

## Environment: The Room

**Dimensions:** 20m × 15m virtual space (scales to real room in MR mode)

**Aesthetic:** Neo-Tokyo Control Room
- Dark ambient environment
- Cyan/magenta holographic accents
- Clean geometry, floating panels
- Soft bloom on emissive surfaces

### Color Palette

| Element | Color | Hex |
|---------|-------|-----|
| Background / Void | Void Black | `#0A0A0F` |
| Primary Hologram | Cyan | `#00F5FF` |
| Secondary Hologram | Magenta | `#FF00E5` |
| Accent | Electric White | `#F0F0FF` |
| Warning / Caution | Amber | `#FFAA00` |
| Success | Neon Green | `#00FF88` |
| Hoss Avatar Glow | Gold | `#FFD700` |
| Panel Background | Dark Glass | `#15151F` (70% opacity) |

---

## Spatial Layout

### Top-Down View

```
┌─────────────────────────────────────────────────────────────┐
│ ░░░░░░░░░░░░░░ CEILING GLOW (soft directional) ░░░░░░░░░░░░░ │
│                                                              │
│   ┌─────────┐                        ┌──────────────────┐   │
│   │  WEB    │                        │     KANBAN       │   │
│   │  PANEL  │                        │     BOARD        │   │
│   │ (left)  │                        │    (right)       │   │
│   └─────────┘                        └──────────────────┘   │
│                                                              │
│                      ┌──────────────┐                       │
│                      │              │                       │
│                      │     HOSS     │ ← User stands here    │
│                      │   (center)   │   (2m in front)      │
│                      │              │                       │
│                      └──────────────┘                       │
│                                                              │
│   ┌─────────┐                        ┌──────────────────┐   │
│   │TERMINAL │                        │      CODE        │   │
│   │ PANEL   │                        │     PANEL        │   │
│   │(bottom) │                        │    (bottom)      │   │
│   └─────────┘                        └──────────────────┘   │
│                                                              │
│ ░░░░░░░░░░░░░░░░░░ FLOOR ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │
└─────────────────────────────────────────────────────────────┘
```

### User Position

- Standing, 2m back from Hoss center
- Head height: 1.6m (average)
- Panels at comfortable arm's reach (~0.8m to 1.2m away)
- Slight downward angle for panels (15° tilt toward user)

---

## Panel System

### Panel Base Properties

| Property | Value |
|----------|-------|
| Default size | 1.2m × 0.8m |
| Min size | 0.4m × 0.3m |
| Max size | 3m × 2m |
| Opacity | 70% background, 100% text |
| Corner radius | 0.05m |
| Border | 2px cyan/magenta glow |
| Font size | 0.02m (readable at 1m) |

### Panel Types

#### 1. Display Panel
- Renders markdown/text from Hoss
- Scrollable with controller trigger
- Selectable text (future)

#### 2. Code Panel
- Syntax-highlighted (dark theme, cyan keywords)
- Line numbers
- Scrollable
- Monospace: JetBrains Mono

#### 3. Terminal Panel
- Real-time stdout from Titan commands
- Monospace green-on-black (classic terminal)
- Auto-scroll with tail
- Input line at bottom (future: voice typing)

#### 4. Kanban Panel
- Flume board visualization
- Cards as mini-cards (title + color only)
- Tap card to expand details
- Drag cards between lists (controller grab)

#### 5. Web Panel
- Embedded browser (VR-optimized WebView)
- Controller-based navigation
- Voice "navigate to [url]"

### Panel Interactions

| Action | Controller |
|--------|------------|
| Grab panel | Grip button |
| Move panel | Hold grip + move |
| Resize panel | Two-hand pinch |
| Rotate panel | Two-hand twist |
| Pin panel | Thumbstick click (locks in MR) |
| Close panel | Point + "Close" voice command |

---

## Lighting System

### Ambient
- Environment skybox: Deep blue-purple gradient
- Ambient light: 0.15 intensity, cool blue tint

### Main Directional
- Position: Above and in front (30° down from ceiling)
- Color: Soft white `#F0F0FF`
- Intensity: 0.4
- Casts soft shadows

### Panel Backlights
- Point light behind each panel
- Color matches panel accent (cyan or magenta)
- Intensity: 0.3
- Range: 0.5m (creates halo effect)

### Hoss Rim Light
- Separate warm gold point light behind Hoss
- Intensity: 0.5
- Range: 1m
- Separates Hoss from background

---

## Hoss Avatar

### Base Design
- Height: 1.8m (human scale)
- Form: Stylized humanoid, semi-transparent
- Render: Volumetric light (not solid geometry)
- Color: Gold core `#FFD700` with cyan outline
- Face: Minimal — glowing eyes, subtle mouth animation

### Idle States
- **Default:** Gentle float (0.05m up/down, 4s cycle)
- **Listening:** Slight forward lean, eyes brighten
- **Thinking:** Subtle head tilt, pulse animation
- **Speaking:** Gestures, mouth animation synced to audio

### Gestures (v0.2+)
| Gesture | Trigger | Animation |
|---------|---------|-----------|
| Point | References panel | Arm extends, finger points |
| Nod | Agreement | Head nods 2x |
| shrugs | Uncertainty | Arms up, palms up |
| Write | Code output | Hand writing motion |
| Check | References watch/time | Glance to side |

---

## World Anchors (MR Mode)

In passthrough mixed reality:
- Hoss avatar locked at position relative to user
- Panels can be placed on real surfaces
- Quest spatial anchor system persists placement
- "Reset room" voice command re-arranges to default

### Anchor Priority
1. Hoss avatar (always follows user head position)
2. Main panel cluster (pinned to default positions)
3. User-placed panels (stay where placed)

---

## Environment Assets Needed

- [ ] Floor: Dark reflective plane with subtle grid
- [ ] Ceiling: Emissive panel with soft glow
- [ ] Skybox: Blue-purple gradient HDR
- [ ] Panel prefab: Glass-like material with glow border
- [ ] Hoss avatar model: TBD (custom or stylized human base)
- [ ] Particle system: Ambient floating particles (subtle)

---

## Performance Targets

| Metric | Target |
|--------|--------|
| Frame rate | 72fps (Quest 3 native) |
| Draw calls | < 100 |
| Panel textures | 1 atlas for all UI |
| Hoss avatar | Single shader, no skeleton |

---

## Future Expansion Hooks

- **Multi-floor:** Elevator between floors (executive suite)
- **Outdoor space:** Balcony overlooking VR cityscape
- **Custom rooms:** User-created environments
- **Guest chairs:** Other agents/people appear as avatars
- **3D model viewer:** Drop .glb files into space