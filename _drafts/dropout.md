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

How to implement dropout in practice? Let's look at [Keras][keras], which is
a high-level neural network API. It will be integrated into [TensorFlow][tf]
soon, but can also use other backends, such as [CNTK][cntk].

Keras [Sequential model][keras_seq] allows us to stack model layers linearly.
Any neural network layer, be it a normal densely connected neural layer or
a convolutional layer, can be added simply using `model.add()`. In this case,
the dropout becomes [just another layer][keras_dropout]. First we need to
import it.

```python
from keras.layers import Dropout
```

After this, you only need to add a dropout layer after any neural network
layer.

```python
model.add(Dropout(rate=0.25))
```

In the above example 25% of the input units of a layer are set to 0 during
training. It really couldn't be simpler.

# Conclusions

TODO.

[fig_overfitting]: /assets/dropout/overfitting.png
[fig_nnet]: /assets/dropout/nnet_no_dropout.png
[fig_nnet_dropout]: /assets/dropout/nnet_dropout.png

[keras]: https://keras.io/
[tf]: https://www.tensorflow.org/
[cntk]: https://www.microsoft.com/en-us/cognitive-toolkit/
[keras_seq]: https://keras.io/models/sequential/
[keras_dropout]: https://keras.io/layers/core/#dropout
