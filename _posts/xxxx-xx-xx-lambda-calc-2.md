---
layout: post
title:  "Lambda Calculus: Beyond Integers"
---

I wrote about lambda calculus and how to encode integers using only function with Church numerals in a [previous post](2020-07-14-lambda-calc.md). This time I would like to take look at how to encode other datatypes within the same restrictions, namely booleans, pairs and lists. I will be reusing some of the things introduced previously, so check that post if you haven't already.

# Booleans

Let's look at booleans (`true` or `false`) next. Just like for integers, we need some kind of encoding for boolean values that still work well when performing computations. Using the same notation as before, we can choose `TRUE := λx.λy.x` and `FALSE := λx.λy.y`, not surprisingly known as [Church Booleans][1].

```python
true = lambda x: lambda y: x
false = lambda x: lambda y: y
```

You might notice that the definition of `false` above is the same as `zero` that we defined before.

I'm not going to add everything into this post since you can check Wikipedia and literally copy the definitions from there and try things out for yourself. Instead, let's define a few things and try them out to see if they work as intended.

```python
if_then_else = lambda p: lambda a: lambda b: p(a)(b)  # if p then a, else b
is_zero = lambda n: n(lambda _: false)(true)  # return true is n is zero, else false
```

Let's test these out:

```python
result = if_then_else(is_zero(zero))(one)(two)  # return one is zero is zero
to_digit(result)
1

result = if_then_else(is_zero(three))(one)(two)  # return two since one is not zero
to_digit(result)
2
```

Cool. Check the link above for more things, like `and`, `or` and `xor`.

# Pairs

A pair is a datatype that allows us to access either the first of the second element. A pair is defined as `PAIR := λx.λy.λf.f x y`. We can use Church booleans to easily define the functions `FIRST := λp.p TRUE` and second `SECOND := λp.p FALSE`.

```python
pair = lambda x: lambda y: lambda f: f(x)(y)
first = lambda p: p(true)
second lambda p: p(false)

p = pair(one)(two)
to_digit(first(p))
1
to_digit(second(p))
2
```

[1]: https://en.wikipedia.org/wiki/Church_encoding#Church_Booleans