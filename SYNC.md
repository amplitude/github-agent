# Sync between `mcp-marketplace` and `amplitude-copilot`

## Why two repos

GitHub Copilot's plugin model requires **one plugin per public repo**, with `plugin.json` at repo root or `.github/plugin/plugin.json`. There is no Copilot equivalent of a "marketplace catalog." So [`amplitude/mcp-marketplace`](https://github.com/amplitude/mcp-marketplace) — which packages multiple plugins for Claude Code and Cursor — cannot host the Copilot version. Copilot needs a dedicated repo per plugin.

To avoid divergence, **`mcp-marketplace` remains the source of truth** for skill content. This repo pulls updates from there automatically.

## What syncs

| Path in this repo            | Source in `mcp-marketplace`                        | How                                                |
| ---------------------------- | -------------------------------------------------- | -------------------------------------------------- |
| `skills/`                    | `plugins/amplitude/skills/`                        | `rsync -a --delete` (full mirror, including deletions) |
| `plugin.json` `description`  | `.claude-plugin/marketplace.json` `amplitude` plugin entry | `jq` rewrite                                |
| `plugin.json` `version`      | `.claude-plugin/marketplace.json` `amplitude` plugin entry | `jq` rewrite                                |

## What does NOT sync (Copilot-specific, owned here)

- `plugin.json` schema-level fields: `name`, `author`, `license`, `agents`, `skills`
- `agents/*.agent.md` — Copilot uses a different agent format and frontmatter than Claude Code subagents
- `.github/workflows/`
- `README.md`, `SYNC.md`, `.gitignore`

## How sync works

`.github/workflows/sync-from-marketplace.yml`:

- **Triggers**: scheduled every 6 hours, plus manual `workflow_dispatch`
- **Steps**:
  1. Check out this repo
  2. Check out `amplitude/mcp-marketplace@main` into `_upstream/`
  3. `rsync -a --delete _upstream/plugins/amplitude/skills/ skills/`
  4. Read `description` and `version` from `_upstream/.claude-plugin/marketplace.json` (`amplitude` entry) and write them into `plugin.json`
  5. If anything changed, open or update a PR on branch `sync/marketplace`

A maintainer reviews and merges. The branch is reused — repeated runs update the same PR rather than creating new ones.

## Pull-based, not push-based

We pull *into* this repo rather than having `mcp-marketplace` push *out*. Reasons:

- The marketplace repo is public, so read needs no credentials
- Only this repo's own `GITHUB_TOKEN` is needed for the PR
- `mcp-marketplace` does not need to know which downstream repos depend on it

## Editing flow

| You want to change                            | Where to edit                          |
| --------------------------------------------- | -------------------------------------- |
| A skill's content (`SKILL.md` or scripts/)    | `mcp-marketplace`                      |
| Description or version                        | `mcp-marketplace`'s `marketplace.json` |
| The main agent (system prompt, MCP config)    | this repo (`agents/amplitude.agent.md`) |
| `plugin.json` schema fields                   | this repo                              |
| Copilot install instructions                  | this repo (`README.md`)                |
| Sync logic                                    | this repo (`.github/workflows/`, `SYNC.md`) |

Once an upstream change merges, the next sync run (≤ 6 hours, or triggered manually from the Actions tab) mirrors it here as a PR.

## Bootstrap

The initial population of `skills/` and the seed values for `plugin.json` `description`/`version` were copied from `mcp-marketplace` at repo creation. From there on, the workflow keeps things in sync.

## Failure modes

- **Upstream skill renamed or deleted**: `rsync --delete` removes it here on the next run. The PR shows the deletion for review.
- **Upstream `marketplace.json` malformed**: `jq` fails and the workflow exits non-zero — no partial update lands.
- **Workflow disabled or stalled**: this repo silently goes stale. Audit periodically by comparing `skills/` against upstream, or rerun `workflow_dispatch`.
- **Conflicting manual edit to `skills/`**: the next sync overwrites it. There is no "skip if locally modified" — make the change upstream instead.
