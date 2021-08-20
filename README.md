# Clean Python Code

## Table of Contents

1. [Introduction](#introduction)

2. [Variables](#variables)
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

## variables

### Use intention revealing name

This rule enforces that programmers should make their code read like well written prose by naming parts <br>
of their code perfectly. With such good naming, a programmer will never need to resort to comments or unneccesary <br> docstrings.
Below is a code snippet from a software system. Would you make sense of it without any explanation?

```python
from typing import List

def f(a : List[List[int]]) -> bool:
    l : int = len(a)
    for i in range(l):
        for j in range(l):
            if i == j and a[i][j] != 0:
                return False
    return True
```

This is a small piece of code without fancy abstractions. All it contains are generics (List), iterations (loops)
selections (ifs), variables and operators. But why does it seem so cryptic?? Something has gone wrong here.

The biggest problem here is that this code speaks the vocabulary of python and not the domain vocabulary, in some way, it lacks context. With meaningfull names, we can convert this code into a well written prose. What does f mean and l?? they don't give out any information and this is bad.

```python
def find_loops(graph : List[List[int]]) -> bool:
    number_of_vertices : int = len(graph)
    for i in range(number_of_vertices):
        for j in range(number_of_vertices):
            if i == j and graph[i][j] != 0:
                return False
    return True
```

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
