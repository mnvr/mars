# apk

Alpine packages keeper

## Repositories

3 layers:

1. `mirror` hosts
2. `release` (collections of snapshots) of
3. `repositories`

3 repositories

1. main
2. community
3. testing

Specified in

```sh
/etc/apk/repositories
```

Format of entries is `[@type] [url]`, e.g.
```
# comment
https://dl-cdn.alpinelinux.org/alpine/edge/main
/var/apk/my-packages
```

Public keys of the official Alpine repositories are already included on the
system by the `alpine-keys` package.

## `apk update`

Updating repositories indexes.

```sh
apk update
```

We don't need to do this usually, other apk commands that depend on the
repository index will automatically do this once the 4 hour cache expires.

## `/etc/apk/world`

List of all installed packages:

```sh
cat /etc/apk/world
```

From
[docs.alpinelinux.org/user-handbook](https://docs.alpinelinux.org/user-handbook/0.1a/Working/apk.html#_world):

> The packages you want to have explicitly installed are listed in the "world
> file" available in `/etc/apk/world`. It is safe to edit by hand. If you've
> edited it by hand, you may run `apk add` with no arguments to bring the
> package selection to a consistent state.

## apk search

- Web interface: [pkgs.alpinelinux.org](https://pkgs.alpinelinux.org)

## Searching packages by binary

`cmd:` prefix. There is also `so:`.

```sh
apk search cmd:dig
```

More info in
[docs.alpinelinux.org/user-handbook](https://docs.alpinelinux.org/user-handbook/0.1a/Working/apk.html#_searching_for_packages).

## Package contents

The `--contents` flag (aka `-L`) to `apk info` can be used to list the files
included in the package.

```sh
apk info --contents doas
```
```
doas-6.8.2-r8 contains:
etc/doas.conf
usr/bin/doas
```

This only works if the package is already installed. To check whether a package
is installed, use `--installed` (nick `-e`), which exits non-zero if it is not
installed, otherwise prints the name of the package.

```sh
apk info --installed doas
```
```
doas
```

The `--provides` flag is similar to `--installed`, but (a) it lists only the
binaries and libraries provided by the package, and (b) works even if the
package is not installed.

```sh
apk info --provides doas
```
```
doas-6.8.2-r8 provides:
cmd:doas=6.8.2-r8
```

## Rdepends

To see which packages depend on a given package, use the `--rdepends` (-r)

```sh
apk info --rdepends font-dejavu
```
```
font-dejavu-2.37-r6 is required by:
xfce4-4.20-r0
```

`apk dot` prints a dependency graph, parseable by Graphviz ideally, but also
humans in a pinch.



