# Guide for upgrading Proxmox 7 to Proxmox 8
All credits to [TechnoTim](https://techno-tim.github.io/posts/upgrade-proxmox-to-8/)

1. Make sure you have added the `pve-no-subscription`-repo. (`Add` -> `No-Subscription` -> `Add`)
  
2. Make sure everything is up-to-date:
```bash
apt update -y && apt upgrade -y
```

3. Run all checks:
```bash
 pve7to8 --full
```
If everything checks out. Move on:

4. Update Debian and Proxmox apt-repos to Debian Bookworm:
```bash
sed -i 's/bullseye/bookworm/g' /etc/apt/sources.list
sed -i -e 's/bullseye/bookworm/g' /etc/apt/sources.list.d/pve-install-repo.list
sed -i -e 's/bullseye/bookworm/g' /etc/apt/sources.list.d/pve-no-enterprise.list
```

5. Verify Bookworm updates:
```bash
cat /etc/apt/sources.list
cat /etc/apt/sources.list.d/pve-install-repo.list
cat /etc/apt/sources.list.d/pve-no-enterprise.list
```

6. Updating and upgrading:
```bash
apt update
apt dist-upgrade
```

7. Approve changes accodingly to the [Proxmox documentation](https://pve.proxmox.com/wiki/Upgrade_from_7_to_8#Upgrade_the_system_to_Debian_Bookworm_and_Proxmox_VE_8.0)

8. Verify the upgrade:
```bash
pve7to8
```

9. Reboot your Proxmox host.
