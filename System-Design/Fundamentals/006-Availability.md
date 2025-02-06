## What is Availability?
The percentage of time a system is operational and accessible, allowing data to be accessed. High availability systems minimize service interruptions by ensuring systems are always accessible. 

How resistant a system is to failure i.e the fault tolerance of a system. 
Also its ability to remain operational over time.
We can also say, it is the percentage of time in a year when the system is operational.

## How Do We Measure Availability?
Even though availability is a measure of uptime, 
there is a standard way of calculating it.

This is referred to as the Nines. 
90% => One Nine
99% => Two Nines
99.9% => Three Nines
99.99% => Four Nines
99.999% => Five Nines (Gold Standard)
99.9999% => Six Nines
99.99999% => Seven Nines
99.999999% => Eight Nines
99.9999999% => Nine Nines (Overall Best)


**Interestingly**, it seems alright to say that the a system is 
90% available until you actually calculate and realize that 
being only 90% available in a year means that for at least 
35 Days in a year (Over a month), the application was down.
Which is an outrageously expensive expenditure for the business in  question.


- What is Gold Standard HA or Highly Available
Five Nine is the gold standard of availability for a system.
Most systems try to be at the level at least.

- Implied vs explicit guarantee of availability
With the advent of the cloud and people getting more and more serious with design scalable 
and available systems, there is an implied guarantee of availability.
There are still some services that have explicit guarantee of availability.

They do this via Service-Level Agreements (SLA)


## What is the difference Between SLA and An SLO
An SLA is an agreement between a service provider and their users where the service providers 
show their offerings in terms of system availability and performance. Also in the document they would usually
be a clause as to what will happen if the service providers perform below the stated agreement.

An SLO (Service-Level Objective) is usually used synonymously with an SLO but is a different though related term.
The SLO is what makes up the promises contained in an SLA. For example, within an SLA, a company could be promising
"Five Nines" level availability. This singe statement is an SLO. A single SLA can have multiple SLOs which cumulatively
state the company's offerings

## How do I ensure high availability
The best way to ensure high availability is to eliminate single points of failure.
A single point of failure is a point in a system whose failure can disrupt
the entire process. For example, For a system with one DB and no backup.
If the DB fails, then automatically, the entire system can not read or write data. 
This is also the same for a system with just one server.
If the requests coming to the server at once are so much that they are able to overwhelm the server
and the server crashes, then the entire system would fail.

### What is the best way to prevent this?
**Redundancy**
Redundancy is the process of adding duplicate components to a system to remove the possibility of a single point of failure.

There are two ways in which this can be done
#### Passive Redundancy: 
- A typical example is adding multiple servers to a system. All the servers work at the same time in such a way that if one goes down the others
just continue working and can suffice until the downtime of the other server is restored.

#### Active Redundancy
- An example is having multiple servers in a system. Some of these servers are actively working 
while some are basically just monitoring the active servers.
The server system is implemented in a way that if anything happens to the active server, the other servers can
immediately replace it before the entire system even notices that there is a glitch.


Note: For redundancy to be possible, we will most likely
employ the services a load balancer to ensure traffic is evenly distributed.
Interestingly, having one load balancer can also be a single point of failure
If the system has the tendency to get to that point, then redundancy will also be implemented at the load balancer level

