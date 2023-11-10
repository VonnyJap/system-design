#### Requirements
Functional:
- able to build the code
- able to deploy the code
Non-functional:
- 5-10 regions
- 100k machines
- availability: 2-3 nines
- 30 mins entire deploy
- 1000s deploy/day
- 10GB per binary
- 1 worker per day = 48 jobs

#### Build code
- Queue system can be a SQL table (if needs to be persisted) - ACID
  - jobs table
    - id
    - name
    - sha
    - timestamp
    - status
    - worker
  - using transaction for concurrency
  - healthcheck
- workers that takes the jobs to deploy the code
- binary can be stored as blob store -> artifactory

#### Deploy code
- setup the auth
- give permission for the pipeline to access the server via SSH
- setup the recipe
- build inventory of hosts
- run the recipe
  - test env
  - stg env
  - prod env
