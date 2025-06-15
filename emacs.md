# Emacs

Hello old friend, we meet again.

## Install

```
apk add emacs-nox
```

nox is not a noxious variant (Emacs is enough of that, and I say that as a
friend), it means "no X".

## Help

* M-x help
* M-x apropos
* M-x describe-function (C-h f)

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
