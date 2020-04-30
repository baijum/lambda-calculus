---
layout: page
title: Scheme Tutorial
permalink: /scheme-tutorial
---

## Introdution

[Scheme] is a small and simple general purpose programming language.  Scheme has
many [implementations] with an evolving set of [standards].  This tutorial is
going to use [GNU Guile][guile] implementation of Scheme.

[GNU Emacs][emacs] would be a good editor to work with Scheme programming.  You
can also use [Geiser] major mode to access an interactive prompt.

Scheme can be considered as a programming language in the [Lisp] family.  The
name LISP derives from "LISt Processor".  As the name suggests, the language
handles lists and lists of lists.  The parentheses are used to mark the
boundaries of the list.  Similar to Lisp, you can see lots of parentheses in
Scheme.

## Quickstart

If you have not installed [GNU Guile][guile] in your system, I would suggest you
to refer my blog post: [Installing GNU Guile]({% post_url
2020-04-26-installing-gnu-guile %}).

After installation, you can invoke the Guile interactive prompt like this:

    $ guile
    GNU Guile 3.0.2
    Copyright (C) 1995-2020 Free Software Foundation, Inc.

    Guile comes with ABSOLUTELY NO WARRANTY; for details type `,show w'.
    This program is free software, and you are welcome to redistribute it
    under certain conditions; type `,show c' for details.

    Enter `,help' for help.
    scheme@(guile-user)>

This is how you can display a "Hello, World!" message from the interactive
prompt:

	(display "Hello, World!")

You can save the above text in a file named `hello.scm` and run it like this:

	$ guile hello.scm
	Hello, World!

In the above source code, what you see is an expression in Scheme.  The whole
expression is within a parenthesis with space separated values.  The word
`display` is a procedure followed by a `"Hello, Word!"` string argument.

Now you can type this expression for calculating sum of two integers:

	(+ 2 3)

The `+` is the procedure here and `2` and `3` are two arguments.  The
parenthesis can be nested and the inner most parenthesis will be evaluated
first.  Here is an example with nested parenthesis:

	(+ 3 (- 6 1))

In the above expression, the inner most list is processed at first.  The result
is `5` which is going to be added to `3` and the final result is `8`.

Befor going to the basics, try these examples and observe the results:

	(* 7 3)
	(/ 27 9)
	(quotient 9 2)
	(remainder 9 2)
	(/ 1 3)
	(exact->inexact (/ 1 3))
	(string-append "Hello," " World!")
	(number->string 78)

The above expressions use built-in procedures for various integer and string
manipulations.  Before explaining some of those things in detail, let's look
into some basics of Scheme.

## Basics

### Lists

From the introdution and the quickstart, you must be got an idea about the
importance of lists in Scheme.  All the expressions are nothing but lists that
is getting processed according to the respective procedure.

To construct a list, you can use the `list` procedure. Here is an example of
constructing a list of few prime numbers:

	(list 2 3 5 8)

If you remove all arguments to the `list` procedure, that becomes an an empty
list:

	(list)

You can use the above syntax to represent an empty list.  But there is another
more commonly used method using quotes.  Unlike the above expression, quotes are
syntatic form provided by Scheme.  An empty list can be represented like any one
of these forms:

	(quote ())
	'()

Both these forms are identical.  The second, more compact form, using the single
quote, is a syntatic-sugur.  You can read more about quotes in the next
section.

To understand the list concept, you need understand the concept of pairs.  Pairs
are the building blocks of lists.  You can create a pair using the `cons`
procedure.

	(cons 2 3)

The above expression creates a pair and the representation is like this: `(2
. 3)` This notation is called dotted pair notation.  There are two procedures
called `car` and `cdr` to access the first and second part of the pair.
Pronunciation for `car` is `/kɑːr/` and pronunciation for `cdr` is `/ˈkʌdər/` or
`/ˈkʊdər/`.


This is how to access `car` for the above pair:

	(car (cons 2 3))

The above expression returns `2`.  Now try the `cdr`:

	(cdr (cons 2 3))

The `cdr` expression above returns `3`.

Technically, list is a [singly-linked list].  The last element in a list must be
an empty list which is considered as a null value.  You can verify that empty
list is `null` using `null?` procedure call:

	(null? '())

If you run the above expression, you can see that it returns `#t` which is the
representation of true (boolean) value.

