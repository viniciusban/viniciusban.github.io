---
layout: post
title: The right way to read a file in Python
summary: There are lots of examples teaching you how to read a file in Python. What are they missing?
description: There are lots of examples teaching you how to read a file in Python. What are they missing?
featured-img: blur-children-class-448877
categories: [english]
---

There are a plethora of examples out there teaching you some wrong way to read a file in Python. Be warned.

Most of them doesn't close the file when an error occurs and several don't close the file at all.

Often they rely on Python to close the file when the program ends (what certainly happens), but if you get used to copy those examples, you will feed the bad habit to open files in an insecure way.

So, the right way to read any file in Python is:

```
with open("input.txt") as f:
    for line in f:
        # handle "line"
        # ...

# here the file is already closed.
```

This is the right way to read a file in Python because it's guaranteed to be closed, even if an error occurs.

The `with` statement is a _context processor_ and it always closes the file. Even if you get out of it using a `break` or raising an exception.
