---
layout: post
title: "Linear Search"
date: 2020-04-30
categories: scheme algorithm
---

[Linear search][linear-search] (a.k.a. sequencial search) is a commonly used
alogorithm to find a particular item within a given list.  This is the solution
I came up after referring many similar solutions:

```scheme
(define (linear-search value array)
  (let loop ([index 0])
    (cond ((= (vector-length array) index) -1)
	  ((eqv? (vector-ref array index) value) index)
	  (else (loop (1+ index))))))

(linear-search 4 '#(4 0 1 3 7))
(linear-search 0 '#(4 0 1 3 7))
(linear-search 7 '#(4 0 1 3 7))
(linear-search 9 '#(4 0 1 3 7))
(linear-search 1 '#())
(linear-search "Jack" '#("Jill" 4 5.6 "Jack" 8))
```

The input for `linear-search` procedure is the value to search followed by a
[vector] of items.  Some Scheme implementations require a quoted form to represent
vectors.  This is the reason for the usage of quoted form when giving input.

I used a [named let] to create a loop to avoid defining a procedure and calling
it.  This is possible because a [tail recursion] is suitable in this case.

Using the `vector-ref` procedure, it is possible to access value at a given
index. Similarly `vector-length` gives the number of items in the vector.  The

`eqv?` procedure check whether the values of the given arguments are equal.
This procedure should work for all types of Scheme values.  This made it
possible to use same `linear-search` procedure for integer and string vectors.

The `1+` procedure add one to the index value.  This way the index value is
incremented bye one before each loop.

[linear-search]: https://en.wikipedia.org/wiki/Linear_search
[vector]: https://www.gnu.org/software/guile/manual/html_node/Vectors.html
[named let]: https://stackoverflow.com/questions/31909121/how-does-the-named-let-in-the-form-of-a-loop-work
[tail call recursion]: https://en.wikipedia.org/wiki/Tail_call
