---
layout: post
title: A Pythonic and powerful approach to build test data
summary: Things can become complicated when generating test data, but Python helps us achieve it in a very simple way.
featured-img: blue-sky-chimney-clouds-208714
categories: [english]
---

You write automated tests for your system and, for some reason, you don't want to use an external library like [ModelMommy](https://model-mommy.readthedocs.io/en/latest/) or [factory_boy](https://factoryboy.readthedocs.io/en/latest/). So you decide to write your own factories. How would you implement them?

Let's suppose we want to generate test data to a `User` class with 3 attributes: `name`, `profile` and `company`. The class definition is:

```
class User:
    name = ""
    profile = ""
    company = ""

    def __init__(self, **kwargs):
        self.name = kwargs["name"]
        self.profile = kwargs["profile"]
        self.company = kwargs["company"]

    def __str__(self):
        return self.name
```

Note: `User` could be a Django model, but for didatical purpose in this article I'm using a simple class.

A factory should do some work for us. Usually it uses some default values to avoid duplication in tests.


## The very basic

The most obvious and first approach would be writing a simple function, like this:

```
def UserTestFactory():
    return User(
        name="John Smith",
        profile="simple",
        company="ACME Corporation")
```

Now you have a factory creating a `User` instance with default data. You can call it this way:

```
u = UserTestFactory()
```

Yet it is very simple and can serve extremely basic cases, it has an obvious flaw: if you want to use some custom value to a given attribute, you must modify the returned instance:

```
mary = UserTestFactory()
mary.name = "Mary Poppins"
```

Mutability is frequently a bad idea, so let's move on to a more powerful alternative.


## Overriding default values

```
def UserTestFactory(**kwargs):
    data = {
        name: "John Smith",
        profile: "simple",
        company: "ACME Corporation",
    }
    data.update(kwargs)
    return User(**data)
```

This new version allows us to create an `User` without modifying the returned instance, like this:

```
mary = UserTestFactory(name="Mary Poppins")
```

Much better, right? We can pass only those attributes with custom value to attend our test. All the others assume their defaults.

When we create similar objects (very common situation in tests), some repetition is necessary:

```
mary = UserTestFactory(name="Mary Poppins", profile="master")
mickey = UserTestFactory(name="Mickey Mouse", profile="master")
```

What do these users above have in common? All of them are "masters". How about creating them without repeating that `profile="master"` part?


## What we are aiming

It would be nice to create those instances of `User` this way:

```
master = UserTestFactory(profile="master")
mary = master.but(name="Mary Poppins").build()
mickey = master.but(name="Mickey Mouse").build()
```

We got rid of the repeated argument (`profile`). Instead, we have the `master` variable, serving as the foundation to all master users.

Let's see a pythonic way to do that.


## A real factory

First of all we'll transform our factory into a class and add some simple methods to give it the power to create similar instances without code repetition.

Let's see the code:

```
class UserTestFactory:
    defaults = {
        name: "John Smith",
        profile: "simple",
        company: "ACME Corporation",
    }

    def __init__(self, **kwargs):
        data = self.defaults.copy()
        data.update(kwargs)
        self._data = data

    def but(self, **kwargs):
        data = self._data.copy()
        data.update(kwargs)
        return self.__class__(**data)

    def build(self):
        return User(**self._data)
```

Don't be intimidated with some details. Let's understand all of it.

Until now our factory simply returned the class instance we wanted. Despite it's being enough for almost all cases, a real factory should be more flexible. So, from now on we'll differentiate two roles:

1. The factory
2. The builder

The factory always return a factory. Itself or another instance of its class. This is what `__init__()` and `but()` methods above do.

The builder uses the data and builds the instance we're looking for. This is what the `build()` method does. Thus, to have an instance of `User`, we should use `build()`, like this:

```
mary = UserTestFactory(name="Mary Poppins").build()
```

But, when calling `UserTestFactory` without `build()`, we get an instance of `UserTestFactory`, the factory. This separation allows us to do this:

```
master = UserTestFactory(profile="master")
```

Or this:

```
journalist = UserTestFactory(company="Daily Planet")
```

These instructions above call the `__init__()` method of the `UserTestFactory` class. What does it return? An instance of `UserTestFactory`, not an instance of `User`, as some should suppose.

But we need another helper to create another factory instance based on the current one. That's where the `but()` method comes in.

The advantage is that we can create similar objects using a very clear syntax, turning our tests more readable, like this:

```
journalist = UserTestFactory(company="Daily Planet")
clark = journalist.but(name="Clark Kent").build()
lois = journalist.but(name="Lois Lane").build()
```

## Exploring it a bit more

If you need to build many journalists in your tests, you could create a specific factory, adding a class' method called `journalist()`, like this:

```
class UserTestFactory:
    defaults = {
        name: "John Smith",
        profile: "simple",
        company: "ACME Corporation",
    }

    def __init__(self, **kwargs):
        data = self.defaults.copy()
        data.update(kwargs)
        self._data = data

    @classmethod
    def journalist(cls, **kwargs):
        data = {"company": "Daily Planet"}
        data.update(kwargs)
        return cls(**data)

    def but(self, **kwargs):
        data = self._data.copy()
        data.update(kwargs)
        return self.__class__(**data)

    def build(self):
        return User(**self._data)
```

And use in your tests:

```
journalist = UserTestFactory.journalist()
clark = journalist.but(name="Clark Kent").build()
lois = journalist.but(name="Lois Lane").build()
```


## Some valid questions

Q: Why do we use factories if we have mocks?

A: Factories and mocks aren't mutualy exclusive. Frequently they complement themselves, using real instances created by factories to populate data returned by mocked functions, avoiding touching the database and speeding up the tests.

Q: Isn't it too complicated to get only a copy of an instance?

A: Not at all. As we demonstrated, we can create many factories based on specific cases, simplify our tests and let their intention much more explicit.

Q: Why did you named the method `but()`?

A: Some people like to use `with()`, but it is a Python reserved keyword because of the `with` statement. So, the method name should be `with_()`. I think `but()` is better than `with_()`, but you can name whatever you want.


## Conclusion

We learned why separating the factory from the build method is a good idea to simplify test methods and clarify their intentions.

Also, we learned how to write a powerful factory class that doesn't mutate state, create a copy of itself with some different values and builds an instance of the target class.

Last but not least, we saw some Python tricks (classmethod, kwargs, etc.) that enable a porwerful and simple codebase.
