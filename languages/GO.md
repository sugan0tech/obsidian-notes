## Commands

 `go mod tidy`

The `go mod tidy` command in Go ensures that your project's `go.mod` and `go.sum` files accurately reflect the current state of your codebase. It performs the following actions:

- **Adds Missing Dependencies**: Incorporates any modules required by your code that are not yet listed in `go.mod`.
- **Removes Unused Dependencies**: Eliminates modules from `go.mod` that are no longer utilized in your code.
- **Updates `go.sum`**: Adjusts the `go.sum` file to include checksums for new dependencies and removes those for unused ones.

Regularly running `go mod tidy` helps maintain a clean and efficient dependency graph, preventing potential issues related to unused or missing modules.

**Reference**: [Go Modules Reference](https://go.dev/ref/mod)

## Pointers
- `&` to get the address
- `*` to get the contents of a pointer
by default function parameter will be duplicated and passed ( so no mutation observed ). So we have to use pointer to see the data updation.
```go
func main(){
  data := "original data"
  check(data)
  fmt.Println(data) 
}

func check(data string){
  data = "updated one"
}
```
outputs: `original data` ==no data mutation is observed==
```go
func main(){
  data := "original data"
  check(&data)
  fmt.Println(data) 
}

func check(data *string){
  *data = "updated one"
}

```
outputs: `updated data`
so that why `fmt.Scan()` uses pointer to update


## Packages
- accessible is created via `Caps in it's begining name`
	- `var Avar`
	- `func TestFunc` will be exported & `func testFunc` will not
	

## Types and methods
```go
type user struct {
	firstName  string // note with small f means private
	secondName string
	age        int
	cratedAt   time.Time
}

// this called as a receiver of types
//       |
func (u user) printUser() {
	fmt.Println(u.firstName, u.secondName)
}

```
we can also add custom types to existing types 
```go
type str string
func (str) log(){} // only done for custom types
```
## Interfaces
```go
type human interface {
	method() return
}
```
by default what every types gonna use the methods of a interface will be considered as the inherited one, no need of `explicitly marking it`



## Functions
- supports function as type so can be used as parameter
- also can be returned
###  anonymous function
```go
  fmt.Println(transformNumbers(&arrays, func (number int) int {
    return number * 5
   }))
```
### variadiac function
```go
func main() {
  filler(1, 3, 5, 6, 6, 8)
}

func filler(lst ...int){
  fmt.Println(lst)
}

```

## Go Routines
```go
func main(){
  dones := make([]chan bool, 4)

  dones[0] = make(chan bool)
  go greet("He:hi", dones[0])
  dones[1] = make(chan bool)
  go greet("She:hi", dones[1])
  dones[2] = make(chan bool)
  go slowGreet("He:how are you", dones[2])
  dones[3] = make(chan bool)
  go greet("She: I have a boyfriend", dones[3])

  for _, done := range dones {
    <- done // displays the data in the channel
  }
// or 
  for done := range dones {
    fmt.Print(done)
  }
}

func greet(str string, doneChan chan bool){
  fmt.Println(str)
  doneChan <- true
}

func slowGreet(str string, doneChan chan bool){
  time.Sleep(3 * time.Second)
  fmt.Println(str)
  doneChan <- true // sends this data to given channel
}

```
`channel` communication link, can be used as a communication between routines. also can store the results of each routine and can be gathered

`handelling returns`  - with go routines we can't use return, instead we can pass those results via channels and utilise them in the `select` statement
```go
	taxRates := []float64{0, 0.7, 0.1, 0.15}
	doneChans := make([]chan bool, len(taxRates))
	errorChans := make([]chan error, len(taxRates))

	for index, taxRate := range taxRates {
		doneChans[index] = make(chan bool)
		fm := filemanager.New("prices.txt", fmt.Sprintf("result_%.0f.json", taxRate*100))
		// cmd := cmdmanager.New()
		priceJob := prices.NewTaxIncludedPriceJob(fm, taxRate)
		go priceJob.Process(doneChans[index], errorChans[index])  // gives error
	}

	for index := range taxRates {
		select {
		case err := <-errorChans[index]:
			if err != nil {
				fmt.Println(err)
			}
		case <-doneChans[index]:
			fmt.Println("Done!")
		}
	}
```



## net/http
source: [digital-ocean net/http](https://www.digitalocean.com/community/tutorials/how-to-make-an-http-server-in-go)
handlers:
	handles the req & res
```go 
func getRoot(w http.ResponseWriter, r *http.Request) {
	fmt.Printf("got / request\n")
	io.WriteString(w, "This is my website!\n")
}
func getHello(w http.ResponseWriter, r *http.Request) {
	fmt.Printf("got /hello request\n")
	io.WriteString(w, "Hello, HTTP!\n")
}
```
- note Request is a pointer 
	- `Efficency` may contains associated payload in large chunks
	- `Mutablity` if we have to use middle-ware parser



--- AI generated further add on-s
### **Go Cheat Sheet for Java/.NET Developers**

### **1. Basic Syntax**

#### **Main Function**

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

#### **Variables**

```go
// Declaration
var x int = 10
var y = "Hello"

// Short Declaration
z := 42

// Constants
const Pi = 3.14
```

#### **Functions**

```go
func add(a int, b int) int {
    return a + b
}

// Multiple Return Values
func divide(a, b int) (int, int) {
    return a / b, a % b
}
```

---

### **2. Control Structures**

#### **If-Else**

```go
if x > 10 {
    fmt.Println("x is greater than 10")
} else {
    fmt.Println("x is 10 or less")
}
```

#### **Switch**

```go
switch day := "Monday"; day {
case "Monday", "Tuesday":
    fmt.Println("Workday")
default:
    fmt.Println("Weekend")
}
```

#### **For Loop**

```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}

// While-like Loop
i := 0
for i < 5 {
    fmt.Println(i)
    i++
}
```

---

### **3. Collections**

#### **Slices (Dynamic Arrays)**

```go
numbers := []int{1, 2, 3}
numbers = append(numbers, 4) // Add element
fmt.Println(numbers[0])      // Access element
```

#### **Maps (Dictionaries)**

```go
m := map[string]int{"key1": 10, "key2": 20}
m["key3"] = 30
delete(m, "key2") // Remove key
fmt.Println(m["key1"])
```

#### **Linked List**

```go
import "container/list"

l := list.New()
l.PushBack(10)
l.PushFront(5)
for e := l.Front(); e != nil; e = e.Next() {
    fmt.Println(e.Value)
}
```

---

### **4. Strings and Runes**

#### **String Manipulation**

```go
s := "Hello, World"
fmt.Println(len(s))       // Length
fmt.Println(s[0:5])       // Substring: "Hello"
fmt.Println(s + " Go!")   // Concatenation
```

#### **Runes (Unicode Characters)**

```go
s := "Hello, 世界"
runes := []rune(s)
fmt.Println(len(runes))          // Rune count
fmt.Println(string(runes[7:9]))  // Access substring: "世界"
```

---

### **5. Error Handling**

Go uses explicit error handling instead of exceptions:

```go
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("cannot divide by zero")
    }
    return a / b, nil
}

result, err := divide(10, 0)
if err != nil {
    fmt.Println("Error:", err)
} else {
    fmt.Println("Result:", result)
}
```

---

### **6. Structs and Methods**

#### **Struct**

```go
type Person struct {
    Name string
    Age  int
}

p := Person{Name: "Alice", Age: 30}
fmt.Println(p.Name)
```

#### **Methods**

```go
func (p Person) Greet() {
    fmt.Printf("Hello, my name is %s\n", p.Name)
}
```

---

### **7. Goroutines and Channels (Concurrency)**

#### **Goroutine**

```go
go func() {
    fmt.Println("Running in a goroutine")
}()
```

#### **Channel**

```go
ch := make(chan int)
go func() { ch <- 42 }()
fmt.Println(<-ch) // Receive value from channel
```

---

### **8. Binary Operations**

```go
a, b := 6, 3
fmt.Println(a & b)  // AND
fmt.Println(a | b)  // OR
fmt.Println(a ^ b)  // XOR
fmt.Println(a << 1) // Left Shift
fmt.Println(a >> 1) // Right Shift
```

---

### **9. Packages and Imports**

```go
import "fmt"
import "math"
```

#### Common Packages:

|**Package**|**Purpose**|
|---|---|
|`fmt`|Input/output formatting|
|`math`|Math functions (e.g., `math.Abs`)|
|`strings`|String manipulation|
|`strconv`|String-to-number conversions|
|`container/list`|Linked list implementation|

---

### **10. Tips for Java/.NET Developers**

|**Java/.NET Concept**|**Go Equivalent**|**Notes**|
|---|---|---|
|`ArrayList`|`[]T` (Slice)|Use `append` for resizing.|
|`HashMap`|`map[keyType]valueType`|Built-in map type.|
|`LinkedList`|`container/list`|Requires manual traversal with pointers.|
|`char`|`rune`|Runes support full Unicode.|
|`Exception`|`error`|No exceptions; explicit error handling.|
|`Multithreading`|Goroutines and Channels|Lightweight threads (concurrency primitives).|
|`try-catch-finally`|Explicit error handling with `if`|No `finally`; use `defer`.|
|`Generic Types`|Generics (`Go 1.18+`)|Use `interface{}` for older Go versions.|

---

### **11. Go vs Java/.NET Quick Comparison**

|Feature|Go|Java/.NET|
|---|---|---|
|**Memory Management**|Garbage-collected|Garbage-collected|
|**Type System**|Static (with type inference)|Static|
|**Inheritance**|Composition over inheritance|Class-based inheritance|
|**Exceptions**|No exceptions|try-catch-finally|
|**Generics**|Available (Go 1.18+)|Fully supported|
|**Multithreading**|Goroutines, Channels|Threads, Tasks|
|**Build System**|Built-in (`go build`)|Gradle/Maven, MSBuild|

---

### **12. Go Tools**

- **Build & Run**: `go run main.go`
- **Compile**: `go build`
- **Format Code**: `go fmt`
- **Install Packages**: `go get <package>`
- **Unit Tests**: `go test`

---

### **13. Sample Go Project**

#### `main.go`:

```go
package main

import "fmt"

func main() {
    fmt.Println("Welcome to Go!")
}
```

#### Run:

```bash
go run main.go
```