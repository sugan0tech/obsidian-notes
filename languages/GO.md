## Pointers
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

