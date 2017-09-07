# Comparison of Containers - Google Cloud and Amazon EC2
The purpose of this repo is to compare and analyze containers of different cloud providers by using different MapReduce Benchmarks in Hadoop.
## Steps
### 1. Running Hadoop Benchmarks
We will conduct experiments using different MapReduce benchmarks to stress Hadoop clusters over distinct situations on the container-based virtualization systems. The Distribution of Hadoop provides number of Benchmarks, which are bundled in `hadoop-*test*.jar` and `hadoop-*examples*.jar`. The approach we took to do this is by conducting experiments on two well known evaluation perspectives `Micro Benchmark` and `Macro Benchmark`.
#### Micro Benchmark
Micro-benchmark is used to test the basic components of the Hadoop system. By micro-benchmarks it is possible to measure the performance of basic components before evaluating the system as a whole. Various Micro-benchmarks that we implemented are `TestDFSIO`, `NameNode` and `MapReduce`
##### TestDFSIO Benchmark
The TestDFSIO is a read and write test for HDFS. It uses one map task per file. It gives an idea of how fast the cluster is terms of I/O. Helpful in stress testing HDFS. It discovers network performance bottleneck. Default output directory is `/benchmarks/TestDFSIO`.
Note: Run write test before read test as the TestDFSIO read benchmark doesn't generate its own input files.
###### TestDFSIO write test
```
$ time bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-2.7.1.jar TestDFSIO -write -nrFiles N -fileSize MB
```
###### TestDFSIO read test
```
$ time bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-2.7.1.jar TestDFSIO -read -nrFiles N -fileSize MB
```
###### Remove and clean the test data
```
$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-2.7.1.jar TestDFSIO -clean
```
##### NameNode(NN) Benchmark
In order to analyse the behaviour of NameNode component while dealing with huge amount of HDFS – related requests we choose NN Benchmark. NN Benchmark generates a lot of HDFS-related requests with normally very small “payloads” for the sole purpose of putting a high HDFS management stress on the NameNode. This benchmark is considered to be the load test for HDFS files where we create, write, open, read and delete files.
Note: Run create_write test before open_read, rename, delete test as the NN benchmark doesn't generate its own input files.
```
$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-2.7.1.jar nnbench \ 
-operation <create_write | open_read | rename | delete> -maps <No. of maps. Default=1. Not mandatory> \ 
-reduces <No. of reduces. Default=1. Not mandatory> -blockSize <Block size in bytes. Default=1. Not mandatory> \ 
-bytesToWrite <Default=0. Not mandatory> -numberOfFiles <No. of files to create. Default=1. Not mandatory> \ 
-replicationFactorPerFile <Default=1. Not mandatory> -baseDir <Base DFS path. Default=/becnhmarks/NNBench> \ 
-readFileAfterOpen<Boolean (true of false)>
```
Eg. 
```
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-2.7.1.jar nnbench \ 
-operation create_write -maps 12 -reduces 6 -blockSize 1 -bytesToWrite 0 -numberOfFiles 1000 \ 
-replicationFactorPerFile 3 -readFileAfterOpen true -baseDir /benchmarks/NNBench-`hostname -s`
```
##### MapReduce(MR) Benchmark
MapReduce Benchmark loops a small job a number of times. This benchmark stresses the MR layer to identify how efficiently it works while dealing with enormous job requests. MRBench checks whether small job runs are responsive and running efficiently on your cluster. It puts its focus on the MapReduce layer as its impact on the HDFS layer is very limited.
```
$ time bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-2.7.1.jar mrbench -numRuns N
```
#### Macro Benchmark
Macro-benchmarks stress out numerous segments of a framework and can typically give more critical outcomes, as it certainly incorporates the segments before assessed by small scale benchmarks. Various Macro-benchmarks that we implemented are `TeraSort` and `WordCount`
##### TeraSort Benchmark
The goal of this benchmark is to sort any amount of data as fast as possible. It is a benchmark that combines testing the HDFS and MapReduce layers of an Hadoop cluster. Terasort is the well-known benchmark to identify how fast a Hadoop cluster is.
A full TeraSort benchmark run consists of the following three steps:
- Generate I/p via `TeraGen`
```
$ time bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.1.jar teragen <number of 100-byte rows> <output dir>
```
- Running TeraSort in I/p generated
```
$ time bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.1.jar terasort <input dir> <output dir>
```
- Validate the sorted data obtained from the above step
```
time bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.1.jar teravalidate <terasort output dir (= input data)> <teravalidate output dir>
```
### 2. Setup WordCount
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
* Take the input files using wget command and move them to a directory where you have all the input files. [Sample Input Files](http://mattmahoney.net/dc/textdata.html)
* Run WordCount.java and also specify the path for input files and output files
```
$ time bin/hadoop jar wc.jar WordCount /path-to-input-file /path-to-output-file
```



