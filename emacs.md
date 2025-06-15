# Emacs

Hello old friend, we meet again.

## Install

```
apk add emacs-nox
```

nox is not a noxious variant (Emacs is enough of that, and I say that as a
friend), it means "no X".

## Batch

```sh
emacs --batch --eval '(print fill-column)'
```

```sh
emacs --batch a.md  --eval '(whitespace-cleanup)' -f save-buffer
```

The file to be visited needs to be before the commands.
