# system-design
Compilation of System Design 

### approach
- A detailed discussion about the requirements and which all stuffs our system supports and which all stuff we can ignore
- After the requirement, it’s better to give the audience a fair idea about the estimation of how many people going to use the system - scope the system
- check whether to start with HLD or API
- Discuss the API (REST API) Involve in the System through which users access our service
- Try to draw End to end the flow of High-level Design of the design
- Think and come up with system components involved in the system and also the flow between the system components
- discuss trade-offs
- In the end, come up with a Low-level design of the system

### Free Reference
- https://github.com/donnemartin/system-design-primer
- https://www.techinterviewhandbook.org/system-design/

### Consistency Models
- eventual
- causal
- sequential
- strict

### CAP theorem
- Consistency, Availability, Partition Tolerance
- a distributed database system can only guarantee two out of these three characteristics: Consistency, Availability, and Partition Tolerance.
- A system is said to be consistent if all nodes see the same data at the same time. Simply, if we perform a read operation on a consistent system, it should return the value of the most recent write operation.
- Availability in a distributed system ensures that the system remains operational 100% of the time. Every request gets a (non-error) response regardless of the individual state of a node. Note: this does not guarantee that the response contains the most recent write.
- Partition tolerance means that the cluster continues to function even if there is a "partition" (communication break) between two nodes (both nodes are up, but can't communicate). In order to get both availability and partition tolerance, you have to give up consistency. Consider if you have two nodes, X and Y, in a master-master setup. Now, there is a break between network communication between X and Y, so they can't sync updates. At this point you can either:
  - Allow the nodes to get out of sync (giving up consistency), or
  - Consider the cluster to be "down" (giving up availability)

### Back of envelope calculations
- number of concurrent tcp conns
- number of requests per second that a webservice, db, cache server can handle
- storage requirements

### Types of DC servers
- web server: low RAM, high CPU, low HDD
- application server: high RAM, medium CPU and HDD
- storage server: low RAM, medium CPU, high HDD

### Storage servers
- blob storage: for its encoded videos
- queue storage: content to be uploaded
- specialized storage: for thumbnails (static views of video for branding)
- relational db: for comments, likes, users

### Request Estimation
- CPU bound
  - num of threads (in a machine) * (1/task time)
- Memory bound
  - (ram size/worker memory) * (1/task time)

### Estimate number of servers
- DAU - daily active users
- Requests per day per user
- total requests per day = DAU * Requests per day per user
- total requests per sec
- total servers - estimates that one server can handle 8000 rqps

### Storage requirements
- DAU + requests/user
- total requests per day
- storage requirement for each activities: tweet, video, image
- storage per day: add storage requirements for each activities
- storage per year

### Bandwidth requirements
- number of incoming data/number of seconds per day
- number of outgoing data/number of seconds per day

### DNS servers
- Name servers
- Resource records

#### Steps
- Requirements
- Resource estimates
  - Traffic
  - Storage
  - Bandwidth
  - Memory for cache
- API design

#### References
[The Design Patterns for Distributed Systems Handbook – Key Concepts Every Developer Should Know](https://www.freecodecamp.org/news/design-patterns-for-distributed-systems/)
