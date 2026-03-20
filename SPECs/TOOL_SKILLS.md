# Tool Skills — VR Triggers

## Overview

Tool Skills enable **spatial voice commands** in VR. Point at any object in Holographic HQ and speak a natural command. The system recognizes the target, interprets the intent, and fires the corresponding OpenClaw skill or API call.

---

## Command Architecture

### Voice Command Flow

```
User (VR) → Voice Input (Quest mic)
          → Speech-to-Text (Whisper)
          → OpenClaw Gateway
          → Intent Parser (target + action)
          → Skill Router
          → OpenClaw Skill / API
          → Result
          → Hoss Avatar (response)
          → XTTS Voice (audio)
          → Quest Audio (Hoss speaking)
```

### Command Format

```
"[action]" on [target]
"Hey Hoss, [action] the [target]"
"[action] [target]"
```

### Examples

| Voice Input | Parsed Action | Target | Result |
|-------------|---------------|--------|--------|
| "Run the build" | run_build | terminal | CI/CD triggers |
| "Show Solana docs" | show_docs | web | Browser navigates |
| "Pause the agent" | pause | agent | Agent stops |
| "What's the status" | status | agent | Status spoken |
| "Add a card for Spencer" | add_card | kanban | Flume card created |

---

## Target System

### Targetable Objects

| Target Type | VR Representation | Valid Actions |
|-------------|------------------|---------------|
| **terminal** | Terminal panel | run, stop, clear, tail |
| **code** | Code panel | execute, copy, format, review |
| **web** | Web panel | navigate, search, scroll, refresh |
| **kanban** | Kanban board | show, add_card, move, filter |
| **hoss** | Hoss avatar | status, research, explain, pause |
| **agent** | Other agent avatar | pause, resume, status, stop |
| **room** | Global space | reset, save, help |
| **whiteboard** | Whiteboard panel | clear, draw, export |

### Raycast Targeting

- Controller emits raycast from tip
- Ray visible as thin cyan line in VR
- Hit detection on panel colliders
- Nearest valid target highlighted (glow pulse)
- Voice command binds to highlighted target

### Target Highlight States

| State | Visual | Trigger |
|-------|--------|---------|
| None | Invisible ray | Controller off |
| Aiming | Dim ray | Controller on |
| Hovering | Bright ray | Ray hits panel |
| Active | Ray + panel glow | Voice command issued |

---

## Action Vocabulary

### Universal Actions

| Action | Synonyms | Valid Targets | Behavior |
|--------|----------|---------------|----------|
| **help** | "what can I do", "commands" | room, any | Speak available commands |
| **status** | "how's it going", "report" | agent, room | Hoss reports status |
| **pause** | "stop", "wait", "hold" | agent, terminal | Pause current action |
| **resume** | "continue", "go" | agent, terminal | Resume paused action |
| **reset** | "restart", "clear" | room, terminal, whiteboard | Reset to default state |
| **save** | "persist", "remember" | room | Save current layout |

### Terminal Actions

| Action | Voice Examples | Behavior |
|--------|---------------|----------|
| **run** | "run build", "execute" | Run predefined command/script |
| **stop** | "stop", "kill it" | Kill running process |
| **clear** | "clear", "cls" | Clear terminal output |
| **tail** | "tail logs", "watch output" | Stream live output |
| **input** | "type [text]" | Input text to terminal |

### Code Panel Actions

| Action | Voice Examples | Behavior |
|--------|---------------|----------|
| **execute** | "run this", "execute" | Execute code via API |
| **copy** | "copy", "duplicate" | Copy code to clipboard |
| **format** | "format", "prettify" | Auto-format code |
| **review** | "review this" | Trigger code review agent |
| **explain** | "explain this" | Hoss explains code verbally |

### Kanban Actions

| Action | Voice Examples | Behavior |
|--------|---------------|----------|
| **show** | "show [list]", "open [board]" | Display specified list |
| **add_card** | "add card [title]" | Create new card |
| **move_card** | "move to [list]" | Move selected card |
| **filter** | "show [person]'s tasks" | Filter by assignee |
| **priority** | "set high priority" | Update card priority |

### Web Panel Actions

| Action | Voice Examples | Behavior |
|--------|---------------|----------|
| **navigate** | "go to [url]", "open [site]" | Navigate browser |
| **search** | "search for [query]" | Search via SearXNG |
| **scroll** | "scroll up", "scroll down" | Scroll page |
| **refresh** | "refresh", "reload" | Reload page |
| **back** | "go back", "back" | Browser back |

