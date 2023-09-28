Dell R5500 Precision
100-110 VMs \
111-130 LXCs \
131-133 Kube \
141-150 Misc.

```bash
pct set 114 -mp0 /mnt/pve/truenas-smb,mp=/mnt/truenas
```
```bash
du -cha --max-depth=1 / | grep -E "M|G"
```

[GPU passthru/IOMMU](https://www.reddit.com/r/Proxmox/comments/lcnn5w/proxmox_pcie_passthrough_in_2_minutes/) \
[GPU in Ubuntu](https://manjaro.site/tips-to-create-ubuntu-20-04-vm-on-proxmox-with-gpu-passthrough/)

```bash
tmux new -s xmrig
tmux attach -t xmrig
```

[Proxmox Win10 best practices](https://pve.proxmox.com/wiki/Windows_10_guest_best_practices)

[Selfhosted Github runner](https://youtu.be/X3F3El_yvFg):
```bash
export RUNNER_ALLOW_RUNASROOT="1"
sudo ./svc.sh install
sudo ./svc.sh start
```

[2fa fix](https://forum.proxmox.com/threads/problem-disabling-pve-6s-2fa.56623/)

[Forward disks to TrueNAS VM (REMEMBER NO SATA CONTROLLER SUPPORT ON YOUR SERVER)](https://www.youtube.com/watch?v=M3pKprTdNqQ)
