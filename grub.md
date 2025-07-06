# GRUB

The bootloader is what the machine's firmware (BIOS, [UEFI](uefi)) hands off
control to once it is done thinking.

Alpine will use GRUB as the bootloader if it detects that is being installed on
a machine that uses UEFI firmware.

> This is done by `/usr/bin/setup-disk`, which runs as part of `setup-alpine`.
>
> ```sh
> setup-disk -h | head
> ```
> ```
> usage: setup-disk [-hLqrve] [-k kernelflavor] [-m MODE] [-o apkovl] [-s SWAPSIZE]
>                   [-w FILE] [MOUNTPOINT | DISKDEV...]
>
> Install alpine on harddisk.
> ...
> ```

> GRUB stands for Grand Unified Bootloader.

## Updating GRUB settings

The (easily) tweakable parts of GRUB config are in `/etc/defaults/grub`. After
making a modification, we need to update GRUB by doing, well, `update-grub`.

Here's me removing the wait on the boot menu.

```
sed -i -E 's/GRUB_TIMEOUT=\d+/GRUB_TIMEOUT=0/' /etc/default/grub
update-grub
```
