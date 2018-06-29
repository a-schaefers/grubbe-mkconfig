### ABOUT
grubbe-mkconfig is a primitive script which can be used as a beadm wrapper to automatically generate ZFS boot environment GRUB menu entries in Linux.

Linux users have various beadm ports available, such as https://github.com/TemptorSent/beadm, but unlike FreeBSD or OpenIndiana, Linux-beadm created boot environments do not display automatically in the GRUB menu. I am aware there is some grub-related code in these various beadm scripts that is disabled by default, but on first appearance it looked messy and hard to figure out what was going on. Instead of hacking on that script, with the help of a friend I put together this little wrapper to do what I wanted.

### THIS REALLY IS A WORK-AROUND
The real problem that needs to be fixed is that GRUB itself is not 'smart' enough in that it does not use the bootfs property when loading a kernel and initramfs-- rendering "beadm activate" nearly worthless...

### Working around GRUB limitations
Since GRUB does not naturally load the kernel and initramfs from the dataset specified by the bootfs property set using "beadm activate", this here proposed "workaround" method does not use the bootfs property to determine which boot environment to load.

### Workflow example 1
beadm create foobar && grubbe-mkconfig > /boot/grub/grub.cfg

### Workflow example 2 (use beadm and grubbe-mkconfig with a .bashrc wrapper)
beadm() { /usr/local/sbin/beadm "$@" && /usr/local/sbin/grubbe-mkconfig > /boot/grub/grub.cfg; }

Now the grub menu will be repopulated every time beadm is executed.

### One last thing...
I do recommend keeping the primary boot environment at the top of the GRUB menu by naming it starting with '00' and also keeping it as the "activated" boot environment. For example,

beadm create 00main && beadm activate 00main

This will autostart 00main and is necessary because "savedefault" is not supported by GRUB for ZFS. This will also keep the ZFS environment nice and consistent.

### Setup instructions:
Read the script for details.

### contribute
Please help make this better. If you know a better way, open an issue, fork or please make a pull request. Thanks.
 
