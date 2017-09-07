# Comparison of Containers - Google Cloud and Amazon EC2
## Introduction
The purpose of this repo is to compare and analyze containers of different cloud providers by using different MapReduce Benchmarks in Hadoop.
## Steps
### 1. Setup WordCount
* Create java file for WordCount program [WordCount.java](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html#Source_Code)
* Setup Environment variables and export path
```
$ export JAVA_HOME=/usr/java/default
$ export PATH=${JAVA_HOME}/bin:${PATH}
$ export HADOOP_CLASSPATH=${JAVA_HOME}/lib/tools.jar
```
* Compile `Word count` java file and create a jar file
```
$ bin/hadoop com.sun.tools.javac.Main WordCount.java
$ jar cf wc.jar WordCount*.class
```
* Take the input files using wget command and move them to a directory where you have all the input files
* Run WordCount.java and also specify the path for input files and output files
```
$ time bin/hadoop jar wc.jar WordCount /path-to-input-file /path-to-output-file
```
### 2. Running Hadoop Benchmarks
We will conduct experiments using different MapReduce benchmarks to stress Hadoop clusters over distinct situations on the container-based virtualization systems. The Distribution of Hadoop provides number of Benchmarks, which are bundled in `hadoop-*test*.jar` and `hadoop-*examples*.jar`. The approach we took to do this is by conducting experiments on two well known evaluation perspectives `Micro Benchmark` and `Macro Benchmark`.
#### Micro Benchmark
Micro-benchmark is used to test the basic components of the Hadoop system. By micro-benchmarks it is possible to measure the performance of basic components before evaluating the system as a whole.
#### Macro Benchmark
Macro-benchmarks stress out numerous segments of a framework and can typically give more critical outcomes, as it certainly incorporates the segments before assessed by small scale benchmarks.
##### TestDFSIO Benchmark
The TestDFSIO is a read and write test for HDFS. It uses one map task per file. It gives an idea of how fast the cluster is terms of I/O. Helpful in stress testing HDFS. It discovers network performance bottleneck. Default output directory is `/benchmarks/TestDFSIO`.
Note: Run write test before read test as the TestDFSIO read benchmark doesn't generate its own input files.
##### NameNode Benchmark

##### MapReduce Benchmark
