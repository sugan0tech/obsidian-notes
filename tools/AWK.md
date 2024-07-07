[ref](https://www.geeksforgeeks.org/awk-command-unixlinux-examples/)

Awk is a scripting language used for manipulating data and generating reports. The awk command programming language requires no compiling and allows the user to use variables, numeric functions, string functions, and logical operators.

- can be integrated with pipe of [[FZF]], `fzf | awk '{print $1}'` -> prints col one value of fzf result

```bash
awk options 'selection _criteria {action }' input-file > output-file
```


#### Examples
1. `awk '{print}' employee.txt` same as `cat employee.txt`
2. `awk '/manager/ {print}' employee.txt` with pattern matching
3. `awk '{print $1,$4}' employee.txt` col indexing ( $0 represents whole line )


Built in variables
1. `$1 $2 $3...` represents cols & `$0` represents whole line
2. `NR` current row's line
3. `$NF` last field 
4. Can introduce custom fillers too under "anything"
```bash
awk '{print NR "- " $1 }' geeksforgeeks.txt
# outputs as 
# 1 - Tarun
# 2 - Manav
# 3 - Praveen
```


#### Some complex query
```bash
awk 'BEGIN { for(i=1;i<=6;i++) print "square of", i, "is",i*i; }'
```
- outputs:
```
square of 1 is 1
square of 2 is 4
square of 3 is 9
square of 4 is 16
square of 5 is 25
square of 6 is 36
```

- conditions
```bash
awk '{ if($3 == "B6") print $0;}' geeksforgeeks.txt
```