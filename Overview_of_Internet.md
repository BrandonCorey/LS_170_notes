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
1. **Application Layer**: the layer that interacts with the user or application
2. **Presentation Layer**: responsible for transforming data into a format that the application layer can use
3. **Session Layer**: responsible for establishing, managing, and terminating connections between applications
4. **Transport Layer**: responsible for reliable end-to-end delivery of data
5. **Network Layer**: responsible for routing packets between different networks
6. **Data Link Layer**: responsible for organizing bits into frames, adding error detection and correction
7. **Physical Layer**: responsible for transmitting bits over a communication channel

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
