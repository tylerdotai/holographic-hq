# Contributing to Holographic HQ

�Thanks for wanting to build the future of spatial AI.

---

## How to Contribute

### 1. Read the Specs First

Before writing any code, read the specification documents:

- [WORLD_SPEC.md](./SPECs/WORLD_SPEC.md) — Spatial design, layout, environment
- [AVATAR_SPEC.md](./SPECs/AVATAR_SPEC.md) — Hoss avatar behavior and animations
- [TOOL_SKILLS.md](./SPECs/TOOL_SKILLS.md) — Voice command system

These define **what** we're building. The code implements **how**.

### 2. Pick a Task

- Look at the Roadmap in [README.md](./README.md)
- Check open issues tagged `good-first-issue` or `v0.1`
- Comment on the issue before starting (avoid duplicate work)

### 3. Set Up Your Dev Environment

**Unity Setup:**
```bash
# Clone the repo
git clone https://github.com/tylerdotai/holographic-hq
cd holographic-hq/Unity

# You'll need:
# - Unity 2022 LTS
# - XR Interaction Toolkit
# - Meta XR SDK (Quest development)
# - Universal Render Pipeline

# See Unity/README.md for full setup guide
```

**OpenClaw Bridge (Python):**
```bash
cd OpenClawBridge
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 4. Development Standards

**Code:**
- C# for Unity scripts (use namespaces: `HolographicHQ.*`)
- Python for OpenClaw bridge (use modules: `openclaw_bridge.*`)
- 100 character line limit
- Document public methods with XML comments

**Commits:**
```
feat: add gesture system for avatar
fix: correct panel raycast targeting
docs: update WORLD_SPEC with new lighting values
test: add voice command parsing unit tests
```

**PR Title Format:**
```
[type]: [short description]

[longer description if needed]

Fixes #[issue]
```

### 5. Testing

**Unity:**
- All prefabs must have no missing references
- Test in editor with XR Device Simulator before PR
- Test on Quest 3 hardware if possible

**Bridge:**
```bash
cd OpenClawBridge
pytest tests/ -v
```

**Voice Commands:**
- Test each skill trigger manually in VR
- Record latencies (voice → response)
- Log any false trigger or misrecognition

### 6. Submit PR

- PR to `main` branch
- Fill out the PR template
- Request review from @tylerdotai
- Link to the relevant spec files

---

## Project Structure

```
holographic-hq/
├── Unity/                    # Unity VR project
│   ├── Assets/
│   │   ├── Scripts/         # C# code
│   │   ├── Prefabs/         # Pre-made objects
│   │   ├── Scenes/          # Unity scenes
│   │   └── Shaders/         # Custom shaders
│   └── ProjectSettings/     # Unity config
├── OpenClawBridge/          # Python bridge server
│   ├── server.py
│   ├── avatar_controller.py
│   ├── xtts_player.py
│   └── requirements.txt
├── Specs/                   # Design documents
│   ├── WORLD_SPEC.md
│   ├── AVATAR_SPEC.md
│   └── TOOL_SKILLS.md
├── .assets/                 # Images, logos
├── LICENSE
├── CONTRIBUTING.md
└── README.md
```

---

## Ideas for Contributions

### High Priority (v0.1)
- [ ] Unity project setup with XR configuration
- [ ] Basic VR scene with dark environment
- [ ] Holographic panel prefab
- [ ] WebSocket client in Unity
- [ ] Voice input (push-to-talk)

### Medium Priority (v0.2)
- [ ] Gesture animation system
- [ ] Panel grab/resize/move
- [ ] Spatial audio setup
- [ ] XTTS integration

### Lower Priority (future)
- [ ] Multi-agent avatars
- [ ] Tool skill trigger system
- [ ] MR passthrough calibration
- [ ] Avatar customization

---

## Questions?

- Open an issue for bugs
- Ask in Discord #vr-hq channel
- Tag @tylerdotai for urgent issues

---

*Let's build a world where AI lives.*