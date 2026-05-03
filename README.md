# Treelinux Documentation

Welcome to Treelinux - a declarative Alpine Linux with runit and sprout package manager.

## Quick Links

- [Getting Started](getting-started/) - QEMU setup, first boot, SSH
- [Sprout Package Manager](sprout/) - .prc format, commands, .smp modules
- [Configuration](configuration/) - system.prc, services, users
- [Contributing](contributing/) - development setup, internals

## What is Treelinux?

Treelinux is Alpine Linux + runit init + sprout declarative package manager.

- **Declarative**: Define your system in `.prc` configs
- **Atomic**: Changes applied atomically with automatic backups
- **Modular**: `.smp` modules for custom boot-time logic

## Quick Start

```bash
# Clone and boot in QEMU
git clone https://github.com/treelinuxos/treelinux.git
cd treelinux
nohup qemu-system-x86_64 -m 2G -drive file=treelinux-base.qcow2,format=qcow2 \
  -netdev user,id=n1,hostfwd=tcp::2222-:22 -device e1000,netdev=n1 \
  -display gtk & disown

# SSH in
sshpass -p "treelinux" ssh -o StrictHostKeyChecking=no -p 2222 root@localhost

# Apply config
sprout apply --non-interactive
```

## License

MIT
