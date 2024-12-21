## What is hashing?
Hashing is the process is converting an arbitrary amount of data into a fixed sized data.

## Where does hashing come in system Design?
Imagine if we have a system that has multiple clients and servers and we are potentially suffering from missing cache hits because we have servers that have in-memory caches. We could decide to make this better by having a loadBalancer that is smart enough to know where exactly to route requests to.
How can we achieve this? We could use hashing to create hashed values that serve as keys and then the cached response will be the value.

- Sample hashing strategy with requests?
let me explain further, a typical example will be say we have 5 servers and we have 5 clients. We create a hashing function that basically takes the client usernames and hashes them into a number. Based on the hash we get, we do a modulo operation with the total number of servers we have available. The resultant number is used as the index to select a server to use
for example
```
const serverToUse = GetHash(clientOneName) % totalNumberOfServers;
```

This will work as long as we have the same number of servers.
- What is uniformity in hashing?
- What is the most popular hashing functions I can use?
- What happens if new servers are added or servers are removed?
For the simple hashing strategy, if we decide to remove or add a server we have to recalculate based on the new number of servers available. This is not optimal. What can we do better?

- What is consistent hashing strategy? 
For consistent, imagine we have a conceptual circle numbered 0- 360 (Hash Ring).

 Where every number on this circle represents a point where we could place a component in our system. If we have 4 servers, they could be kept at point 0, 90, 180 and 270 hypothetically. Then our clients will also be given numbers for example say we have 6 clients and they are give => 10, 110, 180, 60, 250, 310, 320.
The way this works is that the clients will send requests to servers that are deployed at locations equal or immediately after their own location. 
For example, 
- client one (10) => Server2 (90)
- client two (110) => Server3 (180)
- client three(180) => server3(180)
- client four(60) => Server1(90)
- client five(250) => Server4(270)
- client Six(310) => Server4(0).(as the circle wraps around).
- client Six(320) => Server4(0).(as the circle wraps around).

This works because if one server in the system goes missing or bad, a client will simply check for the Server at the next nearest spot and send requests to.

##### Question: Why Cant We Have 360 in The 0 - 360 Hash Ring?
Imagine the hash ring as a clock face:
- The positions are numbered from 0 to 359 (360 values).
- At position 360, it wraps back to 0 (similar to how a clock goes from 12 back to 1).
- Here’s why 360 isn't used directly: In a hash ring numbered from 0 to 359, position 360 is redundant because it is effectively the same as position 0. It ensures a seamless transition where moving clockwise past the highest position brings you back to the lowest position.


## How can we optimize this so that the requests are not routed to just one part of the circle?
Based on this, we could argue that this could be inefficient if most of the client requests are being sent to one part of the fictional circle.
To make this better, we could employ multiple hashing algorithms and so each server has multiple hashed values (aka virtual nodes) and we could place each value in different parts of the same circle. For example, server one is hashed to => 12, 36, 312 positions and clients that stumble on this location are essentially routing to the same server.

### Q1 What if we use different hash functions and encounter two servers with the same value. For example server 1 in hash function One gives 2 and Server 2 in  Hash function 3 gives 2 also?
Here’s how you can handle this:

##### Strategies to Handle Hash Collisions

1. **Use a Composite Key**:
   - Instead of just using the hash value, combine it with an identifier for the hash function or the server. For example, if you get a collision at position 2 from different hash functions, you can differentiate them by adding a prefix or suffix indicating the server or hash function: `H1-2` for Server 1 and `H3-2` for Server 2.

2. **Secondary Hashing**:
   - If a collision is detected, apply a secondary hash function to the colliding servers to get a new position. This way, you can resolve the collision by remapping one of the servers to a different position on the ring.

3. **Virtual Nodes**:
   - By assigning multiple virtual nodes to each server, you reduce the likelihood of collisions having a significant impact. Even if two virtual nodes collide, the load can be balanced across other virtual nodes.

4. **Offset Colliding Values**:
   - When a collision is detected, slightly adjust one of the values to avoid the collision. For example, if both Server 1 and Server 2 hash to 2, you can adjust Server 2’s position to 2.1 or another close but distinct value.

### Example Scenario

Let's consider the scenario where:
- **Server 1** using Hash Function 1 produces: 2
- **Server 2** using Hash Function 3 produces: 2

Using a composite key:
- **Server 1**: `H1-2`
- **Server 2**: `H3-2`

Or, using an offset:
- **Server 1**: 2
- **Server 2**: 2.1

