# Alpine Linux

Homepage: [alpinelinux.org](https://alpinelinux.org)

## Install

Minimal install is just `setup-alpine`, then `setup-desktop`. The following is
what _I_ did last time.

> Likely outdated, read what the [official docs](https://alpinelinux.org/) have
> to say.

```sh
setup-alpine
# setup-keymap us / us-dvorak
# hostname mars
# ntp-client busybox
# disk vda mode sys
# user m
reboot

vi /etc/apk/repositories # enable community, https
apk add doas git
echo "permit nopass :m" >>/etc/doas.d/doas.conf
setup-desktop # xfce
```

Now handoff to setup script in [dotfiles](https://github.com/mnvr/dotfiles).

## setup-*

Simple shells scripts located in `/usr/sbin`.

* [Alpine wiki / Setup scripts](https://wiki.alpinelinux.org/wiki/Alpine_setup_scripts)

```sh
ls /usr/sbin | grep setup-
```
```
setup-acf
setup-alpine
setup-apkcache
setup-apkrepos
setup-bootable
setup-desktop
setup-devd
setup-disk
setup-dns
setup-hostname
setup-interfaces
setup-keymap
setup-lbu
setup-mta
setup-ntp
setup-proxy
setup-sshd
setup-timezone
setup-user
setup-wayland-base
setup-xen-dom0
setup-xorg-base
```

These can be used also after installation. For example, to set the system's
hostname, `hostname mars` doesn't persist the changes, `setup-hostname` does (it
doesn't even invoke `hostname`, only sets `/etc/hostname`, and is quite trivial,
just is "proper").

## sys mode

sys mode is where the Alpine system is installed to a hard disk. During the
installation this is done by `setup-disk`.
