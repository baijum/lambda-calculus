---
layout: post
title: "Selection Sort"
date: 2020-05-10
categories: scheme algorithm
---

Code:

```scheme
(define (smaller first second)
  (if (< first second) first second))

(define (smallest array)
  (if (null? (cdr array))
      (car array)
      (smaller (car array) (smallest (cdr array)))))

(define (list-remove array val)
  (if (eq? val (car array))
      (cdr array)
      (cons (car array) (list-remove (cdr array) val))))

(define (selection-sort-helper smallest-value array)
  (cons smallest-value 
	(selection-sort (list-remove array smallest-value))))

(define (selection-sort array)
  (if (null? array)
      '()
      (selection-sort-helper (smallest array) array)))

(selection-sort '())
(selection-sort '(3))
(selection-sort '(3 2))
(selection-sort '(3 2 4))
```

Go:

```go
package main

import "fmt"

func sortInt(a []int) {
	for i := 0; i < len(a)-1; i++ {
		aMin := a[i]
		iMin := i
		for j := i + 1; j < len(a); j++ {
			if a[j] < aMin {
				aMin = a[j]
				iMin = j
			}
		}
		a[i], a[iMin] = aMin, a[i]
	}
}

func main() {
	m := []int{4, 2, 3, 9, 7}
	sortInt(m)
	fmt.Println(m)
}
```
