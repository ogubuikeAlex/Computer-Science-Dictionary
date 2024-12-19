## What is a LoadBalancer?
A load balancer is a web server (A type of reverse proxy) that manages traffic sent to servers in a way that balances out the throughput of the system so every server is alloted the amount of traffic that it can handle efficiently.

## Examples of Possibles Places You can Find A LoadBalancer?
Basically anywhere you see a client - server model. This can be between clients and servers, between servers and servers and between servers and databases etc.

## Types of LoadBalancers
- Hardware Based Load Balancers: This is actual hardware that can be used to manage load. It is usually more rigid in terms of structure and can be an expensive solution considering that Hardware is generally expensive.
- Software Based Load Balancers: These are software solutions built to manage load. They are usually less expensive and are super flexible and easy to customize.

## How Do LoadBalancers Know Which Server To Send Requests? (Server Selection Strategy)
Solid question!
They are different server selection strategy employed by LoadBalancers:
- **Round Robin Selection Strategy:** For instance in a system with 5 servers, say A - E. The load balancer that employs this strategy will route traffic in a sequential manner. So first to A, then to B, then to C, then to D, then to E and Then back to A again.

- **Weighted Round Robin Selection Strategy:** This is still the Round Robin selection strategy but with a twist. Each server is also given a weight which represents the frequency of traffic that it can handle. For example, say we have 5 servers, labelled A - C. Say Server A is given a weight of 1,Server B is given a weight of 3 and Server C  is given a weight of 1. 
The first request will get to server A because it is the first server in the system. Then the second request will get to server B. But since server B has a weight of 3, the next couple of requests coming in will still be routed to server B before the load balancer moves to server C. What this essentially means is that the load balancer will be route more requests to the server with the higher weight.

- **IP Based Selection Strategy:** 
This is when the load balancer sends requests based on the particular source IP of the request. It could be location based for eg with VPNs. It could also help in caching. For example, if we have restaurants all around Africa and we are expecting people to ask questions to find restaurants closest to them. We could cache the result of some of the requests, so that when a person from Nigeria makes a request we retrieve the info and store it in the server for requests with a Nigerian IP. Whenever anyone else from the same IP tries to receive that same information. It is already cached in the ip selected server. The load balancer could implement this by hashing the request and using the hash as a key in a stored hash table, so its easier to know that requests from the same related IP and with the same demands are routed to servers where that result has been previously cached

- **Path Based Selection Strategy:**
This when the server are selected based on requested path. For example "/users" is routed to server A and "/courses" is routed to server B.

- **Random Selection:** Just Wing It

- **Performance Based Selection:** The load balancer is continually check the performance and health of the serves and based on its findings, it routes traffic to servers that have the potential to best manage traffic.


## Combining Server Selection Strategies 
We introduce load balancers when they is a load problem and we want to move to a distributed system. So for example we had many clients and one server and the server was starting to get over whelmed, so we added more servers to serve the clients. But now we need a way to manage how the clients talk to the server so we do not end up with the same problems we are trying to solve - so we add a load balancer.
The only issue now is that if we have one load balancer, then all the clients will be speaking directly to the load balancer and depending on the load balancers throughput, we might have just introduced a single point of failure because our load balancer can become choked.
So this means we should potentially have more than one load balancer to manage the traffic.

The question now is, is there a way we can combine multiple server selection strategies? Yes

What we could do is to have two layers of load balancers
- Layer one Load balancer receive requests based on IPs and route to layer two load balancers
- This set of load balancers receive the request and then they use the round robin server selection strategy.

## What is a DNS Round Robin
This is usually employed when we have a site which has multiple IP address assigned to it because it can be served by multiple servers. For example, DNS queries for "google.com" usually returns more than one IP address. This is because they is a load balancer managing the process and based on the load will route you to use another server to get the same resource. For this to work all the different servers will serve the same resources.