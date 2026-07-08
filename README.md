# Git Config

Personal Git configuration tracked as a small repository.

## Files

- `.gitconfig`: global Git configuration.
- `.gitignore_global`: global ignore file referenced by `.gitconfig`.
- `bin/git-clr`: helper script for the `git clr` alias.
- `bin/git-frm`: helper script for the `git frm` alias.
- `bin/git-rbm`: helper command to rebase on the remote default branch.
- `bin/git-sc`: helper script for the `git sc` alias.
- `~/.gitconfig.local`: untracked local overrides for personal identity and machine-specific paths.

## Install

Create symlinks from the home directory to this repository:

```sh
REPO_ROOT="$(git rev-parse --show-toplevel)"
ln -s "$REPO_ROOT/.gitconfig" "$HOME/.gitconfig"
ln -s "$REPO_ROOT/.gitignore_global" "$HOME/.gitignore_global"
```

If files already exist at those paths, move or back them up first:

```sh
mv "$HOME/.gitconfig" "$HOME/.gitconfig.backup"
mv "$HOME/.gitignore_global" "$HOME/.gitignore_global.backup"
```

Then create the symlinks.

The `git clr`, `git frm`, `git rbm`, and `git sc` commands call helper scripts from `PATH`. Add this repository's `bin` directory to your shell startup file so script changes are used directly from the checkout.

For Fish:

```fish
printf 'fish_add_path %s/bin\n' (git rev-parse --show-toplevel)
```

For POSIX shells:

```sh
printf 'export PATH="%s/bin:$PATH"\n' "$(git rev-parse --show-toplevel)"
```

## Local Overrides

The tracked `.gitconfig` includes `~/.gitconfig.local` at the end. Use that file for values that should not be committed or that differ per machine:

```ini
[core]
	hooksPath = /path/to/local/hooks

[user]
	name = Your Name
	email = you@example.com
	signingkey = ssh-ed25519 ...

[gpg "ssh"]
	program = /path/to/ssh/signing/program
```

Credential helpers and other machine-specific tool paths also belong in `~/.gitconfig.local`.

## Verify

```sh
git config --global --includes --show-origin --get user.name
git config --global --path --get core.excludesFile
git config --global --includes --show-origin --get core.hooksPath
```

The shared values should resolve through files in this repository. Local override values should resolve through `~/.gitconfig.local`.
