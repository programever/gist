# Functional Programming

```
A programming paradigm in which we treat all our computations as pure mathematical functions that are side-effects free, without any mutations.
```

```
Functions that do not contain any side-effects and mutations inside their implementation, are Pure Functions.
```

A pure function:

- Input X always return Y result. Eg: A function that return Date.now() is not valid here

- No side-effect: Eg: A function that contains `console.log('I am side-effect'); return 1;`

- No mutations

___Concepts:___

- First-class and higher-order functions

- Applying and composing functions

- Pure functions

- TypeClass base on Algebra as a branch of mathematics

- Read this: https://leanpub.com/fp-made-easier


# Object-oriented Programming

A programming paradigm based on the concept of "objects".

There is usually a special name such as `this` or `self` used to refer to the current object.

There are tons of `Design Patterns` for reading.

___Concepts:___

- Objects and classes

- Design Patterns https://en.wikipedia.org/wiki/Object-oriented_programming#Design_patterns


# Reactive Programming

```
An asynchronous programming paradigm where we process data streams to propagate changes in our code.
```

Basically, it have Observable + Observers + Schedulers.

Context is making http request:

- Observable: ["1", "2", "3", "4"] - the transfer data

- Observers: onError, onCompleted, onData - the events

- Schedulers: spawn a new thread to run this asynchronous call


# Functional Reactive Programming

```
A reactive programming paradigm using the building blocks of functional programming.
```

It's reactive streams combined with functional operators. Eg: map, filter, zip, fold,...

Read more: https://blog.danlew.net/2017/07/27/an-introduction-to-functional-reactive-programming


# THE WAR: FP vs OOP

Go and read yourself!

