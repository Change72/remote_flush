# RocksDB-HDFS: An Installation and Usage guide

This document is to show how to setup RocksDB with the HDFS plugin enabled and how to use it. 

---

## Table of Contents

+ [parent](./../RocksDB/)

----

## Setup
We will go over the installation for HDFS, RocksDB and then RocksDB with HDFS. An additional section for Environment setup with some ease-of-life options will also be present but is optional. _In case of permission issues with any commands, try using the same command with `sudo` as the prefix._

### HDFS
This document will not be showing how to setup a HDFS environment, it will only go through the downloading and unzipping since some of the files from the HDFS directories are required by the HDFS plugin in RocksDB. \
For Installation and setup of HDFS, please check: [HDFS](./../HDFS/)

1.	Downloading Hadoop v3.3.1 files: `wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.1/hadoop-3.3.1.tar.gz -O hadoop.tar.gz`
2.  Creating a directorhy for hadoop to be in: `mkdir hadoop`
3.	Untar the files into the newly created directory: `tar -xzf hadoop.tar.gz -C hadoop`
4.  Move all the files down 1 directory: `mv ./hadoop/hadoop-3.3.1/* ./hadoop/`

### RocksDB
This section will deal with the installation of RocksDB with the HDFS plugin

1. Install dependencies that will be requrired for the rocksdb: `sudo apt-get install -y openjdk-8-jdk libgflags-dev libsnappy-dev zlib1g-dev libbz2-dev liblz4-dev libzstd-dev build-essential`
2. Get rocksdb stable release (v7.4.5 for this example): `wget https://github.com/facebook/rocksdb/archive/refs/tags/v7.4.5.tar.gz -O rocksdb.tar.gz` 
3. Create directory for rocksdb files: `mkdir rocksdb`
4. Untar the file: `tar -xzf rocksdb.tar.gz -C ./rocksdb`
5. Move the files down 1 directory: `mv ./rocksdb/rocksdb-7.4.5/* ./rocksdb/`

### HDFS Plugin
> Note: If you are going to perform `make install`, you will need admin priviledges. Better to do `sudo su` and run the below commands. 

1. Setup some environment variables:
    ```
    export HADOOP_HOME=/home/<username>/hadoop
    export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64

    export LD_LIBRARY_PATH=$JAVA_HOME/jre/lib/amd64/server:$JAVA_HOME/jre/lib/amd64:$HADOOP_HOME/lib/native
    export CLASSPATH=`$HADOOP_HOME/bin/hadoop classpath --glob`

    for f in `find $HADOOP_HOME/share/hadoop/hdfs | grep jar`; do export CLASSPATH=$CLASSPATH:$f; done
    for f in `find $HADOOP_HOME/share/hadoop | grep jar`; do export CLASSPATH=$CLASSPATH:$f; done
    for f in `find $HADOOP_HOME/share/hadoop/client | grep jar`; do export CLASSPATH=$CLASSPATH:$f; done
    ```
2.  Browse inside the plugin directory of rocksdb: `cd rocksdb/plugin/`
3.  Clone the hdfs plugin repository: `git clone https://github.com/asu-idi/rocksdb-hdfs hdfs && cd ..` 
4.  Perform a make clean and then your required make configuration (install used for this example): `make clean && DEBUG_LEVEL=0 ROCKSDB_PLUGINS="hdfs" make -j$(nproc) install`

### Wrapping up
1.	Setup the following Environment variables in your .bashrc file for easier access to the commands everytime you use your machine.
    ```
    export HADOOP_HOME=/home/<username>/hadoop
    export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
    export PATH=$PATH:$HADOOP_HOME/bin
    ```

    ![bashrc](./media/hdfs-bashrc.png)

2. Run the `source ~/.bashrc` command once to run the bashrc file and have all your env variables now usable.

---

## Usage

If you have performed `make install`, the rocksdb header files and archive file are now stored in your device at `/usr/local/include/` and `/usr/local/lib/`.
This means that you now do not need the rocksdb files in the user's home directory anymore and you may delete that rockdb directory. If you want to do this, make sure to make a copy of the make_config.mk file beforehand though, the steps before will show you the same.

### Deleting rocksdb files
You may skip this step, it is majorly to save space. Only perform the following steps if you have performed `make install`!

1. Copy the make_config file from the rocksdb directory to your home directory (you can copy this file anywhere else as well, location does not matter): `cp ./rocksdb/make_config.mk ..`
2. Delete the rocksdb files: `rm -r ./rocksdb`


### Using RocksDB with the plugin

Make sure that you have installed HDFS to your device and it is running. You can find the installation guide to HDFS [here](./../HDFS/). If it is not running, the following steps will not work! \
This section also assumed that you have previsouly worked with rocksdb and know the basics of coding in it.

1. Adding required libraries
    ```
    #include "plugin/hdfs/env_hdfs.h"
    #include "hdfs.h"
    ```
2. Connecting rocksdb to HDFS. \
    Add the following lines to the rocksdb code before you are creating the database, this will allow the library to understand that you are trying to connect to the HDFS environment
    ```
    std::unique_ptr<rocksdb::Env> hdfs;
    rocksdb::NewHdfsEnv("hdfs://<hdfs-ip-address>:9000/", &hdfs);

    Options options;
    options.env = hdfs.get();
    ```
3. Connecting the HDFS libraries to the rocksdb code. \
    In your Makefile, you need to include the libhdfs.so file as well, for this you need to add the following while compiling your code: 
    ```
    -I${HADOOP_HOME}/include -L${HADOOP_HOME}/lib/native -lhdfs
    ```

> You can also check out the example code (specailly if you do not understand the above) at the [asu-idi/rocksdb-hdfs repo](https://github.com/asu-idi/rocksdb-hdfs/tree/master/examples)

### Running the examples

This part is optional, to assist in running the code for the first time. \
Make sure that you have the `make_config.mk` file in the same directory where you will perform the following commands!

1. Get the example code: `wget https://raw.githubusercontent.com/asu-idi/rocksdb-hdfs/master/examples/testhdfs.cc`
2. Get the example Makefile: `wget https://raw.githubusercontent.com/asu-idi/rocksdb-hdfs/master/examples/Makefile`
3. Run the `make testhdfs` command
4. Check hdfs to verify that the folder with data has been created: `hdfs dfs -ls /test/rdb_hdfs`

---

## Contributors
1. Viraj Thakkar ([veedata](github.com/veedata))
