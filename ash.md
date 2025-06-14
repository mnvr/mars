# ash

ash is the default alpine shell. It is a symlink to the busybox dash.


```bash
ls -lh `which ash` `which sh`
```

    lrwxrwxrwx    1 root     root          12 May 17 16:56 [1;36m/bin/ash[m -> [1;32m/bin/busybox[m
    lrwxrwxrwx    1 root     root          12 May 17 16:56 [1;36m/bin/sh[m -> [1;32m/bin/busybox[m


> #### Pedigree
>
>
> ash (almquist shell) came around the time of bash. In 90s it was
> posted to debian as dash, from which the shell in busybox
> ([`ash.c`](https://git.busybox.net/busybox/tree/shell)) was forked.

## Customizing

Two step process. First, create a `~/.profile` with 

```sh
export ENV=$HOME/.ashrc ash
```

Then put the actual customizations in `~/.ashrc`, e.g.

```sh
export HISTSIZE=9999999
```

## Moving on

The busybox ash does not support brace expansion. It is eminently
usable, but for the main workstation it is apropos to get a nicer
shell. Having already lived in bash and zsh for few years each, it's
time to move on to [fish](fish), give it a try.
