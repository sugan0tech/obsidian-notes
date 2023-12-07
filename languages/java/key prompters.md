### what's new in 21 
- `Unnamed class`, now we can directly make methods and run without defining it with the classes.
```java
void main(){
	System.out.println("sugan0tech");
}
```
however we have to enable this feature flag with this command while compiling and running
```bash
javac --srouce 21 --enable-preview Main.java
java --source 21 --enable-preview Main
```