### Q1-B: This method of adding virtual nodes does not solve the problem of overloading one node in a situation where the other nodes are offline or unavailable. Does that not make it inefficient?
Yes! While virtual nodes help distribute load more evenly under normal conditions, they may not fully address the issue of overloading when some nodes go offline. To enhance the system's resilience and ensure efficiency, additional strategies can be employed:

##### 1. **Dynamic Rebalancing**
- **Automatic Rebalancing**: Implement mechanisms to automatically redistribute data and load when nodes go offline. This can involve temporarily increasing the number of virtual nodes for the remaining active servers to spread the load.
- **Load Awareness**: Integrate load monitoring so that the system can detect when a node is overloaded and redistribute requests dynamically.

##### 2. **Replication**
- **Data Replication**: Store copies of data on multiple servers. If a node goes offline, its data can still be accessed from another server.
- **Redundant Hashing**: Hash data to multiple positions on the hash ring, so that each piece of data is accessible from more than one server.

##### 3. **Failover Mechanisms**
- **Failover Clusters**: Use clusters of servers that can take over the load of a failed node. This setup ensures that there’s always a backup ready to handle the increased traffic.
- **Graceful Degradation**: Implement strategies to gracefully handle overload situations, such as reducing the quality of service or prioritizing critical requests.

##### 4. **Consistent Hashing with Backup Nodes**
- **Primary and Backup Nodes**: For each virtual node, designate both a primary and a backup server. If the primary server goes offline, the backup server can take over the load.

###### Implementation Example:
- **Primary Hashing**: Assign requests to primary nodes using consistent hashing.
- **Backup Hashing**: Use a secondary hash function to assign backup nodes for each virtual node.

###### Combining Strategies:
Using a combination of these strategies can significantly improve system robustness and efficiency, even when some nodes are unavailable.

###### Example Scenario:
- Server 1 and Server 2 have overlapping virtual nodes. 
- If Server 1 goes offline, its load can be redistributed to Server 2 and the backup nodes.


### Q2 What also happens if we have one server that is stronger than any other servers?
This system can also help us in this case because we can decide to hash the stronger server multiple times so it has more representation on the fictional circle and ultimately gets called more than the weaker ones.

### Q3 What is rendezvous hashing strategy?
This a method also known as Highest Random Weight (HRW) hashing is where the each client creates a numbering system around the servers. With this system they are able to put the servers in a scale ranging from most suitable server to least suitable server. And they will always pick the most available server starting from the most suitable server in th list. 
So basically, any server that is missing will be skipped and the next one will be selected.

- **Q: my thought**: So this does not have anything to do with the server's throughput or physical distance from the client. Its just the name (identifier) of the server and the data item hashed to give a number and the highest number wins?
- **my thought answered**:Yes! In the context of rendezvous hashing, the weights are solely based on the hash values calculated from the combination of the data item and the server identifier. It doesn't consider factors like the server's throughput, physical distance, or other attributes. 
- **my thought answered Part 2**: in the context of rendezvous hashing, weight doesn't refer to physical weight but rather a numerical value assigned to determine the best server to handle a specific request. This "weight" helps to decide which server gets the data based on the highest computed value.

##### How Rendezvous Hashing Works

1. **Assign Weights**:
   - For each data item (or request), calculate a weight for each server using a hash function.
   - The weight is typically a combination of the hash of the data item and the server identifier.

2. **Select Server**:
   - The server with the highest weight for the given data item is chosen to handle the request.
   - This selection process ensures that each data item is consistently routed to the same server.

##### Example:

Suppose we have data item `X` and servers `S1`, `S2`, and `S3`. The weights might be calculated as follows:
- **Weight of S1 for X**: `hash(X, S1)`
- **Weight of S2 for X**: `hash(X, S2)`
- **Weight of S3 for X**: `hash(X, S3)`

The server with the highest weight is chosen to store or process the data item `X`.

### Benefits of Rendezvous Hashing:

1. **Uniform Distribution**:
   - Ensures that data is uniformly distributed across all servers, reducing load imbalances.

2. **Minimized Data Movement**:
   - When a server is added or removed, only a minimal number of data items need to be reassigned, ensuring efficient redistribution.

3. **Simplicity**:
   - The algorithm is simple to implement and doesn't require maintaining complex data structures.

### Use Cases:

- **Distributed Caching**: Ensures consistent and efficient distribution of cached data across multiple cache servers.
- **Load Balancing**: Distributes client requests evenly across multiple servers, optimizing resource utilization.
- **Distributed Databases**: Manages data placement and retrieval efficiently in distributed database systems.


