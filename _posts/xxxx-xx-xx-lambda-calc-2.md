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
second = lambda p: p(false)

p = pair(one)(two)
to_digit(first(p))
1
to_digit(second(p))
2
```

## Factorial using pairs

I've been listing a bunch of functions, and I think it's time we do some actual problem solving using what we have defined. Maybe we could try to calculate factorial. We haven't yet defined anything that allows us to use recursion, but we can come up with an iterative solution using pairs.

Let's take a look at an example. For example, let's define a pair so that the first element is `n` and the second `factorial(n)`. Going through a few iterations, it would like this:

```
first | second
------+-------
  0   |   1
  1   |   1
  2   |   2   # 2nd element is 1*2
  3   |   6   # 2nd element 2*3
  4   |  24   # 2nd element 6*4
```

What we can see immediately is that the first element of the pair is just incremented by one each time, and the second element is `first element of this pair * second element of previous pair`. First, we can define a function that takes one pair and returns the next. In Python this would normally be something like (using tuples):

```python
def next_pair(pair: Tuple[int, int]) -> Tuple[int, int]:
    first, second = pair
    return (first + 1, (first + 1) * second)
```

Lambda calculus version is exactly the same:

```python
next_pair = lambda p: pair(succ(first(p)))(mult(succ(first(p)))(second(p)))

result = next_pair(pair(zero)(one))
to_digit(first(result))
1
to_digit(second(result))
1

result = next_pair(pair(one)(one))
to_digit(first(result))
2
to_digit(second(result))
2

result = next_pair(pair(two)(two))
to_digit(first(result))
3
to_digit(second(result))
6
# and so on...
```

At this point we have basically solved the problem. All we need to now is to repeat this step `n` times and return the second element of the pair. That is our factorial. In this case `n(next_pair)` is a function that executes `next_pair` `n` times. If we apply this to our base case of `pair(zero)(one)`, we get what we want (returning just the second element):

```python
fact = lambda n: second(n(next_pair)(pair(zero)(one)))
four = succ(three)  # I think we did not yet define number four, let's do it now

to_digit(fact(zero))
1
to_digit(fact(one))
1
to_digit(fact(two))
2
to_digit(fact(three))
6
to_digit(fact(four))
24
```

And there it is! In fact, untyped lambda calculus is Turing-complete, meaning we could theoretically solve any computational problem using only it. Of course it's more complicated in practice, and solving practical programming tasks using only lambda calculus will become impractical very quickly.

[1]: https://en.wikipedia.org/wiki/Church_encoding#Church_Booleans