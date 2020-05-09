---
layout: post
title: "Binary Search"
date: 2020-05-09
categories: scheme algorithm
---

[Binary search][binary-search] is a fast alogorithm to find a particular number
within a given sorted array.  This is the solution I came up after referring
many similar solutions:

```scheme
(define (binary-search value array)
  (let loop ([low 0]
	     [high (1- (vector-length array))])
    (let ((mid (floor (/ (+ high low) 2))))
      (cond ((> low high) -1)
	    ((> (vector-ref array mid) value) (loop low (1- mid)))
	    ((< (vector-ref array mid) value) (loop (1+ mid) high))
	    (else mid)))))

(binary-search 42 '#(17 25 36 42 48 49 55))
(binary-search 13 '#(4 8 12 17 25 36 42 48 51))
(binary-search -13 '#(-51 -48 -42 -36 -25 -17 -13 -12 -8 -4))
(binary-search -36 '#(-51 -48 -42 -36 -25 -17 -13 -12 -8 -4))

(binary-search 42 '#())
```

The input for `linear-search` procedure is the value to search followed by a
[vector] of items.  Some Scheme implementations require a quoted form to represent
vectors.  This is the reason for the usage of quoted form when giving input.

I used a [named let] to create a loop to avoid defining a procedure and calling
it.  This is possible because a [tail recursion] is suitable in this case.

I used `floor` procedure to get the [floor] value after dividing the sum of low
and high values by two.

Using the `vector-ref` procedure, it is possible to access value at a given
index. Similarly `vector-length` gives the number of items in the vector.  The

The `1+` procedure add one to the given value.  Similarly `1-` procedure
substract one from the given value.

[binary-search]: https://en.wikipedia.org/wiki/Binary_search_algorithm
[vector]: https://www.gnu.org/software/guile/manual/html_node/Vectors.html
[named let]: https://stackoverflow.com/questions/31909121/how-does-the-named-let-in-the-form-of-a-loop-work
[tail call recursion]: https://en.wikipedia.org/wiki/Tail_call
[floor]: https://en.wikipedia.org/wiki/Floor_and_ceiling_functions
