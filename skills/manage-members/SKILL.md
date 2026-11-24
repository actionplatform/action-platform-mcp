---
name: manage-members
description: Invite people into the organization, cancel invitations, take someone off a team or out of the organization — remote server, with the role shown and an explicit yes for every change.
---

# Managing members

These tools exist on the remote server (`action-platform mcp --remote`) and need `org.manage` with an `admin`-scoped token that is not limited to a project or app. A refusal names the missing role or scope: relay it and stop.

1. `whoami`, then `list_members`, `list_teams` and `list_invitations` — who is there, with which role, and who is invited.
2. Inviting: say the email and the role (`viewer`, `developer`, `deployer`, `admin`, `owner`) and wait for a yes; then `invite_member`. No email is sent: give the user the `link` from the answer to pass on — the person opens it and signs in or signs up with that email. Only an owner invites an owner.
3. `cancel_invitation` with the id from `list_invitations`; the link stops working.
4. `remove_team_member` takes someone off one team and leaves them in the organization. `remove_member` ends the membership — every project and team here — and leaves the account. Name the person and what they lose, and ask first. Only an owner removes an owner.
5. Roles change with `set_member_role`. Access on the services the plugins own (code host, CI, error tracking) is the `manage-access` skill: offboarding someone everywhere is `access_offboard` there.

Rules that always apply — tools only, ask before anything changes, stop on refusal: see the `platform-rules` skill.
