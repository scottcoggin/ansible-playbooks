# Ansible Playbooks

Ansible playbooks for provisioning and managing my homelab systems.

## Contents

- **debian13/**: Debian 13 system setup - installs development tools, configures zsh with oh-my-zsh, adds Azure CLI and Terraform
- **win11/**: Windows 11 automated patching via Windows Update

## Prerequisites

- Ansible installed on your control machine
- SSH access to Debian systems
- WinRM configured on Windows systems

## Usage

### Debian 13 Provisioning

```bash
cd debian13
ansible-playbook -i inventory.ini debian13.yml
```

This playbook:
- Updates and upgrades all packages
- Installs base tools (curl, git, vim, zsh, LaTeX)
- Configures zsh as default shell with oh-my-zsh and plugins
- Installs Azure CLI from Microsoft's apt repository
- Installs Terraform from HashiCorp's apt repository

### Windows 11 Patching

```bash
cd win11
ansible-playbook -i inventory.ini win11.yml
```

This playbook:
- Installs PSWindowsUpdate PowerShell module
- Runs Windows Update for all categories (Security, Critical, Updates, etc.)
- Automatically reboots as needed
- Repeats until system is fully patched

**Note**: This can take a long time on systems that haven't been patched recently.

## Inventory Files

Inventory files (`inventory.ini`) contain IP addresses and credentials and are gitignored for security. Create them in each directory:

**Debian example**:
```ini
[debian]
10.0.0.50 ansible_user=your_user ansible_become=yes
```

**Windows example**:
```ini
[windows]
10.0.0.49

[windows:vars]
ansible_connection=winrm
ansible_port=5985
ansible_winrm_scheme=http
ansible_winrm_transport=ntlm
ansible_user=.\ansible
ansible_password=YourPassword
```

## Notes

- All `.ini` files are gitignored to protect sensitive information
- Playbooks are idempotent - safe to run multiple times
- Windows playbook may require up to 6 update cycles for fully patching
