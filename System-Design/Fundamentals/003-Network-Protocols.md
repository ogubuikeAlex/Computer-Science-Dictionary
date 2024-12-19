Network protocols represent the various ways machines interact, how information is transmitted, etc.

A network protocol will typically cover everything that involves smooth and complete communication between machines.
Some examples of network protocols:

IP: Internet protocol is the oldest protocol and is generally how the internet was built. It sends information in packets. Each packet has a head and a payload/data section. The Head represents the packet information and the payload represents the data being transmitted.

The greatest limitation of IP is that each packet can carry only a small amount of data. So to send a large amount of data, you generally have to split it up in multiple packets. 

This was worrisome because, they was no way to ensure that every single packet would actually be transferred safely without loosing anyone. Even worse was there was no guarantee to the order in which the packets would be delivered.

TCP: Transmission control protocol was built on top of IP. Its sole aim was to solve the problems associated with IP. It had a mechanism to ensure that the packets would arrive in the right order. It had error reporting system to signify the sender machine when a packet was not delivered successfully, etc.
TCP connections happened typically via something called a handshake. The sender will send a couple of packets to the receiver to inform it that it wants to connect. The receiver would send a response if it agrees and then the sender will send a final message to finalize making the connection "Alive". 
As long as the connection is alive, the two machines can exchange data.
TCP solved the IP issue but was still inefficient in really catering for our growing data manipulation and transfer needs.That was how HTTP was born

HTTP: Hypertext transfer protocol introduced the request-response model, where a machine sends a request and the other machine receives the request and sends a response. TBC with (Q and A) 


## FAQs
### At what point does a machine get issued an IP address

A machine is issued an IP address during the process of network configuration, typically when it connects to a network. Here’s how it usually happens:

- Boot-Up/Network Connection:

 When a machine (like a computer, smartphone, or any networked device) is powered on or connects to a network, it begins the process of obtaining an IP address.
- DHCP (Dynamic Host Configuration Protocol):

 - Most networks use DHCP to automatically assign IP addresses. When the machine connects to the network, it sends a DHCP request, asking for an IP address.
 - The DHCP server on the network receives this request and assigns an available IP address to the machine from a pool of IP addresses. This address is typically leased to the device for a specific period.
- Static IP Assignment (if applicable):

 - Alternatively, if the machine is configured with a static IP address, it doesn’t go through the DHCP process. Instead, it uses the pre-assigned IP address every time it connects to the network.
- IP Address Issued:

 - Once the IP address is assigned, either through DHCP or a static assignment, the machine can communicate with other devices on the network or access the internet, depending on the network configuration.

 ### Can a server have multiple ports?
 Yes, a server can have multiple ports, and this is actually very common. Here's how it works:

#### Understanding Ports
- Ports are logical endpoints on a server used to distinguish between different types of network services. Each port is identified by a number, ranging from 0 to 65535.
- IP Address + Port: When a client connects to a server, it connects to a specific IP address and port combination (e.g., 192.168.1.1:80).

#### What happens if I use a Number larger than 65535 as a port?
Ports are indeed identified by numbers ranging from 0 to 65535, as they are represented using a 16-bit unsigned integer. This allows for 2^16 (65536) possible values, but since numbering starts from 0, the highest possible port number is 65535.

If you attempt to use port number 65536, you'll encounter an error because it exceeds the valid range. Operating systems and networking protocols will not recognize port 65536 as valid, and it will be rejected when used in configurations or programming.

For context:
- **Valid port numbers**: 0 to 65535
- **Common uses**:
  - Ports 0-1023: **Well-known ports** for common services (e.g., HTTP on port 80, HTTPS on port 443).
  - Ports 1024-49151: **Registered ports** for user processes or applications.
  - Ports 49152-65535: **Dynamic or private ports** for temporary purposes, often used in client-server communications.

#### Multiple Ports on a Server
- Multiple Services: A single server can host multiple services, each running on a different port. For example:

  - Port 80 for HTTP (web server)
  - Port 443 for HTTPS (secure web server)
  - Port 22 for SSH (secure shell)
  - Port 25 for SMTP (email server)
  - Port 3306 for MySQL (database server)
- Same Service, Different Ports: Sometimes, the same service might be available on different ports. For instance, a web server might listen on both port 80 (HTTP) and port 8080 (for alternative access).

- Load Balancing and Redundancy: In high-availability setups, multiple instances of the same service might run on different ports for load balancing or redundancy.

- Custom Services: Developers might create custom services that listen on non-standard ports, which are not typically used by well-known services.

#### Example
Imagine a server hosting a website, an email server, and an SSH service:

- Port 80: Handles incoming web traffic via HTTP.
- Port 443: Handles secure web traffic via HTTPS.
- Port 22: Handles SSH connections for remote server management.
- Port 25: Handles incoming email traffic via SMTP.
In this setup, the server is using multiple ports to manage different types of network traffic simultaneously.