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
6. [SOLID](#solid)
7. [Testing](#testing)
8. [Concurrency](#concurrency)
9. [Error Handling](#error-handling)
10. [Formatting](#formatting)
11. [Comments](#comments)
12. [Translation](#translation)

## Introduction

Software engineering principles, from Robert C. Martin's book
[_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
adapted for Python. This is not a style guide. It's a guide to producing readable, reusable, and refactorable software in Python.

## Naming Things

Modern software is so complex that no one can understand all parts of a non-trivial project. The only way humans tame details is through abstractions. With abstraction, we focus on the essential and forget about the non-essential. The most useful abstraction is **naming**.

We name everything from memory locations, data, pieces of code etc. A name can refer to a simple concept like a variable or a function to complex structures like classes, modules, packages and entire programs.

Names bring order to the chaotic environment of programmers and thus, we better be good at this skill.

### **Use intention revealing name**

This rule enforces that programmers should make their code read like well written prose by naming parts <br>
of their code perfectly. With such good naming, a programmer will never need to resort to comments or unneccesary <br> docstrings.
Below is a code snippet from a software system. Would you make sense of it without any explanation?

```python
from typing import List

def f(a : List[List[int]])->List[List[int]]:
    lst : List[List[int]] = []
    for i in a:
        if(i[1] == 0):
            lst.append(i)
    return lst
```

This is a small piece of code without fancy abstractions. All it contains are generics (List), iterations (loops),
selections (ifs), variables and operators. But why does it seem so cryptic?? Something has gone wrong here.

The above code snippet has the worst variable and parameter and function names out there. The code snippet takes in a bunch of orders and returns pending orders.

It assumes that each order is coded as a list of ints and that the second is the order status. It assumes 0 means pending and 1 means cleared.

This code is so abstract to be useful. It is so impilicit. Let us try to name things well

```python
def get_pending_orders(orders : List[List[int]])->List[List[int]]:
    pending_orders : List[List[int]] = []
    for order in orders:
        if(order[1] == 0):
            pending_orders.append(order)
    return pending_orders
```

Phew!! This is much more readable. We have somehow simplified the code but it still looks so technical. What is this List[List[int]] cryptic piece of code saying to us? What about order[1] == 0?? Let us introduce some abstractions to perfectly model the application domain.

```python
from typing import List

class Order():
    def __init__(self, order_id : int, order_status : int) -> None:
        self._order_id = order_id
        self._order_status = order_status

    def is_pending(self) -> bool:
            return self._order_status == 0:

    #more code goes here
```

We have now introduced a model entity in our code base, in this case an Order class. This order class should know how to check its pending status since its implementation should be hidden from us. We don't know the data structure that was used to implement its details and so clients shouldn't assume anything... <font color=red>order[1] == 0 is illegal!!!</font>

```python
def get_pending_orders(orders : List[Order])->List[Order]:
    pending_orders : List[Order] = []
    for order in orders:
        if(order.is_pending()):
            pending_orders.append(order)
    return pending_orders
```

Phew!! The function now seems too precise to be true. It takes in a list of order and uses the object's contains_loops method to accomplish its task. This code reads like well written prose. It is typical of Object Oriented Code. We have achieved encapsulation and abstraction. Good naming can clear our achictecture significantly.

We now let the Order class implement the logic to find the pending status because it knows better to do that than anyone else. We call this the **Most Qualified Rule**. We have also arrived at the **Encapsulation rule** by letting the Order class impement that ugly logic making our function clean.

> **Most Qualified Rule** <br>
> Work should be assigned to the class that knows best how to do it.

> **The Rule of Encapsulation** <br>
> A class’s implementation details should be hidden from its clients
> as much as possible.

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
