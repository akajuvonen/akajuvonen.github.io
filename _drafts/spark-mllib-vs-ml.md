---
layout: post
title:  "Spark Mllib vs SparkML"
---

Outline:
- ~~What is Spark~~
- Mllib and RDD API
- ML, Dataframe, Dataset
- Differences in thinking and programming

[Apache Spark][spark] is an open-source framework for cluster computing and fast
data processing. It runs on a variety of platforms, including stand-alone and
on [Hadoop][hadoop]. It can access data stored in HDFS, [Cassandra][cassandra]
etc. For example, we might have an existing Hadoop cluster. Deploying Spark
on top of that might speed up analytics dramatically, because Spark performs
many operations in memory.

[spark]: https://spark.apache.org/
[hadoop]: https://hadoop.apache.org/
[cassandra]: https://cassandra.apache.org/
[rdd]: https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds
