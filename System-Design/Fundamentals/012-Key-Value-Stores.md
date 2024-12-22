## What is a key value store?
A key-value store is a flexible NoSQL Database that is often used for caching and dynamic configuration.

### In what case does an SQL DB become a problem to use.
When the rigidity becomes an issue with how flexible the app data should be stored.

### Why are they fast to access? 
With  key-value pairs we do not need to filter through as table tpo get the information we are looking for. We can simply retrieve them using the specific key.
Types of key value stores => dynamoDB, Etcd, MongoDb, Redis, ZooKeeper