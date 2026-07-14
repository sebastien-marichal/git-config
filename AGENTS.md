# Repository Guidelines

## Project Structure & Module Organization

This repository contains a portable personal Git configuration rather than a compiled application.

- `.gitconfig` defines shared Git defaults and aliases.
- `.gitignore_global` contains global ignore patterns.
- `bin/git-*` contains POSIX shell helpers exposed as Git subcommands through `PATH`.
- `README.md` documents installation, shell integration, and local overrides.

Keep machine-specific identity, signing keys, credential helpers, and absolute tool paths in `~/.gitconfig.local`; `.gitconfig` includes it automatically.

## Build, Test, and Development Commands

There is no build step or dependency manager. Run these checks from the repository root:

```sh
sh -n bin/git-*                 # validate shell syntax
git config --file .gitconfig --list  # parse and inspect the configuration
git diff --check                # detect whitespace errors
```

To exercise a helper directly, put the checkout first on `PATH`, for example `PATH="$PWD/bin:$PATH" git l`. Test destructive helpers such as `git frm` only in a disposable repository because they fetch and hard-reset the working tree.

## Coding Style & Naming Conventions

Write helpers as portable POSIX `sh` scripts with a `#!/bin/sh` shebang. Use tabs for command indentation, lowercase snake_case variable names, quoted expansions, and explicit failure handling (`|| exit`) where later operations depend on success. Name Git subcommands `bin/git-<short-command>` and keep their aliases synchronized in `.gitconfig` and documentation in `README.md`.

## Testing Guidelines

No automated test framework or coverage threshold is configured. For script changes, run `sh -n` and verify success, invalid input, and failure paths in temporary Git repositories. Prefer assertions against complete command output if tests are introduced. Confirm configuration changes with `git config --file .gitconfig --get <key>`.

## Commit & Pull Request Guidelines

Recent commits use short, imperative subjects such as `Add git rbm command` and `Improve git sc remote branch handling`. Keep each commit focused and explain behavior changes in the body when the subject is insufficient. Pull requests should summarize the user-visible Git behavior, list verification commands, and call out destructive operations or platform assumptions. Link relevant issues; screenshots are only useful for terminal output or editor integration changes.
