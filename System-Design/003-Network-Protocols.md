Network protocols represent the various ways machines interact, how information is transmitted, etc.

A network protocol will typically cover everything that involves smooth and complete communication between mac/machines.
Some examples of network protocols:

IP: Internet protocol is the oldest protocol and is generally how the internet was built. It sends information in packets. Each packet has a head and a payload/data section. The Head represents the packet information and the payload represents the data being transmitted.

The greatest limitation of IP is that each packet can carry only a small amount of data. So to send a large amount of data, you generally have to split it up in multiple packets. 

This was worrisome because, they was no way to ensure that every single packet would actually be transferred safely without loosing anyone. Even worse was there was no guarantee to the order in which the packets would be delivered.

TCP: Transmission control protocol was built on top of IP. Its sole aim was to solve the problems associated with IP. It had a mechanism to ensure that the packets would arrive in the right order. It had error reporting system to signify the sender machine when a packet was not delivered successfully, etc.
TCP connections happened typically via something called a handshake. The sender will send a couple of packets to the receiver to inform it that it wants to connect. The receiver would send a response if it agrees and then the sender will send a final message to finalize making the connection "Alive". 
As long as the connection is alive, the two machines can exchange data.
TCP solved the IP issue but was still inefficient in really catering for our growing data manipulation and transfer needs.That was how HTTP was birth

HTTP: Hypertext transfer protocol introduced the request-response model, where a machine sends a request and the other machine receives the request and sends a response. TBC with (Q and A) 