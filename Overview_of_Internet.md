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
