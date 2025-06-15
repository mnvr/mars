# Xfce terminal

Xfce Terminal emulator

> `xterm` is the OG X terminal emulator. Here we inaccurately but
> conveniently use it when talking of the terminal emulator provided
> by the Xfce desktop environment, `xfce4-terminal`.

* More about [terminal emulators](tty.html).

## Keyboard shortcuts

Prefix `Ctrl-Shift`

* New tab - T
* Close tab - W
* New window - N
* Close window - Q
* Copy - C
* Paste - V

Prefix `Ctrl`

* Prev / next tab - Page up / down

> On MacBook keyboards, Page up / down is Fn + up / down arrow.

Prefix `Alt`

* Go to tab - 1...9

Prefix `Shift`

* Scroll line up / down - Arrow

## Preferences

* Turn on "Disable all menu access keys (such as Alt + f)", otherwise it gets in
  the way of emacs.

## Dynamic title

Change "Dynamically set-title" in preferences to "Replaces initial title".

Now we can use tty escape sequences to set the title.

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
