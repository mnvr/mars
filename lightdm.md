# LightDM

Display manager.

The display manager is what runs first when we create a desktop environment. It
manages the X servers, and provides a login prompt.

After we enter the credentials in the login prompt, it starts the display
environment's session manager. In our case, that's [xfce4-session](xfce.html).

From their home page - <https://github.com/canonical/lightdm>

> LightDM is a lightweight, cross-desktop display manager. A display manager is
> a daemon that:
>
> * Runs display servers (e.g. X) where necessary.
> * Runs greeters to allow users to pick which user account and session type to
>   use.
> * Allows greeters to perform authentication using PAM.
> * Runs session processes once authentication is complete.
>
> Cross-desktop means that it supports different desktop technologies (X,
> Wayland, Mir, etc).
>
> The core LightDM project does not provide any greeter with it. Popular options
> are:
>
> * LightDM GTK+ greeter
> * ...
> * Run with no greeter (automatic login only)
> * Write your own...

```sh
cat /etc/os-release
```
```
NAME="Alpine Linux"
ID=alpine
VERSION_ID=3.21.3
PRETTY_NAME="Alpine Linux v3.21"
HOME_URL="https://alpinelinux.org/"
BUG_REPORT_URL="https://gitlab.alpinelinux.org/alpine/aports/-/issues"
```

```sh
cat /etc/alpine-release
```
```
3.21.3
```

For Alpine (currently),
* The display server is X
* The greeter is LightDM GTK+
* The session process that LightDM will run once authentication completes is
  xfce4-session.

## Configuration

Per the docs, this is the order in which config is read.

```sh
/usr/share/lightdm/lightdm.conf.d/*.conf
/etc/lightdm/lightdm.conf.d/*.conf
/etc/lightdm/lightdm.conf
```

```sh
find /usr/share/lightdm/lightdm.conf.d
```
```
find: /usr/share/lightdm/lightdm.conf.d: No such file or directory
```

```sh
find /etc/lightdm/lightdm.conf.d
```
```
find: /etc/lightdm/lightdm.conf.d: No such file or directory
```

```sh
wc -l /etc/lightdm/lightdm.conf
```
```
163 /etc/lightdm/lightdm.conf
```

```sh
ls /etc/lightdm
```
```
keys.conf                 lightdm.conf
lightdm-gtk-greeter.conf  users.conf
```

```sh
cat /etc/lightdm/*.conf | grep -v '^#'
```
```
[keyring]
[greeter]
[LightDM]

[Seat:*]

[XDMCPServer]

[VNCServer]
[UserList]
minimum-uid=500
hidden-users=nobody nobody4 noaccess
hidden-shells=/bin/false /usr/sbin/nologin /sbin/nologin
```

So looks like everything is at the defaults. From the docs

> For example, if a sysadmin wanted to override the system configured default
> session they should make a file `/etc/lightdm/lightdm.conf.d/50-myconfig.conf`
>
> ```
> [Seat:*]
> user-session=mysession
> ```
>
> Configuration is in keyfile format. For most installations you will want to
> change the keys in the `[Seat:*]` section as this applies to all seats on the
> system (normally just one).

* More about seats: [tty](tty)

How does lightdm know which greeter to use?

```sh
cat /etc/lightdm/lightdm.conf | grep greeters-dir
```
```
# greeters-directory = Directory to find greeters
#greeters-directory=$XDG_DATA_DIRS/lightdm/greeters:$XDG_DATA_DIRS/xgreeters
```

```sh
env | grep -e 'XDG'
```
```
XDG_CONFIG_DIRS=/etc/xdg
XDG_SESSION_PATH=/org/freedesktop/DisplayManager/Session0
XDG_MENU_PREFIX=xfce-
XDG_CONFIG_HOME=/home/m/.config
XDG_SEAT=seat0
XDG_SESSION_DESKTOP=xfce
XDG_SESSION_TYPE=x11
XDG_GREETER_DATA_DIR=/var/lib/lightdm-data/m
XDG_CURRENT_DESKTOP=XFCE
XDG_SEAT_PATH=/org/freedesktop/DisplayManager/Seat0
XDG_CACHE_HOME=/home/m/.cache
XDG_SESSION_CLASS=user
XDG_VTNR=7
XDG_SESSION_ID=c2
XDG_RUNTIME_DIR=/run/user/1000
XDG_DATA_DIRS=/usr/local/share:/usr/share
```

```sh
echo $XDG_DATA_DIRS | tr ':' '\n' | xargs -n 1 -I % ls %/xgreeters
```
```
ls: /usr/local/share/xgreeters: No such file or directory
lightdm-gtk-greeter.desktop
```

```sh
apk info --who-owns /usr/share/xgreeters/lightdm-gtk-greeter.desktop
```
```
/usr/share/xgreeters/lightdm-gtk-greeter.desktop is owned by lightdm-gtk-greeter-2.0.8-r3
```

So when I chose Xfce, `/usr/sbin/setup-desktop` installed both lightdm and
lightdm-gtk-greeter, the latter installing this desktop file which lightdm uses
unconfigured (I presume because it's the only one).

There is also a `~/.dmrc`, but from what I gather, lightdm is the one which
writes it for us, and going by the creation time, on first login.


```sh
cat ~/.dmrc
```
```
[Desktop]
Session=xfce
```
