# Guide for adding IOMMU support in Proxmox
When adding a PCI-device to a VM it will say if IOMMU is disabled. This is how I enabled it. \
Credits go to [u/chimera-zen on Reddit](https://www.reddit.com/r/Proxmox/comments/lcnn5w/proxmox_pcie_passthrough_in_2_minutes/) (I really just copied what he wrote to spread the word)

1. Enter the BIOS on your machine.

2. Enable VT-d/AMD-d (sometimes named Virtulization Technology)

3. Edit `/etc/default/grub`:

Intel:
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt"
```
AMD:
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on iommu=pt"
```

4. Run `update-grub2`

5. Edit `/etc/modules` and add:
```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

6. Run `update-initramfs -u`

7. Reboot Proxmox host

8. Verify IOMMU is enabled:
```bash
dmesg | grep -e DMAR -e IOMMU
```
>You should see `DMAR: IOMMU enabled`