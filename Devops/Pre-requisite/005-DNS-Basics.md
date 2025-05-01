
DNS stands for Domain name service

Previously when we have two computers and we want to communicate between them, we will need to use a more friendly name than the IP Address
so instead of saying `192.186.1.11` i want to just say `db-server`.
To do this i can juts update the `/etc/hosts` file:
`cat >> /etc/hosts`

This is not valid because there is noway for us to actually confirm that the name and the IP-address are mapped correctly. Imagine if we have 20 computers speaking 
between themselves and each one has its own `/etc/hosts` file. It will be a mess but it can work for local networks
This kind of name to Ip address mapping is called **Name Resolution**

In recent times, this has become really hard to manage because imagine that one of our hosts changes its IP address, then we will need to change it in every single /etc/hosts file
This is why we have decided to have a central server for storing name-Ip mapping called the **DNS NameServer**
Now we can post our host to look up that server when they need to look up a name
- How do we do this:
    Every host has a DNS config resolution file at `/etc/resolv.conf`. All we need to do is add a new entry to that file:
        `nameserver 192.168.1.100` //assuming 192.168.1.100 is our DNS nameserver Ip
    Anytime a host name comes up that it does not know about, it looks it up from the DNS
    If the IP of any of the hosts changes, we can simply update the DNS and all hosts will have access to the recent info

Now we can use /etc/hosts for personal or test servers.
Note that if we have the same entry on both our DNS and in our `/etc/hosts` file then the `/etc/hosts` takes priority
For example, if we have:
    `192.168.1.100 prod-db` ///etc/hosts
    `192.168.2.220 prod-db` //DNS nameserver

Then by default the one in `/etc/hosts` will be used. To change this behavior, you can update the `/etc/nsswitch.conf` file:

From:
- hosts: files dns
To
- hosts: dns files

What if we try to interact with a server that is not in either lists? Well we can actually have multiple nameservers configured on our host in the `/etc/resolv.conf` file:
 `nameserver 192.168.1.100` 
 `nameserver 8.8.8.8` 

But we will need to do this across every single hosts we have. A more viable option will be to configure the DNS it self to send any unknown host name to `8.8.8.8`
Note: `8.8.8.8` is a popular nameserver from google that has the name ip mapping of most if not all websites

## Domain Name
www.codetivite.org

the root of a domain name is represented by a single dot (".") and is essentially the base level of the domain hierarchy, where all other top-level domains (like .com, .org) are situated
The last part ("org") represents the top level domain. -> 
The first part ("www" or "admin") represents the subdomain. -> 
![alt text](image-3.png)

How?

- When we send a request the host from where we the app is hosted will first try to retrieve the name-ip mapping from its `/etc/hosts`
- It will not see it there, so it will forward the request to the internet
- The first place it goes is a root server and the root server will send it to the server serving .com requests  
- The .com server will then point to the server hosting codetivite
- The server hosting .codetivite will also then serve the IP of the server serving the subdomain

In other for this to be quick, the organizational server may cache the IP for a few seconds

What if we want to be able to have the word "web" be used to refer to web.company.com
In the organization's server we will add a new entry called `search`:
 `nameserver 192.168.1.100` 
 `nameserver 8.8.8.8` 
 `search mycompany.com prod.mycompany.com`


 ## Record Types
 Tells us how our IPs are stored on the Ips
A record -> Storing IP to host name
AAAA record -> storing IPV6 to host name
CNAME record -> used to map one name for another name

## Tools To Test DNS 
nslookup, dig

Nb: nslookup does not take the /etc/hosts file content into consideration

