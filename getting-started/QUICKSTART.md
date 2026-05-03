# Quick Start Guide

Get started with Treelinux in 5 minutes.

## First Boot

```bash
# Boot the VM
qemu-system-x86_64 -m 2G \
  -drive file=treelinux-base.qcow2,format=qcow2 \
  -netdev user,id=n1,hostfwd=tcp::2222-:22 \
  -device e1000,netdev=n1 \
  -display gtk

# SSH in
sshpass -p "treelinux" ssh -o StrictHostKeyChecking=no -p 2222 root@localhost
```

## Package Management

### Check System Status

```bash
sprout status
```

Shows installed packages, enabled services, and config sync status.

### Search for Packages

```bash
sprout search firefox
sprout search "text editor"
```

### Install Packages

```bash
# Install a single package
sprout install vim

# Or add to system.prc and apply
echo "	vim" >> /etc/treelinux/system.prc
sprout apply --non-interactive
```

### Remove Packages

```bash
# Remove a package
sprout remove vim

# Or remove from system.prc and apply
# Edit /etc/treelinux/system.prc, remove the line, then:
sprout apply  # will prompt before removing
```

### Upgrade All Packages

```bash
sprout upgrade
```

### Update Package Index

```bash
sprout update
```

## Configuration

### system.prc Format

Location: `/etc/treelinux/system.prc`

```
packages
	alpine-base
	openssh
	neovim
	firefox

services
	sshd

modules
	update-sprout.smp
```

### Add Packages to Config

```bash
# Edit the config
vi /etc/treelinux/system.prc

# Or use sprout install (adds to config automatically)
sprout install vim

# Apply changes
sprout apply --non-interactive
```

### Include Other Configs

Split your config across files:

`system.prc`:
```
packages
	alpine-base
	openssh

include desktop.prc
```

`desktop.prc`:
```
packages
	i3
	dmenu
```

## Modules

Modules are Python scripts that run at apply time.

### List Available Modules

```bash
ls /etc/treelinux/modules/
```

### Run a Module Manually

```bash
sprout run /etc/treelinux/modules/nvidia.smp
```

### Create a Custom Module

```python
#!/usr/bin/env python3
import sprout_lib

sprout_lib.info("My custom module")
# Your logic here
sprout_lib.info("Done!")
```

Save to `/etc/treelinux/modules/my-module.smp`, then add to `system.prc`:
```
modules
	my-module.smp
```

## Backups

Sprout automatically backs up `/etc/treelinux/` before changes.

### List Backups

```bash
sprout backup list
```

### Rollback to Previous State

```bash
sprout backup rollback
```

## User Management

### Add Package for a User

```bash
sprout user john add vim
sprout user john add git
```

This creates `/etc/treelinux/users/john.prc`.

### Remove Package from User

```bash
sprout user john remove vim
```

## Useful Commands

| Command | Description |
|---------|-------------|
| `sprout status` | Show system status |
| `sprout diff` | Show differences between config and system |
| `sprout apply` | Apply config to system |
| `sprout apply --non-interactive` | Apply without prompts |
| `sprout search <query>` | Search for packages |
| `sprout install <pkg>` | Install a package |
| `sprout remove <pkg>` | Remove a package |
| `sprout upgrade` | Upgrade all packages |
| `sprout run <module.smp>` | Run a module manually |
| `sprout backup list` | List backups |
| `sprout backup rollback` | Rollback to previous state |

## Next Steps

- Read the [full documentation](README.md)
- Create custom `.smp` modules
- Set up a desktop environment (i3, sway, xfce, etc.)
- Test on real hardware
