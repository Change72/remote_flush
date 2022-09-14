# Setup Documentation for ASU-IDI

## Purpose
This repository is for documenting the different installations or implementations that have been performed in the ASU-IDI lab. The documents can vary from being installation guides to being usage or even maintainince guides. A more detailed document is always preferred, and in the future a standard template for all standard documents will be linked right here.

# Table of Contents:

+ [root](./)
    + [HDFS](./HDFS/)
        + [Full-Distirbuted Setup](./HDFS/HDFS%20Full-Distributed%20Setup.md)
        + [Pseudo-Distirbuted Setup](./HDFS/HDFS%20Pseudo-Distributed%20Setup.md)
    + [RocksDB](./RocksDB/)
        + [RocksDB Docker Container](./RocksDB/Rocksdb-Docker.md)
        + [RocksDB (HDFS Plugin)](./RocksDB/Rocksdb-HDFS%20Installation.md)


## Contributing Guidelines
Every documentation will have a parent folder that will classify it into its respective application. For example, the RocksDB folder can detail on the installation of rocksdb with different plugins or with java. The HDFS folder can have the installation and usage of HDFS instances and systems. 

An example tree structure of the above:
```
Root
    |- RocksDB
    |   |- RocksDB Installation Guide
    |   |- RocksDB-HDFS Installation and Usage Guide
    |   -- RocksJava Guide
    |- HDFS
        |- HDFS Pseudo Distributed setup
        |- HDFS Usage guide
        -- HDFS Fully Distributed guide
```

There is a template made available to facilitate easier creation of documents that also does not skip any useful information! The format is quite flexible and you can use the sections that are most relevant to you. But do attempt to keep by the template as much as possible.

## Repository Plan
While in the early stages of this repository, the primary concern will be to set up a proper and easy to use template for everyone to be able to use. The final goal is to have everything in Github Wiki rather than having work documents alone. An option with goog inter-operatibility is required. The current path for the repository is:

1. Setup of Github Wiki
2. Template formation (Word and Markdown)
3. Uploading of initial documents
