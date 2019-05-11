---
layout: post
title:  "An introduction to untyped lambda calculus"
date:   2019-05-09 09:23:31 +0200
categories: jekyll update
---
# Introduction
This post is a short introduction to the untyped lambda calculus. We will not cover anything thoroughly but instead, look briefly at a couple of interesting concepts. My hope is that this quick tour becomes worthwhile and educational, and perhaps even inspiring enough to make you want to know more. The lambda calculus is not only a fascinating idea from a theoretical perspective but also a useful thing for any functional programmer to understand.

# History
Alonzo Church introduced the untyped lambda calculus in the 1930s, and at first, it was a formal system only used in mathematics. But in the 1950s, John McCarthy used the untyped lambda calculus as a base for his new programming language, Lisp. While the original Lisp might not be considered a functional language by some, it nevertheless helped in creating the programming paradigm we today call "functional programming". 

Over the years, many Lisp dialects have been created, such as Scheme, Racket, and Clojure, making the untyped lambda calculus an important part of modern programming. In fact, lambda calculus is ubiquitous in modern programming -- lambdas, or anonymous functions, are featured in most popular programming languages today. 

In the 1940s, Church introduced the simply typed lambda calculus. This system became the foundation for programming languages such as ML and Haskell. Naturally, this system is also very interesting, but for now, we focus on the untyped lambda calculus.

# Rules and expression
The untyped lambda calculus is a very minimal formal system, consisting of only the following three rules:
* Variables, written as just `x` or `y`.
* Lambda abstractions, written as `(λ x . x)`.
* Application, written as `x y` (we are applying `x` on `y`).

In the untyped lambda calculus, everything is a value, an expression. Smaller expressions are composed together to form larger expression, and ultimately a program, no matter the size, is just one large compound expression. With just these three rules, we can encode a lot of things. In fact, the untyped lambda calculus is Turing complete, meaning that it is equivalent to a Turing machine in its computational power -- thankfully, the untyped lambda calculus is much easier to work with than Turing machines. This kind of power means that we could start this introduction by first defining our own primitives, such as integers, arithmetic operators, etc., in terms of lambda abstractions. However, since this is meant to be a short introduction, we will start on a bit more high-level and assume the existence of the integers and arithmetic operators that act on them.

# Abstractions
To actually compute something, we apply abstractions on variables. Just like functions in math, an abstraction defines an entity that takes one input value, computes something according to a set of rules, and then gives back a value. However, as you have probably noticed, while functions are given names, abstractions are nameless. For a couple of examples, we will use an abstraction equivalent to the successor function. In case you have never seen the successor function, this is what it looks like in Haskell syntax:

```
succ x = x + 1
```

In case you get nervous when there are no parentheses or braces, here is the same function in C:

```
int succ(int x) { return x + 1; } 
```

The successor function is perhaps not the most exciting function, but it will serve as an example. Here's how to write it as a lambda abstraction:

```
(λ x . x + 1)
```

As you can see, abstractions are very similar to functions syntactically speaking. A definition consists of two sections, the head, and the body. The head starts off with the symbol `λ`, followed by a variable name, and finally a closing `.`. Then comes the body, which contains an expression, typically one that refers to the variable names in some way. Now, to start applying abstraction on variables, we first have to define two concepts: substitution and beta reduction.

# Substitution and beta reduction
Substitution is a tool for replacing a variable in a lambda expression with another expression. To illustrate, the syntax for replacing a variable `x` with a variable `y` looks like this:

```
x [x := y]
```

To evaluate the Substitution, we simply replace the leftmost `x` with `y`.

```
x [x := y]
y
```

Beta reduction is the process of using substitution to replace variables in the body of a lambda abstraction with the argument expressions. Using beta reduction, we can now apply `(λ x . x + 1)` on `0`.

```
(λ x . x + 1) 0
(x + 1) [x := 0]
(0 + 1)
 0 + 1
 1
 ```

Since abstractions are expressions just like variables, we should be able to treat them like so. In fact, the untyped lambda calculus treats abstractions as first-class values, meaning that we can put them in variables an use them as input and output of another abstraction. Now, let's try that by first applying the identity abstraction, or `(λ x . x)`, on our successor abstraction:


```
(λ x . x) (λ x . x + 1)
(x [x := (λ x . x + 1)])
(λ x . x + 1)
 ```

# Currying
We previously stated that abstractions, like functions in math, take only one parameter. However, that is not really a limitation, thanks to the fact that abstractions are just expressions that we can return from another abstraction. We can define an abstraction that adds two numbers together like this:

```
(λ x . (λ y . x + y))
```

Adding the numbers `1` and `2` then looks like this:

```
(λ x . (λ y . x + y)) 1 2
(λ y . x + y) [x := 1] 2
(λ y . 1 + y) 2
(1 + y) [y := 2]
(1 + 2)
 1 + 2
 3
 ```

We can generalize this. Any multiple parameter abstraction can be constructed by nesting abstractions that return other abstractions. Since we can nest abstractions arbitrarily, we can construct abstractions that take any number of parameters. This concept is called currying and is used in some way in most functional programming languages. In ML and Haskell, functions are curried by default (although only when necessary, because of the incurred runtime overhead). Other languages, like most Lisp dialects, require you to first explicitly specify currying, often with the help of a function `curry`.

# Recursion
A lot of times in computer science and programming we have to perform some action several times in succession. In imperative programming languages, we often choose to encode such patterns with loops. However, just like in functional languages, in the untyped lambda calculus, we use self-application or recursion. To cover the very basics of recursion in untyped lambda calculus, we will take a look at the simplest possible recursive lambda abstraction, the infinite loop:

```
(λ x . (λ x . x x) (λ x . x x))
```

This definition might look odd and cumbersome at first but really it is a clear and compact way to illustrate the essence of recursion. Put into words, the body of the abstraction defines an abstraction that will apply its input to itself and then applies a copy of it to itself. Now let's evaluate a couple of iterations of the loop by applying i on an dummy variable `x`:

```
(λ x . (λ x . x x) (λ x . x x)) x
(λ x . x x) (λ x . x x) [x := x]
(λ x . x x) (λ x . x x)
(λ x . x x) [x := (λ x . x x)]
(λ x . x x) (λ x . x x)
(λ x . x x) [x := (λ x . x x)]
(λ x . x x) (λ x . x x)
```

Of course, we could go on like this forever, but that should be enough for you to get the idea. 

# Where to go from here
We covered the basics of creating and evaluating expressions in the untyped lambda calculus. We also looked briefly at currying and recursion. But, where should you go from here if you want to know more about lambda calculus? I suggest you start by reading the [wikipedia article](https://en.wikipedia.org/wiki/Lambda_calculus). If you prefer to read books, I recommend *An Introduction to Functional Programming Through Lambda Calculus*  by Greg Michealson. If you are really into the theory and also enjoy in programming in statically typed languages like ML and Haskell, *Types and Programming Languages* by Benjamin C. Pierce is a must read.
