# Emacs

Hello old friend, we meet again.

## Install

```
apk add emacs-nox
```

nox is not a noxious variant (Emacs is enough of that, and I say that as a
friend), it means "no X".

## Help

* M-x help (C-h ?)
* M-x help-quick (C-h C-q)
* M-x apropos (C-h a)
* M-x describe-function (C-h f)

Help buffers can be closed with `q`.

Emacs describes itself as self-documenting. From the [Emacs
paper](https://www.gnu.org/software/emacs/emacs-paper.html):

> To help users remember how to ask these questions, we make it simple and
> standard. A special character, called the Help character, is used. This
> character is only used for asking for help, and is always availabel. Help is
> normally followed by another character which specifies the type of inquiry. If
> the user does not remember these characters, they can type Help again to see a
> list of them.

## Keymaps

Generally,

* C-x prefixes commands that work same across modes.
* C-c followed by alpha is for user commands.
* C-c rest is for mode specific commands.

See info page "Key Binding Conventions"

Every user interaction in emacs is a command. From the Emacs paper:

> There is no "insert" command. Each printing character is a command to insert
> that character _(ed: `self-insert-command`)_
>
> The commands for modifying text are nonprinting characters, or begin with
> nonprinting characters. Many-character commands echo if typed slowly; if there
> is a sufficiently long pause, the command so far is echoed, and then the rest
> of the command is echoed as it is typed. Aside from this, Emacs acknowledges
> commands by displaying their effects.

And about **M-x**:

> Functions residing in the dispatch table can be invoked either by the
> character command or by name. A function which does not appear in the dispatch
> table can be called only by name. The user calls functions by name by means of
> a single-character command (Meta-X) whose definition is to read the name of a
> function and call that function.

## Shell command

* M-x shell-command. C-u inserts output. `M-!`
* M-x shell-command-on-region. `M-|`

```elisp
(defun sh-execute-region (start end &optional arg)
  "Execute the region as a shell command in inferior shell; display output, if
  any. With a prefix argument, insert the command's output at point"
  (interactive "r\nP")
  (shell-command (buffer-substring-no-properties start end) arg))
```

## Batch

```sh
emacs --batch --eval '(print fill-column)'
```

```sh
emacs --batch a.md  --eval '(whitespace-cleanup)' -f save-buffer
```

The file to be visited needs to be before the commands.
