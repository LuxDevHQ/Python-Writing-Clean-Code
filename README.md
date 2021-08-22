# Clean Python Code

## Table of Contents

1. [Introduction](#introduction)

2. [Naming Things](#Naming-Things)
   - [Use intention revealing name](#Use-intention-revealing-name)
   - [Meaningful Distinctions](#Meaningful-Distinctions)
   - [Avoid Disinformation](#Avoid-Disinformation)
   - [Pronounceable Names](#Pronounceable-names)
   - [Searchable Names](#Searchable-Names)
   - [Don't be cute](#Don't-be-cute)
   - [Avoid Encodings](#Avoid-Encodings)
     - [Hungarian Notation](#Hungarian-Notation)
     - [Member Prefixes](#Member-Prefixes)
     - [Interfaces & Implementations](#Interfaces-&-Implementations)
   - [Gratuitous Context](#Gratuitous-Context)
   - [Avoid Mental Mapping](#Avoid-Mental-Mapping)
   - [Class Names](#Class-Names)
     - [Types of Objects](#Types-of-Objects)
     - [Simple Superclass Name]()
     - [Qualified Subclass Name]()
   - [Method Names]()
   - [Pick One Word per Concept]()
   - [Don’t Pun]()
   - [Use Solution Domain Names]()
   - [Use Problem Domain Names]()
   - [Add Meaningful Context]()
3. [Functions](#functions)
4. [Objects and Data Structures](#objects-and-data-structures)
5. [Classes](#classes)
6. [SOLID Principles](#SOLID Principles)
   - [Single Responsibility Principle](#Single-Responsibility-Principle)
   - [Open/Close Principle](#Open/Close-Principle)
   - [Liskov Substitution Principle](#Liskov-Substitution)
   - [Interface Segregation Principle](#Interface-Segregation)
   - [Dependency Inversion Principle](#Dependency-Inversion-Principle)
7. [Testing](#testing)
8. [Concurrency](#concurrency)
9. [Error Handling](#error-handling)
10. [Formatting](#formatting)
11. [Comments](#comments)
12. [Translation](#translation)

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://en.wikipedia.org/wiki/Robert_C._Martin#/media/File:Robert_C._Martin_surrounded_by_computers.jpg)

## Introduction

Software engineering principles, from Robert C. Martin's book
[_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
adapted for Python. This is not a style guide. It's a guide to producing readable, reusable, and refactorable software in Python.

## Naming Things

Modern software is so complex that no one can understand all parts of a non-trivial project alone. The only way humans tame details is through abstractions. With abstraction, we focus on the essential and forget about the non-essential. You remember the way you learned body biology?? You focused on one system at a time, digestive, nervous, cardiovascular e.t.c That is abstraction at work.

The most fundamental abstraction in writing software is **naming**. Naming things is just one part of the story, using good names is a skill that unfortunately, is not owned by most programmers.

We name everything from memory locations, data, pieces of code etc. A name can refer to a simple concept like a variable or a function to complex structures like classes, modules, packages and entire programs.

Good names bring order to the chaotic environment of crafting software and hence, we better be good at this skill so that we can enjoy this game.

**[⬆ back to top](#table-of-contents)**

### **Use intention revealing names**

This rule enforces that programmers should make their code read like well written prose by naming parts <br>
of their code perfectly. With such good naming, a programmer will never need to resort to comments or unneccesary <br> docstrings.
Below is a code snippet from a software system. Would you make sense of it without any explanation?

**Bad:**

```python
from typing import List

def f(a : List[List[int]])->List[List[int]]:
    return [i for i in a if i[1] == 0]
```

It would be ashaming that someone would fail to understand such a simple function. What could have gone wrong??

The above code snippet has the worst variable, parameter and function names out there. This code snippet is supposed to take in a bunch of orders and return the pending orders.

It assumes that each order is coded as a list of ints (List[int]) and that the second element is the order status. It assumes 0 means pending and 1 means cleared.

Notice the first problem... that snippet doesn't contain knowledge about the domain. This is a design smell known as a **missing abstraction**. We are missing the Order abstraction.

> **Missing Abstraction** <br>
> This smell arises when clumps of data are used instead creating a class or an interface

We still have a thorny problem right now, we lack meaningful domain abstractions. One of the ways of solving the missing abstraction smell is to **map domain entities**. So lets create an abstraction called Order.

```python
from typing import List

class Order:
    def __init__(self, order_id : int, order_status : int) -> None:
        self._order_id = order_id
        self._order_status = order_status

    def is_pending(self) -> bool:
            return self._order_status == 0:

    #more code goes here
```

Let us now refactor our entity names and use our newly created abstraction too. We arrive at the following code snippet.

**Better:**

```python
def get_pending_orders(orders : List[Order])->List[Order]:
    return [order for order in orders if order.is_pending()]
```

This function reads like well written prose.

Notice that the get_pending_orders function delegates the logic of finding the status to the Order class. This is because the Order class knows its internal representation more than anyone else, so it better implement this logic. This is known as the **Most Qualified Rule** in OOP.

> **Most Qualified Rule** <br>
> Work should be assigned to the class that knows best how to do it.

<font color=blue>You can not achieve good naming with a bad architecture. You can see that mapping domain entities into our code has made our code so simple.</font>

### Meaningful-Distinctions

### Avoid-Disinformation

### Pronounceable-names

### Searchable-Names

### Don't-be-cute

### Avoid-Encodings

#### Hungarian-Notation

#### Member-Prefixes

#### Interfaces-&-Implementations

### Gratuitous-Context

### Avoid-Mental-Mapping

## **SOLID Principles**

### 1. Single-Responsibility-Principle

The single responsibility principle (SRP) instructs developers to write code that has one and only one
reason to change. If a class has more than one reason to change, it has more than one responsibility.
Classes with more than a single responsibility should be broken down into smaller classes, each of
which should have only one responsibility and reason to change.

This section explains that process and shows you how to create classes that only have a single
responsibility but are still useful. Through a process of delegation and abstraction, a class that contains
too many reasons to change should delegate one or more responsibilities to other classes.

It is difficult to overstate the importance of delegating to abstractions. It is the lynchpin of adaptive
code and, without it, developers would struggle to adapt to changing requirements in the way
that Scrum and other Agile processes demand.
