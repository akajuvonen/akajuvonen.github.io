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

The older API is known as MLlib (more on naming a bit later), and it is
based on RDDs, or [Resilient Distributed Datasets][rdd]. They are faul-tolerant
and parallelized collections of elements. For example, we could create a RDD
from an existing data type using the `parallelize` function (in [Scala][scala]):

```scala
// The original data array
val data = Array(1, 2, 3)
/** Create the parallelized dataset.
    Note that sc is the SparkContext.
    Check Spark docs for initializing it.*/
val parallelData = sc.parallelize(data)
```

[spark]: https://spark.apache.org/
[hadoop]: https://hadoop.apache.org/
[cassandra]: https://cassandra.apache.org/
[rdd]: https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds
[dataset]: https://spark.apache.org/docs/latest/sql-programming-guide.html#datasets-and-dataframes
[dfapi]: https://spark.apache.org/docs/latest/ml-guide.html
[rddapi]: https://spark.apache.org/docs/latest/mllib-guide.html
[scala]: https://www.scala-lang.org/