### Q4:if we add a new server, does this not require recalculation and how exactly is this better than the simple strategy?
You're right that adding a new server will require some recalculation, but the way consistent hashing (including rendezvous hashing) handles this is what makes it more efficient and preferable compared to simpler strategies.

##### Adding a New Server

1. **Consistent Hashing**:
   - **Minimal Recalculation**: When a new server is added, only a small subset of data items needs to be reassigned to the new server. This is because the hash ring ensures that only the data items that map to positions between the new server and the previous server in the clockwise direction need to be moved. For example, we had server at 0 and 90. Then we add a new server at point 45, what happens is that all the clients from 0 - 90 who used to go to server at point 90 will now bw split. The ones from 0 - 45 will be reassigned to go the new server and the ones at  45 - 90 will still go to the sever at points 90.
   - **Efficiency**: This reduces the overall disruption and ensures that most of the data remains on the same servers, leading to minimal data movement.

2. **Rendezvous Hashing**:
   - **Minimal Reassignment**: Similar to consistent hashing, only data items that would hash to the highest weight for the new server need to be reassigned. The majority of data remains unaffected, ensuring efficient redistribution with minimal overhead. ### Clarification: Minimizing Data Movement in Rendezvous Hashing

   ##### Example:

    Let's say you have the following setup:
    - **Existing Servers**: `"Server1"` and `"Server2"`
    - **New Server**: `"Server3"`
    - **Data Item**: `"item1"`

    ###### Initial State:

    - **Weights**:
    - `weight_S1 = hash("item1" + "Server1")`
    - `weight_S2 = hash("item1" + "Server2")`

    - Assume:
    - `weight_S1 = 12345`
    - `weight_S2 = 67890`

    - Since `weight_S2` > `weight_S1`, `Server2` handles `"item1"`.

    ###### After Adding New Server:

    - **New Weights**:
    - `weight_S3 = hash("item1" + "Server3")`
    - Assume:
    - `weight_S3 = 78901`

    - Since `weight_S3` > `weight_S2` and `weight_S1`, `Server3` now handles `"item1"`.

    ###### Minimal Data Reassignment:

    - Only the data items for which the new server (`Server3`) has the highest weight need to be reassigned.
    - Data items that still hash to the highest weight for existing servers (`Server1` or `Server2`) remain unaffected.


3. **Simple Hashing (Modulo N)**:
   - In simpler hashing strategies like modulo N (where N is the number of servers), adding or removing a server requires rehashing all data items. For instance, if you use `hash(item) % N` to determine server placement, changing N (the number of servers) changes the modulo result for all items, leading to significant data movement and reconfiguration.

**Uniform Distribution**:
   - **Consistent Hashing** and **Rendezvous Hashing** both aim to distribute data more uniformly across servers. Simple strategies often suffer from load imbalances, where some servers may become overloaded while others are underutilized.
   - By using multiple virtual nodes (consistent hashing) or calculating weights (rendezvous hashing), these methods achieve a more even distribution of load, improving overall system performance.

### Q5. Why is Cache Miss Bad?

cache misses can be problematic. A cache miss happens when the requested data is not found in the cache, forcing the system to retrieve the data from the slower primary storage or database. This increases latency, consumes more resources, and can slow down the overall performance of the system.


### Q6. How Can in-memory cache cause cache miss?

1. **Cache Eviction**:
   - **Eviction Policies**: In-memory caches have limited size, so they use policies like Least Recently Used (LRU) or First-In-First-Out (FIFO) to decide which data to remove when new data comes in. If the evicted data is requested again, it will result in a cache miss.
   - **Cache Size**: If the cache size is too small to hold the working set of data, frequently accessed data might be evicted, causing cache misses.

2. **Cold Start**:
   - When the cache is initially empty (cold start), all requests will result in cache misses until the cache is populated with data.

3. **Data Updates**:
   - **Stale Data**: If the data in the cache becomes stale and is updated in the primary storage, the cache might need to invalidate the old data. Until the cache is repopulated with the updated data, requests for that data will result in cache misses.
   - **Inconsistency**: If the cache and the primary storage are not properly synchronized, it can lead to inconsistencies and cache misses.

4. **Missed Caching**:
   - **Uncached Requests**: If certain requests are not cached due to configuration or logic errors, those requests will always result in cache misses.
   - **High Variability**: If the system handles a wide variety of requests with low repetition, the chances of cache hits decrease.

