### adding path
```nu 
$env.PATH = ($env.PATH | split row (char esep) | prepend '/home/sugan/.tmuxifier/bin/')
```
- Need to append to already existing path in `env.nu` file from config dir