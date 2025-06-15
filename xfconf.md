# xfconf

Xfce configuration subsystem.


```sh
ls /usr/bin | grep xf | paste - - | while read a b; do printf "%-30s %-30s\n" $a $b; done
```
```
startxfce4                     xfce4-about
xfce4-accessibility-settings   xfce4-appearance-settings
xfce4-appfinder                xfce4-display-settings
xfce4-find-cursor              xfce4-keyboard-settings
xfce4-mime-helper              xfce4-mime-settings
xfce4-mouse-settings           xfce4-panel
xfce4-popup-applicationsmenu   xfce4-popup-directorymenu
xfce4-popup-windowmenu         xfce4-power-manager
xfce4-power-manager-settings   xfce4-screensaver
xfce4-screensaver-command      xfce4-screensaver-preferences
xfce4-session                  xfce4-session-logout
xfce4-session-settings         xfce4-settings-editor
xfce4-settings-manager         xfce4-terminal
xfce4-wayland                  xfconf-query
xfdesktop                      xfdesktop-settings
xflock4                        xfrun4
xfsettingsd                    xfwm4
xfwm4-settings                 xfwm4-tweaks-settings
```

Individual Xfce components have their own settings app
(e.g. `xfdesktop-settings`, `xfce4-session-settings`), `xfwm4` has multiple, and
there are freestanding settings too that affect multiple components
(`xfce4-keyboard-settings`).

There are 4 programs that deal with settings themselves:

* `xfce4-settings-manager` - The "Settings" meta app, that shows the
  settings "dialogs" for all Xfce components present on the system.
* `xfce4-settings-editor` - A rawer program to directly edit the key
  values.
* `xfsettingsd` - A background daemon that applies settings updates.

These three form the xfce4-settings component
(<https://docs.xfce.org/xfce/xfce4-settings/start>):

> The xfce4-settings component provides a daemon, manager, and editor to
> centralize the configuration management of the Xfce system.

That leaves the last thing on the list, and the topic of our perusal here -
`xfconf-query`

## xfconf

So we end up with a H1 and H2 with the same value.

Xfconf is a "simple client-server configuration storage and query system"
(<https://www.xfce.org/projects>).

<https://docs.xfce.org/xfce/xfconf/start> has more to say:

> Xfconf is a hierarchical (tree-like) configuration system where the immediate
> child nodes of the root are called "channels". All settings beneath the
> channel nodes are called "properties".
>
> Property names are referenced by their "full path" underneath the
> channel. Everything is case-insensitive. Example:
>
> * Channel: ExampleApp, Property: /main/history-window/last-accessed
>
> Settings stored in Xfconf can be accessed via
>
> - Configuration dialogs within Settings manager.
> - Settings editor
> - Using `xfconf-query`
> - Manually. Xfconf stores all its data in XML files which can be edited when
>   Xfconf is not running.

## `xfconf-query`

```sh
xfconf-query --list
```
```
Channels:
  displays
  keyboard-layout
  keyboards
  thunar
  xfce4-appfinder
  xfce4-desktop
  xfce4-keyboard-shortcuts
  xfce4-panel
  xfce4-session
  xfce4-settings-editor
  xfce4-settings-manager
  xfwm4
  xsettings
```

```sh
xfconf-query -c keyboard-layout --list
```
```
/Default/XkbDisable
/Default/XkbLayout
/Default/XkbVariant
```

```sh
xfconf-query -c keyboard-layout --property /Default/XkbVariant
```
```
dvorak
```

## `~/.config/xfce4`

The XML files the docs speak of are in `~/.config`

```sh
cd ~/.config/xfce4/xfconf && find . -type f
```
```
./xfce-perchannel-xml/keyboard-layout.xml
./xfce-perchannel-xml/xfce4-panel.xml
./xfce-perchannel-xml/xfce4-settings-manager.xml
./xfce-perchannel-xml/xfce4-keyboard-shortcuts.xml
./xfce-perchannel-xml/xsettings.xml
./xfce-perchannel-xml/displays.xml
./xfce-perchannel-xml/keyboards.xml
./xfce-perchannel-xml/xfce4-settings-editor.xml
./xfce-perchannel-xml/thunar.xml
./xfce-perchannel-xml/xfce4-session.xml
./xfce-perchannel-xml/xfce4-appfinder.xml
./xfce-perchannel-xml/xfwm4.xml
./xfce-perchannel-xml/xfce4-desktop.xml
```
