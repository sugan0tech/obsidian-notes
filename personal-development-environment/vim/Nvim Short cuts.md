#shortcuts

## Edits
- `i` for insert
- `esc` for normal
- `v` for visual
- `c` for clear
- `d` for delete
- `y` for yank ( copy )
- `f` for finding withing a line and jumps to it
## Macros
- we can record set of key action and execute it later press `qq` in normal mode to enter macro recording, then after executing those commands and enter `q` to stop recording .
- to redo those macro use `@q` to execute that macro
- `Q` 
	- in visual `q` macro to multiple selected lines
	- in normal will replay and goes down to new line
 ```lua
vim.keymap.set('n', 'Q', "@qj") -- applying macro and enter new line
vim.keymap.set('x', 'Q', ':norm @q<CR>') -- applying macro to multiple lines

```
- we can also do recursive macro ( ==just record a macro inside it's record==)


## lazy.nvim cmd
`:Lazy`


## Treesitter
`<C-p>`
## Telescope
`<Leader>ff` files
`<Leader>fg` find with grep
`<Leader>fb` find for buffer
`<Leader>fh` find for help ( list of man pages for all tools inside nvim )
note for leader will be using ` ` space
`<C-S-P>` to move previous telescope menu
`<C-S-N>` to move next telescope menu


## NeoTree
`<Leader>n`
`<Leader>fs`
`P` for preview


## Surround
`ysiw` you surround inner word to add
`cs` change surround from to
for adding tag 
`cst`
with multiple files
- visually select
- `Shift-st<tag>` enter this
``


## Clipboard
`"+y` -> copy to clipboard
`<leader>y` -> custom just space + y for my scenaro
`<leader>p` -> for paste
```lua
vim.keymap.set('v', '<leader>y', '"+y')
vim.keymap.set('v', '<leader>p', '"+p')
```


## comments
- in visual
	- `gc` makes as line comment
	- `gb` makes as block comment
- in normal 
	- `gcc` makes current line as line comment
	- `gbc` makes current line as block comment
	
## key bindings
`=` - for indentations

## spliting
`C+w` will be the leader
normal vim motion keys `hjkl` ( with Shift for moving the window )
`s` for horizontal split
`v` for vertical split
`Ctrl+N` for new file

## snippets
`<C-k>` to move up
`<C-j>` to move down
`<C-Space` to select 
`<Enter>` initially or `<C-e>`to cancel the snippet

## code commands
- LSP ones
	- `gi` for go to code implementation
	- `gr` for go to references
	- `gd` for go to definition
	- `<Leader>ca` for code actions
	- `K` for list
## code markings
- Upper case for global lower case for buffers
- `mx` mark line as x
- `dmx` demark just removes the mark x
- `dm.` removes current line mark
- `'x` jumps to x
- `:marks a-zA-z` for showing those given marks

## Git
### Lazy git
- need to install manually via  a package manager
	- `yay -S lazygit`
- `<Leader>gg` for the window
### blame
- `<Leader>gm` for `:GitMessage`

## File level edits
-  uses oil plugin we can edit files as a normal buffer
-  `-` opens current parent direction in the buffer
- `_` opens current working directory
- `g.` toggling hidden
- `g\\` toggles trash ==for trash need to configure==
## search & replacement
- `*` for exact match of that word
- `g*` for partial search like occurrence in a word
- `:/term\c` for all cases `\C` for case sensitivity
- using `spectre`
- `<leader>sg` to spectre global file
- `<leader>sc` to spectre current file
- `dd` to toggle selected files
- `<leader>c` to confirm regrex

## LSP
- `hx` to trigger errors

## Harpoon
- `hm` to mark a file for harpoon
- `<C-h>` for toggling previous
- `<C-l>` for toggling next

## UFO - for code folding ==fails in mac==
- `zm` for global folding
- `zr` open folds ( excluding comments and imports)

## session storing
- using `nvim-possesion`
- `<leader>sl` to list session
- `<leader>sn` to list create session
- `<leader>sd` to list delete session
- `<leader>su` to list update session




