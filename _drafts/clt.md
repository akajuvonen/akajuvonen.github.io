---
layout: post
title: "Understanding LLN and CLT with Coin Toss"
---

In this post, I'm going to briefly discuss two simple but important concepts
in probability theory: law of large numbers (LLN) and central limit theorem
(CLT). I'm also going to explain the significance of these theorems and
present visualizations using an example.

# Law of large numbers

[Law of large numbers][lln] or LNN is a theorem in probability theory. It
tells us what happens when you repeat an experiment many times. It states
that the average result of the experiment will get closer and closer to the
expected value as we keep repeating the experiment.

Consider flipping a coin multiple times. If you just do it, say, 5 times,
you can end up with, e.g., getting heads 1 time and tails 4 times. But if
we keep flipping the coin many times, the ratios of heads and tails wil start
to get close to the image below.

![Flips][fig_clt_flips]

This means that on average about 50% of coin flips be heads (and tails).
LLN seems deceptively simple, and it probably is, but it's also an important
observation that has many real-life applications. For example,
[Monte Carlo methods][mcm] use this theorem. In addition, casinos can use it to
guarantee long-term outcomes (and make money).

# Central limit theorem

CLT. [Link][clt].

![Example][fig_clt_cointoss]

[fig_clt_flips]: /assets/clt/flips.png
[fig_clt_cointoss]: /assets/clt/ex.png

[lln]: https://en.wikipedia.org/wiki/Law_of_large_numbers
[clt]: https://en.wikipedia.org/wiki/Central_limit_theorem
[mcm]: https://en.wikipedia.org/wiki/Monte_Carlo_method
