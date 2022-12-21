
# **What is OSI:**

OSI stands for Open Source interconnection and it's a conceptual framework for how applications communicate over network and the model is made of 7 layers


## Layers of the OSI Model 


| Layer | Description   | Protocols |
|-----------------------------|--|--|
|7. Application |  End User Layer   | HTTP,FTP,IRC,SSH,DNS| 
|6. Presentation |Syntax Layer| SSL,SSH,IMAP,FTP,MPEG,JPEG |
|5. Session| Synch & Send to port| API's, Sockets, WinSock |
|4. Transport | End-toend-connections |TCP,UDP |
|3. Network|Packets|IP, ICMP, IPSec, IGMP|
|2. Datalink|Frames| Ethernet, PPP, Switch, Bridge|
|1. Physical| Physical Structure| Coax, Fiber, Wireless, Hubs, Repeaters|



### Application 

- This is the layer that most of end users interact with 
- Application layer provides network services to the end user. 
- Services are protocols that work with the data the client 
- Protocols such as: 
1. HTTP that is used by web browsers such as google chrome
2. File Transfer Protocol (FTP)
3.  Post Office Protocol (POP)
4. Simple Mail Transfer Protocol (SMTP)
5. Domain Name System (DNS)



### Presentation
- The presentation layer prepares data for the application layer
- syntax processing or converting data from one format to another 
- defines how two devices should encode, encrypt, and compress data
- The presentation layer takes any data transmitted by the application layer and prepares it for transmission over the session layer.


### Session Layer

- The session layer creates communication channels called sessions
-  It is responsible for opening sessions, ensuring they remain open and functional and closing them when communication ends
- set checkpoints during a data transfer if the session is interrupted, devices can resume data transfer from the last checkpoint


### Transport Layer ( Segments )

- Receives data from session layer and breaks it into **Segments** on the transmitting end
- reassembling the segments on the receiving end to data and send it to the session layer
- src and dest port number 
- Flow control
- Error control
- Connection control
- Checksum


**The two protocols used in this layer are:** 

-   **Transmission Control Protocol**
    -   It is a standard protocol that allows the systems to communicate over the internet.
    -   It establishes and maintains a connection between hosts.
    -   When data is sent over the TCP connection, then the TCP protocol divides the data into smaller units known as segments. Each segment travels over the internet using multiple routes, and they arrive in different orders at the destination. The transmission control protocol reorders the packets in the correct order at the receiving end.

-   **User Datagram Protocol**
    -   User Datagram Protocol is a transport layer protocol.
    -   It is an unreliable transport protocol as in this case receiver does not send any acknowledgment when the packet is received, the sender does not wait for any acknowledgment. Therefore, this makes a protocol unreliable.


### Network layer ( Packets )

- Breaking up segments into network packets, This process is known as Packetizing
- Reassembling the packets on the receiving end to segments and send it to Transport layer
- Responsible for routing and forwarding the packets.
- The protocols used to route the network traffic are known as Network layer protocols. Examples of protocols are IP and Ipv6.
- adds the source and destination address to the header of the frame. Addressing is used to identify the device on the internet.

### Data Link Layer ( Frame )

- breaks up packets into frames and sends them from source to destination
- error-free transfer of data frames
- Defines the format of the data on the network
- It is mainly responsible for the unique identification of each device that resides on a local network.
- **This layer is composed of two parts:**
	- **Logical Link Control (LLC):**
		- responsible for transferring the packets to the Network layer of the receiver that is receiving.
		- It identifies the address of the network layer protocol from the header.
		-   It also provides flow control.
	- **Media Access Control Layer:**
		-  A Media access control layer is a link between the Logical Link Control layer and the network's physical layer.
		-  It is used for transferring the packets over the network.


### Physical Layer ( Bits )

-   The main functionality of the physical layer is transmission of the raw data from one node to another node which is simply a series of 0s and 1s.
-   It is the lowest layer of the OSI model.
-   It establishes, maintains and deactivates the physical connection.
-   It specifies the mechanical, electrical and procedural network interface specifications.



-   **Line Configuration:** It defines the way how two or more devices can be connected physically.
-   **Data Transmission:** It defines the transmission mode whether it is simplex, half-duplex or full-duplex mode between the two devices on the network.
-   **Topology:** It defines the way how network devices are arranged.
-   **Signals:** It determines the type of the signal used for transmitting the information.