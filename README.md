# 🌀 slinky

**A simple symlink-based dotfile manager.**

Move dotfiles to a central repo, track them with a map file, and replace
originals with symlinks. That's it.

## Install

```sh
brew tap boldandbrad/tap
brew install slinky
```

Or download the [script](./slinky) and add it to your path.

## How it Works

Slinky moves your dotfiles into a central repository (`$DOTFILES`, defaults to
`$HOME/dotfiles`), automatically tracks them in `slinky.map`, and replaces the
originals with symlinks.

```txt
~/.vimrc                                # Before: regular file
~/.vimrc → ~/dotfiles/vimrc             # After: symlink
```

This lets you commit your dotfiles to version control and symlink them on any
machine.

## Usage

### Add

```sh
slinky add ~/.vimrc                     # add to dotfiles root
slinky add ~/.vimrc ~/.bashrc           # add multiple at once
slinky add -p editors ~/.config/nvim    # specify subdirectory
slinky add ~/.vimrc -p editors          # flag can appear anywhere
```

### List

```sh
slinky list                             # list all tracked dotfiles
slinky list -p mac                      # list only those under a subdirectory
```

### Find

```sh
slinky find                             # find existing symlinks to your dotfiles repo
```

Useful for auditing which dotfiles are already managed or for finding symlinks
that weren't added through slinky.

### Install

```sh
slinky install                          # create all symlinks
slinky install -p mac                   # only symlinks under a subdirectory
```

### Drop

```sh
slinky drop config/nvim                 # move back to original location
slinky drop config/nvim .vimrc          # drop multiple at once
```

### Uninstall

```sh
slinky uninstall                        # remove all managed symlinks
```

## slinky.map

Slinky automatically manages a map file that tracks where each dotfile lives in
your repo and where its symlink should exist:

```txt
vimrc|~/.vimrc
config/nvim|~/.config/nvim
emacs|~/.emacs
```

Changes to slinky.map should be committed to version control alongside your
dotfiles.

## Why use slinky?

- ✨ Simple: One command to add, install, drop, or uninstall dotfiles.
- 🔒 Safe: Won't overwrite existing files without confirmation.
- 📜 Git-friendly: Just a folder and a map file - works with any VCS.
- 🗂️ Flexible: Organize files however you want - by host, OS, or category.
- 🔧 No config: Just works with your dotfiles repo.
- 🐚 Portable: Single POSIX shell script with zero dependencies.

## Inspiration

- [lnk](https://github.com/yarlson/lnk)
- [dotbot](https://github.com/anishathalye/dotbot)

## License

[MIT](./LICENSE)

