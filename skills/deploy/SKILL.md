---
name: deploy
description: Ship a release to one of the app's scopes — check the scope exists and the release is ready for it, preflight first, real deploy only on approval, follow the job, then verify.
---

# Deploying

A deploy always ships a **release** (a tag) to a **scope** (a named destination with a kind and a criticality). No scope, no deploy. `deploy` defaults to `dry_run=true`, which runs preflight only: tooling, template validation, credentials.

1. `project_info` — `[deploy] target` must exist; if not, use the add-cloud skill.
2. `list_scopes` — the scopes with their criticality: on the hosted platform the app's scopes, locally `[[scopes]]` of `platform.toml`. None yet on the platform? `create_scope` with the name, kind (`web`, `job`, …) and criticality (`test`, `low`, `medium`, `high`, `critical`) the user names, shown first. `update_scope` changes a scope's criticality or kind.
3. Pick the release: the tag the user names, else the latest. Criticality decides what a scope takes — `test` and `low` take candidates (`-rc.N`), stable and hotfix releases; `medium` and above take stable and hotfix only. A candidate aimed at `prod` will be refused; say so before calling.
4. Readiness for that release and scope — hosted: `release_readiness`; locally: `readiness`. `blocked` means the deploy will fail there: show each failing check with its `fix` and stop. Hosted with no row for the scope: `recheck_readiness`, follow the job with `job_status`, then read `release_readiness` again.
5. `deploy` (dry run) with `stage` = the scope's name and `version`. A `409` means the readiness is blocked — report the checks instead of forcing; `force` is for an organization manager who read them.
6. On approval, `deploy` with `dry_run=false`. Hosted, pass `background=true`: the answer is a job id; follow it with `job_logs` (`after` = the last `next`) until `finished`, then `job_status`. Report the URL.
7. `diagnose` afterwards and report status; `list_deployments` shows what arrived and whether it was verified at the destination. A delivery made outside the platform is written down with `record_deployment`.

Rules that always apply — git-flow, tools only, ask before it leaves the machine: see the `platform-rules` skill.
