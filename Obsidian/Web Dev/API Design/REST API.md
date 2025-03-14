
> [! info] Info 
> REST is an acronym for ==**RE**presentational **S**tate **T**ransfer== and an architectural style for **distributed hypermedia systems**. Roy Fielding first presented it in 2000 in his famous [dissertation](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm). Since then it has become one of the most widely used approaches for building web-based APIs (_Application Programming Interfaces_).

> REST is not a protocol or a standard, it is an architectural style.

---
### REST API Principles
1. Client-Server Architecture
	- The **client-server design pattern** is a way to organize software so that the tasks of an application are split between two parts:
		1. The **client**: Handles what the user interacts with (like a web app or mobile app).
		2. The **server**: Handles storing data and doing the "heavy lifting" (like retrieving or saving data to a database).
2. Statelessness
	- Each request from a client to a server must contain all the information needed to understand and process the request. 
	- The server should not store any client session information between requests.
	- Benefits > Drawbacks
	- Benefits
		- Authentication/Authorization
			- Every request must include credentials
			- No server-side sessions
			- Usually implemented with tokens (JWT, OAuth)
		- Scalability
			- Requests can be handled by any server instance
			- Easier horizontal scaling
			- No need to sync session state between servers
		- Reliability
			- No session state to lose
			- Each request is independent
			- System more resilient to failures
		- Caching
			- Responses can be cached more effectively
			- No session-specific content
			- Better performance
	- Drawbacks
		- Large requests
