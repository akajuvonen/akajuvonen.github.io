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
    with tf.variable_scope(name):
        encoder = tf.layers.dense(x, outer_size, name='encoder')
        code = tf.layers.dense(encoder, inner_size, name='code')
        decoder = tf.layers.dense(code, outer_size, name='decoder')
        output = tf.layers.dense(decoder, output_size, name='output')
        return output
```

From the graph below you can see how TensorFlow constructs the network,
including MSE and gradient calculation. The relevant values are fed to Adam
optimizer.

![High-level graph][graph1]

If we take a look inside the autoencoder graph, we can see that we have exactly
the layers we specified in our model. From this you can also see how the MSE
is only calculated from the output layer (and input layer of course, because
that is the data we are trying to reconstruct).

![Autoencoder graph.][graph2]

```python
#Load MNIST data (NOTE: Removed in TF 1.7)
mnist = tf.contrib.learn.datasets.load_dataset('mnist')
# Example figure to visualize
example_fig = [mnist.train.images[10]]
```

```python
# Learning rate for Adam optimizer
learning_rate = 0.001
# Images are 28*28 pixels
input_size = 28*28
# Mini-batch size
batch_size = 100
# Total number of steps
steps = 2000
# Save training loss and image every [save_every] steps
save_every = 100
```

```python
# Input placeholder
x = tf.placeholder(tf.float32, [None, input_size], name='input')
# Output prediction step
output = autoencoder(x, input_size)
# Using MSE loss
loss = tf.losses.mean_squared_error(x, output)
# Train op using Adam optimizer
train_op = tf.train.AdamOptimizer(learning_rate).minimize(loss)
```

```python
with tf.Session() as sess:
    init = tf.global_variables_initializer()
    sess.run(init)
    for i in range(1, steps+1):
        batch, _ = mnist.train.next_batch(batch_size)
        _, l = sess.run([train_op, loss], feed_dict={x: batch})
        if i % save_every == 0:
            print('Training loss at step {0}: {1}'.format(i, l))
            # Reconstruct example image
            result = sess.run([output], feed_dict={x: example_fig})
            # Code for saving figure here
```

# Visualizing reconstruction

![Visualization][gif_animation]

[fig_ae]: /assets/autoencoder/autoencoder.png
[gif_animation]: /assets/autoencoder/animation.gif
[mnist]: http://yann.lecun.com/exdb/mnist/
[ae_repo]: https://github.com/akajuvonen/autoencoder-reconstruct-visualizer
[graph1]: /assets/autoencoder/tf_graph_1.png
[graph2]: /assets/autoencoder/tf_graph_2.png
