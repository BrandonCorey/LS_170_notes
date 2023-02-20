# Application Layer #
The topmost layer with protocols that provide communication services to applications
- These are the protocols that the application most directly interacts with (HTTP, SMTP, FP etc...)
- The application can also interact with transfer layer protocols (e.g opening a TCP socket), but almost never with any lower layers
- Application protocols are more concerned with the syntax and structure of a message, whereas the lower layers focus on delivering that message
- The primary protocol for communication on the web is HTTP

### Web vs Internet ###
- Internet - A global network of networks that serves as the infrastructure to enable inter-network communication between devices
- Web - A service that can be accessed via the internet consisting of a vast system of resources, accessible using a URL (Uniform Resource Locator)
  - Think of as a layer on top of the internet comprising of web pages, web servers, web clients (browsers) that communicate using the HTTP protocol

Note: The original vision of the web was more akin to something like wikipedia, inter-connected resources linking to other resources
## Definitions ##
- hyper-text - text displayed on a computer display or other electronic devices with references (hyperlinks) to other text that the reader can immediately access
- HTML (hypertext markup language) - Means by which resources in the system (of the web) should be uniformly structured
- Uniform Resource Identifier (URI) - a string of characters which identifies a particular resource (this is different from a URL)
- HTTP (hypertext transfer protocol) - set of rules which provide uniformity to the way resources on the web are transferred

## Client-server ##

![image](https://user-images.githubusercontent.com/93304067/220180778-aea74b79-9563-4966-9762-e2f36672b6c5.png)

When we zoom in on the server, there are two main parts:
- web server
- appliaction server

### web server ###
A server that typically responds to requests for static assets (files, images, css, javascripts) etc.

### application server ###
A server that contains the application or business logic, where most of the complicated HTTP requests are handled
- This is where server-side code lives when it is deployed
- Will often consult with a peristent _data store_ (relational datbase, key/value store, document store etc.)
  - Allows for data to persist between stateless request/response cycles

## HTTP over TCP/IP ###
![image](https://user-images.githubusercontent.com/93304067/220181234-5eaa1e2e-f889-40f7-88c6-b9c079c18b2f.png)

Remember that HTTP requests are sent using other protocls (typically TCP + IP) to get to the correct client
- HTTP operates at the application layer and is concerned with the structure of the messages
- It does not handle the transporation of the message, that is where the network and transport layers come in and their protocols
