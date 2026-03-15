# 🌀 slinky

**A spring-loaded symlink-based dotfile manager built in shell.**

Move dotfiles to a central repo, replace originals with symlinks, and keep track
of them for easy install. That's it.

## Why use slinky?

- ✨ Simple: One command to add, install, drop, or uninstall dotfiles.
- 🔒 Safe: Won't overwrite existing files without confirmation.
- 📜 Git-friendly: Just a folder and a map file - works with any VCS.
- 🗂️ Flexible: Organize files however you want - by host, OS, or category.
- 🔧 No config: Just works with your dotfiles repo.
- 🐚 Portable: Single POSIX shell script with zero dependencies.

## Install

Slinky supports any system that ships with a POSIX-compliant shell.

### Package manager

```sh
brew install boldandbrad/tap/slinky
```

> If this project gains traction I may add slinky to other package managers in
> the future.

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
~/.vimrc → ~/dotfiles/vimrc             # After: symlink
```

By default, slinky assumes your dotfiles repository is located at
`$HOME/dotfiles`, but you can change this behavior by setting the `$DOTFILES`
environment variable.

Slinky maintains a plaintext map file (`slinky.map`) in the root of the dotfiles
repository to keep track the files and directoires it manages. Example:

```txt
vimrc|~/.vimrc
config/nvim|~/.config/nvim
emacs|~/.emacs
```

Combined, these patterns let you backup your dotfiles using your preferred
version control or cloud storage system for easy install on any machine.

## Usage

### Add

```sh
slinky add ~/.vimrc                     # add to dotfiles root
slinky add ~/.vimrc ~/.bashrc           # add multiple at once
slinky add -p editors ~/.config/nvim    # specify repo subdirectory
slinky add ~/.vimrc -p editors          # flag can appear anywhere
```

### List

```sh
slinky list                             # list all tracked dotfiles
slinky list -p mac                      # only those under the given subdirectory
```

### Find

```sh
slinky find                             # find existing symlinks to your dotfiles repo
```

Useful for auditing which dotfiles are already managed or for finding symlinks
that weren't added through slinky.

### Install

```sh
slinky install                          # create symlinks for all tracked dotfiles
slinky install -p mac                   # only dotfiles under the given subdirectory
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

## Example repos

Check out [slinky in the wild](https://github.com/topics/slinky-dotfiles). Add
your dotfiles github repo to the list by adding the `slinky-dotfiles` topic.

## Inspiration

- [lnk](https://github.com/yarlson/lnk)
- [dotbot](https://github.com/anishathalye/dotbot)

## License

[MIT](./LICENSE)

