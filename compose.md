# Compose key

## Unicode input

`Shift - Control - U - <hex> - space` allows entering Unicode symbols.

[unicode.org/charts](https://www.unicode.org/charts) has all of
them. [Uniview](https://r12a.github.io/uniview) seems to be a searchable index.

The motivating examples for me were Hyphen `‐` (2010), En dash `–` (2013) and Em
dash `—` (2014).

## Compose key

An alternative to remembering hex sequences is to use the compose key.

First we need to tell Xfce which key to use as the "compose" key. I used the
unused Right command ("Win") key for example, but there are other options that
are shown in Keyboard > Layout settings.

```sh
xfconf-query -c keyboard-layout -p /Default/XkbOptions/Compose -t string -ns compose:rwin
```

Then, we can type compose key followed by "--." to insert an En dash – like this
– or an Em dash by using "---" — neat!


