- HTTP is a text-based protocol. HTTP Request and Responses involve sending text between the client and server

- In order for the protocol to work, the Request and Response must be structured in such a way that both the client and the server can understand them.

- With HTTP/1.1, the end of the a request is indicated by an empty line.
  - The end of the message is determined by either using `Content-length` or a process known as transfer encoding

- In HTTP/1.0, the end of the messsage is indicated by a `Content-Length` header (length of message in bytes)

- The Content-Length header can be used to indicate the size of the body. This can help determine where the HTTP message should end.
