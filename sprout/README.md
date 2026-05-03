# Sprout Package Manager

Sprout is a declarative package manager for Alpine Linux. It uses `.prc` (Treelinux Config) files to define the desired state of the system.

## Core Concepts

- **Declarative**: You declare what you want, sprout makes it happen
- **Atomic**: Changes are applied atomically with automatic backups
- **Modular**: Supports `.smp` modules for custom logic at apply time

## .prc Config Format

```
# comments start with #
block_name
	item1
	item2
	key value

include other.prc
```

### Block Types

- `packages` - list of packages to install
- `services` - list of services to enable (with `enable ` prefix)
- `modules` - list of `.smp` modules to run at apply time
- `user` - user configuration (dict with `name`, `shell`, etc.)
- `dotfiles` - list of dotfiles to manage

### Example system.prc

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
	nvidia.smp

include desktop.prc
```

## Commands

### Main Commands
- `sprout apply` - apply system.prc to live system
- `sprout apply --non-interactive` - apply without prompts
- `sprout diff` - show differences between config and system
- `sprout status` - show system status

### Package Management
- `sprout install <pkg>` - install a package
- `sprout remove <pkg>` - remove a package
- `sprout search <query>` - search for packages
- `sprout upgrade` - upgrade all packages
- `sprout update` - update package index

### Module Commands
- `sprout run <module.smp>` - run a .smp module manually

### Backup Commands
- `sprout backup list` - list available backups
- `sprout backup rollback` - rollback to previous state

### User Commands
- `sprout user <username> add <pkg>` - add package to user config
- `sprout user <username> remove <pkg>` - remove package from user config

## .smp Modules

`.smp` (Sprout Module Packages) are Python scripts that run at apply time. They have access to the `sprout_lib` library for system interaction.

### Module Example

```python
#!/usr/bin/env python3
import sprout_lib

sprout_lib.info("Configuring nvidia drivers...")
# custom logic here
sprout_lib.info("Done!")
```

### sprout_lib API

The `sprout_lib` module is available at `/usr/lib/sprout_lib/__init__.py`.

- `info(msg)` - log info message
- `warn(msg)` - log warning message
- `error(msg)` - log error message
- `run(cmd)` - run a system command
- `file_write(path, content)` - write file
- `file_read(path)` - read file
- `package_install(pkg)` - install package
- `service_enable(svc)` - enable service

### Available Modules

Modules are stored in `/etc/treelinux/modules/`.

- `update-sprout.smp` - updates sprout from GitHub
- `nvidia.smp` - detects and configures NVIDIA drivers
- `amd.smp` - detects and configures AMD drivers
- `sway.smp` - configures Sway Wayland compositor
- `i3.smp` - configures i3 window manager
- `hyprland.smp` - configures Hyprland compositor
- `xfce.smp` - configures XFCE desktop
- `gnome.smp` - configures GNOME desktop
- `kde.smp` - configures KDE Plasma desktop

## Include Directive

Use `include` to split configs across files:

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

Included files are merged - all packages and services are combined.

## How It Works

1. `sprout apply` reads `system.prc` and all included `.prc` files
2. Parses the config into a structured dict
3. Compares desired state vs actual system (installed packages, enabled services)
4. Installs missing packages, prompts to remove extras
5. Runs `.smp` modules specified in the config
6. Backs up `/etc/treelinux/` before making changes

## Installation

```bash
pip install sprout
```

Or from source:

```bash
git clone https://github.com/treelinuxos/sprout.git
cd sprout
pip install -e .
```

## License

GPLv3 - see LICENSE file.
