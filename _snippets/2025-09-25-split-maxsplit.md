---
title: "Python split: using maxsplit"
date: 2025-09-25
tags: [python, tips]
---

## Using maxsplit for str.split

Was working on an issue where we were at some point in a service were generating an identifier of the form `foo-bar`, where `foo` could be our internal identifier and `bar` an external identifier.

In the code at some point we needed to split those two, and the code that was working was:

```python
key = 'foo-bar'
id_one, id_two = key.split("-")
```

Which worked just well, but the issue is that at some point we got `foo-bar-xyz` and as you'd expect an unpack error was raised:

```python
>>> id_one, id_two = key.split("-")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: too many values to unpack (expected 2)
```

What was my initial solution? This:

```python
>>> id_one, id_two = key.split("-")[0], "-".join(str(x) for x in key.split("-")[1:])
>>> id_one
'foo'
>>> id_two
'bar-xyz'
```

And here is when my colleague dropped a comment:
> Why don't you use maxsplit?

From the Python docs for [str.split](https://docs.python.org/3/library/stdtypes.html#str.split):

> If maxsplit is given, at most maxsplit splits are done (thus, the list will have at most maxsplit+1 elements).

So we can just do:

```python
>>> key.split("-", maxsplit=1)
['foo', 'bar-xyz']
```

This ensures the first separator splits the string, and the remainder stays intact.

### Closing

I can't recall how many times I did the split-then-join hack when I only wanted the first split. `maxsplit` is the clean way to handle that.
