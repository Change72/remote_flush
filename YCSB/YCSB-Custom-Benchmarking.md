# Benchmarking Custom RocksDB on YCSB

Make your Custom RocksDB code run with YCSB. This document goes over the same in some detail.

---

## Table of Contents

+ [parent (ycsb)](./../YCSB/)

---

## About 

High-level Software Version:
+   RocksDB:    Custom Code

---

## Setup

Make sure you have completed setting up YCSB and at least run a single benchmark on the RocksDB version that you are modifying. This can be done by referring the article [YCSB-Benchmarking](./YCSB-Benchmarking.md). 

Once, you have done this, go to the usage and follow the steps there.

---

## Usage

> This is all in Java, so, make sure that your modified version of RocksDB works with Java. If not, you might be better suited to the adding a new database to YCSB (tutorial coming soon)

1. Go to your Custom RocksDB folder and build the jar for it: `make clean && make -j8 rocksdbjavastatic`
2. cd to the location of the jar file and install it in your system:
   
    ```bash
    cd ./java/target
    mvn install:install-file -Dfile=rocksdbjni-<version>-linux64.jar -DgroupId=org.rockscustom -DartifactId=rockscustomjni -Dversion={custom-version} -Dpackaging=jar
    ```

3. Go to your YCSB folder, move to the rocksdb directory there and edit the pom.xml file. Delete the older org.rocksdb dependce and replace it with the below content.

    ```bash
    cd YCSB/rocksdb
    ```

    ```xml
    <dependency>
        <groupId>org.rockscustom</groupId>
        <artifactId>rockscustomjni</artifactId>
        <version>{custom-version}</version>
    </dependency>
    ```

4. Run the command: `mvn -pl site.ycsb:rocksdb-binding -am clean package`

5. Run the command: `./bin/ycsb.sh load rocksdb -s -P workloads/workloada -p rocksdb.dir=/path/to/data/dir -p recordcount=10000 -p rocksdb.optionsfile=/path/to/options.ini`


---

## References

1. [Blog in Chinese](https://www.jianshu.com/p/e9d8a0e3eb1d)
2. [Cusomising Config in YCSB](https://www.programmerall.com/article/26002412760/)
3. [mvn install](https://maven.apache.org/guides/mini/guide-3rd-party-jars-local.html)


---

## Further Reading

Some other cool concepts to explore!!
1. Tunable Consistency: Ever thought about changing consistency on the go?
2. Protocol Buffers: HTTP is a stream of data, how to seperate and send messages efficiently?

Relax a bit, its a good thing
1. This song on [youtube](https://youtu.be/GSBFehvLJDc)


---

## Contributors

1. Viraj Thakkar [[github.com/veedata](https://github.com/veedata)] [[viraj.online@asu.edu](mailto:viraj.online@asu.edu)]
