---
layout: post
title:  "Machine Learning in Spark: RDD vs Dataframe API"
---

[Apache Spark][spark] is an open-source framework for cluster computing and fast
data processing. It runs on a variety of platforms, including stand-alone and
on [Hadoop][hadoop]. It can access data stored in HDFS, [Cassandra][cassandra]
etc. For example, we might have an existing Hadoop cluster. Deploying Spark
on top of that might speed up analytics dramatically, because Spark performs
many operations in memory.

The interesting thing for Data Scientists is the fact that Spark has Machine
Learning libraries built-in. But when I first started using Spark, I was
confused. There seem to be two ML implementations. In addition, the naming
conventions might be a bit confusing, as well. What are the different APis and
which one should I choose? Let's take a brief look.

[spark]: https://spark.apache.org/
[hadoop]: https://hadoop.apache.org/
[cassandra]: https://cassandra.apache.org/
[rdd]: https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds
[dfapi]: https://spark.apache.org/docs/latest/ml-guide.html
[rddapi]: https://spark.apache.org/docs/latest/mllib-guide.html
