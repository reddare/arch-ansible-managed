# Arch ansible managed

## Yet another one Arch installation with ansible.

No magic, only basic tasks to help you install, bootstrap and setup your new Arch system.   
You need to be familiar with Arch manual installation and ansible usage to get through.   

---
### TL;DR:
- Installation: LVM, LUKS, XFS, Plymouth, separated disk drives for system & data
- Bootstrap: Pacman and AUR packages
- Profile: XFCE, Syncthing, Dotdrop, LightDM
---
### Speedrun:
0. **Installation will wipe your drive and cleanup all data**
1. Boot from `archiso` installation media and set password to `12345`
2. Setup network connection
3. Update `inventories/hosts.yml` with your device IP
4. Run ansible with:

```sh
ansible-playbook playbook.yml \
  -e 'arch_install_user_name=user' \
  -e 'arch_install_user_password=password' \
  -e 'arch_install_system_drive=/dev/nvme0n1' \
  -e 'arch_install_luks_passphrase=passphrase'
```
---
### Options:
You can use examples inventory hosts to experiment with.

`user-laptop` - used with defaults roles variables and got minimal installation type.   
`user-desktop` - used with custom roles variables, set in `inventories/host_vars/user-desktop.yml`

Playbook can be used with tags to run phases independently:
```sh
ansible-playbook playbook.yml --tags=install
```

Please inspect roles `defaults/main.yml` variables.   

Important variables which I felt necessary to explain:
| Phase | Variable | Value | Description |
| ------ | ------ | ------ | ------ |
| Install | arch_install_luks_passphrase | PASSPHRASE | unset to disable LUKS (default)
| Install | arch_install_luks_auto_unlock | false | encrypt drive but auto unlock on boot (disabled by default)
| Install | arch_install_data_drive | /dev/nvme1n1 | set to map, encrypt and mount your second data drive
| Install | arch_install_pacstrap_packages | [ *list of packages* ] | update with your custom pacstrap packages
| Install | arch_install_aur_packages | [ *list of packages* ] | update with your custom aur packages
| Install | arch_install_mkinitcpio | [ *list of options* ] | review role default variables to setup mkinitcpio options
| Bootstrap | arch_bootstrap_pacman_packages | [ *list of packages* ] | update with your custom pacman packages
| Bootstrap | arch_bootstrap_yay_packages | [ *list of packages* ] | update with your custom yay packages
| Profile | arch_profile_dotdrop | [ *list of options* ] | [Dotdrop](https://github.com/deadc0de6/dotdrop) options for dotfiles syncing
| Profile | arch_profile_syncthing_config | [ *list of options* ] | [Syncthing](https://github.com/syncthing/syncthing) options for folders sycning
| Profile | arch_profile_xfce_config | [ *list of options* ] | Lots of options to customize XFCE
---
### Lyric:

Yes, this is just another attempt to fix the process of installing and configuring your own system.   
The described methods do not cover all configuration scenarios, but they are enough for my needs.   
Maybe someone will find them useful, or at least witty.
