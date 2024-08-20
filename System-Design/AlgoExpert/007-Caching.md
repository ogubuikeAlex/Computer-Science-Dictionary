## What Is Caching?
Caching is the process of storing data in an alternative
store to reduce latency or improve a system

## Where can we use caching
- If we have a system where there is a lot of network request for 
the same information. We can implement a cache that will store the information and reduce the amount
of requests to the server or DB
- When we want to prevent a computationally expensive operation from being repeated over and over again.
We can simply cache the result so that when they is a request for it the operation is not repeated.
- Caching is not always about improving the availability of information, sometimes it could be as way to prevent
some other bottle necks from arising. For example, if we have a system where our server has a low throughput and we 
are not buoyant enough to manage that. We could incorporate caching to ensure that the amount of requests that gets to
our server does not overwhelm the server

## Types OF Caching
There are two major types of caching
- Write Through:
This is a type of caching where during write operations we update the cache and the main database simultaneously. With this technique
it is easier to assure that we have coherent data. One of the issue with it is that write operations would typically take more time and might even increase latency
- Write Back:
In this type of caching we only update the cache during write operations and then we set up a mechanism that periodically updates the main database. While this is faster, it means that the main data store will mostly be out of sync with data changes. As much as we have a mechanism to periodically update the main data store, it would be a big issue if something happens to the cache before the date is moved to the main data store

## What Is Staleness In Caching
Staleness is a situation where the data retrieved from a cache is not the most updated version stored on the main data store.
This can happen if there is an interruption in the sync process between the cache and the data store or we do not have a proper way to revalidate or invalidate the cache data.

## Rule Of Thumb Of Caching
Caching is super important but can also be dicey. If you must use caching, then:
- Use it for retrieving static or immutable data that will most probably not change in a long while
- Use caching if you have only one single part of the system reading and writing that data.
- Understand that when you have multiple items reading and updating a data store, it becomes more complex - so more care and intentionality has to go into designing the caching system. 
- Understand the needs of the system. Is there any part where stale data will affect the users negatively and automatically also affect the product. If the system is not very heavy on consistent data then it is fine. Else, put in place revalidation and invalidation mechanisms

## Eviction Policies
There are various techniques involved in removing data. The technique you use will be determined largely by the system you are designing:
- LRU: Least Recently Used Policy - When removing items from the cache, remove the data that has not been used recently
- LFU: Least frequently Used Policy - When removing items from the cache, remove the data that was least frequently used by the system
- FIFO: First In First Out Policy - When removing items from the cache, the first item we stored should be removed first
- LIFO: Last In First Out Policy - When removing items from the cache, the last item we stored should be removed first
- Randomly -> 🫵🏽

- What is a cache miss and a cache hit?
Cache Miss Is When Requested Data is found in the Cache

Cache miss is when requested data could have been found in the cache but wasn't found. This is typically caused by by a system failure or a poor design choice. For example, if the server goes down, our load balancer wll have to forward requests to a new server which will result in cache misses