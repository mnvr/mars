# Docs

### Install

Install the docs package to obtain man pages.

```
doas apk add docs
```

The `docs` special package installs docs for both already installed packages and future ones automatically.

### Busybox

Busybox itself has a man page (`man busybox`) which only has a terse overview of the utilities. For more detailed documentation, the opengroup specs are handy (busybox tries to, but does not guarantee, POSIX conformance).

* Top level: https://pubs.opengroup.org/onlinepubs/9799919799/
* Index of shell commands:  https://pubs.opengroup.org/onlinepubs/9699919799/utilities/
* Man page for awk: https://pubs.opengroup.org/onlinepubs/9699919799/utilities/awk.html

### POSIX man pages

But hey, there is a better way!

```
doas apk add man-pages-posix
man awk
???
profit!
```
