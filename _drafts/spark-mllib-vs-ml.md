---
layout: post
title:  "Machine Learning in Spark: RDD vs Dataframe API"
---

[Apache Spark][spark] is an open-source framework for cluster computing and fast
data processing. It runs on a variety of platforms, including stand-alone and
on [Hadoop][hadoop]. It can access data stored in HDFS, [Cassandra][cassandra]
etc. For example, we might have an existing Hadoop cluster. Deploying Spark
on top of that might speed up analytics dramatically because Spark performs
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

First, a few notes on naming conventions. This is to ease some confusion that
might appear when finding information about different APIs. Officially, the new
API is called SparkML Dataframe-based API. This is the name that I will be
using, as well. However, many if not most people use MLlib when referring to the
old RDD API, and SparkML or Spark ML when talking about the new one. This is
not an official name, however.

The [new API][dfapi] is based on [Dataframes][dataset] instead of RDDs, and
is now used as the primary API for Sparks machine learning capabilites.
A Dataset is a distributed data collection that aims to combine the benefits
of RDDs and Spark SQL engine. In the context of machine learning, we are
especially interested in DataFrames. They are Datasets with named columns.
From a programming perspective, two important points come to mind when talking
about DataSets:
- Conceptually, they can be considered relational database tables
- They are well optimized, and the developer doesn't need to worry about
  these optimizations

One more thing: DataFrames are *lazy*, which means any operations on them are
not evaluated until Spark deems it necessary. You can define all the
transformations you want, but they might be executed a bit later. A small
example:

```scala
/** Now we have a DataFrame df with columns "name" and "age", among other
    things. */
// Let's filter out people under 18
df.filter($"age" > 18)
// Note that nothing has been done yet.

// Now let's print it out, this will also execute the query
df.show()
```

DataFrames are also related to an important feature called
[Pipelines][pipeline]. I'll write a separate blog post about that in more
detail. Anyway, in short, DataFrames are higher level than RDDs, and the optimizations happen under the hood.

# The Main Point

I mentioned earlier that RDDs require developers to think imperative (and
functional). With the new API, this has changed, and a completely different
programming paradigm is required. DataFrames resemble database tables, and
Spark even uses an SQL engine in the background. SQL happens to be a
*declarative* language, which means you specify *what* to do, not how.
This means that even though we are still using Spark and its machine learning
capabilities, the style of programming has completely changed. This
demonstrates that it's always necessary to be versatile in the world of Data
Science.

[spark]: https://spark.apache.org/
[hadoop]: https://hadoop.apache.org/
[cassandra]: https://cassandra.apache.org/
[rdd]: https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds
[dataset]: https://spark.apache.org/docs/latest/sql-programming-guide.html#datasets-and-dataframes
[dfapi]: https://spark.apache.org/docs/latest/ml-guide.html
[rddapi]: https://spark.apache.org/docs/latest/mllib-guide.html
[scala]: https://www.scala-lang.org/
[pipeline]: https://spark.apache.org/docs/latest/ml-pipeline.html
