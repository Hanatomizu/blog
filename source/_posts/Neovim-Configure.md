---
layout: posts
title: Neovim Configure
tags: config
lang: en
date: 2024-02-11 21:06:05
---


# NeoVIM Configure

Neovim is a vim that supports plenty of plugins, in my opinion it is a great tool to be configured well.

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

    Well delete the code that you written in `init.lua` and create a folder called `lua`. Then we create a subfolder to put lua files, I use `core`. After that, create files to put configs. Frist of all, I use `options.lua` to put options like line number e.t.c.

    ```lua
    -- ---- lua/core/options.lua ---- --
    -- Define opt as vim.opt so that no need to write vim. any longer
    local opt = vim.opt

    -- Line Number
    opt.number = true
    opt.relativenumber = true
        
    -- Tabstop
    opt.tabstop = 4
    opt.shiftwidth = 4
    opt.expandtab = true
    opt.autoindent = true
    
    -- Disable wrap
    opt.wrap = false
    
    -- Disable cursor line
    opt.cursorline = false
    
    -- Enable mouse
    opt.mouse:append("a")
    
    -- Default split window
    opt.splitright = true
    opt.splitbelow = true
    
    -- Searching
    opt.ignorecase = true
    opt.smartcase = true
    
    ``` 
    
    Well run `:so` to try the new styles. Back to `init.lua` and add the following codes to load the lua file

    ```lua
    -- ----init.lua---- --
    require("core.options");
    ```

    Now restart neovim and you find that the settings works.

    Then, let's work on keymaps, create `core/keymaps.lua` 

    ```lua
    -- ----core/keymaps.lua --

    -- In order to make it easier to read, define vim.keymap as km
    local km = vim.keymap

    -- set the map leader key as space (Alternatively, you can set it as ',' e.t.c)
    vim.g.mapleader = " "
    
    -- use jk to quit insert mode (don't you feel uncomfortable to press ESC)
    km.set("i", "jk", "<ESC>")

    -- Have you learnt the style to set the keymaps?
    -- km.set("Mode", "Shortcut", "command")
    -- more examples:
    -- km.set("n", "<leader>t", ":terminal")
    -- km.set("v", "J", ":m '<-2<CR>gv=gv")
    -- Now write down the shortcuts you like.
    ```
    
    ```lua
    -- ----init.lua---- --
    -- ...
    require("core.keymaps")
    ``` 

4. Plugins
    
    Without doubt, the neovim currently like a normal text editor, make it different by adding plugins!

    `Lazy.nvim` is a plugin manager and it works well. 

    In order to add it, create a special directory to put plugins, call it 'plugins', and create `lazy.lua`. Open it and fill the code which from the README of [folke/lazy.nvim](https://github.com/folke/lazy.nvim).

    ```lua
    -- ----plugins/lazy.lua---- --

    local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
    if not vim.loop.fs_stat(lazypath) then
    	vim.fn.system({
    		"git",
    		"clone",
    		"--filter=blob:none",
    		"https://github.com/folke/lazy.nvim.git",
    		"--branch=stable", -- latest stable release
    		lazypath,
    	})
    end
    vim.opt.rtp:prepend(lazypath)
    ```

    


