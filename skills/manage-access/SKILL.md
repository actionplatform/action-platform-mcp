---
name: manage-access
description: Who has access to what on the services the organization's plugins own (code host, CI, error tracking, container host) — import first, audit the drift, then grant, revoke, offboard or sync with a dry run and an explicit yes. Remote server only.
---

# Managing access

The `access_*` tools exist on the remote server (`action-platform mcp --remote`) and need `org.manage` with an `admin`-scoped token. They cover the services whose plugin the platform bundles and whose options the organization filled in under **Plugins**; `access_audit` names them, with the resources and levels each one grants.

1. `access_audit` — which services are there, which are imported, the drift of each (`drift`), members with no account linked, accounts that belong to no member. A service with `ok: false` carries the reason in `error`; relay it.
2. A service not imported yet (`imported: false`) is imported by `access_sync`: it records what the service holds today and changes nothing there. Show the dry run (`apply=false`) — the proposed mapping and `note` — and ask; then `apply=true`. Accounts that are not the member's email (GitHub logins, Jenkins user ids) go in `links` (`{member email: {service: account}}`). A sync right after must answer no changes.
3. Questions about one person or one service: `access_who` (`person` = email, or `service`).
4. Changes, each a dry run first, shown to the user, then repeated with `apply=true` only after an explicit yes:
   - `access_grant` — one person, one service, one resource and level from the audit's `levels`; `account` when the mapping has none yet.
   - `access_revoke` — the `note` says when a role or team still grants it; that is changed on the platform (role, team), not here.
   - `access_offboard` — every grant of every linked account; `remove_from_platform=true` also ends the membership, and is refused up front for an owner unless an owner asks, and for the last owner.
   - `access_sync` on an imported service — adds, changes and removes to close the drift; read every `remove` to the user before applying.
5. After an apply, report each change's `status`: `applied`, `refused` (the reason — the last administrator of a service is never removed), `failed` (the service's answer).

Rules that always apply — tools only, ask before anything changes outside the machine, stop on refusal: see the `platform-rules` skill.
