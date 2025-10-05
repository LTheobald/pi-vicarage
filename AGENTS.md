# Repository Guidelines

## Project Structure & Module Organization
The control node uses declarative Ansible content. Playbooks live in `playbooks/` (e.g., `site.yml` for full fleet orchestration and `update.yml` for patch-only runs). Host grouping and defaults sit in `inventory.ini`, `group_vars/`, and optional per-device files in `host_vars/`. Roles are split per device domain inside `roles/<role>/` with task logic in `tasks/main.yml`, notifications in `handlers/main.yml`, and reusable variables co-located under `vars/` when needed. Edit only the role you mean to touch; shared idempotent baselines belong in `roles/common`.

## Build, Test, and Development Commands
Run `ansible all -m ping` after inventory edits to confirm SSH access. Deploy the entire fleet with `ansible-playbook playbooks/site.yml`, or target updates only via `ansible-playbook playbooks/update.yml`. Limit runs to a host group with `--limit nas` (replace as needed) and dry-run changes with `--check`. Use `ansible-playbook playbooks/site.yml --syntax-check` any time YAML structure changes.

## Coding Style & Naming Conventions
All YAML uses two-space indentation and lowercase keys. Task names should be imperative ("Ensure packages are installed") and describe intent, not implementation. Prefer Ansible built-ins (`apt`, `lineinfile`) over shell commands. Variables are snake_case and scoped: global defaults in `group_vars/all`, host specifics in `host_vars/<hostname>.yml`, secrets as `vault_*` entries. Jinja templating stays inside double braces with explicit filters.

## Testing Guidelines
Before opening a PR, run syntax check, `--check`, and, when touching package installs, a targeted playbook against a non-production host or `--limit staging`. Record the command and outcome in the PR. If you maintain additional verification (e.g., `ansible-lint`), document overrides in-line via `# noqa` comments sparingly.

## Commit & Pull Request Guidelines
Commits follow the short, capitalized imperatives already in history ("Create MergerFS config"). Group related changes into a single commit so diffs stay reviewable. Pull requests should summarize the host groups impacted, reference issue IDs when available, and include the exact validation commands executed and any follow-up tasks. Screenshots are only needed when modifying UI-facing assets.

## Security & Configuration Tips
Encrypt shared credentials with `ansible-vault create group_vars/all/vault.yml` and reference as `{{ vault_* }}`. Disable SSH password auth only after confirming key distribution via the common role toggle `disable_password_auth`. Never commit real inventory secrets; use sample data when demonstrating patterns.
