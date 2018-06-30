### ABOUT
![Grub Menu with Boot Envs](https://github.com/a-schaefers/grubbe-mkconfig/raw/master/pic.jpg)

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

This will set 00main to auto load after 5 seconds and is necessary because "savedefault" is not supported by GRUB for ZFS. Keeping the activated boot environment set to the one that you are primarily using will help to make sense of various zfs list and beadm list features, keeping the ZFS environment nice and consistent.

### About besnap besnap.cron and befallback

besnap and besnap.cron can be used to create and rotate daily boot environment "snapshots."

befallback destroys a boot env called "fallback" and then creates it again. The use case for this is for a situation where you are about to run a "risky" command-- first manually run "befallback", and then you know you have a working current boot environment to reboot into during worst case scenarios.

### Setup instructions:
Read the script for details.

### contribute
Please help make this better. If you know a better way, open an issue, fork or please make a pull request. Thanks.
 
