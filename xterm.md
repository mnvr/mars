# Xfce terminal

Xfce Terminal emulator

> `xterm` is the OG X terminal emulator. Here we inaccurately but conveniently
> use it when talking of the terminal emulator provided by the Xfce desktop
> environment, `xfce4-terminal`.

* More about [terminal emulators](tty.html).

## Keyboard shortcuts

Prefix `Ctrl-Shift`

* New tab - T
* Close tab - W
* New window - N
* Close window - Q
* Copy - C
* Paste - V

New tab and new window tripped me up for a while, before I found that they are
the same as Firefox's (`Ctrl-T` new tab, `Ctrl-N` new window); and that sorted
out me for both.

Prefix `C` (Ctrl).

* Prev / next tab - Page up / down

> On MacBook keyboards, Page up / down is Fn + up / down arrow.

Prefix `Alt`

* Go to tab - 1...9

Prefix `Shift`

* Scroll line up / down - Arrow

## Preferences

Turn on "Disable all menu access keys (such as Alt + f)", otherwise it gets in
the way of emacs.

```sh
xfconf-query -c xfce4-terminal -np /shortcuts-no-mnemonics -t bool -s true
```

Turn off the nice thought but it get's irritating after once too many a times
option to show a dialog on paste.

```sh
xfconf-query -c xfce4-terminal -np /misc-show-unsafe-paste-dialog -t bool -s false
```

Use the system monospace font.

```sh
xfconf-query -c xfce4-terminal -np /font-use-system -t bool -s true
```

Increase line height.
```sh
xfconf-query -c xfce4-terminal -np /cell-height-scale -t double -s 1.1
```

Increase the default terminal width. Functionally 81 should be enough since my
custom Emacs `fill-column` is 80, plus git diffs require one character gutter.

```sh
xfconf-query -c xfce4-terminal -np /misc-default-geometry -t string -s '90x24'
```

Disable scrollbar.
```sh
xfconf-query -c xfce4-terminal -np /scrolling-bar -t string -s TERMINAL_SCROLLBAR_NONE
```
