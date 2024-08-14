## What is a Proxy
A proxy is a web server that sits between a client (or set of clients) and a server or set of servers and acts on behalf of either the client or the server.

## Types of Proxy
There are two types of proxies
- Forward Proxy
- Reverse Proxy

A forward proxy is a web server that acts on behalf of the client.

A reverse proxy is a web server that acts on behalf of the server.


## Process Flow For Proxies in A Client-Server Interaction

- Forward Proxy: A forward proxy is used when the client does not want its identity to be known by the server it is interacting with - A typical use case is when we use VPNs to access sites or information that ordinarily shouldn't be available to a particular geographical location or blocked by an organization.

The entity that owns the client will configure the forward proxy on their end. When a request is made from the client,the request is routed to the proxy. Then the proxy reroutes the request to the server acting as the source of the request. 

Even though they are some reverse proxies that will still send the IP of the original source of the request, typically the IP will send its information and make the server believe it is the source. THis means that to the server, it is interacting with just one client. So it will simply send the response back to the forward proxy and life continues.

- Reverse Proxy: A reverse proxy is used when serves does not want clients to interact directly with it. This can be for a lot of reasons security, optimization, load balancing, caching etc.

A typical example is when we have a set of servers and we want to have a load balancing to ensure no single server is overwhelmed, we could employ a reverse proxy as a load balancer. Another example is if we want to filter the kind of requests that actually gets to out server.
When a client sends a request, the client has no idea that they is a proxy-layer. It feels like it is speaking to the server directly. 

This is because, when the DNS query is made, the IP address returned for a system with a reverse proxy is the IP address of the reverse proxy web server. This is the IP the client then interacts with.
The reverse proxy will receive the request and based on certain criteria, it could either route the request to the actual server, send a cached response, or just return an error message.

In the scenario where the reverse proxy actually sends the request to the server. The server will then send a response to the reverse proxy and the reverse proxy will send back the response to the client and the flow is completed.


