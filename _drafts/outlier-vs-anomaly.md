---
layout: post
title:  "Outliers and Anomalies"
---

In machine learning, there is a concept called *anomaly detection* or *outlier
detection*. These are often used interchangeably. [Wikipedia][1] explains this
as:
>... the identification of items, events or observations which do not conform
>to an expected pattern or other items in a dataset.

Simply, we are trying to find something abnormal. But what is abnormal? How
do we define it? Could abnormality mean different things in different
situations? Personally, I like to differentiate outliers from anomalies.
They are both similar in the sense that we often want to find the, either to
remove them from the data or to focus our attention to them specifically.
I will explain why I think the difference matters, and how to detect them
using different methodologies.

# Some definitions

Let's first define what I mean by outliers and anomalies:
1. An outlier is an improbable data point, e.g., very high values that differ
from normal.
2. An anomaly is something created by a *different process* entirely. There
is a different underlying function or a distribution. The data point itself
might otherwise seem similar to normal data.

Ok, now that that's out of the way, let's move on to a toy example which will
explain what I mean much better.

# Example

TODO.

![Normal data][fig_normal]
![Outliers][fig_outlier]
![Anomaly][fig_anom]

[1]: https://en.wikipedia.org/wiki/Anomaly_detection

[fig_normal]: /assets/outlier_vs_anom_normal.png
[fig_outlier]: /assets/outlier_vs_anom_outlier.png
[fig_anom]: /assets/outlier_vs_anom_anom.png
