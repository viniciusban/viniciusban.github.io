---
layout: post
title: The right way to read a file in Python
featured-img: blur-children-class-448877
categories: [english]
---

There are a plethora of examples out there teaching you some wrong way to read a file in Python. Be warned.

What's the problem, afterall?

Most of those examples doesn't close the file when an error occurs and some of them doesn't close the file at all.

Often they rely on Python to close the file when the program ends, but if you get used to copy those examples, you will feed the bad habit to open files with an insecure way.

So, the correct way to read any file in Python is:

```
with open("input.txt") as f:
    for line in f:
        # handle "line"
        # ...

# here the file is already closed.
```

This is the right way to read a file in Python because it's guaranteed to be closed, even if there's an error.

The `with` statement is a _context processor_ and it always closes the file. Even if you get out of it using a `break` or raising an exception.