5. **Hash Collisions**:
   - If the hash function used for caching produces the same hash value for different keys (hash collisions), it can cause cache entries to be overwritten and result in cache misses.

### Mitigation Strategies
To minimize cache misses:
- **Optimize Cache Size**: Ensure the cache size is sufficient to hold the working set of data.
- **Fine-tune Eviction Policies**: Choose appropriate eviction policies that match the access patterns.
- **Warm-Up Cache**: Prepopulate the cache during startup to reduce cold start misses.
- **Maintain Consistency**: Implement mechanisms to keep the cache and primary storage synchronized.
- **Monitor and Adjust**: Continuously monitor cache performance and adjust configurations as needed.


### q7: Are There Specific reasons In-Memory Cache can cause cache miss?

There are certain aspects that are more pertinent to in-memory caches on servers:

1. **Memory Constraints**: In-memory caches on individual servers are limited by the physical memory available on those servers. This constraint necessitates frequent evictions to make room for new data, potentially leading to more cache misses.
2. **Server-Specific Data**: In a system with multiple servers, each server's cache is typically independent. If clients are not consistently routed to the same server (due to load balancing), they might miss out on cached data stored on other servers.
3. **Latency Sensitivity**: In-memory caches provide faster access times compared to other caching methods (e.g., disk-based caches). This means that cache misses can have a more noticeable impact on performance, as the latency gap between cache hits and cache misses is larger.

### q8: Is It Still Considered a Cache Miss if the Data Found Was Inconsistent with the Most Updated Data?

Yes, it is still considered a cache miss if the data retrieved from the cache is outdated or inconsistent with the most updated data in the primary storage. This scenario is often referred to as a **stale cache miss** or **stale cache hit**. While technically the cache has a value for the requested key, the inconsistency renders it invalid from a correctness standpoint, and the system has to fetch the correct, updated data from the primary storage. This process can lead to similar performance overheads as a traditional cache miss.

#### Mitigating Stale Cache Hits
1. **Cache Invalidation**: Implement mechanisms to invalidate or update cached data when the underlying data changes.
2. **TTL (Time-to-Live)**: Use TTL values to ensure that cached data is refreshed periodically.
3. **Write-Through/Write-Behind Caching**: Ensure that updates to the primary storage are also reflected in the cache.
4. **Event-Driven Invalidation**: Use events to invalidate or update cache entries when changes occur in the primary storage.

### Q9: How will the system know the data is invalid?
There are several mechanisms that can be used to detect and handle invalid or stale data in the cache:

### 1. **Time-to-Live (TTL)**
- Each cached item is assigned a TTL value, which specifies how long the item remains valid. Once the TTL expires, the cache marks the item as invalid, forcing a fresh retrieval from the primary storage.

### 2. **Cache Invalidation Policies**
- **Write-Through Caching**: Any update to the primary storage immediately updates the cache.
- **Write-Behind Caching**: Updates are queued and written to the cache and primary storage in the background, ensuring consistency.
- **Explicit Invalidation**: Applications explicitly invalidate cache entries when underlying data changes.

### 3. **Event-Driven Invalidation**
- Use events or messages (e.g., from a message queue) to notify the cache system when data changes in the primary storage, prompting it to invalidate or update the relevant cache entries.

### 4. **Database Change Notification**
- Some databases support notifications or triggers that can alert the caching layer when changes occur. This can be used to synchronize the cache with the primary storage.

### 5. **Versioning**
- Each piece of data can have a version number or timestamp. When a request is made, the cache can check the version or timestamp against the primary storage. If they don't match, the cache data is considered invalid.

### 6. **Consistency Checks**
- Periodically, the system can perform consistency checks between the cache and primary storage to ensure data integrity. Any discrepancies found can prompt invalidation or updates.

### Example Implementation
Here’s a simple example using TTL and explicit invalidation:

```python
# Pseudo-code for cache with TTL and explicit invalidation

cache = {}

def set_cache(key, value, ttl):
    expiration = current_time + ttl
    cache[key] = (value, expiration)

def get_cache(key):
    if key in cache:
        value, expiration = cache[key]
        if current_time <= expiration:
            return value  # Cache hit
        else:
            del cache[key]  # TTL expired, remove entry
    return None  # Cache miss

def invalidate_cache(key):
    if key in cache:
        del cache[key]

# Usage example
set_cache("item1", "data", ttl=3600)  # Cache "data" with a TTL of 3600 seconds
data = get_cache("item1")
invalidate_cache("item1")
```
