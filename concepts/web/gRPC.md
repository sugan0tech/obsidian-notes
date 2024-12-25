#backend
#client-server
#api
#protocol
## google Remote Procedure Call


![[Pasted image 20231119183035.png]]

Pros
- proto buffers , binary transport data 

cons
- no browser support
- extra learning curve with the proto buffers schemes


### Update: Introduction of the Opaque API for Go Protocol Buffers

The Go team has introduced a new **Opaque API** for Protocol Buffers to enhance code safety and maintainability by restricting direct access to the internal fields of generated protobuf message structs. This replaces direct field manipulation with getter and setter methods, enforcing stricter adherence to message constraints.

#### Comparison of APIs:

| **Aspect**              | **Open Struct API (Older)**                                     | **Opaque API (Newer)**                                       |
|-------------------------|---------------------------------------------------------------|------------------------------------------------------------|
| **Field Access**        | Direct access to struct fields.                                | Access fields through getter and setter methods.           |
| **Encapsulation**       | Limited; fields are fully exposed.                            | Improved; fields are unexported and encapsulated.          |
| **Safety**              | No control or validation on changes.                          | Controlled access with validation logic.                   |
| **Maintainability**     | Structural changes impact external code.                      | Methods abstract internal changes, ensuring flexibility.   |

---

#### Code Example:

**Open Struct API**:
```go
p := &Person{}
p.Name = "Alice"
p.Age = 30
````

**Opaque API**:

```go
p := &Person{}
p.SetName("Alice")
p.SetAge(30)

name := p.GetName()
age := p.GetAge()
```

---

#### Key Notes:

- Developers must regenerate `.pb.go` files using the updated `protoc` compiler to adopt the Opaque API.
- The **Open Struct API** remains supported for backward compatibility, ensuring a smooth transition.

For further details, refer to the official [Go Blog: Protobuf Opaque API](https://go.dev/blog/protobuf-opaque).
