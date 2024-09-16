---
title: 'Red black trees in F#'
date: 2024-09-15
permalink: /posts/2024/09/fs-red-black-tree
tags:
  - F#
  - Algorithms and Data Structures
---

Red-black trees
------

Days ago when I was browsing the Internet, I came across a [gitbook tutorial](https://cs3110.github.io/textbook/chapters/ds/rb.html) on how to implement specific data structures in OCaml, which included red-black trees. Red-black trees are a type of self-balancing binary search trees that could keep its worst case search/insertion/deletion time within $O(\log(N))$. If you are looking for a complete introduction to what red-black trees are, you may refer to its [Wikipedia page](https://cs3110.github.io/textbook/chapters/ds/rb.html). As I have been looking into F# and .NET for quite some time, I thought it might be good practice to implement the data structure in F#.

Nodes in the red-black trees are painted in either red or black, which gives our definition to the `Color` and `RedBlackTree` types.

```F#
[<Struct>]
type Color =
	| Red
	| Black
type 'a RedBlackTree =
	| Node of Color * 'a * 'a RedBlackTree * 'a RedBlackTree
	| Leaf
```

## Insertion

The reason why insertions and deletions to elements in a red-black tree are complicated is that we must maintain the local and global invariants of the data structure after making changes to it. Naively adding or removing tree nodes could potentially cause connections between two red nodes or break the equality of number of black nodes among all paths.

For the implementation of insertions to our red-black trees, we follow the method proposed by Okasaki, which first inserts a red node to the corresponding location on the fringe of the tree (if the element was previously absent in the tree), then moves up to the root while applying rotations to resolve violations to the local invariant (connection between red nodes). Finally, the algorithm paints the root of the red-black tree black in case it was painted red during the last rotation.

Violations to the local invariant could appear in one of these four configurations:

![Possible violations to the local invariant](images/posts/2024-09-15 okasaki-insertion-1.png)

After our rotation, we want to obtain this:

![We want to rotate the nodes into a red-rooted treelet](images/posts/2024-09-15 okasaki-insertion-2.png)

The corresponding F# implementation would be:

```F#
let balance = function
    | Node(Black, z, Node(Red, y, Node(Red, x, a, b), c), d)
    | Node(Black, z, Node(Red, x, a, Node(Red, y, b, c)), d)
    | Node(Black, x, a, Node(Red, y, b, Node(Red, z, c, d))) ->
        Node(Red, y, Node(Black, x, a, b), Node(Black, z, c, d))
    | n -> n

let insert (x: 'a) (t: 'a RedBlackTree) : 'a RedBlackTree =
    let rec ins = function
        | Leaf Black -> Node(Red, x, Leaf Black, Leaf Black)
        | Node(color, y, a, b) as s ->
            if x < y then balance (Node(color, y, ins a, b))
            elif x > y then balance (Node(color, y, a, ins b))
            else s
        | _ -> failwith "unreachable"
    match ins t with
    | Node(_, y, a, b) -> Node(Black, y, a, b)
    | _ -> failwith "unreachable"
```

The `balance` function works to resolve the violations demonstrated above. The `insert` function first calls the recursive function `ins` to search for the proper location to add the new node, then traverses from the node up to the root while making rotations.

