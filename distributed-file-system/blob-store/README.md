### Blob Store

#### API design
- createContainer(containerName)
- putBlob(containerPath, blobName, data)
- getBlob(blobPath)
- deleteBlob(blobPath)
- listBlobs(containerPath)
- deleteContainer(containerPath)
- listContainers(accountID)

#### Components

- Load balancer
- API server
  - SSL
  - auth
- Rate limiter
- Application manager -> decide which server to go to and which file servers?
- MySQL
  - account info
  - metadata info

#### Garbage Collection
- When a DELETE request is called by user, the blob is not immediately removed, instead it will be marked as "DELETED" and the deletion process happens async. This is done to increase performance.

#### Availability
- parallel fetching of chunks
- Also it is not necessary that a single data node would contain every chunk of a particular file.

#### Consistency
- We synchronously replicate the disk data blocks inside a storage cluster upon a write request, making the data strongly consistent inside the storage cluster. This is done on the user’s critical path. We serve the subsequent read requests on this data from the same storage cluster until we haven’t replicated this data in another data center and or on other storage clusters.
- After responding to the write request and replicating data within the storage cluster, we asynchronously replicate blobs in the data centers placed far away or in other regions to ensure availability.

#### Composition
- Container
- Blob
- Chunks

#### Methods of reading/writing
- Master node tells which data node to read
- And sometimes we need to read/write by chunks in several sectors (most programming languages support this)
  - [Go implementation](https://kgrz.io/reading-files-in-go-an-overview.html#reading-byte-wise)

#### Protocols
- As the client communicates with the NameNode, DataNodes, as the DataNodes whisper to each other, what is the protocol of talking? We are using plain and simple RPC (Remote Procedural Calls) here in this case.