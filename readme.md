# vi does dotfiles

inspired by [holman does dotfiles](https://holman/dotfiles).

## topical

This entire system is built around modular topical areas.  If you're adding a new area to your dotfiles, -- say, "nvim", you could simply add a `nvim` directory and put files in there.  Anything with an extension of `.zsh` will get automatically included into your shell.  Anything with an extension of `.symlink` will get symlinked to [your specified directory](#symlink).  Anything with an extension of `.home` will get automatically symlinked to `$HOME`.  Documentation can be found [here](#file-extensions).


## how it works

This works by having top-level folders acting as modules, which are able to be installed or uninstalled using `dotsi add <module>` and `dotsi remove <module` respectively. Read [below](#file-extensions) on how files in each module works.

### special rules

some files/modules have special rules:

- `bin/` module: Anything in this module will be added to `$PATH`.  It also has [special functionality](#bin-module)
- `<module>/path.zsh`: Any file named path.zsh is loaded first through your .zshrc and is expected to setup `$PATH` or similar.
- `<module>/install.sh`: Whenever you add a module, this file will be run.  Uses `.sh` instead of `.zsh` to avoid be loaded automatically.
- `<module>/uninstall.sh`: Whenever you remove a module, this file will be run.  Uses `.sh` instead of `.zsh` to avoid be loaded automatically.
- `<module>/updated.sh`: Whenever you update modules, this file will be run.  Uses `.sh` instead of `.zsh` to avoid be loaded automatically.

### bin module

This module acts like this when installed/updated:

- Run `updated.sh` if updated.
- Run `install.sh` if installed.
- Install/Update module.
- Create a bin.zsh file, which is automatically loaded, if not there.
- Update bin.zsh file in this way:
  - If a folder only contains files from `bin`, add folder to path.
  - If a folder doesn't only contain files from bin, add only the files from `bin` to path.
- Tell installer/updater that `bin` is installed.

## file extensions

Documentation for filetypes are located here.

All filetype-extensions will be removed, meaning `<file>.home` would be symlinked to `$HOME/<file>` instead of `$HOME/<file>.home`.

All of these abide by the [special rules](#special-rules).

### symlink

`.symlink` files act in a special way.  For a short answer, they act like stow files.  For a long answer, let's use neovim as an example:

So you have your `nvim` directory in the repo, and you want to have your `init.vim` or `init.lua` files symlink to `$HOME/.config/nvim/`.

If you just put `init.vim.symlink` in the `nvim` directory, it wouldn't work, as `.home` is used for `$HOME` files.

Instead, you can put it in either of (currently) two special module directories:

- `config`
- `custom`

Since you want it to be in `.config`, you would put `init.vim.symlink` in a new directory: `<module>/config/nvim` where `<module>` would be the `nvim` directory.  This way would symlink it to `$HOME/.config/nvim/init.vim`, which is where you want it.

If you wanted a file to be put in the directory `$HOME/.custom-path/<file>`, you would put the file in `<module>/custom/.custom-path/<file>`.

### home & zsh

Any files ending in `.home` would be sent to `$HOME` without the `.home` extension.  These can be placed in any directory within the module, and will still be symlinked to `$HOME`.  The same goes for the `.zsh` extension, except `.zsh` files are loaded automatically.
