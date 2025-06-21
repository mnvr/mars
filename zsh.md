# zsh

Homepage: [zsh.org](https://zsh.org/)

## Installation

```sh
apk add zsh
chsh -s `which zsh`
```

I needed to logout-login for the switch to take affect.

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
