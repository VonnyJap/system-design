### Instagram Feed

#### Functional Requirements
- user can create account
- user can follow others
- user can upload/view/update/delete photos
- user can view photos of others that the user follows
- user can like/comment the post
- publish newsfeed

#### Non-functional
- availability
- performance
- a bit of inconsistent is ok
- durability? photos uploaded are not missing?

#### API design
- createAccount(username) -> id
- followAccount(username, anotherAccount)
- uploadPhoto(username, photoName)
- viewPhoto(username, photoID) -> for feed we need only the last photo
- getPhotoMetadata(photoID)

#### Workflow
- user is logged in after authentication
- correlation between user accounts and the following accounts last updated info (this can be stored in Redis) for high performance?
- photos of following accounts with likes and comments are shown on the user feed

#### Design components
- Load balancer
- API/front-end server
  - SSL
  - auth
  - rate limiting
- Application server
  - this layer can be broken down further into each type of works it is responsible for
  - talk to a redis server that gets information like (this can be acting like a cache layer as well)
    - last logged in
    - following accounts that just got updated between last loggen in and now
    - most recent updates of those following accounts
    - get number of likes and comments for a specific photo
  - talk to MySQL db for user accounts relations
  - talk to a nodename for specific blob and get the photo file
    - nodename might be backed up by other metadata about the file
- CDN
- Queue for sending notifications of feeds
- for celebrity that has many followers: we can do pull, normall user can utilize push

### Design Newsfeed
https://liuzhenglaichn.gitbook.io/system-design/news-feed/design-a-news-feed-system
https://github.com/GetStream/mongodb-activity-feed

https://levelup.gitconnected.com/how-to-build-news-aggregator-website-using-vue-and-golang-a48999001238

### Ranking
https://github.com/donnemartin/system-design-primer/tree/master/solutions/system_design/sales_rank
https://engineering.fb.com/2021/01/26/ml-applications/news-feed-ranking/


### Todo List App
https://medium.com/@diogo.fg.pinheiro/simple-to-do-list-app-with-node-js-and-mongodb-chapter-2-3780a1c5b039

#### Pubsub
https://ably.com/blog/event-streaming-with-redis-and-golang


#### Log streaming or metrics


https://www.meziantou.net/creating-a-search-engine-for-websites-using-elasticsearch-and-playwright.htm

https://liuzhenglaichn.gitbook.io/system-design/news-feed/design-a-news-feed-system
