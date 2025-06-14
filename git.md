# git

## Setup

```sh
apk add git
git config --global user.name Name
git config --global user.email name@example.org
```

## PAT

To store the PAT, we'll use `git-credential`.

> ### git-credential

> git's [credential system
  <sup>git-scm.com/book</sup>](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage)
  allows delegating to another program (the "helper") to obtain
  credentials.


> [List
  <sup>git-scm.com/doc</sup>](https://git-scm.com/doc/credential-helpers)
  of available credential helpers.

By default, git comes with two credential helpers:
`git-credential-store` and `git-credential-cache`. We use
`git-credential-store`. This will store our credential in a
**plaintext file** at `~/.git-credentials`.

Storing it is plaintext is not ideal but before freaking out, remember
that that's the same level of security as your ssh key (now you can
freak out; both your git credentials _and_ SSH keys are plaintext).

> There is a reasonably trivial way to use a better credential helper,
  `git-credential-libsecret`. That comes with a GNOME baggage though.
>
> Which leaves me wondering - it should be possible to make a simple
  credential store that use KEKs. I assume that's what libsecret is,
  so maybe what I'm envisioning is libsecret without the gnome
  attachments.

```sh
git config --global credential.helper store
```

Create the [PAT
<sup>github.com/settings</sup>](https://github.com/settings/personal-access-tokens/new),and
perform any operation that requires authentication so that git asks
for credentials and uses libsecret to store them ([more info
<sup>docs.github.com</sup>](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#using-a-personal-access-token-on-the-command-line)).

### PAT using libsecret

To store the PAT using libsecret, we can use
`git-credential-libsecret` (a `libsecret` implementation of `git
credential`).

> ### libsecret
>
> [libsecret
  <sup>wiki.gnome.org</sup>](https://wiki.gnome.org/Projects/Libsecret)
  is a library for storing and retrieving passwords and other
  secrets. It communicates with the "Secret Service" using D-Bus.

```sh
apk add git-credential-libsecret
```

```bash
apk info git-credential-libsecret --contents
```

    git-credential-libsecret-2.47.2-r0 contains:
    usr/libexec/git-core/git-credential-libsecret
    

```sh
git config --global credential.helper /usr/libexec/git-core/git-credential-libsecret
```

### Resetting credentials

```sh
echo "url=https://github.com" | git credential reject
```

The next time you try to push, it'll ask for the new PAT.

## Setup commit signing

Create a SSH key if needed (`ssh-keygen`), ask to git to use it to
sign commits.

```sh
doas apk add openssh-keygen
git config --global user.signingkey ~/.ssh/id_ed25519.pub
git config --global commit.gpgsign true
git config --global gpg.format ssh
```

Also tell GitHub that we're using it as a [signing key
<sup>github.com/settings</sup>](https://github.com/settings/keys).

---

## Merging git repositories

I seem to need something like this once (and only once) every ~4 years.

```sh
git clone https://github.com/mnvr/mnvr.github.io mnvr.in
cd mnvr.in
git remote add old https://github.com/mnvr/mnvr.in
git fetch old
git merge --allow-unrelated-histories old/main
git remote remove old
```
