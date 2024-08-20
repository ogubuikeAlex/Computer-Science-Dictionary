What is hashing?
Hashing is the process os converting an arbitrary amount of data into a fixed sized data.

Where does hashing come in system Design?
Imagine if we have a system that has multiple clients and servers and we are potentially suffering from missing cache hits because we have servers that have in-memory caches. We could decide to make this better by having a loadBalancer that is smart enough to know where exactly to route requests to.
How can we achieve this? We could use hashing to create hashed values that serve as keys and then the cached response will be the value.

- Sample hashing strategy with requests?
let me explain further, a typical example will be say we have 5 serves and we have 5 clients. We create a hashing function that basically takes the client usernames and hashes them into a number. Based on the hash we get, we do a modulo operation with the total number of servers we have available. The resultant number is used as the index to select a server to use
for example
```
const serverToUse = GetHash(clientOneName) % totalNumberOfServers;
```

This will work as long as we have the same number of servers.
- What is uniformity in hashing?
- What the most popular hashing functions i can use?
- What happens if new servers are added or servers are to ber removed?
For the simple hashing strategy, if we decide to remove or add a server we have to recalculate based on the new number of servers available. This is not optimal. What can we do better?

- What is consistent hashing strategy? 
For consistent, imagine we have a conceptual circle numbered 0- 360. Where every number on this circle represents a point where we could place a component in our system. If we have 4 servers, they could be kep at point 0, 90, 180 and 360 hypothetically. Then our clients will also be given numbers for example say we have 6 clients and they are give => 10, 110, 180, 60, 250, 310, 320.\
The way this works is that the clients will send requests to servers that are deployed at locations equal or immediately after their own location. 
For example, client one => Server2, client two => Server3, client three => server3, client four => Server1, client five => SeServer4, client Six => Server4.

This works because if one server in the system goes missing or bad a client will simply check for the Server at the next nearest spot and send requests to.

- How can we optimize this so that the requests are not routed to just one part of the circle?
Based on this, we could argue that this could be inefficient if most of the client requests are being sent to one part of the fictional circle.
To make this better, we could employ multiple hashing algorithms and so each server has multiple hashed values and we could place each value in different parts of the same circle. For example, server one is hashed to => 12, 36, 312 positions and clients that stumble on this location are essentially routing to the same server.
Question: What if we use different hash functions and encounter two servers with the same value. For example server 1 in hash function One gives 2 and Server 2 in  Hash function 3 gives 2 also.

- What also happens if we have one server that is stronger than any other servers?
This system can also help us in this case because we can decide to has the stronger server multiple times so it has more representation on the fictional circle and ultimately gets called more than the weaker ones.

- What is rendezvous hashing strategy?
This a method where the each client creates a numbering system around the servers. With this system they are able to put the servers in a scale ranging from most suitable server to least suitable server. And they will always pick the most available server starting from the most suitable server in th list. 
So basically, anyone that is missing will be skipped ans the next one will be selected.

Question: if we add a new server, does this not require recalculation and how exactly is this better than the simple strategy?