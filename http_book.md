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
  - Default port for HTTP: `80`
  - Default port for HTTPS: `443`
5. path - (optional) local resourece being requested e.g `home`
  - Sometimes path can point to a specific resource on the host like an html file 
6. query string - (optional) made up of query parameters to send data to the server e.g `?item=book`

### Query strings ###
Give: `http://www.example.com?search=ruby+on+rails&results=10`
- `?` - Reserved character that marks start of query string
- `search=javascript` - This is a paramater name/value pair
- `&` - Reserved character for adding more parameters to query string
- `+` - Represents a space character within a query string
  - Can also use `%20` 

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

## Processing Requests ##
### Status code ###
Three digit number that the server sends back after recieivng a request signifiying the status of the request
- `status text` id splayed next to the `status code` and provides a description of the code
- Can be found in the "Status" section of the chrome inspector

### Common Codes ###
- `200` --> `OK` - The request was handled successfully
- `302` --> `Found` - The requested resource has changed temporarily. Usually results in a redirect
- `404` --> `Not Found` - The requested resource cannot be found
- `500` --> `Internal Server Error` - The server has encountered a generic error

**These are codes that should probably be memorized**

### 302 Found ###
A resource has been moved, and the request is rerouted to a new URL
- This is called a `redirect`
- the browser _knows_ the resource has been moved, and will automatically follow the new re-routed URL in the "Location" response header
- Response may also contain a `return_to` parameter in its "Location" if you are being redirected to fill out information,, and intended to be redirected back after

ex) You try to access a webpage that requires the user to be logged in --> reponse `Location` will contain a URL to the sign in page and the browser will automatically issue a `GET` request for that page

### 404 Not Found ###
This is returned as a response when the server cannot locate the resource the is being requested

### 500 Internal Server Error ###
This is returned when something went wrong on the server side of things
- This is generic and could mean a wide range of things happened
- This is something that needs to be deugged by those who have access to the server

### Respone Headers ###
Offer additional information about the resource being sent back. The most common are:
- `Conent-Encoding` - The type of encoding used on the data e.g `Content-Encoding: gzip`
- `Server` - Name of the server e.g `Server:thin 1.5.0 codename Knife
- `Location` - Notify client of new resouce location e.g `Location:https://www.github.com/login`
- `Conent-Type` - The type of data the resonse contains e.g `ContentType:text/html; charset=UTF-8`

Once again, do not need to memorize them

## Stateful web apps ##
HTTP is a stateless protocol. The server does not hang onto state between requests/reponse cycles
- Each request/response is not aware of any others made
- This can make it difficult for developers to build stateful web applications

### A stateful app ###
Developers are able to use different tricks to give the appearance of a stateful app e.g You log into a website and stay logged in 
- Server can send a unique token to the client
  - The client can then append this token to all of its requests to get a modified response (i.e a logged in web page)
  - This token is called the **Session Identifier** (Session ID)
  - We can then display a faux persistant view in the client between page refreshes

**Downsides**
Server must work very hard to simulate a stateful experience. On every request, the server must:
- Inspect the request to see if it contains a session ID
- Server must check that the session ID is still valid
  - Server needs rules for Session ID expiration
  - Server needs a way to store session data
  - Server needs to retrieve session data based on ID
  - Needs to recreate the state (e.g build the HTML) from session data and sent it back to client

### Cookies ###
A common way to store session information in the client. Most browsers have cookies enabled by default
- A piece of data that's sent from the server and stored in the client during request/response cycle
  - Small file that contains session information (including, but nost limited to Session ID)
  - Chrome inspector groups cookies by domain name
- Does not contain session data, that is stored on the server. Cookie only stores info to identify the session
- `set-cookie` - Reponse header that adds cookie data to response for client to store
- `cookie` - Request header that uses cookie to identify the session
- Can also find a `Cookies` dropdown menu on the left side of the Inspector in chrome under `Applications`
- Sessions typically expire in a relatively short period of time
  - Certain actions will also deliberatly delete the session ID from the cookie (like logging out of a website)

### AJAX ###
Asynchronous JavaScript and XML
- Allows browsers to issue requests and process reponses without a full page refresh
- This is a lot less expensive as HTML does not need to be regnerated upon every HTTP request
- Requests are sent as normal, reponses are sent as normal, but each response is processed by a server side callback function
  - The callback is triggered when the response is returned, and is responsible for updating the HTML

## Security ##
Since HTTP is non-encrypted, and requests and responses are sent as plain text, there are security concerns
- A hacker could try to steal session information from a cookie and log into your account, for example

### HTTPS ###
A HTTP protocl where all request/responses are encrypted before being transported on the network
- Sends messages through a cryptographic protocol called TLS
- Ealier versions of HTTPS used SSL (Secure Scockets Layer)
- Use certificates to communicate with servers and exchange security keys before encryption

### Same-origin Policy ###
Retricts certain interactions between reources originating from different origins
- Origin is combination of scheme, host and port
- So `https://example.com` and `https://example.com/home` would be same origin
- But `https://example.com` and `http://example.com` would not be
- Means that our client cannot make requests to URLs that have different schemes, ports, or hosts from the one we are accessing currently
  - Things like redirects, links, form submissions etc are usually allowed

### Session hijacking ###
As we've seen, session IDs are typically stored as normal strings stored inside a cookie on the client side, sent to the server with eqch request
- If a hacker intercepts one of these requests, they could access a session without us knowing
- This could allow them access to our account login without needing to know our information

**Countermeausres**
- Ask for reauthentication when accessing sensitive info, like charging a credit card, resetting password etc.
  - This will reset the session, and typically ask the user to enter their password again when trying to access this info
- Expiration time on sessions - This limits the time the hacker will have before the session is reset
- Use HTTPS - This encrypts the requests/responses, and minimizes the changes a hacker can get the session ID

### Cross-site scripting XSS ###
This happens when you allow users to input HTML or JS that ends up being dipslayed by the site directly
- An example is submitting text using a `<textarea>` in HTML, thus sending it to the server
- If this is for a comment on the page, for example, the updated HTML for the page will contain it
- If the server does not do some type of sanitation, the code will be injected into the page contents and be served to the client

**Countermeasures**
- Sanitize user input e.g get rid of all script tags, or disallow HTML and JS input altogether
- Escape all user input data before displaying it (display code as plain text instead)
  - e.g `<p>Hello World!<\p>` --> `&lt;p&gt;Hello World!&lt;\p&gt;`
