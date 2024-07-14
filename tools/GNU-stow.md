[ref](https://learn.typecraft.dev/tutorial/never-lose-your-configs-again/)
- Tool to create [symlinks](pointer to a directory) for data located on dedicated directory and makes them appear to be present in same place. eg `/usr/local/bin` could contains symlinks to files with `/usr/local/stow/emacs/bin`, `/usr/local/stow/perl/bin` it just creas symslinks there

## symlinks generation
- `ln` -> creates links
- `-S` flag for symlink
- `path` & `reference_name`
- `ln -S ~/source/soruce_file.sh new_reference_file_in_current_dir.sh`
- `ls -l` to see the symlink

> If we edit symlink, the changes will be reflected to the source too.

### How stow works, with symlinks
- stow copies that source file directory and applies the same tree structure to what ever given directory
![[Pasted image 20240714125556.png]]
![[Pasted image 20240714135355.png]]


## Commands
- For applying stow
- `stow -t ~/ dotfiles-repo`, fetches files under `dotfiles-repo` and adds symlinks in `~/` $HOME
