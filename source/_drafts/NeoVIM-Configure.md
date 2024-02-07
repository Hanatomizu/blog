---
title: NeoVIM Configure
tags: config vim
---

# NeoVIM Configure

Neovim is a vim that supports plenty of plugins, it is better than vim if configured well.

## Install neovim

**macOS**  

brew
```sh
brew install neovim
```
ports
```sh
sudo port install neovim
```

**Arch Linux**

```sh
sudo pacman -S neovim
```

## Config neovim

When first started neovim you find that it is completely similar to vim. But you change your mind soon.

1. Create a config directory for neovim

    Neovim has a different config file from vim, create ~/.config/nvim/ to storage configs.

2. Start a initial config

    Neovim supports lua language so we can config it with lua rather than vimrc.

    Create init.lua at ~/.config/nvim so that neovim could detect it.  
    
    Then, we can do something interesting.

    Open the file with `nvim ~/.config/nvim/init.lua` to add some lua sentences to config something, such as line numbers and relative number.  

    ```lua
    vim.opt.number = true
    vim.opt.relativenumber = true
    ```

    Press ESC to quit insert mode and do `:so` to source the config. Have you found if there's any change to your neovim? The line number is shown.

    Well neovim provides a new way to manage configs, neovim support to config differect options in different files a.k.a. funcional config. It is believed that functional configure is a good choice.

3. Functional Config

    Well delete the code that you written in `init.lua` and create a folder called `lua`. 
