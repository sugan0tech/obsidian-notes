## TPM
- tmux package manager

# tmuxifier
- storage path `~/.tmuxifier/layouts`
sample: 
```sh
session_root "~/Documents/GitHub/super-couscous/event-manager-go"

if initialize_session "event-manager-go"; then

  new_window "code"
  run_cmd "nvim"
  split_v 7
  run_cmd "go run ."
  split_h 10
  run_cmd "tty-clock -t -c"
  select_pane 1

  new_window "htop"
  run_cmd "htop"
  select_window 1

fi

finalize_and_go_to_session

```
- configured session for tmux
- `tmuxifier ls` for list session
- `tmuxifier ns` for creating new session

## commands
- `tmux ls` to list all active sessions
- `tmux -u a -t` to attach to a session
- `tmux new -s <session-name>` to create new session with name
- `tmux kill-session -t <session-name>` to kill session

## Short cuts
- `Ctrl+B` as leader
- `<Leader>w` ==the ultimate command==
- `<Leader>Z` to focus current pane and vice versa
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

