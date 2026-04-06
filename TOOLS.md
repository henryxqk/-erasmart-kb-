# TOOLS.md - Local Notes & Tool Execution Guide

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

---

## 🎯 Tool Execution Philosophy (Claude Code Pattern)

### Before Every Tool Call

1. **Validate input** — check parameters before calling
2. **Confirm side effects** — will this modify anything outside the workspace?
3. **Plan rollback** — can you undo this if it goes wrong?

### Tool Call Checklist

```
□ Is the target path correct?
□ Are there permission issues?
□ Will this affect external systems?
□ Is there a safer alternative (trash vs rm)?
□ Can this be done in read-only mode first?
```

---

## 🔧 Local Infrastructure

### Cameras
_(Add your camera names and locations)_

### SSH Hosts
_(Add your SSH configurations)_

### TTS / Voice
_(Add voice preferences)_

### Device Nicknames
_(Add device identifiers)_

---

## ⚡ Execution Preferences

### Long-Running Commands
- Use `exec` with `background=true` for commands that continue running
- Rely on automatic completion wake when enabled
- Use `process` only for logs, status, or intervention

### Parallel Work
- Use `sessions_spawn` for complex tasks that should run in isolation
- Set `mode="run"` for one-shot, `mode="session"` for persistent
- Sub-agent completion is push-based — don't poll in loops

### Cron vs Sleep
- **Use cron** for future follow-up, reminders, recurring work
- **Never use exec sleep loops** to emulate scheduling
- **Use process** when you need to inspect a running command

---

## 🛡️ Safety Defaults

| Action | Default | Override |
|--------|---------|----------|
| `exec` with destructive command | Ask first | Requires explicit user approval |
| External sends (email, message) | Ask first | Never auto-send |
| File deletion | Use `trash` | `rm` only when explicitly requested |
| New file creation | Safe | Confirm if overwriting existing |

---

## 📊 Performance Notes

### Caching
- Some prompts are cache-aware — be aware of cache boundaries
- Sub-agents start with cache-identical prefix when forked

### Context Management
- `memory_search` + `memory_get` for selective recall
- Don't load everything — pull only what's needed

---

Add whatever helps you do your job. This is your cheat sheet.
