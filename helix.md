# Helix

Hello, new friend.

```sh
apk add helix
```

The tree-sitter grammars are currently packaged separately.

> https://tree-sitter.github.io/tree-sitter/
>
> Tree-sitter is a parser generator tool and an incremental parsing library. It can build a concrete syntax tree for a source file and efficiently update the syntax tree as the source file is edited. Aims:
>
> * General enough to parse any programming language
> * Fast enough to parse on every keystroke in a text editor
> * Robust enough to provide useful results even in the presence of syntax errors

```sh
apk add helix-tree-sitter-vendor
```

```sh
hx
```
```sh
hx --tutor
```

Modal: Normal mode and insert mode
* hjkl
* : enters Command mode, q/quit or discard with q!/quit!
* d delete character under cursor
* i enters Insert mode, Esc returns to Normal mode (status bar tells NOR/INS)
* :w/write <name>, :wq write-quit (status bar [+] next to filename if unsaved changes)
* i was insert before selection, a (append) is after
* I/A to insert at start/end of line
* o/O to add a newline and insert below/above the cursor
* w select word
* d doesn't delete character; deletes selection (cursor is a single character selection)
* more motions - w / move forward to before beginning of next word
 - e / move forward to end of current word
 - b / move backward to the beginning of current word
 - e + b select word under cursor
* web has WEB counterparts which act on WORDS instead of words
* WORDS are only separated by whitespace
* c change command - shorthand for di / delete selection and enter insert mode
* number before motion repeats it that many times
* v to enter Select mode (ESC returns to Normal) - in select every movement extends the selection instead of replacing it
* commands also return to normal
* x selects line, x again to select next
