# Xfce

Desktop environment

* [xfce.org](https://xfce.org/)
* [docs.xfce.org/xfce/getting-started](https://docs.xfce.org/xfce/getting-started)

```sh
xfce4-about --version
```
```
xfce4-about 4.18.6 (Xfce 4.18)

Copyright (c) 2008-2024
The Xfce development team. All rights reserved.

Please report bugs to <https://gitlab.xfce.org/xfce/libxfce4ui/-/issues>.
```

## Components

In the UNIX-small-tools spirit, the "Xfce desktop environment" is a collection
of programs that provides a full-featured desktop environment.

> Xfce used to stand for something, an acronymn, whose expansion no longer has
> any relevance, so "Xfce" just means itself now.

Descriptions taken from `xfce4-about` and the Xfce
[about](https://www.xfce.org/about) page.

> These binaries can be run individually. e.g. `xfce4-about &` to show the about
> menu, `xfdesktop --windowlist` to show open windows etc.

* **Window manager** (`xfwm4`): Handles the placement of windows on screen,
  provides window decorations and manages workspaces.

* **Desktop manager** (`xfdesktop`): Sets the background image and provides a
  root window menu, desktop icons or minimized icons and a window list.

* **Panel** (`xfce4-panel`): Switch between open windows, launch applications,
  switch workspaces and use menu plugins to browse programs and directories.

* **Session manager** (`xfce4-session`): Controls login and power management,
  saves and restores sessions.

## Session manager `xfce4-session`

`man xfce4-session`

> The xfce4-session program starts up the Xfce Desktop Environment and is
> typically executed by your login manager (e.g. xdm, gdm, kdm, wdm or from your
> X startup scripts). It will load your last session or a default session that
> includes the standard Xfce programs if no saved session is available.
>
> xfce4-session is a standard X11R6 session manager than can manage any X11R6
> session manager compliant program, including gnome and KDE programs.
>
> xfce4-session uses the contents of the `~/.cache/sessions` directory for
> starting previously saved sessions.

```sh
ls ~/.cache/sessions
```
``` thumbs-localhost:0
xfce4-session-localhost:0
xfwm4-26b706f01-8fc9-4136-a5de-c7a189e18e56.state
```

```sh
ls ~/.cache/sessions/thumbs*
```
```
Default.png
```

`Default.png` is a small 256x160px screenshot of the desktop and running
applications (seems stale, perhaps at the point the session was saved).

`xfce4-session-localhost:0`, where presumably `:0` stands for the X display
number, is a text file containing an entry for all the running programs.

```sh
cat ~/.cache/sessions/xfce4-session-localhost:0 | awk 'NR < 9 || NR > 63 { print }; NR == 63 { print "..." }'
```

```
[Session: Default]
Client0_ClientId=26b706f01-8fc9-4136-a5de-c7a189e18e56
Client0_Hostname=unix/localhost
Client0_CloneCommand=xfwm4,
Client0_DiscardCommand=rm,-rf,/home/m/.cache/sessions/xfwm4-26b706f01-8fc9-4136-a5de-c7a189e18e56.state,
Client0_RestartCommand=xfwm4,--display,:0.0,--sm-client-id,26b706f01-8fc9-4136-a5de-c7a189e18e56,
Client0_CurrentDirectory=/home/m
Client0_Program=xfwm4
...
Client6_DesktopFile=/usr/share/applications/xfce4-terminal.desktop
Client6_Program=xfce4-terminal
Client6_UserId=m
Client6_Priority=50
Client6_RestartStyleHint=0
Count=7
LegacyCount=0
Screen0_ActiveWorkspace=0
LastAccess=1748618509
```

Similarly, but differently, the `xfwm4-<uuid>.state` file is a text file
containing details about all open windows.

```sh
cat ~/.cache/sessions/xfwm4-*.state
```
```
[CLIENT] 0x800003
  [CLIENT_ID] 2cab7566f-86f4-426d-82c3-8edfc5ee80d9
  [CLIENT_LEADER] 0x800001
  [WINDOW_ROLE] xfce4-terminal-1748614300-1539522888
  [RES_NAME] xfce4-terminal
  [RES_CLASS] Xfce4-terminal
  [WM_NAME] Terminal
  [WM_COMMAND] (1) "xfce4-terminal"
  [GEOMETRY] (127,91,817,483)
  [GEOMETRY-MAXIMIZED] (127,91,817,483)
  [SCREEN] 0
  [DESK] 0
  [FLAGS] 0x0
[CLIENT] 0x800216
  [CLIENT_ID] 2cab7566f-86f4-426d-82c3-8edfc5ee80d9
  [CLIENT_LEADER] 0x800001
  [WINDOW_ROLE] xfce4-terminal-1748614362-2333586391
  [RES_NAME] xfce4-terminal
  [RES_CLASS] Xfce4-terminal
  [WM_NAME] Terminal -
  [WM_COMMAND] (1) "xfce4-terminal"
  [GEOMETRY] (618,412,817,483)
  [GEOMETRY-MAXIMIZED] (618,412,817,483)
  [SCREEN] 0
  [DESK] 0
  [FLAGS] 0x10000
```

More details about xfce4-session are available in the Xfce online manuals:

* <https://docs.xfce.org/xfce/xfce4-session>

> Xfce4-session is a session manager for Xfce. Its task is to save the state of
> your desktop (opened applications and their location) and restore it during a
> next startup. You can create several different sessions and choose one of them
> on startup.

## Window manager

### Tips

> Shading (and unshading) a window, or rolling it up to hide its contents and
> only show the title bar, can be done by scrolling the mouse wheel when
> hovering over the title bar.

## Display Manager

Who starts Xfce?

> ### Running Xfce
>
> #### Display managers
>
> `xfce4-session` installs a file that should add an option for display managers
> to run an Xfce session. Xfce does not have its own DM.
>
> #### Commandline
>
> `startxfce4`

In the case of alpine when using `setup-desktop` with the xfce option, the
display manager is [lightdm](lightdm).

Lightdm then would start xfce4-session, which would in turn

> When you start the Xfce session for the first time, several programs are
> started by the Xfce session manager.
>
> * Panel
> * Desktop manager
> * Window manager
> * Settings manager.
>
> The Xfce session manager manages the startup of applications, and also allows
> you to save your session when you quit Xfce, so that the next time you log in,
> the same programs are started for you automatically.

We can see this in the following (ellided) output of `pstree`

```
init-+...
     |-elogind-daemon
     |-7*[getty]
     |-supervise-daem--lightdm-+-Xorg---{InputThread}
			       |-lightdm-+-xfce4-session-+-Thunar
							 |-xfce4-panel
							 |-xfce4-power-man
							 |-xfce4-screensav
							 |-xfce4-terminal
							 |-xfdesktop
							 |-xfsettingsd
							 |-xfwm4
```

## Components II

* **Settings manager** (`xfce4-settings`): A pseudo component which is made up
  of a daemon, manager and editor to centralize the configuration management.

* **Application finder** (`xfce4-appfinder`): `Alt-F2` to launch applications.

* **Thunar** (`thunar`): File manager.

## Application finder `xfce4-appfinder`

> The Xfce panel can be used to access programs by means of _launchers_, these
> program launchers are displayed as icons on the panel to launch the specified
> program. The _Applications Menu_ item on the panel also contains all installed
> programs.
>
> _Application finder_ (`Alt-F2` or chose "Run Program..." from the Application
> or Desktop Menu) can be used to launch programs by name.

By default, the `xfce4-session` will start an instance of `xfce4-appfinder` (as
a child of `xfsettingsd` and keep it running in the background for fast access,
reusing it for invocations.

## Thunar

File manager, named after the German god of Thunder (who is apparently the same
entity as the more widely known Norse god Thor).

`thunar` without arguments does the same thing as `exo-open .`, opening the
current folder in the file manager.

```sh
ls -lh `which thunar` `which Thunar`
```
```
lrwxrwxrwx    1 root     root           6 May 17 17:01 /usr/bin/Thunar -> thunar
-rwxr-xr-x    1 root     root      777.9K Jul 31  2024 /usr/bin/thunar
```

## exo-open

"`exo-open` is the CLI interface to the Xfce Preferred Applications framework".

- `exo-open ~` - File URLs open in Thunar.
- `exo-open https://mnvr.in` - HTTPS URLs open in the default web browser.
- `exo-open --launch WebBrowser mnvr.in` - We all need a little help sometimes,
  like `exo-open` does for URLs without a scheme.

## Keyboard shortcuts

```sh
xfce4-keyboard-settings &
```

* Thunar - `Super + E` (what's [Super](super-and-hyper)?)
* Appfinder - `Super + R`
* `xfce4-session-logout` - `Ctrl + Alt + Delete` (Requires Fn on MacBook
  keyboards)

## xfwm4

Window manager.

* `M-tab` - Cycle through windows.

By default, cycling shows a preview of the window. If needed, this can be turned
off by:

```sh
xfconf-query -c xfwm4 -p /general/cycle_preview -n -t bool -s false
```

* `M-space` brings up the window operations menu.
* `C-M-d` - Show desktop.
* `M-F4` - Close window.

## xfce4-desktop

Desktop. The thing that shows the background, and houses the "desktop" icons.

Changing the wallpaper

```sh
xfconf-query --channel xfce4-desktop \
  --property '/backdrop/screen0/monitorVirtual-1/workspace0/last-image' \
  --type string --create --set '/home/m/Downloads/pattern.svg'
```

Reset it using `--reset`.

The desktop program displays a static background image by default, because that
is what it is programmed to do, but we are programmers, we can reprogram it too.

## Theme

From the [Xfce wiki](https://wiki.xfce.org/howto/install_new_themes):

> There are 5 different themes you can adjust in Xfce:
>
> 1. Window decorations,
> 2. GTK interfaces,
> 3. Cursors,
> 4. Notifications,
> 5. Icons.

Window decoration themes go to `~/.local/share/themes/<theme_name>/xfwm4` and
can be selected in Window Manager settings.

GTK themes theme the toolkit (buttons, textfields, etc). These go in
`~/.local/share/themes/<theme_name>/gtk-3.0` and can be selected in Appearance
settings.

Cursor and icons go to `~/.icons/<theme_name>`.

System wide variants for all live in `/usr/share/{themes,icons}`.

### Defaults

The Window Manager decoration theme:

```sh
xfconf-query -c xfwm4 -p /general/theme
```
```
Default
```

"Default" is both a xfwm4 and GTK-3 theme
```sh
find /usr/share/themes/ -mindepth 2 -type d
```
```
/usr/share/themes/Default-hdpi/xfwm4
/usr/share/themes/Default/xfwm4
/usr/share/themes/Default/gtk-3.0
/usr/share/themes/Moheli/xfwm4
/usr/share/themes/Daloa/xfwm4
/usr/share/themes/Emacs/gtk-3.0
/usr/share/themes/Kokodi/xfwm4
/usr/share/themes/Default-xhdpi/xfwm4
```

The (current, Xfce 4.20) default GTK theme is Adwaita, and so is the icon
theme. No cursor theme seems to be explicitly set.

```sh
xfconf-query -c xsettings -p /Net/ThemeName; \
xfconf-query -c xsettings -p /Net/IconThemeName; \
xfconf-query -c xsettings -p /Gtk/CursorThemeName
```
```
Adwaita
Adwaita

```

### Dark mode

"Dark mode" is just switching to a "dark" GTK variant the GTK theme. These are
identified by the `-dark` suffix.

Firefox will automatically use the current GTK theme mode.

This switch can be scripted by using `xfconf-query` to set
`xsettings/Net/ThemeName`. [Arch
wiki](https://wiki.archlinux.org/title/Dark_mode_switching) lists more options.

### Installing

From [alpinelinux.org/wiki/Xfce](https://wiki.alpinelinux.org/wiki/Xfce):

> It is necessary to add some theme files, to get a proper (themed)
> appearance. By default, there is no theme and `Adwaita` icon in Appearance
> settings, but Adwaita is missing some icons for Xfce.
>
> Install `adw-gtk3` package for basic themes and `adwaita-xfce-icon-theme`
> package for basic icons.

These packages seem to be already transitively installed (?), they show up in
`apk info`. But they don't show up in Appearance settings, and an explicit
install is needed.

```sh
apk add adwaita-xfce-icon-theme adw-gtk3
```

Now they show up in Appearance settings, and can be selected.

```sh
xfconf-query -c xsettings -p /Net/ThemeName -t string -ns adw-gtk3
xfconf-query -c xsettings -p /Net/IconThemeName -t string -ns adwaita-xfce
```

The "Set matching Xfwm4 theme if there is one" toggle can be enabled to also
switch the Window Decorations theme that goes along with the theme (if any).

```
xfconf-query -c xsettings -p /Xfce/SyncThemes -t bool -ns true
```

Also disable the panel dark mode, so that it follows the system theme.

```sh
xfconf-query -c xfce4-panel -p /panels/dark-mode -t bool -ns false
```

### Adwaita, adwaita

The default fallback theme, "Adwaita", comes with GTK3. The ones we just
installed have similar but different name, "adw-gtk3" and "adw-gtk3-dark", and
are also visually different (See also: Xfce forum - [Theme
confusion](https://forum.xfce.org/viewtopic.php?pid=64711#p64711)

There is no option in the Appearance settings to go back to the default, but it
can be done by resetting the value using xfconf.

```
xfconf-query -c xsettings -p /Net/ThemeName -r
```

### Workaround for Adwaita Light

Unfortunately, currently (Xfce 4.20) there is bug preventing the Adwaita light
mode from being usable (Xfce forum - [Xfce doesn't apply Adwaita white
theme](https://forum.xfce.org/viewtopic.php?id=18551))

As a workaround, copy the upstream theme from `/usr/share/themes/adw-gtk`,
rename the variant in `index.theme`, and remove `gtk-3.0/gtk-dark.css`.

As a even simpler workaround, set the theme to `adw-gtk`. This name is
incorrect, it should be "adw-gtk3" (for the light theme), but it seems to work
for reasons that I don't understand.

```sh
xfconf-query -c xsettings -p /Net/ThemeName -t string -ns adw-gtk
```
