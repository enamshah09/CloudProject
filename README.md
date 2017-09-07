# Comparison of Containers - Google Cloud and Amazon EC2
### Introduction
The purpose of this repo is to compare and analyze containers of different cloud providers by using different MapReduce Benchmarks in Hadoop.
### Steps
#### Setup WordCount
1. Create java file for WordCount program [WordCount.java](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html#Source_Code)
2. Setup Environment variables and export path
```
$ export JAVA_HOME=/usr/java/default
$ export PATH=${JAVA_HOME}/bin:${PATH}
$ export HADOOP_CLASSPATH=${JAVA_HOME}/lib/tools.jar
```
3. Compile `Word count` java file and create a jar file
```
$ bin/hadoop com.sun.tools.javac.Main WordCount.java
$ jar cf wc.jar WordCount*.class
```
4. Take the input files using wget command and move them to a directory where you have all the input files
5. Run WordCount.java and also specify the path for input files and output files
```
$ time bin/hadoop jar wc.jar WordCount /path-to-input-file /path-to-output-file
```