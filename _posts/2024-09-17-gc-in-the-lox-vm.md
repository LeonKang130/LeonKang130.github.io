---
title: 'GC in the Lox VM'
date: 2024-09-17
permalink: /posts/2024/09/gc-in-the-lox-vm
excerpt: 'A quick look at the garbage collector in Lox VM.'
tags:
  - Algorithms
  - Garbage Collection
---

## Lox from Crafting Interpreters

Lately I've been reading about garbage collection while learning about .NET. However, the chance for me to get my hands on implementing these GC algorithms somehow seemed slight, and then I came across the scripting language from the book *Crafting Interpreters*, Lox. The book provided an implementation of the language's virtual machine in C, which used **mark-and-sweep** to manage memory on the heap.  I figured that it might be the right place for me to test out writing GC algorithms, so I spent some time sorting out the code and reading the book.

## Mark-and-Sweep in Lox Virtual Machine

The mark-and-sweep algorithm works in two phases:

-   Marking: Starting from the roots (objects directly reachable in the current context), we mark all the objects and ones referenced by them, so that they would be deemed as **active**, which means that they are not garbage.
-   Sweeping: After we are done with the marking phase, we go through the **inactive** (unmarked) objects and free their memory on the heap.

As you may have guessed, this GC algorithm requires us to implement traversal on the graph of objects. The book has a pretty good picture demonstrating this:

![Garbage collector tracing through the graph of objects](https://craftinginterpreters.com/image/garbage-collection/mark-sweep.png)

To implement the mark-and-sweep algorithm, we must first find out the objects directly reachable in the current context, which are the root objects. In Lox, this includes objects in:

-   the evaluation stack of the virtual machine
-   the hash table containing all the global objects of the virtual machine
-   the call frame stack of the virtual machine
-   the list of open upvalue references (which themselves are objects on the heap)
-   the compiler stack (which allocates objects of type `ObjFunction`)

After marking the root objects, we need to further traverse the objects that could be reached indirectly:

-   `ObjBoundMethod` references its receiver and closure
-   `ObjClass` references its name and methods
-   `ObjClosure` references its function and upvalues
-   `ObjFunction` references its name and constants (which may include objects)
-   `ObjInstance` references its class and fields
-   `ObjUpValue` references its closed values

The Lox VM applies DFS to traverse the object graph. When the traversal is done, the garbage collector works to free inactive (unreachable/unused) objects. The VM maintains a linked list of all objects allocated so far, and during the sweep phase all the inactive objects are removed from the linked list and freed.

## Potential Bugs

However, bugs do exist in the lox garbage collector because the C stack is invisible to it. If an object resides only in the C stack and is actually active for it will be used (for example by native code later), it could potentially get freed since it cannot be marked during a GC.

## Reference

-   [Crafting Interpreters](https://craftinginterpreters.com/) by Robert Nystrom (you can read it online)