---

## Skill Definition Schema

Each skill is defined as YAML:

```yaml
skills:
  - name: "Run Build"
    trigger: "run build"
    target: terminal
    openclaw_skill: "github-mcp"
    action: "trigger_workflow"
    params:
      owner: "tylerdotai"
      repo: "flume"
      workflow_id: "ci.yml"
    confirmation: false  # No confirm for fast actions
    
  - name: "Deep Research"
    trigger: "research {query}"
    target: hoss
    openclaw_skill: "autoresearch"
    action: "start_research"
    params:
      topic: "{query}"
      depth: "comprehensive"
    confirmation: true  # Confirm long-running tasks
    
  - name: "Show Agent Status"
    trigger: "status"
    target: agent
    openclaw_skill: null  # Built-in
    action: "report_status"
    params: {}
    confirmation: false
```

### Parameter Interpolation

Curly braces `{}` in params are replaced with voice parsing output:
- `{query}` — Free text captured after action
- `{target}` — Name of pointed-at object
- `{agent_name}` — Specific agent mentioned

### Confirmation Mode

| Mode | Behavior | Use When |
|------|----------|----------|
| `false` | Execute immediately | Fast actions, non-destructive |
| `true` | "Did you mean X?" | Long-running, destructive, financial |
| `voice` | "Say yes or no" | Ambiguous commands |

---

## Built-In Skills

These are always available without external skill loading:

### System Skills

| Skill | Voice | Behavior |
|-------|-------|----------|
| **Help** | "help", "what can I do" | Hoss lists available commands |
| **Status** | "status", "how are you" | Hoss reports system status |
| **Reset Room** | "reset room" | Restore default panel layout |
| **Save Room** | "save room" | Persist current layout |
| **Switch to VR** | "VR mode" | Ensure VR is active |
| **Switch to Desktop** | "desktop mode" | Exit VR gracefully |

### Flume Integration

| Skill | Voice | Behavior |
|-------|-------|----------|
| **Show Board** | "show [board name]" | Display Flume board in Kanban panel |
| **Add Task** | "add task [title]" | Create card via Flume API |
| **My Tasks** | "my tasks", "what's next" | Show user's assigned cards |
| **Spencer's Tasks** | "Spencer's tasks" | Filter by Spencer assignee |

### GitHub Integration

| Skill | Voice | Behavior |
|-------|-------|----------|
| **Run CI** | "run CI", "run build" | Trigger GitHub Actions workflow |
| **PR Status** | "PR status" | Report open PR status |
| **Create Issue** | "create issue [title]" | Open new GitHub issue |

---

## Skill Loading (Future)

### Dynamic Skill Installation

```
User: "Hoss, install the Solana agent skill"
Hoss: "Installing Solana agent skill..."
      → Fetches from ClawHub
      → Installs to OpenClaw skills/
      → Registers with VR trigger system
      → "Solana agent skill installed. Point at Hoss and say 'research Solana'."
```

### Skill Marketplace (v1.0)

- VR panel shows available skills
- Browse by category
- One-click install
- Skills appear as new targetable objects

---

## Error Handling

### Unknown Command
- Hoss: "I didn't catch that. Try 'help' for available commands."
- Suggests similar known commands

### Target Not Found
- Hoss: "I'm not sure which [target] you mean. Try pointing at a panel."
- Raycast reticle flashes amber

### Skill Not Available
- Hoss: "That skill isn't loaded. Say 'install [skill name]' to add it."
- Opens skill marketplace panel (future)

### Permission Denied
- Hoss: "I don't have permission to do that. Check your API keys."
- Error logged with timestamp

### Network Error
- Hoss: "I'm having trouble reaching [service]. Let me try again."
- Retry with exponential backoff (max 3 tries)

---

## Voice Recognition Tuning

### Hotword
- "Hey Hoss" — always listening for this first
- False trigger rate: < 1% with 3-gram model
- Recognition: Vosk (offline) or Cloud API (configurable)

### Command Parsing
- Whisper model: large-v3-turbo (fast, accurate)
- Fallback: cloud STT if offline
- Ambient noise threshold: -40dB

### Multilingual (Future)
- [ ] English (default)
- [ ] Spanish
- [ ] Mandarin
- [ ] French