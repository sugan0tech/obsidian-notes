- To be used to finding files under certain conditions

#### Usages
```bash
# Finding files under size contrainst
find /home/user/documents -type f -size 1024c
# -1024c greaterthan or +1024c lessthan
```
c for bytes, M for MB, k for kb

#### Flags
`-type` file type
`-size` file size
`-mtime` modified time
`-atime` accessed time
`-name "*.txt"` by name
`-perm 644` by permissions
`-delete` find and delete
`-exec rm {} \` find and execute command
`-user` by user
`-group` by user group

```bash
# finding a file containing 33 byes of text & user bandit6 user group & bandit7 as user
find / -type f -size 33c -group bandit6 -user bandit7
```