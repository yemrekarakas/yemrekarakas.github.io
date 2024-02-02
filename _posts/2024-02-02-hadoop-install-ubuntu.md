---
title: Hadoop Install Ubuntu
author: yek
date: 2024-02-02 11:33:35 +03:00
categories: [Blogging]
tags: [hadoop, ubuntu, bigdata]
pin: true
image:
  path: https://yemrekrks.sirv.com/posts/sqlserver/1e389b6114248d19c27e1b6c2eeecd4d.jpg
  alt: Raven's Nest overlook at sunset, Acadia National Park, Schoodic Peninsula, Maine, USA
---

Ubuntu 20.4 üzerinde Hadoop 3.3.6 sürümü

Örnek: docker-compose.yml
```
version: "3"

services:
  hadoop:
    hostname: hdserver
    container_name: hadoop
    image: emrekarakas/hadoop:latest
    ports:
      - "9000:9000" # HDFS
      - "9870:9870" # NameNode
      - "8088:8088" # ResourceManager
      - "9864:9864"
      - "9866:9866"
      
    volumes:
      - ./data:/home/data
    stdin_open: true
    tty: true
```

## Install OpenJDK on Ubuntu
ubuntu 20.04

`sudo apt update`

`sudo apt install openjdk-8-jdk -y`

`java -version; javac -version`

### Install OpenSSH on Ubuntu
`sudo apt install openssh-server openssh-client -y`

### Create Hadoop User
sudo passwd hduser

su - hduser

### Enable Passwordless SSH for Hadoop User
`ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa`

`cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys`

`chmod 0600 ~/.ssh/authorized_keys`

`sudo vim /etc/ssh/sshd_config`

PORT=22 açılacak

`sudo service ssh restart`

ssh i test etmek için

`ssh localhost`

### Download and Install Hadoop on Ubuntu
`wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz`

`tar xzf hadoop-3.3.6.tar.gz`

### Configure Hadoop Environment Variables (bashrc)
`sudo vim ~/.bashrc`

```sh
#Hadoop Related Options
export HADOOP_HOME=/opt/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
```

```sh
alias start-dfs='${HADOOP_HOME}/sbin/start-dfs.sh'
alias start-yarn='${HADOOP_HOME}/sbin/start-yarn.sh'
alias start-all='${HADOOP_HOME}/sbin/start-all.sh'

alias stop-dfs='${HADOOP_HOME}/sbin/stop-dfs.sh'
alias stop-yarn='${HADOOP_HOME}/sbin/stop-yarn.sh'
alias stop-all='${HADOOP_HOME}/sbin/stop-all.sh'

alias hdfs='${HADOOP_HOME}/bin/hdfs'
alias yarn='${HADOOP_HOME}/bin/yarn'
```

`source ~/.bashrc`

### Edit hadoop-env.sh File
`sudo vim $HADOOP_HOME/etc/hadoop/hadoop-env.sh`

`export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64`

`which javac`

`readlink -f /usr/bin/javac`

```sh
export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")
export HADOOP_OPTS="$HADOOP_OPTS -Djava.library.path=$HADOOP_HOME/lib/native"
export HADOOP_OS_TYPE=${HADOOP_OS_TYPE:-$(uname -s)}
```

### Edit core-site.xml File
`sudo vim $HADOOP_HOME/etc/hadoop/core-site.xml`
```xml
<property>
  <name>hadoop.tmp.dir</name>
  <value>/opt/hadoop/tmpdata</value>
</property>
<property>
  <name>fs.default.name</name>
  <value>hdfs://127.0.0.1:9000</value>
</property>
```


### Edit hdfs-site.xml File
sudo vim $HADOOP_HOME/etc/hadoop/hdfs-site.xml
```xml
<property>
  <name>dfs.replication</name>
  <value>1</value>
</property>
<property>
  <name>dfs.namenode.name.dir</name>
  <value>/opt/hadoop/data/hdfs/namenode</value>
</property>
<property>
  <name>dfs.datanode.data.dir</name>
  <value>/opt/hadoop/data/hdfs/datanode</value>
</property>
```


### Edit mapred-site.xml File
`sudo vim $HADOOP_HOME/etc/hadoop/mapred-site.xml`
```xml
<property> 
  <name>mapreduce.framework.name</name> 
  <value>yarn</value> 
</property>
<property>
  <name>yarn.app.mapreduce.am.env</name>
  <value>HADOOP_MAPRED_HOME=/opt/hadoop/</value>
</property>
<property>
  <name>mapreduce.map.env</name>
  <value>HADOOP_MAPRED_HOME=/opt/hadoop/</value>
</property>
<property>
  <name>mapreduce.reduce.env</name>
  <value>HADOOP_MAPRED_HOME=/opt/hadoop/</value>
</property> 
```

### Edit yarn-site.xml File
`sudo vim $HADOOP_HOME/etc/hadoop/yarn-site.xml`
```xml
<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
</property>
<property>
  <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
  <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
<property>
  <name>yarn.resourcemanager.hostname</name>
  <value>127.0.0.1</value>
</property>
<property>
  <name>yarn.acl.enable</name>
  <value>0</value>
</property>
<property>
  <name>yarn.nodemanager.env-whitelist</name><value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PERPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
</property>
```

### Format HDFS NameNode
`hdfs namenode -format`


### Start Hadoop Cluster
`start-dfs`

`start-yarn`

`jps`

### Access Hadoop UI from Browser
http://localhost:9000

http://localhost:9870 # namenode

http://localhost:9864 # datanode

http://localhost:8088 # YARN resourcemanager


`hdfs dfs -ls /`