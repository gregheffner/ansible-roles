# System Update Role

This Ansible role handles system updates for Debian/Ubuntu and macOS systems, including Docker container management and optional reboot functionality.

## Features

- **Docker Management**: Restart all Docker containers
- **Package Updates**: 
  - APT packages for Debian/Ubuntu systems
  - Homebrew packages for macOS systems
- **System Maintenance**: Package cleanup and distribution upgrades
- **Reboot Support**: Optional system reboot with post-reboot script execution

## Requirements

- Ansible 2.9+
- Appropriate privileges for package management (sudo/become)
- Docker (optional, for Docker-related tasks)
- Homebrew (optional, for macOS systems)

## Role Variables

### Docker Settings
```yaml
update_docker: true                    # Enable Docker tasks
restart_docker_containers: true       # Restart all containers
```

### Package Update Settings
```yaml
update_packages: true                  # Enable package updates
update_apt_packages: true             # Update APT packages (Debian/Ubuntu)
perform_dist_upgrade: true            # Perform distribution upgrade
cleanup_packages: true                # Clean up unused packages
```

### Homebrew Settings (macOS)
```yaml
update_homebrew: true                  # Enable Homebrew tasks
update_homebrew_packages: true        # Update Homebrew itself
upgrade_homebrew_packages: true       # Upgrade installed packages
cleanup_homebrew_packages: true       # Clean up old package versions
```

### Reboot Settings
```yaml
perform_reboot: false                  # Enable automatic reboot
force_reboot: false                    # Force reboot regardless of necessity
reboot_method: "modern"               # "modern" or "legacy"
reboot_timeout: 600                   # Timeout for reboot operation
connect_timeout: 20                   # Connection timeout after reboot
```

### Post-Reboot Script
```yaml
post_reboot_script_path: "/opt/1password/op-login.sh"  # Script to run after reboot
```

## Example Playbook

### Basic Usage
```yaml
---
- hosts: localhost
  roles:
    - system_update
```

### With Custom Variables
```yaml
---
- hosts: localhost
  vars:
    perform_reboot: true
    post_reboot_script_path: "/opt/1password/op-login.sh"
  roles:
    - system_update
```

### Advanced Configuration
```yaml
---
- hosts: localhost
  vars:
    update_docker: true
    restart_docker_containers: true
    perform_dist_upgrade: true
    cleanup_packages: true
    perform_reboot: true
    reboot_method: "modern"
    post_reboot_script_path: "/opt/1password/op-login.sh"
  roles:
    - system_update
```

## Usage Examples

1. **Update only packages (no reboot)**:
   ```yaml
   - hosts: localhost
     vars:
       perform_reboot: false
     roles:
       - system_update
   ```

2. **Full system update with reboot and 1Password login**:
   ```yaml
   - hosts: localhost
     vars:
       perform_reboot: true
       post_reboot_script_path: "/opt/1password/op-login.sh"
     roles:
       - system_update
   ```

3. **Docker-only updates**:
   ```yaml
   - hosts: localhost
     vars:
       update_packages: false
       update_homebrew: false
       perform_reboot: false
     roles:
       - system_update
   ```

## File Structure
```
roles/system_update/
├── defaults/main.yml      # Default variables
├── handlers/main.yml      # Handlers
├── meta/main.yml         # Role metadata
├── tasks/
│   ├── main.yml         # Main task file
│   ├── docker.yml       # Docker-specific tasks
│   ├── debian.yml       # Debian/Ubuntu tasks
│   ├── homebrew.yml     # macOS Homebrew tasks
│   └── reboot.yml       # Reboot and post-reboot tasks
└── README.md           # This file
```

## License

MIT

## Author Information

Created for automated system maintenance and updates.