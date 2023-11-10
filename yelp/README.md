#### Functional Requirements
- users can create account
- can search business (by lat and long)
- give ratings/comments
- upload images

#### Nonfunctional
- scalability
- high availability
- performance

#### Estimation
- 1MB for each location
- 1MB x total locations x replication


#### db
- elastic search

#### find the nearest place
- spatial search problem
- divide into smaller boxes with four coordinates bound
- and trying to find which box that the user is
- and then draw a circle from there
- and then find squares overlapping with the circle
- so maybe the cache can be by the box and the box can indicate the businesses it has
- so business is associated with the box

#### Ref
[Video](https://www.youtube.com/watch?v=TCP5iPy8xqo)