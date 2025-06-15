# Upgrade

From APK section in the alpine user handbook (https://docs.alpinelinux.org/user-handbook/0.1a/Working/apk.html):

> Updating the system using apk is very simple. One need only run `apk upgrade`. Technically, this is two steps: `apk update`, followed by `apk upgrade` proper. The first step will download an updated package index from the repositories, while the second step will update all packages in world, as well as their dependencies.

More about world in [apk](apk.html). Continuing...

> `apk` will overwrite files you may have changed. These will usually by in the `/etc` directory. Whenever `apk` wants to install a file, but realizes a potentially edited one is already present, it will write its file to that file name with `.apk-new` appended. You may handle these by hand, but a utility called `update-conf` exists. Simply invoking it normally will present you with the difference between the two files, and offer various choices for dealing with the conflicts.
>
> > `apk update` is only ran once your cache is invalidated, which by default happens every 4 hours.

The `apk update` might not even be necessary, see `man apk-update`:

> This command is not needed in normal operations as all applets requiring indexes will automatically refresh them after caching time expires.

The wiki page of upgrades (https://wiki.alpinelinux.org/wiki/Upgrading_Alpine_Linux_to_a_new_release_branch) says there are three steps we need to do:

> 1. Update repositories file
> 2. Updatating package lists
> 3. Upgrading packages.

Step 1 is
```sh
emacs /etc/apk/repositories
```
and put the latest version.

> There is a way to skip this step for future upgrades by putting `latest-stable` instead of version numbers, but the wiki cautions that this might lead to release upgrades when one is not expecting.

Step 2 is `apk update`.

Step 3 is `apk upgrade --available`. The `--available` is to do with updating pinned packages, but I'm not sure, so will just follow the recommendation (this is the banner command listed in the release announcement).

If every thing goes well, we'll require a `reboot`. See you on the other side!

## Back!

Survived.


```bash
cat /etc/alpine-release
```

    3.22.0


The reboot was apparently not necessary, I could've just restarted the services that were updated, but reboot was easier. From wiki:

> All services that have been upgraded need to be restarted, to begin using the upgraded version. If the kernel is upgraded, it's required to it's required to reboot to begin the upgraded version.
>
> ```sh
> sync
> reboot
> ```

`update-conf --list` shows a changed files - they need to be worked through individually (most of the new ones can be zapped, but not all)
