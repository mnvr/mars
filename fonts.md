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
font-cursor-misc-1.0.4-r1
gtk+3.0-3.24.49-r1
xfce4-settings-4.20.1-r0
pango-1.56.3-r0
cairo-1.18.4-r0
gtk4.0-4.18.5-r0
ffmpeg-libavfilter-6.1.2-r2
libxft-2.3.8-r3
webkit2gtk-4.1-2.48.1-r1
firefox-139.0-r0
poppler-25.04.0-r0
libass-0.17.3-r0
```
