#protocol 
#TCP

## No the connection stays for ever ==keep alive==

cons:
- Only one request can be send at a time
- Imagine visiting a page the request as follow in HTTP 1.1 as
	- fetching the initial file `GET index.html`, then we will needed to fetch the some assets like `image.png, style.css and index.js`. so a request for each file will be dispatched in ==sequence==.
	  ```mermaid
		graph LR
			id1["client"]
			id2["server"]
			r1["GET image.png"]
			r2["GET style.css"]
			r3["GET index.js"]
   
			id1 --> r1
   
		   subgraph TCP connection
			 r1 --> r2
			 r2 --> r3
			end
			r3 --> id2
	  ```
	  - imagine the application requires multiple no of file assests, getting those will be exponentially slower due to pipelineing of request.
	  - so how current browser rectifies it ( since most of the are using http1.1 ):
		  - By using 6 seperate TCP connections for each tab