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

