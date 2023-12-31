## TPM
- tmux package manager

## commands
- `tmux ls` to list all active sessions
- `tmux -u a -t` to attach to a session
- `tmux new -s <session-name>` to create new session with name
- `tmux kill-session -t <session-name>` to kill session

## Short cuts
- `Ctrl+B` as leader
- `<Leader>w` ==the ultimate command==
- `<Leader>"` to split horizontally
- `<Leader>%` to split vertically
- to move `<Leader>}` 
- `<Leader> ` to toggle between different layouts
- `<Leader>B + arrows` to resize current pane
- `<Leader>B + q + num` to select given indexed pane

### windows
- `<Leader><num>` to jump to window
- `<Leader>c` to create new session
- `<Leader>:` give cmd to rename-window or `<Leader>,` to rename
- `<Leader><Alt><1-5>` rearrange the pane in a window

### session
- `<Leader>d` to exit session
- `<Leader>s` to checkout active sessions

