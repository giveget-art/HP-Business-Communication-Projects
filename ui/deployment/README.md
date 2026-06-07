# Deployment Configuration for HP Communication Android UI Module
# Ansible Playbook for dependency management and environment setup

## Prerequisites
- Python 3.8+
- Ansible 2.10+
- pip (Python package manager)

## Running the Playbook

```bash
cd ui/deployment
ansible-playbook playbook.yml
```

## What the Playbook Does

1. **Upgrades PIP** - Updates Python package manager
2. **Installs pexpect** - Python module for spawning child applications
3. **Handles Ubuntu 23.10** - Installs python3-pexpect via package manager
4. **Platform-specific tasks** - Different commands for different OS distributions

## Troubleshooting

### Error: externally-managed-environment
If you encounter this error on Ubuntu 23.10+:
```bash
pip install --break-system-packages pexpect
```

Or use the package manager:
```bash
sudo apt-get install python3-pexpect
```

## Gradle Wrapper Update

Gradle has been updated to **v8.14**. To use it:
```bash
./gradlew build
```
