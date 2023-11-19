#protocol 
#TCP 

To overcome the Pipelining issue in [[HTTP 1.1]] i uses a special system called ==streams== and ==push==
- Where each request is bind in the form of streams (assigned with a stream id).
- so multiple streams can be passed in a single [[TCP]] connection.
```mermaid
graph LR
	id1[client]
	id2[server]
	r1["GET image.png (stream1)"]
	r2["GET style.css (stream2)"]
	r3["GET index.js (stream3)"]
	id1 --> r1
	id1 --> r2
	id1 --> r3
	subgraph "TCP Pipeline"
	r1
	r2
	r3
	end
	r1 --> id2
	r2 --> id2
	r3 --> id2
```
- All request is identified with the **stream_id**.
- Also with push with single request we can configure the server to response with multiple responses.
```mermaid
graph RL
	id1[browser]
	id2[server]
	r1["GET index.html (stream1)"]
	R1["res image.png (stream1)"]
	R2["res style.css (stream2)"]
	R3["res index.js (stream3)"]
	subgraph "Client"
	id1
	end
	id1 --> r1
	R1 --> id1
	R2 --> id1
	R3 --> id1
	 
	subgraph "TCP Pipeline"
	r1
	R1
	R2
	R3
	end
	subgraph "Server"
	id2
	end
	r1 --> id2
	id2 --> R1
	id2 --> R2
	id2 --> R3
	
```
- The default port is changed to ==443 ( with TLS )== instead of 80


Cons:
- Even though I have some advantage, since we are using additional streams. There is a additional ==parsing== process in the protocol level (i.e each stream has to be parsed now)