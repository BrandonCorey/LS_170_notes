## Networks ##
The simplest network is two devices connected in a way in which they can exchange data
- The simplest way to do this would be with a LAN cable

### Local Area Network (LAN) ###
This can be multiple computers conencted via network cables, and perhaps a network hub or switch
- The above "simple" network would also be considered local
- Home networks form a local network, but nowadays, typically the connection between the devices is faciliated over wi-fi (WLAN)
- These connects are limited to those connected to the local switch or hub, which imposes geographic restrictions for exchange of data

### Inter-connected Networks ###
To communicate between networks, we need a **router** 
- Routers are network devices that can route network traffic to other networks.
- For LAN, they act as gateways into and out of the network

## Internet ##
The internet is simply a huge network of networks
- There are systems of routers between each network that faciliate network traffic

![image](https://user-images.githubusercontent.com/93304067/218912926-192f8498-b04f-4474-8cbc-b316c0de78e6.png)

## Protocals ##
- Different protocols were developed to address different aspects of network communication.
  - e.g HTTP vs TCP
- Different protocols were developed to address the same aspect of network communication but differently for a specific use case.
  - e.g TCP vs UDP

## Internet layers ##
1. **Application Layer**: the layer that interacts with the user or application. Protocols at this layer include HTTP, FTP, and SMTP.
2. **Presentation Layer**: responsible for transforming data into a format that the application layer can use. Protocols at this layer include SSL and TLS.
3. **Session Layer**: responsible for establishing, managing, and terminating connections between applications. Protocols at this layer include NetBIOS and RPC.
4. **Transport Layer**: responsible for reliable end-to-end delivery of data. Protocols at this layer include TCP and UDP.
5. **Network Layer**: responsible for routing packets between different networks. Protocols at this layer include IP, ICMP, and ARP.
6. **Data Link Layer**: responsible for organizing bits into frames, adding error detection and correction, and managing access to the physical medium. Protocols at this layer include Ethernet, PPP, and ATM.
7. **Physical Layer**: responsible for transmitting bits over a communication channel. Protocols at this layer include Ethernet, Wi-Fi, and Bluetooth.

## PDU ##
an amount or block of data transferred over a network
- Different protocols or protocol layers refer to PDUs by different names
- At the Link/ Data Link layer, for example, a PDU is known as a frame
- At the Internet/ Network layer it is known as a packet
- At the Transport layer, it is known as a segment (TCP) or datagram (UDP)

![image](https://user-images.githubusercontent.com/93304067/218910668-2927f204-b8d8-4557-b395-34e97e76f1ac.png)

## Encapsulation ##
Hiding entirety of data in one layer (PDU) by encapsulating it within the data payload of the layer bellow
- This means the lower layer does not care about the PDU from the protocal before, and subsequently, what the protocal even was.

![image](https://user-images.githubusercontent.com/93304067/218909893-e6fe63e2-718f-449d-86f7-012d3dbf279c.png)

## Physical Network ##
The functionality at this layer is concerned with the transfer of bits (binary)
- Bits are convetered into digital signals
- The physical networkis the bottom layer of the OSI model

### Characteristics of a physical network ###
### Latency ### 
Time it takes for data to travel from one point to another (usually measured in ms). In is comprised of the different delays below:
- propogation latency - time for a message to travel from a sender to a reciever (speed / distance)
- transmition dleay - time it takes for data to be pushed from one link to the next (wires, switches, routers etc are considered links)
- processing delay - data must be processed before crossing from one link to another. This measures the delay time of the processing
- queuing delauy - network devices can only process so much data at a time. If this amount is exceeded, the data buffers. This delay measures that time
### Other Latency terms ###
- Last-mile-latency - This describes how a lot of the delays above take place near the end of the data transfer journey (between hititng the ISP network and your home)
- Round-trip time - A latency calculation use in networking to describe total latency + added length of time for acknowledgement or reponse to be recieved
- hop - When data travels from one router to the next
### Bandwith ###
amount of data that can be sent in a particular unit of time (usually measured in seconds)
- Bandwith varies through the journe of data transfer. The bandwith of the core network (internet infra) is going to be much higher than in your home network.
- Bandwith is generally measured using the lowest point within the entire connection
- The lowest point is called the bottleneck
- Low bandwith can be an issue when working with large amounts of data, but usually is less of a problem than latency for performance in a networked application

## Link/ Data Link layer ##
An interface between the workings of the physical network and the more logical layers above
- Protocals at this layer are concerned with identification of devices on the physical network and moving data between them e.g hosts (computers), switchers, routers
- Most common protocal at this layer is Ethernet
- The lowest layer at which encapsulation takes place

### Ethernet frames ###
This is the protocal data unit (PDU) for the ethernet protocal
- Takes unstructured bits from phsycial layer and organizes them into a meta-data (header and footer) and data payload
- They encapsulate data from the Intenet/Network layer above
- Header contains certain things like:
  - Source and destination MAC address
  - Length of payload
  - Network protocal used for data payload
- Footer contains:
  - Checksum to see if received frame has all data of sent frame, if not, frame is dropped
- Payloard contains:
  - The entire data protocal from the layer above
