# Helix

Hello, new friend.

* [Helix-editor.com](https://helix-editor.com/)

```sh
apk add helix
```

```sh
hx
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

> Helix also has built in LSP (Language server protocol) support. Tree-sitter and LSP intersect, but
> not completely. Loosely speaking, tree-sitter is more "syntactic", and file level; LSP drives more "semantic" aspects
> and works on a project level.
>
> Their respective blurbs from the Helix home page:
>
> * "Tree-sitter produces error tolerant and robust syntax trees, which enables better syntax
>   higlighting, indent calculation and code navigation."
>
> * "Language server support enables language specific auto completion, goto definition,
>   documentation, diagnostics and other IDE features with no additional configuration".

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
* ; collapses selections to single characters; Alt-; flips the cursors.
* ; can be used to deselect without having to move the cursor.
* u to undo, U to redo
* y to yank (copy) the selection, p / P to paste the yanked text after / before the cursor
* delete and change copy altered text by default, Alt-d / Alt-c to bypass
* Space + y / p to yank / paste using system clipboard
* / to search (? backward), Enter to confirm, n / N next forward / backwards
* C duplicate cursor on next suitable line, , to remove first cursor, Alt-C duplicate above
* s to select matches in selection
* % to select whole file, so %s<regex>c is find-and-replace
* & to align "head" of the selections (keeping the other end, "anchor", fixed)
* C can be number prefixed
* Alt-s to split selection(s) on newlines (aka insert cursors on lines elsewhere)
* S to split on a regex pattern
* f<ch> to select up to and including (find) character, t<ch> "till" it, F / T backwards
* r<ch> to replace selected characters with <ch>
* . to repeat last insertion, Alt-. to repeat last f / t selection
* R to replace selection with yanked text
* J join selected lines
* > < indent unindent line
* Ctrl-a/x increment/decrement number under selection
* Registers are containers identified by characters for storing yanked text, most recent search term and macros.
* "<ch> selects register <ch>
* Q to record macro, Q to stop, q to repeat (default register @)
* Most recent search with / is stored in register /
* n / N both use register /, and it can also be set without searching
* * copies primary selection into register /
* (shorthand for "/y")
* n / N in Select mode (v) adds a new selection on each match
* "jumps" are big movements like searching or jumping to definition
* saved in jumplist; Ctrl-s to manually save current position in jumplist
* Ctrl-i/o "in"/"out" to move forward / backward in jumplist
* gw to jump to 2-char labels
* ) ( to cycle the primary amongst selections forward and backwards, Alt-, to deselect
* Alt ) ( to cycle contents of selection forward / backwards
* ~ to toggle case selection, ` to lowercase, Alt-` to up
* Ctrl-c to toggle comment
* m Match mode mm matching bracket pair mi match inside ma around (includes delimiter)
* ms surround
* md delete surrounding delimiter mr replace surrounding delimiter
* Ctrl-w Window menu
* - nv new empty buffer in a vertical split ns horizontal split
* - hjkl nav, w cycle in opening order
* - q close split, o close others
* - s / v to split the current buffer horizontally / vertically
* :vs (:vsplit) and :hs (:hsplit) commands to split a named file or buffer
* - HJKL swap split
* - t transpose split
* Space f file viewer
* - Ctrl - v / h Open in split
