## Past present and future ##
### HTTP ###
- 0.9:
  - HTTP requests were a single line, without headers or a body. Also, `GET` was the only method and the `path` was to a resource of the server
- 1.0:
  - Methods were added, such as `POST` and `HEAD`
  - Request lines became more robust, `path` could not be an absolute URI instead of jsut a local file path on the server
  - Responses included a status line (code + message)
  - Headers were introduced (introduced meta data)
    - `Content-Type` was an important addition, as the content other than HTML was becomming commonplace on the internet

- 1.1:
  - This became the first official standard version of HTTP
  - Improved performance dramatically (introduced `keep-alive` connections) so that 3 way handshakes did not have to be reinitiated after each request
    - Persistent connections made it possible to "pipe-line" HTTP messages in the transport layer
    - This is because previously, a single HTTP request would be sent over a single TCP connection
  - Also introduced cache control mechanisms for the server
  - Also added more methods --> `PUT`, `DELETE`, `TRACE`, and `OPTIONS`
- 2.0:
  - Improved performance again through the use of multiplexing
  - 1.1 used pipelining for HTTP requests, but 2.0 introduced multiplexing of TCP requests
  - This means the messages are sent in parallel, instead of having to be sent in order (gets rid of HOL latency)
  - Introduced compression of headers (which typically make up a large part of the request or response)
- 3.0
  - In the works as we speak
  - Aims to eliminate the added layers needed to send secure messages (TCP + TLS)
  - Will introduce a new protocol called QUIC that will aim to replace TCP and TLS

## Web performance and HTTP optimizations ##
### Browser optimizations ###
There are two main types of browser optimizations, Document-aware optimizations and speculative optimizations

**Document-aware optimizations**
Occurs when the browser can identify and prioritize fetching resources
- Can load a page more efficiently by prioritizing expensive resources (such as CSS, and JS)

**Speculative optimizations**
Browser learns navigation patterns of user over time and attempts to predict user actions
- Can involve pre-resolving DNS names or pre-rendering web pages completely of frequently visited sites
- Can also involved opening a TCP connection in anticipation of an HTTP request when a user hovers over a link

### Latency Reudction for developers ###
- Only include necessary resources within the relative context of the application
  - e.g do not load all files for all web pages if each page only uses a few
- Compression
  - Use a tool to compress the file size of your application depencies or resources
  - Can also use tools to **minify** files, which involved things like removing whitespace, formatting, shorter var names, no comments etc
- Reusing TCP connections
  - Since this behavior is supported using HTTP/1.1, and retoactively supported in HTTP/1.0, take advantage of it
  - Do not close a TCP connection after a single request if it is expected that many requests will be made to a socket
- Dns optimizations
  - Reduce number of hsotnames that need to be resolved (eliminate unecessary ones for a certain page view)
  - Download any external resources and host them locally instead
  - Use a faster DNS provider like GoDaddy
 - Caching
   - Server implements a cache into its infrastrcture that acts as a short term memory bank
   - This allows it to be served directly from the cache instead of needing to be processed by the application server (for dynamically generated content)

## Browser Networking APIs ##
APIs have been developed to overcome the shortcomings of HTTP in regards to real time synchronization
### XHR ###
XMLHttpRequest
- XML is a markup language that defines a set of rules for encoding documents
- XHR enables clients to manage requests programatically and asyncrhonously
- XHR is a key component of AJAX (Asynchronous JavaScript and XML)
  - Means we could have a JS program running in th background sending periodic `GET` requests to a server (called _polling_)
  - Can also use JS to take the updated data from the server and periodically update elements of the DOM with the new data

Downside is the checks are at set intervals or conditions and may be unecessary a majority of the time

## SSE (Server-sent Events) ##
Enables server-to-client real time streaming of text based event data
- This allows the server to deliver up to date into to the client without the client having to request the data
- Does this through tthe use of a long-lived TCP connection that stays open even after the initial data is served to the client

Downsides:
- Only works in a client-server model
- Does not allow client to request anything after the initial connection is established
- Can only support UTF-8 text based data, not binary

## WebSocket ##
Allows a client and a server to establish a persistent TCP connection that allows either side to independently send messgaes to each other
- Birdirectional communication that **does not require requests or responses from either side**
- Messages can consist of text or application code
- Is an unstructured, low latency, non-parsed, non-buffered connection

Downsides:
- Implementations for data management must be developed, as it is not built in like HTTP
- Might also need to implement transport layer best practices like flow control and congestion avoidance
- Might also need to implement some type of security similar to TLS