3. Cacheability
	- Server's response to client requests can (and should) be stored temporarily (cached) by clients, proxies or intermediate layers to improve performance, reduce server load, and minimize redundant data transfer.
	
	- Why do I need caching?
		- Reduced latency
		- Reduced load on the database
		- Reduced network costs
	- Types of Caches by Level
		- Client caching
		- CDN caching
		- Web Server caching
		- Database caching
		- Application caching
	- Types of Caches by Design
		- Global Cache
			- Has shared cache across application cluster
		- Distributed Cache
			- Multiple duplicate cache for each application
	- Caching strategies & Invalidation
		- Write-around cache (cache-aside or Lazy loading) - ==Reactive approach==
			- ![[lazy-loading.png]]
			- 
			- Cache stays slim and contains data that it really needs
				- Update the cache only when update the database
			- Straightforward: Used by default by most caching providers
			- ==Cache gets filled only after a cache miss, meaning 3 trips.==
		- Write-Through cache - ==Proactive approach==
			- ![[write-through-cache.png]]
			- Well-synced with the database, resulting in fewer reads
			- ==Infrequently-requested data is also written to the cache, resulting in a larger cache.==
		- Write-Back cache - ==Not Good== 
			- ![[write-back-cache.png]]
				- ==Cache and storage can be out of sync in edge cases==
	- Eviction policies
		- Least Recently Used (LRU)
		- Least Frequently Used (LFU)
		- Last In First Out (LIFO)
		- Random Replacement (RR)
		- Cache updating
			- Setting a TTL (Time to live) eventually self-cleans-itself
				- Every entity that is inside the cache gets automatically removed at some time
			- Lazy read repair
				- Every time we read from a db, we randomly pick request, even though the data is inside cache we still read it from the db to make sure that the data is up to date
			- Redis SCAN background process
	- When not to add a caching layer?
		- Adding a caching layer makes no difference in performance
		- ==High randomness of requests==
		- Frequently updated data
	- [Source](https://youtu.be/bP4BeUjNkXc?feature=shared)
4. Uniform Interface
	- A ==Uniform interface== means that there's a standard way for clients (like apps or browsers) to talk with servers(where the data of functionality lives). This makes everything predictable and consistent, no matter what kind of resource or operation you're dealing with. 
	
	- To make this work **REST** has four main rules:
		- Identification of Resources:
			- Each data should have it's own ==unique== address called URI
			- Example:
				- A user might be at `/users/123`.
				- A product might be at `/products/45`.
		- Manipulation of Resources through ==Representations==
			- Resources aren’t handled directly. Instead, you work with a **representation** of the resource, which is usually sent in a format like JSON or XML.
			- Example
				- When you request `/users/123`, the server doesn’t give you the raw database record for the user. Instead, it gives you a JSON representation like this:
				- `{   "id": 123,   "name": "John Doe",   "email": "john@example.com" }`
				- If you want to update the user’s name, you’d send back a modified version of this JSON.
		- Self-Descriptive Messages
			- Each request or response should include all the information needed to understand it.
			- It should also provide extra information about the actions that the user can do with the resource.
			- Example
				- When the server sends you data about a user, it might also include instructions (in the headers or links) on what actions you can take next, like editing or deleting the user.
				
				- A response might be something like:
					- ```{
						  "id": 123,
						  "name": "John Doe",
						  "links": [
						    {"rel": "edit", "href": "/users/123/edit"},
						    {"rel": "delete", "href": "/users/123"}
						  ]
						}```
					- These links tell the client what it can do without needing to know beforehand.
		- Hypermedia as the Engine of Application State (HATEOAS)
			- The client only needs to know the starting point of the API and then the rest of the functionality should be provided by the server dynamically, guiding the user without knowing the API in detail.
			- Example
				- A client starts with `/api`, and the server responds with links to different resources:
					- ```{
						  "links": [
						    {"rel": "users", "href": "/users"},
						    {"rel": "products", "href": "/products"}
						  ]
						}```
					- The client doesn’t need to know how to find users or products beforehand. The server provides everything dynamically.
5. Layered system
	- A **layered system** organizes an application into different levels, or "layers," where each layer has a specific role or responsibility. These layers interact with each other in a step-by-step way, meaning one layer talks only to the layer directly above or below it. This makes the system easy to maintain, scalable and secure.
	- Layman's Example: ==The MVC Pattern==
		- The **MVC pattern** (Model-View-Controller) is a simple example of a layered system:
			1. **Model** (Data Layer): This layer handles the data and business logic, like getting or saving information in a database.
			2. **View** (Presentation Layer): This layer shows the user interface, like buttons and forms on a webpage or app.
			3. **Controller** (Application Layer): This layer acts as the "middleman" between the user (View) and the data (Model). It takes user input, processes it, and updates the model or view as needed.
6. Code on demand (Optional)
	- Mostly, we send data representations in JSON or XML formats, but, at times we might need to send executable code to support  a part of our application. For example, a client might call your API to get a UI widget rendering code. It is permitted.
	- This is not about ==just== returning executable code but violating the Rest API constraints in specific situations when needed for practicality or functionality. 
[Source](https://restfulapi.net) and ChatGPT

---


### Stages of Maturity
>  Assessed Using the Richardson Maturity Model
##### Level 0: The Swamp of POX (Plain Old XML)

- **Description**: 
	- The API functions as a single endpoint (e.g., `/api`).

- **Characteristics**:
	  - All requests are sent to a single endpoint.
	  - Often uses HTTP POST for all operations.
	  - Data is sent as XML or JSON, but the API doesn’t adhere to REST principles.
  
- **Example**: 
	- A SOAP-based or RPC-style API over HTTP.

- **Note**: 
	RPC (Remote Procedure Call) focuses on functions rather than resources. There are no request methods like GET or PUT; instead, operations like `updateUserProfile()` are used, typically with only HTTP POST.

---

##### Level 1: Resources

- **Description**: 
	- The API begins to treat data as resources, with each resource having a unique URI.

- **Characteristics**:
	- Resources are introduced (e.g., `/users`, `/orders`).
	- URIs represent entities or resources.
	- HTTP methods are not used consistently.
  
- **Example**: 
	- An API with endpoints like `/users` and `/users/{id}`, but every operation (GET, POST, DELETE) still uses POST.

- **Note:**
	- **URI**: Uniform Resource Identifiers (can be URLs or URNs).
	- **URL**: Specifies how to access a resource (e.g., `https://library.example.com/books/0-486-27557-4`).
	- **URN**: Focuses on uniquely identifying the resource itself (e.g., `urn:isbn:0-486-27557-4`).

---

##### Level 2: HTTP Verbs

- **Description**: 
	- The API uses HTTP methods (verbs) correctly and leverages HTTP status codes for communication.

- **Characteristics**:
	- Proper use of HTTP methods and status codes:
	    - **GET**: Read operations.
	    - **POST**: Create operations.
	    - **PUT**: Update operations.
	    - **DELETE**: Delete operations.
	- Still lacks hypermedia controls (HATEOAS)

- **Example**: 
	- `/users/{id}` supports GET, PUT, DELETE, while `/users` supports POST for creating new users.

- Note:
	- 

---
##### Level 3: Hypermedia Controls (HATEOAS)

- **Description**: 
	- The API achieves full REST maturity by implementing **Hypermedia as the Engine of Application State (HATEOAS)**. Responses contain links to related actions or resources, guiding the client dynamically through the API without prior knowledge of its structure.

- **Characteristics**:
	- Hyperlinks are included in API responses to guide clients (e.g., a `user` object might include a link to its `orders`).
	- Clients rely on these links to navigate resources dynamically.
	- Fully self-descriptive API interactions.
  
- **Example**: 
	```json
	{
	    "id": 123,
	    "name": "John Doe",
	    "email": "john@example.com",
	    "_links": {
	        "self": { "href": "/api/users/123" },
	        "orders": { "href": "/api/users/123/orders" },
	        "update": { "href": "/api/users/123", "method": "PUT" },
	        "delete": { "href": "/api/users/123", "method": "DELETE" }
	    }
	}
	```

- **Note**: 
	- 

---
[Source](https://youtu.be/CVBpYfPKGlE?feature=shared) and ChatGPT



---


### REST API Best Practices
[Source #1](https://youtu.be/7nm1pYuKAhY?feature=shared)  [Source #2](https://youtu.be/CVBpYfPKGlE?feature=shared)
##### Naming
1. No trailing **forward slash**
	- `/api/GetOrderActiveItems` ==/== `?id=123` ==Wrong==
	- `api/GetOrderActiveItems?id=123` ==Correct==
2. Hierarchical relationships
	- `api/GetOrderActiveItems?id=123` ==Wrong==
	- `api/order/123/GetActiveItems` ==Correct==
3.  Use hyphens for better readability
	- `api/order/123/GetActiveItems` ==Wrong==
	- `api/order/123/get-active-items` ==Correct==
4.  No actions (verbs)
	- `api/order/123/get-active-items` ==Wrong==
	- ==GET== `api/order/123/active-items` ==Correct==
5. Use plurals
	-  ==GET== `api/order/123/active-items` ==Wrong==
	-  ==GET== `api/orders/123/active-items` ==Correct==
6. Avoid complexity
	- Do not use hierarchical relationship for more than two levels
	-  ==GET== `api/order/123/items/active` ==Correct==
7. Use nouns to represent resources
##### Versioning
- Can be specified inside query, path (URI) or body param.
##### Security
- All the communication should happen over a secure protocol like HTTPS
	- HTTP transfers data over the internet using plain text
	- HTTPS uses a cryptographic protocol which encodes data to ensure to protect the attacks from man-in-the-middle attacks
- Some kind of token based authentication should be implemented 
	- JWT Tokens are kind of industry standard
		- The Tokens should be stored secured in the client side so that they can't be compromised using cross side scripting (XSS) or Cross side request forgery (CSRF) attacks
		- Token expiration and renewal policies should be implemented
		- Tokens should be validated in the server

##### Responses
- Never return plain text
	- when the client application receives plain text, it needs to read and process the data to extract necassary data which can cause inefficiencies.
- Use Structured Media Types
	- JSON -> Simple, Efficient, Widely Adopted
	- XML -> Wordy, Large size
	- YAML -> Expressive, Lacks support
##### Exceptions
- **Status codes**:
	- Proper use of HTTP methods:
	    - **GET**: Read operations.
	    - **POST**: Create operations.
	    - **PUT**: Update operations.
	    - **DELETE**: Delete operations.
	- Meaningful HTTP status codes.
	- [Source](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#informational_responses)
		-  **1xx Informational Responses**
			- ==**Description**: Indicates that the request has been received and is continuing to be processed.==
			  - **100 Continue**: The client should continue with its request after receiving this status.
			  - **101 Switching Protocols**: The server agrees to switch protocols as requested by the client.
		-  **2xx Success**
			- ==**Description**: Indicates that the request was successfully received, understood, and accepted.==
			  - **200 OK**: The request succeeded, and the result depends on the HTTP method (GET, POST, etc.).
			  - **201 Created**: A new resource has been successfully created.
			  - **202 Accepted**: The request has been accepted for processing, but it’s not yet complete.
			  - **204 No Content**: The server successfully processed the request but returns no content.
		-  **3xx Redirection**
			- ==**Description**: Indicates that further action is needed to complete the request.==
			  - **301 Moved Permanently**: The resource has been permanently moved to a new URL.
			  - **302 Found**: Temporary redirection; resource is temporarily under a different URI.
			  - **304 Not Modified**: Indicates the resource has not been modified since the last request.
			  - **307 Temporary Redirect**: Temporarily redirects the client but ensures the method stays the same.
			  - **308 Permanent Redirect**: Permanently redirects the client; method remains unchanged.
		-  **4xx Client Errors**
			- ==**Description**: Indicates an issue with the client's request.==
			  - **400 Bad Request**: The server cannot process the malformed request.
			  - **401 Unauthorized**: Authentication is needed to access the resource.
			  - **403 Forbidden**: The server understands the request but refuses to authorize it.
			  - **404 Not Found**: The resource could not be found.
			  - **405 Method Not Allowed**: The request method is not supported for the resource.
			  - **408 Request Timeout**: The server timed out waiting for the request.
			  - **409 Conflict**: Indicates a conflict with the request (e.g., version control).
			  - **429 Too Many Requests**: The client has sent too many requests in a short time.
		-  **5xx Server Errors**
			- ==**Description**: Indicates an issue with the server while processing the request.==
			  - **500 Internal Server Error**: A generic server error message.
			  - **501 Not Implemented**: The server does not recognize the request method.
			  - **502 Bad Gateway**: A gateway or proxy server received an invalid response.
			  - **503 Service Unavailable**: The server is temporarily unavailable.
			  - **504 Gateway Timeout**: A gateway or proxy server timed out.
##### HATEOAS

##### Limit Request Counts
- `/customers?limit=50`
- `/customers?start=50&limit=100`
##### Idempotency

##### Async operations
###### **1. Initial Request (POST)**

- **Endpoint**: `/orders/create`
- **Payload**: `{ orderId: 99 }`
- **Response**:
    - **Status Code**: `202 Accepted`
        - Indicates that the server has received the request and accepted it for processing, but the processing is not yet complete.
    - **Headers**:
        - Includes a `Location` header pointing to `/orders/status/99`, where the client can check the status of the request.

---

###### **2. Asynchronous Processing**

- After the server accepts the request, the processing happens asynchronously.
- This means the operation is performed in the background and might take a significant amount of time (e.g., **5 minutes** in this case).

---

###### **3. Checking Status (GET)**
- **Endpoint**: `/orders/status/99`
- **Response**:
    - **Status Code**: `200 OK`
        - Indicates the request was successful, and the server is providing the status of the asynchronous operation.
    - **Body**:
        - Contains the current status of the operation, e.g., `{ status: "In progress" }`.
        - Additional metadata, such as `{ timeToComplete: 3000 }` (time remaining in milliseconds), may also be included.
    - **Links**:
        - Hypermedia links (HATEOAS) may guide the client to perform further actions, e.g., a `cancel` link:           
            `{   "rel": "cancel",   "method": "DELETE",   "href": "/api/status/12345" }`


###### 4. Error handling


#### Types of Endpoints

> Use **entity endpoints** for specific resources, **domain endpoints** for logical grouping, and **resource endpoints** for consistent RESTful CRUD operations.

1. **Entity/Resource-Based Endpoints**:
    
    - Focus: Represent individual resources or collections (e.g., user, order, product).
    - Use **nouns**, not verbs, to describe resources.
    - Actions defined by HTTP methods:
        - `GET /users`: Get all users.
        - `POST /users`: Create a new user.
        - `GET /users/{id}`: Get details of a specific user.
        - `PUT /users/{id}`: Update a user.
        - `DELETE /users/{id}`: Delete a user.
2. **Domain-Based Endpoints**:
    
    - Focus: Organize resources logically under business domains.
    - Groups related entities for better readability.
    - Example:
        - `GET /users/{userId}/orders`: Fetch orders for a user.
        - `POST /users/{userId}/orders`: Create an order for a user.
        - `GET /orders/{orderId}/items`: Get items of an order.
3. **Resource-Based Endpoints**:
    
    - Focus: Follow REST principles, treating everything as a resource.
    - Use URIs to identify resources (`/resource/{id}`) with CRUD actions:
        - `GET /products`: List all products.
        - `POST /products`: Create a new product.
        - `GET /products/{id}`: Get details of a product.
        - `PUT /products/{id}`: Update a product.
        - `DELETE /products/{id}`: Delete a product.