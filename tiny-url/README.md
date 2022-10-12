### Requirements
  - How short does it need to be?
  - cname kind of thing?
  - we need to store it in db
  - this is like creating a http redirect?
  - do we need to keep history?
  - do we need to rename?

### Functional Requirements
  - long -> short
  - redirect
  - option to choose the alias
  - timespan
  - option to delete? rename?

### Non functional
  - low latency
  - highly available
  - Shortened links should not be guessable

### System APIs
  - create_url
    - auth
    - original url
    - alias
    - expire
  - delete_url
    - auth
    - url

### Prevent system abuse
  - rate limiting

### What about the algo to generate the key?
  - md5 and sha256

### Caching
  - we can use memcached and use the tiny url as the key and the long url as value

### Load balancer
- Between Clients and Application servers
- Between Application Servers and database servers
- Between Application Servers and Cache servers

[Designing a URL Shortening service like TinyURL](https://www.educative.io/courses/grokking-the-system-design-interview/m2ygV4E81AR) 
