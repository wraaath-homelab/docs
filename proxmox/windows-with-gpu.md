# Windows VM
This guide will help you create a best-practice Windows VM in Proxmox. \
I will also be guiding you to adding a GPU to this VM.
Stolen from [u/chimera-zen's post](https://www.reddit.com/r/Proxmox/comments/lcnn5w/proxmox_pcie_passthrough_in_2_minutes/) & [Proxmox's Wiki](https://pve.proxmox.com/wiki/Windows_10_guest_best_practices#Install)

# Creating the VM (with the GPU)
> [!IMPORTANT]
> Before starting you should confirm IOMMU is working if you want GPU passthru. See [my guide](https://github.com/wraaath-homelab/docs/blob/main/proxmox/iommu.md) on that.

1. Press `Create VM`
2. Under `OS`
   * `ISO-image:` your Windows 10 or 11 ISO
   * `Type:` Microsoft Windows
   * `Version:` Whatever Windows version your ISO is
3. Under `System`
   * `Machine:` q35
   * `BIOS:` OVMF (UEFI)
   > Add an EFI Disk with default settings
   * `SCSI Controller:` VirtIO SCSI
   > If you chose Windows 11 add TPM with default settings

4. Under `Disks`
   * `Bus/Device:` SCSI
   * `Cache:` Write back
   * `Discard:` Checked

5. Under `CPU`
   * `Type:` kvm64
   > **Much** performance improvement compared to "host".

   * `Enable NUMA:` Checked

6. Under `Network`
   * `Model:` VirtIO (paravirtualized)

7. Go to `Confirm` and press `Finish`
> DO NOT START AFTER CREATED

## Adding GPU (optional)
8. Go to your VM -> `Hardware`

9. `Add` -> `PCI Device` -> `Raw Device`
   * `Device:` Your GPU should be listed. Make sure not to select the audio controller.
   * `All Functions:` Checked
   * `PCI-Express:` Checked
   > DO NOT SET AS PRIMARY GPU

---

10. Download the VirtIO Windows drivers
   * Go to `local (node)`-storage or where you store ISOs
   * `ISO Images` -> `Download from URL`
   * Go to [VirtIO's website](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/) -> `stable-virtio`
   * Right click `virtio-win.iso` and select `Copy link`
   * Go back to Proxmox
   * `URL:` paste URL you just copied
   * `Query URL` -> `Download`

11. Add VirtIO drivers to VM
   * Go to your VM
   * `Add` -> `CD/DVD Drive`
   * `Bus/Device:` IDE 0
   * `Storage:` local (or wherever you store ISOs)
   * `ISO image:` virtio-win.iso
   * `Add`

12. Launch VM and install \
Follow as normal until you reach storage

13. Choose `Custom (advanced)` -> `Load driver`
   * `Hard disk:` select `vioscsi\w10\amd64\Red Hat VirtIO SCSI pass-through controller`
   * `Network:` select `NetKVM\w10\amd64\Redhat VirtIO Ethernet Adapter`
   * `Memory ballooning:` select `Balloon\w10\amd64\VirtIO Balloon Driver`
   * Choose the drive and continue

14. The rest should be a straight-forward Windows install

15. Remember to reboot

## GPU drivers (if you chose to use GPU)
16. Install drivers using [NVCleanstall](https://www.techpowerup.com/nvcleanstall/)

17. Enable RDP
   * `Settings` -> `System` -> `Remote Desktop`
   * `Enable Remote Desktop` On
   * `About` -> Note the `Device name`
   * On another machine, test if you can connect to this hostname from a RDP client.

18. Shut down VM

19. Go to VM -> `Hardware`
   * `Display` VirtIO-GPU
   * `PCI Device` -> `Edit`
   * `Primary GPU:` Checked

20. Boot VM again and connect via RDP

## Installing Guest Agent and Services (optional, but nice)
21. Open File Explorer -> `This PC` -> `CD Drive` -> `guest-agent`
   * Run `qemu-ga-x86_64.msi`

22. Open File Explorer -> `This PC` -> `CD Drive`
   * Run `virtio-win-gt-x86.msi`


### Resizing disk
> [!NOTE]
> Made from the [Proxmox docs](https://pve.proxmox.com/wiki/Resize_disks#General_considerations)

Resizing the disk of a Windows VM can be a little tricky. These are the steps that work for me.

1. Resize size of VM disk in Proxmox
   * `Hardware` -> `Hard Disk` -> `Disk Action` -> `Resize`
   * Choose amount of GiB to add.

2. Shut down your Windows VM safely
   * Open `Command Prompt` as administrator
   * Run:
```ps
shutdown -s -t 0
```

3. Start the VM as normal from the WebUI

4. Open `Disk Management`
   * Right-click your `(C:)`-partition
   * Select `Extend Volume...`

5. If "Extend Volume" is greyed out, you probably need to delete the recovery partition between them. Open a CMD as admin and do:
> [!IMPORTANT]
> Make sure to pick the right disk and partition, the ones listed are examples.
```ps
diskpart
list disk
select disk 0
list partition
select partiton 4
delete partiton override
```

6. `Extend Volume...` should be selectable.
