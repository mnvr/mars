# Fonts

## fontconfig

The fontconfig package provides utilities for configuring font directories,
customizing font access, etc.

From [wiki.alpinelinux.org](https://wiki.alpinelinux.org/wiki/Fonts):

> Some applications do not specify a specific font to use but rather say
> sans-serif, serif or monospace. This is where Fontconfig comes into place by
> substituting a general font with a specific font.

We get fontconfig both by virtue of running a Xfce desktop, but also via
Firefox. Since GTK depends on it, it is very likely that a desktop environment
is already going to have it.

```sh
apk info --rdepends fontconfig
```
```
fontconfig-2.15.0-r3 is required by:
font-dejavu-2.37-r6
font-misc-misc-1.1.3-r1
...
gtk+3.0-3.24.49-r1
xfce4-settings-4.20.1-r0
pango-1.56.3-r0
cairo-1.18.4-r0
gtk4.0-4.18.5-r0
...
firefox-139.0-r0
```

Fontconfig provides (`apk info --provides fontconfig`) various fc-prefixed
programs:

```sh
apk info --quiet --provides fontconfig | sed -En '/cmd:/s/cmd:(.*)=.*/\1/p' | paste - -
```
```
fc-cache	fc-cat
fc-conflist	fc-list
fc-match	fc-pattern
fc-query	fc-scan
fc-validate	
```

`fc-match` matches available fonts.

```sh
fc-match sans
```
```
DejaVuSans.ttf: "DejaVu Sans" "Book"
```

`fc-list` lists fonts available to programs using fontconfig.

```sh
fc-list "DejaVu Sans"
```

`fc-cache` rebuilds the font information cache files. The `-v` flag asks it to
talk about what it is doing.

## Installing

Place TTF files in `~/.local/share/fonts`. If needed, `fc-cache` to refresh.

### Fira Code

```sh
mkdir /tmp/fira && cd /tmp/fira
wget https://github.com/tonsky/FiraCode/releases/download/6.2/Fira_Code_v6.2.zip
unzip Fira*.zip
mkdir -p ~/.local/share/fonts/FiraCode
mv ttf ~/.local/share/fonts/FiraCode
```
