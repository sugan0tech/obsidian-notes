#shortcuts
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
`<Leader>ff` files
`<Leader>fg` find with grep
`<Leader>fb` find for buffer
`<Leader>fh` find for help ( list of man pages for all tools inside nvim )
note for leader will be using ` ` space


## NeoTree
`:Neotree`
`:Neotree filesystem reveal right`


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


## key bindings
`=` - for indentations

## spliting
`C+w` will be the leader
normal vim motion keys `hjkl` ( with Shift for moving the window )
`s` for horizontal split
`v` for vertical split
`Ctrl+N` for new file

snippets
`<C-k>` to move up
`<C-j>` to move down
`<C-Space` to select 
`<Enter>` initially or `<C-e>`to cancel the snippet
