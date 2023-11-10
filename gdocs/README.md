#### Functional Requirements
- Users can create file
- Users can edit permission
- Users can delete file
- Multiple users can edit file
- Multiple users can view/comment file

#### Non functional
- Availability
- Performance/Low latency
- Scale
- Consistency

#### Building components
- LB
- Pubsub
- Queue system
- Blob storage
  - for videos/images
- DB
  - relational for user data, file metadata, permission
  - comments can be stored in NoSQL for better performance
- Cache: Redis
  - user sessions
  - frequently accessed docs
  - typehead services
- CDN

#### Sync strategies
- event passing -> operational transformation
  - The client can send raw events (operations) character by character to the server (like character M inserted at position 3, character O deleted from position 10 etc.). This is also called operational based sync.
- differential sync
  - The client can send the delta (difference) between server version and local copy. The server will not know of the exact operation performed at the client in this case.


#### HLD
