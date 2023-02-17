# Note before going on... #
Remember that all are the protocals are enforced by **computers** e.g general computer, server, router, switch etc.
- Example, the ethernet protocal is just a set of rules about how computers can format and transfer data over ethernet
- example, the internet protocal is just a set of rules about how computers and routers can format and transfer data over the internet
- **ALL OF THE HEADERS ARE ADDED BY THE HOST OR SERVER AND REMOVED ALONG THE WAY ONCE THEY ARE NO LONGER USEFUL**

## Physical Network ##
The functionality at this layer is concerned with the transfer of bits (binary)
- Bits are convetered into digital signals, radio waves, or light
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
- Takes unstructured bits from phsycial layer and organizes them into a meta-data (header and trailer) and data payload
  - Figures this out based on the set size of each field in bits and the order within the frame
- Encapsulates data from the Intenet/Network layer above
### Fields ###
- Header contains certain things like:
  - Source and destination MAC address
  - Length of payload
  - Network protocal used for data payload
- trailer contains:
  - Checksum to see if received frame has all data of sent frame, if not, frame is dropped
- Payload contains:
  - The entire data protocal from the layer above

### Mac addresses ###
Every network-enabled device (has Network Inteface card inside it) is assigned a MAC address when it is manufactured
- These are almost always unique
- Referred to sometimes as _physical address_ or _burned-in address_
- Formatted as six two-digit hexadecimal numbers e.g `00:40:96:9d:68:0a`
- **These addresses are used by the data link protocals to determine where data was sent from and where it was received**

**Switch vs Hub (using MAC addresses)**
- A hub will send a frame to all devices on the network, and the recieving device will check the MAC and drop the frame if it was not intended for them
- A switch will only send the the frame to the relevant device based on the MAC address
  - It does this by keeping a record of all MAC addresses connected to the switch and which ethernet port they are connected (called MAC address table)

**Scale Issue**
The above system works well for local networks, but is not suitable for inter-network connectivity
- MAC addresses are usually static (they do not change once assigned by a manufacturer)
- They are non-hierarchical, meaning there is no structure to them (no indiication of GEO or orangizational location)
- If we wanted to apply this strategy to inter-connected networks, we would need routers that stored MAC tables similar to switches
  - These tables would be gigantic as the static, non-hierarchical nature of the MAC address means every single one that connected to a router would need to be stored

## Internet / Network Layer ##
Protocals at this layer faciliate communication between hosts (computers) on different networks
- The internet protocal (IP) is the predominant protocal used at this layer
- Currently two versions of IP --> IPv4 and IPv6
  - We'll mostly be looking at IPv4, but both versions have the same primary features
- Features: 
  - Routing capability via IP addressing
  - Encapsulation of data into packets (PDU for IP)
### Data packets ### 
This is the PDU for the internet protocal
- Comprised of a header + data payload
- The payload is comprised of the PDU from the layer above (transport layer)
  - This means it will usually be either a TCP segment (the PDU for TCP) or UDP datagram (PDU for UDP)
- Header has seperate fields for meta data
- All data in the packet (header + payload) is in bits
  - Figures out what goes where based on the set size of each field in bits and the order within the frame
### Fields ###
Header contains:
- Version of IP: (IPv4 vs IPv6)
- Fields related to fragmentation: allowing PDU's from the transfer layer to be broken into multiple packets and reassembled later
- TTL (time-to-live): the number of hops a packet can take before being dropped (so it doesn't bounce around forever if it doesn't reach its destination)
- Protocol: indicates if the payload is TCP or UDP
- Checksum: Algo that generates a value for total data in the packet. Destination device uses same algo to figure out what data it is supposed to recieve
  - If the values don't match, the destination device drops the packet
- Source address: 32 bit IP-address of the sender
- Destination address: 32 bit IP-address of the intended recipient
### IP addresses (IPv4) ###
These are logical, hierachical addresses assigned dynamically to devices as they join a network
- They must fall within a range of addresses available to the network
  - The range is defined by a network hierarchy, where an overall network is divided into subnets, each with their own range (that falls within the overall range)
  - Adressea are 32 bits in length, divided into four sections of 8 bits each
    - When converted to decimals, each of those sections provides a number bween 0-255
- Within the IP range, the overall **Network address* starts the range, and the **Broadcast address** ends it
  - This range can be used to identify packets sent to devices sent on the network or a subnet of the network
 
 ![image](https://user-images.githubusercontent.com/93304067/219284582-fa49432f-405b-403b-9b68-68e979122607.png)

### Routing ###
All routes on a network store a local routing table
- When a packet is recieved by a router, the router looks at the IP address and matches it against a list of network addresses in the table  
  - Remember, the network address is the start of the address range for the network
- The packet will be sent to the matching network using the least "expensive" route
- Kind of like the MAC table for a switch, except instead of having a list of addresses of devices, its a list of addresses of networks

### Limitations of IP ###
It is great to get data from one device to another device on another network, but what do we do once the data gets there?
- The data could be for a videogame, a streaming service, a website eticc..
- This is where the tranport layer comes in
