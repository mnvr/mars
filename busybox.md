# busybox

## more

* Alpine's default more and less both are symlinks to busybox.

* more is POSIX.

  * The busybox implementation is similar but doesn't match exact (nor do they
    claim it does).

  * `LINES=4 more /etc/passwd`

* This less seems to behave like more, except

  * It doesn't exit when the end of file is reached (i.e, it behaves like `more
    -e`).

  * It redraws the entire screen even if the file is shorter than a screenful.

## Moving on

Busybox is eminently usable. The good thing about busybox, beyond the
minimalism, is that it ensures that I don't get in the rabbit hole of using too
custom a option that won't work anywhere else (Docker images, darwin, SSHed
boxes).

However, taking more as an example -- having a pager that's functionally minimal
is fine but not great for a daily use workstation. e.g., `git diff` doesn't show
colors.

This is similar to the reasoning for moving on from the default [shell](ash),
and also a conclusion I reached when trying to get [man pages](docs) for the
applets that come with busybox.

[coreutils](coreutils) does the (reverse) swap of busybox for its equivalents
that are common for interactive use.