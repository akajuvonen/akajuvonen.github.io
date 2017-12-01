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
conventions might be a bit confusing, as well. What are the different APIs and
which one should I choose? Let's take a brief look, focusing mostly on what
all of this means for programmers.

# RDD API

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

Ok, easy enough. Now we are starting to get to the interesting part. How do we
do something with these parallel data collections? If you want to perform
transformations to the data, it's necessary to use many functional programming
methods, such as *map* or *reduce*. Some examples of this:

```scala
// Get the sum of elements
parallelData.reduce((a, b) => a + b)
// Add one to each element
parallelData.map(a => a + 1)
```

In short, working with RDDs requires some functional programming skills. It
also leaves a lot of options to the programmer: *how* to implement something.
It of course also means that poor implementation might have poor performance.
In a way, the old API requires *imperative* style of programming.

Now would be a good time to mention that the RDD API is in
[maintenance mode][dfapi] since Spark 2.0, and that from Spark 3.0 it will be deprecated. The old API will be useful at least until the new one catches up
and reaches feature parity. Anyway, there is a new game in town. Let's take a
look at the new API and what it means from a programming perspective.

# Dataframe API

The [new API][dfapi] is based on [Dataframes][dataset] instead of RDDs, and it
is now used as the primary API for Sparks machine learning capabilites.
TODO: What is Dataset? Examples, implications for programmers. Pros, cons.

[spark]: https://spark.apache.org/
[hadoop]: https://hadoop.apache.org/
[cassandra]: https://cassandra.apache.org/
[rdd]: https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds
[dataset]: https://spark.apache.org/docs/latest/sql-programming-guide.html#datasets-and-dataframes
[dfapi]: https://spark.apache.org/docs/latest/ml-guide.html
[rddapi]: https://spark.apache.org/docs/latest/mllib-guide.html
[scala]: https://www.scala-lang.org/
