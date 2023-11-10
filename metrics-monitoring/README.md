## Design Monitoring and Alerting System

- Data collection
- Data transmission
- Data storage
- Alerting
- Visualization

### Data model
- metric name: CPU, memory
- labels: node ID, environment
- timestamp
- value

### Data storage
- time series data needed but difficult with relational db

### HLD - 1
- The node needs monitoring can be registered to the system
- Once the system is registered then the first initialization is to deploy an agent that can monitor the health in this node (this is a push model), the collection agent is a long running process
- This agent can then report to the monitoring system via API about health of the node
- This system also needs to have the API that reports the alerting when hitting certain treshold
- Treshold is set by the client at the first registration

### HLD - 2
- Metrics source: application servers, message queues, SQL db
- Metrics collector: gather metrics data and writes to timeseries db
- Timeseries db
- Query service: thin wrapper
- Alerting system
- Visualization system

#### How does Promotheus work?
- Data Retrieval worker that takes data from application and services
  - units of measurement is called metrics
- Storage: that stores metrics data
- HTTP server: that accepts queries from UI/Dashboard like Grafana

#### Collecting metrics from targets
- data retrieval worker pulls over HTTP from /metrics endpoint
- must be in correct format
- exporter needs to be installed to convert metrics to something that promotheus understands
- configure promotheus to scrape this endpoint
- client library to expose data to certain endpoint
- pull mechanism
- pushgateway
- service discovery to find the targets to scrape

#### Configuring
- rules
- jobs
- interval

#### Tasks
- Look at how promotheus is configured
- Implement this using an application or kubernetes cluster

#### Consistent hashing
- based on the host IP assigns a range of hash
- hence when new host is added only a part of hash range needs to be recalculated
