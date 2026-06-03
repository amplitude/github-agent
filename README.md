# Amplitude for GitHub Agent HQ

Official Amplitude plugin for GitHub Agent HQ. Wraps Amplitude skills, agents, and the Amplitude MCP server into an Agentic App that runs inside the Copilot harness. You can invoke it via @-mention, issue assignment, or Mission Control.

For direct use in other coding agents, the full marketplace of skills is available at [`amplitude/mcp-marketplace`](https://github.com/amplitude/mcp-marketplace).

---

## Install with [Amplitude GitHub app](https://github.com/apps/amplitude) (Amplitude US endpoint only)

Make sure you are an admin of your Amplitude org and your GitHub org.

1. Goto your [Amplitude profile page](https://app.amplitude.com/user-profile)
2. In the "GitHub Integration" section, click the `Connect` button
3. Choose the repositories
4. Confirm the installation. You'll be redirected back to Amplitude when it's done.

To use it, select "Amplitude" from the dropdown in GitHub agents, or tag `@amplitude[agent]` in an assignee field or comment.

---

## What's Inside

Includes 27 skills, plus a main `amplitude` agent that wires up the Amplitude MCP server.

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
```

---

## Source of Truth

Canonical skill content lives in [`amplitude/mcp-marketplace`](https://github.com/amplitude/mcp-marketplace). For v0, skills are copied into this repo manually. To make changes, submit them to `amplitude/mcp-marketplace` first.

---

## Requirements

- GitHub Agent HQ (preview): Install via the Agentic App, or test locally with `copilot plugin install`
- Amplitude account with API access

---

## License

MIT
