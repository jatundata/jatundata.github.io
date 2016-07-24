---
layout: post
title: Configuring Apache Hadoop
---

Guidelines showing how to configure Apache Hadoop 2.7.2

```
[edward@jatundata Downloads]$ mkdir hadoop-releases
[edward@jatundata Downloads]$ cd hadoop-releases/
[edward@jatundata hadoop-releases]$ tar xzvf ~/Downloads/hadoop/hadoop-dist/target/hadoop-2.7.2.tar.gz 
[edward@jatundata hadoop-releases]$ cd hadoop-2.7.2/
[edward@jatundata hadoop-2.7.2]$ ls
bin  etc  include  lib  libexec  LICENSE.txt  NOTICE.txt  README.txt  sbin  share
[edward@jatundata hadoop-2.7.2]$ mkdir -p /opt/public/hadoop-store/namenode /opt/public/hadoop-store/datanode
[edward@jatundata hadoop-2.7.2]$ cd etc/hadoop/
[edward@jatundata hadoop]$ vim mapred-site.xml
...
<property>
   <name>mapreduce.framework.name</name>
   <value>yarn</value>
</property>
...

[edward@jatundata hadoop]$ vim hdfs-site.xml
...
<property>
   <name>dfs.replication</name>
   <value>1</value>
</property>
<property>
   <name>dfs.namenode.name.dir</name>
   <value>file:/opt/public/hadoop-store/namenode</value>
</property>
<property>
   <name>dfs.datanode.data.dir</name>
   <value>file:/opt/public/hadoop-store/datanode</value>
</property>
...

[edward@jatundata hadoop]$ vim core-site.xml
...
<property>
   <name>fs.default.name</name>
   <value>hdfs://jatundata.localdomain:9000</value>
</property>
...

[edward@jatundata hadoop]$ vim yarn-site.xml 
...
<property>
   <name>yarn.nodemanager.aux-services</name>
   <value>mapreduce_shuffle</value>
</property>
<property>
   <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
   <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
...

[edward@jatundata hadoop]$ vim hadoop-env.sh
...
export HADOOP_HEAPSIZE="500"
export HADOOP_NAMENODE_INIT_HEAPSIZE="500"
...

[edward@jatundata hadoop]$ vim mapred-env.sh
...
export HADOOP_JOB_HISTORYSERVER_HEAPSIZE=250
...

[edward@jatundata hadoop]$ vim yarn-env.sh
...
JAVA_HEAP_MAX=-Xmx500m
YARN_HEAPSIZE=500
...


[edward@jatundata hadoop]$ vim ~/.bashrc
...
#HADOOP VARIABLES START
export HADOOP_HOME=/home/edward/Downloads/hadoop-releases/hadoop-2.7.2
source $HADOOP_HOME/etc/hadoop/hadoop-env.sh
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.91-1.b14.el6.x86_64
export HADOOP_INSTALL=$HADOOP_HOME
export PATH=$PATH:$HADOOP_INSTALL/bin
export PATH=$PATH:$HADOOP_INSTALL/sbin
export HADOOP_MAPRED_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_HOME=$HADOOP_INSTALL
export HADOOP_HDFS_HOME=$HADOOP_INSTALL
export YARN_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_INSTALL/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_INSTALL/lib:$HADOOP_INSTALL/lib/native"
#HADOOP VARIABLES END
...

[edward@jatundata hadoop]$ sudo ln -s /home/edward/Downloads/hadoop-releases/hadoop-2.7.2/etc/hadoop /etc/hadoop

[edward@jatundata hadoop]$ hdfs namenode -format

[edward@jatundata hadoop]$ ssh-copy-id edward@jatundata.localdomain

[edward@jatundata hadoop]$ start-dfs.sh

[edward@jatundata hadoop]$ start-yarn.sh

[edward@jatundata hadoop]$ jps
26150 DataNode
26327 SecondaryNameNode
26604 NodeManager
26958 Jps
26494 ResourceManager
26015 NameNode

[edward@jatundata hadoop]$ hdfs dfs -ls /
[edward@jatundata hadoop]$ hdfs dfs -mkdir /datasets/
[edward@jatundata hadoop]$ hdfs dfs -ls /
Found 1 items
drwxr-xr-x   - edward supergroup          0 2016-07-23 23:57 /datasets
[edward@jatundata hadoop]$ 

```
