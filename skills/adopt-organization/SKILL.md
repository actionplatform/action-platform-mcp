---
name: adopt-organization
description: Bring a GitHub organization onto a hosted platform — preview, import what the user picks, follow the job, then connect each app's existing pipeline. Remote server only.
---

# Adopting an organization

Remote server (`action-platform mcp --remote`). Importing and connecting CI servers need `org.manage` with an `admin`-scoped token. Linking an app's CI needs `app.configure`, and starting a run needs `app.flow`.

1. `whoami`; then `list_hosts` for the GitHub code host and `list_import_organizations` for the organizations it can read. A missing one: give the user `install_url`.
2. `preview_organization_import` — show the repositories (`imported_as` means already an app, archived ones are usually left out), the GitHub projects, the teams (`exists`) and the people with their status (`member`, `invited`, `invitable`, `no_email`). Let the user pick.
3. Say what `import_organization` will create — one project per repository unless a GitHub project or `project` places it, the apps, the teams, the invitations and the role they get — and wait for a yes.
4. `import_organization`, then `job_status` until `done` or `failed` (`job_logs` shows progress). Report the result: projects, apps, teams, invitations (no email is sent; `list_invitations` has the links) and every skipped entry with its reason.
5. For each app whose pipeline already builds or deploys it:
   - Jenkins: `list_ci_hosts`; none yet → `add_ci_host` with the token in an environment variable the user names and exports (`token_env`), never typed into the chat; `test_ci_host`.
   - `set_app_ci` with the CI server and the job path (or no `ci_host` for GitHub Actions / GitLab CI), then `get_app_ci` with `sync=true` to see its runs. `run_ci` starts it — whatever the pipeline does runs, so ask first.
   - A release the pipeline already shipped: `record_deployment` (the deploy skill).
   - A private repository whose clone answers 401: `set_app_host` with the right code host.
6. Imported repositories without `platform.toml` are apps but not on the platform's rules yet. List them and say the platform files are installed one app at a time from the web app.

Rules that always apply — tools only, ask before anything leaves the machine, stop on refusal: see the `platform-rules` skill.
