#### DNS

#### Stateful versus stateless LBs
- Stateful
  - info stored in the lb
- Stateless
  - info stored in the cache or db
- Info needed will be the client vs services info, which requests go where

#### Database
- SQL (relations)
  - ACID
    - Atomicity: all the statements within a transaction will successfully execute, or none of them will execute.
    - Consistency
    - Isolation: In the case of multiple transactions running concurrently, they shouldn’t be affected by each other.
    - Durability
- NoSQL (like file directories)
  - Benefits:
    - Simple design: storing one employee data in a single document rather than in multiple tables that require joining the data
    - Horizontal scaling: can be run in large cluster, data can be spread accross multiple nodes
    - unstructure or semistructured data like JSON, XML
  - Types:
    - Key-value: DynamoDB, redis, memcached (like hash tables)
      - good for session oriented apps like web apps
    - Document: MongoDB
      - like JSON, XML
    - Column: Cassandra
    - Graph: NeoDB
      - normally useful in social media apps
  - drawbacks:
    - no consistency
    - lack of standardization

### DB partitioning
- Sharding
  - Vertical
    - employee table can be divided into a main table and a table just containing employee ID and picture
  - Horizontal
    - key range based
    - hash based
- Service discovery
  - Zookeeper
    - keep track changes in the cluster
    - keep track mappings in the network


### Load balancer
- for example use consistent hashing to determine which server the users need to be sent to
- use geolocation
