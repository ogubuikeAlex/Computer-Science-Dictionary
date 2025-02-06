What is a peer-to-peer network

A p2p network is a collection of machines referred to as peers that divide a workload between themselves to presumably complete the workload faster than would otherwise be possible.

Imagine a system where we are sending data from a machine to a million other machines. The speed or throughput of the machine will determine the speed of this system. Even in a situation where the single machines throughput is 5GB/sec the application will still be slow.

In a p2p network, we would split the file into super small chunks. Then we would send each of these small chunks to different peers in the network.

Then Each peer would communicate with other peers to get the little chunks it needs to build out the initial file. This essentially converts all the peers from being just receivers to being actual senders, as opposed to having just one machine doing the sending. This process is a lot faster

For this system to work, the peers need a way to know which peer to talk to next
What is peer discovery?
What is peer Selection?

We have two methods of doing this
- Using a Tracker: We could have a separate database server that can give accurate information about the information held by each peer.
- Using the gossip protocol: The idea here is the peers talk between themselves and share the information about which peers holds what information. 
So a peer carries mapping that maps each peer to the information it carries.

The gossip protocol is when a set of machines talk to each other in an uncoordinated manner in a cluster to spread information through a system without requiring a central source of data

The way this might work would be via a distributed Hash Table
- The first peer gets information and its Ip address and content is added to a mapping. The mapping is stored on both a DB and on the peer. 
- The second peer get information and its Ip address and content is appended to a mapping. The mapping is updated on both a DB and on the peer. This peer automatically has information about its own information and the information about any peer before it.


Example of P2P Systems => Kraken and Torrenting
