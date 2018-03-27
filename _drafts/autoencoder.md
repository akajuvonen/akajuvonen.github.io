---
layout: post
title: "Visualizing Autoencoder Reconstruction"
---

Autoencoders are neural networks that try to copy its input to output.
The idea sounds a bit silly, but they do have applications in, e.g., denoising
and dimensionality reduction.

In this post, we'll take a look at how an autoencoder reconstructs images
from the [MNIST][mnist] hand-written digit data.

# Autoencoder

This part assumes that the reader has at least a basic understanding of what
neural networks are (on an intuitive level).

As mentioned, autoencoders are neural networks that simply set the output same
as the input. Then they attempt to learn the parameters of the network and
minimize reconstruction error. But what is the point of that? For example,
a network with a hidden layer with the same size as the input (and output)
could just simply copy the inputs to the output, and have zero error. This
would of course be meaningless.

But if we consider the situation where hidden layers are smaller, it's obvious
that the network needs to learn some low-dimensional representation of the data
in order to reconstruct it later.

Below is a visualization of this case, including *encoder* and *decoder* parts
of the network. This kind of autoencoder is called *undercomplete*. Encoder
learns to represent the data in lower dimensions (ending up in the middle
layer *code*). Middle layer represents the latent space. Decoder then
reconstructs the data into ouput.

![Example][fig_ae]

It is also possible to have more neurons in hidden than input layer. These
types of autoencoders are *overcomplete*. In this case it is necessary to
introduce some kind of regularization to encourage sparsity for the network
to actually work.

I don't like to go into too much detail, since a lot has been already written
about autoencoders. However, it would be nice to visualize the learning
process to get a better idea how it all works. Let's do that with some
TensorFlow code.

# Demo with code

Full commented code can be found in [this GitHub repository][ae_repo].
Here are some simplified code snippets to demonstrate how to define an
autoencoder using TensorFlow. All of the code assumes that you have TF
installed and imported:

```python
import tensorflow as tf
```

We can now define a model (graph) for autoencoder. This example architecture
consists of 3 hidden layers with 500, 100 and 500 neurons, respectively.
We can use `tf.layers.dense` for densely connected neural network layers.
TensorFlow layers automatically take care of weight and bias initialization.
Input and output tensors `x` and `output` are of the same size.

```python
def autoencoder(x, output_size, outer_size=500, inner_size=100):
    encoder = tf.layers.dense(x, outer_size)
    code = tf.layers.dense(encoder, inner_size)
    decoder = tf.layers.dense(code, outer_size)
    output = tf.layers.dense(decoder, output_size)
    return output
```

![Visualization][gif_animation]

[fig_ae]: /assets/autoencoder/autoencoder.png
[gif_animation]: /assets/autoencoder/animation.gif
[mnist]: http://yann.lecun.com/exdb/mnist/
[ae_repo]: https://github.com/akajuvonen/autoencoder-reconstruct-visualizer
