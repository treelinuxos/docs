# Automated Alpine Installation for treelinux

## Prerequisites
- QEMU installed
- Python 3 with pexpect (available)
- Alpine virt ISO downloaded

## Method 1: Manual (5 minutes)
1. `qemu-system-x86_64 -m 1024 -cdrom alpine.iso -drive file=treelinux-base.qcow2 -boot d`
2. Login as root (no password)
3. Run: `setup-alpine` (answer defaults, set hostname to treelinux)
4. Run: `apk add runit openssh doas zsh`
5. Run: `ssh-keygen -A && echo 'root:treelinux' | chpasswd`
6. Run: `poweroff`
7. Boot from disk: `qemu-system-x86_64 -m 1024 -drive file=treelinux-base.qcow2 -nic user,hostfwd=tcp::2222-:22`

## Method 2: Automated (Python + pexpect)
```python
# See build-image.py for automation
```

## Quick Test
Once built, SSH access:
```bash
ssh -p 2222 root@localhost  # password: treelinux
```
