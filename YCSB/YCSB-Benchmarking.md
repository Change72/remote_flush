# Benchmarking RocksDB on YCSB

Some changes need to be made to allow for perfect YCSB compatibility. This document goes over the same in some detail 

---

## Table of Contents

+ [parent (ycsb)](./../YCSB/)

---

## About 

High-level Software Version:
+   RocksDB:    v7.5.3
+   YCSB:       master branch

---

## Setup

This document mainly deals with benchmarking the stock RocksDb that is found in the maven repository. In order to do anything non default, some more poking around is required which will be covered in another article.

### YCSB
1. `git clone https://github.com/brianfrankcooper/YCSB.git`
2. `cd YCSB`
3. `mvn -pl site.ycsb:rocksdb-binding -am clean package`

---

## Usage

YCSB allows for 3 different ways to run the benchmark:  `ycsb.bat` for windows, `ycsb.sh` for linux, and `ycsb` which is cross platform. 

### YCSB (Cross-Platform)

> This method isnt preferred as Python 2 has reached End of Life in 2020. The other 2 methods are more preferable until YCSB takes on Python 3.

1. Install python 2.7: \
    If you used the cross-platform option, it needs python 2.7 as a dependency, so, make sure to install python according to your operating system. \
    If you are using Windows, go to [this](https://www.python.org/download/releases/2.7/) website, download the appropriate installer and then install. \
    If you are using Linux, it can be done using `sudo apt install python2-minimal`.

2. You can then run the load command using: `./bin/ycsb load rocksdb -s -P workloads/workloada -p recordcount=10000 -p rocksdb.dir=/path/to/data/dir -p rocksdb.optionsfile=/path/to/options.ini`

### YCSB (Linux)

This is the preferred method for me, however it need some changes before it is good to go.
1. Update `rocksd/pom.xml` with htrace and hdrhistogram dependencies and a rocksdb version number:
    ```
        ...
        <dependency>
            <groupId>org.rocksdb</groupId>
            <artifactId>rocksdbjni</artifactId>
            <version>7.5.3</version>
        </dependency>
        <dependency>
            <groupId>org.apache.htrace</groupId>
            <artifactId>htrace-core4</artifactId>
            <version>4.1.0-incubating</version>
        </dependency>
        <dependency>
            <groupId>org.hdrhistogram</groupId>
            <artifactId>HdrHistogram</artifactId>
            <version>2.1.4</version>
        </dependency>
        <dependency>
            <groupId>site.ycsb</groupId>
            <artifactId>core</artifactId>
            <version>${project.version}</version>
            <scope>provided</scope>
        </dependency>
        ...
    ```
2. Run the command: `mvn -pl site.ycsb:rocksdb-binding -am clean package`
2. Run the command: `./bin/ycsb.sh load rocksdb -s -P workloads/workloada -p rocksdb.dir=/path/to/data/dir -p recordcount=10000 -p rocksdb.optionsfile=/path/to/options.ini`

### YCSB (Windows)

*Untested*

---

## References

1. [Blog in Chinese](https://www.jianshu.com/p/e9d8a0e3eb1d)
2. [Cusomising Config in YCSB](https://www.programmerall.com/article/26002412760/)

---

## Further Reading

Some other cool concepts to explore!!
1. Tunable Consistency: Ever thought about changing consistency on the go?
2. Protocol Buffers: HTTP is a stream of data, how to seperate and send messages efficiently?

Relax a bit, its a good thing
1. This song on [youtube](https://youtu.be/gS9o1FAszdk)


---

## Contributors

1. Viraj Thakkar [[github.com/veedata](https://github.com/veedata)] [[viraj.online@asu.edu](mailto:viraj.online@asu.edu)]
