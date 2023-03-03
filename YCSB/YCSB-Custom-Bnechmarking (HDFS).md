# Benchmarking Custom RocksDB on YCSB

Make your Custom RocksDB code run with YCSB while using HDFS. This document goes over the same in some detail.

---

## Table of Contents

+ [parent (ycsb)](./../YCSB/)

---

## About 

High-level Software Version:
+   RocksDB:    Custom Code
+   HDFS:       v3.3.1

---

## Setup

Make sure you have completed setting up [YCSB-cpp](https://github.com/ls4154/YCSB-cpp) and at least run a single benchmark on the RocksDB version that you are modifying (document coming soon).

Once, you have done this, go to the usage and follow the steps there.

---

## Usage

> This is all in C++, so, make sure that your modified version of RocksDB works with C++. If not, you might be better suited to the adding a new database to YCSB (tutorial coming soon)

1. Go to your Custom RocksDB folder and build the static lib for it: `make clean && make -j8 static_lib`
2. Go to your YCSB-cpp folder, make the following changes.

    `Makefile`
    ```diff
    - BIND_ROCKSDB ?= 0
    + BIND_ROCKSDB ?= 1

    ----------------

    - EXTRA_CXXFLAGS ?=
    - EXTRA_LDFLAGS ?=
    + EXTRA_CXXFLAGS ?= -I${RHDFS_PATH}/include -I${HADOOP_HOME}/include -I${RHDFS_PATH}/plugin/hdfs
    + EXTRA_LDFLAGS ?= -L${RHDFS_PATH} -L${HADOOP_HOME}/lib/native -lhdfs -ldl -lz -lsnappy -lzstd -lbz2 -llz4
    ```

    > Note: HADOOP_HOME refers to the path where your Hadoop installation is and RHDFS_PATH is to where the rocksdb folder is present

    `rocksdb/rocksdb_dc.cc`
    ```diff
    + // HDFS Libraries
    + #include <env_hdfs.h>
    + #include "hdfs.h"

    ----------------

    - const std::string PROP_FS_URI_DEFAULT = "";
    + const std::string PROP_FS_URI_DEFAULT = "hdfs://192.168.49.1:9000/";

    ----------------

    - rocksdb::Options opt;
    - opt.create_if_missing = true;
    + std::unique_ptr<rocksdb::Env> hdfs;
    + rocksdb::NewHdfsEnv("hdfs://192.168.49.1:9000/", &hdfs);
    + rocksdb::Options opt;
    + opt.create_if_missing = true;
    + opt.env = hdfs.get();

    ```

3. Run the command in order to create the ycsb executable that you need to run now: `make`

4. Run the command: `./ycsb -load -run -db rocksdb -P workloads/workloada -P rocksdb/rocksdb.properties -s`


---

## References

1. Just debugging for a month to find a small change!


---

## Further Reading

Some other cool concepts to explore!!
1. LF and CRLF: Windows and Linux.. Why!!?
2. CRDT (Conflict-free Replicated DataTypes): So, How does data get trasferred (safely) between geographically seperated servers?

Relax a bit, its a good thing
1. This song on [youtube](https://youtu.be/HxlysqrFjHo)


---

## Contributors

1. Viraj Thakkar [[github.com/veedata](https://github.com/veedata)] [[viraj.online@asu.edu](mailto:viraj.online@asu.edu)]
