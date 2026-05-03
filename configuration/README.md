# Configuration Guide

Treelinux uses `.prc` (Treelinux Config) files to define the system state.

## System Config

Location: `/etc/treelinux/system.prc`

This is the main config file that defines the system state.

### Example

```
packages
	alpine-base
	alpine-baselayout
	openssh
	neovim
	firefox

services
	sshd

modules
	update-sprout.smp
```

## User Configs

Location: `/etc/treelinux/users/<username>.prc`

Each user can have their own config with personal packages.

### Example

```
packages
	vim
	git
	zsh

dotfiles
	~/.zshrc
	~/.vimrc

user
	name john
	shell /bin/zsh
```

## Services

Services are managed by runit. To enable a service:

```
services
	enable sshd
	enable networking
```

This will create symlinks in `/etc/runit/runsvdir/default/`.

## Modules

Modules are Python scripts (`.smp`) that run at apply time. They're stored in `/etc/treelinux/modules/` and use `sprout_lib` (at `/usr/lib/sprout_lib/`).

### Example Module

```python
#!/usr/bin/env python3
import sprout_lib

sprout_lib.info("Configuring nvidia drivers...")
# custom logic here
```

Available modules:
- `update-sprout.smp` - updates sprout from GitHub
- `nvidia.smp`, `amd.smp` - driver detection
- `i3.smp`, `hyprland.smp`, `sway.smp` - window managers
- `xfce.smp`, `gnome.smp`, `kde.smp` - desktop environments

## Includes

Split your config across multiple files using `include`:

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

services
	xdm
```

All packages and services from included files are merged into the main config.

## Applying Changes

After editing configs, apply changes:

```bash
sprout apply          # interactive - prompts before removing
sprout apply --non-interactive  # automatic - no prompts
```

## Backups

Sprout automatically backs up `/etc/treelinux/` before making changes.

List backups:
```bash
sprout backup list
```

Rollback to previous state:
```bash
sprout backup rollback
```

Backups are stored in `/var/treelinux/backups/`.
