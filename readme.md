## unmaintained
I moved to NixOS where system rollbacks on the bootloader are aleady fully supported by default. It's different than zfs boot environments, but I like it better than this, and this script I was working on when I was new to bash / sh and needs a lot of work, sorry about the mess if anyone uses it. :-)

### ABOUT
![Grub Menu with Boot Envs](https://github.com/a-schaefers/grubbe-mkconfig/raw/master/pic.jpg)

## grubbe-mkconfig is a simple script which can be used as a beadm wrapper to automatically generate ZFS boot environment GRUB menu entries in Linux.

Linux users have various beadm ports available, such as https://github.com/TemptorSent/beadm, but unlike FreeBSD or OpenIndiana, Linux-beadm created boot environments do not display automatically in the GRUB menu. I am aware there is some GRUB-related code in these various beadm scripts that is disabled by default, and only runs default grub-mkconfig, which isn't going to populate the bootloader with all of the boot environments available. With the help of a friend I put together this little wrapper to do what I wanted.

### THIS REALLY IS A WORK-AROUND
The real problem that needs to be fixed is that GRUB itself is not 'smart' enough in that it does not use the bootfs property when loading a kernel and initramfs-- rendering "beadm activate" nearly worthless...

### Working around GRUB limitations
Since GRUB does not naturally load the kernel and initramfs from the dataset specified by the bootfs property set using "beadm activate", this here proposed "workaround" method does not use the bootfs property to determine which boot environment to load.

### grubbe-mkconfig assumptions

This script does not create boot environments, it only updates the GRUB menu.

Root datasets should be arranged in standard pool/ROOT/bename format.

This script assumes dracut was used for initramfs generation by using the root=zfs:pool/ROOT/BeName convention. If using genkernel or other, you will need to modify the line in the script which contains root=zfs:${POOL_NAME}/ROOT/${BE_NAME} to use your required formatting for your system configuration.

GPT with legacy bios is also assumed here. If you need MBR, change the "insmod part_gpt" line.

In order to play nicely with boot environments, grub needs an unchanging location. This is because the grub stage1 is designed to find the stage2 /boot/grub files in a static location. I recommend /boot/grub is located on its own, separate dataset outside of ROOT, such as pool/GRUB, similar to what follows: zfs create -o mountpoint=/boot/grub pool/GRUB

This script assumes a static (non-versioned) kernel naming convention. Arch linux does this by default, I recommend Funtoo or Gentoo so you can change anything however you want-- but however you do it, this is a required step. We need real (NOT SYMLINKS), unchanging kernel and initramfs names in order to make this work-around work. < a pull request that mounts and iterates all boot environment kernels and initramfs names, creating kernel-specific stanzas would be gladly accepted. Until then, we need a static kernel naming convention/compromise.

### One last thing...
I do recommend keeping the primary boot environment at the top of the GRUB menu by naming it starting with '00' and also keeping it as the "activated" boot environment. For example,

beadm create 00main && beadm activate 00main

This will set 00main to auto load after 5 seconds and is necessary because "savedefault" is not supported by GRUB for ZFS. Keeping the activated boot environment set to the one that you are primarily using will help to make sense of various zfs list and beadm list features, keeping the ZFS environment nice and consistent.

### grubbe-mkconfig USAGE EXAMPLE
First configure the script variables:

```bash
#################
# set variables #
#################

# set what comes before the BeName in the menuentry
MENUENTRY_PREFIX="Funtoo GNU/Linux"

# set name of pool containing the ROOT dataset
POOL_NAME="zroot"

# set kernel name (e.g. for /boot/vmlinuz)
KERNEL="vmlinuz"

# set initramfs name (e.g. for /boot/initramfs.img)
INITRAMFS="initramfs.img"

# set kernel command line
CMDLINE="elevator=noop zfs_force=1"

# grub's set root=''
# tip: use grub-probe /boot/grub --target=bios_hints
# or look within your existing /boot/grub/grub.cfg
BIOS_HINTS="hd0,gpt1"
```

Then create some boot environments using a Linux port of the beadm script such as https://github.com/TemptorSent/beadm.

Finally create a grub.cfg populated with your zfs boot environments:

```bash
grubbe-mkconfig > /boot/grub/grub.cfg
```

beadm may also be wrapped with this script by placing in .bashrc as follows:

```bash
beadm() { /usr/local/sbin/beadm "$@" && /usr/local/sbin/grubbe-mkconfig > /boot/grub/grub.cfg; }
```

Now the grub menu will be repopulated every time beadm is executed.

### About besnap besnap.cron and befallback
##### Note: These tools do not replace zfs-auto-snapshot. zfs-auto-snapshot from the zfsonlinux project is high quality software. These are my own personal tools that I am sharing because I didn't only want reliable snapshots, but fully automated boot environments on my boot loader in addition to what zfs-auto-snapshot gives. I use zfs-auto-snapshot with cron to automate snapshots of my HOME directory. I use beadm with grubbe-mkconfig/besnap also with cron to manage boot environments and the grub menu.

**besnap and besnap.cron** can be used with cron.daily to create and rotate boot environment "snapshots." They can also be modified for other interesting ideas, perhaps running the script on every startup, then you know you would always be able to restore to the point of when you last turned on the computer!

**befallback** uses "fallback" and "fallback.old" and rotates/recreates the two automatically upon every execution. This is for certain situations before performing a "risky" operation-- first manually run befallback, then you will have a recent "fallback" boot environment plus one extra "fallback.old" to reboot into upon worst case scenarios.

### Contribute
Please help make this better. If you know a better way, open an issue, fork or please make a pull request. Thanks.
