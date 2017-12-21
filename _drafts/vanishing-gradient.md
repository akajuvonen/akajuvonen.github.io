---
layout: post
title:  "Vanishing Gradient Problem in Deep Neural Nets"
---

Preface.

# Neural network basics

Let's start with some neural network basics. I'm not going to go into too much
detail because there are already countless resources available.

A neural network consists of *nodes*. They take input data multiplied by
weights and add these together. After this, the aggregated sum is input into
an activation function, which determines the output of the node. Optimizing
the weights is essentially what a neural network does when it's learning.

![Example node][fig_node]

A commonly used activation function is the *sigmoid function*:

![Sigmoid function][fig_sigmoidfunc]

If we plot it, it looks like this:

![Sigmoid][fig_sigmoid]

Nodes can be combined to form a neural network, which aims to approximate any
arbitrary function. Each node in the network has its own inputs, outputs and
activations. Below is an example of a simple feed-forward network. It has
an *input layer*, *hidden layer* and *output layer*.

![Example node][fig_nnet]

It is also possible to have more connections or different number of nodes in
each layer. Recently it has become possible to use more layers with more
computing power and the use of [GPUs][cudnn]. This is often associated with
*deep learning* or *deep neural networks*. But, with more layers a new problem
arises. Let's take a look at that next. By the way, I know that this section
did not go into too much detail about neural networks. Really, there is already
a lot of information on those, so I'm keeping it **really** short.

# Backpropagation and vanishing gradient

Example of vanishing gradient. Use sigmoid function as an example.
This is why sigmoid is dead! Sigmoid derivative max 0.25.
Mention chain rule = in this case product of derivatives!
Many more layers = deep learning. Now this problem arises.


![Sigmoid derivative][fig_sigmoid_deriv]

Maybe mention exploding gradient, as well?

#  A possible solution

How to combat this? Relu of some other activation function?


[fig_sigmoid]: /assets/vanishing-gradient/sigmoid.png
[fig_sigmoidfunc]: /assets/vanishing-gradient/sigmoid_function.png
[fig_sigmoid_deriv]: /assets/vanishing-gradient/sigmoid_deriv.png
[fig_node]: /assets/vanishing-gradient/node.png
[fig_nnet]: /assets/vanishing-gradient/nnet.png

[cudnn]: https://developer.nvidia.com/cudnn
