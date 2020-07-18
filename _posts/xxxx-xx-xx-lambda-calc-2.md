---
layout: post
title:  "Lambda Calculus: Beyond Integers"
---

I wrote about lambda calculus and how to encode integers using only function with Church numerals in a [previous post](2020-07-14-lambda-calc.md). This time I would like to take look at how to encode other datatypes within the same restrictions, namely booleans, pairs and lists. I will be reusing some of the things introduced previously, so check that post if you haven't already.

Let's look at booleans (`true` or `false`) next. Just like for integers, we need some kind of encoding for boolean values that still work well when performing computations. Using the same notation as before, we can choose `TRUE := λx.λy.x` and `FALSE := λx.λy.y`, not surprisingly known as *Church booleans*.

```python
true = lambda x: lambda y: x
false = lambda x: lambda y: y
```

You might notice that the definition of `false` above is the same as `zero` that we defined before.