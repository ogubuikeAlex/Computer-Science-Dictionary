### What is the BlobStore?
```BLOB => Binary Large Object.```
When we use the word blob, we are most likely talking about an arbitrary amount of unstructured data. 
BlobStore is a storage solution that is optimized to hold really large amount (massive) of unstructured data. A blob does not make sense to be stored in a tabular form. 
Types of BlobStore solution => Google Cloud Store and S3.
BlobStores are optimized to store and receive really large amount of unstructured data.
They behave like key value stores, but the difference is what they are optimized for. For example, a BlobStore might be more optimized towards availability and durability of data while a key value store would be more optimized for latency. And a key value store might also not be able to hold the amount of data a blobStore can hold
What is the difference between blobStore and key value store

## What is the Time series Db?
This is a database that is specialized for storing time-based or time indexed data for example events that happen at a given time. When we have a use case where we need to perform some time based analysis or operation like logging and monitoring. Another example is capturing telemetry data from IOT machines, Monitoring events that happen in your system, Monitoring crypto price changes etc
Types of time series DBs => InfluxDB, Graphite and Prometheus

## What is the Graph DB?
This serves as a nice bridge between algorithm, data structure and databases. ( Graph Data Structure is a collection of nodes connected by edges. It's used to represent relationships between different entities.)
### What to keep in mind 
A relational database holds data in a tabular format and it makes sense for a lot of data. But what if we are storing data that has a lot of relationships between the data points. 
While this can be represented in relational databases, we should keep two things in mind
- The first is that we are looking for trouble because things will get really complicated really fast
- The relational database has relationships between tables but is not  optimized for relationships that are more granular like relationship between certain properties in one table  and properties in another table

The graph database is one that is built on top of the graph data model. in the graph database the concept of relationships is central. Even if you think about it - Tables are data structures that store info in rows and columns and to get relationships we basically have to enforce it and when the relationships become too complex like having many-to-many-to-many relationships we would always encounter difficulties, a graph The concept while a graph is a data structure that shows nodes and the literal connection or relationships between them. Imagine writing an SQL query for a company that has a network of social networks (facebook, IG and whatsapp) and have relationships in each of the social networks and also across each of these social networks - Crazy!
This why the graph database is very important

## What is a Spatial DB
Are DBS that are optimized for storing and querying spatial data like locations on a map. A special DB usually has a spatial index which is used to perform quick spatial queries.

## What is a QuadTree?
This is a tree data structure that is used to index tw0-dimensional spatial data. Each node in a quadTree has either zero children nodes or exactly four children nodes. 

A typical node in a QuadTree in the context of a spatial DB will have a value N, considered to be maximum capacity. As long as the the amount of spatial data in that node is below the maximum capacity, then it will be a leaf node, one it is equal or over the maximum capacity then it will be given four children nodes and its data entry is split across all four child nodes.

A quadTree lends itself well to storing spatial data because it can be represented as a grid filled with rectangles that are recursively subdivided into four sub-rectangles, where each quadTree node is represented by a rectangle and each rectangle represents a spatial region.

We can imagine the quadTree as follows
- The root node, which represents the entire world is the outermost rectangle.
- If the world has n locations, the outermost rectangle is divided into four quadrants each representing a region of the world
- So long as a region has more than n locations, the outermost rectangle is divided into four quadrants (The corresponding node in the quadTree is given four children nodes)
- Regions that have fewer than n locations are undivided rectangles (leaf nodes)
- The part of the grid that have many subdivided rectangles represent densely populated areas while the part of the grid that have few subdivided rectangles represent sparsely populated areas like rural areas
