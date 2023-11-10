#### Resource Estimation
- For bandwidth
  - number of requests per sec * bytes per request * 8 bits/sec

https://www.youtube.com/watch?v=CeGtqouT8eA

#### Database
- Elasticsearch db

#### Mapreduce
- document partitioning
- term partitioning


#### Steps to design search engine
- Need websites to crawl and then store the contents as blob storage
  - A lot of open source projects for web crawler in python
- Indexing/Map Reduce jobs
- Storing indexing table (table+mapping) into elastic search
- When query comes, each query term is assigned with one partition and then the result is merged after that
- ES comes with an analyzer to remove stop words, etc.

[Build Search Engine](https://blog.devgenius.io/building-your-own-search-engine-from-scratch-e542a1068c44)

[Build Search Engine from scratch](https://medium.com/analytics-vidhya/building-a-basic-search-engine-using-elasticsearch-fscrawler-97104c1ea220)


[FS crawler](https://github.com/dadoonet/fscrawler)