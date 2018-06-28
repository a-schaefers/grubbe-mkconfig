### Use at your own peril
The goal of this script is to auto-generate the most minimally possible, functional grub.cfg, populated with zfs boot environment menuentry stanzas only. The script purposefully disregards the video modes, font etc-- menu entries only.

### THIS IS A WORK-AROUND
The real problem that needs to be fixed is that grub itself is not 'smart' enough in that it does not use the bootfs property when loading a kernel and initramfs...

### instructions:
Read the script for details.

### contribute
Please help make this better. If you know a better way, open an issue, fork or please make a pull request. Thanks.
 
