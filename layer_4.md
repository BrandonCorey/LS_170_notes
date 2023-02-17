# Transport Layer #
This layer relates to how we handle data once it has been recieved by a host or a device
- IP gets us to the device, but the transfer protocals help us relay data to the correct application

## Multiplexing and Demultiplexing ##
A device may have many networked applications running on a single machine, acting as distinct communication channels
- How do we get the data from these channels to our target devices?

### Multiplexing ###
This is the combining of multiple streams of data into a single transmission channel
- Can think of multiple light signals in a fiber-optic cable, or multiple frequencies in a radio wave transmitting data
- We need to do this to this for often for our IP host-to-host connections
- An example would be taking your spotify data, email data, browser data, and slack data, and combining them into a single stream to send to another network

### Demultiplexing ###
This is the seperation of a single data stream back into their individual data channels
- Once all the data mentioned above gets to the correct netowrk or device, it needs to be demultiplexed to arrive at the correct host or application
- The identification of the relevant applications is done using **ports**

![image](https://user-images.githubusercontent.com/93304067/219530722-2158b382-fef2-4cbd-ae6f-f516ffb59318.png)

## Ports ##
A port is an identifier for a specific process running on a host
- Can have an interger range of 0-65335
- 0-1023 --> well known ports (http: 80, FTP: 20-21, SMTP: 25...)
- 1024-49151 --> registered ports (by private entities like Mircosoft, IBM etc...), sometimes used for _ephemeral_ ports
- 49152-65535 --> dynamic ports/private ports, cannot be registered for a specific use. Can be customized for services or allocated as _ephemeral_ ports

**Servers** will likely have a port in the well known range assigned to them. If you're connecting via HTTP, the server will be listening on port 80
**ephemeral port** - A temporary port used for client services (like a browser) that is assigned to it by the operating system for duration of the session

## PDU for transport layer ##
Consists of two things:
- Payload consisting of encapsulated application layer data
- Header with fields for **Source Port** and **Destination Port**
- This entire PDU is the used as the data payload for the IP packet
- The IP addresses in the packet header is used to get data from one host to another, and the port numbers are used to direct the data to specific processes on a host

The combination of an IP address and a port number is referred to as a _**socket**_ e.g `216.3.128.12:8080`

![image](https://user-images.githubusercontent.com/93304067/219535108-b63a2ea9-4dfc-4b0a-ab59-26d17cb0903a.png)

### netstat ###
Can use the below command to look at active network connections (will list sockets as addresses)
```bash
sudo netstat -tunap
```
## Sockets ##
An abstraction for an endpoint used for inter-process communication
- Represented by an IP address concatenated to a port number
- A common internet socket is TCP/IP, used for inter-process communication between networked processes (usually on different machines)
  - Tecnically, you could run a server on your `localhost` and access it via a browser on the same device. These would still use internet sockets to communicate
- In code, sockets are instantiated as objects to create connections between applications
### Keep in mind ###
- There are also UNIX sockets used for communication between local processes, but not important for this course
- There is also a distinction between the concept of a socket and its code implemtation
- This course primarily focuses on the _concept_ of a socket and the application of this concept for inter-network communication between networked applicaiton

### Sockets and connections ###
There are two types of connections that can be facilitated with sockets:

**Connectionless:**
- Could have one socket object defined by IP address of host + port assigned to process on machine listening for all incoming messages directed at its IP/port pair
- Could come from any source at any time, in any order
- Simply processes the incoming messages as they arrive and sends responses as necessary

![image](https://user-images.githubusercontent.com/93304067/219549848-bbf50f65-dee3-4392-9ce7-49466d485ba1.png)

**Connection-oriented:**
- Could have one socket object listening a port similar to connection, but when the object recieves a message, it instantiates a new socket object
- This new instance will contain info about its own socket and the source socket as well
- These new instances will only listen for for messages where soure port, soure IP, destination port, and destination IP match up
  - These four pieces of info above are called a four-tuple
- Any messages not matching the four-tuple would be picked up by the original socket, which then instantiates a new socket object

![image](https://user-images.githubusercontent.com/93304067/219549867-25f3f7d6-6cf9-45a9-86dd-ae20a248ad57.png)
