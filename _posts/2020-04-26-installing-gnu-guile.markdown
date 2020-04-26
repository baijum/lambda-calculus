---
layout: post
title: "Installing GNU Guile"
date: 2020-04-26
categories: scheme guile
---

If you are looking forward to learning [Scheme programming][scheme], I would
suggest you use [GNU Guile][guile].  Most of the GNU/Linux distribution has
GNU Guile package available in their repository.

If you are using Debian or Ubuntu or any of the derivatives of those systems,
you can run this command to install GNU Guile:

    apt-get install guile

For Fedora, RHEL, and CentOS or any of the derivatives of those systems, you can
run:

    dnf install guile

These days I am using a Fedora for development works.  I installed [Guix][guix]
package manager in my Fedora system.  Using Guix, I was able to install the
latest version of Guile with this command:

    guix install guile-next

After installation, you can invoke the Guile interactive prompt like this:

    $ guile
    GNU Guile 3.0.2
    Copyright (C) 1995-2020 Free Software Foundation, Inc.

    Guile comes with ABSOLUTELY NO WARRANTY; for details type `,show w'.
    This program is free software, and you are welcome to redistribute it
    under certain conditions; type `,show c' for details.

    Enter `,help' for help.
    scheme@(guile-user)>

[GNU Emacs][emacs] would be a good editor to work with Scheme programming.  You
can also use [Geiser] major mode to access an interactive prompt.

Since I am using [guix] to install GNU Guile, I have specified the Guile binary
location in the `.emacs` file like this:

	(setq geiser-guile-binary "~/.guix-profile/bin/guile")

Let me finish this blog post with a simple program I wrote to print "Hello,
World!"  message 10 times:

```scheme
(use-modules (ice-9 format))

(define (print-hello count)
  (format #t "Hello, World! ~a\n" count)
  (if (= count 1)
	(display "Done")
	(print-hello (- count 1))))

(print-hello 10)
```

[scheme]: https://en.wikipedia.org/wiki/Scheme_(programming_language)
[guile]: https://www.gnu.org/software/guile
[guix]: https://guix.gnu.org
[emacs]: https://www.gnu.org/software/emacs
[Geiser]: https://www.nongnu.org/geiser
