#### References
[A guide to understanding database scaling patterns](https://www.freecodecamp.org/news/understanding-database-scaling-patterns/)
[How to design a system to scale to your first 100 million users](https://levelup.gitconnected.com/how-to-design-a-system-to-scale-to-your-first-100-million-users-4450a2f9703d)
[Design a Chat Application](https://towardsdatascience.com/ace-the-system-interview-design-a-chat-application-3f34fd5b85d0)

#### SQL
- Relational
- designed for fixed schemas
- highly structured rows and tables
- ACID: Atomicity, Consistency, Isolation, and Durability
- Query with SQL
- Transactional
- difficult to scale

#### NoSQL
- Nonrelational
- for performance but losing features
- hierarchical JSON structures: MongoDB
- wide column databases
- semistructured
- sparse multidimensional matrix
- related data stored in single row
- graph db

#### Analytical
- datawarehouse
- structured
- may use SQL but not relational
- google big data query

#### Improve performance
- Caching
- R/W replicas
- time-based, list-based, hash-based partition

#### Processing
- time-series can use buffer
- user data can be uploaded to API and directly worked with db

#### Human Scale
- backoffice
- user app and web app (a lot of queries per seconds for popular app)
- more read than write
- part of a workflow
- data persisted as it is ingested
- sync processing

#### Machine Scale
- IOT
- geolocation
- application and infra monitoring
- credit card processing
- use buffer or queue
- decoupled from the applications
- spikes in ingestion do not trigger spikes in processing
- process data before persisting

#### Machine Scale Processing
- db is the last component of ingestion pipeline
- devices send data to endpoint
- endpoint validates data and writes to a queue
- application subscribes to the queue
- application pulls from queue to maintain control

#### Message Queues
- decouple services
- able to write data with low latency
- apache kafka and google cloud pub/sub

#### Event Sourcing
- ACID prevents scaling
- Events data read and used to create materialized views
- Materialized views capture current state


#### Transactional
- target small num of rows
- many cols
- row orientation model
- use of indexes with where clause
- normalized so complex joins
- index help reducing latency: jump to middle

#### Analytical
- Column orientation
- Small number of rows
- Large num of rows
- Liberal use of indexes
- denormalized and no complex joins

#### Materialized views
- a little bit like caching

#### WAL file
- sync and async

With message queue: i think the problem is more like
what if the message failed to be written to db?
said a client write async - when acknowledged by a service then - ok 
how to ensure that this is written to db?

As long as the data is not stored or processed, it will be persisted in queue
hence minimizing the lost of data

#### MySQL vs NoSQL
- MySQL respects ACID hence highly consistent
- NoSQL for big data/real-time webapp and faster performance and scalability - eventually consistent
- MySQL
  - relational
  - table
  - tough for big data
  - PK and FK
  - SQL
- NoSQL
  - non relational
  - document
  - easy to scale
  - unique key concept
  - no standard query language: REST based retrieval
  - types:
    - key value: memcached, redis
    - doc based: mongodb, couchdb
    - column based: BigTable, Hbase, Cassandra
    - graph based: Neo4J, InfoGrid

#### Redis
- NoSQL
- supports key/value, list, hash
- pub-sub which is useful for real time comms, i.e. chatroom, social media newsfeed

#### CDN
- normally for static contents only


#### Replication: use kafka to queue statements
- Statement based: writing all statements that got executed to secondary nodes
- WAL (write ahead log) shipping approach
- Row based replication

#### Limitation
- Replication lag due to async process
- High intensive of write
- Eventual consistency

#### Solutions
- Critical read from leader database
- Overprovision kafka during peak traffic
- Design the system to accept the replication lag

#### Leader election
- using a lock object where all replicas writing to inform its current state
- when the current leader crashes, another replica can be appointed as leader
- only one replica can acquire the lock each time
- this is also how the LB working in a way by checking the health
- hosts sending heartbeat

#### Leaderless application
- better availability
- mostly for non relational db
- write client writes to everything - wait only for some nodes to ACK
- read client asks from all nodes and use some logics to send most recent data to the user
- repair
  - read repair during the read request
  - or do a background process
- drawback
  - less consistency

#### Database sharding
- optimization technique
- split into smaller tables
- types
  - horizontal: split rows
    - key-range based sharding
    - hash based sharding
  - vertical: split cols
- need master table or predictable hash to let client knows which shards
  - routing layer: it can be REST
  - zookeeper to maintain mappings in a network
    - each node connects to zookeeper for information
    - zookeeper will inform routing layer when partition is changed, a node is removed or added
- pros:
  - scalability: shards can be recursively chunked
  - availability and fault tolerance
- cons:
  - complexity
    - partition mapping
    - routing layer
    - non-uniformity
  - not suited for analytical
  - rebalancing can be tricky

#### Secondary index in db sharding

#### Eventual Consistency
[Article](https://levelup.gitconnected.com/eventual-consistency-what-how-and-why-50c942472a0c)
- For distributed system, though RDBMS, still data can be inconsistent if multiple writes available.
  - This can be tackled with sync write, when write happens, then the requests only done upon all writes updated
  - when the node is synced or updated, read/write can be locked hence ensuring ACID
    - this is different from eventual/strong consistent over nodes
    - because this is related more to transactions consistency
    - [Eventual Consistency](https://www.keboola.com/blog/eventual-consistency)
- CAP theorem - CP or AP only

#### Strong consistent types of transactions
- banking
- shopping carts
- order processing

#### Eventual consistent
- fb feeds, likes, recommendations

#### ACID
- Atomic: ensuring that when a transaction failed, all steps failed or rolled back
- Consistent: ensuring the integrity of pk and fk
- Isolation: database transactions are capable of being concurrently processed with no conflicts. If we need to ensure race condition protection, then we can secure with locks.
- Durability: data transactions ensure that the data is saved.

#### Consistent hashing in db management or sharding?
- for sharding to work, there will be a node manager which tells which node manages which key and that node manager is probably the only thing that the client knows and it needs to make the request to the node in particular

### WAL
[Video](https://www.youtube.com/watch?v=wI4hKwl1Cn4)
- Using log sequence and byte offset to read or write (can be implemented as well for monitoring log or streaming log)
- Writing logs of query (append only)
- Using crc for integrity

#### Time series db
- basically just a NoSQL database but optimized to be used for querying using timestamp index
- samples:
  - Promotheus
  - influxdb
  - elasticsearch

#### Monarch
- Ingestion: target
- Leaf: in-memory db
- Recovery logs

What about doing ELB monitoring using Promotheus?

