---
layout: post
title:  "Overfitting and Dropout in Neural Networks"
---

Intro.

# Overfitting

First, let's take a look at the problem that we are trying to get rid of:
*overfitting*. Overfitting happens when the model adapts to training data
**too well**. This usually means that we get excellent results with training
data, but poor results with any new data. Overfitting is sometimes called
overtraining in machine learning.

Below you can see a visualization of the issue:

![Overfitting][fig_overfitting]

The black points are the original data points. The red line is the overfitted
model. It learns from the data as well as the noise. The blue line is a better
model. This is an example of a situation where the model is giving us more
useful information than the raw data itself (as long as we don't do
overfitting).

# Using dropout to solve the problem

There are many ways to solve the presented problem. One solution often used
in deep neural nets is *dropout*. Below is a picture of a typical neural net
architecture.

![Neural net][fig_nnet]

Now dropout comes to play. The basic idea is very simple. Just randomly
drop some units in the network in the training phase. This is what it might
look like:

![Neural net with dropout][fig_nnet_dropout]

The idea is to get all of the neurons to contribute to the model evenly (model
averaging). This way individual neurons can't dominate by themselves.

# Example implementation

TODO.

# Conclusions

TODO.

[fig_overfitting]: /assets/dropout/overfitting.png
[fig_nnet]: /assets/dropout/nnet_no_dropout.png
[fig_nnet_dropout]: /assets/dropout/nnet_dropout.png
