### The problem encountered
Linux users have various beadm ports available, such as https://github.com/TemptorSent/beadm, but unlike FreeBSD or OpenIndiana, linux-beadm created boot environments do not display automatically in the Grub menu.

### THIS IS A WORK-AROUND
The real problem that needs to be fixed is that grub itself is not 'smart' enough in that it does not use the bootfs property when loading a kernel and initramfs (rendering "beadm activate" nearly worthless)...

### Working around this grub limitation
This grubbe-mkconfig script is a primitive script and can easily be modified and also may be used as a beadm wrapper to automatically generate grub menu entries.

### Workflow example
beadm create foobar && grubbe-mkconfig > /boot/grub/grub.cfg

### or additionally use beadm and grubbe-mkconfig with a .bashrc wrapper
beadm() { /usr/local/sbin/beadm "$@" && /usr/local/sbin/grubbe-mkconfig > /boot/grub/grub.cfg; }

Now you only "beadm create foobar" and the grub menu will be kept up to date.

### One last thing
I do recommend keeping the primary boot environment at the top of your grub menu by naming is something that begins with '00' and also to keeping it as the active boot environment. For example,

beadm create 00main && beadm activate 00main

This will allow 00main to autostart by grub since savedefault is not supported by GRUB for ZFS. This will also keep your zfs environment nice and consistent.

### Setup instructions:
Read the script for details.

### contribute
Please help make this better. If you know a better way, open an issue, fork or please make a pull request. Thanks.
 
