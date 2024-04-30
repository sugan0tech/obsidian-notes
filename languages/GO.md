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
