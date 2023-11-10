# Distributed Logging

#### Requirements
- Centralized logging capability
- Centralized log tracing
- Generate reports to get specific type of error: metrics
- Alert mechanism based on error spotted in log
- log rotate

#### What to log
- some samples of data
  - it might not work when we need a flow of data like financial transactions
- error logs
- access logs

#### Use log levels
- DEBUG
- INFO
- WARNING
- ERROR
- FATAL/CRITICAL

#### Building blocks
- pub-sub system
- distributed search
  - log collecter (like crawler)
  - blob storage
  - log indexer (like indexer)
- visualizer

#### API design
- write(unique_ID, message_to_be_logged)
- search(keyword)

#### HLD


### Security
- Note: For applications like banking and financial apps, the logs must be very secure so hackers cannot steal the data. The common practice is to encrypt the data and log. In this way, no one can decrypt the encrypted information using the data from logs.

### To prevent logs missing
- due sync update to tell whether or not it has been successfully written
- write to disks for permanent storage
- how does file watcher working?
  - based on splunk
    - The Splunk platform file system change monitor tracks changes in your file system. The monitor watches a directory you specify and generates an event when that directory undergoes a change. It can detect when a file on the system is edited, deleted, or added. It detects changes on any file, including files that are not Splunk platform-specific files.
  - The file system change monitor detects changes on the *nix file system by using the following attributes:
    - modification date/time
    - group ID
    - user ID
    - file mode (read/write attributes, etc.)
    - an optional Secure Hash Algorithm-256 (SHA256) hash of file contents
  - logrotate uses rename-recreate method as recommended by splunk to avoid missing log entries. renaming the file doesn't affect the file descriptor splunk is using to read the file, so it shouldn't miss any lines


