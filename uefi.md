# UEFI

When a computer boots up, what does it do?

It thinks for a long time about the meaning of it all. This takes a very long
time, but since it is very fast, we never notice it.

After it is done thinking and yet again doesn't arrive at a satisfiable
proposition (remember, from its perspective, it is happening the first time), it
busies itself, and runs "UEFI" code present on the firmware.

UEFI is a standard, but the implementations are many and proprietary for the
manufacturer of the motherboard. There is a reference implementation (which is
also open source) called TianoCore EDK 2 (which can be used by qemu).

UEFI looks (or, to be more precise, the UEFI standard asks the implementation to
go and look) for a ESP - EFI system partition. This is a small FAT partition
that contains the bootloaders for one or more OSes. Depending on the user's
choice, it loads one of them.

The user's choice and other settings can be persisted into NVRAM (non-volatile
storage). How these "UEFI vars" are saved is up to the implementation (e.g. qemu
can save them to a JSON file). The OS can also access these, e.g. using the
`efivars` utility (which delegates to `/sys/firmware/efi/efivars/`).

### Shell

If you find yourself in the UEFI shell, things are not going well. Calm down,
you haven't been eaten by a grue (yet).

`help` will give you help, but it will scroll out and there isn't a scroll
bar.

Calm down, `help -b` will paginate.

`bcfg boot dump` will print something, which might or might not help.

You head out of the room by typing `exit`. You find yourself in a new, more
welcoming cavern, lined with menu items and other niceities. Maybe the princess
is in this castle.

## QEMU

Using UEFI with QEMU requires two things:

1. A UEFI firmware implementation
2. A NVRAM emulation to store the UEFI variables

QEMU comes with both (at least the homebrew version)

1. `$(brew --prefix qemu)/share/qemu/edk2-aarch64-code.fd`
2. `$(brew --prefix qemu)/share/qemu/edk2-arm-vars.fd`

Both of these can be offered to the QEMU machine as a pflash drives by tacking
on something like the following to the QEMU invocation:

```sh
  -drive file=edk2-aarch64-code.fd,if=pflash,format=raw,readonly=on \
  -drive file=edk2-arm-vars.fd,if=pflash,format=raw \
```

There is also an option to store the UEFI variables in a JSON file instead of
the provided pflash file by using `-device
uefi-vars-sysbus,jsonfile=uefi.json`. It works, but I was unable to persist the
UEFI boot delay when using this method (they work fine with the pflash
vars). The JSON variable support is very new, so likely it is still work in
progress, and future reader, don't take my word for it, maybe it works now.
