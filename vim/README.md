# VIM

Personal notes about vim (Vi Improved) taken from a lot of sites and different books like [From WTF to OMG](https://jovicailic.org/mastering-vim-quickly/) and [Rob vim tips](https://rwx.gg/tools/editors/vim/)

## Installation

### Debian Based

```bash
# Vim with support for the clipboard and python
sudo apt install vim-gtk3
```

### Arch Linux

```bash
# Vim with support for GTK and X
sudo pacman -S gvim
```

### Gentoo

Configure vim with these flags

```/etc/portage/package.use/vim
app-editors/vim terminal ruby
```

```bash
sudo emerge -a app-editors/vim
```

## Configuration

I use the [Plugin Manager Plug](https://github.com/junegunn/vim-plug) for my vim plugins

```bash
# Installation
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

# Inside vim
:PlugInstall
```

## Intro

Little tips to start with vim and survive

- [Vim Survival Guide](Intro/README.md)

## Learn Vim

Resources to learn vim from the terminal

- [Learn Vim](LearnVim/README.md)

## Working with Files

Commands to play with files inside vim

- [Working with Files](WorkWithFiles/README.md)
