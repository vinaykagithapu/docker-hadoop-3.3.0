# Hadoop Single Node Cluster on Docker.

Following this steps you can build and use the image to create a Hadoop Single Node Cluster containers.

# Setup
## Clone the project
1. clone the project files from github.
```shell
git clone https://github.com/vinaykagithapu/docker-hadoop-3.3.0.git
cd docker-hadoop-3.3.0
```

## Create the hadoop image
1. We can create hadoop image using docker 
```shell
docker build -t hadoop .
```
2. Wait for 5 minutes

## Create the container
1. To run and create a container execute the below command:
```shell
docker run -it --name hadoop-cluster -p 9864:9864 -p 9870:9870 -p 8088:8088 --hostname localhost hadoop
```
2. Change **container-name** (hadoop-cluster) by your favorite name and set **your-hostname** with by your ip or name machine. You can use **localhost** (localhost) as your-hostname

3. When you run the container, at the entrypoint you use the [docker-entrypoint.sh](docker-entrypoint.sh) shell that creates and starts the hadoop environment.

4. You should get the following prompt:
```
     hduser@localhost:~$ 
```
5. To check if hadoop container is working go to the url in your browser http://localhost:9870
6. We can exit from container
```shell
exit
```
7. We can stop hadoop-cluster container 
```shell
docker stop hadoop-cluster
```
8. We can start hadoop-cluster container 
```shell
docker start hadoop-cluster
```
9. We can exec into container 
```shell
docker exec -it hadoop-cluster bash
```
**Notice:** the hdfs-site.xml configure has the property, so don't use it in a production environment.

     <property>
          <name>dfs.permissions</name>
          <value>false</value>
     </property>

## A first example

Make the HDFS directories required to execute MapReduce jobs:

     hduser@localhost:~$ 
     
hdfs dfs -mkdir /user

     hduser@localhost:~$ hdfs dfs -mkdir /user/hduser

Copy the input files into the distributed filesystem:
      
     hduser@localhost:~$ hdfs dfs -mkdir input
     hduser@localhost:~$ hdfs dfs -put $HADOOP_HOME/etc/hadoop/*.xml input

Run some of the examples provided:

     hduser@localhost:~$ hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.0.jar grep input output 'dfs[a-z.]+'

     2020-08-08 01:57:02,411 INFO impl.MetricsConfig: Loaded properties from hadoop-metrics2.properties
     2020-08-08 01:57:04,754 INFO impl.MetricsSystemImpl: Scheduled Metric snapshot period at 10 second(s).
     2020-08-08 01:57:04,754 INFO impl.MetricsSystemImpl: JobTracker metrics system started
     2020-08-08 01:57:08,843 INFO input.FileInputFormat: Total input files to process : 10
     ..............
     .............
     ............
     File Input Format Counters 
          Bytes Read=175
     File Output Format Counters 
          Bytes Written=47

Examine the output files: check the output files from the distributed filesystem and examine them:

     hduser@localhost:~$ hdfs dfs -ls output/
     Found 2 items
     -rw-r--r--   1 hduser supergroup          0 2020-08-08 01:58 output/_SUCCESS
     -rw-r--r--   1 hduser supergroup         47 2020-08-08 01:58 output/part-r-00000

Checking the result using **cat** command on the distributed filesystem:

     hduser@localhost:~$ hdfs dfs -cat output/*
     1	dfsadmin
     1	dfs.replication
     1	dfs.permissions


## Stopping and re-starting the container

1. To stop the container execute the following commands, to gratefully shutdown.
2. Need to run this hadoop-cluster container
```shell
stop-dfs.sh
stop-yarn.sh
```
3. After that.
```shell
exit
```
4. To re-start the container, and go back to our Hadoop environment execute:
```shell
docker start -i hadoop-cluster
```

