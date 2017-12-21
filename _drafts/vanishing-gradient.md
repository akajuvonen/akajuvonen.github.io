---
layout: post
title:  "Vanishing Gradient Problem in Deep Neural Nets"
---

Preface.

# Neural network basics

Let's start with some neural network basics. I'm not going to go into too much
detail because there are already countless resources available.

A neural network consists of *neurons* (or nodes). They take input data multiplied by
weights and add these together. After this, the aggregated sum is input into
an activation function, which determines the output of the node. Optimizing
the weights is essentially what a neural network does when it's learning.

![Example node][fig_node]

A commonly used activation function is the *sigmoid function*:

![Sigmoid function][fig_sigmoidfunc]

If we plot it, it looks like this:

![Sigmoid][fig_sigmoid]

Individual neurons can be combined to form a neural network, which aims to approximate any arbitrary function. Each node in the network has its own inputs, outputs and activations. Below is an example of a simple feed-forward network.
It has an *input layer*, *hidden layer* and *output layer*.

![Example node][fig_nnet]

It is also possible to have more connections or different number of neurons in
each layer. Recently it has become possible to use more layers with more
computing power and the use of [GPUs][cudnn]. This is often associated with
*deep learning* or *deep neural networks*. But, with more layers a new problem
arises. Let's take a look at that next. By the way, I know that this section
did not go into too much detail about neural networks. Really, there is already
a lot of information on those, so I'm keeping it **really** short.

# Backpropagation and vanishing gradient

In practice, neural network learning is done by optimizing the weights.
This can be performed by using [backpropagation][backprop]. We check error
for each layer, and adjust the weights in the appropriate direction.
This error is then propagated all the way back to the first layer. To do this
in practice, [gradient descent][gradientdesc] algorithm is often used.
We need to have a loss function. The gradient tells us which way we need to go
to get smaller values for that function (if we move in the negative direction).
This optimization gives us the *local minimum* of the loss function after some
iterations.

Calculating the gradient involves the use of derivative of the activation
function. We can calculate the gradient for different layers of the network
using the [chain rule][chainrule]. At this point it's enough to know that
we are basically calculation a *product of derivatives*. We end up multiplying
*n* gradients for *n* layers in the network.

Gradient, derivative, chain rule (product of derivatives).

Deriv of sigmoid below.

![Sigmoid derivative][fig_sigmoid_deriv]

Problem with many layers.

Maybe mention exploding gradient, as well?

#  A possible solution

How to combat this? Relu of some other activation function?


[fig_sigmoid]: /assets/vanishing-gradient/sigmoid.png
[fig_sigmoidfunc]: /assets/vanishing-gradient/sigmoid_function.png
[fig_sigmoid_deriv]: /assets/vanishing-gradient/sigmoid_deriv.png
[fig_node]: /assets/vanishing-gradient/node.png
[fig_nnet]: /assets/vanishing-gradient/nnet.png

[cudnn]: https://developer.nvidia.com/cudnn
[backprop]: https://en.wikipedia.org/wiki/Backpropagation
[gradientdesc]: https://en.wikipedia.org/wiki/Gradient_descent
[chainrule]: https://en.wikipedia.org/wiki/Chain_rule
