
#commands
## Git
1. merging head with previous commits
```bash
git rebase -i HEAD~n
```
2. removing last commit
```bash
git reset --hard HEAD^n
```
3. adding changes to head commit
```shell
git commit --amenmd --no-edit
```
4. stashing ( storing the added changes mostly due to branch changes )
```shell
git stash --includce-untracked # for stashing can also use -U for untracked files
git stash apply # applying the stashed changes
```
```shell
git stash save "with custom name"
git stash list
git stash pop stash@{id}
```
5. force pushing, check for any others commit in remote branch
```bash
git push --force--with lease
```
6. some alias in `.gitconfig`
```bash
[alias]
  graph = log --all --decorate --oneline --graph
  chart = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(auto)%d%C(reset)'

```

## Audio utility
1. Getting the audio output list
```shell
pactl list sinks short
```
2. setting the default
```shell
pactl set-default-sink <sink_name>
```

## Brightness utility
1. just brightness alone
```shell
xrandr --output DP-2 --brightness .6
```

## Disabling Linux Bell
```shell
xset b off
```


## Java versioning
```bash
## for arch we use  archlinux-java
archlinux-java status
sudo archlinux-java set java-21-openjdk
```

## Installing via directly downloaded pkg
```bash
sudo pacman -U --noconfirm your-package.pkg.tar.zst
```