#awesome 

### Why Haskell :SiHaskell:
- need to do something crazy ;)
- Lazy ( express are evaluated only on use)
- Functional
- Created by math nerds to be a nerdy language


### How weird it is
- No mutation
- Expression never have side effects ⁉
- same func with same args always results the same 


### Themes

Like how nd what format we gonna do our haskell
1. Types
	- First step always will be to write type info
	- As additional documentation
	- Turns runtime errors to compile time errors

2. Abstraction ( feat: `higher-order-func, polymorphism, type classes`)
3. Wholemeal programming
	- sample java - haskell code comparison
```java
int acc = 0;
for ( int i = 0; i < lst.length; i++ ) {
  acc = acc + 3 * lst[i];
}
```
-- > have to track of index, current sum state then process the result
```haskell
sum (map (3*) lst)
```

