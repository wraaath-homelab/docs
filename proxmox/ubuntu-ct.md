# A guide for creating the perfect Ubuntu container template
All credits go to [TechnoTim](https://techno-tim.github.io/posts/cloud-init-cloud-image/)

On your Proxmox host:
1. Select a cloud image from [Ubuntu's website](https://cloud-images.ubuntu.com). At the time of writing this I suggest jammy/current.

2. Right click the `jammy-server-cloudimg-amd64.img` and press `Copy link`.

3. Use wget to download the image from the link we copied:
```bash
wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img
```

4. Create the container:
```bash
qm create 9999 --memory 2048 --core 2 --name ubuntu-jammy --net0 virtio,bridge=vmbr0
```
(Be sure to adjust any of these values according to setup. Same goes for any of the other commands)

5. Import the image as a virtual disk:
```bash
qm importdisk 9999 jammy-server-cloudimg-amd64.img local-lvm
```

6. Attach the new disk to the container as a scsi drive on the scsi controller:
```bash
qm set 9999 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-9999-disk-0
```

7. Attach the CloudInit-drive:
```bash
qm set 9999 --ide2 local-lvm:cloudinit
```

8. Make the cloud init drive bootable and restrict BIOS to boot from disk only:
```bash
qm set 9999 --boot c --bootdisk scsi0
```

9. Add the serial console:
```bash
qm set 9999 --serial0 socket --vga serial0
```

10. You're done! Now just clone the container in the WebUI. Optionally you can convert it to a template, although not needed.
