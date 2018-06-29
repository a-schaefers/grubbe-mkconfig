## The goal of this script is to auto-generate the most minimally possible, functional grub.cfg, populated with zfs boot environment menuentry stanzas only. The script purposefully disregards the video modes, font etc-- menu entries only.

### The problem encountered
Linux users have various beadm ports available, such as https://github.com/TemptorSent/beadm, but unlike FreeBSD and Solaris, beadm created boot environments do not display in the Grub menu.

### THIS IS A WORK-AROUND
The real problem that needs to be fixed is that grub itself is not 'smart' enough in that it does not use the bootfs property when loading a kernel and initramfs (rendering "beadm activate" nearly worthless)...

### Working around this grub limitation
This grubbe-mkconfig script is a primitive script and can easily be modified and also may be used as a beadm wrapper to automatically generate grub menu entries.

### Workflow example
`beadm create foobar && grubbe-mkconfig > /boot/grub/grub.cfg

### or additionally use beadm and grubbe-mkconfig with a .bashrc wrapper
`beadm() { /usr/local/sbin/beadm "$@" && /usr/local/sbin/grubbe-mkconfig > /boot/grub/grub.cfg; }

Now you only "beadm create foobar" and the grub menu will be kept up to date.

### Setup instructions:
Read the script for details.

### contribute
Please help make this better. If you know a better way, open an issue, fork or please make a pull request. Thanks.
 
