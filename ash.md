# ash

ash is the default alpine shell. It is a symlink to the busybox dash.

```sh
ls -lh `which ash` `which sh`
```
```
lrwxrwxrwx    1 root     root          12 May 17 16:56 /bin/ash -> /bin/busybox
lrwxrwxrwx    1 root     root          12 May 17 16:56 /bin/sh -> /bin/busybox
```

> #### Pedigree
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

See [shells](shells#ENV) for the mechanism this is using.

## Dynamic xterm title

We can use tty escape sequences to set the title.

```sh
printf "\e]0;test\a"
printf "\033]0;test\007" # equivalent
```

The "\e]0;" escape sequence asks whoever is listening to set title to the part
that follows until the next "\a" escape sequence.

Unfortunately I can't get the printf to work on ash (it works in bash), but that
is fine, because while illustrative, we need it to be "dynamic" too.

Fortunately, it works when we put it inside the PS1 (the prompt determinant).

The default prompt for me is "\w \$ " (`echo $PS1`). So I augment it with the
above incantation, so the it runs each time the prompt is displayed.

```sh
export PS1="\e]0;$(basename $(pwd))\a\w \$ "
```

Unfortunately, it still doesn't update on switching directories. Aha! Of course,
that's because the basename is encoded into the PS1, it does not get reevaluated
each time. How?

We can use one of the special sequences that `PS1` supports! Let's use `\w`

```sh
export PS1="\e]0;\w\a\w \$ "
```

Now is also a good time to look at the documentation (_after_ having solved the
issue, as documentation is supposed to be used). Even though not using bash, I
can't think of another place to see the docs except `man bash`. These following
escape sequences look relevant:

```
\[    begin a sequence of non-printing characters, which could
      be used to embed a terminal control sequence into the prompt
\]    end a sequence of non-printing characters
```

It seems to work without it, but let's be nice.

```sh
export PS1="\[\e]0;\w\a\]\w \$ "
```

There is also \t, for the time.

After tweaking to taste, put it in the `.ashrc`.

## Moving on

The busybox ash does not support brace expansion. It is eminently usable, but
for the main workstation it is apropos to get a nicer shell. Having already
lived in bash and zsh for few years each, it's time to move on to [fish](fish),
give it a try.

Update: I did give it a try. It was too friendly. [zsh](zsh) it is then.
