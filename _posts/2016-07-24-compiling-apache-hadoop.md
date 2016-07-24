---
layout: post
title: Compiling Apache Hadoop?
---

I need to perfom some tests over hdfs. This time, I prefer to set up a clean installation of hadoop so I'm sharing useful command lines which could help you if you plan to compile hadoop for yourself. Compiled using a Centos 6.8.

```
1. Requisites
$ sudo yum install gcc gcc-c++ make zlib zlib-devel -y;
$ sudo yum install subversion -y;
$ sudo yum install lzo lzo-devel autoconf automake libtool -y;
$ sudo yum install openssl openssl-devel -y;
$ sudo yum install glibc.i686 -y;
$ sudo yum install java-1.8.0-openjdk-devel -y
$ sudo yum install git -y

$ wget https://protobuf.googlecode.com/files/protobuf-2.5.0.tar.bz2
$ ./configure ; make ; sudo make install
$ wget http://downloads.sourceforge.net/project/findbugs/findbugs/3.0.1/findbugs-3.0.1.tar.gz
$ wget http://www-us.apache.org/dist/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz

$ vim ~/.bashrc
...
MAVEN_HOME=/home/edward/Downloads/apache-maven-3.3.9
FINDBUGS_HOME=/home/edward/Downloads/findbugs-3.0.1
PATH=$PATH:$MAVEN_HOME/bin:$FINDBUGS_HOME/bin
export MAVEN_HOME FIND_BUGS PATH
...

2. Download
$ git clone https://github.com/apache/hadoop.git
$ cd hadop
$ git checkout branch-2.7.2

3. Compile
$ cd hadoop-maven-plugins
$ mvn install -DskipTests
$ cd ..
$ mvn package -Pdist,native -Dtar -DskipTests
```
