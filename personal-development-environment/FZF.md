Ultimate command-line fuzzy finder

Dependency `Bat` -> for file preview

- fetches files from current directory
- builtin integration with tmux `fzf-tmux` spawns fzf in tmux pane

#### Fzf with file preview
- `--preview` flag to be used with view command ( `bat` will work the best) eg: `fzf --preview 'bat --style=numbers --color=always {}' `


#### Indexing
- try to index from current directory, ==lower the path higher the index time and memory usage==
- Have to ignore `.git .node_modules` for better indexing for src file

##### Line Reading
- we can pipe out log file for line based fzf

#### Easy piping with other tools 
[[AWK]]
- `sudo docker images | fzf | awk '{print $3}'` ( prints the images id)