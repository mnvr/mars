# coreutils

Beyond [busybox](busybox). `coreutils` is the headliner, but we need some more
packages too.

```sh
apk add coreutils less
```

## Standards

As convenient as coreutils are, it is easy to get too fancy with the GNU
extensions and write something in a shell script that wouldn't work anywhere
else. Consulting the busybox interactive help is a good way to avoid that.

```sh
busybox cut --help
```

Sometimes the busybox variant might be the one that's non-standards
compliant. Or neither might be, and in different ways.

> `pax` is POSIX compliant. Nobody cares, everyone still ships `cpio`.

## install

Install is convenient for editing files in `/etc` because it can create leading
directories, set file permissions, and of course copy the file all in a single
invocation. `/dev/stdin` is a valid input file.
