## What is leader election?
Imagine a product that helps  users track their friends and also allows them save periodically so that when its time to get a friend a gift, they can alway just have money to spare. We have a db for storing user personal and payment information. We also have a third party integration for us to be able to collect funds from users.

This service needs information from your database to be able to serve you well. For security reasons, you decide to  ensure that the third party service does not interact with the DB directly. Instead we have a Service between the DB and the third party service. 

This service will essentially be able to pull information from the database and periodically send a message to the third-party notifying it of when to deduct funds from a user.

Cool application right?

### **The problem now**
 becomes what will happen if this our service goes down for any reason?
At this point it is natural to think  => that's easy, just add redundancy to the system.

The new problem now is we do not want a debit order to be created across multiple servers for very obvious reasons.

In this scenario, the answer is redundancy but we need a system that will have multiple servers such that once a server goes down another server quickly takes over.

A leader election is a system employed in a distributed system, where a group of machines or servers elect or reach a consensus as to which of them is the "leader" or more accurately which of them is the active server that will perform the business logic at a particular time.

The other servers are called followers and they wait in anticipation for when the current lead would become unavailable and once this happens a new election happens.

The difficult part of this is ensuring consistency in state (state as in info for who the leader is at a point in time) across all the different machines. 

This can be achieved using consensus mechanisms that usually involves complex mathematical calculations. Example of consensus mechanisms are Paxos or Raft.

## How Can We Implement Leader Election

Due to how complex the consensus mechanisms are we simply use services that have implemented them. Examples include ETCD and Zookeeper.

For ETCD, the way we would implement this is to make our servers interact with ETCD in such a way that even though they are all connected only one of them is stored as the leader in ETCD, For context ETCD is a key-value store which is highly available thanks to its underlying implementation of the raft consensus mechanism. 

This means that the server that is considered to be the leader will be stored as a key-value pair on ETCD and to swap that value  we can use the ETCD structure to swap that value securely

## Potential Flow For A Leader Election Using ETCD:
- We have different servers with the same code 
- When a server is connected it would immediately start an infinite while loop that executes code (become_leader) to try to make that server the leader. Since they are multiple servers connecting at once based on the consensus mechanism one of these serves will be implemented  as the leader. each server will send an ETCD client, a lease, key and a value
- This become_leader logic will return a boolean (isLeader) and a lease (a value that is refreshed frequently to show that the leader is still active)
- If the server is not the leader (isLeader == false) it calls the (wait_for_next_election method) and then calls the become_leader function again
- If the server is the leader, then the lease, key , value and client has been updated to reflect the new leader.  the new leader will simply perform the business logic and repeatedly try to refresh the lease and as long as the lease is refreshed with no issues then it will keep being a leader
- If something happens and the lease is not refreshed appropriately because of network errors or the server is manually stopped then we immediately revoke the lease and delete the leader key that was set and this evokes a termination or kill event. 
- The wait_for_election method listens for the kill event. For every iteration it has a watch id which gets deleted if a kill event is deleted. Weh we enter the method, we first check if a kill event happened, if one happened, we evoke a start_election event.
- If a start-election event was not started, then we simply sleep for a few seconds and check again for a start election event
- If a start-election event was started, then we will delete the watch key. Which is us ending the watch part y and then we would return to the initial point of electing a leader





