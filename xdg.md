# XDG

[freedesktop.org](https://www.freedesktop.org/wiki/) is an informal group for
better interop of desktop environments. In their own words, "we do not ourselves
produce a desktop, but we aim to help others to do so".

They "produce specifications for interoperability, but are not an official
certification body". Their most famous specs are "under the banner of 'XDG',
which stands for Cross-Desktop group" (though it is very likely the cross stands
for X as well, in more ways than one).

## Basedir

The [XDG Base Directory
Specification](https://specifications.freedesktop.org/basedir-spec/latest/)
attempts to standardize the standardization of directories in which applications
should place their files.

> Various specs specify files and file formats. This spec defines where these
> files should be looked for by defining one or more base directories relative
> to which files should be located.

Of the four most important ones, I seem to have environment variables for the
following:

* **XDG_CONFIG_HOME**, default `~/.config`
* **XDG_CACHE_HOME**, default `~/.cache`

but not for

* **XDG_DATA_HOME**, default `~/.local/share`.
* **XDG_STATE_HOME**, default `~/.local/state`

From the spec

> XDG_STATE_HOME contains state data that should persist between (application)
  restarts, but is not important or portable enough for XDG_DATA_HOME.

More of them

* **XDG_DATA_DIRS**, default `/usr/local/share:/usr/share`, preference-ordered
  set of base directories to search for configuration files in addition to
  XDG_CONFIG_HOME.

* **XDG_CONFIG_DIRS**, default `/etc/xdg`, the same thing but for
  XDG_CONFIG_HOME.

* **XDG_RUNTIME_DIR**, a temporary directory accessible only to the user, and
  which gets cleared on logout. e.g. `/run/user/1000`
