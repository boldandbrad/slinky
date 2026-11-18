# 🌀 slinky

**A spring-loaded symlink-based dotfile manager built in shell.**

Move dotfiles to a central repo, replace originals with symlinks, and keep track
of them for easy install. That's it.

## Why use slinky?

- ✨ Simple: One command to add, install, drop, or uninstall dotfiles.
- 🔒 Safe: Won't overwrite existing files without confirmation.
- 📜 Git-friendly: Symlinked dotfiles work well with any VCS.
- 🗂️ Flexible: Organize files however you want - by host, OS, or category.
- 🔧 No config: Just works with your dotfiles repo.
- 🐚 Portable: Single POSIX `sh` script that relies on a small, documented set
  of standard Unix utilities.

## Install

### Dependencies

Slinky targets POSIX `sh` and the utilities listed below. It is intended for
Linux, macOS, and BSD.

- `sh`
- `awk`
- `basename`
- `cat`
- `dirname`
- `find` with `-maxdepth`
- `ln`
- `mkdir`
- `mv`
- `rm`
- `readlink`
- `mktemp`

These are common on modern Unix-like systems, but they are not guaranteed in
minimal or strict POSIX-only environments. `find -maxdepth` is a common
extension rather than a POSIX option, and `readlink`/`mktemp` are not part of
older POSIX utility sets.

### Package manager

```sh
brew install boldandbrad/tap/slinky
```

> Slinky is mostly a hobby project, but if it gains traction I may add it to
> other package managers in the future. Feel free to
> [open an issue](https://github.com/boldandbrad/slinky/issues).

### Manual

Add slinky as a submodule.

```sh
git submodule add https://github.com/boldandbrad/slinky
```

Or simply download the [script](./slinky) and add it to your path. You can even
store it directly in your dotfiles repository!

## How it works

Slinky moves your dotfiles into a central repository, places symlinks in their
original locations, and remembers what is has done. You can think of it as a
light wrapper around `mv` and `ln` that can do tricks.

```txt
~/.vimrc                                # Before: regular file
~/.vimrc -> ~/dotfiles/vimrc            # After: symlink
```

By default, slinky assumes your dotfiles repository is located at
`$HOME/dotfiles`, but you can change this behavior by setting the `$DOTFILES`
environment variable.

Slinky maintains a simple pipe-separated value mapping file (`slinky-v1.psv`) in
your dotfiles repository root to track the files and directories it manages.
Each line records the symlink location and the dotfile's path relative to the
mappings file, prefixed with `./`. Slinky tracks at most one link per dotfile.
Example:

```txt
~/.vimrc|./vimrc
~/.config/nvim|./config/nvim
~/.emacs|./emacs
```

Combined, these patterns let you backup your dotfiles using your preferred
version control or cloud storage system for easy install on any machine.

### Mapping file versioning

The mapping file has its own format version, independent of the slinky release.
The current format is version 1, stored in `slinky-v1.psv`. A new slinky release
does not rename it unless the mapping format changes, which can happen in any
release.

On a format change, slinky migrates the file to a new name such as
`slinky-v2.psv` and renames the old one to `slinky-v1.psv.bak`. Only the file
matching its supported format is active; `.bak` files are ignored. If slinky
finds a newer format but no active file it supports, it tells you to update
slinky. Legacy `slinky.psv` files are migrated to `slinky-v1.psv` and backed up
as `slinky-v0.psv.bak`.

## Getting started

These guides assume you have already [installed](#install) slinky, are familiar
with [how it works](#how-it-works), and will use git to backup and sync your
dotfiles.

### Clean slate

Follow this guide if you are new to managing dotfiles or are starting with a
clean slate.

1. Create an empty directory at `~/dotfiles`: `mkdir ~/dotfiles`
2. Initialize a git repository in that directory: `cd ~/dotfiles && git init`
3. Add your first dotfile: `slinky add ~/.vimrc`
4. Commit and push the changes to a remote repo, including the generated
`slinky-v1.psv` file

That's it! To install your dotfiles onto another computer, read
[Syncing multiple computers](#syncing-multiple-computers).

### Migrating to slinky

Follow this guide if you have an existing symlink based dotfile repository and
you'd like to migrate to slinky.

1. Set `$DOTFILES` to the location of your existing dotfiles repository
2. Find your existing symlinks: `slinky find`
3. Import them: `slinky import symlink1 symlink2 ...`
4. Delete your previous dotfile manager

That's it! To install your dotfiles onto another computer, read
[Syncing multiple computers](#syncing-multiple-computers).

### Syncing multiple computers

Follow this guide to easily keep your dotfiles in sync across multiple devices.

1. Make sure slinky is [installed](#install) on each computer
2. Clone your git repository on each computer, example:
`git clone https://github.com/yourname/dotfiles.git ~/dotfiles`
3. Backup any local dotfiles
4. Install dotfile symlinks: `slinky install`
5. Commit and push often as you make local changes, then repeat

That's it!

## Usage

All commands accept `-h`/`--help` and treat `--` as the end of options. Use `--`
before a path that begins with `-`.

Repository paths (`add -p`, `list -p`, `install -p`, `uninstall -p`, `move`, and
`drop`) are relative to `$DOTFILES`. Absolute paths and paths that escape the
repo are rejected.

### Add

```sh
slinky add ~/.vimrc                     # add to dotfiles root
slinky add ~/.vimrc ~/.bashrc           # add multiple at once
slinky add -p editors ~/.config/nvim    # specify repo subdirectory
```

### List

```sh
slinky list                             # list all tracked dotfiles
slinky list -p mac                      # only those under the given subdirectory
```

### Move

```sh
slinky move editors/nvim config/nvim    # move a tracked dotfile within the repo and update its symlink
```

### Drop

```sh
slinky drop config/nvim                 # move back to original location
slinky drop config/nvim .vimrc          # drop multiple at once
```

### Find

```sh
slinky find                             # search $HOME (3 levels deep) for untracked symlinks
slinky find ~/.config                   # search in the specified directory
slinky find -d 5 ~/.config              # search 5 levels deep
```

### Import

```sh
slinky import /path/to/symlink          # track an existing dotfile symlink
slinky import symlink1 symlink2         # import multiple symlinks
```

### Install

```sh
slinky install                          # create symlinks for all tracked dotfiles
slinky install -p mac                   # only install dotfiles under the given subdirectory
```

### Uninstall

```sh
slinky uninstall                        # remove all managed symlinks
slinky uninstall -p mac                 # only uninstall symlinks for dotfiles under the given subdirectory
```

### Help

```sh
slinky help                             # show the help message
```

### Version

```sh
slinky version                          # show the version
```

## Example repos

Check out [slinky in the wild](https://github.com/topics/slinky-dotfiles). Add
your dotfiles github repo to the list by adding the `slinky-dotfiles` topic.

## Inspiration

- [lnk](https://github.com/yarlson/lnk)
- [dotbot](https://github.com/anishathalye/dotbot)

## License

[MIT](./LICENSE)

