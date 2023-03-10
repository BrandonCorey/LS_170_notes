# Transport Layer #
This layer relates to how we handle data once it has been recieved by a host or a device
- IP gets us to the device, but the transfer protocals help us relay data to the correct application

# Communication between Processes #
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
- **In code, sockets are represented as objects and instantiated to create connections between applications**
### Keep in mind ###
- There are also UNIX sockets used for communication between local processes, but not important for this course
- There is also a distinction between the concept of a socket and its code implemtation
- This course primarily focuses on the _concept_ of a socket and the application of this concept for inter-network communication between networked applicaiton

### Sockets and connections ###
There are two types of connections that can be facilitated with sockets:

**Connectionless:**
- Could have one socket object defined by IP address of host + port assigned to process on machine listening for all incoming messages directed at its IP/port pair
- Could come from any source at any time, in any order
- This implementation allows sockets to process the incoming messages as they arrive and sends responses as necessary

![image](https://user-images.githubusercontent.com/93304067/219549848-bbf50f65-dee3-4392-9ce7-49466d485ba1.png)

**Connection-oriented:**
- Could have one socket object listening a port similar to connection, but when the object recieves a message, it instantiates a new socket object
- This new instance will contain info about its own socket and the source socket as well
- These new instances will only listen for for messages where soure port, soure IP, destination port, and destination IP match up
  - These four pieces of info above are called a four-tuple
- Any messages not matching the four-tuple would be picked up by the original socket, which then instantiates a new socket object
- This implementation essentially allows for a dedicated connection for communication between specific processes running on different hosts

![image](https://user-images.githubusercontent.com/93304067/219549867-25f3f7d6-6cf9-45a9-86dd-ae20a248ad57.png)

# Network Reliability #
A major issue with netowrks is that lower level protocals do not provide reliability
- Data-link and IP both drop corrupt data after performing a check sum
- But they do not retransmit that lsot data

## Designing a reliable protocal ##
1. Sender sends a message with ID number and sets a timeout
2. If message recieved, recipient sends acknowledgment with corresponding ID number to indicate which message was recieved
3. When acknowledgement is recieved, sender sends another message
4. If acknowledge is not recieved for a message before timeout, sender sends another message of the same ID
5. If recipient recieves a duplicate message (based on ID), it assumes sender never recieved acknowledge, and resends it for that message ID

###  Benefits of the above protocol ###
- In-order delivery: data is received in the order that it was sent
- Error detection: corrupt data is identified using a checksum
- Handling data loss: missing data is retransmitted based on acknowledgements and timeouts
- Handling duplication: duplicate data is eliminated through the use of sequence numbers

### Making above protocol more efficient with pipelining ###
Instead of sending one message at a time and waiting for acknowledgement....
- We can send a set "window" for maximum number of messages allowed in the pipeline at once
- Once a message is ackowledged, we have room for another message in the pipeline and can immediately send
  - If a message is never recieved or acknowledged, the rules from before still hold
  - A new message is not sent until messages are acknowledged in order

**This is called pipelining**
- Its essentialy what the TCP protocol does
- Here's a nice visualization: http://www.ccs-labs.org/teaching/rn/animations/gbn_sr/

![image](https://user-images.githubusercontent.com/93304067/219556056-0c4b516b-80b9-4cbe-a37f-20a8675d9606.png)

## Transmission Control prototcol (TCP) ##
A **connection oriented** protocol of the transfer layer that provides reliable data transfer
- The TCP recovers from data this damaged, lost, duplicated, or delivered out of order
  - sequence numbers allow recipient to store out of order messages in a buffer until the missing messages are retransmitted
- Provides reliable network communication on top of unreliable lower layers
- Hides the complexity of reliable network communication from the applicaion layer (the layer above transfer)
- Also provides encapsulation and multiplexing through use of TCP segments (the PDU)
  - The _multiplexing_ is provided by the header fields _source port_ and _destination port_ as they allow different data for different services to be streamed through one channel and then demultiplexed when needed to arrive at the correct socket 


### TCP segments ###
This is the PDU for the TCP protocol
- Has a header and a data payload
- Header contains:
  - **Source Port** and **Destination port**, along with other fields
  - Checksum - provides error detection for corrupted data
  - Sequence number & acknowledgement number - two fields used for in-order delivery, data loss, and duplication detection
  - Window size - maximum number of messages allowed in the pipeline at a given time. This determined dynamically based on how much space is available in the recievers buffer
  - flags - One bit boolean fieds --> SYN, ACK, FIN, RST are important ones

### TCP connections ###
**TCP is a connection-oriented protocol**. It doesn't start sending application data until a dedicated connection has been established between application processes
- Uses three-way handshake to establish a connection


**Note that below is the OVERHEAD OF ESTABLISHING A CONNECTION, TCP also uses acknowledgements during the data transfer process after the connection is established**
### Three way handshake ###
SYN (Synchronize): 
- The initiating device sends a SYN message to the receiving device to start the connection. This message contains a sequence number, which is used to keep track of the data sent and received. (Header "SYN" field is set to 1)

SYN-ACK (Synchronize-Acknowledge):
- The receiving device responds with a SYN-ACK message, which acknowledges the receipt of the SYN message and sends its own SYN message back to the initiating device. This message also contains a sequence number. (Header SYN and ACK fields set to 1)

ACK (Acknowledge):
- The initiating device responds with an ACK message, which acknowledges the receipt of the SYN-ACK message. This message also contains a sequence number, and the connection is now established and data can be exchanged between the two devices. (ACK field set to 1)

![image](https://user-images.githubusercontent.com/93304067/219788088-4479c466-bd6c-4ca1-83bb-9cda9f9bd877.png)
![image](https://user-images.githubusercontent.com/93304067/219788189-f13b5b1a-3bed-4b11-88b9-cd61888aefaa.png)

### Flow control ###
A mechanism to prevent the sender overwhelming the reciever with too much data at once
- Data awaiting processing is stored in a buffer (the size of the buffer is depenedent on the amount of memory allocated by the OS)
- Each side the connection can let the other know how much data it is willing to accept at once with the "window" field in the TCP header
- This field can be changed through the duration of the connection depending on how full the recievers buffer is getting

### Congestion Avoidance ###
Similar to flow control, however this mechanism is concerned with not overwhelmng the network instead of the reciever
- Routers also have a buffer as they need to process IP packets (run checksum, route based on sourec/destination IP)
- If the buffer overflows, the packets are dropped, which will most likely cause TCP to retransmits the data
- If the lots of data continues to get lost TCP, takes this as a sign of network congestion, and shrinks the size of the transmission window

### Important Note on flow control vs congestion avoidance ###
Flow control has to do with controlling traffic for application communication, congestion avoidance has to do with controlling traffic for network communication

## Downsides of TCP ##
There is increased latency assoicated with TCP for reasons like:
- Three way handshake as it requires multiple acknowledgements before the connection can be established
- Head-of-line blocking --> Because data needs to be delivered in order, if one piece needs to be retransmitted, the pieces that come after need to be buffered until retransmission has completed

## UDP ##
User Datagram protocol (UDP) is a **connectionless** protocol of the transport layer that provides data transfer
- PDU is called a datagram
- Encapsulates data frrom the application layer and adds header info
- Header only includes **source port, destination port, length of datagram (in bits), and checksum**
  - Checksum is optional when used with IPv4 since it also has a checksum, but mandatory with IPv6 as it doesn't
- UDP also provides multiplexing as it stores port information that can be used to de-multiplex the data later on
- Does **NOT** resolve network unreliability of layers below it
  - No guarentee of delivery
  - No guarentee of correct delivery order
  - No congestion avoidance or flow control mechanism
  - No connection state tracking mechanism (since it is connectionless protocol)

### Benefits of UDP ###
Although less reliable, the simplicity allows for certain advantages over TCP
- Speed: 
  - UDP is connectionless and does not need to wait for a connection to be established process before sending data
  - The actual data transfer is also faster as there are no acknowledgements, retransmissions, or necessities for in order delivery, reduces queue latency
- More flexibility:
  - UDP offers fast, barebones data transfer to an application process that can be built upon in the above application layer
  - This means a developer may implement something like in order delivery, but decide not to implement acknowledges as occasional lost data may not me important
  - **NOTE: There are best practices to adhere to when using UDP, like implementing your own form of congestion avoidance**

A videogame or a video call are examples of applications that would benefit from using UDP as speed is more important than perfect data
  - Applications that suffer greatly from increased latency may benefit from utilizing UDP at the transfer layer
