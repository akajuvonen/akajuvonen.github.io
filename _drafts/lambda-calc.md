---
layout: post
title:  "Calculations without Integers: Lambda Calculus and Church Numerals"
---

What is Lambda Calculus.

Introduce Church numerals.

- 0 := λf.λx.x
- 1 := λf.λx.f x
- 2 := λf.λx.f (f x)

Python `lambda` vs Lambda Calculus. Similar, but not the same. Python lambda is a superset, since it can take more than one argument, e.g., `lambda x, y: x + y`.

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

`SUCC := λn.λf.λx.f (n f x)`

```python
succ = lambda n: lambda f: lambda x: f(n(f)(x))
three = succ(two)
to_digit(three)
3
```

Addition has more than one possible definition. To keep it simple, let's use `succ` defined above:

`PLUS := λm.λn.m SUCC n` -> untuitively applying successor function to `n`, doing it `m` times. 

```python
plus = lambda m: lambda n: m(succ)(n)
result = plus(three)(two)
to_digit(result)
5
```

NOTE: currying above.

Once again, multiplication can be defined in a couple of ways, but here I'll just use `MULT := λm.λn.m (PLUS n) 0`, which just means adding `n`, doing it `m` times and applying that to zero.

```python
mult = lambda m: lambda n: m(plus(n))(zero)
result = mult(2)(3)
to_digit(result)
6
```

Pred is more difficult: `PRED := λn.λf.λx.n (λg.λh.h (g f)) (λu.x) (λu.u)`. There are other definitions, but we need other things that we haven't defined yet.

```python
lambda n: lambda f: lambda x: n(lambda g: lambda h: h(g(f)))(lambda u: x)(lambda u: u)
```

Logic.

By convention, booleans are encoded as `TRUE := λx.λy.x` and `FALSE := λx.λy.y`.

```python
true = lambda x: lambda y: x
false = lambda x: lambda y: y
```

Note that `false` is the same as `zero`, since we could also write it as `lambda f: lambda x: x` as before.

Pairs.

Lists.