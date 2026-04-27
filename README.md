# Amplitude for GitHub Agent HQ

Official Amplitude plugin for GitHub Agent HQ. Wraps Amplitude skills, agents, and the Amplitude MCP server into an Agentic App that runs inside the Copilot harness — invokable via @-mention, issue assignment, or Mission Control.

For direct use in other coding agents, the full marketplace of skills is available at [`amplitude/mcp-marketplace`](https://github.com/amplitude/mcp-marketplace).

---

## What's Inside

27 skills, plus a main `amplitude` agent that wires up the Amplitude MCP server.

| Area                       | Skills                                                                                                                                       |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Core analytics             | `create-chart`, `create-dashboard`, `analyze-chart`, `analyze-dashboard`                                                                     |
| Product insights           | `analyze-experiment`, `monitor-experiments`, `analyze-feedback`, `analyze-account-health`, `discover-opportunities`, `compare-user-journeys` |
| Session replay & debugging | `debug-replay`, `replay-ux-audit`, `diagnose-errors`, `monitor-reliability`                                                                  |
| AI agent analytics         | `analyze-ai-topics`, `investigate-ai-session`, `monitor-ai-quality`, `review-agent-insights`                                                 |
| Analytics instrumentation  | `diff-intake`, `discover-event-surfaces`, `discover-analytics-patterns`, `instrument-events`, `add-analytics-instrumentation`, `taxonomy`    |
| Briefings                  | `daily-brief`, `weekly-brief`                                                                                                                |
| Bonus                      | `what-would-lenny-do`                                                                                                                        |

---

## Repository Structure

```text
plugin.json              # Agent HQ plugin manifest (Copilot CLI plugin spec)
agents/
  amplitude.agent.md     # Main agent (@-mention / Mission Control entry point)
skills/
  <skill-name>/
    SKILL.md
.github/
  workflows/
    sync-from-marketplace.yml
SYNC.md                  # How sync from mcp-marketplace works
```

---

## Source of Truth

Canonical skill content lives in [`amplitude/mcp-marketplace`](https://github.com/amplitude/mcp-marketplace) and is mirrored into this repo by CI. See [SYNC.md](./SYNC.md) for the design.

**Direct edits to `skills/` here will be overwritten.** Submit changes upstream.

Agent HQ-specific files (`plugin.json`, `agents/`, `.github/`, `README.md`, `SYNC.md`) are owned by this repo and edited directly.

---

## Requirements

- GitHub Agent HQ (preview) — install via the Agentic App, or test locally with `copilot plugin install`
- Amplitude account with API access

---

## License

MIT
