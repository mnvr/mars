# zsh

Homepage: [zsh.org](https://zsh.org/)

## Installation

```sh
apk add zsh
chsh -s `which zsh`
```

I needed to logout-login for the switch to take affect.

`.profile` is not sourced, use `.zshrc`.

## FAQ

The ZSH FAQ is a fun read, here are some bits:

> Zsh is a UNIX command interpreter (shell).

> ...it is not a good idea to put this into .cshrc as that will cause csh
> scripts (yes, unfortunately some people write these)...

> ... this makes zsh rather large and feature-ridden so that it seems to appeal
> mainly to hackers. The only answer, perhaps not entirely satisfactory, is that
> you have to igore the bits you don't want.

> "Unicode", or UCS for Universal Character Set, is the modern way of specifying
> character sets. It replaces a large number of ad hoc ways of supporting
> character sets beyond ASCII. "UTF-8" is an encoding of Unicode that is
> particularly natural on Unix-like systems.

> if you'd like to run a bash script or plugin under zsh, you must port just
> like you would when translating a book from American English to British
> English.

## `.zshrc`

Zsh has a host of `~/.z*` startup scripts that it will source on
startup. Usually, putting things in `~/.zshrc` should do what we want.

In particular though, `/etc/profile`, `~/.profile` and `$ENV` will not be
sourced when zsh is invoked as zsh (they're only used when zsh is running in sh
compatibility mode, that is, when zsh is invoked as sh and like)

Conversely, `.zshrc` and friends will not be invoked when running in shell
compatibility mode. This might or might not be important, but good to lodge that
fact in the crevices of my brain.

Sayeth `zsh(1)`:

> whilst reasonable efforts are taken to address incompatibilities when they
> arise, zsh does no guarantee complete emulation of other shells, nor POSIX
> compliance.

### `export`

If you want programs run from zsh to see them, `export` the corresponding
variables in `.zshrc`.

## Introduction

Zsh has an comprehensive online documentation and man pages. Comprehensive to
the point of intimidating.

The [1995 introduction](https://zsh.sourceforge.io/Intro/intro_toc.html) is a
great starting point.

> **_Zsh_** is a shell designed for interactive use, although it is also a
> powerful scripting language. Many of the useful features of _bash_, _ksh_, and
> _tcsh_ were incorporated into zsh; many original features were added.

The examples used in the intro show the historical context in which zsh
originated; where you could finger your friends, and watch for them to login to
the same computer you were working on. The roots of IRC, and the wisps of what
might've been, and what could yet be.

## Priors

### man pages

There are many man pages (`apropos zsh`), or a single man page (`man zshall`)
split into multiple.

### Autoloading

autoloading is where a function's definition is loaded on first use. The
function is expected to be a shell script (so that it can be used both with the
autoloading mechanism and otherwise) present in fpath.

From zshall(1):

> The fpath parameter will be searched to find the function definition when the
> function is first referenced.

```sh
echo $fpath
```
```
/usr/local/share/zsh/site-functions /usr/share/zsh/site-functions ...
```

From the [intro](https://zsh.sourceforge.io/Intro/intro_4.html):

> Instead of defining a lot of functions in your `.zshrc`, all of which you may
> not use, it is often better to use the `autoload` builtin. The idea is, you
> create a directory where function definitions are stored, declare the names in
> your `.zshrc`, and tell the shell where to look for them. Whenever you
> reference the funtion, the shell will automatically load it into memory.

The `-U` prevents alias expansion, something recommended in zshall(1):

> The usual alias expansion during reading will be supressed if the autoload
> builtin or its equivalent is given the option -U. This is recommended for the
> use of functions supplied with the zsh distribution.

The `-z` flag turns off ksh compatibility autoloading (z for "zsh").

Built in functions are thus often declared as `autoload -Uz`.

## Customization

### History

From [Why is my history not being
saved<sup>FAQ</sup>](https://zsh.sourceforge.io/FAQ/zshfaq03.html#l39):

> In zsh, you need to set three variables to make sure your history is written
> out when the shell exits.
>
> ```sh
> HISTSIZE=200
> HISTFILE=~/.zsh_history
> SAVEHIST=200
> ```
>
> `$HISTSIZE` tells the shells how many lines to keep internally, `$HISTFILE`
> tells it where to write the history, and `$SAVEHIST`, the easiest one to
> forget, tells it how many to write out.

### Completion

From [the mysteries of completion<sup>FAQ</sup>](https://zsh.sourceforge.io/FAQ/zshfaq04.html):

> "Completion" is when you hit a particular command key (TAB by default) and the
> shell tries to guess the word you are typing and finish it for you.
>
> There is a related process, "expansion", where the shell sees you have typed
> something which would be turned by the shell into something els, such as a
> variable turning into a value ($PWD becomes /home/m) or a history reference
> (!! becomes everything on the last command line). In zsh, when you hit TAB it
> will look to see if tehre is an expansion to be done; if there is, it does
> that, otherwise it tries to perform completion (You can see if the word would
> be expanded -- not completed -- by TAB by typing `C-x g`, which lists
> expansions).
>
> An elegant completion system comes with the shell, based on functions called
> automatically for completion in particular contexts (e.g. there is a function
> called `_cd` to handle completion for the `cd` command); all we need to do is
> to arrage for it to be loaded by putting `autoload -U compinit; compinit`

zsh does some completion even if we don't do `autoload -U compinit; compinit`,
but that is (I presume) using the older deprecated completion system, not the
newer "elegant" (new) completion system (called compsys).

We can see the difference in trying to complete substrings. `ls /e/passTAB` does
nothing by default, but will complete to `ls /etc/passwd` after `compinit`ing.

## Prompts

See [Prompting](https://zsh.sourceforge.io/Intro/intro_14.html#SEC14) in the
intro, and `PROMPT` in zshall(1).

```sh
PROMPT='%~ $ '; RPROMPT='%(?..[%?]) %(1j.%j.)'
```

* `%~` is the tildified current directory.
* `RPROMPT` is on the right.
* `%(?..%?)` shows the last exit status if non zero.
* `%(1j.%j.)` shows the number of jobs if at least 1.

Related is wanting to display the current directory in the tab bar. The
[FAQ](https://zsh.sourceforge.io/FAQ/zshfaq03.html#l24) mentions:

> You should use the special function `chpwd`, which is called by the shell when
> the directory changes. The following checks that standard output is a
> terminal, then puts the directory in the title bar if the terminal is an xterm
> etc.
>
> ```sh
> chpwd () {
>   [[ -t 1 ]] || return
>   case $TERM in
>     *xterm*) print -Pn "\e]2;%~\a"
>       ;;
>   esac
> }
>
> The -P option tells print to treat its arguments like a prompt string.
>
> Note that when xterm starts up you will probably want to call `chpwd`
> directly.

The -n option supresses the terminating newline, as with echo. The "\e]2;"
starts the escape sequence to communicate with the terminal emulator, the "\a"
ends it.

Something like this suffices for me:

```
chpwd () {
  test -t 1 && case $TERM in; *xterm*) print -Pn "\e]2;%~\a";; esac
}
```

## Feature tour

A stroll through the [intro](https://zsh.sourceforge.io/Intro).

## Globbing

> Otherwise known as _globbing_, filename generation is quite extensive in zsh.

* `ls *.c`
* `ls *.[co]`
* `ls *.[^co]` (negation)

`<x-y>` matches a range of integers, both ar optional.

```sh
ls /dev/tty<->
```
```
/dev/tty0   /dev/tty19	/dev/tty29  /dev/tty39	/dev/tty49  /dev/tty59
/dev/tty1   /dev/tty2	/dev/tty3   /dev/tty4	...
```

`**/*` does a recursive search.

### `$RANDOM`

```sh
echo $RANDOM $[$RANDOM%100]
```
```
699 8
```

### Directory stacks

`pushd`, `popd`, and `dirs` to list.

`pushd` by itself swaps the top two.

### Command substitution

* `$(...)` is the nestable sibling of backticks.
* `<(...)` creates a named pipe (FIFO)

### alias

`alias`

### History

* csh-style ! history
* `fc`
* `r` (redo the last command, with optional `x=y` substitutions).

### ZLE

zsh's command line editor, ZLE. emulates emacs.

* ^P, ^N
* Incremental search, ^R, including within the command being typed.
* TAB completion works with parameter names `echo $HOME<TAB>`, options `setopt
  noc<TAB>` and bindings `bindkey '^X^X' pu<TAB>`.

`bindkey` lists default bindings.

* C-f forward-char, C-b backward-char
* M-f forward-word, M-b backward-word
* C-d delete-char, C-h backward
* M-d delete-word
* C-w backward-kill-word
* M-l downcase-word, M-u upcase-word
* C-i expand-or-complete (same as TAB)
* C-j accept line (same as RET or C-m)
* C-r / C-s history-incremental-search-(back / forward)
* C-u kill-whole-line
* C-v quoted-insert
* C-x = what-cursor-position
* C-x g list-expand
* C-x e expand-word
* C-y yank
* M-0 .. M-9 digit argument
* M-h run-help (open man for the current command)
* M-a accept-and-hold (execute command, but retain it in buffer)
* M-q push-line (push buffer, and pop it the next time prompt is printed)
* M-' (shell) quote-line
* M-p history-search-backwards (will search backwards through history for lines
  beginning with what we've typed so far).

### Bindings

These editor commands are functions bound by default to certain keys.

```
expand-or-complete	 TAB
push-line		 M-q
run-help		 M-h
accept-and-hold		 M-a
quote-line		 M-'
```
