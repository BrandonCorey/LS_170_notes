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

# HTTP #
## Making requests ##
- Can view HTTP requests to server in your browser using chrome dev tools
  - Open inspector --> Network
  - If you click on a reponse, you can view the headers, a preview, the raw response, cookies, and timing
- Browsers will automatically make HTTP requests to resources referenced any other requests
  - HTTP tools or the command line `curl` command will not make these additional requests

To make a request from the command line:
```
curl -X <method> <url> -m 30 -v
```
 - `-X` - Used to denote which HTTP method to use
 - `-m` - How long should we wait for response (max time)
 - `-v` - verbose, give HTTP details (not just raw response)

## Request Methods ##
Two most common HTTP request methods are `GET` and `POST`
- `GET` - Used to retrieve information from the server (most common)
- `POST` - Used to send information to the server

Can view these methods and status of the requests under the "Network" section on the inspector as well

### GET requests ###
These are intiated by either clicking or link or entering something in the address bar via the browser
- Used to retrieve information from the server (most common)
- Resposnse can be anything, but if its HTML, your browser will automatically issue `GET` requests to any other resources references in the HTML
- These means entering a URL in an address bar initiates a `GET` request
  - This is the default behavior of a link as well (issue a `GET` request to the URL)

Can make request using this in the terminal: `curl -X GET <URL> -m 30 -v`

### POST Requests ###
Used when we want to initiate some action on the server or send data to the server
- `POST` is typically used when submitting a form within a browser
  - Form elements in the HTML can be referenced with their name within a `POST` request
- Allows us to send much larger and sensitive data to the sever than with query strings
- The data sent in the `POST` request is contained within the `HTTP` body
  - The body of the HTTP message is optional
  - Can contain HTML, images, videos etc..
- Reponses to `POST` requests may contain links to other webpages in the reponse header "Location", which your browser automatically issues a `GET` request for

Exampole of `POST` request: `curl -X POST "http://al-blackjack.herokuapp.com/new_player" -d "player_name=Albert" -m 30 -v`

### HTTP Headers ###
Allow the client and the server to send additional information during the HTTP request/response cycle
- Colon seperated name-value pairs sent in plain text
- Can see them using Inspect --> Network --> Headers
- Reqeusts and responses have different sets of headers available to them

**Important Request headers**
- `Host`- domain name of the server e.g `Host:www.google.com`
- `Accept-Language` - List of acceptable languages e.g `Accept-Language:en-US,en;q=0.8
- `User-Agent` - A string that identifies the client e.g `User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/38.0.2125.101 Safari/537.36
- `Connection` - Type of the connection the client would prefer e.g `Connection:keep-alive`

Don't bother memorizing the headers

