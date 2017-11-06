# Assignment:
### 1.
The minimum seek time for an HDD is 9msec, and the maximum seek time is 90msec. The block size of this HDD is 4KB. How long on average does it take to read 100MB of data? 
There are MANY factors affecting read time. Platter spin speed, rotational latency, disk-to-buffer transfer rate, data fragmentation, number of platters, sector size, temperature, EMI, how fast a monarch butterfly is flapping it's wings during its yearly migration to south Florida... But with the given information, the formula for culculation  is 

(9 + 90) / 2 = 49ms avg. seek time
100 * 1024 = 102400KB of data
102400 / 4 = 25600 blocks in data
(25600 * 49) / 1000 = 1267.2 seconds

1267.2 seconds to read 100MB of data is almost an absolute worst case scenario with data that is 100% fragmented across the drive where the head would have to move across the entire platter and wait one full rotation to read each block of data, not really a real world scenario.  A more accurate average for a 7200 rpm HDD with a 150 MB/s read speed would be ~750ms for a 100MB file

***
### 2.

Describe a TCP/IP packet in detail. Describe the header, how many bytes it is, and which components it contains. What data can come after the header?

TCP packets
> TCP almost always operates in full-duplex mode (two independent byte streams traveling in opposite directions). Only during the start and end of a connection will data be transferred in one direction and not the other. TCP uses segments to determine whether the receiving host is ready to receive the data.

>When the sending TCP host wants to establish connections, it sends a segment called a SYN to the peer TCP protocol running on the receiving host. The receiving TCP returns a segment called an ACK to acknowledge the successful receipt of the segment. The sending TCP sends another ACK segment and then proceeds to send the data. This exchange of control information is referred to as a three-way handshake.

The TCP packet format consists of these fields:
1. Source Port and Destination Port fields (16 bits each) identify the end points of the connection.
2. Sequence Number field (32 bits) specifies the number assigned to the first byte of data in the current message. Under certain circumstances, it can also be used to identify an initial sequence number to be used in the upcoming transmission.
3. Acknowledgement Number field (32 bits) contains the value of the next sequence number that the sender of the segment is expecting to receive, if the ACK control bit is set. Note that the sequence number refers to the stream flowing in the same direction as the segment, while the acknowledgement number refers to the stream flowing in the opposite direction from the segment.
4. Data Offset (a.k.a. Header Length) field (variable length) tells how many 32-bit words are contained in the TCP header. This information is needed because the Options field has variable length, so the header length is variable too.
5. Reserved field (6 bits) must be zero. This is for future use.
6. Flags field (6 bits) contains the various flags:
    * RG—Indicates that some urgent data has been placed.
    * ACK—Indicates that acknowledgement number is valid.
    * PSH—Indicates that data should be passed to the application as soon as possible.
    * RST—Resets the connection.
    * SYN—Synchronizes sequence numbers to initiate a connection.
    * FIN—Means that the sender of the flag has finished sending data.
7. Window field (16 bits) specifies the size of the sender's receive window (that is, buffer space available for incoming data).
8. Checksum field (16 bits) indicates whether the header was damaged in transit.
9. Urgent pointer field (16 bits) points to the first urgent data byte in the packet.
10. Options field (variable length) specifies various TCP options.
11. Data field (variable length) contains upper-layer information.

The IP packet format consists of these fields:
1. Version field (4 bits) indicates the version of IP currently used.
2. IP Header Length (IHL) field (4 bits) indicates how many 32-bit words are in the IP header.
3. Type-of-service field (8 bits) specifies how a particular upper-layer protocol would like the current datagram to be handled. Datagrams can be assigned various levels of importance through this field.
4. Total Length field (16 bits) specifies the length of the entire IP packet, including data and header, in bytes.
5. Identification field (16 bits) contains an integer that identifies the current datagram. This field is used to help reconstruct datagram fragments.
6. Flags field (4 bits; one is not used) controls whether routers are allowed to fragment a packet and indicates the parts of a packet to the receiver.
7. Time-to-live field (8 bits) maintains a counter that gradually decrements to zero, at which point the datagram is discarded. This keeps packets from looping endlessly.
8. Protocol field (8 bits) indicates which upper-layer protocol receives incoming packets after IP processing is complete.
9. Header Checksum field (16 bits) helps ensure IP header integrity.
10. Source Address field (32 bits) specifies the sending node.
11. Destination Address field (32 bits) specifies the receiving node.
12. Options field (32 bits) allows IP to support various options, such as security.
13. Data field (32 bits) contains upper-layer information.
***
### 3.
How does the network protocol guarantee that a TCP/IP packet is complete after transmission?

The short answer would be hashing.  The header and data in the packet are hashed before transmission and re hashed and compared to the originating hash on the recieving end. If they match, the packet is good.

TCP packets are very complex and incorporate several mechanisms to ensure connection state, reliability, and flow control of data packets:
* Streams: TCP data is organized as a stream of bytes, much like a file.
* Reliable delivery: Sequence numbers are used to coordinate which data has been transmitted and received. TCP will arrange for retransmission if it determines that data has been lost.
* Network adaptation: TCP will dynamically learn the delay characteristics of a network and adjust its operation to maximize throughput without overloading the network.
* Flow control: TCP manages data buffers and coordinates traffic so its buffers will never overflow. Fast senders will be stopped periodically to keep up with slower receivers.
* Round-trip time estimation: TCP continuously monitors the exchange of data packets, develops an estimate of how long it should take to receive an acknowledgement, and automatically retransmits if this time is exceeded.

***

### 4.
 What is the difference between TCP and IP?

TCP is a connection-oriented Layer 4 protocol that provides full-duplex, acknowledged, and flow-controlled service to upper-layer protocols. It moves data in a continuous, unstructured byte stream. Sequence numbers identify bytes within that stream. TCP can also support numerous simultaneous upper-layer conversations.

TCP makes up for IP's deficiencies by providing reliable, stream-oriented connections. The protocol suite gets its name because most TCP/IP protocols are based on TCP, which is in turn based on IP. TCP and IP are the twin pillars of TCP/IP. TCP adds a great deal of functionality to the IP service.

IP is the Layer 3 protocol that provides fragmentation and reassembly of datagrams and error reporting. Along with TCP, IP represents the core of the Internet protocol suite. 
***

### 5.
 Why is 3d performance so much higher with a graphics card than without? Modern CPUs are extremely fast, what is limiting their performance?

Parallelism...
 GPU's highly parallel structure makes them more efficient than general-purpose CPUs for algorithms where the processing of large blocks of data is done in parallel.