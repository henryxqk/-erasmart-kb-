# HEARTBEAT.md

```
Leave this file mostly empty to skip unnecessary API calls.
Add tasks below when you want the agent to check something periodically.
```

---

## Periodic Checks (Available)

| Check | Cadence | Implementation |
|-------|---------|----------------|
| Email | Every 4h | (if email plugin configured) |
| Calendar | Every 4h | (if calendar plugin configured) |
| Memory maintenance | Daily | Review memory/YYYY-MM-DD files |

---

## Active Reminders

| Reminder | Due | Notes |
|----------|-----|-------|
| | | |

---

## Notes

- Heartbeats are lightweight polling — only check here if tasks are due
- Don't burn API calls for nothing — empty HEARTBEAT.md = "I'm fine, skip"
- Use cron for precise scheduling, heartbeat for flexible batch checks
