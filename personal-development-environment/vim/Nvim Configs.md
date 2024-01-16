
- for two spaced tabs
```lua
vim.cmd("set expandtab")
vim.cmd("set tabstop=2")
vim.cmd("set softtabstop=2")
vim.cmd("set shiftwidth=2")
```
- for numbering
```lua
vim.cmd("set nu rnu")
```
- lazy.nvim integration
```lua
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
end -- if lazy is not present the installed it
vim.opt.rtp:prepend(lazypath)

local plugins = {}
local opts = {}

require("lazy").setup(plugins, opts) -- loads lazy with plugins and options

```
- to include a plugin 
	- add it `plugins tuple`
	- call it's `.setup` function

  ```lua
plugins = {
{ "catppuccin/nvim", name = "catppuccin", priority = 1000 }
}
local opts = {}

require("lazy").setup(plugins, opts) -- loads lazy with plugins and options

require("catppuccin").setup()
vim.cmd.colorscheme "catppuccin" -- sets up the color scheme


```


# seperate lua files for plugins
- `~/.config/nvim/plugins`  lazy will look for these files and automatically loads them on change
- so here let's move catppuccin to `catppuccin.lua` -> which should return the plugin tuble from `init.lua`
```lua
return {
  "catppuccin/nvim",
  name = "catppuccin",
  priority = 1000,
  config = function() -- config is the setup call in init.lua ( require().setup will be called automatically)
    vim.cmd.colorscheme "catppuccin" -- just execute the cmds
  end
},
```




## Plugins
- themes
	- catppuccin
 - fuzzyfinder
	 - telescope
- treesitter
	- for syntax highlighting
 - neotree
	 - for file explorer 
- lualine
	- custome status bar for nvim
 - [[lsp]]
 - none-ls
 - nvim-cmp
 - luasnip
	 - for code snippets and autocompletions
- surround
- telescope
- oil
	- edit like view for file changes
- Sniprun
	-  need to install the binaries externaly, by executing `install.sh` in their core file
- harpoon
- vim-tmuxifier