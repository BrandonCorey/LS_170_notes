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
