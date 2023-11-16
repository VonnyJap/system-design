### Caching Services
- It is a nonpersistent storage area used to keep repeatedly read and written data
- Need to be served from storage within the machine
- Distributed cache is a caching system where multiple cache servers coordinate to store frequently accessed data
- locality of reference principle
- caching is expensive, it is based on RAM because it needs to be fast

### How Distributed Cache Works 
- using hashtable to map object values and keys
- based on LRU
- doubly linked list
- also needs to consider invalidation - c_time + duration

### Methods
- read-through
  - pro: The data is written into the cache and the corresponding database at the same time. This scheme maintains the complete data consistency between the cache and the main storage. Even this scheme also ensures that nothing will get lost in case of a crash, power failure, or any other system disruptions.
  - cons: The disadvantage of this scheme is the Higher Latency, since every write operation is done twice before returning success to the client.
- Write-Through
  - In this strategy, every information directly written to the database just bypassing the cache.
  - cons: cache miss
- write back
  - The data is written to cache only and completion is immediately confirmed to the client. The write to the permanent storage is done after specified intervals or under certain conditions. This results in low latency and high throughput for write-intensive applications.

### Zookeeper
- registry for large distributed systems. 
- It is beneficial for tasks like master election, crash detection and managing meta data related to distributed systems.

### Cache expired
- normally for write request, we should perform a write back to avoid inconsistency

### Memcached vs Redis
- Pros and cons

### Eviction policy
- LRU (Least Recently Used)
- LFU (Least Frequently Used)
- MRU (Most Recently Used)


