# 🌀 slinky

**A spring-loaded symlink-based dotfile manager built in shell.**

Move dotfiles to a central repo, replace originals with symlinks, and keep track
of them for easy install. That's it.

## Why use slinky?

- ✨ Simple: One command to add, install, drop, or uninstall dotfiles.
- 🔒 Safe: Won't overwrite existing files without confirmation.
- 📜 Git-friendly: Just a script and a map file - works with any VCS.
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

Slinky maintains a simple pipe-separated value file (`slinky.psv`) in the
dotfiles repository root to track the files and directories it manages. Example:

```txt
vimrc|~/.vimrc
config/nvim|~/.config/nvim
emacs|~/.emacs
```

Combined, these patterns let you backup your dotfiles using your preferred
version control or cloud storage system for easy install on any machine.

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

That's it! All that's left is to commit and push your changes to a remote repo,
including the slinky.psv file.

To install your dotfiles onto another computer, read
[Syncing multiple computers](#syncing-multiple-computers).

### Migrating to slinky

Follow this guide if you have an existing symlink based dotfile repository and
you'd like to migrate to slinky.

1. Move your local repository to `~/dotfiles`, or set `$DOTFILES` to tell slinky
where it is
2. Find your existing symlinks: `slinky find`
3. Import them: `slinky import symlink1 symlink2 ...`

That's it! All that's left is to delete your previous dotfile manager.

To install your dotfiles onto another computer, read
[Syncing multiple computers](#syncing-multiple-computers).

### Syncing multiple computers

Follow this guide to easily keep your dotfiles in sync across multiple devices.

1. Make sure slinky is [installed](#install) on each computer
2. Clone your git repository, example:
`git clone https://github.com/yourname/dotfiles.git ~/dotfiles`
3. Backup any local dotfiles
4. Install dotfile symlinks: `slinky install`

That's it!

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
slinky find                             # search the user's home directory for untracked symlinks pointing to the dotfiles repo
slinky find ~/.config                   # search in the specified directory
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
```

### Help

```sh
slinky help                             # show the help message
```

## Example repos

Check out [slinky in the wild](https://github.com/topics/slinky-dotfiles). Add
your dotfiles github repo to the list by adding the `slinky-dotfiles` topic.

## Inspiration

- [lnk](https://github.com/yarlson/lnk)
- [dotbot](https://github.com/anishathalye/dotbot)

## License

[MIT](./LICENSE)

