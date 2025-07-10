# Helix

Hello, new friend.

* [helix-editor.com](https://helix-editor.com/)

```sh
apk add helix
```

```sh
hx
```

## Tutor

```sh
hx --tutor
```

Also `:tutor`.

## Theme

Default theme (name `default`) is fine but dark. `base16_terminal*` themes use
the terminal's 16 color palettes (`:theme 16_` to see more adaptive themes).

[Helix wiki](https://github.com/helix-editor/helix/wiki/Themes) has a partial
list of themes with screenshots. Live previewing is instant though:

```vim
:theme <tab><tab>
```

Just the light ones:

```vim
:theme _light<tab><tab>
```

Save choice in config (`:config-open`):

```toml
theme = adwaita-light
```

## LSP and tree-sitter

The tree-sitter grammars are currently packaged separately.

> https://tree-sitter.github.io/tree-sitter/
>
> Tree-sitter is a parser generator tool and an incremental parsing library. It
> can build a concrete syntax tree for a source file and efficiently update the
> syntax tree as the source file is edited. Aims:
>
> * General enough to parse any programming language
> * Fast enough to parse on every keystroke in a text editor
> * Robust enough to provide useful results even in the presence of syntax errors

```sh
apk add helix-tree-sitter-vendor
```

Helix also has built in LSP (Language server protocol) support. Tree-sitter
and LSP intersect, but not completely. Loosely speaking, tree-sitter is more
"syntactic", and file level; LSP drives more "semantic" aspects and works on a
project level.

## Config

Config saved in `~/.config/helix/config.toml`.

```vim
:config-open
```
