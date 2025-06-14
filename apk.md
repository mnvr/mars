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

Public keys of the official Alpine repositories are already included on the system by the `alpine-keys` package.


### Updating repositories indexes

```sh
apk update
```

### World

List of all installed packages:

```sh
cat /etc/apk/world
```

From Alpine docs (https://docs.alpinelinux.org/user-handbook/0.1a/Working/apk.html#_world):

> The packages you want to have explicitly installed are listed in the "world file" available in `/etc/apk/world`. It is safe to edit by hand. If you've edited it by hand, you may run `apk add` with no arguments to bring the package selection to a consistent state.
