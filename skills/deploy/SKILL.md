---
name: deploy
description: Ship a release to one of the app's scopes — check the scope exists and the release is deployable, preflight first, real deploy only on approval, then verify.
---

# Deploying

A deploy always ships a **release** (a tag) to a **scope** (a named destination with a kind and a criticality). No scope, no deploy. `deploy` defaults to `dry_run=true`, which runs preflight only: tooling, template validation, credentials.

1. `project_info` — `[deploy] target` must exist; if not, use the add-cloud skill.
2. `list_scopes` (hosted) — the app's scopes with their criticality. None yet? `create_scope` with the name, kind (`web`, `job`, …) and criticality (`test`, `low`, `medium`, `high`, `critical`) the user names; show what will exist first. Locally the scopes are `[[scopes]]` in `platform.toml`.
3. Pick the release: the tag the user names, else the latest. Criticality decides what a scope takes — `test` and `low` take candidates (`-rc.N`), stable and hotfix releases; `medium` and above take stable and hotfix only. A candidate aimed at `prod` will be refused; say so before calling.
4. `deploy` (dry run) with `stage` = the scope's name and `version`. A `409` means the release's readiness for that scope is blocked — report the checks the error names (permissions, stack state, release shape) instead of forcing; `force` is for an organization manager who read them.
5. On approval, `deploy` with `dry_run=false`. Report the URL.
6. `diagnose` afterwards and report status; `list_deployments` shows what arrived and whether it was verified at the destination.

Rules that always apply — git-flow, tools only, ask before it leaves the machine: see the `platform-rules` skill.
