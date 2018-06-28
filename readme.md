### Use at your own peril
the goal of this script is to auto-generate plain grub.cfg zfs boot environment
menuentry stanzas (disregards the video modes, font etc-- menu entries only.)

### THIS IS A WORK-AROUND
Read below to see what compromises and standardization is necessary for this to work.
The real problem that needs to be fixed is that grub itself is not
'smart' enough in that it does not use the bootfs property when loading a kernel and initramfs...

### instructions:
read script for details

### contribute
Please help me make this better. if you know a better way, open an issue, fork or please make a pull request.
