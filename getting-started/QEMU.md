# QEMU Testing for treelinux

## Files
- `alpine-test.qcow2` - Alpine 3.23.4 cloud image (8GB)
- `cloud-init-seed.iso` - Cloud-init config with SSH access
- `treelinux_key` / `treelinux_key.pub` - SSH keys for access
- `qemu-run.sh` - Interactive QEMU launch script
- `qemu-test.sh` - Automated test script (boots, runs command, kills)

## Quick Start

### Interactive (with GUI)
```bash
./qemu-run.sh
```

### SSH Access
Once booted, SSH in:
```bash
ssh -i treelinux_key -p 2222 root@localhost
```
Password: `treelinux`

### Automated Test
```bash
./qemu-test.sh "cat /etc/os-release"
```

## Port Forwarding
- Host SSH: localhost:2222 -> VM port 22

## Notes
- The image uses Tiny Cloud (Alpine's cloud-init alternative)
- Cloud-init may take a minute to configure on first boot
- If SSH doesn't work, the cloud image might not have Tiny Cloud configured
