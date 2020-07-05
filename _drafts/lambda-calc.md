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

Helper function:

```python
def to_digit(f):
    def counter(x):
        return x + 1
    return f(counter)(0)
```

Resulting in
```python
to_digit(zero)
0
to_digit(one)
1
```

SUCC := λn.λf.λx.f (n f x)

```python
succ = lambda n: lambda f: lambda x: f(n(f)(x))
three = succ(two)
```

Addition has more than one possible definition. To keep it simple, let's use `succ` defined above:

PLUS := λm.λn.m SUCC n -> untuitively applying successor function to `n`, doing it `m` times. 

```python
plus = lambda m: lambda n: m(succ)(n)
```

Once again, multiplication can be defined in a couple of ways, but here I'll just use `MULT := λm.λn.m (PLUS n) 0`, which just means adding `n`, doing it `m` times and applying that to zero.

```python
mult = lambda m: lambda n: m(plus(n))(0)
```

TODO: PRED.