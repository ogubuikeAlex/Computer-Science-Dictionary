## What is rate limiting?
This is the restriction of the frequency per time at which a particular action can be performed. 

## Why is it important?
If we do not have a rate limited system we are open to denial of service attack.
This is when a bad actor floods the target system in a bid to bring down or damage the system. a DDos is short for distributed denial of service attack. It is an attack in which the traffic flooding the system is coming from many different sources making it harder to defend against.

## How can this be implemented?
We could use redis as a server that stores information about the rate limiting of a system. Remember we can rate limit using a lot of factors for example, we could use a users email to check how many times they are sending requests. We could rate limit a region using their IP addresses etc. 

When the client sends a request to the server, the server first checks on redis to confirm that the user is not abusing the app or has not passed the rate limiting threshold and based on the reply, the server either sends an error to the client or proceeds to perform the operation.

### Tier-based rate limiting:
For example, you can only run 3 requests per second but you can only run 6 requests per 30 seconds and you can only run 10 requests per 1 minute.

This is a bit more complicated but is a better approach to things