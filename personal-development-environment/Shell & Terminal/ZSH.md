# Zsh Setup macOS

## Features Enabled

- Powerlevel10k Prompt
- Vim Mode & Navigation
- Fuzzy Finder (`Ctrl+R` for history search)
- Syntax Highlighting & Autosuggestions
- Environment Display (Git, Exit Code, Directory)
- Better `ls` (`eza`), Better `cd` (`zoxide`)
- Open Command in Vim (`Ctrl+E`)
- Terraform Autocompletion

> [!info] Required Packages Install the necessary dependencies before proceeding.

```sh
brew install zsh fzf zoxide bat eza git starship
brew install zsh-syntax-highlighting
brew install zsh-autosuggestions
```

## Set Zsh as Default Shell

```sh
chsh -s $(which zsh)
```

Restart the terminal.

## Install Oh My Zsh & Powerlevel10k

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

## Configure `.zshrc`

Edit `~/.zshrc` and add:

```sh
# Enable Powerlevel10k instant prompt
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi

export ZSH="$HOME/.oh-my-zsh"
ZSH_THEME="powerlevel10k/powerlevel10k"

# Plugins
plugins=(git fzf zsh-autosuggestions zsh-syntax-highlighting history-substring-search)
source $ZSH/oh-my-zsh.sh

[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh

# Syntax Highlighting & Autosuggestions
source /opt/homebrew/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source /opt/homebrew/share/zsh-autosuggestions/zsh-autosuggestions.zsh

# Smart cd (zoxide)
eval "$(zoxide init zsh)"
alias cd='z'

# Better ls (eza)
alias ls='eza --icons --color=auto'
alias ll='eza -lh --icons'
alias la='eza -lha --icons'

# Terraform Autocompletion
autoload -U +X bashcompinit && bashcompinit
complete -o nospace -C /opt/homebrew/bin/terraform terraform
```

## Enable Vim Mode & Navigation

```sh
# Enable Vim mode
bindkey -v

# Change cursor shape (block for Normal, line for Insert)
export KEYTIMEOUT=1
function zle-keymap-select {
    if [[ $KEYMAP == vicmd ]]; then
        echo -ne "\e[1 q"
    else
        echo -ne "\e[5 q"
    fi
}
zle -N zle-keymap-select

# Vim-style keybindings
bindkey -M vicmd 'h' backward-char
bindkey -M vicmd 'l' forward-char
bindkey -M vicmd 'w' forward-word
bindkey -M vicmd 'b' backward-word
bindkey -M vicmd '0' beginning-of-line
bindkey -M vicmd '^' beginning-of-line
bindkey -M vicmd '$' end-of-line
bindkey -M vicmd 'd' delete-char
bindkey -M vicmd 'dw' delete-word
bindkey -M vicmd 'dd' kill-whole-line
bindkey -M vicmd 'p' insert-last-word
bindkey -M vicmd 'u' undo
bindkey -M vicmd 'k' up-line-or-history
bindkey -M vicmd 'j' down-line-or-history

# Open command in Vim
autoload -Uz edit-command-line
zle -N edit-command-line
bindkey -M vicmd '^e' edit-command-line
```

## Fuzzy Finder for History (`Ctrl+R`)

```sh
export FZF_DEFAULT_OPTS="--height=40% --border --reverse --inline-info"
export FZF_CTRL_R_OPTS="--preview='echo {}' --preview-window=up:5:wrap"

# Bind fuzzy history search
bindkey '^r' fzf-history-widget
```

## Install Starship Prompt (Optional)

```sh
curl -sS https://starship.rs/install.sh | sh
echo 'eval "$(starship init zsh)"' >> ~/.zshrc
```

## Apply Changes

```sh
source ~/.zshrc
```

> [!tip] Powerlevel10k Configuration If Powerlevel10k needs configuration, run:

```sh
p10k configure
```

## Summary

| Feature                           | Keybinding / Command             |
| --------------------------------- | -------------------------------- |
| Vim Mode                          | `Esc` for Normal, `i` for Insert |
| Vim Navigation                    | `h`, `l`, `w`, `b`, `0`, `$`     |
| Delete Commands Like Vim          | `dd`, `dw`, `d`                  |
| History Navigation                | `k` (up), `j` (down)             |
| Fuzzy History Search              | `Ctrl + R`                       |
| Open Command in Vim               | `Ctrl + E`                       |
| Smart `cd` with Zoxide            | `z <directory>`                  |
| Improved `ls` with Icons & Colors | `ls`, `ll`, `la`                 |
| Powerlevel10k Theme               | Enabled                          |