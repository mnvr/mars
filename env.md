# env

All variables in the `env` output, and where they come from.

`/bin/login` sets **HOME**, **USER**, **LOGNAME** and **SHELL** by reading it
from `/etc/passwd`.

In theory (from [a mailing list
thread](https://lists.ubuntu.com/archives/ubuntu-users/2015-July/281503.html)):

> The environment variable LOGNAME is a convention from the early days of AT&T
> UNIX. USER has been around for the same time in the BSD community.
>
> login(1) initializes the user and group IDs and the working directory, and
> sets the HOME, PATH, TERM, SHELL and USER environment variables.

And also in practice -
[`setup_environment`](https://git.busybox.net/busybox/tree/libbb/setup_environment.c?h=1_37_stable#n62)
in busybox source.

```
HOME=/home/m
USER=m
LOGNAME=m
SHELL=/bin/sh
```

`/etc/profile` sets **PATH** and **PAGER**.

```sh
export PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
export PAGER=less
```

`/etc/profile.d/20locale.sh` sets **CHARSET**, **LANG** and **LC_COLLATE**.

```sh
export CHARSET=${CHARSET:-UTF-8}
export LANG=${LANG:-C.UTF-8}
export LC_COLLATE=${LC_COLLATE:-C}
```

Resolved:
```
CHARSET=UTF-8
LANG=C.UTF-8
LC_COLLATE=C
```

Set by startxfce4 when it is started by [lightdm](lightdm):

```sh
cat /etc/xdg/xfce4/xinitrc | grep 'export XDG'                                                     2
  export XDG_MENU_PREFIX
  export XDG_CURRENT_DESKTOP
  export XDG_CONFIG_HOME
  export XDG_CACHE_HOME
```

Resolved:
```
XDG_MENU_PREFIX=xfce-
XDG_CURRENT_DESKTOP=XFCE
XDG_CONFIG_HOME=/home/m/.config
XDG_CACHE_HOME=/home/m/.cache
```

Set by
[xfce4-terminal<sup>docs.xfce.org</sup>](https://docs.xfce.org/apps/xfce4-terminal/getting-started)
when starting the shell running in the terminal:

> - **TERM**
> - **COLORTERM**
> - **DISPLAY**: The X11 display of the terminal
> - **WINDOWID**: The X11 window identifier of the terminal

```
TERM=xterm-256color
COLORTERM=truecolor
DISPLAY=:0.0
WINDOWID=8389164
```

Set by `~/.zshrc`:

```
export EDITOR=emacs
```

---

Remaining

```sh
env
```
```
DBUS_SESSION_BUS_ADDRESS=unix:path=/tmp/dbus-hoCtpCoy38,guid=9ec5c4207ec60ef6327026176856125d
DESKTOP_SESSION=xfce
GDMSESSION=xfce
PANEL_GDK_CORE_DEVICE_EVENTS=0
PWD=/home/m/.config
SESSION_MANAGER=local/mars:@/tmp/.ICE-unix/2458,unix/mars:/tmp/.ICE-unix/2458
SHLVL=5
VTE_VERSION=8002
XAUTHORITY=/home/m/.Xauthority
XDG_CONFIG_DIRS=/etc/xdg
XDG_DATA_DIRS=/usr/local/share:/usr/share
XDG_GREETER_DATA_DIR=/var/lib/lightdm-data/m
XDG_RUNTIME_DIR=/run/user/1000
XDG_SEAT=seat0
XDG_SEAT_PATH=/org/freedesktop/DisplayManager/Seat0
XDG_SESSION_CLASS=user
XDG_SESSION_DESKTOP=xfce
XDG_SESSION_ID=c2
XDG_SESSION_PATH=/org/freedesktop/DisplayManager/Session0
XDG_SESSION_TYPE=x11
XDG_VTNR=7
```