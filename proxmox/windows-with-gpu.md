# Coming soon...
More coming soon



## Resizing disk
Resizing the disk of a Windows VM can be a little tricky. These are the steps that work for me.

1. Right click the "Hard Disk" of your VM. Typically `(scsi0)`
2. `Disk Action` -> `Resize` -> choose the amount of GiB you want to add.
3. Enter your Windows VM. Either from RDP or the Proxmox Console.
4. Open CMD as admin.
5. Shutdown your VM with this:
```ps
shutdown -s -t 0
```
([From Proxmox docs](https://pve.proxmox.com/wiki/Resize_disks#General_considerations))

6. Start the VM as normal from the WebUI
7. You should see the new free space.
8. Choose "Extend Volume" on your disk.
9. If "Extend Volume" is greyed out, you probably need to delete the recovery partition between them. Open a CMD as admin and do:
```ps
diskpart
list disk
select disk 0
list partition
select partiton 4
delete partiton override
```
10. Now the "Extend Volume" should be selectable.
