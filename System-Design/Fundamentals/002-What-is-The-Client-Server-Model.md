What is the client server model

let’s start with definition of terms

- Client is an application that has the ability to receive input from people, package that input in the proper format and then send the input to a server eg Browser like Chrome, Client apps like Postman
- A server is an application that can receive requests from a client and then send a response back to the client.
- IP Address is the unique identifier that differentiates all the machines in the  world. It is an address given to every machine that is connected to the public internet.
- HTTP Request: is a request that follows the guidelines of the  hypertext transfer protocol
- Port: is a specific channel that a server listens in. A machine can have multiple servers running on it. And each server will have a port it is listening on. For example if I send two requests using a machines IP, one to 3000 and one to 3002. I am essentially interacting with two servers via the different ports
- DNS means domain name system. We have large and trusted guys who basically tells the client the correct IP address to interact with

The client-server architecture basically shows the relationship between a server and a client.

A client exists on its own and can interact with multiple servers and a client does not know the exact server it is interacting with.

When I go and put in [codetivite.org](http://www.codetivite.org) in my browser.

I am trying to view the files that make up codetivite website. These files are stored in a data storage that the client doesn’t know. But the server actually has the information of the data storage and even how to get the information.

What the browser knows is that [codetivite.org](http://codetivite.org) is actually a fancy name for the actual address where the files I am looking for are.

The browser would first do a DNS query to confirm the IP address of the server that has the information it needs. Then it will send an HTTP request to the server on the correct PORT.

The server would receive this request retrieve the files from the data storage and send it back to the client via the source IP Address contained in the request it received.