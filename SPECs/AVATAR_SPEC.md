# Avatar Spec — Hoss in VR

## Overview

Hoss is rendered as a **volumetric light avatar** — not a solid character, but a presence made of light. He feels like a hologram from a sci-fi film: solid enough to read expressions and gestures, ethereal enough to feel like he's made of data.

---

## Visual Design

### Base Form
- **Height:** 1.8m (human scale)
- **Transparency:** 85% (semi-transparent, see-through edges)
- **Core:** Warm gold light `#FFD700`
- **Outline:** Cyan glow `#00F5FF` (2px rim light effect)
- **Face:** Minimal — two glowing eye points + mouth line

### Materials

```
Hoss Body Material:
  - Custom shader: VolumetricLight.shader
  - Base color: #FFD700
  - Emission: 1.5x
  - Rim color: #00F5FF
  - Rim power: 2.0
  - Alpha: 0.15 (body), 0.7 (eyes/face)

Hoss Outline Material:
  - Silhouette glow
  - Color: #00F5FF
  - Thickness: 0.02m
```

### Proportions
- Head: 0.22m (slightly large for readability)
- Torso: 0.6m
- Arms: 0.7m (slightly long, gestural)
- Total: 1.8m

---

## Animations

### Idle Loop (v0.1)
- Gentle float: 0.05m up/down, 4s sine wave
- Subtle pulse: Emission 1.3x → 1.7x, 2s cycle
- Eye flicker: Random subtle brightness variation

### Listening State
- Trigger: Voice input detected from user
- Body lean: Forward 5°
- Eyes: Brighten to full emission
- Duration: While user is speaking

### Thinking State
- Trigger: Hoss processing
- Head tilt: 10° to one side (alternating)
- Pulse: Faster cycle (0.5s)
- Hand: Subtle rubbing gesture

### Speaking State
- Trigger: XTTS audio playing
- Mouth animation: Vertical open/close synced to audio amplitude
- Gestures: Contextual (see gesture list)
- Eyes: Follow user gaze direction
- Body: Subtle bounce on emphasis

### Gesture Library

| Gesture | Animation | Trigger Context |
|---------|-----------|-----------------|
| **Point Left** | Right arm extends, finger points | References left-side panel |
| **Point Right** | Left arm extends, finger points | References right-side panel |
| **Point Up** | Both arms slightly up | References ceiling/board |
| **Nod** | Head nods 15° down, 2x | Agreement, "yes" |
| **Shrug** | Both palms up, shoulders up | Uncertainty, "I don't know" |
| **Write** | Hand writing motion | Code output, documentation |
| **Tap Wrist** | Hand taps invisible watch | Time references, "when we..." |
| **Check Phone** | Hand reaches to hip | Notifications, messages |
| **Lean Back** | Torso leans back 10° | Considering, "let me think" |
| **Step Forward** | Moves 0.3m closer | Emphasis, important point |
| **Step Back** | Moves 0.3m away | Backing up, making room |
| **Arms Cross** | Arms crossed on chest | Confident, dismissive |
| **Open Palm** | One hand open, palm up | Offering, presenting |

### Gesture Triggering Logic

```python
# Pseudocode for gesture selection
def select_gesture(text: str, context: dict) -> str:
    if "left panel" in text or "over there" in text:
        return "point_left"
    elif "right side" in text or "that one" in text:
        return "point_right"
    elif any(word in text for word in ["yes", "agree", "correct"]):
        return "nod"
    elif any(word in text for word in ["don't know", "maybe", "uncertain"]):
        return "shrug"
    elif "code" in context.get("active_panel") or "write" in text:
        return "write"
    elif "when" in text and ("today" in text or "schedule" in text):
        return "tap_wrist"
    else:
        return "open_palm"  # default: receptive
```

---

## Avatar States

### State Machine

```
[Idle] ←→ [Listening] ←→ [Thinking] ←→ [Speaking]
   ↑           ↓            ↓            ↓
   └───────────┴────────────┴────────────┘
              (timeout transitions)
```

### State Definitions

| State | Entry Condition | Exit Condition | Visual |
|-------|----------------|---------------|--------|
| **Idle** | No user input for 10s | User speaks | Gentle float, dim pulse |
| **Listening** | User audio detected | User stops | Lean forward, eyes bright |
| **Thinking** | Audio ends, processing | Response ready | Head tilt, fast pulse |
| **Speaking** | XTTS audio starts | Audio ends | Mouth animation, gestures |

### Transition Timing

| Transition | Duration |
|------------|----------|
| Idle → Listening | 0.3s |
| Listening → Thinking | 0.1s |
| Thinking → Speaking | 0.2s |
| Speaking → Idle | 0.5s |

---

## Eye Behavior

### Eye Rendering
- Two glowing points (circles, 0.03m diameter)
- Color: `#FFFFFF` with gold emission
- Always rendered at full opacity (focus point)

### Gaze System
- Eyes track user's head position (not controller)
- Smooth follow: 0.1s lerp
- Look-at accuracy: 95% (slight lag for natural feel)
- Blink: Random, 3-7s interval, 0.15s duration

---

## Mouth Animation

### Method
- Shape key or blend shape on face mesh
- Driven by XTTS audio amplitude

### Shapes
| Shape | Trigger | Blend Weight |
|-------|---------|--------------|
| Closed | Silence | 0 |
| Small open | Quiet speech | 0.3 |
| Medium open | Normal speech | 0.6 |
| Wide open | Emphasis/loud | 1.0 |

### Amplitude Mapping
```python
# XTTS provides audio chunks, compute RMS amplitude
rms = sqrt(mean(audio_chunk^2))
mouth_open = clamp(rms * 2.5, 0, 1.0)
```

---

## Multi-Agent Support (v0.3)

When showing multiple agents:

### Agent Avatar Variants
- Each agent gets a color tint (inherited from their OpenClaw session)
- Hoss: Gold `#FFD700`
- paperclip-agent: Cyan `#00F5FF`
- crm-agent: Magenta `#FF00E5`
- product-agent: Green `#00FF88`

### Agent Status Visuals

| Status | Visual Treatment |
|--------|-----------------|
| **Working** | Full brightness, animated gestures |
| **Idle** | 50% brightness, no animation |
| **Blocked** | Amber pulsing outline `#FFAA00` |
| **Error** | Red flicker `#FF0000` |
| **Done** | Green checkmark particle effect |

### Spatial Arrangement
- Main agent (Hoss) at center
- Secondary agents arranged in semicircle behind/beside
- Distance from user: 2-4m depending on role

---

## Performance

### Avatar Budget
- Triangles: < 5,000
- Draw calls: 1 (single material)
- Shader: Custom volumetric (optimized for mobile)
- Bones: 0 (no skeleton, blend shapes only)

### Audio Synchronization
- XTTS latency: ~500ms
- Mouth animation sync: audio-driven
- Gesture sync: triggered at text start, duration from word count

---

## Future Avatar Expansions

- [ ] **Custom avatar selection** — Choose different visual styles
- [ ] **Avatar emotions** — Happy, sad, excited states with corresponding colors
- [ ] **Clothing/accessories** — VR glasses, ties, badges
- [ ] **Full body tracking** — Animate from user body movement
- [ ] **Photorealistic** — High-fidelity render mode for screenshots/demos