<!-- Document under construction - needs a lot of work -->

# RocksDB-Docker: An Installation and Usage guide

This document is to show how to use the RocksDB container present at [veedata/docker-rocksdb](https://github.com/veedata/docker-rocksdb). 

> The repository will be renamed in the future or the code in the repository updated but the following steps remain the same unless specified.

---

## Table of Contents

+ [parent (rocksdb)](./../RocksDB/)

----

<!-- Maybe a pre-requisite section will be really helpful - so, if someone needs to know a concept, just link it in the pre-req section and they can read up on it -->
<!-- For example: the pre-reqs for this will be - basic working knowledge fo docker commands, what is containerization, what is rocksdb -->


<!-- Section to be removed in the future -->
## Installation
There are 2 methods of installing this particular container. The first being the docker-hub method where you can directly grab a built image or you can build the image yourself on your local machine. In the future there might be a different document that shows on how to build your own docker image, but I feel that Medium/Youtube has you covered - maybe this will go in a different section of recommended reading or pre-requisites! Can be as footnotes as well.

The repository [veedata/docker-rocksdb](https://github.com/veedata/docker-rocksdb) was made to serve a single purpose and hence is not at all flexible in its current state and only offers very basic functionalities. This shoiuld change in the future wiwth my growing knowdlege of containers and docker commands. But for the time being, the repository will create a table (hard-coded path). There are again 2 options to create tables - docker-rocksdb-primary (to create a primary db that can perform `put`, `delete`, and `update`) and docker-rocksdb-secondary (to create a secondary db that can perform `get` and `scan`)

> It is highly recommended not to use this repository unless you know exactly what you are doing. A redesign is coming for the repository that makes it more generalised for more use cases, but in the meanwhile, avoid usage. This not will be removed the the said redesign arrives. 

> The following steps are only meant for rocksdb-primary docker images since the secondary images exist but are severly hard-coded for my specific use case. In case you would like to update the same to suit your use case, you can find them at: [docker-rocksdb-secondary](https://github.com/veedata/docker-rocksdb/tree/secondary-db) 

### Docker Hub
This is the more striaghtforward way to dob it, although you do lose a lot of flexibility
1. `docker run -p 36728:36728/tcp -p 9090:9090/tcp veedata/rocksdb-hdfs-primarydb`

### Building locally
<!-- To Do -->
The better option, although it is more time and resource consuming
1. `git clone https://github.com/veedata/docker-rocksdb`
2. `git checkout -b primary-db`
3. `cd docker-rocksdb`
4. `docker build -t veedata/rocksdb-hdfs-primarydb .`
5. `docker run -p 36728:36728/tcp -p 9090:9090/tcp veedata/rocksdb-hdfs-primarydb`

---

## References

<!-- To Do -->

---

## Contributors
1. Viraj Thakkar [[github.com/veedata](https://github.com/veedata)] [[viraj.online@asu.edu](mailto:viraj.online@asu.edu)]