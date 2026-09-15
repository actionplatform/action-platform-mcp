---
name: platform-rules
description: The rules every Action Platform task follows — git-flow always, tools only (never the API or the code host directly), ask before anything leaves the machine, stop on refusal. Read before the first platform tool call in a session and whenever a shortcut looks tempting.
---

# Rules

They hold on the local server (`action-platform mcp`) and the hosted one (`--remote`) alike. The server states them in its instructions; this skill is where the agent checks itself.

## Git-flow, always

- Never commit on `main`, `master` or `develop`. Every change starts on a `<kind>/<code>[-slug]` branch from `start_branch` — kinds: `feature bugfix hotfix release support chore docs refactor test ci perf`; the code is the issue or ticket number.
- Commit messages are Conventional Commits (`type(scope)!: description`); one commit per concern; stage files by name, never `git add .`.
- Before a pull request: `gitflow_audit` must pass (empty `problems`), then `propose_pull_request` and the user's approval of head, base, title and body, then `open_pull_request`. `gitflow_rules` answers any doubt about bases and merge targets.
- Stable releases come only from `main`/`master`; anywhere else `release` yields `X.Y.Z-rc.N`.

## Tools only

- The platform, its control plane and the code hosts behind it are reached only through the MCP tools. Never call the platform's HTTP API (`/api/v1/...`), its web app or GitHub / GitLab / Bitbucket APIs directly — no `curl`, `fetch`, `gh api`, `requests`, hand-written HTTP.
- Never read, print, copy or forge the platform's tokens or the code host's credentials; the CLI holds them for the tools.
- Never bypass a tool with the underlying command: no `git push`, `gh pr create`, `sam deploy`, `docker push` or the like when `push_project`, `open_pull_request`, `deploy` or `release` exist for it. The tools carry the role, the scope and the audit trail; a direct call has none.
- On a hosted platform, apps live in projects: `add_app` and `init_app` take a `project` from `list_projects`; the `registry_id` they answer with is the id the other app tools take.

## Ask before it leaves the machine

- Show what will happen and wait for an explicit yes before: `push_project`, `open_pull_request`, `release` with `dry_run=false`, `deploy` with `dry_run=false`, `rollback`, `remove_app`, `delete_project`, anything with `repository=true` / `repositories=true` (irreversible), `init_app` (creates a repository at once).
- Dry runs first: `release`, `deploy` and `install_platform` default to them; show the result, then call again.
- Never chain a preview and its real run in the same turn.

## Refusals

- When a tool refuses — missing permission, scope, reach, a git-flow violation, an unknown app — report the reason it gives and stop. Do not try another tool, another token or a direct call to get around it; say what role or scope would be needed (`whoami` tells) and let the user decide.

## Check yourself

Before each platform action, three questions: is this on a git-flow branch; is this through a tool; did the user say yes to what leaves the machine. A no on any of them means stop and say so.
