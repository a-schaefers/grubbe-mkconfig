### The problem encountered
Linux users have various beadm ports available, such as https://github.com/TemptorSent/beadm, but unlike FreeBSD or OpenIndiana, Linux-beadm created boot environments do not display automatically in the GRUB menu.

### THIS IS A WORK-AROUND
The real problem that needs to be fixed is that GRUB itself is not 'smart' enough in that it does not use the bootfs property when loading a kernel and initramfs (rendering "beadm activate" nearly worthless)...

### Working around this GRUB limitation
This grubbe-mkconfig script is a primitive script and can easily be modified and also may be used as a beadm wrapper to automatically generate GRUB menu entries.

### Workflow example 1
beadm create foobar && grubbe-mkconfig > /boot/grub/grub.cfg

### Workflow example 2 (use beadm and grubbe-mkconfig with a .bashrc wrapper)
beadm() { /usr/local/sbin/beadm "$@" && /usr/local/sbin/grubbe-mkconfig > /boot/grub/grub.cfg; }

Now you only "beadm create foobar" and the GRUB menu will be kept up to date.

### One last thing...
I do recommend keeping the primary boot environment at the top of the GRUB menu by naming it starting with '00' and also keeping it as the "activated" boot environment. For example,

beadm create 00main && beadm activate 00main

This will autostart 00main and is necessary because "savedefault" is not supported by GRUB for ZFS. This will also keep your zfs environment nice and consistent.

### Setup instructions:
Read the script for details.

### contribute
Please help make this better. If you know a better way, open an issue, fork or please make a pull request. Thanks.
 