The `cons` procedure can be used construct a by giving the second part of the
pair as empty list:

	(cons 2 '())

The expression given above produce a list with one value like this: `(2)`.  In
the above expression, an empty list is given using quotes.  You can also use the
`list` expression:

	(cons 2 (list))

The above expression also produce the same result as before.  To construct a
"proper list", the last value should be an empty list.  If the last value is not
an empty list, it becomes an "improper list".  Here is example constructing a
proper list using an empty list at the end:

	(cons 2 (cons 3 (cons 5 (cons 5 '()))))

The result will be same as using the `list` expression:

	(list 2 3 5 8)

A schematic of this list is going to be like this:

    +---+---+   +---+---+   +---+---+   +---+---+
	| * | * +-->| * | * +-->| * | * +-->| * | * |-->'()
    +-+-+---+   +-+-+---+   +-+-+---+   +-+-+---+
      |           |           |           |
      v           v           v           v
      2           3           5           8

This is called the box notion.

### Quotes

Literal form of any Scheme expression can be created using special syntax called
quotes.  Here is an example:

	(quote my-var)

The above quoted form represent the `my-var` variable.  To understand this
concept, assume there no such variable defined and you tried to access it like
this:

	my-var

If you type above text into a GNU Guile prompt, you will get a message like
this:

	Unbound variable: my-var

Previously when you used the quoted form, it displayed the literal form.
Here are few more example quotes:

	(quote (+ 2 3))
	(quote (2 3 4 5))

As mentioned before, there is syntatic-sugur for quotes.  Using that syntax, you
can write quotes like this:

	'(+ 2 3)
	'(2 3 4 5)

Scheme has a procedure called `eval` to evaluate quotes.  Here is an example:

	(eval '(+ 2 3) (interaction-environment))

When you execute the above expression in a Guile prompt, you should see the
result as `5`.

### Procedures

You can create your own procedure using the `define` procedure.

Here is a procedure to calculate sum of two integers:

	(define (sum x y)
	  (+ x y))

You can call the above procedure like this:

	(sum 2 3)


## Frequently Asked Questions

#### When evaluating an expression I am getting `Unbound variable: scheme-report-environment` error.  How to fix it?

You need to load the `(ice-9 r5rs)` module to use the
`scheme-report-environment` procedure call.  Here is an example:

{% highlight scheme %}
(use-modules (ice-9 r5rs))
(define a '(+ 3 5))
(eval a (scheme-report-environment 5))
{% endhighlight %}

The above code should return the result as `8`.  For more details this topic
refer the [GNU Guile manual page about
environments](https://www.gnu.org/software/guile/manual/html_node/Environments.html).

## Resources

1. [schemers.org](https://schemers.org) website has a good collection of resources.
2. [Introduction to Scheme by Andy Balaam](https://www.youtube.com/watch?v=byofGyW2L10&list=PLfHYba8zC7hTNImXmUAxuPcmhpgWlZLAc) has 7 videos.
3. [Scheme FAQ](http://community.schemewiki.org/?scheme-faq) is a wiki FAQ.
4. [The Scheme Programming Language book by R. Kent Dybvig](https://scheme.com/tspl4/)
5. [Structure and Interpretation of Computer Programs](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book.html)

## Summary

I hope you found the tutorial helpful for getting started with the Scheme
programming.  If you are looking for an advanced book on Scheme and programming
in general, you can study [SICP].

[Scheme]: https://en.wikipedia.org/wiki/Scheme_(programming_language)
[implementations]: http://community.schemewiki.org/?scheme-faq-standards#implementations
[standards]: https://schemers.org/Documents/Standards/
[guile]: https://www.gnu.org/software/guile
[emacs]: https://www.gnu.org/software/emacs
[Geiser]: https://www.nongnu.org/geiser
[Lisp]: https://en.wikipedia.org/wiki/Lisp_(programming_language)
[singly-linked list]: https://en.wikipedia.org/wiki/Linked_list#Singly_linked_list
[SICP]: https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book.html
