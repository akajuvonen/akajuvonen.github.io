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

A commonly used activation function is the *sigmoid function*.

![Sigmoid][fig_sigmoid]

![Example node][fig_nnet]

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
[fig_sigmoid_deriv]: /assets/vanishing-gradient/sigmoid_deriv.png
[fig_node]: /assets/vanishing-gradient/node.png
[fig_nnet]: /assets/vanishing-gradient/nnet.png
