---
layout: post
title:  "Vanishing Gradient Problem in Deep Neural Nets"
---

Preface.

# Some basics

Some neural network basics. Feed-forward. Backpropagation learning.

Many more layers = deep learning. Now this problem arises.

# Example with sigmoid function

Example of vanishing gradient. Use sigmoid function as an example.
This is why sigmoid is dead! Sigmoid derivative max 0.25.
Mention chain rule = in this case product of derivatives!

![Sigmoid][fig_sigmoid]

![Sigmoid derivative][fig_sigmoid_deriv]

Maybe mention exploding gradient, as well?

#  A possible solution

How to combat this? Relu of some other activation function?


[fig_sigmoid]: /assets/vanishing-gradient/sigmoid.png
[fig_sigmoid_deriv]: /assets/vanishing-gradient/sigmoid_deriv.png
