# Upgrade

From [APK section in the alpine user
handbook](https://docs.alpinelinux.org/user-handbook/0.1a/Working/apk.html):

> Updating the system...one need only run `apk upgrade`. Technically, this is
> two steps: `apk update`, followed by `apk upgrade` proper.
>
> The first step will download an updated > package index from the repositories,
> while the second step will > update all packages in world, as well as their
> dependencies.

* More about world in [apk](apk.html).

* Also, the `apk update` might not even be necessary, see `man apk-update`:

Continuing...

> `apk` will overwrite files you may have changed. These will usually by in the
> `/etc` directory.
>
> Whenever `apk` wants to install a file, but realizes a potentially edited one
> is already present, it will write its file to that file name with `.apk-new`
> appended.
>
> You may handle these by hand, but a utility called `update-conf`
> exists. Simply invoking it normally will present you with the difference
> between the two files, and offer various choices for dealing with the
> conflicts.

The first time I did an update, there were a reasonable number of these
`.apk-new` ones. `update-conf --list` was helpful, and most of the new ones can
be zapped, but not all; seem need to be dealt with more deftly.

---

The handbook is missing one step, the first one, which the [Alpine
wiki](https://wiki.alpinelinux.org/wiki/Upgrading_Alpine_Linux_to_a_new_release_branch)
mentions:

> 1. Update repositories file
> 2. Updatating package lists
> 3. Upgrading packages.

Step 1 is
```sh
emacs /etc/apk/repositories
```
and put the latest version.

> There is a way to skip this step for future upgrades by putting
> `latest-stable` instead of version numbers, but the wiki cautions that this
> might lead to release upgrades when one is not expecting.

Step 2 is `apk update`.

Step 3 is `apk upgrade --available`. The `--available` is to do with updating
pinned packages, but I'm not sure, so will just follow the recommendation.

If every thing goes well, we need a fourth step: `reboot`.

The reboot is not necessary, I could've just restarted the services that were
updated, but reboot was easier, plus that also deals with the kernel
update. From the wiki:

> All services that have been upgraded need to be restarted, to begin using the
> upgraded version. If the kernel is upgraded, it's required to it's required to
> reboot to begin the upgraded version.
>
> ```sh
> sync
> reboot
> ```

Post reboot, 

```sh
cat /etc/alpine-release
```
```
3.22.0
```



