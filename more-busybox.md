# more

* Alpine's default more and less both are symlinks to busybox.

* more is POSIX.

  * The busybox implementation is similar but doesn't match exact (nor
    do they claim it does).

  * `LINES=4 more /etc/passwd`

* This less seems to behave like more, except

  * It doesn't exit when the end of file is reached (i.e, it behaves
    like `more -e`).

  * It redraws the entire screen even if the file is shorter than a
    screenful.

## Moving on

Having a pager that's functionally minimal is fine but not
great. While busybox is eminently usable, similar to the reasoning for
the [shell](ash), and also a conclusion I reached when trying to get
their [man pages](docs), for a workstation a more fullfleged UNIX
toolkit is apropos. Hence [coreutils](coreutils).
