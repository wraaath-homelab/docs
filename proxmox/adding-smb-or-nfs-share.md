# Adding share to Proxmox
This makes it easy to mount in containers and VMs.

## Adding SMB share to Proxmox cluster
Go to `Datacenter` -> `Storage` -> `Add` -> `SMB/CIFS`

1. Set `ID:` to a friendly name. Something like truenas-smb

2. Set `Server:` to the IP of your NAS

3. Set `Username:` & `Password:` with the credentials for your SMB share

4. Set `Share:` to the specific share. In TrueNAS you can see it under "Shares". Look for "Name"

5. Set `Content:` to something you are willing to store on your NAS. I usually choose "ISO image" and store some ISOs on there, but you don't actually have to. Just don't delete the folder it creates. And yes, something needs to be selected.

6. Click `Add`

## Adding NFS share to Proxmox cluster
Go to `Datacenter` -> `Storage` -> `Add` -> `NFS`

1. Set `ID:` to a friendly name. Something like truenas-nfs

2. Set `Server:` to the IP of your NAS

3. Set `Export:` to the specific share. In TrueNAS you can see it under "Shares". Look for "Path"

4. Set `Content:` to something you are willing to store on your NAS. I usually choose "ISO image" and store some ISOs on there, but you don't actually have to. Just don't delete the folder it creates. And yes, something needs to be selected.

5. Click `Add`


> Note:
>> I have actually had a bunch of problems with this before. I don't remember the errors, but a restart fixed it for me.
>> SMB usually worked for me, tho NFS works now.

# Adding SMB/NFS share to LXC
Go to `Shell`

Use this command:
```bash
pct set 114 -mp0 /mnt/pve/truenas-nfs,mp=/mnt/truenas
```
Make sure to adjust VM/CT ID from 114