# Pi Fleet Ansible (Starter)

This repository manages your Raspberry Pi fleet from your Mac using Ansible.

## 0) Install Ansible on macOS
```bash
# If you use Homebrew (recommended)
brew install ansible

# (Optional) create a venv and install via pip
# python3 -m venv .venv && source .venv/bin/activate && pip install ansible
```

## 1) Generate and distribute SSH keys
```bash
# Create a key if you don't have one
ssh-keygen -t ed25519 -C "mac-control-node"

# Copy your key to each Pi (replace hosts or use IPs from inventory/hosts.ini)
# If ssh-copy-id isn't installed: brew install ssh-copy-id
ssh-copy-id pi@192.168.1.10
ssh-copy-id pi@192.168.1.11
ssh-copy-id pi@192.168.1.12
ssh-copy-id pi@192.168.1.13
```

_No ssh-copy-id? Use:_
```bash
cat ~/.ssh/id_ed25519.pub | ssh pi@192.168.1.10 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys"
```

## 2) Verify connectivity
```bash
ansible all -m ping
```

## 3) Keep systems updated
Run weekly or on demand:
```bash
ansible-playbook playbooks/update.yml
```

## 4) Deploy apps
- Picture frame app: `ansible-playbook playbooks/deploy_picture_frame.yml --limit frame`
- Door panel app: `ansible-playbook playbooks/deploy_door_panel.yml --limit door`

## 5) Secrets with Vault
```bash
# Create a vault file for all hosts
ansible-vault create group_vars/all/vault.yml
# Then reference variables like {{ vault_wifi_psk }}
```

## 6) Layout
See tree below:
```
playbooks/
roles/
inventory/
group_vars/
host_vars/ (optional per-host settings)
```
```


## 7) Docker services
- Docker Engine and Compose V2 are provisioned automatically via the new `docker` role.
- Compose bundles are rendered on each host under `/opt/containers/<project>/docker-compose.yml`.
- Add global stacks to `group_vars/all.yml` under `docker_compose_projects`; extend per host via `host_vars/<hostname>.yml` in `docker_compose_projects_host`.
- Run `ansible-playbook playbooks/site.yml --limit <host>` after editing compose definitions to redeploy containers (`--check` previews file changes only).
