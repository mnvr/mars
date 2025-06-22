# Shells

Each user has an associated shell

```sh
cat /etc/passwd | grep '^m:'
```
```
m:x:1000:1000:M:/home/m:/bin/sh
```

By default, on an Alpine system `/bin/sh` points to the busybox [ash](ash).

```sh
ls -l /bin/sh
```
```
lrwxrwxrwx    1 root     root       12 Jun  8 19:48 /bin/sh -> /bin/busybox
```

> A shell which can be linked to from `/bin/sh` is expected to be a POSIX
> compliant shell.

The shell can be changed using `chsh`.

> `chsh` requires the shell to be listed in `/etc/shells`. We can directly
> change `/etc/passwd` too for the same outcome.

From `man chsh`

```
NAME
       chsh - change login shell

DESCRIPTION
       The chsh command changes the user login shell. This determines the name
       of the usr's initial login command.
```

Which tells us that this can be any program, not necessarily a "shell".

This program is run (by `/bin/login`) when the user logs in the console or a
tty. Since it is usually a shell, hence the name "login shell".

The aforementioned program can be run in other contexts too. In such contexts it
is then referred to as a "non-login shell".

## ENV

If it is indeed a "shell", then usually it behaves in a manner expected of
shells.

From the [POSIX
standard<sup>opengroup.org</sup>](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/sh.html):

> The following environment variables shall affect the execution of _sh_:
>
> _ENV_. This variable, when and only when an interactive shell is invoked,
> shall be subjected to parameter expansion by the shell, and the resulting
> value shall be used as a pathname of a file containing shell commands to
> execute in the current environment.

So we can put our customizations in some file, say `~/.myrc`, and if we set
`ENV` to point to `~/.myrc` when our shell is invoked, it will get sourced.

```sh
echo "echo hello" >~/.myrc
ENV=$HOME/.myrc sh
```
```
hello
```

Note the use of "$HOME" instead of "~". Tilde expansion might not be available
at this point in the initialization sequence.

## /etc/profile, ~/.profile

But where do we set `ENV`?

If it is indeed a "shell", then usually it also behaves in a manner customary to
shells; a manner set by the behaviour of the first shell, the Bourne shell.

Some of this behaviour is POSIXified, some not (or maybe I can't find the
corresponding reference). Either way, one thing that shells following in the
tradition of the read `/etc/profile` and `~/.profile` when it is invoked as a
"login shell".

How does a shell know it is the login shell? Apparently, the
`/bin/login` (the command that authenticates the user when they login
on the console or a tty) passes `arg[0]` by prefixing it with "-" when
invoking the shell (e.g. `exec_login_shell` in busybox source).

We can however also nicely ask the shell to behave as if was being invoked as
the login shell by passing the "-l" flag (short for "--login", but not all
shells recognize the longer form). Doing so, we can see it reading `~/.profile`.

```sh
echo "echo HELLO" >> ~/.profile
sh -c exit # prints nothing
sh -l -c exit
```
```
HELLO
```

Similarly, when invoked as a login shell a Bourne-compatible shell will also
read `/etc/profile`. From the [arch
wiki](https://wiki.archlinux.org/title/Command-line_shell):

> A _login shell_ is an invocation mode, in which the shell reads files intended
> for one-time initialization, such as system-wide `/etc/profile` or the user's
> `~/.profile` or other shell-specific file(s).

So we now have a mechanism for getting our shell to configure the environment
and do other things when we invoke it.

1. Add `export ENV=/path/to/our/custom/shell/commands` in `~/.profile`.
2. Add our custom shell commands in `/path/to/our/custom/shell/commands`.

This is indeed the mechanism we _need_ to use for the default shell that comes
with Alpine, the busybox [ash](ash#customizing).

```sh
export ENV=$HOME/.ashrc
```

> What happens to the login shell itself? The `ENV` will come into effect for
> any subsequent shells launched when the `ENV` is set, but for the login shell
> itself, `/path/to/our/custom/shell/commands` will never be sourced since `ENV`
> wasn't set when it was started. Or will it?
>
> Turns out (not coincidentally, likely) that this scenario is handled, at least
> by ash. From the source
> ([ash.c](https://git.busybox.net/busybox/tree/shell/ash.c#n14707)):
>
> ```c
> /*
>  * Read /etc/profile, ~/.profile, $ENV.
>  */
> static void read_profile(...
> ```
>
> Since they are read in sequence, and since we set ENV in `~/.profile`, `$ENV`
> will be set and sourced at the end of the sequence, and everything will work
> out in the end.

However, since this is a basic requirement, its bigger siblings like bash and
zsh provide a way to skip this two step dance by reading special "rc" files
automatically, whether they are invoked as login shells or not. For example, zsh
will read `~/.zshrc`.

> zsh reads /etc/profile, ~/.profile and $ENV only if it is running in sh
> compatibility mode.

## X

When logging in using a display manager, we never run a shell (until we do), so
who sources the profiles then, or are they?

From [zsh FAQ](https://zsh.sourceforge.io/FAQ/zshfaq03.html):

> Login shells are often interactive, but this is not necessarily the case. It
> is the programme that starts the shell that decides if it is to be a login
> shell, and it is not required that the shell be run interactively. A possible
> example is a display manager that starts a shell to initialize your
> environment before running the window manager to create terminals: it might
> run this as a login shell but with no terminal, so it is not interactive.

Our display manager, and the mechanism seems to be via
`/usr/bin/lightdm-session`:

```sh
readlink /usr/bin/lightdm-session
```
```
/etc/X11/xinit/Xsession
```

In `/etc/X11/xinit/Xsession` we find
```sh
# Load profile
for file in /etc/profile "$HOME/.profile" /etc/xprofile "$HOME/.xprofile"; do
    if [ -f "$file" ]; then
        echo "Loading profile from $file";
	. "$file"
    fi
done
```
