---
layout: post
title:  "Calculations without Integers: Lambda Calculus and Church Numerals"
---

TODO: intro.

- 0 := λf.λx.x
- 1 := λf.λx.f x
- 2 := λf.λx.f (f x)

```python
zero = lambda f: lambda x: x
one = lambda f: lambda x: f(x)
two = lambda f: lambda x: f(f(x))
```

SUCC := λn.λf.λx.f (n f x)

```python
succ = lambda n: lambda f: lambda x: f(n(f)(x))
three = succ(two)
```