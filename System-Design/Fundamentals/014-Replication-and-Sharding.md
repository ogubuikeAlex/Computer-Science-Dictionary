## What is the Idea Behind Replication?
A system's database is a super important part of the systems overall efficiency. If a system has just one database and that database goes down the entire system would stop responding. This is where replication comes in. We can have a data replica that would ensure that the system does not go down simply because of a malfunction. 

The question now is how do we put up with data inconsistency because its bad to not have your system online but it is equally bad to serve stale data to users. To ensure consistency with the databases, we could either update both databases simultaneously so that they always have the same data or we could follow an asynchronous approach where we sync up date at specific time intervals. The issue with the first approach is that it will drag down the speed at which writes operation happen because we are going to be updating two servers for every time an update is sent.

At this point, we would have to have trade-offs based on the system we are building. We would think out the most important character our system needs to best serve our clients. 

### How can we improve latency using replication?
Imagine a company that serves users in different parts of the world, we can have database replicas assigned per location. 

For example if i make an update from Nigeria, everyone in my location will have access to read that information fast (Lower Latency), then at a specific time interval (say 5 mins) the database will speak to a particular master database to sync up data - send information for update that has happened from its own location and receive information from other locations. 


### This is a great way to reduce latency. But is it enough??

As much as we can try to improve the latency of our systems. How about throughput? If any of these serves in my example that are serving a particular location does not have the capacity to handle the load for that location (low throughput). This would slow down the process and the latency strategy will not work.

Yes! we can always scale our servers vertically but there is a limit to vertical scaling. Welcome sharding.

## What is Sharding?
Database sharding is the process of splitting up a database into smaller more manageable chunks called shards or database partitions

## Why Do We Need It?
Two reasons actually
- When replicating a database, it can feel counter intuitive to simply copy all the data from one database into another one. So we can simply split up the data and store different sections of the data instead. This way we avoid repetition of unnecessary data
- For horizontally scaling a database server

Advice; The logic that manages which shard data goes to should be in a reverse proxy that manages the database servers, this is a better approach than setting up the the logic in the application server.

### Sample Sharding Strategies => 
- Based on location
- Based on type of data (users in one DB, Payments in one Db etc)
- Based on the hash of a column

Read More: https://medium.com/@ogubuikealex/what-is-database-sharding-d045be7c50b5


## Conclusion:
Database Replication can be used to increase the redundancy of a system and other times it can be used to improve latency.
Sharding on the other hand is done mostly to increase the throughput of the system.