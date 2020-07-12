---
layout: post
title:  "Calculations without Integers: Lambda Calculus and Church Numerals"
---

Lambda calculus is an abstract theory of computation which includes only variables, functions and function applications. Without going into too much detail, I became interested in it after realizing it introduces an interesting problem: how to perform calculations without numbers (e.g., integers) when all we have is functions? And how could this be implemented using some programming language? Let's try to do something in Python.

A note on syntax: `λx.x` would be `lambda x: x` in Python, `λf.λx.f x` is `lambda f: lambda x: f(x)` and so on. Also, function applications are left associative, meaning `x y z` would be `x(y)(z)`.

Back to numbers. Clearly we need some kind of way to encode numbers using only functions. Something has to represent a number, e.g., one or three. Since we have only functions and function applications at our disposal, one way to encode numbers is to interpret the number of times we apply a function as the value. Think of it this way: when a function is applied to a variable one time, this could be interpreted as number one. Zero applications would be zero, two times equals two and so on. The resulting numerals are called *Church numerals*. This choice of number encoding is technically arbitrary, but it allows us to perform calculation really nicely.

- 0 := λf.λx.x
- 1 := λf.λx.f x
- 2 := λf.λx.f (f x)

I will be using Python's lambda functions in this post. Keep in mind that usually giving a name to a lambda function is an anti-pattern, but I'll make an exception in this case for clarity. Python lambdas can be thought of as a superset of actual lambda expressions in lambda calculus, since Python allows things like multiple arguments, e.g., `lambda x, y: x + y` which is not allowed.

Let's introduce an implementation on Church numerals, where the number of function applications equals the value:

```python
zero = lambda f: lambda x: x
one = lambda f: lambda x: f(x)
two = lambda f: lambda x: f(f(x))
```

As we will be doing some computations using these numerals, it would be nice to be able to decode a Church numeral into digits, just to confirm that we are doing things correctly. I'll use this helper function for that:

```python
def to_digit(f):
    def counter(x):
        return x + 1
    return f(counter)(0)
```

We pass a counter function to a numeral, where each function application increments the counter by one. For example:

```python
to_digit(zero)
0
to_digit(one)
1
```

Now we can start implementing some useful functions that perform calculations using our numerals. Figuring these out by yourself is a challenge of its own, but in this post I'm more interested in implementing some of them and seeing them in action. Some are more intuitive than others.

Let's start with the successor function, which can be defined as `SUCC := λn.λf.λx.f (n f x)`. In this function the function `f` is applied to `x` `n` times (`n f x`). When we add one more function application (`f (n f x)`) we get the next number according to the definition of numbers we introduced above. In Python it would be this:

```python
succ = lambda n: lambda f: lambda x: f(n(f)(x))
three = succ(two)
to_digit(three)
3
```

Addition has more than one possible definition. To keep it simple, let's use `succ` defined above to our advantage. Since successor function gives us the next number, we could use it by applying it to a number several times to get the results we are looking for. In lambda calculus it would be `PLUS := λm.λn.m SUCC n`, which does what we just described. For example, `3 + 2` can be obtained by applying `succ` to `3` and doing it `2` times, resulting in `3 + 1 + 1 = 5` (or the other way round). 

```python
plus = lambda m: lambda n: m(succ)(n)
result = plus(three)(two)
to_digit(result)
5
```

As mentioned before, functions in lambda calculus can only take one argument. Therefore, in the above example `plus(three)` returns a function that is then applied to `two` to get `five` (although the final result itself is another function, it just happens to be the one we have defined as number five).

Once again, multiplication can be defined in a couple of ways, but here I'll just use `MULT := λm.λn.m (PLUS n) 0`, which just means adding `n`, doing it `m` times and applying that to zero.

```python
mult = lambda m: lambda n: m(plus(n))(zero)
result = mult(2)(3)
to_digit(result)
6
```

Pred is more difficult: `PRED := λn.λf.λx.n (λg.λh.h (g f)) (λu.x) (λu.u)`. There are other definitions, but we need other things that we haven't defined yet.

```python
pred = lambda n: lambda f: lambda x: n(lambda g: lambda h: h(g(f)))(lambda u: x)(lambda u: u)
to_digit(pred(three))
2
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