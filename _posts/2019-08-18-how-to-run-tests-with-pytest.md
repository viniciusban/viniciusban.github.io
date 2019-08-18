---
layout: post
title: How to run tests with pytest
summary: This is a self reminder about pytest invocation
featured-img: don-t-forget-1434063
categories: [english]
---

I always forget how to run `pytest` for a specific target. So, here it goes.

For paths:

```
$ pytest some/directory
$ pytest path/to/file.py
$ pytest path/to/file.py::test_function
$ pytest path/to/file.py::SomeClass
$ pytest path/to/file.py::SomeClass::test_method
```

For packages and/or modules:

```
$ pytest --pyargs some.package  # a.k.a directory
$ pytest --pyargs some.package.module  # a.k.a file
$ pytest --pyargs some.package.module::test_function
$ pytest --pyargs some.package.module::SomeClass
$ pytest --pyargs some.package.module::SomeClass::test_method
```

