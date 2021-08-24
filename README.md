# Clean Python Code

## Table of Contents

![Uncle Bob surrounded with computers](Robert_C._Martin_surrounded_by_computers.jpg)

1. [Introduction](#introduction)

2. [Naming Things](#Naming-Things)
   - [Use intention revealing name](#Use-intention-revealing-name)
   - [Meaningful Distinctions](#Meaningful-Distinctions)
   - [Avoid Disinformation](#Avoid-Disinformation)
   - [Pronounceable Names](#Pronounceable-names)
   - [Search-able Names](#Searchable-Names)
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
   - [Small](#Small)
   - [Do One Thing](#Do-One-Thing)
   - [One level of Abstraction](#One-level-of-Abstraction)
   - [Avoid Conditionals](#Avoid-Conditionals)
   - [Use Descriptive Names](#Use-Descriptive-Names)
   - [Function Arguments](#Function-Arguments)
   - [Avoid Side Effects](#Avoid-Side-Effects)
     - [Pure Functions](#Pure-Functions)
     - [Niladic Functions](#Niladic-Functions)
     - [Argument Mutation](#Argument-Mutation)
     - [Exceptions](#Exceptions)
     - [I/O](#I/O)
   - [Command Query Separation](#Command-Query-Separation)
   - [Don't Repeat Yourself](#Don't-Repeat-Yourself)
4. [Objects and Data Structures](#objects-and-data-structures)
5. [Classes](#classes)
6. [SOLID Principles](#SOLID-Principles)
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
---

This rule enforces that programmers should make their code read like well written prose by naming parts <br>
of their code perfectly. With such good naming, a programmer will never need to resort to comments or unnecessary <br> doc strings.
Below is a code snippet from a software system. Would you make sense of it without any explanation?

**Bad: :angry: **

```python
from typing import List

def f(a : List[List[int]])->List[List[int]]:
    return [i for i in a if i[1] == 0]
```

It would be ashaming that someone would fail to understand such a simple function. What could have gone wrong??

The problem is simple.This code is littered with **mysterious names**. We have to agree that this is code and not a detective novel. Code should be clear and precise.

What this code does is so trivial. It takes in a collection of orders and returns the pending orders. Let's pause for a moment and appreciate the extreme over engineering in this solution.
The programmer assumes that each order is coded as a list of `ints` (`List[int]`) and that the second element is the order status. He decides that 0 means pending and 1 means cleared.

Notice the first problem... that snippet doesn't contain knowledge about the domain. This is a design smell known as a **missing abstraction**. We are missing the Order abstraction.

> @icon-info-circle **Missing Abstraction** <br>
> This smell arises when clumps of data are used instead creating a class or an interface

We have a thorny problem right now, we lack meaningful domain abstractions. One of the ways of solving the missing abstraction smell is to **map domain entities**. So lets create an abstraction called Order.

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

> @icon-info-circle We could also have used the **namedtuple** in the python standard library but we won't be too functional that early. Let us stick with OOP for now. **namedtuples** contain only data and not data with code that acts on it.

Let us now refactor our entity names and use our newly created abstraction too. We arrive at the following code snippet.

**Better: :smile:**

```python
def get_pending_orders(orders : List[Order])->List[Order]:
    return [order for order in orders if order.is_pending()]
```

This function reads like well written prose.

Notice that the `get_pending_orders()` function delegates the logic of finding the order status to the Order class. This is because the Order class knows its internal representation more than anyone else, so it better implement this logic. This is known as the **Most Qualified Rule** in OOP.

> @icon-info-circle **Most Qualified Rule** <br>
> Work should be assigned to the class that knows best how to do it.


> @icon-info-circle We are using the listcomp for a reason. Listcomps are examples of **iterative expressions**. They serve one role and that is creating lists. On the other hand, for-loops are **iterative commands** and thus accomplish a myriad of tasks. Pick the right tool for the job. 

Never allow client code know your implementation details. In fact the ACM A.M Laureate Babra Liskov says it soundly in her book [Program development in Java. Abstraction, Specification and OOD](https://book4you.org/book/1164544/93467d). The **Iterator design pattern** is one way of solving that problem.

Here is another example of a misleading variable name.

**Bad** :angry:
```python
student_list= {'kasozi','vincent', 'bob'}
```

This variable name is so misleading.
* It contains noise. why the list suffix?
* It is lying to us. Lists are not the same as sets. They may all be collections but they are not the same at all.

To prove that lists are not sets, below is a code snippet that returns the methods in the List class that aren't in the Set class.

```python
sorted(set(dir(list())) - set(dir(set())))
```
Once it has executed, `append()` is one of the returned functions implying that sets don't support `append()` but instead support `add()`. So you write the code below, your code breaks.

>@icon-info-circle Sets are not sequences like lists. In fact, they are unordered collections and so adding the `append()` method to the set class would be misleading. `append()` means we are adding at the end which may not be the case with sets.

**Bad** :angry:
```python
student_list= {'kasozi','vincent', 'bob'}
student_list.append('martin') #It breaks!!
```
It is better to use a different variable name is neutral to the data structure being used.
In this case, once you decide to change data structure used, your variable won't destroy the semantics of your code.

**Good** :smile:
```python
students = {'kasozi', 'vincent', 'bob'}
```
> @icon-info-circle You can not achieve good naming with a bad design. You can see that mapping domain entities into our code has made our codebase use natural names.

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

## **Functions**

### **Small**

### Do-One-Thing

### One-level-of-Abstraction

### Avoid-Conditionals

### Use-Descriptive-Names

### Function-Arguments

### Avoid-Side-Effects

#### **Pure Functions**

What on earth is a pure function?? Well, adequately put, a pure function is one without side effects.
Side effects are invisible inputs and outputs from functions. In pure Functional programming,functions behave like mathematical functions. Mathematical functions are transparent-- they will always return the same output when given the same input. Their output only depends on their inputs.

Below are examples of functions with side effects.

#### **Niladic-Functions**

```python
class Customer:
    def __init__(self, first_name : str)-> None:
        self.first_name = first_name

    #This method is impure, it depends on global state.
    def get_name(self):
        return self.first_name

    #more code here
```

Niladic functions have this tendency to depend on some invisible input especially if such a function is member function of a class. Since all class members share the same class variables, most methods aren't pure at all. Class variable values will always depend on which method was called last. In a nutshell, most niladic functions depend on some **global state** in this case self.first_name.

The same can be said to functions that return None. These too aren't pure functions. If a function doesn't return, then it is doing something that is affecting global state. **Such functions can not be composed in fluent APIs.** The best examples are the python sort method and sorted built-in function.

```python
names = ['Kasozi', 'Martin', 'Newton', 'Grady']

#wrong
sorted_names = names.sort()

#correct
sorted_names = sorted(names)
```

#### **Argument Mutation**

Functions that mutate their input arguments aren't pure functions. This becomes more pronounced when we run on multiple cores. More than one function may be reading from the same variable and each function can be context switched from the CPU at any time. If it was not yet done with editing the variable, others will read garbage.

**Bad :angry:**

```python
marks = [43, 78, 56, 90, 23]

def sort_marks(marks : List[int]) -> None:
    marks.sort()

def calculate_average(marks : List[int]) -> float:
    return sum(marks)/float(len(marks))
```

From the above code snippet, we have two functions that both read the same list. `sort_marks()` mutates its input argument and this is not good. Now imagine a scenario when `calculate_average_mark()` was running and before it completed, it was context switched and `sort_marks()` allowed to run.

sort_marks will update the list in place and change the order of elements in the list, by the time `calculate_average_average()` will run again, it will be reading garbage.

**Good :smile:**

```python
marks = [43, 78, 56, 90, 23]

#sort_marks now returns a new list and uses the sorted function
def sort_marks(marks : List[int]) -> List[int]:
    return sorted(marks)

def find_average_mark(marks : List[int]) -> float:
    return sum(marks)/len(marks)
```

This problem can also be solved by using immutable data structures.

> Function purity is also vital for unit-testing. Impure functions are hard to test especially if the side effect has to do with I/O. Unlike mutation, you can’t avoid side effects related to I/O; whereas mutation is an implementation detail, I/O is usually a requirement.

#### **Exceptions**

Some function signatures are more expressive than others, by which I mean that they give us
more information about what the function is doing, what inputs are permissible, and what outputs we can expect. The signature `() → ()`, for example, gives us no information at all: it may print some text, increment a counter, launch a spaceship... who knows! On the other hand, consider this signature:

`(List[int], (int → bool)) → List[int]`

Take a minute and see if you can guess what a function with this signature does. Of course, you
can’t really know for sure without seeing the actual implementation, but you can make an
educated guess. The function returns a list of `ints` as input; it also takes a list of `ints`, as well as a
second argument, which is a function from int to `bool`: a predicate on int.

But is not honest enough. What happens if we pass in an empty list?? This function may throw an exception.

> @icon-info-circle Exceptions are hidden outputs from functions and functions that use exceptions have side effects. This is why functional programming pure functions use other strategies like the **Monads** in Haskell.

**Bad** :angry: 
```python
def find_quotient(first : int, second : int)-> float:
    try:
        return first/second
    except ZeroDivisionError:
        return None
```

What is wrong with such a function? In its signature, it claims to return a float but we can see that sometimes it fails. Such a function is not honest and such functions should be avoided.

#### I/O
Functions that perform input/output aren't pure too. Why? This is because they return different outputs when given the same input argument. Let me explain more about this. Imagine a function that takes in an URL and returns HTML, if the HTML is changed, the function will return a different output but it is still taking in the same URL. Remember mathematical functions don't behave like this.

```python
def read_HTML(url : str)-> str:
    try:
        with open(url) as file:
            data = file.read()
        data = file.read()
        return data
    except FileNotFoundError:
        print('File Not found')
```
This function is plagued with more than one problem.
- Its signature is not honest. It claims that the function returns a string and takes in a string but from the implementation, we see it can fail.
- This function is performing IO. IO operations produce side effects and thus this function is not pure.


> @icon-info-circle You can build pure functions in python with the help of the **operator** and **functools** modules. There is a package **fn.py** to support functional programming in Python 2 and 3. According
to its author, Alexey Kachayev, fn.py provides “implementation of missing features to
enjoy FP” in Python. It includes a @recur.tco decorator that implements tail-call optimization
for unlimited recursion in Python, among many other functions, data structures,
and recipes.

### Command-Query-Separation

### Don't Repeat Yourself (DRY)

## **SOLID Principles**

### 1. Single Responsibility Principle

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
