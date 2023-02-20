## overview ##
- Browser - the interface or window that lets you interract with the world wide web
  - Has many files under the hood like Imgaes, videos, HTML, CSS, and JS
- Server sends files to the browser (the client) by an application protocl called HTTP
  - This is why all links start with `http://`

- HTTP - A system of rules that serve as a link between applications and the transfer of hypertext documents
  - Handles structure of the messages that a host sends another host
- Request-response protocol - client makes reuest to server using HTTP and waits for response

## Domain Name Systen (DNS) ##
- A distributed database that translates domain names like www.google.com to an IP address, which can then be used to make a request to the server
  - Makes accessing webpages easier (since you don't need to memorize each websites IP address)
  - The computers that store the DNS databases are called **DNS servers**
  - DNS servers are organized in a hierachical structure. If one server doesn't recognize a domain name, it routers the request to a server higher up
  - Devices may also store IP addresses in their DNS cache if you have already visited the site

## Client vs Server ##
- client - the most common client is a application called a _Web browser_
  - Responsible for issuing HTTP requests and processing HTTP responses in a user-friendly way to dispay of the screen of the device
- server - the conent being requested by the client is located on a remote computer called a _server_
  - These are typically just computers capable of handling inbound requests
  - Job is to send a response to inbound request

![image](https://user-images.githubusercontent.com/93304067/219985198-e1117794-9cdd-49c4-b4bc-e28e092c323f.png)

## Resources ##
General term for things you interract with on the internet via a URL
- images, videos, web pages etc...
- Can also include software that interracts web servers (stock trading app, videogame)

## Statelessness ##
A stateless protocol is one that is designed in a way where each request/response pair is independent
- HTTP is a stateless protocol
- The request/response pair is completely unaware of all others
- Is easier to use and consumes less resources as the server does not need to hang on to state between requests
  - If a requests breaks, the server does not have to do any cleanup

## URL ##
Uniform Resource Locator (URL) is an address that can be used to access resources on the web

Given: `http://www.example.com:88/home?item=book`
1. scheme - first part of the url, commonly `http`, `ftp`, `mailto`, or `git`. Sometimes used to indicate which protocol to use to access the resource
2. host - where the resource is hoisted e.g `www.example.com`
3. port - (optional) port number to be provided if you don't want to use the defualt one e.g `88`
4. path - (optional) local resourece being requested e.g `home`
  - Sometimes path can point to a specific resource on the host like an html file 
6. query string - (optional) made up of query parameters to send data to the server e.g `?item=book`

### Query strings ###
Give: `http://www.example.com?search=ruby&results=10`
- `?` - Reserved character that marks start of query string
- `search=javascript` - This is a paramater name/value pair
- `&` - Reserved character for adding more parameters to query string

- Because query strings can only be passed through the URL, they can only be used in HTTP GET requests
- The name/value pairs are visible in the URL, so sensitive info cannot be passed to server using them
- Reserved characters cannot be used in query strings. Neither can the space character

### URL encoding ###
Replaces reserved or unsafe ASCII characters with a `%` symbol followed by UTF-8 charcode
- `Space`	--> `%20`
- `$`	--> `%24`
- `£`	--> `%C2%A3`
- `€`	--> `%E2%82%AC`

Note: Safe characters include all alphanumeric + `$-_.+!'()",`

