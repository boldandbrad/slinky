# 🌀 slinky

**A simple-as-it-gets symlink-based dotfile manager.**

Track and sync dotfiles across workstations with ease. Use it to build out a
dotfile repository from scratch or seamlessly integrate it into your existing
workflow.

## Install

```sh
brew tap boldandbrad/tap
brew install slinky
```

Or download the [script](./slinky) manually and append it to your path.

## How it Works

Slinky moves your real dotfiles into a central repository, tracks them with a
simple map file at the repository root (`slinky.map`), and replaces them with
symlinks back in their original locations.

By default, slinky expects your dotfiles repository to be located at
`$HOME/dotfiles`. Set a custom location by exporting the `$DOTFILES` environment
variable in your shell configuration.

```txt
~/.vimrc (regular file)                      # Before
~/.vimrc (symlink to ~/dotfiles/.vimrc)      # After

~/.config/nvim (regular directory)           # Before
~/.config/nvim (symlink to ~/dotfiles/nvim)  # After
```

Slinky also allows you to specify exactly where in your dotfiles repo you want
to store the file or directory. This makes it easy to separate files by host or
use-case, however you see fit. See [Usage](#usage) for more info.

Then simply commit the added dotfiles and updated `slinky.map` file with your
preferred version control system and you're set.

## Usage

### Add

```sh
slinky add ~/.vimrc                   # move file/dir into dotfiles directory, create symlink
slinky add ~/.vimrc ~/.bashrc         # add multiple files/dirs at once
slinky add -p editors ~/.config/nvim  # specify subdirectory in dotfiles to move file/dir to
```

Use the `-p/--path` option to specify where in the dotfiles repo you'd like to
store the new dotfiles. You are free to adopt any organizational structure you
like, including separating dotfiles by host type or use-case.

### List

```sh
slinky list                           # all managed dotfiles in slinky.map
slinky list -p mac                    # only dotfiles under the given subdirectory
```

### Find

Useful for finding existing symlinks not (yet) managed by slinky.

```sh
slinky find                           # find all symlinks pointing to the dotfiles repository
```

### Drop

```sh
slinky drop config/nvim               # moves file/dir back, removes symlink
slinky drop config/nvim .vimrc        # drop multiple files/dirs at once
```

### Install

```sh
slinky install                        # create symlinks for all managed dotfiles
slinky install -p config              # only dotfiles under the given subdirectory
```

### Uninstall

```sh
slinky uninstall                      # remove all managed symlinks
```

## Why use slinky?

- ✨ Simple: Easy to add, install, drop, and uninstall dotfiles.
- 📜 Git-friendly: Centralizes dotfiles, ready for version control.
- Pattern agnostic: Doesn't impose structure, easy to adopt.
- Scriptable: Easily add it to your preferred bootstrap process.
- 🎒 Portable: Single posix-compliant shell script.
- 🎈 Lightweight: Under 250 lines with zero dependencies.

## Inspiration

- [lnk](https://github.com/yarlson/lnk)
- [dotbot](https://github.com/anishathalye/dotbot)

## License

[MIT](./LICENSE)

