---
layout: post
title:  "Calculations without Integers: Lambda Calculus and Church Numerals"
---

TODO: intro.

```python
zero = lambda f: lambda x: x
one = lambda f: lambda x: f(x)
two = lambda f: lambda x: f(f(x))
```