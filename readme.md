Use at your own peril
Free software

the goal of this script is to auto-generate plain grub.cfg zfs boot environment
menuentry stanzas (disregards the video modes, font etc-- menu entries only.)

## <THIS IS A WORK-AROUND> ##
#### Read below to see what compromises and standardization is necessary for this to work.
#### The real problem that needs to be fixed is that grub itself is not
#### 'smart' enough in that it does not use the bootfs property when loading a kernel and initramfs...

# instructions

0.0)
/boot/grub should be located on its own, separate dataset outside of ROOT.
in order to play nicely with boot environments, grub needs its own, unchanging location
This is because grub stage1 is hardwired to find grub in the same dataset every time.
(Not going to work well if /boot/grub is always moving around on various boot envs!!!)

0.1)
This script assumes dracut was used for initramfs generation
by using the root=zfs:pool/ROOT/BeName convention on linux cmdline

1.)
This script assumes a static kernel naming convention.
We need unchanging kernel and initramfs names in order to make this work.
e.g. for /boot/vmlinuz
KERNEL="vmlinuz"
for /boot/initramfs.img
INITRAMFS="initramfs.img"

2.)
set kernel command line
CMDLINE="elevator=noop intel_iommu=on iommu=pt ipv6.disable=1 zfs.force=1"

3.)
run ./grubbe-mkconfig > /boot/grub/grub.cfg
Every time you beadm create, beadm rename, and beadm destroy

make it better !
submit improvements to github.com/a-schaefers/grubbe-mkconfig
or email me sch@efers.org
